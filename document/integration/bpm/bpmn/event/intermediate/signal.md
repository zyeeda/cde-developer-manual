# 信号中间触发事件

### 描述

中间触发信号事件为定义的信号抛出一个信号事件。

在activiti中，信号会广播到所有激活的处理器中（比如，所以捕获信号事件）。信号可以通过同步和异步方式发布。

* 默认配置下，信号是**同步**发送的。就是说，抛出事件的流程实例会等到信号发送给所有捕获流程实例才继续执行。 捕获流程实例也会在触发流程实例的同一个事务中执行，意味着如果某个监听流程出现了技术问题（抛出异常），所有相关的实例都会失败。
* 信号也可以**异步**发送。这时它会在到达抛出信号事件后决定哪些处理器是激活的。对这些激活的处理器，会保存一个异步提醒消息（任务），并发送给jobExecutor。

### 图形标记

中间信号触发事件显示为普通中间事件（圆圈套圆圈），内部又一个信号小图标。信号图标是黑色的（有填充）， 表示触发语义。

[![信号事件 example](./images/bpmn.intermediate.signal.throw.event.png)](./images/bpmn.intermediate.signal.throw.event.png)

### XML

消息中间事件定义为标准中间触发事件。 指定类型的子元素是signalEventDefinition元素。

```xml
<intermediateThrowEvent id="signal">
  <signalEventDefinition signalRef="newCustomerSignal" />
</intermediateThrowEvent>
```

异步信号事件如下所示：

```xml
<intermediateThrowEvent id="signal">
  <signalEventDefinition signalRef="newCustomerSignal" activiti:async="true" />
</intermediateThrowEvent>
```