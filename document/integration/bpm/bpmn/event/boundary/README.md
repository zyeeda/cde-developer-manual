# 边界事件

边界事件都是捕获事件，它会附在一个环节上。 （边界事件不可能触发事件）。这意味着，当节点运行时， 事件会监听对应的触发类型。 当事件被捕获，节点就会中断， 同时执行事件的后续连线。

所以边界事件的定义方式都一样：

```xml
<boundaryEvent id="myBoundaryEvent" attachedToRef="theActivity">
      <XXXEventDefinition/>
</boundaryEvent>
```

边界事件使用如下方式进行定义：

* 唯一标识（流程范围）
* 使用caught属性引用事件衣服的节点。注意边界事件和它们附加的节点在同一级别上。（比如，边界事件不是包含在节点内的）。
* 格式为XXXEventDefinition的XML子元素 （比如，TimerEventDefinition，ErrorEventDefinition，等等） 定义了边界事件的类型。参考对应的边界事件类型， 获得更多细节。