### Chapter 10 - ***Internal class***
* 内部类自动拥有对其外部类所有成员的访问权，**包括private成员**   
* 内部类的对象信息与外部类对象信息独立，即可以对象和方法重名
* 匿名内部类相当于是该类的一个导出类，且在匿名内部类中使用的参数，必须是final  
* 因为普通类只能单继承多实现，而要想多继承时，内部类可以解决该缺陷。
* 回调
回调就是已知A方法要调用B方法，但B方法会根据实际的操作有不同的实现，所以定义一个包含B方法的接口，
让A方法调用此接口中的方法，而这个接口方法的实现则根据不同的情况实现即可。   
```sh 
pubilc interface CallBack{
    public void callbackMethod();
}
public class A implements CallBack{ // A实现接口CallBack
    B b = new B();
    public void do(){
        b.doSomething(this); // A运行时调用B中doSomething方法,以自身传入参数，B已取得A，可以随时回调A所实现的CallBack接口中的方法
    }
    public void callbackMethod(){ // 对A来说，该方法就是回调方法
        System.out.println("callbackMethod is executing!");
    }
}
public class B{
    public void doSomething(CallBack cb){ // B拥有一个参数为CallBack接口类型的方法
        System.out.println(“I am processing my affairs… ”);
        System.out.println(“then, I need invoke callbackMethod…”);
        cb.callbackMethod();
    }
}
```