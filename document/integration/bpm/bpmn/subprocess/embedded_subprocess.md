# 嵌入式子流程

### 描述

嵌入式子流程（embedded-subprocess）是一个包含其他节点，网关，事件等等的节点。它自己就是一个流程，同时是更大流程的一部分。 子流程是完全定义在父流程里的 。

子流程有两种主要场景：

* 子流程可以使用**继承式建模**：多建模工具的子流程可以折叠，把子流程的内部细节隐藏，显示一个高级别的端对端的业务流程总览。
* 子流程会创建一个新的**事件作用域**： 子流程运行过程中抛出的事件，可以被子流程边缘定义的 边界事件捕获， 这样就可以创建一个仅限于这个子流程的事件作用范围。

使用子流程要考虑如下限制：

* 子流程只能包含**一个空开始事件**， 不能使用其他类型的开始事件。子路程必须至少有一个结束节点。注意，BPMN 2.0规范允许忽略子流程的开始和结束节点，但是当前activiti的实现并不支持。
* **顺序流不能跨越子流程的边界。**

### 图形标记

子流程显示为标准的节点，圆角矩形。 这时子流程是折叠的，只显示名称和一个加号标记， 展示了高级别的流程总览：

[![嵌入式子流程](./images/bpmn.subprocess.collapsed.png)](./images/bpmn.subprocess.collapsed.png)

这时子流程是展开的，子流程的步骤都显示在子流程边界内：

[![基于事件网关](./images/bpmn.subprocess.expanded.png)](./images/bpmn.subprocess.expanded.png)

使用子流程的主要原因，是定义对应事件的作用域。 下面流程模型演示了这个功能：调查软件/调查引荐任务需要同步执行， 两个任务需要在同时完成，在二线支持解决之前。 这里，定时器的作用域（比如，节点需要及时完成）是由子流程限制的。

[![基于事件网关](./images/bpmn.subprocess.with.boundary.timer.png)](./images/bpmn.subprocess.with.boundary.timer.png)


### XML

嵌入式子流程定义为subprocess元素。 所有节点，网关，事件，等等。它是子流程的一部分，需要放在这个元素里。

```xml
<subProcess id="subProcess">

  <startEvent id="subProcessStart" />

  ... other Sub-Process elements ...

  <endEvent id="subProcessEnd" />

 </subProcess>
```
