# grid

### 描述

grid 是指表格信息。通过 grid 属性可以配置所有与表格相关的信息，如显示的列数、表头名称、是否有单（多）选按钮，是否显示序号等。


grid 的定义方式如下：

```js
exports.grid = {
	...
};
```

如果没有特殊要求则可以省略 grid 配置，省略 grid 配置的情况下 Cdeio 将默认采用 labels 中定义的字段作为表格的列进行显示。

### 属性

grid 包含如下属性：

* **columns**，数组类型，列表的列信息。
* **defaultOrder**，字符串类型，默认排序字段。格式为 "fieldName-asc|desc" ，fieldName 为字段名称，  asc|desc 代表排序方式（正序或倒序）。有多个排序字段可以使用 `,` 进行枚举，如 ```'name-asc, age-desc'```。
* **checkBoxColumn**， 布尔类型，是否在记录前显示单选按钮。默认为 `true`。
* **multiple**，布尔类型，是否可以多选。在 checkBoxColumn 为 `true` 的情况下生效。multiple 为 `true` 时会将单选按钮变为复选框。默认为 `false` 。
* **paginate**，布尔类型，是否分页。默认为 `true` 。
* **numberColumn**， 布尔类型，是否显示行数。默认为 `false` 。
* **fixedHeader**， 布尔类型，在列表出现滚动后进行滚动操作时是否固定表头。默认为 `false` 。
* **events**， 可以向 grid 添加事件，具体用法请参见 7.2.1 章节。

### columns 配置

columns 是一个数组，内部存放每一列的配置信息。

```js
exports.labels = {
    name: '姓名',
    sex: '性别',
    age: '年龄'
};

exports.grid = {
	columns: [
		'name',
		'sex',
		'age'
	]
}
```

默认情况下，表头信息即为 labels 中与之对应的名称。如果不想使用 labels 中定义的信息作为表头，则可以通过 header 属性进行指定，如下：

```js
exports.grid = {
	columns: [
		{name: 'name', header: '员工姓名'},
		'sex',
		'age'
	]
}
```

这样 name 列的表头将显示为 "员工姓名"。

除了 header 属性还可以对如下几个属性进行设置：

* **defaultContent**，当字段的值为 null 时显示的值。
* **sortable**，是否显示排序图标。默认 `false` 。
* **width**，列宽。
* **renderer**，渲染器，可以对字段的值进行渲染。具体用法请参见 7.2.1 章节。

