---
title: Windows 10 下访问校园网共享服务器
date: 2018-12-15 13:38:18
categories:
- 校园生活
tags:
- 技巧
- 教程
---
## 前言

在我们学校的课堂和日常学习中，经常会访问 `\\192.168.88.88` 这么一个 `IP` 地址

它是学校为了解决各终端设备之间的`文件共享`问题所搭建的内网 `SMB` 服务器

经过初步扫描发现学校的 `SMB` 有三务器进行负载均衡,每台各存储不同的资料内容

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/VLAN.png)

## 问题

由于宿舍宽带接入通过学校的`网关`我们可以直接在`资源管理器`中直接访问 `SMB` 服务器

我猜想学校使用了 `VLAN` 技术把宿舍的宽带和内网以某种方式形成一个 `LAN` 局域网

然而在实际使用中却发生了问题，已知任何几台电脑都可以正常访问，但是在宿舍却打不开



> #### 分析:

经过测试 `SMB` 在 `Windows 7` 系统中能够正常访问，而 `Windows 10` 或者 `macOS Mojave` 和 `Linux` 中均无法打开；而在 `macOS` 中能够使用命令扫描到内网有 `SMB` 开放端口和共享目录

后来通过 `Google` 发现了问题的关键，原因是学校的 `SMB` 文件服务器使用的协议版本太老了



> #### 解决：

我们可以通过启用系统中的 `SMB` 旧版本支持来实现正常访问

1. 通过 Windows 搜索或者面板找到 `启用或关闭 Windows 功能`

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Windows Search.png)

2. 勾选 `SMB 1.0/CIFS File Sharing Support` 支持, 并按 `确定`键应用更改
![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Enable Windows Feature.png)

3. 重启电脑
4. 在资源管理器访问 `\\192.168.88.88` 并输入你的 `校园统一门户` 账号密码完成登录

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/VLAN_ID.PNG)



