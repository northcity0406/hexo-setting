---
title: Ubuntu 开启WiFi
date: 2017-10-24 10:40:46
tags: [Ubuntu]
copyright: true
categories: "Ubuntu"
---


# 1.下载hostapd：
>hostapd_1.0-2ubuntu5_amd64.deb
http://old-releases.ubuntu.com/ubuntu/pool/universe/w/wpa/

安装： 

>sudo dpkg -i hostapd_1.0-2ubuntu5_amd64.deb
sudo apt-mark hold hostapd   保留当前hostapd ,使其不被升级



# 2.下载ap-hotspot:
>ap-hotspot_0.2.1-1-webupd8-1_all.deb 
https://launchpad.net/~nilarimogard/+archive/ubuntu/webupd8/+packages
在packet name contains 输入 "ap-" ,点击Filter,然后下载相应的软件

安装：
>sudo dpkg -i ap-hotspot_0.2.1-1-webupd8-1_all.deb 

# 3.网络配置

# 4.网络开启