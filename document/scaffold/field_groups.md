# fieldGroups

### 描述
fieldGroups 用于对实体的属性（含关联实体的属性）进行分组，而分组中的各个属性是表示它在表单中的呈现方式，也就是下面所要提到的属性类型，而分组是可以在 form 任意混用的。

fieldGroups 中应包含一个或多个 group 信息，如果实体不需要多余分组，那么可以将所有的属性放到 defaults 分组中。默认情况下 form 将会读取 defaults 分组的内容进行显示。


### 基本配置

defaults 分组可以按如下的方式定义：

```js
exports.fieldGroups = {
    defaults: [
		'name',
		'sex',
		'age'
    ]
};
```

多个分组可以按照如下的方式定义：

```js
exports.fieldGroups = {
    basicInfo: [
		'name',
		'sex',
		'age'
    ],
    detailInfo: [
		'identification',
		'phone',
		'qq',
		'address'
    ]
    ...
};
```

### 属性类型

通过 type 属性可以指定属性的类型，默认的属性类型为 text 。

```js
exports.fieldGroups = {
    defaults: [
        {name: 'description', type: 'textarea'}
    ],
```

目前 Cdeio 所支持的类型有：

* **text**，纯文本类型。
* **textarea**，文本域类型。
* **dropdown**，下拉列表类型。
* **datepicker**，日期类型。
* **date-range**，日期范围。
* **number-range**，数值范围。
* **file-picker**，附件类型。
* **hidden**，隐藏类型。
* **inline-grid**，内联表格类型。
* **mask**，自定义规则。

[更多详细请看](/document/extention/ui/component.html)

