---
title: 配置 V2Ray 和路由器透明代理
date: 2018-08-07 18:37:21
categories: 
- 科学上网
tags: 
- 折腾
---

#### 什么是 V2Ray？
>V2Ray 是 Project V 下的一个工具。
>Project V 是一个包含一系列构建特定网络环境工具的项目，而 V2Ray 属于最核心的一个。
> 官方中介绍Project V 提供了单一的内核和多种界面操作方式。内核（V2Ray）用于实际的网络交互、路由等针对网络数据的处理，而外围的用户界面程序提供了方便直接的操作流程。
> 不过从时间上来说，先有 V2Ray 才有 Project V。 如果还是不理解，那么简单地说，V2Ray 是一个与 Shadowsocks 类似的代理软件，可以用来科学上网（翻墙）学习国外先进科学技术。


下面贴出我使用的配置
使用了 `VMess` + `Mux`  如果需要 `mKCP` `动态端口` 等其他功能请自行参考官方文档

##### 客户端配置

```json
{
    "log": {
        "loglevel": "warning"
    },
    "inbound": {
        "listen": "127.0.0.1",
        "port": 1080,
        "protocol": "socks",
        "settings": {
            "auth": "noauth",
            "udp": true,
            "ip": "127.0.0.1"
        }
    },
    "outbound": {
        "protocol": "vmess",
        "settings": {
            "vnext": [
                {
                    "address": "xxxxxx.xxxx", //你的服务器域名或者地址
                    "port": xxxxx, //你想要的端口 和服务器配置保持一致
                    "users": [
                        {
                            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", //替换成你生成的 UUID 与服务器配置一致
                            "level": 1,
                            "alterId": 64
                        }
                    ]
                }
            ]
        },
        "mux": {
            "enabled": true,
            "concurrency": 8
        }
    },
    "outboundDetour": [
        {
            "protocol": "freedom",
            "settings": {},
            "tag": "direct"
        }
    ],
    "routing": {
        "strategy": "rules",
        "settings": {
            "rules": [
                {
                    "type": "field",
                    "port": "54-79",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "port": "81-442",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "port": "444-65535",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "domain": [
                        "gc.kis.scr.kaspersky-labs.com"
                    ],
                    "outboundTag": "direct"
                },
                {
                    "type": "chinasites",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "ip": [
                        "0.0.0.0/8",
                        "10.0.0.0/8",
                        "100.64.0.0/10",
                        "127.0.0.0/8",
                        "169.254.0.0/16",
                        "172.16.0.0/12",
                        "192.0.0.0/24",
                        "192.0.2.0/24",
                        "192.168.0.0/16",
                        "198.18.0.0/15",
                        "198.51.100.0/24",
                        "203.0.113.0/24",
                        "::1/128",
                        "fc00::/7",
                        "fe80::/10"
                    ],
                    "outboundTag": "direct"
                },
                {
                    "type": "chinaip",
                    "outboundTag": "direct"
                }
            ]
        }
    }
}
```

##### 服务器配置


```json
{
    "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
    },
    "inbound": {
        "port": xxxxx, //你想要的端口
        "protocol": "vmess",
        "settings": {
            "clients": [
                {
                    "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", //你生成的 UUID 
                    "level": 1,
                    "alterId": 64
                }
            ]
        }
    },
    "outbound": {
        "protocol": "freedom",
        "settings": {}
    },
    "inboundDetour": [],
    "outboundDetour": [
        {
            "protocol": "blackhole",
            "settings": {},
            "tag": "blocked"
        }
    ],
    "routing": {
        "strategy": "rules",
        "settings": {
            "rules": [
                {
                    "type": "field",
                    "ip": [
                        "0.0.0.0/8",
                        "10.0.0.0/8",
                        "100.64.0.0/10",
                        "127.0.0.0/8",
                        "169.254.0.0/16",
                        "172.16.0.0/12",
                        "192.0.0.0/24",
                        "192.0.2.0/24",
                        "192.168.0.0/16",
                        "198.18.0.0/15",
                        "198.51.100.0/24",
                        "203.0.113.0/24",
                        "::1/128",
                        "fc00::/7",
                        "fe80::/10"
                    ],
                    "outboundTag": "blocked"
                }
            ]
        }
    }
}
```

##### 路由器透明代理配置文件


```json
{
    "log": {
        "loglevel": "warning"
    },
    "inbound": {
        "listen": "0.0.0.0",
        "port": 1080,
        "protocol": "socks",
        "settings": {
            "auth": "noauth",
            "udp": true,
            "ip": "127.0.0.1"
        }
    },
    "outbound": {
        "protocol": "vmess",
        "settings": {
            "vnext": [
                {
                    "address": "xxx.xxx.xxx.xxx", //你的服务器地址或者域名
                    "port": xxxxx, //你想要的端口号
                    "users": [
                        {
                            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx", //你生成的UUID
                            "level": 1,
                            "alterId": 64
                        }
                    ]
                }
            ]
        },
        "mux": {
            "enabled": true,
            "concurrency": 8
        }
    },
    "inboundDetour":[
        {
           "protocol":"dokodemo-door",
           "port":1099, 
           "settings":{
              "address":"",
              "network":"tcp",
              "timeout":0,
              "followRedirect":true
           }
        }
     ],
    "outboundDetour": [
        {
            "protocol": "freedom",
            "settings": {},
            "tag": "direct"
        }
    ],
    "routing": {
        "strategy": "rules",
        "settings": {
            "rules": [
                {
                    "type": "field",
                    "port": "54-79",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "port": "81-442",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "port": "444-65535",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "domain": [
                        "gc.kis.scr.kaspersky-labs.com"
                    ],
                    "outboundTag": "direct"
                },
                {
                    "type": "chinasites",
                    "outboundTag": "direct"
                },
                {
                    "type": "field",
                    "ip": [
                        "0.0.0.0/8",
                        "10.0.0.0/8",
                        "100.64.0.0/10",
                        "127.0.0.0/8",
                        "169.254.0.0/16",
                        "172.16.0.0/12",
                        "192.0.0.0/24",
                        "192.0.2.0/24",
                        "192.168.0.0/16",
                        "198.18.0.0/15",
                        "198.51.100.0/24",
                        "203.0.113.0/24",
                        "::1/128",
                        "fc00::/7",
                        "fe80::/10"
                    ],
                    "outboundTag": "direct"
                },
                {
                    "type": "chinaip",
                    "outboundTag": "direct"
                }
            ]
        }
    }
}
```

笔者在自己的 `Newifi Mini` 上安装了 `老毛子Pandora固件` 自带 `V2Ray` 扩展程序 经过一番配置终于得以正常使用😂 并开启了 `ChinaDNS` 防止 `DNS` 污染

对于路由器透明代理主要是添加一下代码 `port` 就是透明代理的端口

```json
    "inboundDetour":[
        {
           "protocol":"dokodemo-door",
           "port":1099, 
           "settings":{
              "address":"",
              "network":"tcp",
              "timeout":0,
              "followRedirect":true
           }
        }
     ],
```

其他路由器固件可以参考以上配置自行修改 不保证一定可用
建议 `UUID` 一定要自己生成因为它相当于 `Shadowsocks` 的密码为了安全性切勿直接 `Copy` 别人配置里的
对于 `macOS` 我们可用直接通过 `Terminal` 使用 `uuidgen` 命令轻松生成 `UUID` 其他系统请自行 `Google` 也可以使用在线 `UUID` 网站生成