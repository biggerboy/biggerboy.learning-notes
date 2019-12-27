

# Linux之Redis

## linux安装redis

选择在Linux下安装redis，现在采用虚拟机安装的centos7 进行安装的

### 1.安装gcc 

redis是c语言编写的，需要用gcc编译，若已安装请忽略。

`yum install gcc-c++`

### 2.下载redis安装包

下载redis安装包，执行

  `wget http://download.redis.io/releases/redis-5.0.4.tar.gz`

### 3.解压redis安装包

 ` tar -zxvf redis-5.0.4.tar.gz`

### 4.进入redis目录

 ` cd redis-5.0.4`

### 5.编译

  `make`

报错：

![image-20191211160555693](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211160555693.png)

解决方案：
执行命令：make MALLOC=libc 

### 6.安装

 ` make PREFIX=/usr/local/redis install`

### 7.配置

拷贝redis.conf到安装目录

`cp redis.conf /usr/local/redis`

进入 /usr/local/redis目录

 `cd /usr/local/redis/`

然后编辑redis.conf  

 `vim redis.conf`

```bash
#后台启动
daemonize yes
#绑定端口，默认是6379 需要安全组开放端口
port 6379 
#绑定IP
bind 192.168.2.128
#指定数据存放路径，rdb存放的路径
dir /usr/local/redis/log
#指定持久化方式
appendonly yes
# 设置密码
requirepass redis129
```



### 7.后端启动redis

  `./bin/redis-server ./redis.conf`

### 8.查看是否启动成功

 `ps aux | grep redis`

### 9.进入客户端

 处理中文乱码问题

 ` ./bin/redis-cli --raw `

### 10.关闭redis进程

` ./bin/redis-cli shutdown`

或`kill -9 pid 进程`

## Windows无法连接虚拟机的redis

 https://www.cnblogs.com/gara/p/9524014.html 

### 1.检查网络是否通

本机与虚拟机是否可以互相ping通，如本机IP：192.168.22.111 虚拟机IP：192.168.44.129 (设置虚拟机静态IP已设置)

- 本机 win+R 输入cmd 进入dos 输入 ping 192.168.44.129 ，查看数据输送情况
- 虚拟机： ping 192.168.22.111 查看数据输送情况

### 2.检查端口是否开放

查看6379（或者自定义redis端口）时候打开

- firewall-cmd --query-port=6379/tcp 如果返回no则端口没有开启
- firewall-cmd --add-port=6379/tcp （加 --permanent 永久有效），返回success说明开启成功

### 3.检查虚拟机防火墙设置

第2步无误之后，检查虚拟机防火墙设置

- 关闭防火墙两种方式

  - iptables 形式防火墙关闭
    - service iptables stop
    - chkconfig iptables off 永久关闭
  - firewalld 形式防火墙关闭
    - systemctl stop firewalld && systemctl disable firewalld 
    - chkconfig firewalld off 永久关闭

- 检查防火墙状态(是否

  dead

  状态)

  - systemctl status iptables
  - systemctl status firewalld
  - ![img](https://images2018.cnblogs.com/blog/863179/201808/863179-20180827150501236-918541377.png)
    　　

### 4.修改配置文件

第3步无法解决问题的话，就出绝招了——修改redis.conf文件

- `ifconfig `查看虚拟机分配的IP
- 修改redis.conf中bind指定虚拟机IP
  - \# bind多个IP，用空格隔开
  - bind 192.168.43.175 172.203.144.74 127.0.0.1
  - protected-mode yes 改为 protected-mode no
  - daemonized no 改为 daemonized yes 以后台静默进程启动（可选）
- 重启reidis 服务

　　

### 5.测试

 在本机端用Redis Desktop Manager 测试，出现如下惊喜界面，大功告成！

　　![img](https://images2018.cnblogs.com/blog/863179/201808/863179-20180823151859671-756023582.png)