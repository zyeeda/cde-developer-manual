# scaffold

### 基本属性
##### 【样式】

需要将 style 设置为 `process`，这样与流程有关的设置才会生效，否则 Cdeio 只会将其当做普通的功能模块进行解析。

```js
exports.style = 'process';
```

##### 【流程定义键】

在流程中 processDefinitionKey 是流程的唯一标识（但并不区分具体的流程版本），用以与其他的流程进行区分。processDefinitionKey 需要与流程定义文件(.bpmn)中的 process 标记的 id 属性进行关联，这样在启动流程时系统会默认找到指定的流程并启动最新版本的流程实例。

```xml
<!-- 流程定义 -->
<process id="example-generate-bpm-task-userTask" >
	...
</process>
```

```js
// processDefinitionKey 与流程 id 相对应
exports.processDefinitionKey = 'example-generate-bpm-task-userTask';
```

### 表格
##### 【标签页】

默认的标签页分为 待认领的(claiming)、待办理的(todo)、已办理的(dong)、我发起的(launchByMe)和全部(all)共五个。如果默认的标签不满足需求，则可以自定义标签页。

##### 【活动标签页】

活动标签页是指进入功能模块中默认显示的标签页，需要用 activeTab 进行指定，默认值是 'claiming'，如果需要可以指定其他的标签页。

```js
// 指定'全部'标签页为活动标签页
exports.activeTab = 'all';
```

##### 【定制表格列】

默认情况下，所有标签下的表格列显示都是一致的，如果想为不同的表格指定不同的显示列，可以通过如下方式指定：

```js
exports.grid = {
	claiming: {
	    columns: [
	        'name', 'age', 'sex', 'phone', 'address', 'status'
	    ]
	},
	todo: {
	    columns: [
	        'name', 'age', 'sex', 'phone', 'status'
	    ]
	},
	done: {
	    columns: [
			'name', 'age', 'sex', 'status'
	    ]
	},
	launchByMe: {
	    columns: [
	        'name', 'age', 'status'
	    ]
	},
	all: {
	    columns: [
	        'name', 'status'
	    ]
	}
};
```

### 表单
##### 【show 表单中的标签页】

表单中共有三个内置的标签页，分别是：

* **基本信息**，是用来显示具体的业务信息的，用户可以自己定义所要显示的组及所要放置的位置等。系统默认显示此标签。
* **流程信息**，上半部分会显示流程的基本信息，下半部分会显示流程图。此标签的结构部能改变。
* **历史信息**，显示流程的审核过程记录，包括任务的创建时间、认领时间、执行人、完成时间、审核意见等。此标签的结构也不能改变。

##### 【内置组】

系统内默认包含任务、审核和流程三个组：

* **task-info-group** 即任务组，包含 任务名称、创建时间和执行人
* **task-audit-group** 即审核组，包含 是否通过和审核意见
* **process-info-group** 即流程组，包含流程名称、开始时间、结束时间、发起人和流程描述

任务组我们建议放在基本信息的最下面，审核组可以在审核任务时使用，流程组默认放在流程流程信息标签中。

```js
exports.forms = {
    show: {
        groups: [
        	// task-audit-group 和 process-info-group 是内置组，不需要在这里设置
            {name: 'base-info-group', columns: 2, labelOnTop: true, label: '基本信息'}
        ],
        tabs: [
        	// task-info-group 被放在基本信息的下面
            {title: '基本信息', groups: ['base-info-group', 'task-info-group']}
        ]
    }
}
```

##### 【complete 表单】

complete 表单是指用户在完成任务时所要填写的表单，默认情况下系统会统一为所有的任务提供统一的表单样式，定义方式如下：

```js
exports.forms = {
    complete: {
    	groups: [
    		// 指定可以编辑的基本业务信息
    		{name: 'base-info-group', columns: 2, labelOnTop: true, label: '基本信息'},
    		// 审核意见
    		{name: 'task-audit-group', columns: 2, labelOnTop: true, label: '审核信息'}
    	]
    }
};
```
如果需要为流程的某个任务节点定制表单，则需要如下配置：

```xml
<userTask id="user_task_01" name="入职初审" >
  ...
</userTask>
```

```js
exports.forms = {
	/**
	 * 为具体的任务指定表单样式
	 * 格式为 complete-[taskId]，在 complete- 后面指定具体的 taskId 即可
	 */
    'complete-user_task_01': {
    	groups: [
    		// 指定可以编辑的基本业务信息
    		{name: 'base-info-group', columns: 2, labelOnTop: true, label: '基本信息'},
    		// 审核意见
    		{name: 'task-audit-group', columns: 2, labelOnTop: true, label: '审核信息'}
    	]
    }
};
```