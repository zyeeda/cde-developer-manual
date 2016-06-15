# 验证
验证采用的是 BeanValidator 技术，平台对其进行了封装，让它变得更简单好用，而且能同时生成前台验证。

## 基础
相对简单的一些验证，在实体上通过标记就可以实现，复杂的或者有关业务逻辑的验证则需要在 scaffold.js 中进行编写。

下面将对实体上的验证注解进行介绍。

###1. 非空

标记：`@NotNull`

适用类型：所有类型

描述：验证一个字段的值不能为空或空字符串

```java
@NotNull
public Integer getAge() {
	return age;
}
```

###2. 非空(字符串)

标记：`@NotBlank`

适用类型：String

描述：验证一个 String 类型字段的值不能为空或空字符串

```java
@NotBlank
public String getMobile() {
	return mobile;
}
```

###3. 电子邮件

标记：`@Email`

适用类型：String

描述：验证一个 String 类型字段的值是否是一个有效的Email地址

```java
@Email
public String getEmail() {
	return email;
}
```

###4. 范围(字符串)

标记：`@NullableSize`

适用类型：String

描述：验证一个 String 类型字段要么为空， 要么长度是否在 min ~ max 的区间内

```java
@NullableSize(min = 2, max = 200)
public String getAddress() {
	return address;
}
```

###5. 范围(数字)

标记：`@Range`

适用类型：Long、Integer、Double、BigDecimal

描述：验证一个数字类型字段的值是否在 min ~ max 的区间内

```java
@Range(min = 1, max = 100)
public Integer getAge() {
	return age;
}
```

###6. 最小值

标记：`@Min`

适用类型：Long、Integer、Double、BigDecimal

描述：验证一个数字类型字段的值必须不小于指定值

```java
@Min(value = 5)
public Integer getQq(){
	return qq;
}
```

###7. 最大值

标记：`@Max`

适用类型：Long、Integer、Double、BigDecimal

描述：验证一个数字类型字段的值必须不大于指定值

```java
@Max(value = 11)
public Integer getQq(){
	return qq;
}
```

### 8. 匹配
标记： `Matches`
适用类型：所有类型
描述：验证两个字段是否匹配

```java
@Matches(source = "password", target = "password2", bindingProperties = "password")
public class Account extends DomainEntity {
    ...
}
```
> 注意: 这个注解是标注在类上的，source 跟 target 是匹配相等的字段名，bindingProperties 是触发这个验证的字段名。

### 9. 惟一
标记： `Unique`
适用类型：所有类型
描述：验证此字段数据是否惟一

```java
@Unique.List({
        @Unique(groups = { Create.class }, namedQuery = "findDuplicateUsernameCountOnCreate", bindingProperties = "userName"),
        @Unique(groups = { Update.class }, namedQuery = "findDuplicateUsernameCountOnUpdate", bindingProperties = "userName")
})
@Matches(source = "password", target = "password2", bindingProperties = "password")
public class Account extends DomainEntity {
    ...
}
```

> 注意：这个注解是标注在类上的，@Unique.List是它多配置形式，namedQuery 绑定的是一个 [SQL命名查询](/document/extention/service/manager/README.html)，bindingProperties 是触发这个验证的字段名，groups下面会讲到。

## 高级
验证有 Create, Update 两种分组，对应添加 和 编辑操作，在同一实体，不同业务操作的情况下，验证会有不同，当你为验证分了groups属性时，验证就中会在对应的操作上生效。
```java

@NotNull(groups = Create.class)
public Boolean getDiabled() {
    return diabled;
}

@Min(value = 5, groups = Update.class)
public Integer getQq(){
    return qq;
}

```
