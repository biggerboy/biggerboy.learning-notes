昨天安装好的MySQL今天开机启动时报错

```bash
12月 12 09:38:36 centos-7 systemd[1]: Unit mysql.service entered failed state.
12月 12 09:38:36 centos-7 systemd[1]: mysql.service failed.
12月 12 09:38:36 centos-7 polkitd[690]: Unregistered Authentication Agent for unix-process:4515:229312 (system bus name :1.87, object path /org/freedesktop/PolicyKit1/AuthenticationAgent
12月 12 09:40:01 centos-7 systemd[1]: Created slice User Slice of root.
-- Subject: Unit user-0.slice has finished start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit user-0.slice has finished starting up.
-- 
-- The start-up result is done.
12月 12 09:40:01 centos-7 systemd[1]: Started Session 5 of user root.
-- Subject: Unit session-5.scope has finished start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit session-5.scope has finished starting up.
-- 
-- The start-up result is done.
12月 12 09:40:01 centos-7 CROND[4749]: (root) CMD (/usr/lib64/sa/sa1 1 1)
12月 12 09:40:01 centos-7 systemd[1]: Removed slice User Slice of root.
-- Subject: Unit user-0.slice has finished shutting down
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit user-0.slice has finished shutting down.
12月 12 09:40:21 centos-7 su[4760]: (to root) zyp on pts/0
12月 12 09:40:21 centos-7 su[4760]: pam_unix(su:session): session opened for user root by zyp(uid=1000)
12月 12 09:40:21 centos-7 dbus[725]: [system] Activating service name='org.freedesktop.problems' (using servicehelper)
12月 12 09:40:21 centos-7 dbus[725]: [system] Successfully activated service 'org.freedesktop.problems'
12月 12 09:44:01 centos-7 polkitd[690]: Registered Authentication Agent for unix-process:4843:262894 (system bus name :1.95 [/usr/bin/pkttyagent --notify-fd 5 --fallback], object path /o
12月 12 09:44:01 centos-7 systemd[1]: Starting LSB: start and stop MySQL...
-- Subject: Unit mysql.service has begun start-up
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit mysql.service has begun starting up.
12月 12 09:44:02 centos-7 mysql[4849]: Starting MySQL. ERROR! The server quit without updating PID file (/usr/local/mysql/data/centos-7.pid).
12月 12 09:44:02 centos-7 systemd[1]: mysql.service: control process exited, code=exited status=1
12月 12 09:44:02 centos-7 systemd[1]: Failed to start LSB: start and stop MySQL.
-- Subject: Unit mysql.service has failed
-- Defined-By: systemd
-- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
-- 
-- Unit mysql.service has failed.
-- 
-- The result is failed.
12月 12 09:44:02 centos-7 systemd[1]: Unit mysql.service entered failed state.
12月 12 09:44:02 centos-7 systemd[1]: mysql.service failed.
12月 12 09:44:02 centos-7 polkitd[690]: Unregistered Authentication Agent for unix-process:4843:262894 (system bus name :1.95, object path /org/freedesktop/PolicyKit1/AuthenticationAgent

```

启动时报错

Starting MySQL. ERROR! The server quit without updating PID file (/usr/local/mysql/data/centos-7.pid).



登录MySQL输入密码后报错

```bash
mysql -u root -p
Enter password: 
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

cd进入tmp目录，ll查看没有mysql.sock文件

vim /etc/my.cnf查看文件内容

发现这个文件路径设置如下

![image-20191212103038821](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191212103038821.png)

cd /var/lib/mysql到这个目录，ll查看确实存在

于是建立一个软连接：`ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock`

再次`mysql -u root -p`输入密码正常进入。



如果 /var/lib/mysql 下没有这个文件（我的就没有这个文件，我昨天安好的数据库正常启动，第二天就不行了），所以如果数据库没有重要文件或者有备份，那就初始化数据库吧。执行

`./mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql`初始化，然后再执行上面添加软连接的命令即可。