---
layout: post
title: Docker 简单上手
tags: [Docker]
index_img: https://th.wallhaven.cc/small/r7/r7jolj.jpg
banner_img: https://w.wallhaven.cc/full/r7/wallhaven-r7jolj.png
categories: [Docker]
date: 
---

# 一、 Docker 简介

## 1.1 是什么

- 使用最广泛的开源容器引擎
- 一种操作系统级别的虚拟化技术
- 依赖于 Linux 内核特性：**Namespace**（<font color=red>资源隔离</font>）和 **Cgroups**（<font color=red>资源限制</font>）
- 一个简单的应用程序打包工具

## 1.2 基本组成

- Docker Client：客户端
- Docker Daemon：守护进程
- Docker Images：镜像
- Docker Container：容器
- Docker Registry：镜像仓库

![](https://docs.docker.com/engine/images/architecture.svg)
  
## 1.3 容器 VS VM

![](/img/Docker/Docker_1.jpg)

||容器|VM|
|---|---|---|
|启动速度|秒级|分钟级|
|运行性能|接近原生|5%左右损失|
|磁盘占用|MB|GB|
|数量|成百上千|一般几十台|
|隔离性|进程隔离|系统级（彻底）|
|操作系统|只支持 Linux|所有|
|封装进度|只打包项目代码和依赖关系，共享宿主机内核|完整的操作系统|

## 1.4 应用场景

- 应用程序打包和发布
- 应用程序隔离
- 持续的集成
- 部署微服务
- 快速搭建测试环境
- 提供 PaaS（Platform as a Service），指平台即服务

# 二、 Docker 安装

## 2.1 CentOS 7 安装（使用 yum 安装）

### 官方安装手册

地址：https://docs.docker.com/engine/install/centos/

### 阿里源安装手册（推荐）

使用[阿里源](https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.3e221b11q3kwWA)安装 Docker


删除旧版本的 docker：

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装步骤：

```
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新 yum 软件包索引并安装Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo service docker start

# 注意：
# 官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，您可以通过以下方式开启。同理可以开启各种测试版本等。
# vim /etc/yum.repos.d/docker-ee.repo
#   将[docker-ce-test]下方的enabled=0修改为enabled=1
#
# 安装指定版本的Docker-CE:
# Step 1: 查找Docker-CE的版本:
# yum list docker-ce.x86_64 --showduplicates | sort -r
#   Loading mirror speeds from cached hostfile
#   Loaded plugins: branch, fastestmirror, langpacks
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
#   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
#   Available Packages
# Step2: 安装指定版本的Docker-CE: (VERSION例如上面的17.03.0.ce.1-1.el7.centos)
# sudo yum -y install docker-ce-[VERSION]
```

校验安装：

```
[root@test ~]# docker version
Client: Docker Engine - Community
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46d9d
 Built:             Wed Sep 16 17:03:45 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46d9d
  Built:            Wed Sep 16 17:02:21 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

运行 **HELLO WORLD**:

```
[root@test ~]# docker run hello-world
#由于本地没有 hello-world 这个镜像，所以会下载一个 hello-world 的镜像，并在容器中运行
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
Status: Downloaded newer image for hello-world:latest

#上面安装，下面运行
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

**run** 启动流程：

![](/img/Docker/Docker_2.jpg)

## 2.2 获取镜像

### Docker Hub 官方镜像

官方镜像地址：https://hub.docker.com/explore

### Docker 镜像加速

国内从 DockerHub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。Docker 官方和国内很多云服务商都提供了国内加速器服务，例如：

- 网易：https://hub-mirror.c.163.com/
- 阿里云：https://<你的ID>.mirror.aliyuncs.com

阿里云镜像获取地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors ，登录后可见加速地址。

![](/img/Docker/Docker_3.jpg)

通过修改 `/etc/docker/daemon.json` 文件配置镜像加速器：

```
#-p 确保目录名称存在，不存在的就建一个。
sudo mkdir -p /etc/docker
#下面加速二选一，如果效果不理想，试着切换
#163加速
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  {"registry-mirrors": ["http://hub-mirror.c.163.com"] }
}
EOF
#阿里源加速
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0hm4nl4c.mirror.aliyuncs.com"]
}
EOF

cat daemon.json 
{
  "registry-mirrors": ["https://0hm4nl4c.mirror.aliyuncs.com"]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 三、 Docker 常用命令

## 3.1 帮助命令

### 3.1.1 docker version

```
[root@test ~]# docker version
Client: Docker Engine - Community
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46d9d
 Built:             Wed Sep 16 17:03:45 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46d9d
  Built:            Wed Sep 16 17:02:21 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

### 3.1.2 docker info
### 3.1.3 docker --help

## 3.2 镜像命令

### 3.2.1 docker images

显示本地镜像：
```
[root@test ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               latest              992e3b7be046        6 days ago          133MB
hello-world         latest              bf756fb1ae65        9 months ago        13.3kB
```
#### options:
- -a ：列出本地所有镜像
- -q ：只显示镜像 ID
- --digests ：显示镜像的摘要信息
- --no-trunc ：显示完整的镜像信息

### 3.2.2 docker search 

`docker search 指定镜像名`

```
[root@test ~]# docker search nginx
NAME                               DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                              Official build of Nginx.                        13838               [OK]                
jwilder/nginx-proxy                Automated Nginx reverse proxy for docker con…   1893                                    [OK]
richarvey/nginx-php-fpm            Container running Nginx + PHP-FPM capable of…   790                                     [OK]
linuxserver/nginx                  An Nginx container, brought to you by LinuxS…   127                                     
jc21/nginx-proxy-manager           Docker container for managing Nginx proxy ho…   96                                      
tiangolo/nginx-rtmp                Docker image with Nginx using the nginx-rtmp…   95                                      [OK]
```
#### options:

docker search [options] 镜像名字

- --no-trunc ：显示完整的镜像描述
- -s ：列出收藏数不小于指定值的镜像
- --automated ：只列出 automated build 类型镜像

### 3.2.3 docker pull

`docker pull 镜像名[:TAG]` ：下载镜像，不加 TAG 默认下载 latest

```
[root@test ~]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
```

### 3.2.4 docker rmi

`docker rmi 镜像ID` ：删除镜像

- docker rmi -f 镜像ID ：删除单个
- docker rmi -f 镜像1ID 镜像2ID ：删除多个镜像
- docker rmi -f $(docker images -qa) ：删除所有镜像

## 3.3 容器命令

<font color=red>有镜像才能够创建容器</font>，下载 CentOS 镜像演示：`docker pull centos`

### 3.3.1 新建并启动容器

`docker run [options] image [command] [arg]`

#### options
- --name="new name" ：为容器指定一个名称
- -d ：后台运行容器，并返回容器ID，也即启动守护式容器
- -i ：以交互模式运行容器，通常与 -t 同时使用
- -t ：为容器重新分配一个伪终端，通常与 -i 同时使用
- -P ：随机端口映射
- -p ：指定端口映射，有以下四种格式
  - ip:hostPort:containerPort
  - ip::containerPort
  - hostPort:containerPort
  - containerPort

```
#使用镜像centos:latest以交互模式启动一个容器，在容器内执行/bin/bash命令
[root@test ~]# docker run -it centos /bin/bash
[root@7808a5311ae1 /]# cat /etc/redhat-release 
CentOS Linux release 8.2.2004 (Core)
```

### 3.3.2 列出当前运行的容器

`docker ps [options]`

#### options
- -a ：列出当前所有正在运行的容器和历史上运行过的
- -l ：显示最新创建的容器
- -q ：静默模式，只显示容器编号
- --no-trunc ：不截断输出

### 3.3.3 退出容器

#### 容器停止并退出

`exit`

#### 容器不停止退出

`ctrl+P+Q`

### 3.3.4 启动容器

`docker start 容器ID或者容器名`

### 3.3.5 重启容器

`docker restart 容器ID或者容器名`

### 3.3.6 停止容器

`docker stop 容器ID或者容器名`

### 3.3.7 强制停止容器

`docker kill 容器ID或者容器名`

### 3.3.8 删除已停止的容器

`docker rm 容器ID`

一次性删除多个容器：

- `docker rm -f $(docker pa -a -q)`
- `docker ps -a -q | xargs docker rm`

## 3.4 比较重要的命令

### 3.4.1 启动守护式容器

`docker run -d 容器名`


使用镜像centos:latest以后台模式启动一个容器`docker run -d centos`

**问题**：
然后 `docker ps -a` 进行查看, 会发现容器已经退出。很重要的要说明的一点: <font color=red>Docker容器后台运行，就必须有一个前台进程。</font>容器运行的命令如果不是那些<font color=red>一直挂起的命令</font>（比如运行top，tail），就是会自动退出的。
这个是 docker 的机制问题，比如你的 web 容器，我们以 nginx 为例，正常情况下，我们配置启动服务只需要启动响应的 service 即可。例如 `service nginx start`，但是，这样做，nginx 为后台进程模式运行，就导致 docker 前台没有运行的应用，这样的容器后台启动后，会立即自杀因为他觉得他没事可做了。所以，最佳的解决方案是,将你要运行的程序以前台进程的形式运行

### 3.4.2 查看容器日志

`docker logs -f -t --tail 容器ID`
- -t ：加入时间戳
- -f ：跟随最新的日志打印
- --tail 数字 显示最后几条

### 3.4.3 查看容器内运行的进程

`docker top 容器ID`

### 3.4.4 查看容器内部细节

`docker inspect 容器ID`

### 3.4.5 进入正在运行的容器并以命令行交互

#### docker exec

`docker exec -it 容器ID /bin/bash`，在容器中打开终端，并且可以启动新的进程

```
[root@test ~]# docker ps -a 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
150fcfbacdfa        centos              "/bin/bash"              3 minutes ago       Up 3 minutes                                    flamboyant_driscoll
[root@test ~]# docker exec -it 150fcfbacdfa /bin/bash
[root@150fcfbacdfa /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 12:08 pts/0    00:00:00 /bin/bash
root        14     0  0 12:13 pts/1    00:00:00 /bin/bash
root        27    14  0 12:13 pts/1    00:00:00 ps -ef
```

#### docker attach

`docker attach 容器ID`，直接进入容器启动命令终端，不会启动新的进程

### 3.4.6 从容器内拷贝文件到主机上

`docker cp 容器ID:容器内路径 目的主机路径`

```
#在容器内创建 hello 文件
[root@b7394537e333 ~]# pwd
/root
[root@b7394537e333 ~]# ls
hello
#将容器内 /root/hello 文件拷贝到主机 /tmp 下
[root@test ~]# docker cp b7394537e333:/root/hello /tmp/
[root@test ~]# ls /tmp/
hello
```

# 四、 Docker 镜像

## 4.1 镜像是什么

- 一个分层存储的文件
- 一个软件的环境
- 一个镜像可以创建 N 个容器
- 一种标准化的交付
- 一个不包含 Linux 内核而又精简的 Linux 操作系统

镜像不是一个单一的文件，而是有多层结构。我们可以通过 `docker history <ID/NAME>` 查看镜像中各层内容及大小，每层对应着 Dockerfile 中的一条指令。Docker 镜像默认存储在 `/var/lib/docker/\<storage-driver\>中。

```
[root@docker ~]# docker history nginx
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
7e4d58f0e5f3        4 weeks ago         /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  STOPSIGNAL SIGTERM           0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  EXPOSE 80                    0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB              
<missing>           4 weeks ago         /bin/sh -c #(nop) COPY file:1d0a4127e78a26c1…   1.96kB              
<missing>           4 weeks ago         /bin/sh -c #(nop) COPY file:e7e183879c35719c…   1.2kB               
<missing>           4 weeks ago         /bin/sh -c set -x     && addgroup --system -…   63.4MB              
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENV PKG_RELEASE=1~buster     0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENV NJS_VERSION=0.4.3        0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  ENV NGINX_VERSION=1.19.2     0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop)  CMD ["bash"]                 0B                  
<missing>           4 weeks ago         /bin/sh -c #(nop) ADD file:e7407f2294ad23634…   69.2MB    
```

### 4.1.1 UnionFS

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种**分层**、**轻量级**并且**高性能**的文件系统，它支持对文件系统的修改作为<font color=red>一次提交来一层层的叠加</font>，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

**特性**：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

镜像文件分层结构的好处--**共享资源**。比如：有多个镜像都从相同的 base 镜像构建而来，那么宿主机只需在磁盘上保存一份base镜像，同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。

### 4.1.2 Docker 镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含 **bootloader** 和 **kernel** , **bootloader** 主要是引导加载 kernel, Linux 刚启动时会加载 bootfs 文件系统，<font color=red>在 Docker 镜像的最底层是 bootfs</font>。这一层与我们典型的 Linux/Unix 系统是一样的，包含 boot 加载器和内核。当 boot 加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs 转交给内核，此时系统也会卸载 bootfs。

rootfs (root file system) ，在 bootfs 之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs 就是各种不同的操作系统发行版，比如 Ubuntu，Centos 等等。 

![](/img/Docker/Docker_5.jpg)

## 4.2 镜像特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层通常被称为“容器层”，“容器层”下都叫“镜像层”。

## 4.3 镜像与容器的联系

![](/img/Docker/Docker_4.jpg)

如图，容器其实是在镜像的最上面加了一层读写层，在运行容器里文件改动时，会先从镜像里要写的文件复制到容器自己的文件系统中（读写层）。如果容器删除了，最上面的读写层也就删除了，改动也就丢失了。所以无论多少个容器共享一个镜像，所做的写操作都是从镜像的文件系统中复制过来操作的，<font color=red>并不会修改镜像的源文件</font>，这种方式<font color=red>提高磁盘利用率</font>。
若想持久化这些改动，可以通过 `docker commit` 将容器保存成一个新镜像。
- 一个镜像创建多个容器
- 镜像增量式存储
- 创建的容器里面修改不会影响到镜像

## 4.4 镜像 commit 操作

`docker commit [options]` ：提交容器副本使之成为一个新的镜像

- -m ：提交的描述信息
- -a ：作者



































# 五、 Docker 容器数据卷

# 六、 DockerFile

# 七、 Docker 安装常用服务
