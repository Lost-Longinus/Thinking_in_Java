### Chapter 13 - ***String***
* String对+和+=方法重载，实际上仍然调用了StringBuilder的append方法  
所以，当字符串连接数量少时，直接用+法操作方便。但在循环体中，使用StringBuilder会提高效率。

* printf--格式化输出  
使用%X占位符表示数据将来的位置，插入数据以逗号分隔开。  
printf()==format()   
* Formatter  
构造该对象时，可以向其中传入如system.out等参数，表示向哪里输出结果  
其抽象语法：%[argument_index$][flags][width][.precision]conversion  
（1）argument_index$：指定对应的内容参数位置，默认按照顺序依次对应。  
（2）flags：格式控制。  
（3）width：区域宽度。  
（4）.precision：对于浮点型数据，表示显示的小数位数；对于字符串数据，表示显示的字符数量。  
（5）conversion：类型转换字符。  
例如：
```sh
public class App {
	public static void main(String[] args) {
		Formatter f = new Formatter(System.out);
		f.format("%-15.15s %5d %10.2f\n","Beans",4,4.25);
	}
}
//output
Beans               4       4.25
```
