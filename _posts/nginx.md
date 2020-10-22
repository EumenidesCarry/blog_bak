---
layout: post
title: Nginx
tags: [Nginx]
index_img: https://th.wallhaven.cc/small/k9/k9vygm.jpg
banner_img: https://w.wallhaven.cc/full/k9/wallhaven-k9vygm.png
categories: [Nginx]
date: 2020-10-21 09:59:00
---

# 一、 基础概念

Nginx 是一个高性能的 **HTTP** 和**反向代理服务器**，特点是占用**内存少**，**并发能力强**，事实上nginx的并发能力确实在同类型的网页服务器中表现较好。

Nginx 相对于 Apache 优点：
- 高并发响应性能非常好，官方 Nginx 处理静态文件并发 5w/s
- 负载均衡及反向代理性能非常强
- 系统内存和 CPU 占用率低
- 可以对后端服务进行将康检查
- 支持 PHP cgi 方式和 FastCGI 方式
- 可作为缓存服务器、邮件代理服务器
- 配置代码简洁且容易上手

## 1.1 静态页面

**静态页面**通常指不与数据库发生交互的页面，是一种基于 w3c 规范的一种网页书写格式



## 1.2 动态页面



# 二、 工作原理

Nginx WEB 服务器最主要的就是各个模块的工作，模块从结构上分为**核心模块**、**基础模块**和**第三方模块**，其中三类模块分别为：
- 核心模块：**HTTP** 模块、**EVENT** 模块和 **MAIL** 模块等
- 基础模块：HTTP Access 模块、HTTP FastCGI 模块、HTTP Proxy 模块和 HTTP Rewrite 模块
- 第三方模块：HTTP Upstream Request Hash 模块、Notice 模块、HTTP Access Key 模块、Limit_req 模块和 Upstream check module 等

Nginx 的模块从功能上分为：
- <font color=red>Handlers（处理器模块）：此类模块直接处理请求，并进行输出内容和修改 headers 信息等操作，Handlers 处理器模块一般只能有一个</font>
- <font color=red>Filters（过滤器模块）：此类模块主要是对其他模块输出的内容进行修改操作，最后有 Nginx 输出</font>
- Proxys（代理类模块）：此类模块是 Nginx 的 HTTP Upstream 之类的模块，这些模块










