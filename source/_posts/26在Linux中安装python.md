---
title: 在Linux中安装python
date: 2023-08-15 22:39:36
tags: python
---

## 在Linux中安装python

- wget  https://www.python.org/ftp/python/3.9.0/Python-3.9.0.tgz

  > ​	得到tgz格式的python3.9.0版本压缩包

- 解压缩

  > ​	tar -xf Python-3.9.0.tgz
  >
  > tar是解压和压缩的命令	-x 是解压的参数	-f 是指定文件名

- 解决编译安装所需的软件依赖，就是python环境需要的各种软件包

  ```
  yum install gcc patch libffi-devel python-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel zlib-devel bzip2devel openssl-devel ncurses-devel xz-devel -y
  ```

- 进入python3的源代码目录，开始编译三部曲

  - 释放makefile,释放编译文件

    ```
    cd Python-3.9.0/
    ./configure  --prefix=/opt/python39/		# 告诉编译器，python3安装到哪里
    ```

  - 执行make命令开始编译，编译完成后开始安装，make install

    ```
    make && make install
    ```

  - 配置python3的环境变量

    ```
    echo $PATH		# 查看系统的环境变量
    PATH="在原有的环境变量前添加上python3所在的目录即可"
    若想永久生效环境变量，需要在Linux的/etc/profile全局变量配置文件中写入上述一句，编辑命令：vim /etc/profile, 然后读取配置文件就会生效，读取命令：source /etc/profile
    ```


### django项目运行

> 确认网址能否访问
>
> 确认django端口是否存在
>
> 确认django进程是否存在

## Linux配置阿里云的yum源

- 备份默认的yum仓库

  ```
  linux默认的yum仓库地址：/etc/yum.repos.d/	# 这层目录下的所有repo结尾的文件都是yum仓库
  cd /etc/yum.repos.d
  mkdir backrepo
  mv * ./backrepo
  ```

- 阿里的镜像站

  ```
  https://opsx.alibaba.com/mirror
  ```

- 下载阿里云镜像站的仓库

  ```
  下载阿里云的yum源,-O是改名且指定位置
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
  下载第二个源，epel源，很多工具都放在这里
  wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
  ```

- 测试安装mariadb数据库(MySQL)

  ```
  yum install mariadb-server mariadb -y
  ```

  