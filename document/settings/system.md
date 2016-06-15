# 系统配置
它好比数据字典，在做项目时，我们会经常用到配置项，而有时会根据环境不同，这些配置项也不尽相同，这类配置我们称之为系统配置，比如 项目名称，项目附件上传路径等，为了达到通用性，一些共用的配置，都可以将其归为系统配置。

在 cdeio 平台中，这些配置主要体现在 WEB-INF/app/config.js 文件中，而系统配置分两类配置，后端使用配置、前端使用配置。

```js
var {SecurityUtils} = org.apache.shiro;
var {FrontendSettingsCollector} = com.zyeeda.cdeio.web;
var {mark} = require('cdeio/mark');

exports.cdeio = {
    entityPackages : [
        'com.zyeeda.cdeio.commons.resource.entity'
    ],
    orms: [
        'src/main/resources/META-INF/mappings/role.orm.xml',
    ],
    dateTimeFormat: 'yyyy-MM-dd hh:mm:ss',
    disableAuthz: true
};

FrontendSettingsCollector.add('collector', 'registered in collector');

exports.frontendSettings = {
    'application_name': 'cdeio.application.name',
    'application_logo': 'cdeio.application.logo',
    'test': 'collector',
    currentUser: function(context) {
        var subject = SecurityUtils.getSubject(),
            p = subject.getPrincipal(),
            isAdmin;

        if (p == null) {
            return {};
        }

        if('admin' === (p.getAccountName()).toLowerCase()){
            isAdmin = true;
        }

        return {
            accountName: p.getAccountName(),
            realName: p.getRealName() || p.getAccountName(),
            email: p.getEmail(),
            isAdmin: isAdmin,
            photo: 'assets/images/avatars/user.jpg'
        };
    },
    signOutUrl: mark('beans', 'openIdProvider').on(function(openIdProvider, context) {
        return openIdProvider.getSignOutUrl();
    })
};

```

### `export.cdeio` 为后端使用配置
* `entityPackages` - 配置实体包路径
* `orms` - 配置 orm 文件
* `dateTimeFormat` - 配置项目日期和时间格式
* `disableAuthz` - 配置项目是否要启用权限控制

### `export.frontendSettings` 为前端使用配置
在前端用 app.settings中可以拿到返回的值，或者 {{settings.application_name}} 也可以调用。
* FrontendSettingsCollector 字符串形式设置值，例如: 前台会拿到 test: 'registered in collector'

* properties 文件形式， 例如： 'application_name': 'cdeio.application.name' 会取自 coala.properties 文件中 cdeio.application.name 的值，coala.properties是 src/main/resources/settings下的文件，这个下面的properties文件都会被平台加载。

* 如果以上两种方法不够用，还可以是函数形式返回动态的值，甚至函数里还可以访问数据库。
