# Router

### 描述

router 是用来分发和处理请求的，有些类似于 Spring 中的 DispatcherServlet。当有请求来到服务端的时候，router 会拦截所有请求，并根据其中定义的规则，分发请求到一个请求处理方法。

router 是通过 REST 方式处理请求的，我们这里支持 get 、 post 、 put 和 delete 四中方式请求，下面将会对这四种请求做详细的介绍。

### 1. get

使用 get 方法请求一个地址，正如其字面所言，应该是获取存在于该地址的某些资源或数据。

除此之外，更深层次的含义，每次 get 请求都不应该对服务器的状态产生任何影响，也就是说对某个资源进行 0 次 get 请求、1 次 get 请求或若干次 get 请求得到的结果都应该是一样的，否则就不应该使用 get。因此我们称 get 请求具有安全和幂等的特性。


#### 基本配置

```js
// method = get
http://localhost:8000/cdeio-examples/invoke/scaffold/employee
```

可以通过如下的方式接手此请求：

```js
router.get('/', function (request) {
	...
}));
```

#### 参数设置

可以通过两种方式传递请求参数：

* **地址参数**，直接在请求地址后添加参数，通过冒号的方式接收参数。
<br/><br/>设置'1234567' 为请求参数：
```js
// method = get
http://localhost:8000/cdeio-examples/invoke/scaffold/employee/1234567
```
可以通过如下方式接收参数：
```js
router.get('/:employeeId', function (request, employeeId) {
	...
}));
```
这种方式用起来比较方便，缺点是只能传递一个参数。如果需要传递多个参数则需要通过问号的方式进行传递。
<br/><br/>
* **问号参数**，在请求地址后添 `?` 后可以 key-value 的方式传递参数，多个参数以 `&` 进行分割。
```js
// method = get
http://localhost:8000/cdeio-examples/invoke/scaffold/employee?employeeId=1234567
```
与地址参数接收方式不同，问号方式的参数需要通过 request 对象的 params 对象进行接收，如下：
```js
router.get('/', function (request) {
	var employeeId = request.params.employeeId;
	...
}));
```

post 、 put 和 delete 的参数设置方式与 get 一样，下面就不再分别进行介绍了。

### 2. post

使用 post 方法请求一个地址，通常是要向该地址发送数据，并且这些数据要能够对服务端的数据产生影响，通常是用来新增一些数据，或者完成其他三种方法无法完成的任务，比如批量操作等。因此对一个地址执行 0 次、1 次或若干次 post 请求，其结果都可能会不一样。因此 post 请求既不是安全的也不是幂等的。

### 3. put

使用 put 方法请求一个地址，也会通过请求体（request body）向服务端发送数据，与 post 方法不同的是，通常 put 方法用来处理一些诸如更新和修改的操作。可以想像对某一个地址执行 0 次和执行 1 次 put 请求，其结果是不同的，因此 put 请求不是安全的；但是执行 1 次和执行若干次 put 请求产生的结果却应该是相同的，因此 put 请求是幂等。

### 4. delete

使用 delete 方法请求一个地址，就是要删除该地址对应的资源或数据。同 put 请求类似，delete 请求也是不安全但幂等的。

因为 delete 是 JavaScript 的关键字，所以无法使用 delete 全称，只能使用缩写 del。

