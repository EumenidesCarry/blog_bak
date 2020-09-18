---
layout: post
title: Linux 内核及系统日志分析
tags: [Linux, kernel]
index_img: https://th.wallhaven.cc/small/od/odqlol.jpg
banner_img: https://w.wallhaven.cc/full/gj/wallhaven-gj69rq.png
categories: [Linux]
date: 2020-02-18 22:24:19
---

# 由系统服务 rsyslogd 统一管理

* 软件包：rsyslogd-x.x.x_0.x86_64
* 主要程序：/sbin/rsyslogd
* 配置文件：/etc/rsyslog.conf

# 日志消息级别

* 0 EMERG （紧急）：导致主机系统不可用的情况
* 1 ALERT（警告）：必须马上采取措施解决的问题
* 2 CRIT（严重）：比较严重的情况
* 3 ERR（错误）：运行出现错误
* 4 WARNING（提醒）：可能会影响系统功能的时间
* 5 NOTICE（注意）：不会影响系统但是值得注意
* 6 INFO（信息）：一般信息
* 7 DEBUG（调试）：程序或系统调试信息等

# 日志记录的一般格式

```shell
[root@localhost ~]# tail -f /var/log/messages
Jul 19 06:06:33 localhost ModemManager[905]: <warn> Couldn't find support for device at '/sys/devices..'
```


| Jul 19 06:06:33 | localhost | ModemManager[905] | \<warn> | Couldn't find |
| :----:| :----: | :----: | :----:| :----: |
| 时间戳 | 主机名 | 子系统 |消息级别|消息字段内容|
