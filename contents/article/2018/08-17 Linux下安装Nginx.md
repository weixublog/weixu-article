# Linux下安装Nginx

标签（空格分隔）： Linux Nginx

---

以下步骤详细说明如何在Linux下安装Nginx

首先在一个Linux的系统上装上基本的软件，包括`gcc`，`openssl`，`wget`，`vim`

```
yum install -y gcc-c++
yum install -y openssl openssl-devel
yum install -y wget
yum install -y vim
```

**安装pcre**
`Nginx`的`http`模块需要使用`pcre`来解析正则表达式

- 方式一 - 直接使用 yum 安装
```
yum -y install pcre pcre-devel
```

- 方式二 - 从官方下载压缩包进行安装
```
wget http://downloads.sourceforge.net/project/pcre/pcre/8.42/pcre-8.42.tar.gz
tar zxvf pcre-8.42.tar.gz
cd pcre-8.42
./configure
make && make install
```

安装完成之后，我们使用如下命令查看安装的`pcre`的版本
```
pcre-config --version
```

**安装Nginx**
由于`yum`库中没有`nginx`，所以需要我们自行下载安装，选择一个文件夹，执行如下指令
```
wget -c https://nginx.org/download/nginx-1.13.10.tar.gz
tar -zxvf nginx-1.13.10.tar.gz
cd nginx-1.13.10
./configure
make && make install
```
一般情况下`nginx`会安装在`/usr/local/nginx/`目录中，如果不是，可以使用如下指令查找
```
whereis nginx
```
安装完成之后，我们使用如下命令查看安装的`nginx`的版本
```
cd /usr/local/nginx/sbin
./nginx -v
```

对于`Nginx`的常用命令如下

> `./nginx` 启动nginx
>`./nginx -s stop` 停止nginx,此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。
>`./nginx -s quit` 停止nginx，此方式停止步骤是待nginx进程处理任务完毕进行停止。
>`./nginx -s reload` 重启nginx，一般是重新载入配置文件时使用
>`./nginx -s reopen` 重启nginx，重新打开日志文件
>`./nginx -v` 查看nginx版本
>`./nginx -t` 查看配置文件正确性

接下来我们启动`nginx`来查看是否可行
```
cd /usr/local/nginx/sbin
./nginx
```

`nginx`默认的端口是`80`端口，我们这个时候需要将CentOS7的`80`端口打开，CentOS7使用firewalld打开关闭防火墙与端口，打开端口命令如下
```
firewall-cmd --zone=public --add-port=80/tcp --permanent 
firewall-cmd --reload
firewall-cmd --zone=public --list-ports
```
如果第三个命令执行后显示的list中有80端口，则说明80端口开放成功，我们就可以在浏览器上访问nginx了。

对于`firewalld`的常用指令如下

> 查看所有打开的端口： `firewall-cmd --zone=public --list-ports`
> 添加一个端口： `firewall-cmd --zone=public --add-port=80/tcp --permanent`（`--permanent`表示永久生效，没有此参数重启后失效）
> 重新载入： `firewall-cmd --reload`
> 删除一个端口：`firewall-cmd --zone= public --remove-port=80/tcp --permanent`

以上我们就完成了`Nginx`的安装




