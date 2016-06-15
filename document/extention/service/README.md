# 服务

## 预备知识
在开始详细介绍平台功能之前，首先需要了解一下平台引入的新技术和新概念。如果对这些内容已经掌握，可以跳过本章节。

### JavaScript

cdeio平台和基于本平台开发的系统会大量使用 JavaScript 语言，如果不熟悉该语言，请参考如下一些学习资源：

- [JavaScript Tutorial from w3schools.com](http://www.w3schools.com/js/default.asp)
- [Learn JavaScript from Mozilla Developer Network](https://developer.mozilla.org/en-US/learn/javascript)

### exports 和 require

exports 和 require 来源于 CommonJS 规范，为 JavaScript 提供了模块化功能。服务器端 JavaScript 不同于客户端 JavaScript 的一大区别在于其每一个单独的 JavaScript 文件会形成一个模块（module），在不做任何额外操作的情况下，各模块之间是无法相互贯通的。也就是说模块定义了一个程序边界，变量与方法只能在模块内部相互访问，想要在模块之间实现互操作，就必须进行所谓的“导出”与“导入”操作，在 CommonJS 中的术语称为 exports 和 require。

**exports** 顾名思义就是将模块外可访问的内容进行“导出"声明，以标识其被模块公开，可以跨越模块边界被其他模块访问。

```javascript
// path/to/demo.js
exports.app = function (req) {
    return {
        status: 200,
        headers: {'Content-Type': 'text/plain'},
        body: ['Hello World!']
    };
};
```

可以看出 exports 类似于一个 JavaScript 对象，所有该对象的属性和方法都会被公开出来。因此在一个 JavaScript 文件内（或者说模块内），可以多次使用 exports 来声明导出若干属性和方法。

**require** 用来请求被其它模块 exports 出来的内容。require 是一个方法，接收要请求的模块路径作为参数。不同于 Java 的类加载机制，由于 JavaScript 是解析执行的，直到文件被首次 require 的时候，引擎才会解析其内容，并将结果缓存起来，再次访问的时候就不用重新解析。当文件发生变化的时候，require 会再次解析该文件并缓存，从而达到动态语言的效果。

```javascript
var app = require('path/to/demo'); // 模块路径可以省略 .js 扩展名
```

### 解构赋值

```javascript
var user = {
    firstName: 'Tom',
    lastName: 'Smith'
};
var firstName = user.firstName;
var lastName = user.lastName;
```

上面的代码的功能是将 user 对象的两个属性 firstName 和 lastName 分别赋值给两个和属性同名的变量。设想一种情况，假如 user 具有非常多的属性，想要进行类似的赋值，就必须写很多行赋值语句，而且每一行都要包含相同的对 user 对象的引用。针对这种情况，JavaScript 1.7 版本以后，开始引入一种新的操作，称为“解构赋值”。关于解构赋值的更多信息，请参考[这里](https://developer.mozilla.org/en/New_in_JavaScript_1.7, "New in JavaScript 1.7")。

```javascript
var {firstName, lastName} = user;

// 以上写法等同于
var firstName = user.firstName;
var lastName = user.lastName;
```

## 应用程序入口点

JSGI servlet 在启动的时候会默认加载 WEB-INF/app 目录，并寻找名为 main.js 的启动文件。该文件**必须**导出名为 app 的对象，用来供 JSGI servlet 启动整个系统。所以 main.js 就是整个后端应用程序的入口点。

main.js 的内容通常来说只有一行，看上去是这个样子的：

```javascript
exports.app = require('coala/router').createApplication(this);
```

入口定义好后，平台会自动加载 Router，请看后面要讲到的 Router（控制器层） 、 Service（业务层） 、 Manager（数据处理层），它们共同组成三层架构，各司其职。
