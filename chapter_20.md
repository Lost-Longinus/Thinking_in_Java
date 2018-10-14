# Chapter 20 - ***Annotation***  
> 三个标准注解
>> @Deprecated 意思是“废弃的，过时的”   
@Override 意思是“重写、覆盖”   
@SuppressWarnings 意思是“抑制警告”    

> 四个元注解  
>>  @Target  @Retention  @Document  @Inherited        
*  @Target定义注解作用域
![meta](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539539064113&di=51ce4ef63a899789d344e147d1da17c7&imgtype=jpg&src=http%3A%2F%2Fimg1.imgtn.bdimg.com%2Fit%2Fu%3D1491412921%2C531024205%26fm%3D214%26gp%3D0.jpg)
```sh 
package java.lang.annotation;

public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */

    FIELD,              /* 字段声明（包括枚举常量）  */

    METHOD,             /* 方法声明  */

    PARAMETER,          /* 参数声明  */

    CONSTRUCTOR,        /* 构造方法声明  */

    LOCAL_VARIABLE,     /* 局部变量声明  */

    ANNOTATION_TYPE,    /* 注释类型声明  */

    PACKAGE             /* 包声明  */
}
```
```sh 
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */

    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */

    RUNTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

* @Retention定义注解可用级别    

| 注解常量 | 可用级别 |  
| :---: | :---: |  
| SOURCE | 源代码中，即java文件 |  
| CLASS | 类文件中，即class文件 |  
| RUNTIME | 运行时，即内存中的字节码 |  

*  @Document注解包含在JavaDoc中
* @Inherited允许子类继承父类中的注解   
* 注解通用定义方式
```sh 
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation1 {
}
```
