---
title: 一些坑
date: 
tags: [others]
copyright: true
categories: "others"
---


# 1. Maven打jar包，设置主类
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.northcity.cache.CacheApplication</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

# 2. SpringBoot yml文件设置jdbc的URL规范
```
spring:
    datasource:
        driver-class-name:com.mysql.cj.jdbc.Driver
        url:jdbc:mysql://172.30.213.213:3306/user?serverTimezone=UTC
        &useUnicode=true&characterEncoding=utf8&useSSL=false
        username:root
        password:"1234"   //这里密码一定要用双引号包起来
```