---
title: 使用 BaiduPCS-Go 破解百度云下载限速
date: 2018-08-13 20:15:34
categories:
- 工具
tags:
- 技巧
- 工具
---

近几年来网盘服务一个个的“倒下”，留给我们用户的选择不多了。更由于一些众所周知的原因诸如 `Dropbox` `Google Drive` 这类优秀的国外网盘天朝用户是用不了的，你可能会说不是还有 `Microsoft OneDrive` 吗？

但是对于非付费用户 `OneDrive` 的访问速度也是差强人意的；还记得在那个”百家争鸣“的年代各类国产网盘服务如雨后春笋一般不断“涌现”。

一时间`百度云`，`360网盘` ，`华为网盘` ，`UC云` ……
动辄上 `TB` 的免费存储容量支持离线下载在那个网速不算快的年代给我们提供了极大的便利。由于竞争激烈网盘服务商们纷纷声称自己可以提供下载加速，我记得那时候的 `百度云` 确实可以加速下载。

后来国家对知识产权的保护得到加强和完善，盗版资源越来越难留存于世各类网盘服务商需要面临整治盗版和严打网络低俗资源的传播。监管的压力和运营成本是他们意识到再做下去已经不划算了。于是乎随便找个理由“关门大吉”。

最后剩下来的只有少数几个其中的大头就是 `百度云` (由于百度业务调整“百度云”更名为“百度网盘”但是我们还是习惯将其称作“百度云”)；失去了大量竞争对手也就成了“垄断”的存在，`百度云` 完成了从 `百度云` 到 `百毒云` 的“华丽”转变。

普通用户下载限速到 `1Mbps` 以下，普通会员`限半速`超级会员`不限速`但也不加速😂 这套路满满啊！但是你能不用他吗？答案是否定的。。。

最近发现了 [`GitHub`](https://github.com) 上有一个优秀的开源项目 [`BaiduPCS-GO`](https://github.com/iikira/BaiduPCS-Go) 是一款`百度云` `命令行`客户端


>BaiduPCS-Go 百度网盘客户端
>仿 Linux shell 文件处理命令的百度网盘命令行客户端.
>This project was largely inspired by GangZhuo/BaiduPCS


特色:

>多平台支持, 支持 Windows, macOS, linux, 移动设备等.
>百度帐号多用户支持;
>通配符匹配网盘路径和 Tab 自动补齐命令和路径, 通配符_百度百科;
>下载网盘内文件, 支持多个文件或目录下载, 支持断点续传和单文件并行下载;
>上传本地文件, 支持上传大文件(>2GB), 支持多个文件或目录上传;
离线下载, 支持http/https/ftp/电驴/磁力链协议.

![Releases](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/BaiduPCS-Go Release.jpg)

首先在官方 [`Releases`](https://github.com/iikira/BaiduPCS-Go/releases) 页面下载你需要的版本，笔者使用的是 `macOS` 选择 [BaiduPCS-Go-v3.5.3-darwin-osx-amd64.zip](https://github.com/iikira/BaiduPCS-Go/releases/download/v3.5.3/BaiduPCS-Go-v3.5.3-darwin-osx-amd64.zip)

CPU 架构区别：

|amd | arm | misp | 说明|
|----|-----|------|-----|
|amd64, x64|arm64|mips64, mips64le|适用于64位CPU或操作系统的计算机|
|386, x86	|armv5, armv7|mips, mipsle|适用于32位CPU或操作系统的计算机|


![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Copy Document.jpg)

下载之后解压到你想要的地方，为了方便调用我用 `alias` 创建了一个快捷命令 `baidudl` 修改你的 `.bashrc` `.zshrc` 或者其他 `Shell` 配置文件，修改完成之后使用 `source ~/.zshrc` 同步修改(这里拿`zsh`作为例子)

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/addition alias.jpg)

然后就可以使用 `baidudl` 轻松调用了

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Login.jpg)


我们可以使用 `help` 命令获得使用帮助 操作还是比较简单的☺️

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/exmple download.jpg)

速度还是挺不错的，因为我家带宽的原因看起来不算很快但是已经达到峰值了

>注：如果你是 `Windows` 用户直接下载对应的版本解压之后直接双击 `exe` 文件就可以直接运行了，然后你可以创建 `桌面快捷方式` 这样就可以快速打开了









	

	


