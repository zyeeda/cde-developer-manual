# 数据源配置

系统默认使用 MySql 数据库，采用第三方组件 bitronix 提供数据源。在 src/main/webapp/WEB-INF/jetty-evn.xml 文件中已经将与数据源相关的信息配置完成，如果需要修改数据库信息则修改其中的 ```driverProperties``` 配置即可，其中：

* **URL**，代表协议。
* **user**，代表用户名。
* **password**，代表密码。
```xml
<Get name="driverProperties">
    <Put name="URL">jdbc:mysql://localhost:3306/cdeio-examples?pinGlobalTxToPhysicalConnection=true&amp;useUnicode=yes&amp;characterEncoding=UTF-8</Put>
    <Put name="user">root</Put>
    <Put name="password">root</Put>
</Get>
```

如果需要使用其他的数据库，则应进行如下几步操作，我们以 oracle 数据库为例，假定实例、用户名和密码均为 "orcl"：

* **引入依赖**，在 pom.xml 中引入 oracle 驱动的依赖。
```xml
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc14</artifactId>
    <version>10.2.0.4.0</version>
</dependency>
```

* **更改 hibernate 方言**，在 src/main/resources/META-INF/persistence.xml 中增加方言配置。
```xml
<properties>
  ...
  <property name="hibernate.dialect" value="org.hibernate.dialect.Oracle10Dialect" />
</propertie
```

* **修改协议及账户信息**，在 jetty-evn.xml 中做如下修改。
```xml
<Get name="driverProperties">
    <Put name="URL">jdbc:oracle:thin:@localhost:1521:ORCL</Put>
    <Put name="user">orcl</Put>
    <Put name="password">orcl</Put>
</Get>
```
