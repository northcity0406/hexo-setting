---
title: JAVA注解原理解析和自定义注解
date: 
tags:
	- JAVA
	- 注解
categories: "JAVA"
copyright: true
---

## 1.JAVA注解原理解析
### JAVA注解基本形式
>注解是Java 1.5引入的，可以提供代码的额外信息，目前正在被广泛应用。
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Msg {
  String DEFAULT_MSG = "msg";
  String msg() default DEFAULT_MSG
}
```
上述代码展示了一个基本的注解，可以看出注解都有以下特点：

    1) @Interface关键字定义注解；
    2) 注解可以被其他的注解修饰；
    3) 注解和接口相似，内部可以定义常量和方法；
### 元注解
####  @Target 描述注解使用的范围

|	范围描述						 |	作用						|
|	:---:							|	:-----:					|
|	ElementType.PACKAGE				|	注解作用于包					|
|	ElementType.TYPE				|	注解作用于类型（类，接口，注解，枚举）|
|	ElementType.ANNOTATION_TYPE		|	注解作用于注解					|
|	ElementType.CONSTRUCTOR			|	注解作用于构造方法				|
|	ElementType.METHOD				|	注解作用于方法					|
|	ElementType.PARAMETER			|	注解作用于方法参数				|
|	ElementType.FIELD				|	注解作用于属性					|
|	ElementType.LOCAL_VARIABLE		|	注解作用于局部变量				|

####  @Retention 描述注解使用的范围
| 范围描述  | 作用  |
|:---:|:---:|
| RetentionPolicy.SOURCE  | 源码中保留，编译期可以处理  |
|RetentionPolicy.CLASS    |Class文件中保留，Class加载时可以处理|
|RetentionPolicy.RUNTIME|运行时保留，运行中可以处理|

**默认RetentionPolicy.CLASS值**

#### @Inherited注解
>@Inherited注解修饰的注解作用于一个类，则该注解将被用于该类的子类。

#### @Documented
>描述注解可以**文档化**，是一个标记注解。在生成javadoc的时候，是不包含注释的，但是如果注解被@Documented修饰，则生成的文档就包含该注解。


## 自定义JAVA注解

####  自定义的Printer注解
```
import java.lang.annotation.*;

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface PrinterAnnotation {
    String value() default "";
}
```

####  IPrinter的Printer接口
```
public interface IPrinter {
    public void Print();
}
```

#### 图片打印机的实现
```
import com.northcity.api.IPrinter;
public class ImagePrinter implements IPrinter {
    public void Print() {
        System.out.println("Image Printer");
    }
}
```

#### 文本打印机
```
import com.northcity.api.IPrinter;
public class TextPrinter implements IPrinter {
    public void Print() {
        System.out.println("Text Printer");
    }
}
```

#### 打印功能的实现
```
import com.northcity.annotation.PrinterAnnotation;
import com.northcity.api.IPrinter;
public class Printer{
    @PrinterAnnotation("TextPrinter")
    private IPrinter iPrinter;
    public void setiPrinter(IPrinter iPrinter) {
        this.iPrinter = iPrinter;
    }
    public void Print(){
        iPrinter.Print();
    }
}
```

#### 使用反射注入Printer
```
import com.northcity.annotation.PrinterAnnotation;
import com.northcity.impl.ImagePrinter;
import com.northcity.impl.TextPrinter;
import java.lang.reflect.Field;
public class Main {
    public static void main(String [] args) throws IllegalAccessException, InstantiationException {
        //获取Printer的Class对象
        Class<Printer> printerClass = Printer.class;
        //获取Printer的实例对象
        Printer printer = (Printer)printerClass.newInstance();
        //遍历Printer的Class的所有Filed对象
        for(Field field : printerClass.getDeclaredFields()){
            //获得该Field的PrinterAnnotation注解
            PrinterAnnotation spiAnnotation = (PrinterAnnotation) field.getAnnotation(PrinterAnnotation.class);
            //获得该注解的值
            String value = spiAnnotation.value();
            //根据注解的值注入对应的
            if(value.equals("TextPrinter")) printer.setiPrinter(new TextPrinter());
            else printer.setiPrinter(new ImagePrinter());
        }
        printer.Print();
    }
}
```
>在本示例中，我们使用注解来指定打印时所使用的打印机的具体实现，然后使用反射的方法来获取该注解的值，接下来根据注解的值生成相应的对象并注入，最后就可以使用具体的Printer来打印。

[**github 源码**](https://github.com/northcity0406/CodeHub/tree/master/Annotation)









