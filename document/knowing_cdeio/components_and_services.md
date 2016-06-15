# 基础组件和服务

## 工作流

CDE.IO 使用 Activiti 作为底层工作流引擎。Activiti 是一款基于 Apache 许可的工作流和业务流程管理开源平台，从基础开始构建，支持 BPMN 2.0 标准，是一种轻量级、嵌入式的 BPM 引擎，适合于在各种企业级业务场景中使用。

CDE.IO 扩展了系统自动生成能力，可以根据业务流程图的描述自动生成流转功能，并且以任务代办的形式对数据及流程进行管理和操作。

CDE.IO 的工作流支持图形化操作，在设计器中通过简单的托拽及配置就可以完成一个业务流程的设计。

通过对 Activiti 的封装和扩展，CDE.IO 的工作流可以支持下述一些业务场景：

1. **流程召回**：可以将某个已经完成的业务流程，在下一个环节的负责人没有认领之前恢复到操作之前的状态；
2. **回退**：可以将某个需处理或认领的业务流程，回退给上一个环节（或前面某个环节）的负责人；
3. **指定负责人**：某个业务流程，当前环节处理完成后，可以由当前环节的负责人为下一个环节直接指定负责人；
4. **自由流程**：可以支持用户自定义流程。流程发起后由，发起人指定下一个环节及其负责人。下一个环节的负责人处理完成后，可以再指定更下一个环节及其负责人，无限循环；
5. **脚本任务**：可以在复杂的流程中执行程序脚本；
6. **排他网关**：流程在进入排他网关时，会根据设定的条件自动计算流程路径，排他网关会选定第一个符合条件的路径执行；
7. **并行网关**：流程在进入并行网关时，会分成多个并行的分支同时执行，互不影响。当所有的分支执行结束后，根据流程设定，进入下个环节。通过并行网关可以实现多个部门的会签等业务；
8. **包含网关**：包含网关是并行网关和排他网关的组合，包含网关也会计算流程的路径，但所有成立的路径都会被执行，并且是并行执行；
9. **事件网关**：可以根据网关后面事件触发的情况来执行；
10. **监听**：可以在流程的各个环节（启动、结束、脚本节点、人工任务节点和流程流转过程中）添加监听，监听所执行的业务逻辑可以自行定制；
11. **子流程**：支持嵌入式和外部引用式两种形式的子流程。对于分析和设计复杂的业务流程有很大帮助，同时能提高流程的复用性，进而提升工作效率；
12. **多实例**：任务节点、脚本节点及子流程等多种节点都支持多实例的运行方式。多实例可以使用并行和顺序执行两种方式。不确定部门或人员个数的会签业务可以使用多实例来完成；
13. **事件**：事件用来表明流程的生命周期中发生了什么事情。事件分为捕获和触发两种类型，当流程执行到捕获事件时，它会等待被触发；而当流程执行到触发事件时，会发出一个事件。为人工任务设置一个定时事件，是流程事件典型的应用场景。

## 目录服务

目录服务用于统一管理整个组织的用户及目录。

目录支持树状结构，用来满足公司的组织机构管理要求。系统管理员在目录服务里为每个使用系统的用户建立账号，用户通过该账号和密码才能登录系统。

目录服务使用基于 OpenID 2.0 标准的接口实现了单点登录认证功能，任何实现了 OpenID 2.0 标准的系统均可以由目录服务提供用户数据，并对用户身份进行认证。这样就不需要在每个业务系统中都重复建立一套用户和组织机构数据，也不用自行实现一套没有经过科学论证的用户认证机制。各个业务系统可以通过目录服务提供的接口同步数据到该系统，以实现系统内的数据查询与业务集成。

## 角色权限管理

在系统中，实体对象的显示列表、增删改查操作、以及流程的流转等都称之为功能点，CDE.IO 支持对各功能点操作的权限进行控制。功能点在系统中是权限控制的最小单位。

CDE.IO 的权限控制功能使用“基于角色的访问控制列表”方式来实现。每个功能点就是一个权限，可以为角色分配多个不同的权限，而角色下的多个用户就同时具备了该角色所分配的所有权限（即可操作这些功能点）。如果一个用户同时属于多个角色，则该用户的权限是所有角色权限的合集。

CDE.IO 的权限验证机制包括前后端的双重限制。也就是说，某个用户不具有新增某个业务对象功能点的操作权限，则他既不能在浏览器打开新增操作的界面，也不能绕过浏览器直接发送请求到后台来完成数据的提交，从而保证数据的安全性、有效性、完整性和一致性。

以上功能在系统中是默认开启的，在开发过程中，开发人员只需要做好对用户的角色及功能点权限的正确配置即可。

## 报表服务

CDE.IO 的报表服务是基于 Eclipse 的开源报表工具 Birt 来实现的。Birt 是一个灵活、强大且完全开源的报表工具，具有良好的兼容性和可扩展性。除了能方便设计各种强大的报表和图表之外，报表服务对各种形式的数据源都有很好的支持。

报表服务对 Birt 进行了封装，不但重新定义了报表的界面风格，还支持独立部署报表服务器，用户可以根据自身需求在现有应用的 Web 页面中嵌入报表服务器中的整个报表或报表的某些元素，为应用项目注入强大的报表功能。

### 报表设计器

Birt 的报表设计器提供了一组“所见即所得”的报表设计界面，这种界面和众多 IDE 设计器的界面类似，用户可以根据需要在报表中添加文本、数据表格或是图片等简单报表元素，也可以添加图表和交叉表等复杂的报表元素，并且还可以对这些元素进行格式设置及属性配置。

报表设计器的核心功能包括：

1. 多样的向导来简化复杂报表设计的任务；
2. 拖拽式报表布局可以加快开发速度；
3. 多种多样的可视化报表部件，包括网格（grid）、表（table）、图片（image）和图表（chart）等；
4. 报表可自由转换为 HTML、PDF、Word 或 Excel 等格式，并可以通过扩展设计器来支持其他格式；
5. 交互式查看功能让用户可以进行报表定制；
6. 支持创建多维数据集与交叉表；
7. 可向报表设计中添加脚本程序来处理复杂的业务逻辑或数据访问，而且这些脚本也是基于 JavaScript 语言的；
8. 通过调用开发接口可以让报表在运行时动态添加可视化部件或改变现有的部件；
9. 可扩展的图表引擎支持动态图表与多维图表，并可以让用户添加定制图表和图像格式；
10. 多报表设计中重用报表部件可以加速新报表的开发或现有报表的更新；
11. 封装部件中的复杂数据访问、业务逻辑或布局逻辑，使得其他开发人员可以在此基础之上创建更高级的设计，而不暴露底层的复杂性；
12. 报表模板与报表库可以定义常用的布局和格式化选项的可重用样式，保存下来并在多个报表间复用，使得开发人员更高效地创建拥有统一外观的报表。

### 报表查看器

Birt 自带一个标准报表查看器，用户可以使用该查看器浏览报表的执行结果。在报表执行过程中，这个查看器也会提示用户为具有参数的报表输入运行时参数。CDE.IO 的报表服务对此查看器进行了集成与扩展，通过一种代理机制解决了应用服务器和报表服务器的跨站访问问题，使得报表界面可以完美的嵌入业务系统中，同时也对默认的报表样式和参数输入界面的样式进行了调整，使其更加适配平台的统一界面风格。