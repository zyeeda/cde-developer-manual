# 脚本任务

### 描述

脚本任务时一个自动节点。当流程到达脚本任务， 会执行对应的脚本。

### 图形标记

脚本任务显示为标准BPMN 2.0任务（圆角矩形）， 左上角有一个脚本小图标。

[![脚本任务图标](./images/bpmn.script.task.png)](./images/bpmn.script.task.png)

### XML

脚本任务定义需要指定script 和scriptFormat。

```xml
<scriptTask id="theScriptTask" name="Execute script" scriptFormat="groovy">
  <script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
  </script>
</scriptTask>
```

scriptFormat的值必须兼容 JSR-223（java平台的脚本语言）。默认Javascript会包含在JDK中，不需要额外的依赖。 如果你想使用其他（JSR-223兼容）的脚本引擎， 需要把对应的jar添加到classpath下，并使用合适的名称。 比如，activiti单元测试经常使用groovy， 因为语法比java简单太多。

**注意**，groovy脚本引擎放在groovy-all.jar中。在2.0版本之前， 脚本引擎是groovy jar的一部分。这样，需要添加如下依赖：

```xml
<dependency>
      <groupId>org.codehaus.groovy</groupId>
      <artifactId>groovy-all</artifactId>
      <version>2.x.x<version>
</dependency>
```

### 脚本中的变量

到达脚本任务的流程可以访问的所有流程变量，都可以在脚本中使用。 实例中，脚本变量'inputArray'其实是流程变量 （整数数组）。

```xml
<script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
</script>
```

也可以在脚本中设置流程变量，直接调用 execution.setVariable("variableName", variableValue)。 默认，不会自动保存变量（注意：activiti 5.12之前存在这个问题）。 可以在脚本中自动保存任何变量。 （比如上例中的sum），只要把scriptTask 的autoStoreVariables属性设置为true。 然而，最佳实践是不要用它，而是显示调用execution.setVariable()， 因为一些当前版本的JDK对于一些脚本语言，无法实现自动保存变量。 参考这里获得更多信息。

```xml
<scriptTask id="script" scriptFormat="JavaScript" activiti:autoStoreVariables="false">
```

参数默认为false，意思是如果没有为脚本任务定义设置参数， 所有声明的变量将只存在于脚本执行的阶段。

如何在脚本中设置变量的例子：

```xml
<script>
    def scriptVar = "test123"
    execution.setVariable("myVar", scriptVar)
</script>
```

**注意**：下面这些命名已被占用，不能用作变量名： out, out:print, lang:import, context, elcontext。