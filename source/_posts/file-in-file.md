---
title: file_in_file
date: 2017-10-23 21:35:22
tags:
	- python
	- 算法设计
categories: "千里码"
copyright: true
---
[File in File](http://www.qlcoder.com/task/766e)
>互联网应用经常需要存储用户上传的图片，比如facebook相册。
facebook目前存储了2600亿张照片，总大小为20PB，每张照片约为80KB。用户每周新增照片数量为10亿。（总大小60TB），平均每秒新增3500张照片（3500次写请求），读操作峰值可以达到每秒百万次。
考虑到一台标配的服务器的硬盘是10TB，理论上可以存 10TB/80KB=1.3亿张左右的照片。
然而linux服务器的文件索引的设计最多只支持500w左右的文件数，如果超过500w，性能会大幅下降。
在普通的linux文件系统中，读取一个文件包括三次磁盘io:首先读取目录元数据到内存，其次把文件中的inode节点装载到内存，最后读取实际的文件内容。由于小文件个数太多，无法将所有的目录以及文件的inode信息缓存到内存，因此磁盘IO次数很难达到每个图片读取只需要一次磁盘IO的理想状态。
因此，facebook的图片存储系统haystack设计采用的思路是: 多个逻辑图片文件共享一个物理文件。
1个物理文件的大小=32MB。因此linux服务器中的文件个数在10TB/32MB=1024*1024/32=327680..远远小于linux服务器的文件索引的阈值。
照片文件在物理文件中的存放为依次的顺序存放。每个照片文件的存放规格如下:
1字节的标记位。0代表接下来的照片仍然可用，1代表接下来的照片已经被删除，2代表该物理文件接下来已经没有图片了。
4字节的size。标记照片的大小x。
x字节，照片文件本身。
这里有一块遵循该规格的[物理文件](http://www.qlcoder.com/download/rf.data),里面存放着诺干张图片，正确的拿到图片即可看到本题的答案。


```
f = open('rf.data','rb')
location = 0
cnt = 0
while True:
    f.seek(location,0)
    flag_str = f.read(1)
    hexstr = "%s" % (flag_str.encode('hex'))
    flag = int(hexstr, 16)
    print flag
    if flag == 0:
        f.seek(location + 1,0)
        size_tmp = f.read(4)
        hexstr = "%s" % (size_tmp.encode('hex'))
        size = int(hexstr, 16)
        print size
        f.seek(location + 5, 0)
        image = f.read(size)
        im_txt = "%d.jpg" %(cnt)
        d = open(im_txt,'wb')
        print im_txt
        d.write(image)
        d.close()
        location += 5 + size
    elif flag == 1:
        f.seek(location + 1, 0)
        size_tmp = f.read(4)
        hexstr = "%s" % (size_tmp.encode('hex'))
        size = int(hexstr, 16)
        f.seek(location + 5, 0)
        location += 5 + size
        print size
    elif flag == 2:
        break
    cnt += 1
```