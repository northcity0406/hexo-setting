---
title: 创建数据库时设置编码为UTF-8
date: 2017-10-24 08:32:58
tags: [数据库,编码]
copyright: true
categories: "数据库"
---

     更新的数据中有中文，出现如下错误： 
>OperationalError at /admin/blog/post/add/ 
(1366, "Incorrect string value: '\\xE6\\xB7\\xB1\\xE5\\x85\\xA5...' for column 'title' at row 1") 
Request Method: POST 
Request URL: http://localhost:8000/admin/blog/post/add/ 
Exception Type: OperationalError 
Exception Value: 

>(1366, "Incorrect string value: '\\xE6\\xB7\\xB1\\xE5\\x85\\xA5...' for column 'title' at row 1") 

>Exception Location: C:\Python25\Lib\site-packages\MySQLdb\connections.py in defaulterrorhandler, line 35 

       类似这样的错误，应该是数据库表的charset和collation问题。尝试把所有表的charset改为utf-8, collation改为utf8-unicode-ci。如果还是不能解决，最好是重建数据库，然后修改数据库的属性，选择charset为utf-8,collation为utf8-unicode-ci。
**命令行：**
>create database news default charset utf8 collate utf8_unicode_ci;