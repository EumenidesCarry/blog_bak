---
layout: post
title: Linux 运维故障排查与系统调优技巧 
tags: [Linux]
index_img: https://th.wallhaven.cc/small/kw/kweqx6.jpg
banner_img: https://w.wallhaven.cc/full/3k/wallhaven-3k953v.jpg
categories: [Linux]
date: 2019-10-24 17:47:09
---
# 更改主机名（hostname）

在更改主机名时将主机名添加至 `/etc/hosts`下，做个映射

`127.0.0.1		hostname` 

这样能够避免后续安装应用所出现的问题

<!-- more -->

# SSH 设置

ssh 配置文件路径，修改配置文件前记得备份文件

`cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`

`/etc/ssh/sshd_config`



## 关闭 GSSAPI 验证，可以提高 ssh 连接速度

 `GSSAPIAuthentication no`

## 不使用 DNS 反查，可以提高 ssh 连接速度

 `UseDNS no`



# yum 设置

## 常用 yum 源

* epel源：https://fedoraproject.org/wiki/EPEL 
* repoforge 源：http://repoforge.org/use/ 


# 排障关注点

* /var/log/messages	\#应用日志查询
* /var/log/secure         \#登录日志查询
* dmesg                        \#系统日志查询
* /var/tmp 、/tmp       #容易攻击点查询
* crontab -l 、/etc/crontab    \#计划任务查询（经常攻击对象）

## dmesg 命令、文件

直接输出当前最新的系统日志。

系统每次开机将日志保存到`/var/log/dmesg`

旧的日志会保存到`/var/log/dmesg.old`

## secure 文件

ssh 登录日志

可以通过过滤查看登录成功的日志

`tail -1000 /var/log/secure |grep Accepted`

## temp 文件夹（/var/tmp 、/tmp ）

通过`ls -al`查看 temp 文件是否有异常文件

## crontab

通过`crontab -l`查看是否有异常的计划任务

系统计划任务配置文件在`/ect/crontab`，目录`/etc/cron.*/`里放的脚本科自动执行，也要检查