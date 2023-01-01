> 视频地址：https://www.bilibili.com/video/BV1Ls411n7mx
>
> 文档地址：https://docs.docker.com/

### 常见问题

#### Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied

执行docker命令一般需要root权限，加上`sudo`即可



#### sudo docker run -d 镜像名/镜像ID

以后台模式创建一个容器，再使用`docker ps`命令时容器已经退出。

**因为docker容器后台运行，就必须要有一个前台进程**。



### 什么是Docker

开发人员将开发好的软件或者代码移交给运维时，经常会出现部署失败的情况（大多数是开发环境成功，但是运维环境失败）

为了避免这类情况的发生，Docker给出了一个**标准化的解决方案**

即，在软件安装的时候，把原始环境一模一样的复制过来

传统上认为，软件编码开发/测试结束以后，所产出的成果即是程序或是能够编译执行的二进制字节码等。为了让这些程序可以顺利执行，开发团队也得准备完整的部署文件，让运维团队部署应用。开发团队需要告诉运维团队，用的全部配置文件+所有软件环境。

不过，即便如此，仍然常常发生部署失败的情况。

Docker镜像的设计，使得Docker得以打破过去[程序即应用]的观念。透过镜像(images)将作业系统的核心除外，运作应用所需要的系统环境（操作系统发行版、运行依赖包、运行环境、配置环境、运行文档），由下而上的打包，达到应用程式跨平台间的无缝接轨运作。

> Docker是基于Go语言实现的云开源项目
>
> Docker的主要目标是“Build，Ship and Run any App，anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的App（可以是一个WEB应用或者是数据库应用等等）及其运行环境做到`一次封装，到处运行`

将应用运行在Docker容器上面，而Docker容器在任何操作系统上都是一致的，这就实现了跨平台，跨服务器。只需要一次配置好环境，换到别的机子上就可以一键部署，大大简化了操作。

> 一句话描述：Docker解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术

### Linux Container

与虚拟机虚拟化技术不同，是另一种虚拟化技术

Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

容器内的应用进程直接运行于宿主的内核，容器没有自己的内核，而且也没有硬件虚拟。

每个容器之间互相隔离，都有自己的文件系统，容器之间进程不会相互影响，能区分计算资源。

#### 好处

* 更快速的应用交付和部署
* 更便捷的升级和扩缩容
* 更简单的系统运维
* 更高效的计算资源利用



### 支持版本

Centos 6.5 以上

Ubuntu 16.04以上



### 三要素

![](C:\zhan\Note\容器\docker-pic\1.PNG)

* 镜像 image
* 容器 container
* 仓库 repository

镜像就是模板，容器就是用镜像创建的一个运行实例

容器可以被启动、开始、停止、删除。每个容器都是互相隔离的、保证安全的平台。

`可以把容器看做是一个简易版的Linux环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序`

容器的定义和镜像一模一样，也是一堆层的统一视角，唯一区别是容器的最上面那一层是可读可写的。

仓库是集中存放镜像文件的地方。

仓库repository和仓库注册服务器registry是有区别的。仓库注册服务器往往存放着多个仓库，每个仓库又包含了多个镜像，每个镜像有不同的标签tag。

仓库分为公开仓库public和私有仓库private。

最大的公开仓库是Docker Hub[https://hub.docker.com/]

```
需要正确的理解仓库/镜像/容器这三个概念：
Docker本身是一个容器运行载体，或称之为管理引擎。
我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就是image镜像文件。只有通过这个镜像文件才能生存Docker容器。image文件可以看做是容器的模板，docker根据image文件生成容器的实例。同一个image文件，可以生成多个同时运行的实例。

image文件生成的容器实例，本身也是一个文件，成为镜像文件

一个容器运行一种服务，当我们需要的时候，就可以通过docker客户端创建一个对应的运行实例，也就是我们的容器。

至于仓库，就是存放了一堆镜像的地方，可以把镜像发布到仓库中，需要的时候就从仓库上拉下来就行。
```



### 安装Docker

#### CentOS 6.8

```
1. yum install -y epel-release
2. yum install -y docker-io
3. 安装后的配置文件  /etc/sysconfig/docker
4. 启动Docker后台服务  service docker start
5. docker version 验证
```

**配置阿里云镜像加速**

* 在阿里云拥有账号，进去容器镜像服务查看自己的专属加速器地址
* 在/etc/sysconfig/docker中，使用`other_args="--registry-mirror=专属加速器地址"`
* 重新启动docker后台服务 service docker restart
* 检查是否生效 `ps -ef|grep docker`



#### CentOS 7

```
1. sudo yum install -y yum-utils device-mapper-persident-data lvm2
2. sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
// 推荐使用阿里云镜像
   sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
3. sudo yum-config-manager --enable docker-ce-edge
   sudo yum-config-manager --enable docker-ce-test
4. sudo yum makecache fast
5. sudo yum -y install docker-ce
6. systemctl start docker
```

**配置镜像加速**

```
1. mkdir -p /etc/docker/
2. vim /etc/docker/daemon.json
	{
		"registry-mirrors": ["专属加速器地址"]
	}
3. systemctl daemon-reload
4. systemctl restart docker
```

**卸载**

```
1. systemctl stop docker
2. yum -y remove docker-ce
3. rm -rf /var/lib/docker
```



#### Ubuntu 

```
// -- Set up repository
// update the apt package index and install packages to allow apt to use // a repository over HTTPS
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
// 添加Docker官方GPG Key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg \
--dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
// 设置docker稳定版本库
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

// -- Install Docker engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

// 启动docker（启动时最好是以root权限启动）
systemctl start docker
docker version
```



### docker run hello-world

测试是否安装docker成功

![](C:\zhan\Note\容器\docker-pic\2.PNG)

查找image与maven查找dependency类似



### 底层原理

#### docker是怎么工作的

Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上，然后通过Socket连接从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。



#### 为什么docker比VM快

1. docker有着比VM更少的抽象层。由于docker不需要Hypervisor实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此，在CPU、内存利用率上docker将会在效率上有明显优势
2. docker利用的是宿主机的内核，而不需要Guest OS。因此，当新建一个容器的时候，docker不需要和VM一样重新加载一个OS内核，进而避免引寻、加载OS内核这个比较费时费资源的过程。当新建一个虚拟机时，虚拟机软件需要加载Guest OS，这个新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统，省略了这个过程，因此新建docker容器是秒级的。

|            | Docker容器              | VM                          |
| ---------- | ----------------------- | --------------------------- |
| 操作系统   | 与宿主机共享OS          | 宿主机上运行虚拟机OS        |
| 存储大小   | 镜像小，便于存储和传输  | 镜像庞大                    |
| 运行性能   | 几乎无额外性能损失      | 操作系统额外的CPU、内存消耗 |
| 移植性     | 轻便、灵活，适应于Linux | 笨重，与虚拟化技术耦合度高  |
| 硬件亲和性 | 面向软件开发者          | 面向硬件运维者              |
| 部署速度   | 快速，秒级              | 较慢，10s以上               |



### 帮助命令

#### docker version

查看docker版本等信息

#### docker info

查看docker的相关信息

#### docker --help

查看docker命令的相关帮助信息



### 镜像命令

#### docker images

列出本地主机上的镜像

-a  显示本地所有的镜像

-q  只显示镜像ID

--digests  显示镜像的摘要信息

--no-trunc  显示完整的镜像信息



#### docker search [镜像名]

在docker hub上查找带有指定镜像名的镜像

`docker search -f stars=30 tomcat`  只显示stars数大于30的tomcat镜像

--no-trunc  显示完整的镜像信息

`docker search -f is-automated=true tomcat`  只显示automated build类型的镜像



#### docker pull [镜像名:tag]

拉取一个镜像，如果不加tag，默认是latest



#### docker rmi [镜像名:tag 或者是 镜像ID]

-f  强制删除

删除多个  `docker rmi -f 镜像名1:tag 镜像名2:tag ...`

删除全部  `docker rmi -f $(docker images -qa)`



#### docker history 镜像名

查看镜像的变更历史



### 容器命令

#### docker run [options] 镜像名/镜像ID [command] [args]

新建并启动容器

options

​	`--name "容器名称"`  为容器指定一个名称，不指定则docker随机分配

​	-d  后台运行容器，并返回容器ID，即守护式启动容器

​	-i  以交互模式运行容器，通常与-t一起使用

​	-t  为容器重新分配一个伪输入终端，通常与-i一起使用

​	-P  随机端口映射

​	-p  指定端口映射，有以下四种格式

​		ip:hostPort:containerPort

​		ip::containerPort

​		**hostPort:containerPort**

​		containerPort

​	`sudo docker run -it -p 8888:8080 tomcat`  启动tomcat，宿主机端口8888，容器端口8080



#### docker ps [options]

列出当前所有正在运行的容器

options

​	-a  列出所有当前运行的+历史运行的容器

​	-l  显示最近创建的容器

​	-n 3  显示最近3个创建的容器

​	-q  静默模式，只显示容器编号

​	--no-trunc  显示完整信息



#### 退出容器

exit  容器停止并退出

Ctrl + P + Q  容器不停止退出



#### docker start 镜像名/镜像ID

启动已经关闭的容器



#### docker restart 镜像名/镜像ID

重启容器



#### docker stop 镜像名/镜像ID

停止容器



#### docker kill 镜像名/镜像ID

强制停止容器



#### docker rm 容器ID

删除容器

-f  强制关闭并删除

`docker rm -f $(docker ps -aq)`  删除所有（包括历史运行的）容器

`docker ps -a -q | xargs docker rm`  删除所有（包括历史运行的）容器



#### docker logs [options] 容器ID

查看容器的日志

options

​	-t  添加时间戳

​	-f  跟随最新的日志打印

​	--tail 5  显示最后5条记录



#### docker top 容器名/容器ID

查看容器内的进程



#### docker inspect 容器ID

以json字符串的形式返回容器的内部细节



#### docker attach 容器ID

重新进入容器，并启动命令终端



#### docker exec [options] 容器ID bashshell命令

进入容器，执行bash shell命令，并返回

`docker exec -it 容器ID /bin/bash`   重新进入容器，并执行bashshell



#### docker cp 容器ID:容器内路径 宿主机路径

将容器内的文件发送至宿主机中

宿主机路径在前就是把文件发送到容器内



### 镜像

#### 镜像是什么

> 镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。



#### UnionFS（联合文件系统）

Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层一层叠加，同时可以将不同目录挂载到同一个虚拟文件系统（unite several directories into a single virtual filesystem）.

UnionFS是Docker镜像的基础。镜像通过分层来继承，基于基础镜像（即没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。



#### Docker镜像加载原理

docker的镜像由一层一层的文件系统组成，这种层级的文件系统叫UnionFS。

bootfs（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含bootloader和kernel。当boot加载完成之后，整个内核就都在内存中了，此时内存的使用权已由bootfs转交给kernel，此时系统也会卸载kernel。

rootfs（root file system），在bootfs之上，包含的是典型的Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu和CentOS等。

> Q: 为什么linux的docker镜像都这么小？

对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令、工具和程序库。底层直接使用Host的Kernel，自己只需要提供rootfs就行了。

由此可见对于不同的Linux的发行版，bootfs基本是一致的，而rootfs会有差别。

> Q: 为什么Docker镜像要采用这种分层结构呢？

最大的一个好处就是 - 共享资源

比如：有多个镜像都从相同的base镜像构建而来，name宿主机只需要在磁盘保存一份base镜像。同时内存中也只需要加载一份base镜像，就可以为所有容器服务了，而且镜像的每一层都可以被共享。



#### 特点

Docker镜像都是只读的

当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称为“容器层”，“容器层”之下的被称为“镜像层”。



### docker commit

提交容器的副本使其成为新的镜像，保存在local repo

`docker commit -m="提交的描述信息" -a="作者名" 容器ID 创建的镜像名:[标签名] `

`docker commit -a="bzhan" -m="tomcat with no docs" 1031305e4ad1 nodocs\tomcat:1.1`



### 容器数据卷

> 用途：数据共享，容器持久化

#### 什么是容器数据卷

卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于UnionFS，因此能够绕过UnionFS提供一些用于持续存储或共享数据的特性。

卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此docker不会在容器删除时，删除其挂载的数据卷

特点：

* 数据卷可在容器之间共享或重用数据
* 卷中的更改直接生效
* 数据卷中的更改不会包含在镜像的更新中
* 数据卷的生命周期一直持续到没有容器使用它为止



#### 用命令添加数据卷

docker run -it -v 宿主机绝对路径目录:容器绝对路径目录 镜像名/镜像ID

e.g. `sudo docker run -it -v /home/bzhan/mydatavolumn:/containerdatavolumn centos`

自动在宿主机和容器内创建文件夹作为数据卷，两者的数据共享，默认是读写权限



docker inspect 镜像名/镜像ID  通过json字符串中的volumns查看数据卷是否挂载成功（Ubuntu查看key为Mounts）



docker  run -it -v 宿主机绝对路径目录:容器绝对路径目录:ro 镜像名/镜像ID

容器只读权限，添加数据卷



#### 用dockerfile添加数据卷

在dockerfile中使用**Volume** 语句来指定容器中多个数据卷

```dockerfile
FROM centos
VOLUME ["容器数据卷绝对路径1", "容器数据卷绝对路径2", ...]
CMD echo "finished, --success1"
CMD /bin/bash
```

> 说明：处于可移植性和安全性的考虑，用 `-v 主机目录:容器目录`这种方法不能够直接在Dockerfile中实现。由于宿主机目录是依赖于特定宿主机的，并不能够保证在所有的宿主机上都存着这样的特定目录。



##### docker build

`docker build -t 镜像名:tag dockerfile目录路径`

-f   Dockerfile的完整路径

​	`docker build -f /root/dockerfile -t mycentos:1.0 .`

--rm  删除中间环节的容器

--privileged=true  添加权限

`docker build -t mycentos .`  加载并build当前目录下的dockerfile



#### --volumes-from

命名的容器挂载数据卷，其他容器通过挂载这个父容器实现数据共享，挂载数据卷的容器称之为数据卷容器

本质上是不同容器共享宿主机上的同一个文件夹

```
sudo docker run -it --name dc01 centos
sudo docker run -it --name dc02 --volumes-from dc02 centos
```

容器之间的数据卷的共享，它的生命周期一直持续到没有容器使用它为止



## DockerFile

### 什么是DockerFile

用来构建image的文件



### DockerFile构建解析

* 每条保留字指令必须是大写字母且后面要跟随至少一个参数
* 指令按照从上到下，顺序执行
* #表示注释
* 每条指令都会创建一个新的镜像层，并对镜像进行提交



### DockerFile的执行流程

1. docker从基础镜像运行一个容器
2. 执行一条指令对容器做出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行下一个新的容器
5. 执行dockerfile中下一条指令直到所有指令都执行完成



### DockerFile，镜像与容器

从应用软件的角度来看，DockerFile、Docker镜像与Docker容器分别代表软件的三个阶段

* DockerFile是软件的原材料
* Docker镜像是软件的交付品
* Docker容器则是软件的运行态

DockerFile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维。

DockerFile：定义了进程需要的一切东西，涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程（当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制）等等

Docker镜像：在用dockerfile定义一个文件，docker build时会产生一个Docker镜像，当运行Docker镜像时，会真正开始提供服务

Docker容器：容器是直接提供服务的



### 保留字指令

#### from 镜像名

基础镜像，当前新镜像是基于哪个镜像的



#### maintainer 作者名  邮箱



#### run linux命令

容器构建时需要运行的命令



#### expose 端口号

当前容器对外暴露的端口



#### workdir 目录地址

指定在创建容器后，终端默认登录的进来的工作目录



#### env 环境变量名 值

用来在构建过程中设置环境变量

这个环境变量可以在之后的run命令中使用，也可以在其他命令中直接使用该环境变量



#### Add 文件

将宿主机目录下的文件拷贝进镜像并且自动处理URL和tar压缩包

拷贝+解压缩



#### copy 文件

拷贝文件

`COPY src dest`  或者  `COPY ["src", "dest"]`



#### volume ["多个数据卷地址"]

容器数据卷，用于数据保存和持久化

`VOLUME ["容器数据卷绝对路径1", "容器数据卷绝对路径2", ...]`



#### cmd 

指定一个容器启动时，要运行的命令

shell格式 `CMD 命令`

exec格式  `CMD ["可执行文件", "参数1", "参数2" ...]`

**注意**：dockerfile可以有多个cmd命令，但是只有最后一个生效，并且会被`docker run`之后的参数覆盖掉



#### entrypoint

指定一个容器启动时，要运行的命令

不会被docker run之后的参数覆盖掉，而是进行追加，变成新的命令组合



#### onbuild

当构建一个被继承的dockerfile时，父镜像在被子镜像继承后父镜像的onbuild会触发 



### 案例1

> centos镜像的默认工作目录是/,并且vim和ifconfig命令都无法使用

```dockerfile
FROM centos
MAINTAINER bzhan <benson.zhan@infor.com>

# 设置环境变量
ENV Work_Path /tmp
WORKDIR $Work_Path

# 安装命令
RUN yum -y install vim
# ifconfig
RUN yum -y install net-tools

# 容器的端口是7777
EXPOSE 7777

CMD /bin/bash
```



### 案例2

> 定制一个自己的tomcat

dockerfile

```dockerfile
FROM centos
MAINTAINER bzhan <benson.zhan@infor.com>

COPY host.txt /usr/local/host_in_container.txt
# add jdk and apache-tomcat, and unzip
ADD jdk-8u301-linux-x64.tar.gz /usr/local
ADD apache-tomcat-9.0.54.tar.gz /usr/local

# install vim
RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

# configure environment variables
ENV JAVA_HOME /usr/local/jdk1.8.0_301
ENV CLASS_PATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.54
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.54
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.54/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.54/bin/logs/catalina.ou
```

build

```docker
docker build -t mytomcat:9.0.54 .
```

run

```docker
run -d -p 7777:8080 --name mytomcat9 -v /home/bzhan/mydocker/mytomcat/test:/usr/local/apache-tomcat-9.0.54/webapps/test -v /home/bzhan/mydocker/mytomcat/tomcat9logs:/usr/local/apache-tomcat-9.0.54/logs --privileged=true mytomcat:9.0.54
```



## 镜像推送到阿里云

1. 创建`bzhan/mytomcat`仓库镜像

   1. 在阿里云上创建

2. 将镜像推送到registry

   1. 登录阿里云Docker Registry

   ```docker
   docker login --username=benson**** registry.cn-hangzhou.aliyuncs.com
   ```

   2. 将镜像推送到Registry

   ```docker
   # tag 用来重新生成一个镜像并给它重新命名
   docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/bzhan/mytomcat:[镜像版本号]
   docker push registry.cn-hangzhou.aliyuncs.com/bzhan/mytomcat:[镜像版本号]
   ```



拉取：`docker pull registry.cn-hangzhou.aliyuncs.com/bzhan/mytomcat:[镜像版本号]`



## docker创建Jenkins

1. 拉取jenkins

```shell
sudo docker pull jenkins/jenkins:lts
```

2. 配置宿主机容器卷目录

```shell
sudo mkdir /home/jenkins_home
sudo chown -R 1000:1000 /home/jenkins_home
```

3. 启动jenkins

```shell
sudo docker run -it --name jenkins -p 8888:8080 -p 50000:50000 \
-v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

4. 查看root用户的初始密码

```shell
sudo cat /home/jenkins_home/secrets/initialAdminPassword
```

