---
layout: post
title: 硬盘安装DEBIAN SQUEEZE 64BIT备忘
---

这不是一个安装手册，只是自己安装的一些重点项目记录   
   
 安装前的情况  
 软件：WIN7旗舰版-64bit EN + BT5双系统  
 硬件：LENOVO E46A  
   
 规划安装后情况：  
 WIN7 64+DEBIAN 64，去除BT5.  
 BT5下/home分区保留，安装完后修改debian的fstab再将/home挂载  
   
 所有常用软件放/usr/local,并将/home/local单独分区，防止再重装  
 1.下载DEBIAN 6.0 amd64,放在FAT32或LINUX格式分区下

    http://cdimage.debian.org/debian-cd/6.0.6/amd64/iso-dvd/debian-6.0.6-amd64-DVD-1.iso

2.下载GRUB4DOS所用引导文件，3个全下，放任一位置，建议放C盘根下――boot.img.gz不知做什么用的

    http://ftp.nl.debian.org/debian/dists/squeeze/main/installer-amd64/current/images/hd-media/

3.GRUB4DOS按C命令行,如果引导文件未放在C盘根下或安装有多块硬盘则需要改变(hd0,0)

    kernel (hd0,0)/vmlinuz
    initrd (hd0,0)/initrd.gz
    boot

4.引导不安装在/dev/sda上，而装在/dev/sdaX上，因为目前还是在WINDOWS上工作的多些，还是用WIN7的引导  
 5.安装只安装标准系统，其它的装好再说。   
   
 到这里已经装完了  

6.下载安装nonfree的网卡驱动firmware-iwlwifi\_0.28+squeeze1\_all.deb，我以前装32位DEBIAN时就下过了，这个包不分32位和

    dpkg -i firmware-iwlwifi_0.28+squeeze1_all.deb

7.添加ISO源,ROOT运行

    mkdir /mnt/source
    mount -o loop /<ISOFILEPATH>/debian-6.0.6-amd64-DVD-1.iso /mnt/source
    echo file:///mnt/source squeeze main contrib > /etc/apt/source.list.d/ISO.list

8.安装一些软件，然后把ntfs3g装上（代码不写了）

    aptitude update
    aptitude install vim gcc make ia32-libs ia32-libs-gtk

9.安装图形

    aptitude install xfce4

  
 10.连接WPA2无线:家里的无线ESSID是隐藏的

    aptitude install wireless-tools wpasupplicant
    wpa_passparse ESSID PWD > file
    ifconfig wlan0 up
    iwconfig wlan0 essid "<ESSID>"
    wpa_supplicant -Dwext -iwlan0 -c<file> 2>&1 > /dev/null &
    dhclient wlan0

最后4句我做成了一个sh文件，以后就可以直接运行了

11.添加163源

    echo deb http://mirrors.163.com/debian squeeze main contrib > /etc/apt/source.list.d/163.list
    echo deb http://mirrors.163.com/debian-backports squeeze-backports main contrib >> /etc/apt/source.list.d/163.list

第二行是为了装fcitx用，我平时只有基本的办公用，所以no-free没有加，万一有需要再加好了  
  
 12.安装WQY字体

    aptitude install xfont-wqy

13.安装设置FCITX

    aptitude install fcitx-table-wubi fcitx-table-wbpy

我基本只用五笔，因为会自动依赖，所以就没把fcitx放进去  
创建\~/.xinitrc——将export那行加入.profile也可以,内容为

    #!/bin/sh
    export LC_CTYPE=zh_CN.utf8 >> ~/.profile

    . /etc/X11/Xsession

我的locale是英文(自己EN太差，用这个方法强制学习了)，所以上面这一行一定要加，不然无法使用fcitx  
   
 14.安装OFFICE  
   
 赶上WPS for
Linux出beta版了，就赶快下来用,下载.tar.gz包，因为WPS只有32位，所以还需要做一些操作：  
   

下载最新的libstdc++6:http://packages.debian.org/wheezy/i386/libstdc++6/download

    ar vx <下载的deb文件>
    tar xvf data.tar.gz
    cp ./usr/lib/i386-linux-gnu/libstdc++.so.6.0.17 <WPS解包路径>/office6/libstdc++.so.6

挂载WINDOWS的C盘，然后将面下的字体复制到\~/.fonts下  
   
 MTEXTRA.TTF  simhei.ttf  simsun.ttc  webdings.ttf  WINGDNG2.TTF  
 simfang.ttf  simkai.ttf  symbol.ttf  wingding.ttf  WINGDNG3.TTF

先到这里了，附上一张近照。

![](http://c.hiphotos.baidu.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=a58802b2d833c895a27e987ee12802cd/43a7d933c895d14307e7910672f082025aaf0705.jpg?referer=e0416e668ad4b31ca92ba18b611c&x=.jpg)
