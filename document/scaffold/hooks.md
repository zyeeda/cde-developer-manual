## hooks
hook技术是用于改变原有业务执行的一种技术，通俗来讲，即是在cdeio平台中，创建、更新、删除（create，update，remove）业务是默认实现的，如果我们需要在三个业务之前，或者之后处理一些别的业务，那么通过 `exports.hooks` 来暴露其它业务，你可以轻松搞定。

### 基础
定义如下
```js
exports.hooks = {
    beforeCreate: {
        defaults: function (entity, request, meta) {

            return false;
        }
    },

    beforeUpdate: {
        defaults: function (entity, request, meta){
           ...
        }
    },

    afterRemove: {
        defaults: function (entity, request, meta) {
            ...
        }
    }
};
```

方法中可写任意逻辑代码，且可以[mark services](/document/term/service.html)，调用service定义的方法。另外hook分以下几种模式：

- beforeCreate  创建数据之前执行，可中止创建操作
- beforeUpdate  更新数据之前执行，可中止更新操作
- beforeRemove  删除数据之前执行，可中止删除操作
- afterCreate   创建数据之后执行，不可中止创建操作
- afterUpdate   更新数据之后执行，不可中止更新操作
- afterRemove   删除数据之后执行，不可中止删除操作

传入参数：
- entity  已经填充好数据当前实体对象
- request 请求对象，里面包含界面请求的参数， 详细请看[request](document/extention/service/router/request.html) 章节。
- meta    当前实体的反射信息，字段、注解等

> 注意：  
只有before型的hook才能中止操作，如果需要中止，请 return false;

### 高级
`default` 表示默认的添加、更新、删除操作，除了 default 外，还可以配置其它方式的操作

场景：以下展示的是一个修改密码的的 hook，因为修改密码是更新操作，因此也会触发 beforeUpdate，不过此处要将 default 改为 changePassword - 表单名称，其它几种hook参照这个。

```js
exports.hooks = {

    beforeUpdate: {
        changePassword: function (entity, request, meta){
           ...
        }
    }
};

exports.fieldGroups = {
    editPwdInfo: [
        {name: 'oldPassword', type: 'password', required: true, validations: {rules: {required: true, rangelength:[6, 60]}, messages: {required: '不能为空', rangelength:'个数必须在6和60之间'}}},
        {name: 'newPassword', type: 'password', required: true, validations: {rules: {required: true, rangelength:[6, 60]}, messages: {required: '不能为空', rangelength:'个数必须在6和60之间'}}},
        {name: 'newPassword2', type: 'password', required: true, validations: {rules: {required: true, equalTo: 'newPassword'}, messages: {required: '不能为空', equalTo: '不匹配'}}}
    ]
};

export.forms = {
    changePassword: {
        groups: ['editPwdInfo']
    }
};
```
