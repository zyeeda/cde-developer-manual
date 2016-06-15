# 定义流程

目前定义流程有两种方式：

* **手动编写 xml **，并将 xml 以 .bpmn 后缀命名。此种方式需要对 bpmn 有深入的了解，一般情况下我们不建议这样做。
* **使用 Eclipse 插件 Activiti BPMN 2.0 designer **，通过 Activiti BPMN 2.0 designer 可以以绘图的方式设计流程，同时能够精确的定位每一个图元的坐标。

### 定义流程的要点

1. 流程需要至少有一个开始和一个结束节点。子流程内部只能有一个开始节点，但至少需要一个结束节点。

1. 需要为任务指定参与者。我们建议使用 Activiti 扩展的标记来指定参与者，bpmn 所提供的方式有些过于复杂。
	* 指定任务执行人
	```xml
	<userTask id="theTask" name="my task" activiti:assignee="luffy"/>
	```
	* 指定任务候选人
	```xml
	<userTask id="theTask" name="my task" activiti:candidateUsers="luffy, zoro"/>
	```
	* 指定任务候选组
	```xml
	<userTask id="theTask" name="my task" activiti:candidateGroups="management"/>
	```
机构和角色都可以通过指定后寻找的方式进行指定。

1. 使用条件分支即``` exclusiveGateway ``` 节点时，多个条件需要互斥，这样降低程序调试的复杂度。