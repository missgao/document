wheezy 不像前些版本直接安装ia32-libs ia32-libs-gtk就可以，因为从wheezy开始搞了个Multiarch，具体的含义不是特别清楚，主要是英文不好，不过从debian的网站上还是看明白了命令。

以下列出方法

首先加入对应架构支持

    dpkg --add-architecture i386

然后在源中加入对应架构，像我使用的163的源

    deb [arch=amd64,i386] http://mirrors.163.com/debian/ wheezy main

再更新下

    aptitude update

然后就可以安装ia32-libs了

    aptitude install ia32-libs

在安装时可以看到一些包的后面多出了:i386字样
