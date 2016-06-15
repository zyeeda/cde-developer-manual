## 界面
### 1、介绍
前端界面 是一套基于开源组件封装的、面向快速构建、且功能全面的前端开发框架和控件库。主要具有如下一些功能特性：

- **模块支持** \- 有过前端 JavaScript 开发经验的人员都应该了解，如果 JavaScript 组件设计得不够充分，加之被无限制的引用，就会导致严重的命名空间冲突，从而影响各部分的协调工作。因而出现了各种技术来解决组件命名不协调的问题。比如：在设计组件的时候定义专属的命名空间，尽量不要向 Global Object 中写入信息，使用 [Immediately-Invoked Function Expression (IIEF)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) 等等。这些解决办法各有利弊：相对简单一点的，比如声明命名空间，不能百分百保证命名不会冲突，只是降低了冲突出现的几率；而像 IIEF 这样的技术，实践起来又不是很容易，需要对代码进行精细的规划。那么是否有一种既简便又能绝对解决命名空间冲突问题的技术存在呢？答案自然是肯定的。前端界面 使用 [Asynchronous Module Definition (AMD)](http://requirejs.org/docs/whyamd.html, "Why AMD?") 技术，为前端 JavaScript 引入了模块和包的概念。
- **MVC** \- [Model-View-Controller (MVC)](http://en.wikipedia.org/wiki/Model-view-controller) 基本上是每个主流框架都必须引入的架构模式，本框架自然也不例外。
- **事件总线** \- 通常来说，使用 JavaScript 来响应界面控件事件的方法，是在声明组件的同时，声明该事件的事件处理方法。这种做法无疑是正确的，但随着界面越来越复杂，组件越来越多，维护控件事件和其处理方法就会变得越来越困难。而且使用这种原始的实现方式也和 MVC 模式中要将 View 和 Controller 分开的思想相违背。前端界面 在此引入前端事件总线的概念，任何需要触发事件的控件，都将一个事件信息发布到事件总线上，任何对该事件感兴趣的组件或方法都可以去侦听，然后做出响应。使用事件总线就将控件事件和处理方法完全解耦，使得程序能够更好的支持模块化设计。
- **前端路由** \- 习惯服务器端应用程序开发的人员都会清楚，当有请求到达服务器的时候，后端程序会根据请求路径的不同，将其分发给不同的程序片段去处理，从而达到一种模块划分的目的。再看前端，多年以前，当前端应用还处在 HTML 年代的时候，并不存在这个问题，因为前端的一个界面和后端的一个请求响应是一一对应的，开发人员很少会在前端处理过多的界面逻辑。但是随着 Web 2.0 的盛行，有越来越多的前端界面使用复杂的 HTML、CSS 和 JavaScript 等技术去模拟富客户端下的用户体验，这就使得前端程序变成了一个和后端程序同样复杂的系统。[单页面应用程序](http://en.wikipedia.org/wiki/Single-page_application, "Single-page Application ")的出现，更将前端程序的复杂度推向极致，因为用户界面从始至终都只有一个页面，开发人员通过使用 Ajax 和 Dom 等技术从后台获取数据后动态更新显示界面，那么如何对这样的界面进行划分，并快速访问就变得非常重要了。另外在不成熟的单页面应用程序系统下，很难通过某个指定的 URL 路径跳转到相应的界面，在 HTML 年代看似不需要过多关注的能力，在单页面环境下变成了症结，引入前端路由功能就可以很好的解决上述这些问题。
- **丰富的控件库** \- 前端界面 底层使用的是 jQuery 基础库，因此活跃的社区能够为本框架提供丰富的控件库支持。而且 前端界面 也开箱封装了常用的控件，例如：布局、标签页、菜单、树、数据网格、表单组件、对话框、提示信息和按钮等。

### 2、引入的组件

前端界面引入了大量的第三方开源组件和基础库，需要对这些内容有一个全面的了解，才能更好的理解和使用本框架。

- [RequireJS](http://requirejs.org/, "RequireJS") \- 一种 AMD 的实现。
- [Bootstrap](http://twitter.github.com/bootstrap/, "Twitter Bootstrap") \- 前端框架起始包。具有非常丰富的功能，包括：浏览器默认样式的重置和归一、各种预定义样式、网格布局、常用控件和前端开发的最佳实践等。
- [jQuery](http://jquery.com/, "jQuery: The Write Less, Do More, JavaScript Library") \- 大名鼎鼎的 jQuery 就不用多说了。
- [jQuery UI](http://jqueryui.com/, "jQuery UI") \- jQuery 官方的 UI 控件库，也不用多说了。
- [jQuery UI Layout](http://layout.jquery-dev.net/, "jQuery UI Layout Plug-in") \- 基于 jQuery 实现的类似 Ext border 布局的组件。
- [Select2](http://ivaynberg.github.com/select2/, "Select2") \- 下拉选择框组件。
- [dataTables](http://www.datatables.net/, "dataTables") \- 数据网格组件。
- [zTree](http://www.ztree.me/v3/main.php#_zTreeInfo, "zTree") \- 树组件。
- [Underscore.js](http://underscorejs.org/, "Underscore.js") \- JavaScript API 扩展，用来在跨浏览器的环境下使用更加丰富的 JavaScript 接口。
- [Backbone.js](http://backbonejs.org/, "Backbone.js") \- 前端 MVC 框架。
- [Backbone.Marionette](https://github.com/derickbailey/backbone.marionette, "Backbone.Marionette") \- 针对 Backbone 的一些最佳实践的封装。
- [Handlebars.js](http://handlebarsjs.com/, "Handlebars.js") \- 前端模板组件，是 mustache 模板库的超集。
- [Modernizr](http://modernizr.com/, "Modernizr") \- 前端功能嗅探器组件，用来辨别浏览器是否支持某些指定功能。


### 3、工作区

![image](/assets/extention/ui/README1.png)

从上图可以看出，前端界面 要求的前端目录和文件结构十分简单。

- **asserts** \- 存放所有除脚本以外的静态资源文件，如：图片、图标、样式、皮肤和 Flash 动画等
- **chrome-frame.html** \- chrome-frame安装页面，可忽略
- **scripts** \- 存放所有前端界面的脚本文件，该目录下又包含如下内容：
    - **app** \- 存放当前系统的前端应用程序脚本文件，这也是项目开发时绝大多数时间需要关注的地方
    - **application.js** \- 应用程序文件，应用怎么呈现，都在这个文件里定义
    - **cdeio** \- 存放平台库文件，不需要关注
    - **main.js** \- 前端应用程序的入口点文件，通常会调用 application.js
    - **config.js** \- 前端应用程序的配置文件
    - **require-config.js** \- requirejs 依赖配置文件
    - **vendors** \- 存放所有框架内，或者第三方的组件和类库
- **WEB-INF** \- Java Web Application 定义的系统目录，里面存放有后端应用的配置和程序


