# response

response 对象用作对请求作出响应，包含以下三个必有属性：

* **status**， HTTP Status Code，用来表示响应的状态。例如：200 代表成功，404 代表请求的资源未找到等
* **headers**， HTTP Response Header，响应的头信息
* **body**， HTTP Response Body，响应的主体内容

```js
router.get('/users/:userId', function (req, userId) {
    var user = findByUserId(userId); // 自定义方法
    return {
        status: 200,
        headers: {
            'Content-Type': 'application/json'
        },
        body: '{userId: ' + user.userId + ', name: ' + user.userName + '}'
    }
});
```
使用这种方式返回响应，需要对 HTTP 协议有深入的了解，尤其需要了解各请求和响应的头信息的含义。而且此处 body 字段的内容必须是静态的数据，如果需要返回流式数据的话，仅仅使用 body 就无能为力了。因此框架引入了更方便的方式对请求进行响应。


#### charset()

如果调用此方法时不传入参数，则返回当前正在使用的字符编码（默认为 UTF-8）；如果传入参数，则将该参数设置为当前字符编码。字符编码会跟随在响应头信息的 Content-Type 字段处。

```js
var currentChartset = response.charset(); // 返回当前使用的字符编码，默认为 UTF-8

response.charset('GB2312'); // 设置当前使用的字符编码为 GB2312
currentCharset = response.charset() // 返回 GB2312
```

#### html()

返回 HTML 格式的响应。当仅有一个传入参数，且该参数是一个 JavaScript 对象的时候，方法使用此对象作为请求响应。与规范中定义的不同之处在于，除了 body 字段之外，status 和 headers 字段都是可选的。如果 status 字段省略则默认为 200，如果 headers 字段省略则默认为 Content-Type: text/html。当传入参数只有一个，但不是 JavaScript 对象的时候，或者有多个传入参数，则方法将这些参数对应的字符串连接后输出为 body 的内容，status 取默认的 200，headers 取默认的 Content-Type: text/html。

```js
response.html('<h1>Hello World</h1>');

// 等同于
response.html({
    body: '<h1>Hello World</h1>'
});

// 等同于
response.html({
    status: 200,
    headers: {
        'Content-Type': 'text/html'
    },
    body: '<h1>Hello World</h1>'
});

response.html({
    status: 201,
    body: '<h1>201 Created</h1>'
});

response.html(
    '<table><tr>',
    '<td>姓名</td>',
    '<td>年龄</td>',
    '</tr></table>'
);
```

#### xml()

返回 XML 格式的响应。与 html() 方法雷同，只是 headers 默认取值为 Content-Type: application/xml。

#### redirect()

返回 303 重定向响应，将页面重定向到指定路径。该方法需要一个重定向的目标路径作为参数。

```js
response.redirect('/sso/acounts/openid/signin');
```

#### notFound()

返回 404 请求路径未找到响应，该方法需要一个未找到的路径地址作为参数。

```js
response.notFound('/users/steve-jobs');
```

#### error()

返回 500 服务器错误响应，该方法需要一个错误消息作为参数。

```js
response.error('系统发生错误，请联系管理员！');
```

#### json()

在介绍此方法之前，需要先对本框架使用的 JSON 序列化组件有一个简单的认识。本框架的 JSON 序列化采用了 Jackson 库的 filter 功能，就是可以根据指定的 filter 名称和包含的以及排除的字段来自动生成 JSON 结果。其中的 filter 可以想像为一组属性的集合，通常定义在实体类上，例如：

```java
@JsonFilter("userFilter")
public class User extends DomainEntity {
    private String userName;
    private String password;
    private int age;
    private List<Group> groups;

    // 省略了相应的 getter 和 setter 方法
}

@JsonFilter("groupFilter")
public class Group extends DomainEntity {
    private String groupName;
    private List<User> users;

    // 省略了相应的 getter 和 setter 方法
}
```

以上代码定义了两个 filter，分别命名为 userFilter 和 groupFiter，名为 userFilter 的 filter 包含五个属性，分别是 id（继承自 DomainEntity），userName，password，age 和 groups；名为 groupFilter 的 filter 包含三个属性，分别是 id（继承自 DomainEntity），groupName 和 users。

了解了 filter 的含义，再来看此方法的定义，此方法需要传入两个参数：

- **object** \- 待序列化的实体对象，可以是 JavaScript 对象也可以是 Java 实体对象
- **config** \- 序列化过程的配置信息
    - **include** \- 需要包含在序列化结果中的字段列表
    - **exclude** \- 不需要包含在序列化结果中的字段列表
    - **status** \- HTTP Status Code，可选，默认为 200
    - **headers** \- 响应头信息，可选，默认为 Content-Type: application/json

include 和 exclude 的类型都是 JavaScript 对象，对象的键是 filter 的名称，值为属性列表。需要注意的是，同一个名称的 filter 不能同时出现在 include 和 exclude 中。承接上例：

```js
response.json(user, { // user 是 User 类型的一个实例
    include: {
        groupFilter: ['id', 'groupName', 'users']
    },
    exclude: {
        userFilter: ['password']
    }
});
```

这段代码的含义就是生成的 JSON 结果要包含 Group 类型对象里面的 id, groupName 和 users 属性，而去掉 User 类型对象里面的 password 属性。有关 [Jackson](http://jackson.codehaus.org/ "Jackson JSON Processor") 及 [JsonFilter](http://wiki.fasterxml.com/JacksonFeatureJsonFilter "JacksonFeatureJsonFilter") 的更多用法请参考官方文档。

#### stream()

以流的形式返回响应。该方法的第一个参数是 JSGI request 对象，第二个参数可以是一个对象或者一个方法。如果是一个对象，则包含如下字段：

- **status** \- HTTP Status Code，可选，默认为 200
- **headers** \- 响应头信息，可选，默认为 Content-Type: binary/octet-stream
- **callback** \- 数据流处理方法，该方法会被传入 request.env.servletResponse.getOutputStream() 对象，用来向响应中以流的方式写入数据

如果第二个参数是一个方法，其含义就相当于上文中的 callback。

```js
response.stream(request, {
    status: 200,
    headers: {
        'Content-Type': 'application/ms-word'
    },
    callback: function (outputStream) { // outputStream = request.env.servletResponse.getOutputStream()
        // 向 outputStream 中写入数据流
    }
});
```

**注意**，不要在 callback 方法中手动关闭 outputStream 流，当响应完毕后，系统会自动将其关闭。