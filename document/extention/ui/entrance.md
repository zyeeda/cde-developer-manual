## 应用程序入口

前端界面 使用 RequireJS 管理所有前端的 JavaScript 脚本，因此在起始页面中需要引入 RequireJS 并通过 data-main 属性，指定其加载的应用程序入口文件（也就是上面代码中的 scripts/main.js 文件），RequireJS 会自动执行该文件。其大致内容如下：

```js
// main.js
define([
        'jquery',
        'cdeio/cdeio',
        'application',
    ], function($, cdeio) {

        $(function() {
            var app = window.app = cdeio.startApplication('application');
        });
    });
```

通过 define 方法引入了三个脚本，并于回调方法中，在 DomReady 事件里启动应用程序，
cdeio.startApplication('application') 调用的是application.js文件，具体如下：

```js
// application.js
define([
    'jquery',
    'underscore',
    'cdeio/cdeio',
    'cdeio/applications/default',
    'cdeio/core/config',
    'cdeio/vendors/ace-elements',
    'cdeio/vendors/ace'
], function($, _, cdeio, createDefault, config) {

    return function(options) {
        $.ajaxSetup({
            cache: false
        });

        var app, deferred = $.Deferred();

        options = _.extend(options, { useDefaultHome: false });
        app = createDefault(options);
        app.config = config;
        
        app.done(function() {
            if (location.hash) {
                app.startFeature('main/viewport', { container: $(document.body), ignoreExists: true }).done(function() {
                    deferred.resolve();
                });
            } else {
                deferred.resolve();
            }
        });

        app.addPromise(deferred.promise());
        return app;
    };

});

```
`app.startFeature` 是用来开始一个功能，此文件逻辑根据实际项目需求自由发挥。 application.js 一般是项目首次进入的呈现功能，也可以当作是项目布局（常用的是有上面logo，左边菜单，右边功能这种上），而进入之后的项目其它功能，通常借助 [前端路由器](/document/extention/ui/router.html) 来开启。