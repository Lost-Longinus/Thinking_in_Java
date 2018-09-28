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
* 正则表达式  

>字符  
>>    x 字符 x。举例：'a'表示字符a  
    \\ 反斜线字符。   
    \n 新行（换行）符 ('\u000A')   
    \r 回车符 ('\u000D')  
    
>字符类    
>>    [abc] a、b 或 c（简单类）     
    [^abc] 任何字符，除了 a、b 或 c（否定）   
    [a-zA-Z] a到 z 或 A到 Z，两头的字母包括在内（范围）     
    [0-9] 0到9的字符都包括  
    
>预定义字符类  
>>    . 任何字符。我的就是.字符本身，怎么表示呢? \.  
    \d 数字：[0-9]  
    \D 非数字:[^\d]/[^0-9]  
    \w 单词字符：[a-zA-Z_0-9]   
    \W 非字符[^\w]   

>边界匹配器   
>>    ^ 行的开头    
    $ 行的结尾   
    \b 单词边界， 就是不是单词字符的地方。  
     
>Greedy 数量词   
>>    X? X，一次或一次也没有   
    X* X，零次或多次  
    X+ X，一次或多次  
    X{n} X，恰好 n 次    
    X{n,} X，至少 n 次   
    X{n,m} X，至少 n 次，但是不超过 m 次    

>运算符    
>>	XY 　　	X后跟 Y   
	X|Y 　　X 或 Y    
	(X) 　　X，作为捕获组      
* String类中的三个基本操作使用正则：  
匹配：matches()  
切割: split()  
替换: replaceAll()/replaceFirst()  

* Matcher类中的三个操作：  
matches()：输入字符串全部匹配正则表达式   
find(): 在输入的任意位置定位正则表达式  
lookingAt(): 输入的第一部分匹配正则表达式  
```sh 
public class Findding {
	public static void main(String[] args) {
		Matcher matcher = Pattern.compile("\\w+")
				.matcher("Evening is full of linnet's wings");
		while (matcher.find()){
			System.out.print(matcher.group()+" ");
		}
		System.out.println();
		System.out.println(matcher.replaceAll("match"));
	}
}
//output
Evening is full of linnet s wings 
match match match match match'match match 
```  
* Scanner类  
***既可以扫描基本类型，还可以使用正则表达式***  
```sh
public class App {
	
	public static void main(String[] args) {
		Scanner scanner = new Scanner("12, 34 , 4,45");
		scanner.useDelimiter("\\s*,\\s*");
		while(scanner.hasNext()){
			System.out.println(scanner.nextInt());
		}
	}
}
//output
12
34
4
45
```
