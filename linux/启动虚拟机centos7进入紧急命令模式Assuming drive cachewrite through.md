# 启动虚拟机centos7进入紧急命令模式Assuming drive cache:write through

启动centos7后进入如下页面，提示如下图

![image-20191211093130275](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211093130275.png)

```bash
Assuming drive cache:write through

Generating "/run/initramfs/rdsosreport.txt"

Entering emergency mode. Exit the shell to continue.

Type "journalctl" to view system logs.

You might want to save "/run/initramfs/rdsosreport.txt" to a USB stick or /boot after mounting them and attach it to a bug report.
```

键入`journalctl`

![image-20191211094754624](C:\Users\zyp\AppData\Roaming\Typora\typora-user-images\image-20191211094754624.png)

Failed to switch root:Specified switch root path /sysroot does not seem to be an OS tree.os-release fil



百度了一圈最后没解决，只能删了系统重新安装了。