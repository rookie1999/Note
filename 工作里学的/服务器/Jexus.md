## Jexus安装与配置

1. 安装

下载Jexus安装包

```shell
wget http://www.linuxdot.net/down/jexus-5.1.tar.gz
```

解压

```shell
tar -zxvf jexus-5.1.tar.gz
```

配置

```shell
sudo mv jexus-5.1 /usr/jexus
```



2. 配置文件jws.conf

```
SiteLogDir=log    #网站日志以及Jexus系统日志的存放位置，必填项。可以使用基于jws.exe文件的相对路径
SiteConfigDir=siteconf     #网站配置文件存放的位置，是必填项。可以使用绝对路径，也可以使用基于jws.conf文件的相对路径
Runtime=v4.0.30319    #设定Jexus工作进程运行于哪个.NET版本
httpd.processes=1     #工作进程的数量，建议每6-8核CPU用一个进程，最多可设4个进程
httpd.user=www-data     #工作进程以什么用户身份和对应权限工作，默认为root
php-fcgi.set=/usr/bin/php-cgi,6    #如果需要Jexus同时充当PHP FastCGI服务器，这一句就是fast-cgi设置，分两个部分，逗号前为php-cgi这个文件的路径，逗号后是php进程数
CertificateFile=/xxxx/xx.crt    #SSL证书路径（如果需要使用https协议才填）
CertificateKeyFile=/xxxx/xx.key    #SSL密钥文件路径（如果需要使用https协议才填）

注：jws.conf 中,SiteConfigDir 和 SiteLogDir 两项是必填项。
```



3. 每个网站的配置文件

```
port=80                          # jexus WEB服务器侦听端口（必填。当然可以是其它端口）
root=/ /var/www/mysite           # 网站URL根路径（虚拟目录）和对应的物理路径，两个路径字串之间必须用空格分开（必填。既使这个网站是一个纯粹的反向代理站，也得填）

#可选项
hosts=mysite.cn,www.mysite.cn    # 网站域名（建议填写），可以用泛域名，比如：*.mysite.cn（不填此项或只填一个“*”号表示这是默认网站，一个端口只能有一个默认站）
indexs=index.aspx,index.htm      # 首页文件名，可以写多个，用英文逗号分开（可以不填。因为JWS系统含有常用首页名）
aspnet_exts=mspx,ttt             # 添加新出现的或自定义的ASP.NET扩展名（不建议填。多个扩展名用英文逗号分开，不加点号。系统含有常用扩展名）
```

