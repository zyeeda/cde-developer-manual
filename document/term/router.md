# Router

router 是用来分发和处理请求的，有些类似于 Spring 中的 DispatcherServlet。当有请求来到服务端的时候，router 会拦截所有请求，并根据其中定义的规则，分发请求到一个请求处理方法，该请求处理方法会调用 service（进而是 manager）来实现业务逻辑处理，并最终将响应返回给客户端。
