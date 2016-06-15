# 人工任务

### 描述

人工任务用来设置必须由人员完成的工作。 当流程执行到人工任务，会创建一个新任务， 并把这个新任务加入到分配人或群组的任务列表中。

### 图形标记

人工任务显示成一个普通任务（圆角矩形），左上角有一个小用户图标。

[![人工任务图标](./images/bpmn.user.task.png)](./images/bpmn.user.task.png)

### XML

XML中的人工任务定义如下。id属性是必须的。 name属性是可选的。

```xml
<userTask id="theTask" name="Important task" />
```

人工任务也可以设置描述。实际上所有BPMN 2.0元素都可以设置描述。 添加documentation元素可以定义描述。

```xml
<userTask id="theTask" name="Schedule meeting" >
  <documentation>
          Schedule an engineering meeting for next week with the new hire.
  </documentation>
</userTask>
```

描述文本可以通过标准的java方法来获得：

```java
task.getDescription()
```

### 用户分配

人工任务可以直接分配给一个用户。 这可以通过humanPerformer元素定义。 humanPerformer定义需要一个 resourceAssignmentExpression来实际定义用户。 当前，只支持formalExpressions。

```xml
<process ... >
  ...
  <userTask id='theTask' name='important task' >
    <humanPerformer>
      <resourceAssignmentExpression>
        <formalExpression>kermit</formalExpression>
      </resourceAssignmentExpression>
    </humanPerformer>
  </userTask>
```

只有一个用户可以坐拥任务的执行者分配给用户。 在activiti中，用户叫做执行者。 拥有执行者的用户不会出现在其他人的任务列表中， 只能出现执行者的个人任务列表中。

直接分配给用户的任务可以通过TaskService像下面这样获取：

```java
List<Task> tasks = taskService.createTaskQuery().taskAssignee("kermit").list();
```

任务也可以加入到人员的候选任务列表中。 这时，需要使用potentialOwner元素。 用法和humanPerformer元素类似。注意它需要指定表达式中的每个项目是人员还是群组 （引擎猜不出来）。

```xml
<process ... >
  ...
  <userTask id='theTask' name='important task' >
    <potentialOwner>
      <resourceAssignmentExpression>
        <formalExpression>user(kermit), group(management)</formalExpression>
      </resourceAssignmentExpression>
    </potentialOwner>
  </userTask>
```

这会获取所有kermit为候选人的任务， 例如：表达式中包含user(kermit)。 这也会获得所有分配包含kermit这个成员的群组 （比如，group(management)，前提是kermit是这个组的成员， 并且使用了activiti的账号组件）。 用户所在的群组是在运行阶段获取的，它们可以通过 IdentityService进行管理。

如果没有显示指定设置的是用户还是群组， 引擎会默认当做群组处理。所以下面的设置与使用group(accountancy)效果一样。

```xml
<formalExpression>accountancy</formalExpression>
```

### Activiti对任务分配的扩展

当分配不复杂时，用户和组的设置非常麻烦。 为避免复杂性，可以使用人工任务的自定义扩展。

* **assignee属性**：这个自定义扩展可以直接把人工任务分配给指定用户。

```xml
<userTask id="theTask" name="my task" activiti:assignee="kermit" />
```

它和使用上面定义的humanPerformer 效果完全一样。
* **candidateUsers属性**：这个自定义扩展可以为任务设置候选人。

```xml
<userTask id="theTask" name="my task" activiti:candidateUsers="kermit, gonzo" />
```

它和使用上面定义的potentialOwner 效果完全一样。 注意它不需要像使用potentialOwner通过user(kermit)声明， 因为这个属性只能用于人员。

* **candidateGroups属性**：这个自定义扩展可以为任务设置候选组。

```xml
<userTask id="theTask" name="my task" activiti:candidateGroups="management, accountancy" />
```

它和使用上面定义的potentialOwner 效果完全一样。 注意它不需要像使用potentialOwner通过group(management)声明， 因为这个属性只能用于群组。

* candidateUsers 和 candidateGroups 可以同时设置在同一个人工任务中。

**注意**：虽然activiti提供了一个账号管理组件， 也提供了IdentityService， 但是账号组件不会检测设置的用户是否存在。 它嵌入到应用中，也允许activiti与其他已存的账户管理方案集成。

如果上面的方式还不满足需求，还可以使用创建事件的任务监听器 来实现自定义的分配逻辑：

```xml
<userTask id="task1" name="My task" >
  <extensionElements>
    <activiti:taskListener event="create" class="org.activiti.MyAssignmentHandler" />
  </extensionElements>
</userTask>
```

DelegateTask会传递给TaskListener的实现， 通过它可以设置执行人，候选人和候选组

```java
public class MyAssignmentHandler implements TaskListener {

  public void notify(DelegateTask delegateTask) {
    // Execute custom identity lookups here

    // and then for example call following methods:
    delegateTask.setAssignee("kermit");
    delegateTask.addCandidateUser("fozzie");
    delegateTask.addCandidateGroup("management");
    ...
  }

}
```

使用spring时，可以使用向上面章节中介绍的自定义分配属性， 使用表达式 把任务监听器设置为spring代理的bean， 让这个监听器监听任务的创建事件。 下面的例子中，执行者会通过调用ldapService这个spring bean的findManagerOfEmployee方法获得。 流程变量emp会作为参数传递给bean。

```xml
<userTask id="task" name="My Task" activiti:assignee="${ldapService.findManagerForEmployee(emp)}"/>
```
也可以用来设置候选人和候选组：

```xml
<userTask id="task" name="My Task" activiti:candidateUsers="${ldapService.findAllSales()}"/>
```

注意方法返回类型只能为String或Collection<String> （对应候选人和候选组）：

```java
public class FakeLdapService {

  public String findManagerForEmployee(String employee) {
    return "Kermit The Frog";
  }

  public List<String> findAllSales() {
    return Arrays.asList("kermit", "gonzo", "fozzie");
  }

}
```
