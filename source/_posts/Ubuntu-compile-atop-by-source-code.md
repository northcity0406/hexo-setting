---
title: Ubuntu使用源码编译安装atop
date: 2017-10-24 08:11:37
tags: [Ubuntu,监控工具]
copyright: true
categories: "Ubuntu"
---

# 1.官网：
>http://www.atoptool.nl/downloadatop.php

# 2.下载解压编译

>wget http://www.atoptool.nl/download/atop-2.2-3.tar.gz
tar xvf atop-2.2-3.tar.gz
cd atop-2.2-3
make prefix=/usr/local/atop install

# 3.warnning
>（报错rawlog.c:154:18: fatal error: zlib.h: No such file or directory，yum -y install zlib未解决，因为只安装lib没有头文件，提示需要zlib头文件）

# 4.解决方案：
>标准安装zlib
官网: http://www.zlib.net/
找下载链接：http://zlib.net/zlib-1.2.8.tar.gz 
或者http://iweb.dl.sourceforge.net/project/libpng/zlib/1.2.8/zlib-1.2.8.tar.gz
cd ../
wget http://zlib.net/zlib-1.2.8.tar.gz
tar xzf zlib-1.2.8.tar.gz
cd zlib-1.2.8
##创建动态库
>./configure --shared
make test
make install
cp zutil.h /usr/local/include
cp zutil.c /usr/local/include


# 5.重新编译
>cd atop-2.2-3
make clean
make install

提示Choose either 'make systemdinstall' or 'make sysvinstall' #关于采用哪种方式安装可以参考这里：http://www.linuxdiyf.com/linux/10711.html
>make systemdimstall



# 6.运行
>atop

# 7.退出
>参考：http://my.oschina.net/nox/blog/221014
http://blog.sina.com.cn/s/blog_714dacd10102v6et.html

