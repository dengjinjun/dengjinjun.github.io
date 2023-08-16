---
title: Linux安全防护与破解
date: 2023-08-15 22:39:36
tags: Linux
---

# Linux安全防护与破解

## 操作系统启动流程

- 加电
- 启动`BIOS`，检测启动项目
- 读取启动盘的第一个扇区，大小：521Bytes。包含`MBR`主引导记录（446引导程序`bootloader`->`grub`    64分区信息    2结束标记）
- `grub`启动
- 加载内核，读取运行级别，`/etc/systemd/system/default.target`
  - `0`  => 关机
  - `1`  => 单用户模式，不需要账号密码进入管理员账号，用于破解管理员密码
  - `2`  => 多用户模式，没有网络
  - `3`  => 多用户模式，有网络
  - `4` => 系统未使用，保留
  - `5`  => 图形界面模式，有网络
  - `6`  => 重启

- 启动第一个进程：`systemd`,`pid=0`
- 启动其他服务

![image-20230808001401565](C:\Users\dengp\AppData\Roaming\Typora\typora-user-images\image-20230808001401565.png)

#### 关闭Linux防火墙

```
systemctl stop firewalld		--关闭本次防火墙
systemctl disable firewalld		--永久关闭防火墙功能
```



#### 关闭SELinux

```
setenforce 0					--本次关闭selinux(将selinux设置为Permissive状态，会告警但不会拦截)
```

```
vim /etc/sysconfig/selinux		--修改配置文件
SELINUX=enforcing ==> SELINUX=disabled	修改此行
重启机器
```

```
getenforce						--查看selinux状态
```



#### 进入单用户模式

1、开机时进入如下界面，（按下方向键盘，阻止系统自动继续）



![img](https://egonlin.com/wp-content/uploads/2021/07/059.jpg)



按e键出现下面界面



![img](https://egonlin.com/wp-content/uploads/2021/07/060.jpg)



按方向键下，定位到最后，找到“ro”一行，ro的意思是read only，将“ro”替换成 rw init=/sysroot/bin/sh，如下图



![img](https://egonlin.com/wp-content/uploads/2021/07/061.jpg)



2、按Ctrl-x 进入单用户模式

3、执行chroot /sysroot。其中chroot命令用来切换系统，/sysroot/目录就是原始系统

4、如果要修改root密码

passwd是修改root密码的命令,touch /.autorelabel 执行这行命令作用是让SELinux生效（或者干脆关闭SELinux）
如果不行，密码不会生效。按Ctrl+D，执行reboot重启生效。如下图



![img](https://egonlin.com/wp-content/uploads/2021/07/062.jpg)



 5、如果因为启用x-window或者显卡驱动更新，无法进入桌面，可以修改默认启动级别（开机进入命令行模式）

```
systemctl set-default multi-user.target  #设置成命令模式
init 3 # 切换到字符模式，有时只使用上面的语句没有效果
按下Ctrl+D后，执行reboot
```



### grub加密

```
grub2-setpassword				--给grub设置密码
```

```
vim /boot/grub2/grub.cfg		--修改配置文件
找到并删除 --unrestricted
重启，进入grub菜单时，会要求输入grub密码
```



### 光盘修复模式

```python
#1、进入bios、从光盘启动
#2、点击Troubleshooting
#3、进入到Troubleshooting界面
选择：Rescue a CentOS Linux system
 
#4、三:进入到Rescue选项   按 ENTER键 选1 ，其他选项意思如下
1)continue:救援模式程序会自动查找系统中已有的文件系统，并可读写挂载到/mnt/sysimage目录下。
2)Read-Only:会以只读的方式挂载已有的文件系统。
3)Skip to shell: 手动挂载
 
#5、sh切换bash模式
chroot /mnt/sysimage/
 
#6、执行命令
passwd root

#7、退出并重启
exit
reboot
```



#### 设置BIOS密码

进入BIOS、进入security选项、选择Supervisor Password，输入密码。



