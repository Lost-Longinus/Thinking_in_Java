# Chapter 20 - ***Concurrent***  
* 定义一个任务(实现Runnable接口)，并附着到一个线程上   
```sh 
/**
 * Created by Pengfei Jin on 2018/10/15.
 */
public class ConcurrentDemo1 {
	public static void main(String[] args) {
		Thread thread = new Thread(new LiftOff());
		thread.start();
		System.out.println("Waiting for Liftoff...");
	}
}
class LiftOff implements Runnable{
	protected int countDown = 10;
	private static int taskCount = 0;
	private final int id = taskCount++;
	public LiftOff() {
	}
	public LiftOff(int countDown) {
		this.countDown = countDown;
	}
	public String status(){
		return "#" + id + "(" + (countDown > 0 ? countDown : "Lift off!!") + ")";
	}
	@Override
	public void run() {
		while (countDown-- > 0){
			System.out.println(status());
			Thread.yield();
		}
		System.out.println()
	}
}
//output
Waiting for Liftoff...
#0(9)  #0(8)  #0(7)  #0(6)  #0(5)  #0(4)  #0(3)  #0(2)  #0(1)  #0(Lift off!!) 
```
* 任何线程都可以启动另一个线程。  
* main()方法也是一个线程，它无法管理另一个线程。  
管理线程须用执行器(Executor)，它暴露了执行的单一类ExecutorService。
* 只创建一个线程
```sh 
public static void main(String[] args) {
    ExecutorService executorService = Executors.newSingleThreadExecutor();//--只创建一个线程
    for (int i = 0; i < 2; i++) {
        executorService.execute(new LiftOff());
    }
    executorService.shutdown();
}
//output
#0(9)  #0(8)  #0(7)  #0(6)  #0(5)  #0(4)  #0(3)  #0(2)  #0(1)  #0(Lift off!!)  
#1(9)  #1(8)  #1(7)  #1(6)  #1(5)  #1(4)  #1(3)  #1(2)  #1(1)  #1(Lift off!!)  
```
* 创建一个缓冲线程池
```sh 
public static void main(String[] args) {
    ExecutorService executorService = Executors.newCachedThreadPool();//--创建一个缓冲线程池
    for (int i = 0; i < 2; i++) {
        executorService.execute(new LiftOff());
    }
    executorService.shutdown();
}
//output
#1(9)  #0(9)  #1(8)  #0(8)  #1(7)  #0(7)  #1(6)  #0(6)  #1(5)  #0(5)  #1(4)  #0(4)  #1(3)  
#0(3)  #0(2)  #1(2)  #0(1)  #1(1)  #0(Lift off!!)  #1(Lift off!!)  
  

```
* 如果希望任务产生返回值，应该实现Callable，并调用其call()方法。
* 休眠sleep()方法，将使任务停止给定时间后再执行。
* 优先级   
优先级较低的线程执行频率较低，所以，调度器倾向于让优先级最高的线程先执行。
* 让步   
yield方法表示让步，即给调度机制暗示：这项任务已经做得差不多了，可以让别的线程使用CPU。  
* volatile关键字  
对一个变量的写操作先行发生于后面对这个变量的读操作      
1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。    
2）禁止进行指令重排序。  
 * 解决共享资源竞争  
 1）synchronized关键字标记方法或代码块，其它访问将被阻塞。   
 注意同步代码块需要指定锁，常使用当前对象this。   
 而下面是一个使用其它对象作为同步锁的例子   
 ```sh 
 /**
  * Created by Pengfei Jin on 2018/10/18.
  */
 public class SyncObject {
 	public static void main(String[] args) {
 		final DualSynch ds = new DualSynch();
 		new Thread(){
 			public void run(){
 				ds.f();
 			}
 		}.start();
 		ds.g();
 	}
 }
 class DualSynch{
 	private Object syncObject = new Object();
 	public synchronized void f(){
 		for (int i = 0; i < 5; i++) {
 			System.out.println("f()");
 			Thread.yield();
 		}
 	}
 	public void g(){
 		synchronized (syncObject){
 			for (int i = 0; i < 5; i++) {
 				System.out.println("g()");
 				Thread.yield();
 			}
 		}
 	}
 }
 //output
 g()
 g()
 g()
 f()
 g()
 f()
 f()
 g()
 f()
 f()
 ```
 2）显式互斥机制--Lock对象，创建，锁定，释放。   
 记住在finally块中释放Lock对象，以免忘记。   
 3）线程本地存储--ThreadLocal对象     
 可以为使用相同变量的每个不同的线程都创建不同的存储。 
 * 线程四种状态：新建（new）、就绪（Runnable）、阻塞（Blocked）、死亡（Dead）    
 * 一个任务进入阻塞状态，可能有如下原因：   
 1、调用sleep()方法，使线程休眠；  
 2、调用wait()方法，使线程挂起；   
 3、任务在等待某个输入/输出；   
* 如何中断单个任务   
使用执行器Executor的submit()方法，而非executor方法启动任务，    
在其返回的Future<?>的基础上，调用cancle(true)，   
即拥有了在该线程上调用interrupt()以停止这个线程的权限。  
 * 线程之间的协作   
 1、wait()方法挂起线程时，**会释放对象锁**，而sleep()和yield()方法不会释放。
 2、调用对象锁的notify()、notifyAll()，将唤醒在等待该对象锁而被挂起的任务。  
 * 死锁产生条件  
 1、互斥条件。任务使用的资源中至少有一个是不能共享的。  
 2、至少有一个任务必须持有一个资源且正在等待获取一个当前被别的任务持有的资源。  
 3、资源不能被任务抢占，任务必须把资源释放当作普通事件。  
 4、必须有循环等待。这时，一个任务在等待其它任务所持有的资源，   
 后者又在等待另一个任务所持有的资源。这样循环往复，   
 直至有一个任务在等待第一个任务所持有的资源。 
 * 乐观锁和悲观锁   
 **悲观锁**：总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，   
 所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。   
 传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，   
 都是在做操作之前先上锁。再比如Java里面的同步原语synchronized关键字的实现也是悲观锁。
 **乐观锁**：顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，   
 所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，   
 可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，  
 像数据库提供的类似于write_condition机制，其实都是提供的乐观锁。   
 在Java中java.util.concurrent.atomic包下面的原子变量类就是使用了乐观锁的一种实现方式CAS实现的。