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

![向上转型](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537715513905&di=798aba637e3d250cf1bc9cb086cf4157&imgtype=0&src=http%3A%2F%2Fimage.codes51.com%2FArticle%2Fimage%2F20150512%2F20150512141922_9180.jpg "向上转型")

* 引用计数跟踪共享对象示例 
```sh
public class Refference {
	public static void main(String[] args) {
		Shared shared = new Shared();
		Composing[] composings = {new Composing(shared), new Composing(shared), new Composing(shared), new Composing(shared), new Composing(shared)};
		for(Composing c:composings){
			c.dispose();
		}
	}
}
class Shared{
	private int refcount = 0;
	private static long counter = 0;
	private final long id = counter++;
	public Shared(){
		System.out.println("Creating "+this);
	}
	public void addRef(){
		refcount++;
	}
	protected void dispose(){
		if (--refcount == 0) {
		    System.out.println("Disposing "+this);
		}
	}
	@Override
	public String toString() {
		return "Shared "+id;
	}
}

class Composing{
	private Shared shared;
	private static long counter = 0;
	private final long id = counter++;

	public Composing(Shared shared) {
		System.out.println("Creating "+this);
		this.shared = shared;
		this.shared.addRef();
	}
	protected void dispose(){
		System.out.println("Disposing "+this);
		shared.dispose();
	}
	@Override
	public String toString() {
		return "Composing "+id;
	}
}
```
*其中(static)counter为类型Composing的**共享**成员变量，*
*final则决定每个Composing对象id不能改变。*
> //output  
Creating Shared 0  
Creating Composing 0  
Creating Composing 1  
Creating Composing 2  
Creating Composing 3  
Creating Composing 4  
Disposing Composing 0  
Disposing Composing 1  
Disposing Composing 2  
Disposing Composing 3  
Disposing Composing 4  
Disposing Shared 0  




