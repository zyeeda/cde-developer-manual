## 视图（View）& 模型 (model) &  处理器 (Handler)

### 视图（View）
前面讲到，视图是feature的一部分，也是最重要最复杂的一部分。那么 feature 中定义的 view 当然只是定义的view的名称，具体view的配置不需要单独有个描述文件，在feature.js级下建个views目录，然后创建 view名称相同的js文件：
```js
// views/test.js
define({
    events: {
        'click btn': 'clickIt'
    },
    avoidLoadingHandlers: true,
    extend: {
        serializeData: function(su) {
            var data = su.apply(this);
            data.abc = [
                {name: 'aaa', sex: 'x', id: 'b'},
                {name: 'bbb', sex: 'x', id: 'c'},
                {name: 'ccc', sex: 'x', id: 'd'},
                {name: 'ddd', sex: 'x', id: 'e'},
                {name: 'eee', sex: 'x', id: 'f'},
                {name: 'fff', sex: 'x', id: 'g'}
            ];
            return data;
        }
    }
});
 
```

同时 view 是有模板的，同样也是定义在 templates.html，`{{view "test"}}` 里面的内容是 test 这个view的模板，里面的表达式用户请参照 [Handlebars](http://www.handlebars.com):

```html
{{/layout}}
...
{{layout}}

{{#view "test"}}
<button id="btn" class="btn">Click Me</button><br/>
    {{#each abc}}
        name: {{name}}, sex: {{sex}}, id: {{id}}<br/>
    {{/each}}
{{/view}}
```

extend方法：
- serializeData 数据处理
- afterRender   view 渲染后事件
- templateHelpers  模板数据绑定

API
- [backbone.marionettev0.8.1](http://marionettejs.com/docs/v0.9.0/)
- [backbonev0.9.2](http://docs.appcelerator.com/backbone/0.9.2/)

### 模型 (Model)
另一方面，它又与 Model、Handler 组成 MVC 模式，共同组成界面，这样看来 feature 不过是 view 的一个管理容器，界面的复杂度完全由 view 掌控。view 除了模板外，还需要后台数据，Model 就是用来与后台交互的，而在cdeio中，model 其实是一个url，通常是从featurePath开始，model 正好对应服务中的 [Router](/document/extention/service/router/README.html)
```js
// views/main.js
define({
    events: {
        'click parent-*': 'toggleSubMenu'
    },
    
    avoidLoadingHandlers: true,
    
    model: 'system/menu'

});
```

###  处理器 (Handler)
用来绑定界面的事件，在处理器中，你可以任意发挥，调用 cdeio平台 所提供的各种 API 来完成你所需的功能。
```js
// handlers/test.js
define({
    clickIt: function() {
        var app = this.feature.module.getApplication();
        app.loadFeature('test/forms/frontend').done(function(feature) {
            app.showDialog({
                title: 'Hello',
                view: feature.views.simple,
                buttons: []
            });
        });
    }
});

```
