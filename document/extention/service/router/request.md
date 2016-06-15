# request

request 对象是 JSGI 规范中定义的，主要有如下一些属性：

* **params**，以键值对方式存储的所有查询字符串中的参数以及请求体中的参数
* **env.servletRequest**， 当需要使用底层 Servlet API 的时候，获取 ServletRequest 类型的请求对象
* **env.servletResponse**， 当需要使用底层 Servlet API 的时候，获取 ServletResponse 类型的响应对象

