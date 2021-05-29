---
title: macOS 使用 DOSBox 搭建 8086 汇编环境
date: 2019-06-17 14:33:50
categories: 汇编
tags:
- 教程
---

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/assembly.jpg)

## 0x00 什么是汇编语言
>汇编语言（英语：assembly language) 是一种用于电子计算机、微处理器、微控制器，或其他可编程器件的低级语言。在不同的设备中，汇编语言对应着不同的机器语言指令集。一种汇编语言专用于某种计算机系统结构，而不像许多高级语言，可以在不同系统平台之间移植。

## 0x01 安装 DOSBox
在 `macOS` 平台我们一般习惯于通过 `App Store` 来安装所需的软件,可惜的是 `DOSBox` 并没有在 `App Store` 上架, 我们可以通过访问其官方网站来了解和下载它

官方网站:
https://www.dosbox.com/download.php?main=1

除此之外在 `macOS` 平台还有一种优雅的软件安装方式, 使用 Homebrew 包管理来安装想要的软件,如果你安装了 `Homebrew` 可以在终端输入下面的命令快速获得 `DOSBox`

```bash
brew cask install dosbox
```

我这里已经安装好了,所以出现 `already installed` 提示信息

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/dosbox_install.png)

## 0x02 配置 DOSBox 模拟器

初次打开 `DOSBox` 你将会看到如下图所示的界面

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/DOSBox_Main.png)

我们需要用到 `debug` 和 `masm` 编译器等程序来学习 `8086 汇编`, 下面将通过 `mount` 命令挂载当前 `用户目录` 下的 `masm` 文件夹为 `C:\` 盘驱动器共享本机和模拟器的数据

![](https://github.com/cloverkits/hexo_picture_resource/blob/master/picture/DOSBox_Mount.png?raw=true)

`masm` 文件夹里有以下文件:

![](https://github.com/cloverkits/hexo_picture_resource/blob/master/picture/DOSBox_document.png?raw=true)

然后直接在命令行执行 `debug` 命令就可以进入 `debug` 调试程序的交互界面

![](https://github.com/cloverkits/hexo_picture_resource/blob/master/picture/DOSBox_debug.png?raw=true)

不过,每一次都需要手动挂载目录岂不是太不方便了!
通过查阅 `DOSBox` 的官方文档,发现可以通过修改 `dosbox.conf` 配置文件来设置模拟器的各项参数和启动时执行的命令, 这样的话我们只需要在 `dosbox.conf` 中加入所需的 “初始化” 命令就可以实现自动化操作

>macOS 系统中 `dosbox.conf` 位于:
`~/Library/Preferences/DOSBox\ 0.74-2\ Preferences`

图中的 **高亮行** 就是我加入的命令,大家可以根据实际情况修改

![](https://github.com/cloverkits/hexo_picture_resource/blob/master/picture/DOSBox_chconfig.png?raw=true)

## 0x03 相关程序下载
>百度云☁️：
链接: https://pan.baidu.com/s/1GlbyyEF6MgAQoqAf_K9BDg
邀请码: `jhjg`

## 0x04 参考资料
[DOSBox 官方文档](https://www.dosbox.com/wiki/Dosbox.conf#Mac_OS_X)