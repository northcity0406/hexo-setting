---
title: Dubbo SPI扩展原理和实践
date: 
tags:
	- Dubbo
	- JAVA
categories: "JAVA"
copyright: true
---

# 1. Dubbo SPI扩展原理
## 1.1 Dubbo SPI扩展相比JAVA SPI扩展的缺点
> 1. JAVA SPI会加载所有的所有的扩展，如果有JAVA的扩展加载很耗时，或者没有用上，这会很浪费资源。Dubbo的SPI扩展类按需加载，减少了资源的浪费。
> 2. JAVA SPI的扩展类加载失败可能会丢失异常信息。
> 3. JAVA的扩展类记载后会直接实例化所有的扩展。Dubbo扩展类会先加载所有的扩展类（即Class对象），但不立即初始化，只会按需进行实例化。并且实例化的过程中添加了IOC和AOP的支持。

## 1.2 Dubbo SPI扩展的配置规范

1. 路径
    *  META-INF/service
    *  META-INF/dubbo
    *  META-INF/dubbo/internal

2. SPI配置文件名称
   * 被扩展接口的全路径名称

3. 文件格式
   * key = value格式
   * 换行分割   

## 1.3 扩展点注解

### @SPI注解
```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
public @interface SPI {
    String value() default "";
}
```
@SPI注解可以表示为该扩展的默认使用的扩展，该注解的value将会是扩展配置文件中的key中的一个。

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface Adaptive {
    String[] value() default {};
}
```
@Adaptive注解使用一个数组来表示一系列扩展的实现。这里面的数组里面的值都是扩展配置文件中的key。该注解表示该方法会按照顺序来使用实现的扩展，如果前面的无法加载使用，直接使用下一个，以次类推。但是只会使用一个实现。

# 2. Dubbo SPI扩展实例

## 2.1 IPrinter接口
* 使用@SPI注解修饰，默认值为text
* print方法使用@Adaptive({"text","image"})修饰，表示按顺序选择实际执行的实例
```
package com.northcity.api;
import com.alibaba.dubbo.common.extension.Adaptive;
import com.alibaba.dubbo.common.extension.SPI;

@SPI("text)
public interface IPrinter {
    @Adaptive({"text","image"})
    public String print();
}
```

## 2.2 ImagePrinter实例
```
package com.northcity.printer;
import com.northcity.api.IPrinter;
public class ImagePrinter implements IPrinter {
    public String print() {
        return "Image Printer";
    }
}

```
## 2.3 TextPrinter实例
```
package com.northcity.printer;
import com.northcity.api.IPrinter;
public class TextPrinter implements IPrinter {
    public String print() {
        return "Text Printer";
    }
}
```

## 2.4 Main方法
```
package com.northcity;
import com.alibaba.dubbo.common.extension.ExtensionLoader;
import com.northcity.api.IPrinter;
public class Main {
    public static void main(String [] args){
        //使用默认的Printer实现
        IPrinter iPrinter = ExtensionLoader.getExtensionLoader(IPrinter.class).getDefaultExtension();
        System.out.println("default:\t" + iPrinter.print());

        //使用ImagePrinter实现
        iPrinter = ExtensionLoader.getExtensionLoader(IPrinter.class).getExtension("image");
        System.out.println("image:\t" + iPrinter.print());

    }
}

```

## 2.5 Dubbo配置实现
```
image=com.northcity.printer.ImagePrinter
text=com.northcity.printer.TextPrinter`
```

**[项目源码](https://github.com/northcity0406/CodeHub/tree/master/DubboSPI)**