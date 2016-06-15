## picker
使用 `exports.picker` 暴露配置是供字段组件 grid-picker 、tree-picker 使用的， 例如 用户实体关联了部门实体，是多对一关联，那么在用户表单界面，部门字段会呈现出picker组件，默认是 grid-picker，如果部门是树状结构的，那么是tree-picker，而以下配置是定义在部门的scaffold.js中的：

* gird-picker 组件需要的配置
```js
// department.feature/scaffold.js
exports.picker = {
    grid: {
        columns: [
            { name: 'name', header: '名称'},
            { name: 'description', header: '描述'}
        ]
    }
};
```
>注意
此处gird的配置跟之前讲到的scaffold中 exports.grid 配置是完全一样的，[详细请看](/document/scaffold/grid.html)
* tree-picker 组件需要的配置
```js
// department.feature/scaffold.js
exports.picker = {
    tree: {
        check: {
            chkboxType: {'Y': '', 'N': ''}
        }
    },
};
```
>注意
此处tree的配置跟之前讲到的scaffold中的 exports.tree 配置是完全一样的，[详细请看](/document/scaffold/tree.html)