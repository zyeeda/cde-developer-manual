# labels

labels 用于指定实体属性在列表和表单中所显示的名称。在 labels 中没有指定名称的属性将显示其英文属性名称。

如果表格或表单中不想使用 labels 中所指定的名称，则可以在表格或表单中进行定制。具体使用方法请参考 grid 和 form 章节。

labels 的配置非常简单，看起来像这个样子：

```js
exports.labels = {
    name: '姓名',
    sex: '性别',
    age: '年龄'
};
```
