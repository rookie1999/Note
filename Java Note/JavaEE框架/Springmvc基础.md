# Springmvc基础
## 首先，导入依赖
    commons-fileupload
    commons-io
但是，导入fileupload会自动导入io依赖
## 传统文件上传
### 上传步骤
```
    // 首先，创建在服务器存放上传文件的文件夹
    String path = request.getServletContext().getRealPath("/uploads/");
    if (!path.exists()) {
        // 如果文件夹不存在则创建文件夹
        // 注：mkdir()和mkdirs()的区别：
                //mkdir():只能创建一级目录
                //mkdirs():可以创建多级目录
        path.mkdirs();
    }
    
    // 创建磁盘文件项工厂
    DiskFileItemFactory factory = new DiskFileItemFactory();
    ServletFileUpload upload = new ServletFileUpload(factory);
    // 解析请求，获取文件上传项
    List<FileItem> items = upload.parse(request);    // 会抛异常
    
    // 上传文件
    for (FileItem item : items) {
        if (!item.isFormField()) {
            // 上传的文件项如果是文件，则不是表单字段
            // 生成唯一文件名  防止上传相同文件进行覆盖
            String filename = UUID.randomUUID().toString().replace("-","") + item.getName();
            // 上传
            item.write(path, file);            // 会抛异常
            // 如果上传的文件超过一定大小，会生成临时文件
            item.delete();
        }
    }
    return "success";
```
## springmvc方式上传文件
### 原理分析
基于springmvc的组件式开发
对于文件上传来说，传统的方式是需要通过编程式进行请求解析，并且遍历文件项再上传，最后还要将临时文件删除
使用Springmvc之后，对于文件上传，可以使用文件解析器CommonsMultipartResolver进行自动解析，并且生成文件对应的文件项，类型是MultipartFile

### 配置文件解析器
在spring的配置文件中，增加一个文件解析器的bean，类型是CommonsMultipartResolver，bean的id必须是multipartResolver
可以在内部配置文件的相关属性
```
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--配置文件上传的最大限制-->
<!--        <property name="maxUploadSize" value="102400"/>-->
    </bean>
```
***注：bean的id必须是multipartResolver***
### 上传步骤
前端控制器接收到请求之后，先交由文件解析器解析，再通过处理器映射器执行相应的方法，并且把解析结果填充到MultipartFile类型的方法参数
***注：MultipartFile类型的方法参数的参数名必须和前端页面的file表单元素的name属性的值相同***
```
    // 创建服务器文件夹用以存放上传文件
    String path = request.getServletContext().getRealPath("/uploads/");
    File file = new File(path);
    if (!file.exists()) {
        file.mkdirs();
    }
    
    // 上传文件项
    String fileName = UUID.randomUUID().toString().replace("-", "") + "_" + upload.getOriginaiName();
    upload.transferTo(path, fileName);     // 抛出IOException
    // 临时文件会自动删除
    return "success";
```
## 跨服务器上传
首先，是在springmvc的配置的基础上
### 导入依赖
* com.sun.jersey
    * jersey-client
    * jersey-core
### 上传代码
```
    // 1.远程服务器的接收文件的url
    String url = "http://localhost:9090/uploads/";
    // 2.上传文件的fileName   upload的类型是MultipartFile
    String fileName = UUID.randomUUID().toString().replace("-", "") + "_" + upload.getOriginalName();
    // 3.创建客户端对象
    Client client = Client.create();
    // 4.与远程服务器进行连接
    WebResource webResource = client.resource(path + fileName);
    // 5.上传文件(通过字节数组上传)
    webResource.put(upload.getBytes());
```

# SSM框架整合
## Spring整合SpringMVC
1.首先，导入依赖，创建三层架构，都是通过spring向另外两个框架进行整合
2.在web.xml中配置前端控制器并设置读取springmvc配置文件以及服务器启动就创建，和字符编码过滤器配置UTF-8编码
3.spring的配置文件和springmvc 的配置文件不要写在一起，要分成两个
&emsp;a.在spring的配置文件中，对service层和dao层进行扫描，在开启扫描的注解中使用exclude字段忽略Controller注解
&emsp;b.在springmvc的配置文件中，扫描controller层，并配置
4.由于spring的配置文件没有进行读取，所以整合的时候需要使用ServletContext域对象的Listener进行读取，因为该对象在服务器创建时创建，springmvc中已经实现了该类ContextLoaderListener，但默认读取的是WEB-INF文件夹下的applicationContext，可以设置全局配置中指定ContextConfiguration，指定为classpath:spring.xml
* 上述通过ContextLoaderListener加载的spring配置文件，spring会创建一个WebApplicationContext上下文对象，称为父上下文，保存在ServletContext中，key是WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE的值
    * 可以使用spring提供的工具类来取出上下文对象
    * WebApplicationContextUtils.getWebApplicationContext(ServletContext);
* DispatcherServlet可以同时配置多个，每一个都可以加载多个配置文件，并且拥有一个自己的上下文对象（WebApplicationContext），称为子上下文对象，保存在Request对象中，key是DispatcherServlet.class.getName() + ".CONTEXT"。
    * 可以使用工具类取出上下文对象：RequestContextUtils.getWebApplicationContext(request);
* 需要注意的地方：父上下文对象不可以访问子上下文对象的的内容，但子上下文对象可以访问父上下文对象的内容
* 对于大项目：使用传统方式（方便扩展维护）
    * 父上下文容器中保存数据源、服务层、DAO层、事务的Bean
    * 子上下文容器中保存Mvc相关的Action的Bean
* 对于小项目：使用激进方式（方便开发）
    * 全部放入子上下文容器中
## Spring整合Mybatis
1.需要额外导入mybatis-spring
2.将原来的mybatis的主配置文件的内容，配置到spring的配置文件
整合步骤如下：
```
    // a.配置连接池
    <bean id="dataSource" class="DataSource的类">
        <!--配置数据库需要的参数-->
        <property name=""  ...
    </bean>
    // b.配置SqlSessionFactory
    // SqlSessionFactoryBean是mybatis-spring的包中的类，根据注入的连接池创建SqlSessionFactory
    <bean id="sqlSessionFactory" class="SqlSessionFactoryBean">
        <!--需要注入datasource-->
        <property name="dataSource" ref="dataSource" />
        <!--如果是xml配置还需要设置映射配置文件的位置-->
        <property name="mapperLocations" value="cn/zhanguozhi/dao/mapper/*.xml" />
    </bean>
    // c.配置接口所在包
    // 核心是MapperScannerConfigurer（映射扫描配置类）
    <bean id="mapperScannerConfigurer" class="MapperScannerConfigurer">
        <!--扫描dao层-->
        <property name="basePackage" value="cn.zhanguozhi.dao" />
    </bean>
```
最后需要在dao层接口上添加***@Repository***注解
注：如果是采用xml配置，若是写在java目录下，不会编译到war包中，需要在pom文件中加入resources节点