# 消息中间捕获事件

### 描述

一个中间捕获消息事件，捕获特定名称的消息。

### 图形标记

中间捕获消息事件显示为普通中间事件（圆圈套圆圈），内部是一个消息小图标。消息图标是白色的（无填充）， 表示捕获语义。

[![消息事件 example](./images/bpmn.intermediate.message.catch.event.png)](./images/bpmn.intermediate.message.catch.event.png)

### XML

消息中间事件定义为标准中间捕获事件。 指定类型的子元素是messageEventDefinition元素。

```xml
<intermediateCatchEvent id="message">
  <messageEventDefinition signalRef="newCustomerMessage" />
</intermediateCatchEvent>
```