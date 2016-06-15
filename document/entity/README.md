# 面向实体编程

在 Cdeio 中，实体是一切工作的基础，所有的其它工作都是围绕着实体而展开的。


Cdeio 提供了四个个基础实体类且都实现了 Serializable ，这样所有继承自技术实体类的实体都将支持序列化。

### 1. DomainEntity
DomainEntity 中定义了主键 id 属性，采用 FallbackUUIDHexGenerator 主键生成器。

FallbackUUIDHexGenerator 内部采用 UUID 策略生成主键，与普通生成器不同的是 FallbackUUIDHexGenerator 首先会对实体的主键进行检测，只在实体主键不存在的情况下才会为实体生成主键。也就是说 FallbackUUIDHexGenerator 允许使用者自行设置实体的主键。

所有自定义业务实体类都应该继承 DomainEntity。

### 2. RevisionDomainEntity
继承自 DomainEntity，并且扩展了 creator 、 createdTime 、 lastModifier 和 lastModifiedTime 四个属性。系统在运行时，这些字段的值会根据登录用户的上下文状态自动填充。因此有需要记录修订信息的业务实体，应该继承此实体。

### 3. TreeNodeDomainEntity
继承自 DomainEntity，并且扩展了checked 、 icon 、 open 等属性，有树形层级结构的实体应继承此实体。

### 4. BpmDomainEntity
继承自 DomainEntity，并且扩展了 processDefinitionId 、 processInstanceId 、 submitter 和 status 四个属性。继承此实体后，在流程启动时流程引擎会自动使实体与流程实例建立关联（通过 processInstanceId 字段）。同时流程引擎将自动为实体维护发起人(submitter)、流程状态(status)和流程定义Id(processDefinitionId)信息。流程状态所体现的是流程中的任务环节，当任何环节变化时此流程状态也会随之改变。与流程有关的实体应继承此实体。
