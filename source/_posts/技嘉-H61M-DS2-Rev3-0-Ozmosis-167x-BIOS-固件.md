---
title: 技嘉 H61M-DS2 Rev3.0 Ozmosis 167x BIOS 固件
date: 2018-08-07 11:39:46
categories: 
- Ozmosis
tags: 
- 折腾
---

![](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/about-macOS.png)

之前在逛论坛的偶然时候发现了 `Ozmosis` 这个引导程序
刚开始我也没在意就是挺好奇的，一直想尝试一下
毕竟 `Clover EFI Bootloader` 早已经用腻了

由于 `Ozmosis` 的模块需要插入到BIOS固件中风险比较高
我一直想使用别人做好的固件 但是几度 `Google` 未果
便萌生了自己修改 BIOS 固件的想法

得益于技嘉主板的 `Dual BIOS` 技术我几乎不用担心刷写 `BIOS` 所带来的潜在风险

------

由于 `BIOS` 芯片的容量只有 4MB 我不得不精简掉一些组件
来插入最基本的 `Ozmosis` 模块 也因为容量的原因额外的驱动不得不需要放置到 `EFI` 分区的特定目录下

 > 主要精简了以下组件:
 > Filesystem
 > TcpDxe
 > DhcpDxe
 > Ip4ConfigDxe
 > Ip4Dxe
 > Mtftp4Dxe
 > Udp4Dxe
 > Dhcp6Dxe
 > Ip6Dxe
 > Mtftp6Dxe
 > Udp6Dxe

  

 ![Screen Shot 2018-07-07 at 01.19.03.png](http://bbs.pcbeta.com/data/attachment/forum/201807/07/114045vl2cdl80e22dni3n.png)

以上除了 `Filesystem` 外均为网络组件是网络启动使用的底层驱动程序一般用户用不到 ~~很遗憾因为 `apfs.efi` 驱动太大了 在保证足够稳定的情况下 `apfs.ffs` 是塞不进去的 因而无法支持 `APFS` 文件系统安装 `macOS` 时请选择 macOS日志式进行安装~~

Edit:

经测试通过添加 `ApfsDriverLoader.ffs` 可以成功驱动 `APFS` 文件系统因为 `Ozmosis` 并不会自动创建 `APFS` 容器里所安装 `macOS` 的引导项我们需要手动添加它

Method：
* 添加一个 `UEFI Shell` (可以使用 CLOVER 自带的)
* 使用以下命令手动添加引导

```uefi
bcfg boot addp 0 fsx:\System\Library\CoreServices\boot.efi "你想要的名称"
```
注： `fsx` 为你安装 `macOS`的驱动器编号请自行更改
通过 Shell 命令可以查看修改引导项 添加额外的驱动可以使用 `help -b` 查看帮助信息

Working:

- 网卡
- 显卡
- 声卡
- 休眠 / 唤醒


