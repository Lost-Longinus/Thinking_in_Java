### Chapter 9 - ***Interface***
* 接口中方法的访问修饰符可以不定义，自动就是且仅是public。  
>策略模式
```sh 
import java.util.Arrays;
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
> //output  
  Using Processor Upcase  
  DISAGREEMENT WITH BRIEFS IS BY DEFINITION INCORRECT  
  Using Processor Downcase  
  disagreement with briefs is by definition incorrect  
  Using Processor Splitter  
  [Disagreement, with, briefs, is, by, definition, incorrect]  

>适配器模式  
```sh 
public class Adapter {
	public static void process(Processors p, Object s){
		System.out.println("Using Processor "+p.name());
		System.out.println(p.process(s));
	}
	public static void main(String[] args) {
		Waveform waveform = new Waveform();
		Adapter.process(new FilterAdapter(new LowPass(1.0)),waveform);
		//FilterAdapter构造器接收具有接口Filter的Waveform,并产生所需要的Processors对象
	}
}
interface Processors{
	String  name();
	Object process(Object input);
}
class Filter{
	public String  name(){
		return getClass().getSimpleName();
	}
	public Waveform process(Waveform input){
		return input;
	}
}
class Waveform{
	private static long counter;
	private final long id = counter++;
	@Override
	public String toString() {
		return "Waveform "+id;
	}
}
class LowPass extends Filter{
	double cutoff;
	public LowPass(double cutoff){
		this.cutoff = cutoff;
	}
	public  Waveform process(Waveform input){
		return input;
	}
}
class FilterAdapter implements Processors{
	Filter filter;
	public FilterAdapter(Filter filter){
		this.filter = filter;
	}
	@Override
	public String  name() {
		return filter.name();
	}

	@Override
	public Object process(Object input) {
		return filter.process((Waveform) input);
	}
}
```  
> //output  
Using Processor LowPass  
Waveform 0
* implements多个接口的实现类，可以向上转型为其中任意一个接口  
* 接口和抽象类创建场合 

| 类型 | 创建场合 |
| :---: | :---: |
| 接口-interface | 不带任何方法定义和成员变量 |
| 抽象类-abstract | 除抽象方法外，带有具体方法定义，或有成员变量 |
***Interface可以继承Interface!!***
>工厂模式 
```sh 
public class Factories {
	public static void serviceConsumer(ServiceFactory factory){
		Service service = factory.getService();
		service.method01();
		service.method02();
	}
	public static void main(String[] args) {
		serviceConsumer(new ServiceFactoryImpl());
	}
}
interface Service{
	void method01();
	void method02();
}
interface ServiceFactory{
	Service getService();
}
class ServiceImpl implements Service{
	@Override
	public void method01() {
		System.out.println(getClass().getSimpleName()+" "+getClass().getMethods()[1].getName());
	}
	@Override
	public void method02() {
		System.out.println(getClass().getSimpleName()+" "+getClass().getMethods()[0].getName());
	}
}
class ServiceFactoryImpl implements ServiceFactory{
	@Override
	public Service getService() {
		return new ServiceImpl();
	}
}
```  
> //output  
  ServiceImpl method01  
  ServiceImpl method02 
