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
	}
}
//output
Waiting for Liftoff...
#0(9)
#0(8)
#0(7)
#0(6)
#0(5)
#0(4)
#0(3)
#0(2)
#0(1)
#0(Lift off!!)
```