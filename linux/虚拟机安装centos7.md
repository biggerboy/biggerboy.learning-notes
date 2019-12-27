# 虚拟机安装centos7

## 1、创建虚拟机

打开虚拟机，点击create..

![image-20191211100542265](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211100542265.png)



点击next

![image-20191211100820752](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211100820752.png)

继续next



![image-20191211100750231](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211100750231.png)



继续next

![image-20191211100850103](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211100850103.png)

填个名字，选择存放目录

![image-20191211101044599](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101044599.png)

next

![image-20191211101119809](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101119809.png)

## 2、硬件配置

然后进入这个页面，虚拟机硬件配置，点击自定义

![image-20191211101158082](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101158082.png)

进去默认有一些配置，我改一下

![image-20191211101248984](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101248984.png)

我把内存改为4G，然后选择镜像的位置，其他默认就行

![image-20191211101406851](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101406851.png)

改后的，然后close

![image-20191211101549534](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101549534.png)

点击完成

![image-20191211101506504](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101506504.png)

进入这个画面

![image-20191211101655952](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101655952.png)

## 3、启动虚拟机

点击power on...启动

![image-20191211101728122](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101728122.png)

然后显示这样的，我们用键盘上下键切换，选第一个install centos 7，然后回车。

![image-20191211101845496](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211101845496.png)

然后按照提示继续按回车，会初始化安装，等待初始化完成。

## 4、系统初始化设置

之后进入初始化系统设置阶段

### 4.1、设置系统语言

默认English，点击右下角继续。

![image-20191211102342754](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211102342754.png)

### 4.2、设置系统时间

进入下个页面，选择Date&Time设置系统时间，选择亚洲，上海，时间对照一下修改正确的，点击左上角done

![image-20191211104045931](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211104045931.png)



### 4.3、设置键盘

选择 KEYBOARD 键盘就默认是**English（US）** ，我这里添加了两个中文的

![image-20191211104205872](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211104205872.png)



### 4.4、设置支持的语言

选 LANGUAGE SUPPORT语言支持 ，增加中文

![image-20191211104249145](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211104249145.png)

### 4.5、设置系统分区

INSTALLATION DESTINATION 安装位置---即进行系统分区 

选择如下，点击done

![image-20191211104944240](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211104944240.png)

然后选择如下，然后点击左下角的加号+添加分区

![image-20191211105205141](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211105205141.png)

我们总共20G，/boot分200M放启动文件，swap交换分区一般是内存的2倍，我的内存是4G，所以swap设置8G，余下的全是根分区/的。然后点击左上角done， 点击**Accept Changes** 。

![image-20191211105907490](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211105907490.png)



### 4.5、设置网络连接和主机名 

然后回到刚才的界面，选择 NETWORK & HOST NAME 设置网络连接和主机名 

![image-20191211110716820](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211110716820.png)

###  4.7、开始初始化

此时初始化设置完成，**点击** **Begin Installation** 

![image-20191211110806592](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211110806592.png)

 

### 4.8、设置管理员密码

然后需要设置管理员**Root Password**（**一定要记住密码！**） 

![image-20191211110842112](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211110842112.png)



点击ROOT PASSWORD设置管理员密码，然后可以点击右边的USER CREATION创建用户，也可以不现在创建，安装完成在创建。

## 5、完成安装

然后等待进度条走完，点击右下角reboot就安装成功了。

![image-20191211112456380](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211112456380.png)

root/147258ppzy,

zyp/123456pp,