# Chapter 15 - Class
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

