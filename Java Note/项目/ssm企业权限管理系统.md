# ssm企业权限管理系统
## 前期准备
### 数据库表
- 产品表
```
    create table product(
    id varchar(36) primary key,          
    product_num varchar(50) unique not null comment "产品编号",
    product_name varchar(50) comment "产品名称",
    city_name varchar(50) comment "出发城市",
    departure_time timestamp comment "出发时间",
    product_price int comment "产品价格",
    product_desc varchar(500) comment "产品描述",
    product_status tinyint comment "状态（0关闭 1 开启）"
    );
    insert into product
    values
    (uuid(), 1, "北京三日游", "天津", "2020-10-1 10:00:00", 502, "感受首都的魅力吧", 1),
    (uuid(), 2, "上海五日游", "上海", "2020-5-1 9:00:00", 1038, "魔都我来了", 1),
    (uuid(), 3, "德国游玩半年", "香港", "2020-3-1 8:00:00", 15400, "工业现代化的国家", 1);
```
    注：在mysql数据库中，不能将字段的默认值设置为函数，需要在插入数据时指定；但是在orcale数据库可以

## 实践中遇到的一些问题以及解决方法
* 静态资源加载的问题
    * 通过springmvc提供的mvc:resources标签的mapping和location属性进行配置
    * 如果通过相对路径来加载，请求路径上会缺少项目名
        * 对于jsp来说，可以使用EL表达式来获取项目名    request.contextPath
        * 对于html，可以使用js代码获取项目名，并同时修改路径
    * 但是对于***图片***，即使请求路径正确也无法加载
        * 由于所有的请求都是通过前端控制器响应的，所以对于图片的加载需要使用其他方式
        * springmvc提供了这种方式，使用名为default的servlet进行响应，处理url-pattern为**.png**的请求
* EL表达式除法运算符不能取整的问题
    * 使用fmt标签，导入fmt标签空间
        * ```
                <%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
            ```
    * fmt标签  value属性中放置表达式  type属性是表达式的结果的类型 maxFractionDigits属性是允许超过的分数最大位数，多余的位数按照四舍五入处理
    * ```
            <!--a/b向下取整: a减去a与b的余数之后除以b-->
            <fmt:formatNumber type="number" value="${((products.size() - products.size() % 5) / 5) + 1}" maxFractionDigits="0"/>
        ```
    * springmvc前端数据传递到后端，用对应的实体类接收，需要表单内的字段的name属性为实体类的属性名对应
        * springmvc绑定参数类型之类型转换有三种方法
            * 实体类中添加日期格式化注解
                * ```
                           //只对当前属性生效
                           @DateTimeFormat(pattern="yyyy-MM-dd HH:mm")
                            private Date departureTime;
                    ```
                * 使用Converters进行类型转换
                * 属性编辑器是spring3.1之前的方法，已经弃用
        * 对于Date类与字符串前后端传递数据，一定要保证解析和格式化的pattern相同，否则参数转换失败
* mybatis创建实体类字段与数据库表的映射
    * 使用配置文件时，可以使用ResultMap标签
    * 使用注解时，可以使用@Results
        * ```
                       @Results(id="product", value={
                        @Result(id=true, column="id", property="id"),
                        @Result(column="product_num", property="productNum"),
                        @Result(column="product_name", property="productName"),
                        @Result(column="city_name", property="cityName"),
                        @Result(column="departure_time", property="departureTime"),
                        @Result(column="product_price", property="productPrice"),
                        @Result(column="product_desc", property="productDesc"),
                        @Result(column="product_status", property="productStatus"),
                        })
        * 如果想要使用已经创建好的映射关系，可以使用@ResultMap注解，其value属性为@Results的id值
                        @ResultMap(value="product")
* js代码进行**页面跳转**
    ```
        onclick="location.href="product-add.jsp"
    ```
* mybatis的参数绑定问题
    * 如果参数多于一个，需要额外处理，需要对于每一个参数使用@Param注解，其value值即为sql语句中的***#{@Param的value}***
* 关于jquery
    * $ is not defined：出现这个问题说明jquery的文件导入失败，需要在**页面的开头部分**引入jquery文件
