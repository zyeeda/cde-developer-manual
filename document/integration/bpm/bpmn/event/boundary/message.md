# 消息边界事件

### 描述

节点边界上的中间捕获消息， 或简称边界消息事件，根据引用的消息定义捕获相同消息名称的消息。

### 图形标记

边界消息事件显示成一个普通的中间事件（圆圈里有个小圆圈），位于节点边缘，内部是一个消息小图标。消息图标是白色（无填充）， 表示捕获语义。

[![消息事件 example](./images/bpmn.boundary.message.event.png)](./images/bpmn.boundary.message.event.png)

**注意**，边界消息事件可能是中断（右侧）或非中断（左侧）的。

###　XML

边界消息事件定义为标准的边界事件：

```xml
<boundaryEvent id="boundary" attachedToRef="task" cancelActivity="true">
          <messageEventDefinition messageRef="newCustomerMessage"/>
</boundaryEvent>
```
