# Ajax
    Asynchronous Javascript and XML
    允许浏览器与服务器通信而无需刷新当前页面的技术
## Ajax的原生使用方式
* XMLHttpRequest
    * 基本上所有浏览器都兼容，除了ie6及以下的版本
    * 使用此对象来发送Ajax请求
        * open(method, url, asyc)
            * method:请求的方式，一般是GET或者POST，不区分大小写
            * url:请求的url地址
            * asyc:是否为异步请求，默认是true，异步方式
        * setRequestHeader(key, value)
            * key：http请求中请求头信息的键
            * value：http请求中请求头信息的值
            * ***注意***：如果ajax想要发送post请求，需要添加setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        * send(string)：发送ajax请求
            * string：请求体内容
    * 使用此对象来获取响应数据
        * responseText：获取字符串形式的响应数据
        * responseXML：获取XML形式的响应数据
        * status和statusText：以数字和文本形式返回Http状态码
        * getAllResponseHeader()：获取所有响应报头
        * getResponseHeader()：查询响应中某个字段的值
    * readState属性
        * 0：请求未初始化，open还没有调用
        * 1：服务器连接已建立，open调用了
        * 2：请求已接收，也就是接收到请求头信息了
        * 3：请求处理中，也就是接收到请求体了
        * 4：请求完成，并且响应就绪（一般客户端直接判断是否为此值即可）
* 范例
```
    var request = new XMLHttpRequest();
    request.open("GET", "get.php", true);
    request.send(null);
    request.onreadystatechange = function() {
        if (request.readyState === 4 && request.status === 200) {
            // request.responseText
        }
    }
```
## 用jQuery实现ajax
### 形式
    jQuery.ajax([settings])
### 参数
* type：类型，“POST”或“GET”，默认是“GET”
* url：请求的地址
* data：js对象，是连同请求一起发送到服务器的数据
* dataType：预期服务器返回的数据类型。如果不指定，jQuery将自动根据HTTP包MIME信息来智能判断，一般采用json格式，可以设置为“json”
* success：请求成功后的回调函数。参数是返回的数据，以及包含成功代码的字符串
* error：请求失败后的回调函数。参数是XMLHttpRequest对象

## 跨域
### 域名的组成
* http://www.abc.com:8080/scripts/jquery.js
    * [http://]：协议
    * [www]：子域名
    * [abc.com]：主域名
    * [8080]：端口号
    * [scripts/jquery.js]：请求资源地址
### 跨域
* 当协议、子域名、主域名、端口号中有任意一个不同时，都算作不同域
* 不同域之间相互请求资源，就是“跨域”
* Javascript出于安全方面的考虑，不允许跨域调用其他页面的对象。
* 解决方法
    * 当A服务器需要请求B服务器资源时，可以在A服务器的后台做一个**代理**，调用B服务器的服务，再把响应结果返回给前端
    * 


### 在使用Spring MVC进行ajax开发遇到的一些细节和感悟
* 首先，我在springmvc项目中开始避免使用jsp等前后端耦合程度比较高的技术，但springmvc对于html等静态资源是自动过滤的。对于index.html，由于在mvc:resources配置项中location属性不能填写"/",所以将其放入到html文件夹下，通过项目启动时使用一个接收"/"路径请求的方法，该方法返回首页。还需要配置视图解析器。
* 对于使用@ResponseBody注解，该注解返回的结果不会被视图解析器解析，而是自动将返回的数据作为json字符串返回。但是，项目中CharacterEncodingFilter中的编码设置对其无效，对中文会返回乱码，可以采用在@RequestMapping注解中添加produces属性，该属性是用来指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回***（produces="application/json;charset=UTF-8"）***
* 返回的结果要符合json数据形式的格式，对于值为字符串的数据还需要添加双引号
    * 示例
    "{\"name\":\"" + member.getName() + "\", \"number\":" + member.getNumber() + ",\"age\":" + member.getAge() + ",\"post\":\"" + member.getPost() + "\"}"
* 对于post请求，一定要设置RequestHeader中Content-type的值为application/x-www-form-urlencoded，否则后台无法接收到请求体信息
* 由于每次返回json形式的字符串比较麻烦（因为自己拼接，极大可能遗漏，吃力不讨好），采用一个ResultVO类，该类是所有controller层的json形式的数据返回给前端的对象，但因为使用@ResponseBody注解返回的时候会将其解析成json形式的字符串，因此需要***添加jackson依赖让springmvc将其转化***。在该类中，有是否操作成功的字段，此次操作处理结果的信息，以及返回的数据。
### 使用Ajax完成三级联动需求
* 对于mybatis整合springmvc，导入整合依赖包，在配置文件中添加连接池，SqlSessionFactoryBean和MapperScannerConfigurer的配置，然后在pom文件中增加对接口配置文件的过滤，最后使用注解将接口添加到Spring容器
* 对于集合转数组，使用toArray的无参方法有时候不管用，需要使用有参方法传入一个与集合大小相同的数组
* tomcat使用Druid连接池的时候在关闭时，无法终止mysql进程，需要添加监听器手动关闭
    ```
    Enumeration<Driver> drivers = DriverManager.getDrivers();
        Driver d = null;
        while (drivers.hasMoreElements()) {
            try {
                d = drivers.nextElement();
                DriverManager.deregisterDriver(d);
                System.out.println(String.format("ContextFinalizer:Driver %s deregistered", d));
            } catch (SQLException ex) {
                System.out.println(String.format("ContextFinalizer:Error deregistering driver %s", d) + ":" + ex);
            }
        }
            AbandonedConnectionCleanupThread.checkedShutdown();
    ```
* 使用springmvc注解时，如果返回的结果是json形式的数据，需要添加@ResponseBody属性，并且添加jackson的依赖，spingmvc会自动对数据进行转换
* 使用Druid数据源时，连接数据库除了四个参数，还需要指定testWhileIdle和validationQuery属性
* mybatis的接口和接口配置文件的名字要相同，不然会报错