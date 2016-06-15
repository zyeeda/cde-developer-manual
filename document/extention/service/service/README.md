# Service

### 描述

service 在本框架中充当业务逻辑层的角色，所有与业务有关的代码都应该在这里完成。但是 service 中的代码不应该参与访问底层存储或其它基础组件，这部分工作应该是由 manager 来完成的，service 应该只是调用 manager 里面的方法而已。为了使得 marker 可以自动注入 service，要求每个 service 文件必须导出一个名为 createService 的方法，marker 在每次注入的时候都会调用该方法，生成一个新的 service 实例，因此 service 的注入模式是 protoytype 的，而不是 singleton 的。


### 使用 service


使用 service 需要用 marker 向方法注入 service 对象再使用。

```js
mark('services', 'modulePath1:serviceName1', 'modulePath2:serviceName2').on(function (service1, service2) {
    ...
});
```

### 事务

请参见 mark 章节 tx 部分。

### 创建 service

如果 Cdeio 所提供的 service 无法满足需要，则可以对 service 进行扩展。以 system/account 功能为例，首先需要在 app/system/account.feature 下创建 service.js 文件，内容下：

```js
var {mark} = require('cdeio/mark');
var {Account} = com.zyeeda.cdeio.commons.organization.entity;

exports.createService = function() {

    var service = {
		changePassword: mark('managers', Account).mark('tx').on(function (accountMgr) {
			...
		})

    };

    return service;
};
```

通过mark注入此 AccountService，就可以调用到 `changePassword` 方法了，
```js
var {mark} = require('cdeio/mark');
mark('services', 'system/account').on(function (accountService) {
    accountService.changePassword();
    ...
});
```

但这种方式的Service不会包含平台所提供的原有方法，（如 get 、 create、update 、list 等），那么有一种方式可以把平台原有方法继承过来，那就是在 scaffold.js里定义 `exports.serivce` 这样会把平台默认service注入到baseSvc中，通过 underscore的extend方法就可以轻松继承了。

```js
var {mark} = require('cdeio/mark');
var _ = require('underscore');

exports.service = mark('services', 'system/account').on(function(accountSvc, baseSvc){
    return _.extend(baseSvc, accountSvc);
});
```
> 注意：如果 accountSvc 中定义了名为create 等平台中已有的方法，则是覆写平台的原有方法哦。
> 