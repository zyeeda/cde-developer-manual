# filters

### 描述

filters 用来指定实体需要序列化的属性。 filters 能够对列表实体（list）、查看的实体（get）和编辑的实体（update）进行序列化。可以为所有的操作指定相同的序列化方式，也可以为某个操作单独指定序列化方式。


### 为所有的操作指定相同的序列化方式

如果 list 、 get 和 update 采用相同的序列化方式，则可以统一在 defaults 中进行定义。下面以实体 Employee 为例进行介绍。

```js
exports.filters = {
    defaults: {
        'employeeFilter': ['name', 'sex', 'age']
    }
};
```

注意其中的 `employeeFilter` ，所有的 filter 都是以 `Filter` 后缀结尾的， `Filter` 的前面需要加上实体的名称（首字母要小写）。这样做的目的是当 filters 中要为多个实体指定序列化规则的时候就能够通过 filter 的名称去匹配相应的实体了。下面以 Employee 和 Account 两个实体为例进行说明。假定 Employee 实体与 Account 是一对一的关系，下面的代码同时指定了 Employee 和 Account 的序列化方式

```js
exports.filters = {
    defaults: {
        // 为 Employee 指定序列化规则
        'employeeFilter': ['name', 'sex', 'age'],
        // 为 Account 指定序列化规则
        'accountFilter': ['account', 'createTime']
    }
};
```
### 为特定的操作单独指定序列化方式

为 list 单独指定序列化方式，get 和 update 没有单独指定则使用 defaults 中的序列化方式：

```js
exports.filters = {
    defaults: {
        'employeeFilter': ['name', 'sex', 'age']
    },
    list: {
		'employeeFilter': ['name', 'sex']
	}
};
```

get 和 update 也一样可以单独指定序列化方式，指定的方法同 list 一样。

### 序列化规则

序列化分为包含和排除两种方式。

##### 1. 包含方式

包含方式直接在 filter 后面列出所要序列化的属性即可，如下：

```js
exports.filters = {
    defaults: {
		'employeeFilter': ['name', 'sex', 'age']
	}
};
```

此 filter 表示对 Employee 实体的 `name` 、 `sex` 和 `age` 属性进行序列化，其他的属性将不会被序列化。假设 Employee 实体拥有 `address` 属性，那么即使列表或表单中定义了 `address` 也是不会获取到后台传回的结果的。


##### 2. 排除方式

排除方式需要在 filter 名称前面加上 `!` 以表示“排除”的意思。

```js
exports.filters = {
    defaults: {
        '!employeeFilter': ['address', 'attachment']
    }
};
```

此 filter 表示除 `address` 和 `attachment` 属性外，其他的属性都将被序列化。


### 层级的指定

当实体存在自引用（如属性结构）关系时，为避免将整个属性结构都序列化的情况，可以通过 `(x)` 的方式指定递归的层级。

假定 Organization 是一个属性结构，其 `parent` 属性是一个自引用关系，那么可以通过下面的方式指定层级的序列化规则：

```js
exports.filters = {
    defaults: {
        'organizationFilter': ['id', 'name', 'parent(1)']
    }
};
```

此 filter 表示对实体的 `parent` 属性只向上序列化一个层级。