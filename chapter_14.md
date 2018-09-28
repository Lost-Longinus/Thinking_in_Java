### Chapter 14 - ***Class***
* RTTI(Run-time Type Identify)  
运行时类型识别，基于class对象实现。   
这个类型必须在编译之前已知，这样才能使用RTTI来识别它。   
编译器在编译期间打开和检查 .class文件  
* Class类  
用newInstance()方法创建对象的类，必须带有默认构造器；
建议使用类字面常量--类名.class而非Class.forName()创建类，因为它会在编译期受检查；  
但须注意，仅使用类字面量只会获得对类的引用，而不会引发初始化。  
* 泛化的Class引用  
```sh 
public class GenericClass {
	public static void main(String[] args) {
		Class<Integer> intClazz01 = int.class;
		//intClazz01 = double.class;报错！！
		//解决方法一：选择非具体
		Class<?> intClazz02 = int.class;
		intClazz02 = double.class;
		//解决方法二：指定范围（Number是Integer父类）
		Class<? extends Number> intClazz03 = int.class;
		intClazz03 = double.class;
	} 
}
```  
***反射***  
 Class类与java.lang.reflect类库一起对反射进行了支持，该类库包含Field、Method和Constructor类，  
 这些类的对象由JVM在启动时创建，用以表示未知类里对应的成员。这样的话就可以使用Contructor创建新的对象，  
 用get()和set()方法获取和修改类中与Field对象关联的字段，用invoke()方法调用与Method对象关联的方法。  
 另外，还可以调用getFields()、getMethods()和getConstructors()等许多便利的方法，  
 以返回表示字段、方法、以及构造器对象的数组。  
 * 动态代理   
 ```sh 
 import java.lang.reflect.InvocationHandler;
 import java.lang.reflect.Method;
 import java.lang.reflect.Proxy;
 
 /**
  * Created by Pengfei Jin on 2018/9/28.
  */
 public class DynamicProxy {
 	public static void main(String[] args) {
 		SomeMethods proxy = (SomeMethods) Proxy.newProxyInstance(
 				SomeMethods.class.getClassLoader(),
 				new Class[]{SomeMethods.class},
 				new InvocationHandler() {
 					@Override
 					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
 						if (method.getName().equals("interesting")){
 						    System.out.println("Proxy detected the interesting method.");
 						}
 						return method.invoke(new Implementation(),args);
 					}
 				}
 		);
 		proxy.boring();
 		proxy.interesting("BlueHole");
 	}
 }
 interface SomeMethods{
 	void boring();
 	void interesting(String arg);
 }
 class Implementation implements SomeMethods{
 	@Override
 	public void boring() {
 		System.out.println("boring!");
 	}
 	@Override
 	public void interesting(String arg) {
 		System.out.println("interesting "+arg);
 	}
 }
 //output
 boring!
 Proxy detected the interesting method.
 interesting BlueHole
 ``` 
 
 
  

