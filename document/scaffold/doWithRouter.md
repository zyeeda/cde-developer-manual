## doWithRouter
在这里提一下路由器(Router)的概念，如果你写过node.js程序，用过它的 express 框架，我想你对路由器就不会陌生。
路由器用来分发你的界面请求，会将你的界面请求根据 URL 及请求方式分发到对应的函数上去，而对应的URL我们称做为路由，路由函数不处理任何业务逻辑，更多工作就是获取界面请求数据或者拼装数据返给界面。

cdeio实现的路由器支持 restful 方式，暂时，router 仅提供以下四个创建路由方法：
- get    用于查询操作
- post   用于创建操作
- put    用于更新操作
- del    用于删除操作

更详细介绍请看 [Router](/document/extention/service/router/README.html)

### 基础
通过 `exports.doWithRouter` 暴露方法，能拿到一个 router 输入参数，而扩展点就是router这个对象。
遵守平台的最简原则，在没有 exports.doWithRouter 情况下，平台会提供一个默认路由器。如下： 

```js
exports.doWithRouter = function(router) {

    router.get('/', function (request, id) {
        ...
    });

    router.get('/:id', function (request, id) {
        ...
    });

    router.post('/', function (request) {
        ...
    });

    router.put('/:id', function (request, id) {
        ...
    });

    router.del('/:id', function (request, id) {
        ...
    });

};
```
如果你想覆写这五个中的一个，只需要用 router 创建 url对应的路由就行了，当然我们不建议这么做。更建议覆写 service 对应方法，后续会讲到。

## 高级
路由器虽默认只提供五个路由，如果你需要，你可以很轻松创建多一个路由，根据你的操作方式用router对象不同的方法创建一个路由，以下示例就是修改密码的路由：
```js
router.put('/passwd/:id', function (request, id) {
    ...
});
```

>注意
在scaffold.js中通过 doWithRouter 创建的路由，通常要在访问路径前加上scaffold标识，例如以上路由的请求方式为put，请求url为：invoke/scaffold/demos/user/passwd/xxxxxxx ，invoke是固定的前缀，demos/user是当前feature路径。