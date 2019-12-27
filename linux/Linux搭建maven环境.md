## 1、配置maven环境

需要事先安装JDK

### 1.1、下载maven

`wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz`

### 1.2、解压

`tar vxf apache-maven-3.5.4-bin.tar.gz`

`mv apache-maven-3.5.4 /usr/local/maven-3.5.4`

### 1.3、环境变量

`vim /etc/profile`

添加环境变量

```shell
MAVEN_HOME=/usr/local/maven-3.5.4`

export MAVEN_HOME

export PATH=${PATH}:${MAVEN_HOME}/bin
```

保存退出执行`source /etc/profile`让环境变量生效

然后执行`mvn -v`看是否生效。如下表示已配置成功。

![image-20191212141016680](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191212141016680.png)

## 2、配置maven仓库

### 2.1 创建本地仓库目录

`mkdir /usr/local/maven_repository`

### 2.2 打开settings.xml文件

`vim /usr/local/maven-3.5.4/conf/settings.xml`

### 2.3 添加本地仓库地址

`<localRepository>/usr/local/maven_repository</localRepository>`