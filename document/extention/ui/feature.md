## 功能点（Feature）
功能点是由 功能点描述、模板、视图、处理器四部分组成。在这里详细介绍下如何创建 功能点描述以及模板。其它部分后面会依次讲到。

功能点描述文件下包含 layout 和 views 两个必要属性
- layout 中是定义了多个区域（region），每个区域是键值对形式，区域名本功能中惟一就行，区域值对应的是 templates.html 中的id，这就规划了每个区域在html中位置。

- views 属性对应的是一个数组，可以定义多个 view 对象 name 是view名称，本功能中惟一就行，region 指定的是 layout中的 region名称，这样 view 跟 region 绑定后，它在html的显示位置也就确定了

```js
// feature.js
define({
    layout: {
        regions: {main: 'main', test: 'test'}
    },
    views: [{
        name: 'inline:tree', region: 'main'
    }, {
        name: 'test', region: 'test'
    }]
});

// templates.html
{{#layout}}
<div class="page-header position-relative">
  <h1>
    Treeview
  </h1>
</div>
<div id="main"></div>
<div id="test"></div>
{{/layout}}
...

```
> 注意
在cdeio中，app 是个全局变量，表示整个应用，而通过 app.startFeature(featurePath) 方法可以启动feature。

另外在功能点描述中还有个常用的属性 `extend`，顾名思义，它用来继承覆盖 feature 本身的方法，常用的有两个回调函数：
- onStart  当 app.startFeature('xxx/xxx')执行完的时候调用。
- onStop   当 app.stopFeature(feature) 执行完的时候调用。

```js
define({
    layout: {
        regions: {main: 'main', test: 'test'}
    },
    views: [{
        name: 'tree', region: 'main'
    }, {
        name: 'test', region: 'test'
    }],

    extend: {
        onStart: function() {
            
        },
        onStop: function() {
            
        }
    }

});
```

feature 还有更复杂的用法，请查阅 API， 关于view的具体用法请看 [下一章节](/document/extention/ui/mvc.html)