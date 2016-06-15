# haveFilter

haveFilter 用于条件过滤查询，开启后将会在表格的右上角出现查询过滤条件按钮，点击按钮后将出现过滤查询界面。需要使用哪些字段进行过滤查询可以自行设置。

haveFilter 只能在 style 为 grid 的情况下使用。

### 配置

1. 开关设置
使用条件过滤查询首先需要开启 haveFilter 开关，如下：
```js
exports.haveFilter = true;
```
2. 设置过滤组
```js
exports.fieldGroups = {
    defaults: [
        'name',
        'sex',
    ]
    , myFilter: [
        'name'
    ]
};
```

3. 定义以 `filter` 命名的form 

filter 中可以指定任意 fieldGroups 中定义的组，defaults 组也是可以的。有关 fieldGroups ，[详细请看](/document/scaffold/field_groups.html)
```js
exports.forms = {
    defaults: {
        groups: [{name: 'defaults', columns: 2}],
    }
    // 此处为 filter 配置，可以为过滤器指定一个或多个组
    , filter: {
        groups: [{
            name: 'myFilter',
            columns: 4
        }]
    }
};
```
