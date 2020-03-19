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

### Linux常用命令
shutdown

halt

reboot

sync

su-用户名

logout

cat /proc/cpuinfo
