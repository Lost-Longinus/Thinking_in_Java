# Thinking_in_Java
*A notebook of reading &lt;Thinking in Java>*
***
### Chapter 7 - ***extend***
* final关键字  
应用于基本类型前，则基本类型值不变；
应用于对象前，对象引用不变，但对象内基本类型值可变；
应用于方法前，则该方法在子类中不能被复写。
final类无法继承

### Chapter 8 - ***Polymorphism***
* 多态与向上转型
前期定义方法形参为父类，传入实参为子类时，子类向上转型。具体方法调用时，通过后期/动态绑定，调用具体类型方法。
从而只编写与父类打交道的程序！

[向上转型]:C:/Users/金鹏飞/Desktop/Thinking_in_Java/向上转型.jpg


