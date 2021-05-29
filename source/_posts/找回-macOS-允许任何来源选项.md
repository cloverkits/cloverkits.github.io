---
title: 找回 macOS 允许任何来源选项
date: 2018-08-27 19:25:45
categroies:
- macOS
tags:
- 技巧
- Terminal
---

在 `macOS Sierra` 之后 `Apple` 默认隐藏了 `允许任何来源` 的安装选项，给我们日常使用带来了极大的不便

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Allow.png)

当你安装其他来源应用的时候😂

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/not-open.png)

解决方法：
1. 打开终端，键入命令，输入密码，然后回车
`sudo spctl --master-disable`
2. 然后打开“安全性与隐私”，发现久违的“任何来源”回来了

![allow](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/allowed.png)





