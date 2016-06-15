# entityLabel

entityLabel 是指实体用于显示在页面（新增、查看、编辑）上的标题名称。默认情况下 entityLabel 为空，页面的标题只显示为新增、查看或编辑。设置 entityLabel 后，页面的标题将按“新增xxx”的格式显示。

entityLabel 的配置非常简单，看起来像这个样子：

```js
exports.entityLabel = "xxx";
```

例如，我们按照下面的方式配置 entityLabel

```js
exports.entityLabel = "员工信息";
```

则新增、查看和编辑页面的标题应显示为：“新增员工信息”、“查看员工信息”和“编辑员工信息”。