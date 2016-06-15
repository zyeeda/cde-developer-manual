# 运行环境配置

从 Cdeio 网站下载 Cdeio 并将 JDK 和 Maven 安装配置成功后，可以按照下列步骤快速开发一个完整的增、删、改、查功能。

### 1. 创建实体
在项目的 ```src/main/java/com/zyeeda/test/entity``` 目录下创建 Employee.java 文件，内容如下：

```java
package com.zyeeda.test.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Table;

import com.zyeeda.cdeio.commons.annotation.scaffold.Scaffold;
import com.zyeeda.cdeio.commons.base.entity.DomainEntity;

@Entity
@Table(name = "CDEIO_EMPLOYEE")
@Scaffold("/test/employee")
public class Employee extends DomainEntity {

    private String name;

    private String description;

    public String getName() {
        return this.name;
    }

    public void setName(final String name) {
        this.name = name;
    }

    public String getDescription() {
        return this.description;
    }

    public void setDescription(final String description) {
        this.description = description;
    }

}
```

### 2. 创建 scaffold

在项目的 ```src/main/webapp/WEB-INF/app/test/employee.feature``` 目录下创建 scaffold.js 文件，内容如下：

```js
exports.filters = {
    defaults: {
        '!employeeFilter': ''
    }
};

exports.labels = {
    name: '名称',
    description: '描述'
};

exports.fieldGroups = {
	defaults: ['name', 'description']
};
```

### 3. 配置实体路径

打开 ```src/main/webapp/WEB-INF/app/config.js```，在 ```entityPackages``` 属性下添加实体所在的包路径 ```com.zyeeda.test.entity```。

### 4. 启动项目

在命令行中进入项目根目录，然后运行 ```mvn jetty:run```命令。

### 5. 查看效果

假定项目名称为 ```helloworld```，在浏览器中输入 ```http://localhost:8000/helloworld/#feature/test/scaffold:employee```后，系统会进入登录页面，输入任意相同的用户名和密码后即可看到功能页面。