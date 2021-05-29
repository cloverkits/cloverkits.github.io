---
title: "ArchLinx 安装教程 [图\U0001F430文]"
date: 2018-08-20 23:37:20
categories:
- ArchLinux
tags:
- Linux
- 折腾
- 教程
---
# ArchLinux 安装教程

![archlinux-logo](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/archlinux-logo.png)

## 启动
![bootmenu](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/bootmenu.png)

从`安装介质`启动 `ArchLinux` 并选择 `ArchLinux archlinuxiso x86_64 UEFI CD` 引导启动
> PS:
> 当前假设你以 `UEFI` 方式引导 Arch Linux
> 如果你是 `Legacy BIOS` 方式界面会稍有不同

![boot-finish](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/boot-finish.png)

当屏幕出现`命令提示符`及闪烁的`光标`即启动完毕

## 设置 Arch ISO

### 网络连接测试

通过 ping 命令测试网络连通性

```bash
$ ping -c 4 qq.com
```

![pingtest](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/pingtest.png)

### 同步时间

同步时间以确保时间准确无误

```bash 
$ timedatectl set-ntp true
```

### 选择软件源

由于你知道的原因在天朝访问一些国外网站的速度可想而知，选择合适的软件源头以获得更高的下载速度

```bash
$ vim /etc/pacman.d/mirrorlist
```

打开`镜像源`列表 搜索 `China` 然后把需要的`镜像源`地址剪切到文件的最前面即可

![changemirrorlist](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/changemirrorlist.png)

>中国大陆用户可使用以下命令选取大陆镜像服务器。
```bash
$ sed -i '/China/!{n;/Server/s/^/#/};t;n' /etc/pacman.d/mirrorlist
```

## 启动环境检测 
### UEFI/BIOS 检测

```bash
$ ls /sys/firmware/efi/efivars
```

![envcheck](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/envcheck.png)

如果出现上图执行结果则表明 `ArchISO` 以 `UEFI` 模式启动反之则是 `Legacy BIOS` or `CSM` 兼容模式

## 分区

> UEFI 启动对应 `GUID` 分区表 `Legacy BIOS` 方式启动对应 `MBR`分区表

### 分区方案
- 一般而言对于 `Linux` 需要至少需要一个分区分配给`根目录` `/`
- 对于 `UEFI` 则额外需要一个 `UEFI` 系统分区 `ESP` 通常也叫做 `EFI` 分区

### 分区操作

你可以使用 `fdisk -l` or `lsblk` 查看和确定目标磁盘或分区

```bash
$ fdisk -l
## 或者
$ lsblk
```

![lsblk-map](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/lsblk-map.png)

使用 `cfdisk` 对目标磁盘进行分区操作 [推荐 `cfdisk`]

```bash
$ cfdisk /dev/sdX       # sdX 为目标磁盘
```

![cfdisk](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/cfdisk.png)

由于在虚拟机中作为演示我只分配了 20G 的磁盘空间作为分割，我分了三个分区分别是：

> EFI System ( ESP 分区) 
> /boot     （用于存储启动文件和内核）
> /         （根分区 也就是上面提到的必须要分配的）

对于 `ESP` 分区 300MB 就差不多够了，`boot` 分区因为内核更新新旧版本残留可能会占用一些空间可以酌情分配

然后使用格式化工具进行格式化操作，推荐使用 `ext4` 文件系统 （一般都是使用这个经历了时间的考验）`ext4` 的格式化工具是 `mkfs.ext4`

```bash
$ mkfs.ext4 /dev/sdXY    # sdXY 为目标分区
```
依次格式化你分配的分区 `/boot` `/` 如有其他需要也可以分配 `/home` 等，根据实际情况自行判断

![format](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/format.png)

>对于 `ESP` 分区 使用 `msfs.fat` 进行格式化 不然会导致 `UEFI BIOS` 无法获取启动引导文件
>![format-esp](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/format-esp.png)

### 挂载分区
>挂载需要按照一定的顺序进行

```bash
$ mount <分区> <挂载点>
```

![mount-part](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/mount-part.png)

## 安装基础系统

使用 `pacstrap` 脚本，安装 `base` 组

```bash
$ pacstrap -i /mnt base linux linux-firmware dhcpcd
##  如果需要其他软件 比如 `base-devel`
$ pacstrap -i /mnt base base-devel linux linux-firmware dhcpcd
```

## 配置基础系统

### 生成分区表
> 生成只会可以手动 `cat` 查看生成是否有误

```bash
$ genfstab -U /mnt >> /mnt/etc/fstab
```

使用 `arch-chroot` 进入刚安装好的基础系统进行配置操作

```bash
$ arch-chroot /mnt
```

### 修改时区

假设你在中国可以使用 `Asia` `Shanghai` 作为时区位置

```bash
$ ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 硬件时间设置

```bash
$ hwclock --systohc
```

### 区域设置 (Region)

通过编辑 `/etc/locale.gen` 将需要的 `locale` 取消注释即可 (行头的 `#` 号)

然后使用命令 `locale-gen` 生成 `locale`

> 我一般只选择：
> en_US.UTF-8 UTF-8
> zh_CN.UTF-8 UTF-8
> zh_TW.UTF-8 UTF-8

#### 设置默认 `locale`

```bash
$ echo LANG=en_US.UTF-8  > /etc/locale.conf
```
>PS:
>我建议使用 `en_US` 作为默认的 `locale` 因为这样不造成 `tty` 乱码，而且一般遇到问题也可以凭借`嘤文`错误信息在 `搜索引擎` 中更加方便的找到解决方案

### 设置主机名

```bash
$ echo <主机名> > /etc/hostname
## 例如
$ echo ArchLinux > /etc/hostname
```

然后在 `/etc/hosts` 添加

```bash
127.0.0.1	localhost.localdomain   localhost
::1		   localhost.localdomain   localhost
127.0.1.1	<主机名>.localdomain <主机名>
```

## 网络配置

* 有线连接

```bash
$ systemctl enable dhcpcd
```

* 无线连接

```bash
$ pacman -S iw wpa_supplicant dialog
```

## Initramfs

> 一般情况下你不需要创建 `initramfs` 如果你修改了 `mkinitcpio.conf` 则用下面的命令生成 `initramfs`

```bash
$ mkinitcpio -P
```

>PS：
>保险起见我建议还是执行一下为妙 

## 设置 Root 密码

```bash
$ passwd
```

## 安装引导程序

一般通常使用 `Grub` 作为引导程序

* BIOS 系统

```
$ pacman -S grub os-prober
$ grub-install --target=i386-pc /dev/sdX    # sdX 为目标磁盘
$ grub-mkconfig -o /boot/grub/grub.cfg
```

* UEFI 系统

```bash
$ pacman -S dosfstools grub efibootmgr
$ grub-install --target=x86_64-efi --efi-directory=<EFI 分区挂载点> --bootloader-id=grub
$ grub-mkconfig -o /boot/grub/grub.cfg
```

用我的虚拟机做示例：


```bash
$ pacman -S dosfstools grub efibootmgr
$ grub-install --target=x86_64-efi --efi-directory=/boot/EFI --bootloader-id=archlinux
$ grub-mkconfig -o /boot/grub/grub.cfg
```

## 完成安装
使用 `exit` 或者 `Ctrl + D` 返回安装环境

卸载分区

```bash
$ umount -R /mnt
```

重启：

```bash
$ reboot
```
> 记得移除安装介质

## 用户管理
> 日常不建议使用 `root` 用户
> 虽然我 `VPS` 用惯了敲命令老是不加 `sudo` 烦躁了就直接换成 `root` 随便浪😂
### 添加用户

使用 `useradd` 添加用户，具体详细使用方面请自行找男人(manual) 或者 `help` 帮助信息

```bash
$ useradd -m -g users -s /bin/bash cloverkit
```
>创建一个名为 cloverkit 的用户，指定登录 shell 为 bash，所属主用户组 users，并在 /home 下创建同名用户文件夹。

```bash
passwd cloverkit
```
> 为 `cloverkit` 设置用户密码

### 普通用户提权配置
> 使用刚创建的用户登陆系统，你会发现进行一些需要`提权`的操作即使加入 `sudo` 也无法 `提权`，我们可以通过 `visudo` 修改配置文件将我们的普通用户加入`提权`列表

![visudo](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/visudo.png)

大概是在第 `79` 行 是 `root` 用户代配置，我们只需要比葫芦画瓢就行在其下方添加如下 `:wq` 保存即可
```
<用户名> ALL=(ALL) ALL
```


![arch-neofetch](https://raw.githubusercontent.com/cloverkits/hexo_picture_resource/master/picture/archlinux-neofetch.png)


## 参考资料
>[官方 Wiki | Installation guide (简体中文)](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
>[Arch Linux 安装指南[2018.03.01]](https://bbs.archlinuxcn.org/viewtopic.php?id=1037)