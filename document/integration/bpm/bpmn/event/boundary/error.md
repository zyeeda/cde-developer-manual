# 错误边界事件

### 描述

节点边界上的中间捕获错误事件， 或简写成**边界错误事件**， 它会捕获节点范围内抛出的错误。

定义一个边界错误事件，大多用于内嵌子流程， 或调用节点，对于子流程的情况，它会为所有内部的节点创建一个作用范围。 错误是由错误结束事件抛出的。 这个错误会传递给上层作用域，直到找到一个错误事件定义向匹配的边界错误事件。

当捕获了错误事件时，边界任务绑定的节点就会销毁， 也会销毁内部所有的执行分支 （比如，同步节点，内嵌子流程，等等）。 流程执行会继续沿着边界事件的外出连线继续执行。

### 图形标记

边界错误事件显示成一个普通的中间事件（圆圈内部有一个小圆圈） 放在节点的标记上，内部有一个错误小图标。错误小图标是白色的， 表示它是一个捕获事件。

[![错误事件 example](./images/bpmn.boundary.error.event.png)](./images/bpmn.boundary.error.event.png)

### XML

边界错误事件定义为普通的边界事件：

```xml
<boundaryEvent id="catchError" attachedToRef="mySubProcess">
  <errorEventDefinition errorRef="myError"/>
</boundaryEvent>
```

和错误结束事件一样， errorRef引用了process元素外部的一个错误定义：

```xml
<error id="myError" errorCode="123" />
...
<process id="myProcess">
...
```

errorCode用来匹配捕获的错误：

* 如果没有设置errorRef，边界错误事件会捕获 所有错误事件，无论错误的errorCode是什么。
* 如果设置了errorRef，并引用了一个已存的错误， 边界事件就只捕获错误代码与之相同的错误。
* 如果设置了errorRef，但是BPMN2.0中没有定义错误，errorRef就会当做errorCode使用（和错误结束事件的用法类似）。

### 实例

下面的流程实例演示了如何使用错误结束事件。当完成'审核盈利'这个用户任务是，如果没有提供足够的信息， 就会抛出错误，错误会被子流程的边界任务捕获，所有'回顾销售'子流程中的所有节点都会销毁。（即使'审核客户比率'还没有完成）， 并创建一个'提供更多信息'的用户任务。

[![错误事件 example](./images/bpmn.boundary.error.example.png)](./images/bpmn.boundary.error.example.png)