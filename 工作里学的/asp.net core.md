# ASP.NET

## 介绍

> 

* 跨平台
* MVC与WebAPI统一的编程模型
  * 在MVC Controller和ASP .NET Web API在这两种情况下，我们创建的Controller都从相同的Controller基类继承，并返回IActionResult
* 依赖注入
* 可测试性
* 开源
* 模块化
  * 提供了模块化的中间件组件
  * Request和Response的管道都使用了中间件组件



## ASP.NET Core 项目

### Program和Startup

> Host.CreateDefaultBuilder(args)

* 设置Web服务器
* 加载主机和应用程序配置表信息
* 配置日志记录



### 进程托管形式

* InProcess 进程内托管

  * 在项目的csproj文件中指定

  * ```xml
    <PropertyGroup>
    <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
    </PropertyGroup>
    ```

  * 在进程内托管的情况下，CreateDefaultBuilder()方法调用UseIIS()方法并在IIS进程（w3wp.exe或iisexpress.exe）内托管应用程序（**整个应用托管在IIS进程中**）

  * 从性能的角度看，InProcess托管比OutOfProcess托管提供了更高的请求吞吐量

* OutOfProcess  进程外托管 （默认）

  * 两个Web服务器，内部服务器和外部服务器
  * 内部服务器是Kestrel
    * Kestrel是asp.net core的跨平台服务器，本身可用作边缘服务器
  * 外部服务器是IIS，nginx或apache
  * 使用cmd的`dotnet run`模式并在csproj文件中设置OutOfProcess即可运行
  * 进程名是ProjectName
  * 多了内部服务器与外部服务器之间请求的过程的性能损耗



### launchsettings.json

```xml
{
<!--配置iis-->
  "iisSettings": {
	<!--关闭windows验证-->
    "windowsAuthentication": false,
	<!--开启匿名验证-->
    "anonymousAuthentication": true,
	<!--iis express的url和端口-->
    "iisExpress": {
      "applicationUrl": "http://localhost:9937",
      "sslPort": 44313
    }
  },
  <!--对应运行的不同环境-->
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "ASPNetDemo": {
      "commandName": "Project",
      "launchBrowser": true,
      "applicationUrl": "https://localhost:5001;http://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

| Command Name | AspNetCoreHostingModel | Web Server                   |
| ------------ | ---------------------- | ---------------------------- |
| Project      | 忽略进程托管的配置     | Kestrel                      |
| IISExpress   | InProcess              | IIS Express                  |
| IISExpress   | OutOfProcess           | 内部Kestrel，外部IIS Express |
| IIS          | InProcess              | IIS                          |
| IIS          | OutOfProcess           | 内部Kestrel，外部IIS         |



### appsettings.json

>  ASP.NET Core的配置源

1. appsettings.json, appsettings.{Environment}.json
2. User secrets 用户机密
3. Environment variables
4. Command line arguments

**优先级逐渐升高**

通过构造器的依赖注入使用`IConfiguration`



### ASP.Net Core中间件

* 可同时被访问和请求
* 可以处理请求后，然后将请求传递给下一个中间件
* 可以处理请求后，并使管道短路
* 可以处理传出响应
* 中间件是按照添加的顺序执行的

执行顺序：第一个中间件处理请求，将结果传给第二个中间件，第二个中间件处理请求，将结果传递给下一个中间件，直到最后一个中间件处理完，开始往回传递，传出响应



### ASP.NET Core中的静态文件

* ASP.NET Core默认不支持静态文件服务
* 默认的静态服务文件夹为wwwroot
* 要使用静态文件，必须使用UseStaticFiles()
* 要使用默认文件，必须使用UseDefaultFiles()
  * 默认支持的文件列表
    * index.html
    * index.htm
    * default.html
    * default.htm
* UseDefaultFiles()必须注册在UseStaticFiles()之前
* UseFileServer结合了UseStaticFiles，UseDefaultFiles和UseDirectoryBrowser中间件的功能