## 前端路由器
前端路由器，显然是能够控制url指向某个函数的。
在现在的单页应用中，所有的操作、内容都在一个页面上呈现，那么一个页面中的向前向后的操作怎么能定位到当前页面，前端路由器是通过hash的方式（即#help）来完成。当浏览器地址发生改变时，对应的路由函数会触发，具体实现请看：

```js
define([
    'backbone'
], function (Backbone) {
    return {
        routes: {
            "help":                 "help",    // #help
            "search/:query":        "search",  // #search/kiwis
            "search/:query/p:page": "search",  // #search/kiwis/p7
            'feature/*name': 'startFeature'    // #feature/demos/test
        },
        help: function() {
            ...
        },
        search: function(query, page) {
            ...
        },
        startFeature: function(name) {
            ...
        },
    };
});
```

- 字符串完全匹配，比如 "help": "help"
- 用“:” 来匹配两个“/”之间的字符串
- 用“*” 可以匹配后面所有的url路径

更详细文档请看 [Backbone#Router](http://backbonejs.org/#Router)