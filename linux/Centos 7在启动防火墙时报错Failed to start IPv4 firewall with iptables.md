今天在虚拟机的Linux系统（centos7）里安装Redis，准备学习一下布隆过滤器呢，安装完后使用Windows本机访问，Telnet不通能够ping通。于是就去看防火墙，是否关闭或是否把6379端口放开了。

于是就往这方面查问题，发现没有iptables文件，然后我启动iptables服务，报错。

`Centos 7`在启动iptables（防火墙）时报错：
`Failed to start IPv4 firewall with iptables.`

原因：因为`centos7.0`默认不是使用`iptables`方式管理，而是`firewalld`方式。`Centos6.0`防火墙用`iptables`管理。（原来是这样，centos7默认防火墙时firewalld啊[笑哭]。:-D）

解决办法有两个：一是继续使用默认的`firewalld`方式。二是关闭`firewalld`，然后安装`iptables`。以前都是用iptables，所以想换回来，于是找到如下切换教程。

## 从firewalld切换到iptables：关闭firewalld安装iptables
1、首先执行如下命令
```
#关闭
systemctl stop firewalld
systemctl mask firewalld
```
2、然后安装`iptables-services`

```
#安装
yum install iptables-services
#设置开机启动
systemctl enable iptables
```
3、开放443端口(HTTPS)

`iptables -A INPUT -p tcp --dport 443 -j ACCEPT`

4、保存防火墙配置
```
service iptables save
#或者
/usr/libexec/iptables/iptables.init save
```

5、iptables的一些命令，停止/启动/重启 防火墙：

```
systemctl [stop|start|restart] iptables
#或者
service iptables [stop|start|restart]
```
然后启动iptables服务，这样就搞定了。

但是，从firewalld切换到iptables后会有这样那样的问题，还不如用系统默认的。

## 从iptables切换回firewalld
1、先看firewalld的状态：inactive
`systemctl status firewalld`

2、安装firewalld
`yum install firewalld`

3、切换
```
#关闭iptables
systemctl mask iptables
systemctl stop iptables
#切换
systemctl unmask friewalld
systemctl start friewalld
```
总算恢复了。

饶了一大圈，最后找到这篇文章
[windows本地连不上虚拟机redis服务完美解决](https://www.cnblogs.com/gara/p/9524014.html)，解决了这个问题。
## 附：firewalld相关命令
### 常用命令

```
#查看状态，是否已经安装firewalld
systemctl status firewalld
#开启防火墙
systemctl startfirewalld.service
#关闭防火墙
systemctl stop firewalld.service
#设置开机自动启动
systemctl enable firewalld.servic
#设置关闭开机制动启动
systemctl disable firewalld.service
#在不改变状态的条件下重新加载防火墙
firewall-cmd --reload
```

### 启用某个服务

```
#临时
firewall-cmd --zone=public --add-service=https
#永久
firewall-cmd --permanent --zone=public --add-service=https
```

### 开启某个端口

```
#永久
firewall-cmd --permanent --zone=public --add-port=8080-8081/tcp
#临时
firewall-cmd  --zone=public --add-port=8080-8081/tcp
```
### 查看开启的端口和服务
```
#查看开启的服务 空格隔开
firewall-cmd --permanent --zone=public --list-services
#查看开启的端口 空格隔开
firewall-cmd --permanent --zone=public --list-ports
#修改配置后需要重启服务使其生效
systemctl restart firewalld.service
#查看服务是否生效 （例：添加的端口为8080）
firewall-cmd --zone=public --query-port=8080/tcp 
```
如下,查看开启的服务、端口
![](http://q1pqt7tv2.bkt.clouddn.com/FvM1gXdwhlQRlkaq3DB-n8LK35T9)