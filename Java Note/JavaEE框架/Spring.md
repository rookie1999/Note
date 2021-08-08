# Spring
## Spring实战
### DI依赖注入
    目的：让相互协作的软件组件保持松耦合
* 创建应用组件之间协作的行为称为装配（wiring）
* 配置信息
    * xml：ClassPathXmlApplicationContext
    * java：AnnotationConfigApplicationContext
### AOP
              目的：把应用各处的功能分离出来形成可重用的组件
#### 底层技术
* JDK动态代理
    * 要求：被代理对象需要实现一个接口
    * 使用Proxy类的newProxyInstance方法
        * 第一个参数ClassLoader：ClassLoader.getSystemClassLoader()，用来加载代理对象的字节码文件
        * 第二个参数interfaces：被代理对象.getClass().getInterfaces()
        * 第三个参数InvocationHandler：通过匿名内部类来实现InvocationHandler，用来提供增强的代码
            * 复写接口的invoke(Object proxy, Method method, Object[] args):执行被代理对象的任何方法都会执行此方法
                * Object proxy:代理对象的引用
                * Method method:需要被执行的方法
                * Object[] args:方法执行的参数
#### 配置步骤
* 导入aop依赖和aspectj依赖
* 在配置文件中导入aop约束
* 将通知bean交给spring管理
* 使用aop:config标签表明开始aop的配置
* 使用aop:aspect标签表明配置切面
    * id属性：切面的唯一标识
    * ref属性：通知bean的id
* 在aop:aspect内部使用对应标签来配置通知的类型
    * aop:before：表示配置前置通知
        * method属性：用于指定通知bean的需要执行的方法
        * pointcut属性：用于指定切入点表达式，该表达式的含义是对哪些方法进行增强
        * 切入点表达式的写法：
            * 关键字：execution(表达式)
            * 访问修饰符  包名.包名...类名.方法名(参数列表)
            * 示例：public void cn.zhanguozhi.*.*(..)
            * 访问修饰符可以不写
            * 参数列表
                * 基本数据类型直接写名称    int
                * 引用数据类型写包名.类名    java.lang.String
                * *通配符表示任意参数，但是不匹配无参
                * ..表示任意参数
            * 全通配写法：* *..*.*(..)
            * 实际开发中切入点表达式写到业务层实现类的所有方法
        * pointcut-ref属性：切入点表达式的id
* aop:pointcut标签用来配置切入点表达式
    * id属性：切面表达式的唯一标识
    * expression属性：切入点表达式
    * 写在aop:aspect标签内部，只在当前切面内可以引用
    * 在aop:aspect标签外部，需要写在该标签之前，否则报错
* 通知类型
    * aop:before 前置通知，在切入点方法执行之前执行
    * aop:afterReturning 后置通知，在切入点方法正常执行之后执行
    * aop:afterThrowing 异常通知，在切入点方法产生异常之后执行，与后置通知不同时出现
    * aop:after 最终通知，最后一定执行
    * aop:around 环绕通知 需要手动执行目标方法，Spring框架提供了一个接口ProceedingJoinPoint。该接口有一个方法proceed(),表明明确调用目标方法，该接口可以作为环绕通知的参数，Spring框架会在运行时自动提供该接口的实现类
### 基于注解的aop配置
* 声明切面类：@Aspect
* 声明切入点表达式
```
   @Pointcut("execution(* cn.zhanguozhi.service.impl.*.*(..))")
   private void pt1() {}
```
* 声明通知类型
    * @Before("pt1()")：前置通知，value的值是切入点表达式
    * @AfterReturning
    * @AfterThrowing
    * @After
    * @Around
* 开启spring对注解aop的支持
    * xml方式：添加aop:aspectj-autoproxy标签
    * 注解：@EnableAspectjAutoproxy
* 注意：基于注解的aop在调用顺序上有问题，如果需要一起使用，推荐使用**环绕通知**
## Spring事务控制
### Spring事务控制的一组API
* PlatformTransactionManager：进行事务管理的接口
    * DataSourceTransactionManager
    * HibernateTransactionManager
* TransactionDefinition:事务定义
    * String getName():获取事务对象名称
    * int getIsolationLevel():获取事务隔离级别，spring的隔离级别默认使用的是**数据库的隔离级别**
    * int getPropagationBehavior():获取事务传播行为
        * REQUIRED:如果当前没有事务，就新建一个事务；如果当前存在事务，就加入到该事务中（**增删改**的选择）
        * SUPPORTS:支持当前事务。如果当前不存在事务，就以非事务方式执行（**查询**）
    * int getTimeout():获取事务超时时间
        * 默认值是-1，即没有超时时间
        * 如果有，以秒为单位
    * boolean isReadOnly():获取事务是否只读
        * 建议查询设置为只读
* TransactionStatus:该接口描述了某个时间点上事务对象的状态信息
    * void flush():刷新事务
    * boolean hasSavePoint():是否存在存储点
    * boolean isCompleted():事务是否完成
    * boolean isNewTransaction():事务是否为新的事务
    * boolean isRollbackOnly():事务是否回滚
    * void setRollbackOnly():设置事务是否回滚
### 基于配置文件的Spring事务控制配置步骤
* 配置事务管理器
    * 配置DataSourceTransactionManager的bean，并且为dataSource属性注入一个数据源bean的id
* 配置事务通知tx:advice标签
    * 需要导入tx和aop的约束
    * id属性：事务通知的唯一标识
    * transaction-manager属性：事务管理器的beanid
* 配置切入点表达式
    * 在aop:config标签内，配置aop:pointcut标签
* 建立事务通知与切入点表达式的对应关系
    * 在aop:config内部，配置aop:advisor标签
        * advice-ref属性：事务通知的id
        * pointcut-ref属性：切入点表达式的id
* 配置事务的属性tx:attributes
    * 在事务通知tx:advice内部，使用tx:attributes标签
    * 在tx:attributes标签内部使用tx:method标签
        * method属性：事务属性生效的方法，可以使用通配符*，如find*
        * isolation属性：事务的隔离级别，默认是DEFAULT，使用数据库的隔离级别
        * propagation属性：事务的传播行为，默认是REQUIRED
        * read-only属性：事务是否只读，默认是false
        * timeout属性：事务的超时时间，默认是-1，永不超时。以秒为单位
        * rollback-for属性：指定一个异常，产生该异常时，事务回滚，产生其他异常，事务不回滚。
        * no-rollback-for属性：指定一个异常，产生该异常时，事务不回滚，产生其他异常，事务回滚
* 例子
```
    <!--1.配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--2.配置事务通知-->
    <tx：advice id="txAdvice" transaction-manager="transactionManager">
        <!--5.配置事务属性-->
        <tx:addtributes>
            <!--设置增删改方法的事务属性-->
            <tx:method name="*" propagation="REQUIRED" read-only="false"/>
            <!--设置查询方法的事务属性-->
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>
    <!--3.配置aop-->
    <aop:config>
        <aop:pointcut id="pt1" expression="execution(* cn.zhanguozhi.service.impl.*.*(..))"/>
        <!--4.建立事务通知与切入点表达式的对应关系-->
        <aop:advisor advice-ref="txAdcice" pointcut-ref="pt1"/>
    </aop:config>
```
### 基于注解的Spring事务控制配置步骤
* 在xml文件中配置事务管理器
* 在xml文件中使用tx:annotation-driven开启对注解事务的支持
    ```
    <tx:annotation-driven transaction-manager="transactionManager"/>
    ```
* 在需要事务的类或方法上添加@Transactional注解，在类上则对类中所有的方法生效
    * 该注解拥有事务通知中的属性相同的属性，如propagation
### 基于Java配置的Spring事务控制
* 在配置类上开启对事务的支持，使用注解@EnableTransactionManagement
## Spring新注解
* @Configuration
    * 在类上添加，可以使得该类成为配置类（作用与xml配置文件相同）
    * 配置类可以有多个
* @ComponentScans
    * 在类上添加，可以声明多个@ComponentScan
* @ComponentScan
    * 在类上添加，通过value或basePackages配置要扫描的包，可以配置多个类路径
* @Bean
    * 在方法上添加，用于把当前方法的返回值添加到ioc容器中
    * name属性可以设置bean的id，如果不写时，默认值是**当前方法的名称**
    * 细节：当使用注解配置方法的时候，如果有参数，spring框架会去容器中按照**@Autowired**的方式去查找有没有可用的bean对象
* IOC容器的创建
    * 之前对于配置文件的形式，使用的创建方式是通过Class'pathXmlApplicationContext
    * 对于采用Java配置类形式，需要使用**AnnotationConfigApplicationContext**
    * 其参数是Java配置类的Class对象类型的数组
* @Scope
    * 设置bean的作用范围
    * value属性可以设置bean对象的作用范围
        * singleton：默认，单例
        * prototype：多例
        * request：web应用的请求范围
        * session：web应用的会话范围
        * global-session：作用于集群环境的会话范围，当不是集群环境时，与session相同
* @Import
    * 导入其他spring配置类
    * value属性设置其他配置类的字节码文件
* @PropertySource
    * 用来导入properties文件
    * value属性设置properties的全限定路径的字符串形式，可以同时指定多个、
    * properties文件内部的键值对可以通过@Value属性直接获取
## Spring整合Junit
* 导入spring整合junit的依赖：spring-test
* @Runwith：将原来的main方法替换成spring提供的
* @ContextConfiguration：告知spring是基于xml还是注解配置，并提供配置的所在位置
    * locations：xml配置文件所在位置
    * classes：注解类所在位置

    注：当使用spring 5.x以上的版本时，需要junit的坐标是4.12及以上的
## 问题
### 在原生Servlet中使用spring的注解进行自动注入
```
    这个是不行的
    因为spring的自动注入要求注入的类和被注入的类都需要交由spring容器管理，而servlet是由服务器创建的，所以不受spring管理，因此无法注入
```