# centos7输入init5无法进入图形界面

正常登陆后，输入`init 5`没有反应，无法进入图形界面

## 启动模式检查

输入`vi /etc/inittab`查看有两种模式

```bash
multi-user.target：analogous to runlevel 3
graphical.target: analogous to runlevel 5
1
2
```

输入`:wq`（注意有个冒号）退出`vi`

退出vi模式后，输入命令`systemctl get-default `查看当前系统启动模式 
`graphical.target`

## 修改启动模式

若当前系统启动模式为`multi-user.target`

输入`systemctl set-default graphical.target `之后再输入`reboot`重启

重启后还是无法进入图形化界面。

输入`yum grouplist` 查看可安装软件列表，若显示There is no installed groups file

## 网络连接设置

是应为网络是不是没有连接好，我是用的wifi连接的

下面是用到的命令:

将无线网口wls1开启 
ip link set wlp4s0 up 
显示无线网口wlp4s0连接情况 
ip link show wlp4s0 
显示分配的ip地址，特别适用于查看是否成功地通过dhcp自动获取了ip地址 
ip addr show wlp4s0 
连接无线网ssid，密码psk 
wpa_supplicant -B -i wlp4s0-c <(wpa_passphrase “ssid” “psk”) 
为wlp8s0自动分配ip地址 
dhclient wlp4s0 
连接好后可通过”ip addr show wlp4s0”查看是否正确分配ip地址,”ping”测试是否正常连接网络

但是重启登录后会发现

1.无线网卡没有启动

2.启动后无法自动连接WiFi

3.使用ip link set xxx up,wpa_xxx启动网卡连接WiFi后重新登陆无法自动启动连接WiFi

解决方案如下:

1.设置NetworkManager自动启动 
chkconfig NetworkManager on 
2.安装NetworkManager-wifi 
yum -y install NetworkManager-wifi 
3.开启WiFi 
nmcli r wifi on 
4.测试（扫描信号） 
nmcli dev wifi 
5.连接 
nmcli dev wifi connect password

网络连接参考:https://blog.mrabit.com/details/25

## 查看可安装软件

接着使用yum grouplist 查看可安装软件 
出现GNOME Desktop软件

## 安装可视化界面

yum groupinstall “GNOME Desktop”进行安装

约下载700MB文件

======================================================= 
重启后设置登陆用户名及密码

若默认为图形界面登陆，重启后输入设置的用户名和密码即可登陆

若默认为字符界面登陆，输入init 5即可切换至图形界面。

完结
\--------------------- 
作者：lcgroger 
来源：CSDN 
原文：https://blog.csdn.net/lcgroger/article/details/82120178 
版权声明：本文为博主原创文章，转载请附上博文链接！