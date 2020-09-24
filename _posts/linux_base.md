---
layout: post
title: Linux 运维基本功学习路径
tags: [Linux]
index_img: https://th.wallhaven.cc/small/45/45j2k1.jpg
banner_img: https://w.wallhaven.cc/full/45/wallhaven-45j2k1.jpg
categories: [Linux]
date: 2020-09-24 09:43:03
---

# 一、 安装、配置、基础命令

## 1.1 镜像文件下载

### 1.1.1 CentOS 镜像

#### 简介

CentOS，是基于 Red Hat Linux 提供的可自由使用源代码的企业级 Linux 发行版本；是一个稳定，可预测，可管理和可复制的免费企业级计算平台。

#### 相关链接

下载地址：
- [阿里源](https://mirrors.aliyun.com/centos/)
- [163源](http://mirrors.163.com/centos/)

### 1.1.2 Ubuntu 镜像

#### 简介

Ubuntu，是一款基于 Debian Linux 的以桌面应用为主的操作系统，内容涵盖文字处理、电子邮件、软件开发工具和 Web 服务等，可供用户免费下载、使用和分享。

#### 相关链接

- [阿里源](https://mirrors.aliyun.com/ubuntu/)
- [163源](http://mirrors.163.com/ubuntu-releases/)

## 1.2 安装

### 1.2.1 分区

关于分区详细：[关于 Linux 分区的小事](https://ecarry.cc/2020/06/09/Linux_part/#)

### 1.2.2 强制使用 GPT 来分区

>如果硬盘容量小于 2TB 的话，系统预设会使用 MBR 模式来安装。

1. 在安装界面，如图所示，按下 `Tab` 键，在后面输入 `inst.gpt`：

![](/img/linux_base/linux_base_1.jpg)

2. 在分区界面需要添加 `biosboot` 分区，此分区供 **bios** 使用，必须创建，而使用 MBR 分区就无需此分区。

![](/img/linux_base/linux_base_2.jpg)

参考资料: 

wiki: [BIOS boot partition](https://en.wikipedia.org/wiki/BIOS_boot_partition)

3. 其余分区按需分配

### 1.2.3 测试安装设备的稳定性

CentOS 安装镜像自带烧机功能，如图：

![](/img/linux_base/linux_base_3.jpg)

测试界面：

![](/img/linux_base/linux_base_4.jpg)

## 1.3 基础命令

### 1.3.1 locale 

运行时地区、语言、字符集和时区

```bash
[root@localhost ~]# locale
LANG=en_US.UTF-8                    <--- 语言，只与输出信息有关
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"               <--- 时区
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=                             <--- 修改所有
```

修改输出信息为中文：

```bash
[root@localhost ~]# LANG=zh_CN.UTF-8
```

### 1.3.2 date 

时间命令

```bash
[root@localhost ~]# date
Thu Sep 24 21:50:28 CST 2020
```

格式化时间：

```bash
[root@blog ~]# date '+%Y-%m-%d %H:%M:%S'
2020-09-24 21:52:49
```

### 1.3.3 cal 

日历

```bash
[root@blog ~]# cal
#显示当月月份
   September 2020   
Su Mo Tu We Th Fr Sa
       1  2  3  4  5
 6  7  8  9 10 11 12
13 14 15 16 17 18 19
20 21 22 23 24 25 26
27 28 29 30

[root@blog ~]# cal 2020
#显示2020年月份
                               2020                               

       January               February                 March       
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
          1  2  3  4                      1    1  2  3  4  5  6  7
 5  6  7  8  9 10 11    2  3  4  5  6  7  8    8  9 10 11 12 13 14
12 13 14 15 16 17 18    9 10 11 12 13 14 15   15 16 17 18 19 20 21
19 20 21 22 23 24 25   16 17 18 19 20 21 22   22 23 24 25 26 27 28
26 27 28 29 30 31      23 24 25 26 27 28 29   29 30 31

        April                   May                   June        
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
          1  2  3  4                   1  2       1  2  3  4  5  6
 5  6  7  8  9 10 11    3  4  5  6  7  8  9    7  8  9 10 11 12 13
12 13 14 15 16 17 18   10 11 12 13 14 15 16   14 15 16 17 18 19 20
19 20 21 22 23 24 25   17 18 19 20 21 22 23   21 22 23 24 25 26 27
26 27 28 29 30         24 25 26 27 28 29 30   28 29 30
                       31
        July                  August                September     
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
          1  2  3  4                      1          1  2  3  4  5
 5  6  7  8  9 10 11    2  3  4  5  6  7  8    6  7  8  9 10 11 12
12 13 14 15 16 17 18    9 10 11 12 13 14 15   13 14 15 16 17 18 19
19 20 21 22 23 24 25   16 17 18 19 20 21 22   20 21 22 23 24 25 26
26 27 28 29 30 31      23 24 25 26 27 28 29   27 28 29 30
                       30 31
       October               November               December      
Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa   Su Mo Tu We Th Fr Sa
             1  2  3    1  2  3  4  5  6  7          1  2  3  4  5
 4  5  6  7  8  9 10    8  9 10 11 12 13 14    6  7  8  9 10 11 12
11 12 13 14 15 16 17   15 16 17 18 19 20 21   13 14 15 16 17 18 19
18 19 20 21 22 23 24   22 23 24 25 26 27 28   20 21 22 23 24 25 26
25 26 27 28 29 30 31   29 30                  27 28 29 30 31


[root@blog ~]# cal 10 2020
#显示2020年10月
    October 2020    
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31

```

### 1.3.4 man 

**查看某个命令介绍及使用方法**，用法：

```bash
[root@blog ~]# man date
DATE(1)                                                             User Commands                                                             DATE(1)

NAME
       date - print or set the system date and time

SYNOPSIS
       date [OPTION]... [+FORMAT]
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

DESCRIPTION
       Display the current time in the given FORMAT, or set the system date.

       Mandatory arguments to long options are mandatory for short options too.
```

### 1.3.5 sync

写盘命令，将还在内存中的数据写入磁盘。


# 二、 文件及目录管理

## 2.1 文件命名规则

## 2.2 目录管理

## 2.3 文件管理

## 2.4 文件的打包和压缩

# 三、 VIM

## 3.1 VIM 编辑器

## 3.2 VIM 模式

### 3.1 命令模式

### 3.2 编辑模式

### 3.3 可视化模式

### 3.4 末行模式

## 3.3 VIM 相关操作

## 3.4 VIM 项目实战

# 四、 用户和组

## 4.1 用户和组的概念

## 4.2 用户管理操作

## 4.3 用户组管理操作

## 4.4 企业级用户管理实战

# 五、  管道命令和 ssh

## 5.1 管道命令 |

## 5.2 grep 

## 5.3 网络配置

### 5.3.1 网络相关命令

## 5.4 SSH

### 5.4.1 ssh 协议

### 5.4.2 sshd 服务

## 5.5 远程管理和文件传输

# 六、 权限管理

## 6.1 文件权限描述

## 6.2 linux 权限与用户身份类别

## 6.4 文件权限设置

## 6.5 权限扩展

设置位 S 与 沾附位 T

## 6.6 ACL 权限

## 6.7 UMASK

# 七、 服务管理 

## 7.1 systemctl

## 7.2 ntpd 时间同步

## 7.3 firewalld 防火墙

## 7.4 rpm 包管理

## 7.5 crond 计划任务详解

# 八、 环境配置

## 8.1 LAMP 环境概述

## 8.2 LAMP 环境编译安装

## 8.3 YUM 源配置

## 8.4 YUM 配置 LAMP 环境

