---
title: Vim 开启python/python3支持
date: 2017-10-24 08:36:57
tags: [Ubuntu,vim]
copyright: true
categories: "Python"
---

###1 . 检查vim是否支持python
![检查vim是否支持python.png](http://upload-images.jianshu.io/upload_images/3732745-ad6b39d12a66d078.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
经检查，发现vim不支持python2
###2. 下载vim8源码
```
git clone https://github.com/vim/vim.git
```
###2 . 编译安装vim8
```
cd vim/src
make clean
./configure --with-features=huge --enable-python3interp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-multibyte --enable-cscope      --prefix=/usr/local/vim/
sudo make install
```
备注说明:
>--with-features=huge：支持最大特性
--enable-rubyinterp：打开对ruby编写的插件的支持
--enable-pythoninterp：打开对python编写的插件的支持
--enable-python3interp：打开对python3编写的插件的支持
--enable-luainterp：打开对lua编写的插件的支持
--enable-perlinterp：打开对perl编写的插件的支持
--enable-multibyte：打开多字节支持，可以在Vim中输入中文
--enable-cscope：打开对cscope的支持
--with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ 指定python 路径
--with-python-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/ 指定python3路径
--prefix=/usr/local/vim：指定将要安装到的路径(自行创建)

如果出现问题请自得安装python-dev 再执行上面命令
>sudo apt-get install python-dev
sudo apt-get install python3-dev
sudo apt-get install libncurses5-dev