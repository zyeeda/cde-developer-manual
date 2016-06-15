# 中间捕获事件

所有中间捕获事件都使用同样的方式定义：

```xml
<intermediateCatchEvent id="myIntermediateCatchEvent" >
      <XXXEventDefinition/>
</intermediateCatchEvent>
```

中间捕获事件的定义包括：

* 唯一标识（流程范围内）
* 一个结构为 XXXEventDefinition 的 XML 子元素（比如TimerEventDefinition等）定义了中间捕获事件的类型。参考特定的捕获事件类型， 获得更多详情。