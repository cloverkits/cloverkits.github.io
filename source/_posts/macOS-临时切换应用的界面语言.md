---
title: macOS 临时切换应用的界面语言
date: 2018-08-12 22:48:47
categories:
- Hackintosh & macOS
tags:
- 技巧
- Terminal
---

话说自从 `macOS 10.13.2` 发布之后 `Apple` 给天朝用户带来了独一无二的 `Apple` 式中文体验。让我们领略到了中华文化的博大精深我不由得发出感叹：“这他喵的都是些什么东西？！”

此次更新 `Apple` 为多个应用和功能新增了简体中文名称
你们自行感受下😨😂
<!-- more --> 

>Finder：访达
>Safari：Safari 浏览器
>Dock：程序坞
>Mission Control：调度中心
>Spotlight：聚焦
>Automator：自动操作
>Spotlight 搜索：聚焦搜索
>Handoff：接力
>Nightshift：夜览
>FileVault：文件保险箱
>Power Nap：电能小憩
>AirDrop：隔空投送

Apple：“很惭愧，就做了一点微小的工作，谢谢大家”


因为看起来实在是辣眼睛，笔者直接将 `嘤文` 设置为系统默认语言；但是由次也带来了一些问题，例如我使用 `CrossOver` 安装一些 `Win32` 应用会因为系统语言设置的问题而乱码，最初我还以为是配置除了问题知道我折腾到怀疑人生，才意识到是语言的问题。

还有就是当我打开 `Photoshop` 亦或是 `Office 办公套件` 这类大型专业软件的时候会显得不知所措，很多陌生的词汇看不懂还有像 `Word` `Excel` `PowerPoint` 在处理中英文时存在差异，也会带来不必要的麻烦。

那么我们难道就此退缩了吗？去面对那辣眼睛的特色翻译😂？

当然不可以！我们可以使用 `Terminal` 加入相应的参数来达到临时修改切换应用界面语言的目的 

示例：

```bash
## 以简体中文界面启动 Safari 浏览器
open -a /Applications/Safari.app --args -AppleLanguages '(zh-CN)'

## 以英文界面启动 Safari 浏览器
open -a /Applications/Safari.app --args -AppleLanguages '(en)'
```

当然如果你想以其他语言启动某个应用程序，只需要修改将上述命令最后的地区代码。

常见的有：

>繁体中文（zh-TW)
>日文（ja）
>法文（fr)
>德文（de)

如果你想要始终以某种与系统设置不同的语言启动特定应用，可以用 `defaults write` 命令来修改其默认设置。

具体语法是：

```bash
defaults write [应用的 Bundle ID] AppleLanguages '([要默认显示的语言])'
```

例如

```bash
# 默认以简体中文打开「文本编辑」应用
defaults write com.apple.TextEdit AppleLanguages '(zh-CN)'
```

其中，应用的 `Bundle ID` 可以通过运行 `mdls -name kMDItemCFBundleIdentifier [应用程序路径]` 来查找。

例如：

```bash
# Chrome 浏览器的 Bundle ID
mdls -name kMDItemCFBundleIdentifier /Applications/Google\ Chrome.app
> kMDItemCFBundleIdentifier = "com.google.Chrome"
```

如果不再需要固定应用程序的显示语言，在终端运行：

```bash
defaults delete [应用的 Bundle ID] AppleLanguages
```

参考资料：

* [如何临时修改 macOS 应用的界面语言](https://sspai.com/post/44536)


