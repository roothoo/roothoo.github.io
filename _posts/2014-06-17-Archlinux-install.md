---
layout: post
title:  "Archlinux硬盘安装及配置"
date:   2014-06-17 23:25:02
categories: linux 
tags: [linux, archlinux]
---

参考资料：[硬盘安装 Arch Linux](http://blog.fooleap.org/hard-disk-installation-for-archlinux.html)  
[安装ArchLinux 2014.01.05 过程记录](http://hi.baidu.com/ocnzhao/item/650334c7b578e9105050585f)  
[ archlinux 2014.03.01 硬盘安装 win + grub4dos + arch ](http://blog.csdn.net/xiaoyanghuaban/article/details/22613987)  


## U盘安装Archlinux

#### 准备工作
下载镜像文件`archlinux-2014.06.01-dual.iso`，并放入u盘根目录

####  在U盘上安装`Grub2`
参考资料：[如何在Linux制作启动U盘（使用grub或grub2）](http://www.lsychina.com/howto_make_bootable_usb_disk_using_grub_or_grub2.html)  
假设u盘是`fat32`分区

```bash
# 创建一个目录用于挂载
mkdir /mnt/udisk/     
# 挂载U盘
mount /dev/sdb1 /mnt/udisk/ 
# 安装grub2
grub-install --force --no-floppy --root-directory=/mnt/udisk/ /dev/sd1     
```
grub2安装好后，在u盘存在一个`/boot/grub/`的目录,在此目录下新建引导配置文件`grub.cfg`：

```bash
menuentry "archLinux ISO" {
	loopback loop /archlinux-2014.06.01-dual.iso
	linux (loop)/arch/boot/i686/vmlinuz archisolabel=ARCH_201406
	initrd (loop)/arch/boot/i686/archiso.img
	boot
}

menuentry "System shutdown" {
    echo "System shutting down..."
    halt
}

menuentry "System restart" {
    echo "System rebooting..." 
    reboot
}

```
启动过程提示找不到，得到一个 Shell，进行下面的操作

```bash
$ mkdir/udisk
$ mount -r -t vfat /dev/sdb1 /udisk
$ modprobe loop
$ losetup /dev/loop6 /udisk/archlinux-2014.06.01-dual.iso
$ ln -s /dev/loop6 /dev/disk/by-label/ARCH_201406
$ exit
```

## 安装基本系统

##### 硬盘分区

```bash
$ mkfs.ext4 /dev/sda6
$ mount /dev/sda6 /mnt
```

##### 设置网络

```bash
wifi-menu wlp2s0
```
选择软件源：

```bash
$ vi /etc/pacman.d/mirrorlist
```

```bash
Server = http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = http://mirrors.163.com/archlinux/$repo/os/$arch

```

```bash
#更新软件源
$ pacman -Sy
```

##### 安装系统

```bash
#通过 pacstrap 安装基本系统
$ pacstrap -i /mnt base base-devel

#生成 fstab 配置
$ genfstab -U -p /mnt >> /mnt/etc/fstab

# chroot 到刚安装的新系统
$ arch-chroot /mnt

#现在已经根目录已经切换到刚安装的系统，所进行的一切操作也即是对新系统的。

#安装 Grub
$ pacman -S grub-bios
$ grub-install --recheck /dev/sda
$ grub-mkconfig -o /boot/grub/grub.cfg

#修改40_custom文件，增加启动xp选项
$ vi /etc/grub.d/40_custom

#增加如下内容：
menuentry "Boot XP"{
	insmod ntfs
	set root=(hd0,1)
	drivemap -s (hd0) ${root}
	chainloader +1
	boot
}

#更新grub.cfg
$ grub-mkconfig -o /boot/grub/grub.cfg
```

## 配置系统

##### 配置语言 
修改 Locale，定义用户所使用的语言及字符集:

```bash
$ vi /etc/locale.gen
```

```bash
en_US.UTF-8 UTF-8
zh_CN.GB18030 GB18030
zh_CN.GBK GBK
zh_CN.UTF-8 UTF-8
zh_CN GB2312
```

```bash
$ locale-gen
$ echo LANG=zh_CN.UTF-8 > /etc/locale.conf
$ export LANG=zh_CN.UTF-8
```

##### 配置网络

```bash
$ pacman -S wirelesstools wpasupplicant wpa_actiond dialog
$ wifi-menu wlp2s0
$ systemctl enable net-auto-wireless.service
```

```bash
# 配置Yaourt软件源
# vi /etc/pacman.conf
[archlinuxfr]
SigLevel = Optional TrustedOnly
Server = http://repo.archlinux.fr/$arch
```

```bash
#安装Yaourt,方便未进官方源软件的安装
$ pacman -Sy yaourt
```

##### 配置声音

```bash
#安装 ALSA，驱动声卡
$ pacman -S alsa-utils

#使用 alsamixer 调整声音或通过下面命令来取消静音
$ amixer sset Master unmute

#测试以判断声卡是否工作
$ speaker-test -c 2
```

##### 配置显示

```bash
#安装 Xorg
$ pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils
$ pacman -S mesa
$ pacman -S xf86-video-intel lib32-intel-dri xf86-video-vesa

#安装Xorg测试环境：
$ pacman -S xorg-twm xorg-xclock xterm
#测试X环境，启动X会话:
$ startx
#退出X：
$ exit

#配置触摸板
$ pacman -S xf86-input-synaptics

#安装字体
$ pacman -S wqy-bitmapfont wqy-zenhei wqy-microhei ttf-dejavu

#安装桌面环境，这里选KDE
$ pacman -S kde
#安装KDE workspace
$ pacman -S kdebase-workspace

#vi ~/.xinitrc
exec startkde

#测试KDE
$ startx

$ pacman -S kdemultimedia-kmix #音量调节 
$ pacman -S kdeplasma-applets-plasma-nm #网络管理 
$ systemctl enable NetworkManager.service #开启网络管理服务 

#开机自动运行KDM(KDE Display Manager)
$ systemctl enable kdm.service

#允许root用户在KDM登录
$ sed -ie 's/AllowRootLogin=false/AllowRootLogin=true/' /usr/share/config/kdm/kdmrc
```

##### fcitx输入法

```bash
$ pacman -S fcitx fcitx-gtk2 fcitx-gtk3 fcitx-qt kcm-fcitx fcitx-googlepinyin
```
新建`~/.xprofile`,添加如下内容：

```bash
export XMODIFIERS=@im=fcitx 
export GTK_IM_MODULE=fcitx 
export QT_IM_MODULE=fcitx
```

##### 安装Firfox和Flash插件

```bash
$ pacman -S firefox flashplugin
```

##### 安装 Zathura

```bash
$ pacman -S zathura zathura-pdf-mupdf
```

>(end)
