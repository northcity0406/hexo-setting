---
title: Django setting详解
date: 2017-10-24 08:24:47
tags: [django,python,Web]
copyright: true
categories: "python"
---

**setting源代码**
```
# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
import os

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/1.8/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'w5l7yqw$4o@mf%!ydt)xz+aq-2^(lu@q&z4*8q_&oh5bi*8@nw'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []

# Application definition
```
>**项目中安装的APP**
```
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
)
```
当我们新创建一个APP时,需要把APP添加到上面，例如上面新添加的blog APP(**注意不要忘记末尾的"**，**"**)

```
MIDDLEWARE_CLASSES = (
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django.middleware.security.SecurityMiddleware',
)

ROOT_URLCONF = 'newsite.urls'
```
```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'newsite.wsgi.application'
```

>[**数据库配置**](https://docs.djangoproject.com/en/1.8/ref/settings/#databases)
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'blog',          #数据库的名字
        'USER':'',               #数据库的用户
        'PASSWORD':'',           #数据库的用户对应的密码
        'HOST':'',               #数据库的地址
        'PORT':'',               #数据库对应的端口
    }
}
 ```
**重要补充**:**当使用mysql最为数据库时，需要先使用命令行[创建相应的数据库](http://www.jianshu.com/p/4bb43b2df08c)。**

<br>
>**时区与语言设置**
```
LANGUAGE_CODE = 'zh_cn'
TIME_ZONE = 'Asia/Shanghai'
```
**注意要把时区和时间改成上述样式。**

```
USE_I18N = True

USE_L10N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.8/howto/static-files/

STATIC_URL = '/static/'
```