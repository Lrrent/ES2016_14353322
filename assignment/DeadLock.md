### DeadLock


----------
#### 一、死锁代码分析
* 类A和类B的定义，最关键的是 synchronized， 当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。
下面的代码表示，methodA调用B类的last函数，Synchronized 相当于加锁
 ```
 class A{
	 synchronized void methodA(B b){
		 b.last();
	 }	
	 synchronized void last(){
		 System.out.println("Inside A.last()");
	 }
 }

 class B{
	 synchronized void methodB(A a){
		 a.last();
	 }
	 synchronized void last(){
		 System.out.println("Inside B.last()");
	 }
 }
 ```
* 主程序，就是首先new一个线程，开始运行new出来的线程调用start函数（调用函数run()里面的内容，调用资源B的methodB），接着系统计数10000次以后，主线程开始对调用资源A的methodA。
 ```
class Deadlock implements Runnable{
	A a=new A();
	B b=new B();
//构造函数
	Deadlock(){
		Thread t = new Thread(this);
		int count = 10000;
		
		t.start();//线程t开始，调用函数run()
		while(count-->0);//等待10000时间周期
		a.methodA(b);
	}
	//runnable运行时调用的方法
	public void run(){
		b.methodB(a);
	}
	
	public static void main(String args[]){
		new Deadlock();
	}
}
 ```
主程序时间轴如下

    ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/3606513.jpg)



----------
#### 二、产生死锁的结果及原因分析
* 运行程序并截图出现死锁的情况

    ![] (http://7xrn7f.com1.z0.glb.clouddn.com/16-11-3/6164039.jpg)
    
    由上图可见，第22次出现死锁，一开始并没有出现，之后慢慢增加count的值(由10000增加到200000)，才出现了死锁
    ```
    int count = 200000;
		
		t.start();
		while(count-->0);
		a.methodA(b);
    ```
    其实还有一种方法是更改类A和类B手握资源的时间
    ```
     class A{
	 synchronized void methodA(B b){
	     int count = 200000;
	     while(count-->0);
		 b.last();
	 }
	
	 synchronized void last(){
		 System.out.println("Inside A.last()");
	 }

 }
    ```
*  解释本次实验产生死锁的原因
  代码其实在上文中已经分析过了，由于类A和类B都有synchronized修饰，保证在同一时间最多只有一个线程执行该段代码。也就是说，因为b.last()是一个synchronized同步代码块，当Deadlock()在访问这部分代码块的时候,同为synchronized同步代码块的a.last()的访问将被阻塞。
  在主函数里面，构造函数内的a.method(b)有调用b.last()，当他先申请得到b.last()这部分资源的时候，run()如果再去申请a.last()就会被阻塞，只有等b.last()完全被释放，run()才会从阻塞的状态变为被唤醒，才能去申请资源a.last()，我们会产生死锁就是因为产生了资源占用的冲突，当run在运行b.method时候，如果恰好构造函数中计数完毕，也开始运行a.method就会产生死锁。



----------
#### 三、死锁产生的四个必要条件
1. 互斥条件：一个资源每次只能被一个进程使用
2. 请求与保持条件（占用并等待）：一个进程因请求资源而阻塞时，对已获得的资源保持不放
3. 不剥夺条件（非抢占）:进程已获得的资源，在未使用完之前，不能强行剥夺
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系

