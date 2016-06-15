# 数据验证（validators）
数据验证是指服务端的验证，通过暴露 `exports.validations` 来配置创建、更新、删除（create，update，remove）业务操作时需要处理的数据验证，配置方式跟hooks类似。
### 基础
以下场景表示启用的用户不允许删除：

```js
exports.validators = {
    create: {
        defaults: function(context, entity, request){
        }
    },
    update: {
        defaults: function(context, entity, request){
        }
    },
    remove: {
        defaults: function(context, entity, request){
            if(entity.status === '启用') {
                context.addViolation({ message: '不能删除状态为启用的用户'});
            }
        }
    }
};
```

validator 分以下几种：
- create  添加操作验证
- update  更新操作验证
- remove  删除操作验证

context 有以下几个方法：
- addViolation(obj)增加验证错误信息，obj必须是带message属性的对象，message信息将会提示在界面上
- hasViolations() 判断context中有没有验证错误信息
- collectViolations() 返回错误信息列表
- skipBeanValidation() 跳过验证，即使有错误信息，函数也会执行下去，相当于放行的意思。

> 注意：  
数据验证中 context 中只要有一条 violation 记录，验证便不通过，界面就会提示错误信息。

传入参数：
- entity 已经填充好数据当前实体对象
- request 请求对象，里面包含界面请求的参数，详细请看[request](document/extention/service/router/request.html) 章节。

### 高级
同hooks一样，除了验证 default 表单的数据外，还可以验证其它表单的数据。

```js
exports.validators = {
    update: {
        changePassword: function(context, entity, request){
        }
    }
};
```