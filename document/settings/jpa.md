# JPA 配置


### 基本配置

JPA 的配置文件为 src/main/resources/META-INF/persistence.xml 。由于 Cdeio 在启动时会根据 src/mian/webapp/WEB-INF/app/config.js 中的 [cdeio.entityPackages](/document/settings/system.html)配置自动解析各个包中所包含的实体信息，所以在开发过程中不需要在 persistence.xml 中对实体进行配置，但是如果是在发布模式，则需要配置：
```xml
<persistence-unit name="default" transaction-type="JTA">
    <class>com.zyeeda.cdeio.commons.organization.entity.Account</class>
    <class>com.zyeeda.cdeio.commons.organization.entity.Department</class>
    ...
</persistence-unit>
```

如果你使用了命名查询，则需要在 persistence.xml 中指定命名查询文件的位置，配置如下：

```xml
<persistence-unit name="default" transaction-type="JTA">
    <mapping-file>META-INF/mappings/document.orm.xml</mapping-file>
    ...
</persistence-unit>
```

### 自动加载

在 persistence.xml 中指定的的 orm 文件只会在系统启动时加载并预编译其中的 HQL ，但是当修改了或新增了某些命名查询时，系统不会自动重新装载，只有重启系统后才会生效。为解决此问题 Cdeio 提供了动态加载的方式，动态加载能够在不需要重新启动系统的情况下自动重新装载修改过的 orm 文件。允许在 src/mian/webapp/WEB-INF/app/config.js 中配置 [cdeio.orms](/document/settings/system.html) 属性，在 `cdeio.orms` 中指定的 orm 文件在修改后会被 Cdeio 自动重新装载。

```js
exports.cdeio = {
    orms: [
           'src/main/resources/META-INF/mappings/account.orm.xml'
    ]
};
```

**注意**，自动加载的方式只在开发模式下才会生效。