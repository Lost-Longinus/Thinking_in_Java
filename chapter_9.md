### Chapter 9 - ***Interface***
* 接口中方法的访问修饰符可以不定义，自动就是且仅是public。  
>策略模式(示例)
```sh 
import java.util.Arrays;
/**
 * Created by Pengfei Jin on 2018/9/24.
 */
public class Strategy_pattern {
	public static void process(Strategy_pattern.Processor p, Object s){
		System.out.println("Using Processor "+p.name()); 
		System.out.println(p.process(s)); 
	}
	public static String s = "Disagreement with briefs is by definition incorrect";

	public static void main(String[] args) {
		process(new Upcase(),s);
		process(new Downcase(),s);
		process(new Splitter(),s);
	}
}
class Processor{
	public String name(){
		return getClass().getSimpleName();
	}
	Object process(Object input){
		return input;
	}
}
class Upcase extends Strategy_pattern.Processor {
	String process(Object input){
		return ((String)input).toUpperCase();
	}
}
class Downcase extends Strategy_pattern.Processor {
	String process(Object input){
		return ((String)input).toLowerCase();
	}
}
class Splitter extends Strategy_pattern.Processor {
	String process(Object input){
		return Arrays.toString(((String)input).split(" "));
	}
}
```
***其中process方法，即为应用不同Processor策略作用于String上。***  

>适配器模式  
* implements多个接口的实现类，可以向上转型为其中任意一个接口  
* 接口和抽象类创建场合 

| 类型 | 创建场合 |
| :---: | :---: |
| 接口-interface | 不带任何方法定义和成员变量 |
| 抽象类-abstract | 除抽象方法外，带有具体方法定义，或有成员变量 |