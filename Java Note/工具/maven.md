# maven
项目构建工具
## 1. maven的两大功能
### 1.1 依赖管理
* maven对依赖（jar包）会进行统一管理
* 对于传统项目来说，jar包一般会使得项目的大小很大，因为jar包占用了很大的空间；而使用maven进行管理的项目，不含有jar包，所以体积很小
* maven使用坐标信息（公司名称 + 项目名称 + 版本号）来定位jar包，将jar包统一存放在maven仓库中，在项目中如果需要使用到某一jar包，只需要在配置文件中配置该jar包的坐标，maven会自动的在仓库中找到该jar包（maven会在本地仓库中建立索引，提高寻找jar包的效率），并添加到本项目的classpath
* 使jar包与项目分离，减少项目体积
* ### 1.2 项目构建
* 可以对项目进行编译、测试、打包、部署一系列操作。这些操作都是通过maven的`命令`实现的
* 使用maven命令将web项目发布到tomcat`mvn tomcat:run`，即一键构建
    * 使用需要先进行两个步骤
    * 1.添加tomcat插件
        ```
        <build>
    <finalName>babasport</finalName>
    <!-- 添加tomcat插件,eclipse不用再配置tomcat运行环境.
         Run As-Maven build...-clean package 启动web页面
    -->
    <plugins>
        <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.0.2</version>
            <!-- 本地jdk版本 -->
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat8-maven-plugin</artifactId>
            <version>3.0-r1756463</version>
            <configuration>
                <port>8080</port>
            </configuration>
            <!-- 在package阶段启动web容器 -->
            <executions>
              <execution>
                      <!-- 打包阶段 -->
                    <phase>package</phase>
                    <!-- 运行目标 -->
                    <goals>
                        <goal>run</goal>
                    </goals>
              </execution>
          </executions>
        </plugin>
    </plugins>
  </build>
        ```
    * 2.添加servlet、jsp依赖
        ```
        <!-- 配置jsp依赖 -->
        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
            </dependency>
        <!-- https://mvnrepository.com/artifact/jsptags/pager-taglib -->
        <dependency>
            <groupId>jsptags</groupId>
            <artifactId>pager-taglib</artifactId>
            <version>2.0</version>
            <!--  -->
            <scope>provided</scope>
        </dependency>
        ```
### 2. Maven的好处
1. 依赖管理
2. 跨平台性，因为是用纯Java开发的，需要依赖JDK，可以在window、linux上使用
3. 清晰的项目结构
4. 多工程开发，将模块拆分成若干个工程，利于团队协作开发
    * 如果多模块之间需要进行通信，那么可以将模块打包成jar放到私服或者本地仓库，其他模块需要在pom文件中导入坐标即可使用，无需编译
5. 一键构建项目，使用命令可以对项目进行一键构建，无需ide和tomcat
### 3.配置环境变量
* 配置MAVEN_HOME环境变量，值为maven的安装目录
* 在Path中添加一个值`%MMAVEN%\bin`
* `mvn -v`查看maven的版本信息
### 4.自定义本地仓库
* 在maven的安装目录下的conf文件夹下的settings.xml中修改LocalRepository的值为自定义的本地仓库
### 5.配置镜像仓库——阿里云
* 在mirrors标签中增加一组配置
```
    <mirrors>
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
```
### 6.仓库类型
* 本地仓库
* 私服：存在于局域网上的一台服务器，局域网其他服务器如果想使用，需要安装私服服务器
* 中央仓库：存在于互联网上