### Chapter 12 - ***Exception***
* Java异常类
![Exception](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537972588043&di=104e72de900604d8b1ce0e0b7d883065&imgtype=jpg&src=http%3A%2F%2Fimg1.imgtn.bdimg.com%2Fit%2Fu%3D1244181466%2C1471403492%26fm%3D214%26gp%3D0.jpg)  

* try块中的局部变量和catch块中的局部变量（包括异常变量），以及finally中的局部变量，他们之间不可共享使用。

* 每一个catch块用于处理一个异常。异常匹配是按照catch块的顺序从上往下寻找的，
只有第一个匹配的catch会得到执行。匹配时，不仅运行精确匹配，也支持父类匹配，
因此，如果同一个try块下的多个catch异常类型有父子关系，应该将子类异常放在前面，父类异常放在后面，这样保证每个catch块都有存在的意义。

* java中，异常处理的任务就是将执行控制流从异常发生的地方转移到能够处理这种异常的地方去。
也就是说：当一个函数的某条语句发生异常时，这条语句的后面的语句不会再执行，它失去了焦点。
执行流跳转到最近的匹配的异常处理catch代码块去执行，异常被处理完后，执行流会接着在“处理了这个异常的catch代码块”后面接着执行。

* 有的编程语言当异常被处理后，控制流会恢复到异常抛出点接着执行，这种策略叫做：resumption model of exception handling（恢复式异常处理模式 ）
而Java则是让执行流恢复到处理了异常的catch块后接着执行，这种策略叫做：termination model of exception handling（终结式异常处理模式）

* 当子类重写父类的带有 throws声明的函数时，其throws声明的异常必须在父类异常的可控范围内——用于处理父类的throws方法的异常处理器，
必须也适用于子类的这个带throws方法 。这是为了支持多态。

| Checked异常 | Unchecked异常 |  
| :---: | :---: |  
| 基类：java.lang.Exception | 基类：java.lang.RuntimeException |  
| 必须被显式地捕获或者传递 | 不必捕获或抛出 | 