# 排他网关

### 描述

排他网关（也叫异或（XOR）网关，或更技术性的叫法 基于数据的排他网关），用来在流程中实现决策。 当流程执行到这个网关，所有外出顺序流都会被处理一遍。 其中条件解析为true的顺序流（或者没有设置条件，概念上在顺序流上定义了一个'true'） 会被选中，让流程继续运行。

注意这里的外出顺序流 与BPMN 2.0通常的概念是不同的。通常情况下，所有条件结果为true的顺序流 都会被选中，以并行方式执行，但排他网关只会选择一条顺序流执行。 就是说，虽然多个顺序流的条件结果为true， 那么XML中的第一个顺序流（也只有这一条）会被选中，并用来继续运行流程。 如果没有选中任何顺序流，会抛出一个异常。

### 图形标记

排他网关显示成一个普通网关（比如，菱形图形）， 内部是一个“X”图标，表示异或（XOR）语义。 注意，没有内部图标的网关，默认为排他网关。 BPMN2.0 规范不允许在同一个流程定义中同时使用没有X和有X的菱形图形。

[![排他网关图标](./images/bpmn.exclusive.gateway.notation.png)](./images/bpmn.exclusive.gateway.notation.png)

### XML

排他网关的XML内容是很直接的：用一行定义了网关， 条件表达式定义在外出顺序流中。 参考条件顺序流 获得这些表达式的可用配置。

参考下面模型实例：

[![排他网关业务模型](./images/bpmn.exclusive.gateway.png)](./images/bpmn.exclusive.gateway.png)

它对应的XML内容如下：

```xml
<exclusiveGateway id="exclusiveGw" name="Exclusive Gateway" />

<sequenceFlow id="flow2" sourceRef="exclusiveGw" targetRef="theTask1">
  <conditionExpression xsi:type="tFormalExpression">${input == 1}</conditionExpression>
</sequenceFlow>

<sequenceFlow id="flow3" sourceRef="exclusiveGw" targetRef="theTask2">
  <conditionExpression xsi:type="tFormalExpression">${input == 2}</conditionExpression>
</sequenceFlow>

<sequenceFlow id="flow4" sourceRef="exclusiveGw" targetRef="theTask3">
  <conditionExpression xsi:type="tFormalExpression">${input == 3}</conditionExpression>
</sequenceFlow>
```