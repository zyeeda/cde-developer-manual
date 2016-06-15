# mark

marker 类似于 Java 中的 annotation，主要用来向方法中注入对象。基本用法类似这样：
```js
var {mark} = require('cdeio/marker');

mark('something').on(function () {
    ...
});
```
直接通过字面理解，就是将什么东西标记在某个方法之上。经过 mark 的方法会被注入一些参数，而返回结果是另一方法，该方法绑定了所有被注入的参数，而留下那些没有被注入的参数作为新方法的参数。mark 方法注入的参数会按照 marker 出现先后顺序以此排列。

mark 方法是可以串联使用的，也就是多个 mark 方法可以连接在一起，形成一个 marker 链，最后以 on 作为结束：

```js
mark('first').mark('second').mark('third').on(function () {
    ...
});
```

目前框架支持四种类型的 marker，分别为 tx 、 beans 、 services 和 managers

### 1. tx

该 marker 向方法注入事务。经过注入的方法会运行在事务当中。事务的具体注入过程是依赖 Spring 容器的，因此关于事务注入的详细信息，请参考 Spring Transaction 的官方文档。

```js
mark('tx').on(function () {
    ...
});
```
如果需要配置事务的属性，或控制事务的提交、回滚策略，可以在第二个参数里面进行配置：

```js
mark('tx', config).on(function () {
    ...
});
```
可以配置的选项有：

* **readOnly**，配置该事务是否只读，布尔类型，默认值 false
* **name**，事务名称，字符串类型，默认值 transaction name
* **propagationBehavior**，事务传播行为，整型枚举，默认值 0，即 PROPAGATION_REQUIRED
* **isolationLevel**，事务隔离等级，整型枚举，默认值 -1，即 ISOLATION_DEFAULT
* **timeout**，事务超时时间，整型，单位秒，默认值 -1
* **needStatus**，是否需要注入 TransactionStatus 参数，布尔类型，默认值 false

**提示**，有关 propagationBehavior 和 isolationLevel 的枚举值请参考 TransactionDefinition 类型。

如果使用了 needStatus 配置，可以在 on 后面的方法中接收这个 TransactionStatus 参数，使用方法如下：

```js
mark('tx', {needStatus: true}).on(function (status) {
    ...
});
```

**注意**，如果在 marker 链中使用 tx marker，则其必须出现在 marker 链的最尾部。

### 2. beans

该 marker 向方法注入 Spring 容器中声明的 Java Bean 对象。

```js
mark('beans', 'bean1', 'bean2', JavaBeanClass).on(function (bean1, bean2, javaBeanInstance) {
    ...
});
```
此 marker 的第一个参数表明了该 marker 的类型，后面是一组变参，将需要注入的 Java Bean 的 ID 或者类型罗列在此，后面的方法就可以按次序接收到注入的 Java Bean 对象。

### 3. services

该 marker 向方法注入 service 对象。

```js
mark('services', 'modulePath1:serviceName1', 'modulePath2:serviceName2').on(function (service1, service2) {
    ...
});
```

此 marker 的第一个参数表明了该 marker 的类型，后面也同样是一组变参。其中的 service 使用了一种特殊的表示方法，称为“坐标”。该坐标由两部分组成，以冒号（:）分割。冒号的左边是该 service 所在模块的绝对路径，冒号右边是该 service 的文件名。如果路径中只给出模块的路径没有使用冒号指定 service 文件的名称，则默认注入 feanture 下的 service.js 文件中定义的 service 。


**注意**，marker 要求注入的 service 必须导出一个名为 createService 的方法。

### 4. managers

该 marker 向方法注入 manager 对象。

```js
mark('managers', User, Group, 'modulePath:managerName').on(function (userManager, groupManager, otherManager) {
    ...
});
```

同上，此 marker 的第一个参数也表明了该 marker 的类型，后面也是一组变参，该参数里面可以包含两种类型的对象。首先可以指定实体类型，则框架会自动注入一个通用的 manager 用来访问该实体，另外如果是指定到 manager 的坐标（同 service 的坐标概念类似），则框架注入该坐标指向的 manager。如果坐标中不包含 manager 文件名称，则默认注入 feanture 下的 manager.js 文件中定义的 manager 。

注意 此 marker 要求注入的 manager 必须导出一个名为 createManager 的方法。

