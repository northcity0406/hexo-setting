---
title: 创建第一个Python Django项目
date: 2017-10-24 08:26:28
tags: [django,python,Web]
copyright: true
categories: "python"
---

      如果这是你第一次使用Django，你需要完成一些初始化设置。 你需要自己用代码来创建一个Django项目——一个Django框架开发的网站，创建项目后我们需要的配置的东西，包括数据库的配置、针对Django的配置选项和app的配置选项。
>cd Workspace/Python/PythonWeb         #进入Django的开发目录
>django-admin.py startproject newsite      #使用命令创建第一个Django项目
>tree
![第一个项目创建的过程.png](http://upload-images.jianshu.io/upload_images/3732745-30c81df596b2e661.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<br>
**备注:**

>**外层的newsite/**
根目录仅仅是项目的一个容器。它的命名对Django无关紧要；你可以把它重新命名为任何你喜欢的名字。

<br>
>**manage.py**：
一个命令行工具，可以使你用多种方式对Django项目进行交互。你可以在[**django-admin和manage.py**](http://python.usyiyi.cn/documents/django_182/ref/django-admin.html)中读到关于manage.py
的所有细节。

<br>
>**内层的newsite/**
目录是你的项目的真正的Python包。它是你导入任何东西时将需要使用的Python包的名字（例如 newsite.urls）。

<br>
>**newsite/__init__.py**
一个空文件，它告诉Python这个目录应该被看做一个Python包。 （如果你是一个Python初学者，[**关于包的更多内容**](https://docs.python.org/tutorial/modules.html#packages)请阅读Python的官方文档）。

<br>
>**newsite/settings.py**
该Django 项目的设置/配置。[**Django 设置**](http://python.usyiyi.cn/documents/django_182/topics/settings.html) 将告诉你这些设置如何工作。

<br>
>**newsite/urls.py**
该Django项目的URL声明；你的Django站点的“目录”。 你可以在[**URL 转发器**](http://python.usyiyi.cn/documents/django_182/topics/http/urls.html) 中阅读到关于URL的更多内容。

<br>
>**newsite/wsgi.py**
用于你的项目的与WSGI兼容的Web服务器入口。 更多细节请参见[**如何利用WSGI进行部署**](http://python.usyiyi.cn/documents/django_182/howto/deployment/wsgi/index.html)。