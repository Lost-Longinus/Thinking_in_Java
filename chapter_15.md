# Chapter 15 - Class
* 类型参数--用<>括住，放在***类名后面
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

