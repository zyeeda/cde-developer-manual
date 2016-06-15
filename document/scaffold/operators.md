## operators

用于配置功能按钮，在 scaffold.js 中声明如下：

```js
exports.operators = {
    
};
```

### 基础
遵守平台的最简原则，在没有 exports.operators 情况下，平台会提供一个默认配置，所有功能默认拥有五个功能按钮，依次是 `添加（add）`、`查看（show）`、`编辑（edit`）、`删除（del）`、`刷新（refresh）` :

![image](/assets/scaffold/operators/operators1.png)

以下是平台默认生成的五个按钮配置：

```js
exports.operators = {
    add: {
        label: '添加',
        icon: 'icon-plus',
        group: '10-add',
        style: 'btn-success',
        show: 'always',
        order: 100.0
    },
    show: {
        label: '查看',
        icon: 'icon-eye-open',
        group: '20-selected',
        style: 'btn-grey',
        show: 'single-selected',
        order: 100.0
    },
    edit: {
        label: '编辑',
        icon: 'icon-edit',
        group: '20-selected',
        style: 'btn-primary',
        show: 'single-selected',
        order: 200.0
        },
    del: {
        label: '删除',
        icon: 'icon-minus',
        group: '20-selected',
        style: 'btn-danger',
        order: 300.0
    },
    refresh: {
        label: '刷新',
        icon: 'icon-refresh',
        group: '30-refresh',
        style: 'btn-purple',
        show: 'always',
        order: 100.0
    }
}
```

* `label`  按钮显示名称

* `show`  按钮显示模式，有以下几种，通常我们对数据选中或没选中的情况下，会呈现不一样的功能按钮：
    - always 任何时候都显示
    - unselected 没有选中数据的时候显示
    - single-selected 选中单条数据的时候显示
    - multi-selected 选中多条数据的时候显示
    - selected 选中单条或多条数据都显示，当不设置show属性的时候，为`默认值`
  
* `style`  按钮显示颜色，比如 btn-primary 为蓝色调，如果不设置，默认值为 btn-primary，更多请看[bootstrap2#按钮](http://v2.bootcss.com/base-css.html#buttons)
* `icon`  按钮显示图标，比如 icon-file 为文件图标，如果不设置，默认值为 icon-file 更多请看[bootstarp2#图标](http://v2.bootcss.com/base-css.html#icons)

* `group` 分组属性，格式为 `数字-xx`，数字部分用于每个分组的排序。

* `order` 排序属性, 用于同一分组中按钮的顺序

> 注意：  
如果需要将某个默认按钮去掉，请将其设置为false，例如：del: false

### 高级
当然功能按钮不仅仅限于以上五个，参照以上配置，你可以很简单的配置出自定义的按钮：
```js
exports.operators = {
    sendProcess: { label: '上报', style: 'btn-info', icon: 'icon-envelope-alt', group: '20-selected', order: 310 }
};
```

sendProcess 为按钮名称， 用于按钮事件绑定，详细请看 [事件绑定](/document/extention/ui/scaffold.html) 章节。