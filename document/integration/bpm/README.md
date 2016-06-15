# 工作流

### Activiti

Cdeio 使用的是开源工作流引擎 Activiti。
Activiti 去除了 BPMN2.0 规范中的繁杂部分，并从易用性角度出发进行了适度的扩展，从而在功能性和易用性上都表现的极为出色。

### 我们的工作

我们从业务角度出发，经过综合、谨慎的分析后，对 Activiti 与 Cdeio 进行了集成。我们的目的是让每一个与流程有关的业务都能从以业务为主体和以任务为主体的两个角度方便的展示和操作，在 Cdeio 简单快捷的基础上即能够让开发人员快速的进行开发又能够让最终用户拥有良好的用户体验。

### 引入依赖

使用工作流组件需要引入工作流依赖，如下：

```xml
<dependency>
  <groupId>com.zyeeda</groupId>
  <artifactId>zyeeda-cdeio-bpm</artifactId>
  <version>${cdeio.framework.version}</version>
</dependency>
```

### 导入数据库脚本

使用工作流组件需要向数据库中导入 Activiti 的脚本信息。脚本文件 在 src/main/sql 目录下，导入方式如下：

首先，根据 MySQL 的版本导入相文件夹下的如下两个脚本：
* activiti.mysql.create.engine.sql
* activiti.mysql.create.history.sql。

然后，导入 mysql 文件夹下的如下三个脚本：
* activiti.mysql.create.engine.sql
* activiti.mysql.create.history.sql
* activiti.mysql.create.identity.sql