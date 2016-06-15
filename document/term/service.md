# Service

Service 在　Cdeio 中充当业务逻辑层的角色，所有与业务有关的代码都应该在这里完成。但是 Service 中的代码不应该参与访问底层存储或其它基础组件，这部分工作应该是由 Manager 来完成的，Service 应该只是调用 Manager 所提供的方法而已。为了使得 marker 可以自动注入 Service，要求每个 Service 文件必须导出一个名为 createService 的方法，marker 在每次注入的时候都会调用该方法，生成一个新的 Service 实例，因此 Service 的注入模式是 protoytype 的，而不是 singleton 的。
