---
title: JAVA SPI原理分析
date: 
tags: [JAVA]
copyright: true
categories: "JAVA"
---

### 1.什么是SPI?
>SPI 全称为 (Service Provider Interface) ，是JDK内置的一种服务提供发现机制。SPI是一种动态替换发现的机制， 比如有个接口，想运行时动态的给它添加实现，你只需要添加一个实现。我们经常遇到的就是java.sql.Driver接口，其他不同厂商可以针对同一接口做出不同的实现，mysql和postgresql都有不同的实现提供给用户，而Java的SPI机制可以为某个接口寻找服务实现。

![JAVA SPI类图](https://user-gold-cdn.xitu.io/2018/5/14/1635dec2151e31e4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.SPI应用实例
>当服务的提供者提供了一种接口的实现之后，需要在classpath下的META-INF/services/目录里创建一个以服务接口命名的文件，这个文件里的内容就是这个接口的具体的实现类。当其他的程序需要这个服务的时候，就可以通过查找这个jar包（一般都是以jar包做依赖）的META-INF/services/中的配置文件，配置文件中有接口的具体实现类名，可以根据这个类名进行加载实例化，就可以使用该服务了。JDK中查找服务实现的工具类是：java.util.ServiceLoader。

#### 打印机接口
```
package com.northcity.spi.PrinterInterface;
public interface IPrinter {
    public void Print();
}
```

#### 图片打印机
```
package com.northcity.spi.PrinterImpl;
import com.northcity.spi.PrinterInterface.IPrinter;
public class ImagePrinter implements IPrinter {
    public void Print() {
        System.out.println("Image printer");
    }
}
```

#### 文字打印机
```
package com.northcity.spi.PrinterImpl;
import com.northcity.spi.PrinterInterface.IPrinter;
public class TextPrinter implements IPrinter {
    public void Print() {
        System.out.println("Text printer");
    }
}
```

### 调用方法
```
package com.northcity.spi;
import com.northcity.spi.PrinterInterface.IPrinter;
import java.util.ServiceLoader;
public class Main {
    public static void main(String [] args){
        ServiceLoader<IPrinter> serviceLoader = ServiceLoader.load(IPrinter.class);
        for(IPrinter iPrinter : serviceLoader){
            iPrinter.Print();
        }
    }
}

```

#### 接口实现配置
```
com.northcity.spi.PrinterImpl.ImagePrinter
com.northcity.spi.PrinterImpl.TextPrinter
```

#### 目录结构
![目录结构](https://github.com/northcity0406/CodeHub/blob/master/ImageHub/2019-11-19/1.png?raw=true)

#### 运行结果
![运行结果](https://github.com/northcity0406/CodeHub/blob/master/ImageHub/2019-11-19/2.png?raw=true)

#### 源码地址
[SPI实例，请参考github](https://github.com/northcity0406/CodeHub/tree/master/PrinterSPI)

### 3.原理分析
```
private boolean hasNextService() {
    if (nextName != null) {
        return true;
    }
    if (configs == null) {
        try {
            //private static final String PREFIX = "META-INF/services/";
            //读取PREFIX下面的所有的Service的名字，
            String fullName = PREFIX + service.getName();
            if (loader == null)
                configs = ClassLoader.getSystemResources(fullName);
            else
                configs = loader.getResources(fullName);
        } catch (IOException x) {
            fail(service, "Error locating configuration files", x);
        }
    }
    while ((pending == null) || !pending.hasNext()) {
        if (!configs.hasMoreElements()) {
            return false;
        }
        pending = parse(service, configs.nextElement());
    }
    nextName = pending.next();
    return true;
}

private S nextService() {
    if (!hasNextService())
        throw new NoSuchElementException();
    String cn = nextName;
    nextName = null;
    Class<?> c = null;
    try {
        //加载具体的实现类
        c = Class.forName(cn, false, loader);
    } catch (ClassNotFoundException x) {
        fail(service,
             "Provider " + cn + " not found");
    }
    if (!service.isAssignableFrom(c)) {
        fail(service,
             "Provider " + cn  + " not a subtype");
    }
    try {
        //实例化实现类
        S p = service.cast(c.newInstance());
        providers.put(cn, p);
        return p;
    } catch (Throwable x) {
        fail(service,
             "Provider " + cn + " could not be instantiated",
             x);
    }
    throw new Error();          // This cannot happen
}
```

>JAVA的SPI使用了java.util.ServiceLoader类来加载外部包含的服务的实现，使用迭代器的方式来遍历"META-INF/services/"下的所有的服务的具体的实现的类的全路径。nextService函数主要通过加载实现类c = Class.forName(cn, false, loader)以及实例化实现类p = service.cast(c.newInstance())，来返回具体的实现类对象，这样再上文测试代码中就能够获取到配置的实现类的实例。
