---
title: Ubuntu系统CPU、内存、网络、进程监控工具
date: 2017-10-23 21:41:04
tags: [Ubuntu,CPU,Memory]
copyright: true
categories: "Ubuntu"
---
# 1.1 Top
>作用：查看在系统中运行的进程或线程,以及CPU、内存、交换分区等
>运行方法：`sudo top`
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-eef76f173393d97b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一行：

参数 | 解释
----|------
22:14:44  |当前系统时间                                                                   
57min        |系统已经运行了57分钟                                                    
1 users      |当前有1个用户登录系统                                                   
load average: 0.96, 0.74, 0.58|是1分钟、5分钟、15分钟的负载情况

第二行：

参数 | 解释
----|------
257 total |当前系统有257个进程                                                             
1 running       |一个进程正在运行                                            
240 sleeping      |240个进程在休眠                                                  
13 stopped |13个进程停止运行
3 zombie |3个zombie状态（僵尸）

第三行：cpu状态

参数 | 解释
----|------
22.8 us|用户空间占用CPU的百分比。
5.2 sy | 内核空间占用CPU的百分比。
0.0  ni | 改变过优先级的进程占用CPU的百分比
71.6 id |空闲CPU百分比
0.3 wa |IO等待占用CPU的百分比
0.0 hi| 硬中断（Hardware IRQ）占用CPU的百分比
0.0 si |软中断（Software Interrupts）占用CPU的百分比

第四行：内存状态

参数 | 解释
----|------
16326320 total |物理内存总量
2797296 used |使用中的内存总量
11844292 free | 空闲内存总量
1684732 buff/cache |缓存的内存量 （79M）

第五行：swap交换分区

参数 | 解释
----|------
0 total |  交换区总量（2GB）
0 used |  使用的交换区总量（2.5M）
0 free | 空闲交换区总量（2GB）
13082364 avail Mem |  缓冲的交换区总量


第七行以下：各进程（任务）的状态监控



参数 | 解释
----|------
PID | 进程id
USER | 进程所有者
PR | 进程优先级
NI | nice值。负值表示高优先级，正值表示低优先级
VIRT | 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES | 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
SHR | 共享内存大小，单位kb
S | 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
%CPU | 上次更新到现在的CPU时间占用百分比
%MEM | 进程使用的物理内存百分比
TIME+ | 进程使用的CPU时间总计，单位1/100秒
COMMAND | 进程名称（命令名/命令行）



# 1.2 Htop
>作用：查看在系统中运行的进程或线程,以及CPU、内存、交换分区等
>运行方法：`sudo htop`
>运行结果演示：
![](http://upload-images.jianshu.io/upload_images/3732745-468f5a61dd1c89ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

左上角:CPU、内存、交换分区

参数 | 值|解释
----|------|----
1 |  14.9%|CPU1的占用百分比
2 |  16.0%|CPU2的占用百分比
3 |  16.1%|CPU3的占用百分比
4 |  17.4%|CPU4的占用百分比
Mem |  2.95G/15.6G|内存的使用量与总量
Swp |  0K/0K|交换分区的使用量与总量

右上角:任务、线程

参数 | 解释
----|------
Task 163,359 thr;6 running |总的进程、线程数，正在运行的进程数                                                                   
load average:0.43 0.67 0.68        |是1分钟、5分钟、15分钟的负载情况                                                                                       
uptime 01:22:38|系统运行时间

下方的解释详见 # 1.1 Top

# 1.3 atop
>介绍：atop 和 top，htop 非常相似，它也能监控所有进程，但不同于 top 和 htop 的是，它可以按日记录进程的日志供以后分析。它也能显示所有进程的资源消耗。它还会高亮显示已经达到临界负载的资源。
>作用：查看在系统中运行的进程或线程,以及CPU、内存、交换分区等
>运行方法：sudo atop
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-a59212fcfb11a42e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.4 powertop
>作用：powertop 可以帮助你诊断与电量消耗和电源管理相关的问题。它也可以帮你进行电源管理设置，以实现对你服务器最有效的配置。
>运行方法：`sudo powertop`
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-c99a0ac232873c8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.5 iotop
>作用：iotop 用于检查 I/O 的使用情况，并为你提供了一个类似 top 的界面来显示。它按列显示读和写的速率，每行代表一个进程。当发生交换或 I/O 等待时，它会显示进程消耗时间的百分比。
>运行方法：`sudo iotop`
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-801890aee9b5c3c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.6 iftop
>作用：iftop 类似于 top，但它主要不是检查 cpu 的使用率而是监听所选择网络接口的流量，并以表格的形式显示当前的使用量。
>运行方法：sudo iftop
>结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-c03965dd07e4f37f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.7 jnettop
>作用：jnettop 以相同的方式来监测网络流量但比 iftop 更形象。它还支持自定义的文本输出，并能以友好的交互方式来深度分析日志。
>运行：sudo jnettop
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-23282d84b5aa306e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.8 BandwidthD
>作用:BandwidthD 可以跟踪 TCP/IP 网络子网的使用情况，并能在浏览器中通过 png 图片形象化地构建一个 HTML 页面。它有一个数据库系统，支持搜索、过滤，多传感器和自定义报表。
>[运行结果演示（外链）](http://bandwidthd.sourceforge.net/demo/)

# 1.9 NetHogs
>作用：NetHogs 打破了网络流量按协议或子网进行统计的惯例，它以进程来分组。所以，当网络流量猛增时，你可以使用 NetHogs 查看是由哪个进程造成的。
>运行方法：sudo Nethogs
>运行结果演示：![](http://upload-images.jianshu.io/upload_images/3732745-5b4d18c78abae1ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 1.10 dstat
>作用：dstat 旨在替代 vmstat，iostat，netstat 和 ifstat。它可以让你查实时查看所有的系统资源。这些数据可以导出为 CSV。最重要的是 dstat 允许使用插件，因此其可以扩展到更多领域。