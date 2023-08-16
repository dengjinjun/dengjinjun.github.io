---
title: Linux命令基础
date: 2023-08-15 22:39:36
tags: Linux
---

# Linux快捷键操作

- Ctrl + C 	停止指令的执行
- Ctrl + D    退出键盘输入/exit
- Ctrl + L     清屏/clear
- Ctrl + a     光标移动到命令行最前端
- Ctrl + e     光标移动到命令行最末端
- Ctrl + r      搜索历史命令
- Shift + PageUp    向前翻页
- Shift + PageDown   向后翻页
- tab    自动补全命令或文件路径/文件名
- 向上方向键    取上一条命令
- 输入"!$"    取上一条命令的最后一个参数

## 命令查找优先级

​	==》绝对路径

​				==》组合命令

​							==》函数

​									==》内置命令

​											==》哈希命令

​													==》$PATH

# man page 相关介绍

- man page 各部分内容

  | 代号        | 内容说明                                                     |
  | :---------- | :----------------------------------------------------------- |
  | NAME        | 简短的指令、数据名称说明                                     |
  | SYNOPSIS    | 简短的指令下达语法（syntax）简介                             |
  | DESCRIPTION | 较为完整的说明，这部分最好仔细看看！                         |
  | OPTIONS     | 针对 SYNOPSIS 部分中，有列举的所有可用的选项说明             |
  | COMMANDS    | 当这个程序（软件）在执行的时候，可以在此程序（软件）中下达的指令 |
  | FILES       | 这个程序或数据所使用或参考或链接到的某些文件                 |
  | SEE ALSO    | 可以参考的，跟这个指令或数据有相关的其他说明！               |
  | EXAMPLE     | 一些可以参考的范例                                           |

- 第一行，命令名称后面的序号的含义

  | 代号 | 代表内容                                                     |
  | :--- | :----------------------------------------------------------- |
  | 1    | 使用者在shell环境中可以操作的指令或可可执行文件              |
  | 2    | 系统核心可调用的函数与工具等                                 |
  | 3    | 一些常用的函数（function）与函数库（library），大部分为C的函数库（libc） |
  | 4    | 设备文件的说明，通常在/dev下的文件                           |
  | 5    | 配置文件或者是某些文件的格式                                 |
  | 6    | 游戏（games）                                                |
  | 7    | 惯例与协定等，例如Linux文件系统、网络协定、ASCII code等等的说明 |
  | 8    | 系统管理员可用的管理指令                                     |
  | 9    | 跟kernel有关的文件                                           |

- man page 页内的一些操作

  | 按键        | 进行工作                                                     |
  | :---------- | :----------------------------------------------------------- |
  | 空白键      | 向下翻一页                                                   |
  | [Page Down] | 向下翻一页                                                   |
  | [Page Up]   | 向上翻一页                                                   |
  | [Home]      | 去到第一页                                                   |
  | [End]       | 去到最后一页                                                 |
  | /string     | 向“下”搜寻 string 这个字串，如果要搜寻 vbird 的话，就输入 /vbird |
  | ?string     | 向“上”搜寻 string 这个字串                                   |
  | n, N        | 利用 / 或 ? 来搜寻字串时，可以用 n 来继续下一个搜寻 （不论是 / 或 ?） ，可以利用 N 来进行“反向”搜寻。举例来说，我以 /vbird 搜寻 vbird 字串， 那么可以 n 继续往下查询，用 N 往上查询。若以 ?vbird 向上查询 vbird 字串， 那我可以用 n 继续“向上”查询，用 N 反向查询。 |
  | q           | 结束这次的 man page                                          |

# Linux指令：基础指令

- shell命令的语法格式

  `命令`+`选项`+`参数`

  - 命令：就是一个单词，对应着一个功能/程序，运行一条命令就启动了一个进程
  - 选项：对命令的描述，控制着命令的具体运行
  - 参数：命令的操作对象

- 主机名

  ```
  hostname					# 查看主机名
  hostnamectl set-hostname [自定义的主机名]		# 修改主机名（通过命令）
  vim /etc/hostname			# 修改主机名（通过修改配置文件）
  ```

- 系统版本

  ```
  cat /etc/redhat-release		# 查看操作系统发布版本
  uname -r					# 查看操作系统内核
  uname -a					# 查看操作系统所有信息
  ```
  
  
  
- 系统默认启动级别

  ```
  systemctl get-default		# 查看系统默认启动级别
  ```

  ```
  systemctl set-default multi-user.target		# 设置默认以多用户命令行模式启动
  systenctl set-default graghical.target		# 设置默认以图形界面启动
  ```

- 添加用户并修改密码

  ```
  useradd [用户名]		#添加用户
  passwd [用户名]		#修改密码第一种方式
  echo "新密码" | passwd [用户名 ] --stdin		#修改密码第二钟方式
  ```

- 退出当前用户

  ```shell
  exit
  logout
  ```

-  查看终端

  ```
  tty						# 查看当前终端
  who						# 查看目前主机上有哪些终端
  ctrl + alt + [f1~f7]	# 切换终端
  ```

  
  
-  时间与日期

  ```shell
  date				# 查看当前系统时间
  date +%Y%m%d		# 指定格式查看时间
  hwclock				# 查看当前硬件时间
  ```

  ```shell
  date -s "具体时间"		# 修改操作系统时间
  hwclock -w				# 将系统时间写入硬件时间
  hwclock -s				# 将硬件时间写入系统时间
  ```

  ```shell
  ntpdate ntpl.aliyun.com		# 自动同步网络时间（采用aliyun服务器）
  ```

  注意：外网ping不通，可能的原因：CentOS系统上，目前有NetworkManager和network两种网络管理工具。如果两种都配置会引起冲突，而且NetworkManager在网络断开的时候，会清理路由，如果一些自定义的路由，没有加入到NetworkManager的配置文件中，路由就被清理掉，网络连接后需要自定义添加上去。

  解决方法：只保留network服务，将NetworkManager关闭，并禁止开机启动。

  `systemctl stop NetworkManager`

  `systemctl disable NetworkManager`

- 显示日历

  ```
  cal
  ```

  ```
  cal 2023
  ```

  ```
  cal 8 2023
  ```

- 计算器

  ```
  bc
  ```

  ```
  scale=2		==保留两位小数
  ```

  ```
  quit		==退出
  ```

- 历史命令

  ```
  history			# 查看历史命令
  history -c		# 清空历史命令
  cat ~/.bash_history		# 查看历史命令（保存在.bash_history文件中）
  ```
  
  ```
  输入history之后：
  ！cat			# 运行最近的一条命令
  ！[编号]			# 运行指定编号的命令
  ```
  
  注意：history会显示所有已输入命令的累计，而.bash_history文件只会保存上一次登录状态所输入的命令
  
- 查找命令路径

  ```
  which [-a] [command] # 查找指令的完整文件名
  # -a 表示将所有由 PATH 目录中可以找到的指令均列出，而不止第一个被找到的指令名称
  ```
  
  
  
- 别名

  ````
  alias			# 查看系统命令当前的别名
  alias [自定义命令名]='一条系统命令'			# 建立别名
  ````

  取消别名（以`ls`为例）

  ```
  which ls			# 查看命令的完整路径,然后以完成路径来运行命令
  \ls 				# 反斜杠+命令，此次除去命令的特殊意义来运行
  unalias ls			# 取消命令的别名，此后都将以正常意义运行
  ```

- 关机和重启

  ```
  shutdown -h now			# 立刻关机，其中 now 相当于时间为 0 的状态
  shutdown -h 20:25		# 系统在今天的 20:25 分会关机，若在21:25才下达此指令，则隔天才关机
  shutdown -h 10			# 系统再过十分钟后自动关机
  halt       				# 进入系统停止的模式，屏幕可能会保留一些讯息
  poweroff   				# 进入系统关机模式，直接关机没有提供电力
  suspend    				# 进入休眠模式
  ```
  
  ```
  reboot     				# 直接重新开机
  shutdown -r now			# 系统立刻重新开机
  shutdown -r 10			# 系统10分钟后重新开机
  init 6					# 立即重启
  ```
  
  

