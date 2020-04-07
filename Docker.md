# Docker
```
2020/4/4 fanfuni
在此感谢尚硅谷
```
## 什么是Docker
要理解什么是docker，首先要理解docker为什么会出现。在企业中，开发与上线是两回事。程序员在本机开发完功能，然后打成包，部署在服务器上上线。这一过程中就会出现一个问题。即是程序在程序员本机上没问题，而部署到服务器上就有问题。一般这都是由环境配置问题导致的。这就要要求运维人员复制程序员本机的环境，面对多台服务器时，这是十分费时费力的。更不要说更新环境之类的工作了。

于是 Docker 应运而生

Docker是基于Go语言实现的云开源项目。Docker的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“一次封装，到处运行”。

一句话就是解决了运行环境和配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。

## Docker有什么牛的？
在这之前我们就需要探讨以前的虚拟化技术，比如我们最常使用的虚拟机。但是虚拟机会有几个缺点。例如 1.占用资源多，即使是最小化也需要2GB内存。 2.启动慢 动则几十秒甚至几分钟。 3.虚拟机模拟了很多无用设备  比如声卡，打印机等，对于服务器来说无用。

那么容器虚拟化技术呢？

容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。

传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；

而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。

## Docker的基本组成
- 镜像 就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。
- 容器 Docker 利用容器（Container）独立运行的一个或一组应用。容器是用镜像创建的运行实例。
它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台。
- 仓库 是集中存放镜像文件的场所。

## Docker的 安装
百度一下 你就知道 不多做赘述

安装完之后  systemctl start docker 启动docker  docker run hello-world 运行docker的hello-world 测试是否安装成功

最后 systemctl stop docker 停止服务

## docker 常用命令
### 帮助命令
- docker version 查看docker版本号
- docker info 查看docker的一些详细信息
- docker -help 查看有关help的各种相关指令 

### 镜像指令
- docker images 查看镜像
```
-a 列出本地所有的镜像
-q 只显示镜像id
--digests 显示镜像的摘要信息
--no-trunc 显示完整的镜像信息

```
- docker search xxx（镜像名字）
```
--no-trunc 显示完整的镜像描述
-s 列出收藏数不小于指定值的镜像
--automated 只列出automated bulid类型的镜像
```
- docker pull xxx(镜像名字)：[tag] 下载镜像
- docker rmi xxx（镜像名字id） 删除镜像
```
docker rmi -f 镜像id 删除单个
docker rmi -f 镜像名1：tag 镜像名2：tag
docker rmi -f $(docker images -qa)
```
### 容器命令
有镜像才能创建容器 我们下载一个centos来进行操作 docker pull centos

- **新建**并启动容器 docker run 【options】 image 【commmand】【arg...】
```
options 说明
：有些是一个减号，有些是两个减号
--name="容器新名字": 为容器指定一个名称；
-d: 后台运行容器，并返回容器ID，也即启动守护式容器；
-i：以交互模式运行容器，通常与 -t 同时使用；
-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-P: 随机端口映射；
-p: 指定端口映射，有以下四种格式
      ip:hostPort:containerPort
      ip::containerPort
      hostPort:containerPort
      containerPort
```
- 列出**正在运行**的容器 docker ps [options]
```
-a :列出当前所有正在运行的容器+历史上运行过的
-l :显示最近创建的容器。
-n：显示最近n个创建的容器。
-q :静默模式，只显示容器编号。
--no-trunc :不截断输出。

```
- 退出容器 exit 容器停止推出 ctrl+P+Q 容器不停止退出
- 进入**正在运行**的容器 docker attch  容器id或容器名
```
docker exec -it 容器id bash shell  在容器外对容器里的内容进行操作
```
- 启动容器 docker start 容器id或容器名
- 重启容器同上 换成restart
- 停止容器同上 换成stop
- 强制重启同上 换成kill 
- 删除已停止容器 docker rm 容器id
- 查看容器日志 docker logs -f -t --tail x(数字)
```
-t 加入时间戳
-f 跟随最新的日志打印
--tail x 显示最后x条
```
- docker top 容器名字/id 查看容器内运行进程
- docker inspect 容器名字/id 查看内部细节
- docker cp 容器id：容器内路径  目的路径

## Docker镜像
### 是什么
镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

**Union文件系统（UnionFS）**是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。

### 特点
Docker镜像都是只读的当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”。

## DOcker容器数据卷
### 是什么
DOcker 的持久化文件
### 做什么
用于 容器的持久化 和 容器间继承+共享数据

## DockerFile
### 是什么
Dockerfile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。