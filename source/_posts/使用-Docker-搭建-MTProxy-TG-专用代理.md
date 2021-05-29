---
title: 使用 Docker 搭建 MTProxy TG 专用代理
date: 2018-08-18 21:48:35
categories:
- docker
- 科学上网
tags:
- Telegram
- 工具
---

## Telegram

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/telegram_background.png)

> Telegram 是一款开源且跨平台的 IM 工具（类似 Whatsapp、Messenger、微信），是我用过所有同类软件中用户体验最好的一个，同时我也是 Telegram 重度用户和开发者。当然，这么好用的工具在天朝是难以访问的。 ---[李钊同学](https://zhuanlan.zhihu.com/p/25219007)

由于你知道的原因，`Telegram` 在天朝默认情况下是无法使用的，当然你可以先打开 `Shadowsocks` 或是 `V2Ray` 等代理软件正常访问国际互联网，然后才可以正常使用 `Telegram` 但是这样就让 `IM`软件失去了一部分使用体验。

前段时间 `俄罗斯`🇷🇺当局因 `Telegram` 公司不提供通讯加密密钥(为了信息审查)为由决定屏蔽 `Telegram` 服务，但最终没有获得成功为此 `俄罗斯`当局还屏蔽了 `Google` `Microsoft` 等公司的云服务器 `IP段` 可谓是用心良苦。`Telegram` 为了对抗封锁开发了 `MTProxy` `Telegram` 专用代理；对于处于同样情况的天朝来说也可以通过它正常使用 `Telegram` 服务

## MTProxy
![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/mtproxy_toVPS.png)
>MTProxy 是在新版本 Telegram 中内置的代理程序
>
>MTProxy的命名，大概和MTProto有关
Telegram 团队使用自己设计的加密协议 MTProto ，并以 30 万美金的高价奖赏漏洞的提交者。Telegram 使用基于 MTProto 的通讯协议。

在新版的 `Telegram` 中 `Proxy` 设置已经新增了 `MTProxy` 支持，我们只需要把相应的地址和密钥填入就能愉快的玩耍了

下面说一下如何使用 `Docker` 快速部署

#### 安装 Docker

###### 使用官方简化命令安装:
```bash
## 适用于 Linux
$ curl -fsSL get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh --mirror Aliyun //使用阿里云镜像源加速

## 启动 Docker
$ sudo systemctl enable docker
$ sudo systemctl start docker

```

###### 部署 MTProxy 官方 Docker 镜像

```bash
$ docker pull telegrammessenger/proxy
$ docker run -d -p<port>:443 --name=mtproto-proxy --restart=always -v proxy-config:/data telegrammessenger/proxy:latest 
## <port> 修改为你想要的端口
$ docker logs mtproto-proxy
## 查看你的链接信息
## 会输出如下信息
####
#### Telegram Proxy
####

[+] Using the secret in /data/secret: 'xxxxxxxxxxxxxxxxxxxxxxxx'.
[*] Final configuration:
[*]   Secret 1: xxxxxxxxxxxxxxxxxxxxxxxx
[*]   tg:// link for secret 1 auto configuration: tg://proxy?server=你的服务器地址6&port=443&secret=xxxxxxxxxxxxxxxxxxxxxxxx
[*]   t.me link for secret 1: https://t.me/proxy?server=你的服务器地址6&port=443&secret=xxxxxxxxxxxxxxxxxxxxxxxx
[*]   Tag: no tag
[*]   External IP: 你的服务器地址
[*]   Make sure to fix the links in case you run the proxy on a different port.
```
>PS:
> 使用 `docker logs mtproto-proxy` 查询到的`链接`
>其中的`端口`是默认的请根据你使用的端口酌情修改

###### 使用 macOS Telegram 客户端作示例
![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/telegram-naive-proxy.png)

> 参考：
> [Docker 从入门到实践 | 安装 Docker](https://yeasy.gitbooks.io/docker_practice/install/)
> [telegrammessenger/proxy](https://hub.docker.com/r/telegrammessenger/proxy/)







