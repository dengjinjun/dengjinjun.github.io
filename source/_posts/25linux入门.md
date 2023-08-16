---
title: linux入门
date: 2023-08-15 22:39:36
tags: python
---

## linux入门

- 安装Linux

  ```
  VMware workstation + Linux镜像文件
  ```

  账号和密码：

  ```
  dengpangpang
  Deng@3364546586
  ```

- 第一个命令

  ```
  ifconfig		#查看或配置网络设备
  #  IP地址：192.168.110.129
  ```

  ![image-20230329223140489](assets/image-20230329223140489.png)

- 通过xshell远程控制Linux

  ```
  xshell和secureCRT都是Windows系统下远程连接的工具，封装了ssh命令
  Linux和mac系统在终端就可以直接使用ssh命令
  ssh root@192.168.110.129		#ssh root@ip地址
  ```

  - xshell快捷键

    ```
    ctrl l  		# 清屏
    ctrl d  		# 快速退出登录
    ctrl shift r	# 快速重新连接
    ```

- Linux目录结构

  ![image-20230329222739420](assets/image-20230329222739420.png)

- 命令提示符解释

  ```
  登入Linux之后，会出现命令提示符 [root@localhost ~]#
  root 代表当前系统登录的用户名
  @ 占位符
  localhost 代表当前机器主机名
  ~ 当前所在的路径
  # 超级用户的身份提示符
  $ 普通用户的身份提示符
  ```

- 创建普通用户

  ```
  useradd 用户名		#创建的用户信息会放在/etc/passwd目录下
  passwd 用户名		#给用户修改密码
  ```

- 查看帮助信息

  ```
  man 命令名			# man手册，可以查看当前命令的解释文件
  命令名 --help		# 查看命令提示
  ```

- 打印一些信息

  ```
  echo "任意字符串"
  ```

- 查看进程信息

  ```
  ps -ef
  kill 进程id		# 杀死进程
  ```

- 查看端口信息

  ```
  netstat  -tunlp
  常用端口解释：
  80		# HTTP web服务器端口
  443		# HTTPS 加密的HTTP协议
  3306	# MySQL服务
  8000	# django服务
  22		# ssh协议端口
  8080	# 自定义的端口
  ```

  

## vim文本编辑器

Linux下有两个文本编辑器，vi和vim，vi如同Windows下的记事本，vim是支持编程的编辑器

- `vim 文件名`

  ```
  进入了vim编辑器编辑文本，此时在命令模式，等待输入相关的指令
  ```

- 相关指令

  ```
  输入字母i : 进入编辑模式，i是insert的意思
  esc : 退出编辑模式，回到命令模式
  :q! : 不写入强制退出，!是强制的意思
  :w! : 只保存写入，不退出
  :wq! : 强制保存写入并退出
  ```

## Linux安装软件

Linux安装软件有三种方式：

- `yum`安装

  ```
  比如：yum install 模块名		# yum是Linux自带的工具，最省心的安装方式
  ```

- `rpm`包安装，需要手动解决依赖关系，麻烦

- 源代码编译安装，可以自定义软件版本以及功能扩展。（大多公司选择）

## Linux的特殊符号含义

```
~	# 家目录
-	# 上一次的工作目录
.   # 当前目录
..  # 上一级目录
./  # 当前目录下的某些内容
>	# 重定向覆盖输出符号，相当于python文件操作模式的w模式
>>	# 重定向追加输出符号，相当于python文件操作模式的a模式
<<	# 重定向追加写入符号，配合cat命令和EOF命令使用
|	# 管道符，比如：命令1 | 命令2 ，可以对输出结果执行2次指令，常用于查看Linux进程信息和端口信息
```



## 文件夹常用命令

- `pwd`

  ```
  打印当前工作所在目录(print work directory)
  ```

- `cd  文件目录`

  ```
  更改工作目录(change directory)
  ```

- `mkdir`

  ```
  创建文件夹(make directory)
  ```

  - 递归创建文件夹

    ```
    mkdir -p /tmp/信管191/{男同学/邓金俊, 女同学}
    # -p是递归的意思，创建的文件夹如下：
    --tmp
    	--信管191
    		--男同学
    			--邓金俊
    		--女同学
    ```

    

- `mv`

  - `mv 原文件夹名称  新文件夹名称`

  ```
  重命名文件夹
  ```

  - `mv 原文件夹路径  新文件夹路径`

  ```
  移动文件夹
  ```

- `ls 文件夹路径`

  ```
  查看文件夹中的文件(list)，只输入ls查看当前文件夹中的文件
  ```

  > Windows的目录具有盘符的概念，往下衍生出多级目录。而Linux的目录分隔符是正斜杠`/`，所有目录都在根目录下，所以Linux的目录以`/`为源头。
  >

  > `ls`命令看不到Linux中的隐藏文件（以`.`开头的文件），要使用`ls  -a`命令
  >
  > `ls -la`以列表形式查看所有文件
  >
  > `ls -l`显示文件的详细信息，等同于`ll`, `ll -h`,显示详细信息和大小单位

- 复制文件和文件夹

  ```
  文件：
  cp 文件名 文件名.bak
  文件夹：
  cp -r 文件夹名 新文件夹名
  ```

- 删除文件和文件夹

  ```
  rm -i 文件名		# 需要确认删除
  rm -f 文件名		# 强制删除
  rm -r 文件夹名		# 递归删除目录和内容
  ```

  - 别名

    ```
    alias 命令="其他命令"		# 给一些危险的命令如rm，创建一个其他命令
    unalias 命令				# 取消别名
    ```


## 文件常用命令

- `touch 文件名`

  ```
  新建文件
  ```


- `cat 文件名`

  ```
  查看文件（纯文本文件）
  ```

  - `cat -n 文件名`

    ```
    查看文件，显示行号
    ```

  - `more 文件名`

    ```
    查看很大的文本文件，因为cat是一次性读取，比较耗费内存，适用于小文本，more是逐页读取
    ```

- `find 搜索范围 -name 文件名`

  ```
  搜索文件
  find / -name 文件名		# 全局搜索
  find /目录名 -name 文件名		# 局部搜索
  find / -name "*.txt"		# 寻找根目录下所有txt后缀的文件
  搜索文件夹
  find / -type d -name python*		
  # -type 代表指定文件类型，d是文件夹类型，f是文件类型 python*代表python相关的文件夹
  ```

- `grep 参数 要过滤的字符串 要操作的文件名`

  ```
  grep -i "all" settings.py	   # 在settings.py文件中查看含有all字符串的行，-i代表不区分大小写
  ```

  ```
  grep -v "^#" settings.py 	# 在settings.py文件过滤到注释行，-v代表反转搜索结果
  ```

- 从文件的头尾开始查看

  ```
  head -5 文件名		# 查看文件的前5行
  tail -5 文件名		# 查看文件的后5行
  tail -f 文件名		# 实时监控文件信息
  ```

## 其他命令

- 远程传输命令

  ```
  scp命令，可以在两台Linux机器之间传输文件,如果传输文件夹需要递归，加上-r参数
  ```

  ```
  scp 你想传输的文件路径 你想传到的地方		# 将本机的文件传输到另一台Linux机器上
  scp hello.py root@192.168.16.42:/tmp/	#将本机的hello.py文件传输到另一台机器的/tmp/文件夹下
  ```

  ```
  scp 你想从哪里拿到文件 存放到本地的路径		# 从别人的Linux机器上获取文件
  scp  root@192.168.16.42:/tmp/hello.py  /tmp/		
  # 在别人的机器上的/tmp文件夹下的hello.py文件获取到本机的/tmp文件夹下
  ```

- 查看文件和文件夹合计大小

  ```
  du 参数 文件名或目录
  参数包括：-h 显示大小单位	-s 显示总计
  ```

- 任务管理器

  ```
  top			# 用户动态地监视进程信息和系统负载信息
  q			# 退出
  ```

- 给文件（夹）加锁

  ```
  chattr +a 文件名		# 给文件加锁
  lsattr 文件名			# 查看文件锁状态
  ```

- 从网站上下载内容

  ```
  wget url
  ```

- Linux和Windows互相传输文件

  ```
  yum install lrzsz
  rz		# 接收文件
  sz		# 发送文件
  ```

- Linux的网络防火墙

  ```
  iptables -F		# 清空防火墙规则
  systemctl stop firewalld.service		# 关闭防火墙服务
  systemctl disable firewalld.service		# 禁止防火墙开机自启
  ```
  
  

## 网络命令

- 启停网卡的快捷指令

  ```
  ifup
  ifdown
  ```

- 查看端口

  ```
  netstat -tunlp
  ```
