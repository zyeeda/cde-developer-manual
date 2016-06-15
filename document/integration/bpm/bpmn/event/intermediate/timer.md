# 定时中间捕获事件

### 描述

定时中间事件作为一个监听器。当执行到达捕获事件节点， 就会启动一个定时器。 当定时器触发（比如，一段时间之后），流程就会沿着定时中间事件的外出节点继续执行。

### 图形标记

定时器中间事件显示成标准中间捕获事件，内部是一个定时器小图标。

[![定时事件 example](./images/bpmn.intermediate.timer.event.png)](./images/bpmn.intermediate.timer.event.png)

### XML

定时器中间事件定义为标准中间捕获事件。 指定类型的子元素为timerEventDefinition元素。

```xml
<intermediateCatchEvent id="timer">
  <timerEventDefinition>
    <timeDuration>PT5M</timeDuration>
  </timerEventDefinition>
</intermediateCatchEvent>
```