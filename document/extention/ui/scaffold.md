## Scaffold
界面的scaffold 跟自动生成scaffold是对应的，它的文件路径是 scripts/{featurePath}/scaffold.js 只要功能点的 `exports.enableFrontendExtension = true;` cdeio 就会加载这个 scaffold.js 文件，她主要用来事件处理、回调处理、格式处理等事情：

### 事件绑定
在cdeio平台中，一些复杂的功能无法自动生成，就需自定义实现，而事件绑定是连接自动生成与自定义的桥梁，与 MVC 中的 [Handler](/document/extention/ui/mvc.html) 是一个概念。示例中两个事件： 一个是绑定上报按钮点击事件；一个是审批事件，并且审批事件中展示了怎么显示自动生成的表单：

```js

define([
    'jquery',
], function($) {
    return {
        handlers: {
            sendProcess: function(){
                // 上报
            },
            audit: function() {
                var view = this.feature.views['form:audit'];
                app.showDialog({
                    view: view,
                    title: '审批',
                    buttons: [{
                        label: '确定',
                        status: 'btn-primary',
                        fn: function() {
                            // 业务逻辑
                        }
                    }]
                });
            }
        }
    };
});

```

### 回调函数
cdeio平台会提供很多种回调函数，让你能在自动生成的功能外增加你额外的需求。
```js

define([
    'jquery',
], function($) {
    return {
        afterShowDialog: function(dialogType, v, data){
            
        }
    };
});

```

- beforeShowDialog 弹出框弹出之前
- afterShowDialog  弹出框弹出之后
- beforeCloseDialog 弹出框关闭之前
- afterCloseDialog 弹出框关闭之后
- beforeShowInlineGridDialog 点击inline-grid上方按钮的弹出框弹出之前
- afterShowInlineGridDialog 点击inline-grid上方按钮的弹出框弹出之后
- beforeShowPicker picker弹出之前
- validInlineGridFormData gird验证函数 


### 格式渲染
通常 Grid 需要显示一些个性化的内容，那么通过 renderers 你能很容易解决，以下代码是让grid中的 disabled 字段显示有样式的禁用、启用效果。
```js
define([
    'jquery',
], function($) {
    return {
        renderers: {
            disabledRenderer: function(disabled) {
                if (disabled === true) {
                    return '<span class="red"><i class="icon-lock"></i>&nbsp;禁用</span>';
                }
                return '<span class="green"><i class="icon-unlock-alt"></i>&nbsp;启用</span>';
            }
        }
    };
});

```

渲染配置方式：

```js
exports.grid = {
    columns: [
        { name: 'realName', sortable: false },
        'accountName', 'email', 'mobile',
        { name: 'disabled', type: 'boolean', renderer: 'disabledRenderer' }
    ]
};
```

