## 一、Zookeeper简介

> Apache Zookeeper致力于开发和维护开源服务器，实现高度可靠的分布式协调

```
Zookeeper是一种集中式服务，用于维护配置信息，命名，提供分布式同步和提供组服务。
所有这些类型的服务都以分布式应用程序的某种形式使用。
```



### 1.1 Zookeeper功能

* 存储数据
* 监听



### 1.2 Zookeeper安装

* 从apache的zookeeper下载最新的安装包，在`/usr/local/zookeeper`下解压
* 解压后的文件目录与tomcat类似（bin、conf、docs、lib）
* 运行`zkServer.sh`会报错，现在没有`zoo.cfg`（这是因为默认的sample配置文件是`zoo_sample.cfg`，需要修改名字）
* 可以改配置文件中的`dataDir`，来改变zookeeper的数据的位置

注：如果运行的时候不指定配置文件，默认是`zoo.cfg`

最好指定配置文件

服务器启动命令: `./zkServer.sh start [配置文件的相对路径或绝对路径]`

客户端启动命令: `./zkCli.sh`  (需要等到服务器启动才能启动)



### 1.3 Zookeeper命令

#### 服务器命令

* 启动

  * ```shell
    ./zkServer.sh start [配置文件]
    ```

* 查看

  * ```shell
    ./zkServer.sh status [配置文件]
    ```

* 停止

  * ```shell
    ./zkServer.sh stop [配置文件]
    ```



#### 客户端命令

* 创建path

  * ```
    create path data
    
    // e.g.
    // 创建一个test的path
    create /test  
    // 创建一个test的path，值为abc
    create /test abc
    ```

  * path必须以`/`开头，zookeeper的znode是通过路径引用来访问节点的

  * 不带任何参数，默认是创建`持久节点`

  * `-s`  : 创建`序号节点`

    * 节点的路径会在path的基础上加一个值

  * `-e`  : 创建`临时节点`

  * `-c`  : 创建容器container节点

    * 当容器节点内没有子节点之后，会被zk定期删除

  * `-t ttl`  : 指定节点的到期时间

* 获取znode的数据

  * ```
    get path
    ```

  * `-s`  : 查看详细信息

* 查看路径下的子节点

  * ```
    ll [znode节点路径]
    ```

  * 默认是直接子节点

  * `-R`  : 递归查询所有子节点

* 删除节点

  * ```
    // 删除empty node
    delete path
    ```

  * `-v version`  : 指定node的version（乐观锁）

  * ```
    // 递归删除node
    deleteall path
    ```

* 为当前会话添加用户名和密码

  * ```
    addauth 账号:密码
    ```

* 创建node并添加权限

  * ```
    create path data auth:账号:密码:权限(crwda)
    ```

  * 