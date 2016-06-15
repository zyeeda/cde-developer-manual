## feature
在一个功能点中，平台会默认生成 添加、编辑、查看的表单视图，但这些视图不可能满足我们的所有需求，有时候我们需要一些个性化的功能，但同时也需要用到表单视图，比如审批功能，需要填写审批意见等等。
通过 `exports.feature` 可以暴露一个动态feature 出来，而 feature 中的 views 可直接引用生成的表单，下面代码 `form:` 是固定前缀，audit是表单名称：

```js
exports.feature = {
    views: ['form:audit']
};
```

audit表单的配置如下：

```js

exports.fieldGroups = {
    audit: [
        {name: 'flowComment', label: '审批结果', type: 'dropdown', source: [{id: '1', text: '同意'}, {id: '2', text: '不同意'}]},
        {name: 'flowSuggestion', label: '审批意见', type: 'textarea', colspan: 2}
    ]
};

export.forms = {
    audit: {
        groups: [
            {name: 'audit', label: '审批信息'}
        ]
    }
}
```

### 使用
这里先提一下用法，通过 `feature.views['form:audit']` 能拿到审批这个视图，详细请看扩展 [事件绑定](/document/extention/ui/scaffold.html) 章节。