# Manager

### 描述
manager 在 Cdeio 中充当数据访问层的角色。大多数情况下，如果只需完成基本的添、删、改、查等操作，用户无需自定义 manager，直接使用 Cdeio 提供的通用的 manager 就可以了。

通用 manager 还有一个功能就是可以动态加载并自动调用 orm.xml 文件中定义的 named query。JPA 规范中允许将 named query 以配置文件的形式保存到 orm.xml 文件中，这样做可以带来若干好处：比如可以很方便的集中编辑所有的 HQL/SQL 语句，而且预先定义的 HQL 语句会在系统启动的时候进行预编译，提高运行期的语句解析速度。但是随之产生一个问题，就是该文件改动以后，必须重新启动系统才可以被重新加载，这个过程显然有悖于 Cdeio 的基本理念。因此 Cdeio 在实现通用 manager 的时候在这方面进行了加强。只要 orm.xml 文件的内容被修改过，调用 manager 方法的时候就会自动加载这些变更了的文件。这种形式的动态加载自然会对运行效率有一定的影响，因此系统可以通过参数配置运行的模式，只有运行在开发模式下，此项功能才会启用。

### 命名查询

通用的 manager 充分利用了 JavaScript 语言的动态性，声明在 orm.xml 文件中的 named query 的名称，可以直接当作 manager 的方法名来使用。比如在某个 orm.xml 文件中声明了一个名为 findByUserName 的 named query，那么就可以直接在通用的 manager 里面调用以 findByUserName 为名称的方法，而且 query 中定义的命名参数，可以以一个 JavaScript 对象的形式传入。

```xml
<named-query name="findByUserName">
    <query>
        from User u where u.userName = :userName
    </query>
</named-query>
```

```js
...
var paramaters = {userName = 'nami'};
var user = userManager.findByUserName(paramaters);
```

### mixin

当通用 manager 无法满足要求的时候， Cdeio 也提供了扩展机制。想要扩展一个 manager 就需 feature 目录下创建一个 manager.js 文件，为了满足 marker 的要求，该文件要导出名为 createManager 的方法。

```js
var {User} = com.xxx.xxx.entity;
exports.createManager = mark('managers', User).on(function (userManager) {
    return userManager.mixin({
        method1: function (entityManager) { ... },
        method2: function (entityManager) { ... }
    });
});
```

可以看到这里仍然使用了 managers mark 来注入一个通用的 manager，原因是我们希望扩展的 manager 仍然具有通用的 manager 的所有功能。在这里调用了通用 manager 内置的 mixin 方法，顾名思义就是将当前对象与参数中的对象的属性和方法进行混合，即返回的结果是一个新的对象，该对象拥有这两个对象的属性和方法的并集。而且 mixin 里面的方法会被自动注入 EntityManager 对象以便对底层数据库进行访问。这里的 EntityManager 就是由 Open Session in View 来控制的，所以在开发过程中无需关注其生命周期状态，直接就可以使用。

如果在通用 manager 的基础上进行扩展仍然无法满足需求，则可以直接在 EntityManager 的基础上进行扩展，甚至编写 native SQL（当然编写 native SQL 的方式是不建议使用的）。

```js
var {mark}  = require('cdeio/mark');
var manager = require('cdeio/manager');

exports.createManager = function(){
    var mgr, sql, query, entity;
    sql = 'from Entity e where e.id = :id';

    mgr = manager.createManager();
    mgr.mixin({
        getEntityById: function(em, id){
            query = em.createQuery(sql);
            query.setParameter('id', id);

            entity = query.getSingleResult();

            return entity;
        }
    });

    return mgr;
};
```

**注意**，在 manager 的方法中不要进行任何与事务有关的操作，这些操作应该是在 service 中进行的。
