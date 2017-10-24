---
title: Django分离的urls.py
date: 2017-10-24 08:34:34
tags: [Django,Python,Web]
copyright: true
categories: "Python"
---

前言：在python的Django框架中，当页面过多时，urls也会逐渐变多，太多的urls放在一起会太繁杂，此时需要一种把urls分类的办法。下面就介绍三种方法。

#### 1.初始状态下的urls.py
**目录:**./mysite/urls.py
```
from django.conf.urls import patterns, include, url
from blog import views as blog
urlpatterns = patterns('',
    url(r'^blog/index/$','blog.views.index'),
    url(r'^blog/time/$','blog.viewstime'),
 )
```
#### 2.第一种修改方式
**目录:**./mysite/urls.py
```
from django.conf.urls import patterns, include, url
from blog import views as blog
urlpatterns = patterns('blog.views',
    url(r'^blog/index/$','index'),   #把'blo.views'向上合并
    url(r'^blog/time/$','time'),     #把'blo.views'向上合并
 )
```
#### 3.第二种修改方式
**目录:**./mysite/urls.py
```
from django.conf.urls import patterns, include, url
from blog import views as blog
from testDjango import views as testDjango

#testDjango的所有urls.py
urlpatterns = patterns('testDjango.views',
    url(r'^testDjango/index/$','index'),
    url(r'^testDjango/time/$','time'),
 )

#blog的所有urls.py(追加)
urlpatterns += patterns('blog.views',
    url(r'^blog/index/$','index'),
    url(r'^blog/time/$','time'),
 )
```
#### 4.第三种修改方式(在blog和testDjango的app下新建urls.py)
**目录:**./mysite/urls.py
```
from django.conf.urls import patterns, include, url
from blog import views as blog
from testDjango import views as testDjango
urlpatterns = patterns('',
    url(r'^testDjango/',include('testDjango.urls')),    #注意这里面没有’$‘符号
    url(r'^blog/',include('blog.urls')),                 #注意这里面没有’$‘符号
 )
```
**目录:**./blog/urls.py
```
from django.conf.urls import patterns, include, url
urlpatterns = patterns('blog.views',
    url(r'^index/$','index'),
    url(r'^time/$','time'),
 )
```
**目录:**./testDjango/urls.py
```
from django.conf.urls import patterns, include, url
urlpatterns = patterns('testDjango.views',
    url(r'^index/$','index'),  #注意这里面有’$‘符号
    url(r'^time/$','time'),    #注意这里面有’$‘符号
 )
```