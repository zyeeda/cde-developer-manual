# 任务监听

任务监听器可以在发生对应的任务相关事件时执行自定义java逻辑 或表达式。

任务监听器只能添加到流程定义中的用户任务中。 注意它必须定义在BPMN 2.0 extensionElements的子元素中， 并使用activiti命名空间，因为任务监听器是activiti独有的结构。

```xml
<userTask id="myTask" name="My Task" >
  <extensionElements>
    <activiti:taskListener event="create" class="org.activiti.MyTaskCreateListener" />
  </extensionElements>
</userTask>
```

任务监听器支持以下属性：

* **event（必选）**：任务监听器会被调用的任务类型。 可能的类型为：<br/>
  create：任务创建并设置所有属性后触发。<br/>
  assignment：任务分配给一些人时触发。 当流程到达userTask， assignment事件 会在create事件之前发生。 这样的顺序似乎不自然，但是原因很简单：当获得create事件时， 我们想获得任务的所有属性，包括执行人。<br/>
  complete：当任务完成，并尚未从运行数据中删除时触发。<br/>
  delete：只在任务删除之前发生。 注意在通过completeTask正常完成时，也会执行。
* **class**：必须调用的代理类。 这个类必须实现org.activiti.engine.impl.pvm.delegate.TaskListener接口。

```java
public class MyTaskCreateListener implements TaskListener {

  public void notify(DelegateTask delegateTask) {
    // Custom logic goes here
  }

}
```

可以使用属性注入把流程变量或执行传递给代理类。 注意代理类的实例是在部署时创建的（和activiti中其他类代理的情况一样），这意味着所有流程实例都会共享同一个实例。

expression：（无法同时与class属性一起使用）： 指定事件发生时执行的表达式。 可以把DelegateTask对象和事件名称（使用task.eventName） 作为参数传递给调用的对象。

```xml
<activiti:taskListener event="create" expression="${myObject.callMethod(task, task.eventName)}" />
```

delegateExpression可以指定一个表达式，解析一个实现了TaskListener接口的对象， 这与服务任务一致。

```xml
<activiti:taskListener event="create" delegateExpression="${myTaskListenerBean}" />
```

在activiti 5.12中，我们也介绍了新的任务监听器，org.activiti.engine.impl.bpmn.listener.ScriptTaskListener。 脚本任务监听器可以为任务监听器事件执行脚本。

```xml
<activiti:taskListener event="complete" class="org.activiti.engine.impl.bpmn.listener.ScriptTaskListener" >
  <activiti:field name="script">
    <activiti:string>
      def bar = "BAR";  // local variable
      foo = "FOO"; // pushes variable to execution context
      task.setOwner("kermit"); // test access to task instance
      bar // implicit return value
    </activiti:string>
  </activiti:field>
  <activiti:field name="language" stringValue="groovy" />
  <activiti:field name="resultVariable" stringValue="myVar" />
<activiti:taskListener>
```