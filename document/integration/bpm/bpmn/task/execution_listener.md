# 执行监听器

执行监听器可以执行外部java代码或执行表达式，当流程定义中发生了某个事件。

可以捕获的事件有：

* 流程实例的启动和结束。
* 选中一条连线。
* 节点的开始和结束。
* 网关的开始和结束。
* 中间事件的开始和结束。
* 开始时间结束或结束事件开始。

下面的流程定义包含了3个流程监听器：

```xml
<process id="executionListenersProcess">

    <extensionElements>
      <activiti:executionListener class="org.activiti.examples.bpmn.executionlistener.ExampleExecutionListenerOne" event="start" />
    </extensionElements>

    <startEvent id="theStart" />
    <sequenceFlow sourceRef="theStart" targetRef="firstTask" />

    <userTask id="firstTask" />
    <sequenceFlow sourceRef="firstTask" targetRef="secondTask">
    <extensionElements>
      <activiti:executionListener class="org.activiti.examples.bpmn.executionListener.ExampleExecutionListenerTwo" />
    </extensionElements>
    </sequenceFlow>

    <userTask id="secondTask" >
    <extensionElements>
      <activiti:executionListener expression="${myPojo.myMethod(execution.event)}" event="end" />
    </extensionElements>
    </userTask>
    <sequenceFlow sourceRef="secondTask" targetRef="thirdTask" />

    <userTask id="thirdTask" />
    <sequenceFlow sourceRef="thirdTask" targetRef="theEnd" />

    <endEvent id="theEnd" />

</process>
```

第一个流程监听器监听流程开始。监听器是一个外部java类（像是ExampleExecutionListenerOne）， 需要实现org.activiti.engine.delegate.ExecutionListener接口。 当事件发生时（这里是end事件）， 会调用notify(ExecutionListenerExecution execution)方法。

```java
public class ExampleExecutionListenerOne implements ExecutionListener {

  public void notify(ExecutionListenerExecution execution) throws Exception {
    execution.setVariable("variableSetInExecutionListener", "firstValue");
    execution.setVariable("eventReceived", execution.getEventName());
  }
}
```

也可以使用实现org.activiti.engine.delegate.JavaDelegate接口的代理类。代理类可以在结构中重用，比如serviceTask的代理。

第二个流程监听器在连线执行时调用。注意这个listener元素不能定义event， 因为连线只能触发take事件。 为连线定义的监听器的event属性会被忽略。

最后一个流程监听器在节点secondTask结束时调用。这里使用expression 代替class来在事件触发时执行/调用。

```xml
<activiti:executionListener expression="${myPojo.myMethod(execution.eventName)}" event="end" />
```

和其他表达式一样，流程变量可以处理和使用。因为流程实现对象有一个保存事件名称的属性， 可以在方法中使用execution.eventName获的事件名称。

流程监听器也支持使用delegateExpression, 和服务任务相同。

```xml
<activiti:executionListener event="start" delegateExpression="${myExecutionListenerBean}" />
```

在activiti 5.12及后续版本，我们也引入了新的流程监听器，org.activiti.engine.impl.bpmn.listener.ScriptExecutionListener。 这个脚本流程监听器可以为某个流程监听事件执行一段脚本。

```xml
<activiti:executionListener event="start" class="org.activiti.engine.impl.bpmn.listener.ScriptExecutionListener" >
  <activiti:field name="script">
    <activiti:string>
      def bar = "BAR";  // local variable
      foo = "FOO"; // pushes variable to execution context
      execution.setVariable("var1", "test"); // test access to execution instance
      bar // implicit return value
    </activiti:string>
  </activiti:field>
  <activiti:field name="language" stringValue="groovy" />
  <activiti:field name="resultVariable" stringValue="myVar" />
<activiti:executionListener>
```

### 流程监听器的属性注入

使用流程监听器时，可以配置class属性，可以使用属性注入。 这和使用服务任务属性注入相同， 参考它可以获得属性注入的很多信息。

下面的代码演示了使用了属性注入的流程监听器的流程的简单例子。

```xml
 <process id="executionListenersProcess">
    <extensionElements>
      <activiti:executionListener class="org.activiti.examples.bpmn.executionListener.ExampleFieldInjectedExecutionListener" event="start">
        <activiti:field name="fixedValue" stringValue="Yes, I am " />
        <activiti:field name="dynamicValue" expression="${myVar}" />
      </activiti:executionListener>
    </extensionElements>

    <startEvent id="theStart" />
    <sequenceFlow sourceRef="theStart" targetRef="firstTask" />

    <userTask id="firstTask" />
    <sequenceFlow sourceRef="firstTask" targetRef="theEnd" />

    <endEvent id="theEnd" />
  </process>

```

```java
public class ExampleFieldInjectedExecutionListener implements ExecutionListener {

  private Expression fixedValue;

  private Expression dynamicValue;

  public void notify(ExecutionListenerExecution execution) throws Exception {
    execution.setVariable("var", fixedValue.getValue(execution).toString() + dynamicValue.getValue(execution).toString());
  }
}
```

ExampleFieldInjectedExecutionListener类串联了两个注入的属性。（一个是固定的，一个是动态的），把他们保存到流程变量'var'中。

```java
@Deployment(resources = {"org/activiti/examples/bpmn/executionListener/ExecutionListenersFieldInjectionProcess.bpmn20.xml"})
public void testExecutionListenerFieldInjection() {
  Map<String, Object> variables = new HashMap<String, Object>();
  variables.put("myVar", "listening!");

  ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("executionListenersProcess", variables);

  Object varSetByListener = runtimeService.getVariable(processInstance.getId(), "var");
  assertNotNull(varSetByListener);
  assertTrue(varSetByListener instanceof String);

  // Result is a concatenation of fixed injected field and injected expression
  assertEquals("Yes, I am listening!", varSetByListener);
}
```