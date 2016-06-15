# 基于事件网关

### 描述

基于事件网关允许根据事件判断流向。网关的每个外出顺序流都要连接到一个中间捕获事件。当流程到达一个基于事件网关，网关会进入等待状态：会暂停执行。 与此同时，会为每个外出顺序流创建相对的事件订阅。

注意基于事件网关的外出顺序流和普通顺序流不同。这些顺序流不会真的"执行"。相反，它们让流程引擎去决定执行到基于事件网关的流程需要订阅哪些事件。 要考虑以下条件：

* 基于事件网关必须有两条或以上外出顺序流。
* 基于事件网关后，只能使用intermediateCatchEvent类型。 （activiti不支持基于事件网关后连接ReceiveTask。）
* 连接到基于事件网关的intermediateCatchEvent只能有一条进入顺序流。

### 图形标记

基于事件网关和其他BPMN网关一样显示成一个菱形， 内部包含指定图标。

[![基于事件网关](./images/bpmn.event.based.gateway.notation.png)](./images/bpmn.event.based.gateway.notation.png)

### XML

用来定义基于事件网关的XML元素是 eventBasedGateway。

### 实例

下面的流程是一个使用基于事件网关的例子。当流程执行到基于事件网关时，流程会暂停执行。与此同时，流程实例会订阅警告信号事件，并创建一个10分钟后触发的定时器。这会产生流程引擎为一个信号事件等待10分钟的效果。如果10分钟内发出信号，定时器就会取消，流程会沿着信号执行。如果信号没有出现，流程会沿着定时器的方向前进，信号订阅会被取消。

[![基于事件example](./images/bpmn.event.based.gateway.example.png)](./images/bpmn.event.based.gateway.example.png)

```xml
<definitions id="definitions"
        xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
        xmlns:activiti="http://activiti.org/bpmn"
        targetNamespace="Examples">

        <signal id="alertSignal" name="alert" />

        <process id="catchSignal">

                <startEvent id="start" />

                <sequenceFlow sourceRef="start" targetRef="gw1" />

                <eventBasedGateway id="gw1" />

                <sequenceFlow sourceRef="gw1" targetRef="signalEvent" />
                <sequenceFlow sourceRef="gw1" targetRef="timerEvent" />

                <intermediateCatchEvent id="signalEvent" name="Alert">
                        <signalEventDefinition signalRef="alertSignal" />
                </intermediateCatchEvent>

                <intermediateCatchEvent id="timerEvent" name="Alert">
                        <timerEventDefinition>
                                <timeDuration>PT10M</timeDuration>
                        </timerEventDefinition>
                </intermediateCatchEvent>

                <sequenceFlow sourceRef="timerEvent" targetRef="exGw1" />
                <sequenceFlow sourceRef="signalEvent" targetRef="task" />

                <userTask id="task" name="Handle alert"/>

                <exclusiveGateway id="exGw1" />

                <sequenceFlow sourceRef="task" targetRef="exGw1" />
                <sequenceFlow sourceRef="exGw1" targetRef="end" />

                <endEvent id="end" />
</process>
</definitions>
```