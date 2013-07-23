---
layout: post
title: 64位debian使用firefox
---
主要解决相应wrong ELF class: ELFCLASS64错及插件异常

不多描述，直接放方法：

首先要有安装ia32-libs、lib32-libs-gtk，因为firefox只有32位的，但好像他默认是去读32位下对应的so文件，虽然我们装了32位Lib它也不去找。

说明下我的情况：我直接在mozilla上下载的firefox，然后放在了/usr/local下，这时启动firefox是要用完整路径才可以。

方法：创建/usr/local/bin/firefox，内容如下

    #!/bin/bash -e
    export LD_LIBRARY_PATH=/usr/lib32:$LD_LIBRARY_PATH
    export GTK_PATH=/usr/lib32/gtk-2.0
    /usr/local/firefox/firefox "$@" 2>&1 > /dev/null &

具体原因不再解释，懂的自然懂，不懂的解决问题即可

  
下面描述下flash插件问题：

我测试的情况如下，在adobe网站下载linux的64不可用。我纠结了一两天，后来下载了32位的竟然可以了。当然上面的步骤需要先做一下，上面的步骤如果没有进行也是可以启动firefox的，我是在处理支付宝密码输入控件时想到的，当时支付宝默认给我装的是64控件，结果也有报ELFCLASS64错，然后我将支付宝的控件也换做32的竟然可以了。

  
附：支付宝的控件默认会根据你的系统环境会安装控件，所以装的是64的，想装32的只需做一个简单的改动：得到对应安装文件先不要执行，而修改下，将里边删除\~/.aliedit语句删除，然后前往\~/.aliedit/install/lib下看，会看到一个64一个32的lib，对应复制到\~/.mozilla/plugins下即可
