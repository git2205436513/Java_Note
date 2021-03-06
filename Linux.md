# Linux
```
20202/3/18 fanfuni
感谢尚硅谷
```
## 什么是Linux
linux是一个开源、免费的操作系统，其稳定性、安全性、处理多并发已经得到
业界的认可，目前很多企业级的项目都会部署到Linux/unix系统上。
## Linux的目录结构
linux的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录“/”，然后在此目录下再创建其他的目录。

**在Linux的世界里，一切皆文件**

- **/bin** 是Binary的缩写, 这个目录存放着最经常使用的命令
- /sbin s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
- **/home** 存放普通用户的主目录，在Linux中每个用户都有一个自己的目录，一般
该目录名是以用户的账号命名的。
- **/root** 该目录为系统管理员，也称作超级权限者的用户主目录
- /lib 系统开机所需要最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几
乎所有的应用程序都需要用到这些共享库
- /lost+found 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件
- **etc** 所有的系统管理所需要的配置文件和子目录 my.conf
- **/usr**这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与
windows下的program files目录。
- **/boot**存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件
- proc这个目录是一个虚拟的目录，它是系统内存的映射，访问这个目录来获取系统信息。
- /srv service缩写，该目录存放一些服务启动之后需要提取的数据
- /sys 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sy
- /tmp 这个目录是用来存放一些临时文件的。
- /dev 类似于windows的设备管理器，把所有的硬件用文件的形式存储
- **/media** linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux
会把识别的设备挂载到这个目录下。
- **/mnt** 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂
载在/mnt/上，然后进入该目录就可以查看里的内容了
- /opt 这是给主机额外安装软件所摆放的目录。如安装ORACLE数据库就可放到该目录下。
默认为空。
- **/usr/local** 这是另一个给主机额外安装软件所安装的目录。一般是通过编译源码方式安装的程序
- **/var** 这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下。
包括各种日志文件
- /senlinux SELinux是一种安全子系统,它能控制程序只能访问特定文件。

## linux常用指令
### vi与vim
- 正常模式：进入vim的默认模式
- 插入模式：按下i、o、a、r等字母进入编辑模式
- 命令行模式：提供相关指令，完成读取、存盘、替换、离开等操作

#### vim的常用指令
yy 拷贝当前行

5yy拷贝当前向下的5行并粘贴

dd 删除当前行

5dd 删除当前向下5行

/ 在文件中查找关键字，输入n就是查找下一个

set nu/nonu 设置文件的行号，取消文件的行号

G 移动到最下行 gg 移动到第一行

### 登录、注销

**shutdown**
-  -h now 立刻关机
-  -h 1 1分钟后关机
-  -r now 立刻重启

halt 立刻关机

reboot  立刻重启

sync 把内存数据同步到磁。不管是重启还是关闭，都应运行sync命令防止数据丢失

logout 用户登出  这个指令在图形运行级别无效

cat /proc/cpuinfo

### 用户管理
useradd 用户名 添加用户

userdel 用户名 删除用户

id 用户名

su-用户名 切换用户 (用exit指令返回原来用户)

whoami 查看当前用户/登录用户

### 用户组
gruopdel 组名 删除组

useradd -g 用户组 用户名 增加用户时直接加上组

usermod -g 用户组 用户名

#### 用户和组的相关文件
/etc/passwd 用户的配置文件。每行的含义：用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell

/etc/shadow 口令的配置文件。每行的含义：登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志

/etc/gruop 组(group)的配置文件，记录Linux包含的组的信息
每行含义：组名:口令:组标识号:组内用户列表

### 指定运行级别
- 0 关机
- 1 单用户：用于找回密码
- 2 多用户状态没有网络服务
- 3 多用户状态有网络服务
- 4 系统未使用保留给用户
- 5 图形界面
- 6 系统重启

常用运行级别是3和5 ，要修改默认的运行级别可改文件
/etc/inittab的id:5:initdefault:这一行中的数字
命令：init [012356]

### 帮助指令
man/help

### 文件目录
pwd 显示当前工作目录的绝对路径

ls 
- -a 显示当前目录所有的文件和目录，包括隐藏的
- -l 以列表的方式显示信息

cd （默认返回自己的家目录）
- ~  返回自己的家目录
- .. 返回当前目录的上一级目录

mkdir 创建目录
- -p 创建多级目录

rmdir 删除空目录

rm 移除文件或目录
- -r 递归删除整个文件夹
- -f 强制删除步提示

touch 创建空文件

cp 拷贝文件到指定文件夹
- -r 递归复制整个文件夹
- \cp 强制覆盖不提示

mv 移动文件或目录或重命名
- mv name name 重命名
- mv 路径 路径 移动文件

cat 查看文件内容
- n 显示行号

more 基于vi编辑器的文本过滤器，内置了若干快捷键

less 分屏查看文件内容，一般用于显示大型文件

echo 输出内容到控制台

head 用于显示文件的开头内容 默认显示前10行
- head -n 5 文件  显示文件的前五行内容

tail 显示文件尾部内容 默认显示最后10行 用法同head
- tail -f 文件 实时追踪该文档的所有更新

>与>> 输出重定向和追加

ln 类似windows里的快捷方式

history 查看已经执行过的历史命令
### 时间日期类
date 显示当前日期

date -s 字符串时间 设置日期

cal 日期指令

### 搜索查找类
find 从指定目录向下递归遍历其各个子目录，将满足的目录和文件显示在终端
- -name 按指定文件名查找
- -user 按指定所属用户名查找
- -size 按指定文件大小查找

locate 快速定位文件路径

grep指令和管道符号|

- grep -n 显示匹配行及行号
- grep -i 忽略字母大小写

### 压缩和解压类
gzip 压缩 gunzip解压

zip 压缩 unzip解压

tar 打包 

### 组管理和权限管理
在linux中的每个用户必须属于一个组，不能独立于组外。在linux中每个文件
有所有者、所在组、其它组的概念。
- 所有者
- 所在组
- 其他组
- 改变用户所在的组

#### 所有者
ls -ahl 查看文件所有者

chown 用户名 文件名 修改文件所有者

#### 组的创建
gruopadd 组名

#### 文件/目录 所在组
ls -ahl 查看文件/目录所在组

chgrp 修改文件所在组

#### 改变用户所在组
usermod -g 组名 用户名
usermod -d 目录名 用户名 改变该用户登录的初始目录

chmod 修改权限

chown 修改所有者

chgrp 修改文件所在组

### 实时任务调度
crontab 进行实时任务的设置
```
-e 编辑crintab定时任务
-l 查询crontab任务
-r 删除当前用户所有的contab任务

```
### 磁盘分区、挂载
#### 分区基本方式
mbr分区
```
最多支持四个主分区
系统只能安装在主分区
扩展分区要占一个主分区
MBR最大只支持2TB，但拥有最好的兼容性
```

gtp分区
```
支持无限多个主分区
最大支持18EB的大容量
win7以后支持gtp
```

### 进程管理
ps 查看目前系统中有那些进程正在执行
- -a 显示所有进程信息
- -u 以用户格式显示进程信息
- -x 显示后台进程运行参数
- ps -aux|grep xxx
- -e显示所有进程
- -f 全格式
- ps -ef|grep xxx

kill 通过进程号杀死进程
- -9 强迫进程立即停止

killall 通过进程名称杀死进程

pstree 直观的查看进程信息
- -p 显示进程的PID
- -u 显示进程的所属用户

### 服务管理
service 服务管理
- start/stop/restart/reload/status


### RPM和YUM
#### RPM
一种用于互联网下载包的打包及安装工具，它包含在某些Linux分发版中。它生成
具有.RPM扩展名的文件。RPM是RedHat Package Manager（RedHat软件包管理工
具）的缩写，类似windows的setup.exe，这一文件格式名称虽然打上了RedHat的
标志，但理念是通用的。

#### YUM
Yum 是一个Shell前端软件包管理器。基于RPM包管理，能够从指定 的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并 且一次安装所有依赖的软件包。

