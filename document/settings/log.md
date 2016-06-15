# 日志配置

Cdeio 采用 logback 进行日志管理。logback 的配置文件为 logback.xml 。

默认情况下采用控制台和文件两种输出方式，Cdeio 会每天生成一个独立的日志文件记录当天的日志信息。

logback 中定义了众多组件（如 Spring、 Hibernate 、 Jetty 、 Shiro 等）的日志级别，可以根据需要对各个组件的级别进行调整。也可以根据需要向 logback 中添加需要管理的组件。

```xml
<!-- 修改 level 属性即可 -->
<logger name="org.springframework" level="INFO" />
```


当然也可以通过全局的方式进行调整。

```xml
<!-- 修改 level 属性即可 -->
<root level="INFO">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="FILE" />
</root>
```

logback 的能力还不只是这些，诸如与 JMS 集成、与数据库集成、自定义适配器、自定义日志格式、自定义过滤规则等都是可以的。想要了解更多的信息请参考 <a href="http://logback.qos.ch/documentation.html" target="_blank">&nbsp;logback&nbsp;</a>官方文档。