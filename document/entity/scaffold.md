# @Scaffold

实体在打上 ```@Scaffold``` 标记后表示与实体对应的增、删、改、查等功能将通过 scaffold 自动生成。 ```@Scaffold``` 标记只有一个参数用于指定与之相对应的 feature 的位置， feature 所在的根目录为 src/main/webapp/WEB-INF/app 。

@Scaffold 的使用方式如下：

```java
@Scaffold("/employee")
public class Employee extends DomainEntity {
	...
}
```

与之相对应，应该在 WEB-INF/app 下应该创建 employee.feature 文件夹，并在其中包含 scaffold.js 文件。

一个实体也可以对应多个 feature ， 只需要在 ```@Scaffold```中进行枚举就可以了。如下：

```java
@Scaffold("/employee,/account")
public class Employee extends DomainEntity {
	...
}
```