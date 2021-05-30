---
title: 使用 Github 搭建免费图床
date: 2018-08-17 23:15:41
categories: 工具
tags:
- 教程
- GitHub
- Hexo
---

## 前言
最近尝试着使用 `GitHub` + `Hexo` 搭建了一个个人博客，之所以选择这样的方案主要是因为 `免费` 还有就是对于 `Hexo` 的好奇吧！使用 `GitHub Page` 服务可以免除一些麻烦同时得益于 `Git` 强大的版本控制，我可以随时追溯到任何一个版本；后来在实际维护博客的时候我发现了一个问题：“我的那些图片该怎么办？！” 

<!-- more -->

经过一番搜索 🔍 在网上看到的推荐大都是使用 `七牛云` `imgur` 等服务或者现成的图床，因为你知道的原因 `Imgur` `Google Photos` 等服务在天朝是无法使用的，而 `七牛云` 则需要`实名认证` 我不是很愿意提供自己的身份信息（还有就是因为懒）就放弃了这个方案。

就在今天我实在受不了了，决定拿出`身份证`准备`实名认证`的同时，我突然想起来既然 `Blog` 都托管到 `GitHub` 了那图片为什么不放到 `GitHub` 呢？

于是我发现了这样一个实用的工具 [`gitPic`](https://github.com/zzzzbw/gitPic) 使用它可以非常便捷的使用 `GitHub` 搭建图床

## 依赖

* Java => 8 
* git

> PS: 如果你没有安装 Java 环境那么你打开 `gitPic` 会看到如下提示

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/Java Guid.png)

## 使用

首先在 `GitHub` 创建一个 `Repository` 名称随意(You like jiu OK👌) 其他细节不在赘述。在创建完成之后克隆到本地目录，然后打开 `gitPic` 进行简单的配置就可以使用了

![gitPic](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/gitPic Push.png)

>PS:
1. 项目目录就是你刚克隆到本地的 `Repository` 仓库
2. 图片保存目录可以自定义你可以在本地仓库中新建一个文件夹📁
3. Git 图片路径是在你 `选择图片` 并 `提交上传` 之后生成的我们复制它到博文就可以看到效果了

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/PS1.png)

下面是我的本地 `Repository` 目录结构

![tree](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/document tree.png)

>本文参考：
>[gitPic,利用Github做图床小工具](http://zzzzbw.cn/article/6)
>[gitPic GitHub](https://github.com/zzzzbw/gitPic)