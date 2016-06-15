# get

### 描述

使用 get 方法请求一个地址，正如其字面所言，应该是获取存在于该地址的某些资源或数据。

除此之外，更深层次的含义，每次 get 请求都不应该对服务器的状态产生任何影响，也就是说对某个资源进行 0 次 get 请求、1 次 get 请求或若干次 get 请求得到的结果都应该是一样的，否则就不应该使用 get。因此我们称 get 请求具有安全和幂等的特性。


### 基本配置

```js
//method = get
http://localhost:8000/cdeio-examples/invoke/scaffold/employee
```

可以通过如下的方式接手此请求：

```js
router.get('/', function (request) {
	...
}));
```

### 参数设置

可以通过两种方式传递请求参数：

* **地址参数**，直接在请求地址后添加参数，通过冒号的方式接收参数。
<br/><br/>设置'1234567' 为请求参数：
```js
//method = get,
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
//method = get
http://localhost:8000/cdeio-examples/invoke/scaffold/employee?employeeId=1234567
```
与地址参数接收方式不同，问号方式的参数需要通过 request 对象的 params 对象进行接收，如下：
```js
router.get('/', function (request) {
	var employeeId = request.params.employeeId;
	...
}));
```
