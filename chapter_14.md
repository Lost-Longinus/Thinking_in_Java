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
