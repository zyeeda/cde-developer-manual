# forms

### 描述

forms 是指表单信息，目前 Cdeio 默认支持新增、查看、编辑三种类型表单，依次供新增、查看、编辑功能使用。如果需要其他类型的表单，可以自行定制，[定制请看](/document/scaffold/feature.html)

### 基本配置

* forms 的定义方式如下， groups 中可以指定一个或多个组：
```js
exports.forms = {
	defaults: {
	    groups: [
	    	...
	    ]
	}
};
```
如果 scaffold 中没有定义 forms ，则 Cdeio 默认 defaults 组为 froms 的分组信息。



* 如果要为新增、查看、编辑表单分别指定不同的分组信息，则可以按照如下的方式进行定义：
```js
exports.forms = {
	// 指定新增表单的分组信息
	add: {
	    groups: [
	    	...
	    ]
	},
	// 指定查看表单的分组信息
	show: {
	    groups: [
	    	...
	    ]
	},
	// 指定编辑表单的分组信息
	edit: {
	    groups: [
	    	...
	    ]
	}
};
```


* 可以为表单指定大小，为表单中的组信息指定名称、显示列数（默认为一列）、可见性等：
```js
exports.forms = {
    add: {
    	// 指定表单大小
        size: 'large',
        groups: [
            // 指定组的名称
            {name: 'basicGroup', label: "基本信息"},
        	// 指定组的列数
            {name: 'defaults', columns: 2},
            // 指定组的可见性
            {name: 'detailGroup', visible: false}
        ]
    }
}
```

### 多标签页

表单中可以定义多个标签，定义方式如下：

```js
exports.forms = {
    defaults: {
        tabs: [
            {title: '基本信息', groups: ['basicGroup']},
            {title: '详细信息', groups: ['detailGroup']}
        ]
    }
};
```
