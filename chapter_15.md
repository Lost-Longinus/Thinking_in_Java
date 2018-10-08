# Chapter 15 - ***Generics***
* 类型参数--用<>括住，放在类名后面
```sh 
public class App<T> {
	private T a;
	public App(T a){
		this.a = a;
	}
	public void set(T a){
		this.a = a;
	}
	public T get(){
		return a;
	}
	public static void main(String[] args) {
		App<Double> app = new App<Double>(new Double(3.1415926));
		System.out.println("圆周率： "+app.get());
	}
}
//output
圆周率： 3.1415926
```
* 元组数据  
```sh 
public class TwoTuple<A,B>{
	public final A first;
	public final B second;
	public TwoTuple(A a,B b){
		first = a;
		second = b;
	}
	public String toString(){
		return "("+first+","+second+")";
	}
}
```
* RandomList容器  
```sh 
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class App<T> {
	List<T> list = new ArrayList<T>();
	Random random = new Random(System.nanoTime());
	public void add(T t){
		list.add(t);
	}
	public T select(){
		return list.get(random.nextInt(list.size()));
	}
	public static void main(String[] args) {
		App<String> app = new App<String>();
		for(String s:("The quick brown fox jumped over"
				+"the lazy brown dog").split(" ")){
			app.add(s);
		}
		for(int i = 0; i<11; i++){
			System.out.print(app.select()+" ");
		}
	}
}
//output  
jumped quick The quick dog The dog The overthe brown jumped 
```
* 泛型接口 
```sh 
public interface Generator<T>{T next();}
```
* 泛型方法  
是否拥有泛型方法，与其所在的类是否是泛型，没有关系；
只需将泛型参数列表置于返回值之前。
```sh 
public <T> void method(T t){};
```
* 使用该方法时，不必指明参数类型，编译器通过**类型参数推断**，找到具体类型，
使用时和普通方法无异。  
```sh 
public class GenericTest {
    public static void main(String[] args) {
        Box<String> name = new Box<String>("corn");
        Box<Integer> age = new Box<Integer>(712);
        System.out.println("name class:" + name.getClass());      
        System.out.println("age class:" + age.getClass());        
        System.out.println(name.getClass() == age.getClass());    
    }
}
class Box<T> {
    private T data;
    public Box() {
    }
    public Box(T data) {
        this.data = data;
    }
    public T getData() {
        return data;
    }
}
//output
name class:class Box
age class:class Box
true
```  
* Java中的泛型其只是作用于代码编译阶段，在编译过程中，对于正确检验泛型结果后，  
会将泛型的相关信息擦出，也就是说，成功编译过后的class文件中是不包含任何泛型信息的。  
泛型信息不会进入到运行时阶段。  
* 类型通配符   
类型通配符上限<? extends T>  
类型通配符下限<? super T>  
* 泛型--擦除  
Java在编译时会校验泛型参数，生成时会以泛型实参的上限类型替代真实的泛型实参。   
但，Java虚拟机会以签名的形式保留这些泛型实参类型(包括类的定义、泛型方法、泛型字段都会保留参数的签名信息)     
> 潜在类型机制--JAVA不支持  
>> Python是动态类型语言（类型检查都发生在运行时）;  
>> C++是静态类型语言（类型检查都发生在编译期）.