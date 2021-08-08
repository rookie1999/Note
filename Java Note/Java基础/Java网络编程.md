# Java网络编程
## 1.网络通信的三要素
* IP地址：网络设备标识
* 端口号：数据要发送到对方指定的应用程序上。为了标识这些应用程序，所以给这些网络应用程序都用数字进行标识。范围是0-65535，并且0-1024通常被系统进程使用或作为保留端口
* 协议：对等实体层之间的数据交换的规则的集合
## 2.Java网络编程的API
### 2.1 InetAddress
* 此类表示IP地址，并且其实现子类为`Inet4Address`和`Inet6Address`
* 由于此类没有构造方法，并且包含非静态方法，则其中含有静态方法可以返回本类对象
* static InetAddress getLocalHost(): 返回本地主机的地址
* static InetAddress getByName(String host): 根据主机名获取IP地址
* static InetAddress[] getAllByName(String host): 根据主机名获取所有的IP地址（此时该主机可以对应多个IP地址）
* String getHostName() :获取主机名
* String getHostAddress() :获取IP地址字符串