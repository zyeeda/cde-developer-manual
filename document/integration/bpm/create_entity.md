# 定义实体

使用流程的实体，需要继承 BpmDomainEntity 实体。BpmDomainEntity 包含如下信息：

```java
// 流程定义Id(含具体版本)
protected String processDefinitionId;
// 流程实例Id
protected String processInstanceId;
// 发起人
protected String submitter = null;
// 流程状态
protected String status = null;
```

在流程启动后流程引擎会自动使实体与流程实例建立关联（通过 processInstanceId 字段）。同时流程引擎将自动为实体维护发起人(submitter)、流程状态(status)和流程定义Id(processDefinitionI)信息。流程状态所体现的是流程中的任务环节，当任何环节变化时此流程状态也会随之改变。

具体关联哪个流程是在 ```@Scaffold``` 标记所对应的 scaffold.js 中进行定义的

```js
exports.processDefinitionKey = 'example-generate-bpm-task-userTask';
```

除了继承 BpmDomainEntity 外，关于流程就不再需要其他的任何设置了。当然可以向其他业务实体一样进行关联、验证等设置。

```java
@Entity
@Table(name = "CDEIO_EMPLOYEE")
@Scaffold("/employee")

public class Employee extends BpmDomainEntity {
	// 具体的业务配置
	...
}
```
