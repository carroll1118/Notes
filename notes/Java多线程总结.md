> 再谈线程之前我们需要先了解以下概念。
* `程序Program` : 程序是一段静态的代码，它是应用程序执行的蓝本
* `进程Process` : 进程是指一种正在运行的程序，有自己的地址空间
 	* `进程的特点`: 动态性 、并发性 、独立性
* `并发和并行的区别` 
	* 多个CPU同时执行多个任务 
	* 一个CPU（采用时间片）同时执行多个任务
## 什么是线程Thread 
* 进程内部的一个执行单元，它是程序中一个单一的顺序控制流程.
* 线程又被称为`轻量级进程`(lightweight process) 
* 如果在一个进程中同时运行了多个线程，用来完成不同的工作，则称之为`多线程`
## 线程特点
* 轻量级进程 
* 独立调度的基本单位 
* 可并发执行
* 共享进程资源
## 线程和进程的区别
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404164136536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
## 线程的创建和启动
### 线程的创建 
#### 方式1：继承Thread类
* 继承Java.lang.Thread类，并覆盖run() 方法 
```java
public class TestThread extends Thread{
    public void run(){
        for(int i=1;i<100;i++){
            System.out.println( Thread.currentThread().getName() + "  = " + i);
        }
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        //子线程
        TestThread testThread = new TestThread();
        testThread.start();
        
        //主线程
        for(int i = 0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
    }
}
```
#### 方式2：Runnable接口
* 实现Java.lang.Runnable接口，并实现run() 方法 
```java
public class TestRunnable implements Runnable{
    @Override
    public void run() {
        for(int i=1;i<100;i++){
            System.out.println( Thread.currentThread().getName() + "  = " + i);
        }
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        //子线程:通过Thread类执行TestRunnable类
        Thread thread = new Thread(new TestRunnable());
        thread.start();

        //主线程
        for(int i = 0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
    }
}
```
* **方式2的匿名内部类写法**

```java
public class Test {
    public static void main(String[] args) {
        //子线程:匿名内部类写法
        new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i=1;i<100;i++){
                    System.out.println( Thread.currentThread().getName() + "  = " + i);
                }
            }
        }).start();

        //主线程
        for(int i = 0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
    }
}
```

* **方法run( )称为线程体。**
#### 两种线程创建方式的比较 
* 继承Thread类方式的多线程 
	* 优势：编写简单 
	* 劣势：无法继承其它父类 
* 实现Runnable接口方式的多线程
	* 优势：可以继承其它类，多线程可共享同一个Runnable对象 
	* 劣势：编程方式稍微复杂，如果需要访问当前线程，需要调用Thread.currentThread()方 法 
* `实现Runnable接口方式要通用一些`
---------
* Thread类常用方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404164813255.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
#### 方式3：实现Callable接口 
```java
public class TestCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
        return "子线程执行成功！";
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        //子线程：实现Callable接口
        //创建FutureTask实例，实现TestCallable实例
        FutureTask<String> task = new FutureTask<String>(new TestCallable());
        //创建Thread实例，执行FutureTask
        Thread thread = new Thread(task);
        thread.start();

        //主线程
        for(int i = 0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
    }
}
```
* 实现Callable
	* 与实行Runnable相比， Callable功能更强大些
	* 方法不同 
	* 可以有返回值，支持泛型的返回值 
	* 可以抛出异常 
	* 需要借助FutureTask，比如获取返回结果 
-----------
* Future接口 
	* 可以对具体Runnable、Callable任务的执行结果进行取消、查询是否完成、获取结果等。 
	* FutrueTask是Futrue接口的唯一的实现类 
	* FutureTask 同时实现了Runnable, Future接口。它既可以作为Runnable被线程执行，又可以作为 Future得到Callable的返回值
####  方式4：线程池 Executor
```java
public class TestRunnable implements Runnable{
    @Override
    public void run() {
        for(int i=1;i<100;i++){
            System.out.println( Thread.currentThread().getName() + "  = " + i);
        }
    }
}
```
```java
public class Test {
    public static void main(String[] args) {
        //子线程：使用线程池创建线程
        //1.使用Executors获取线程池对象
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        //通过线程池对象获取线程并执行TestRunnable实例
        executorService.execute(new TestRunnable());

        //主线程
        for(int i = 0;i<100;i++){
            System.out.println(Thread.currentThread().getName()+"="+i);
        }
    }
}
```
### 线程的启动 
* 新建的线程不会自动开始运行，必须通过start( )方法启动 
* 不能直接调用run()来启动线程，这样run()将作为一个普通方法立即执行，执行完毕前其他线程无法并发执行 
* Java程序启动时，会立刻创建主线程，main就是在这个线程上运行。当不再产生新线程时， 程序是单线程的
## 线程同步
> 线程同步：当两个或两个以上线程访问同一资源时，需要某种方式来确保资源在某一时刻只被一个线程 使用 。当多个线程访问同一个数据时，容易出现线程安全问题。需要让线程同步，保证数据安全 
### 线程同步的实现方案
* **同步代码块** 
	* synchronized (obj){ } 
* **同步方法** 
	* private synchronized void makeWithdrawal(int amt) {}
### 同步监视器 
* 同步监视器 
	* `synchronized (obj){ }`中的obj称为同步监视器 
	* 同步代码块中同步监视器可以是任何对象，但是推荐使用共享资源作为同步监视器 
	* 同步方法中无需指定同步监视器，因为同步方法的同步监视器是this，也就是该对象本事 
* 同步监视器的执行过程 
	* 第一个线程访问，锁定同步监视器，执行其中代码 
	* 第二个线程访问，发现同步监视器被锁定，无法访问 
	* 第一个线程访问完毕，解锁同步监视器 
	* 第二个线程访问，发现同步监视器未锁，锁定并访问
###  锁机制
* **Lock锁**
	* JDK1.5后新增功能，与采用`synchronized`相比，`lock`可提供多种锁方案，更灵活 
	* `java.util.concurrent.lock` 中的 `Lock` 框架是锁定的一个抽象，它允许把锁定的实现作为 Java 类，而不是作为语 言的特性来实现。这就为 Lock 的多种实现留下了空间，各种实现可能有不同的调度算法、性能特性或者锁 定语义。 
	* `ReentrantLock` 类实现了 `Lock` ，它拥有与 `synchronized` 相同的并发性和内存语义， 但是添加了类似锁投票、 定时锁等候和可中断锁等候的一些特性。此外，它还提供了在激烈争用情况下更佳的性能。 
* 注意：`如果同步代码有异常，要将unlock()写入finally语句块`
-----------
*   **Lock和synchronized的区别** 
	* 1.Lock是显式锁（手动开启和关闭锁，别忘记关闭锁），`synchronized`是隐式锁 
	* 2.Lock只有代码块锁，`synchronized`有代码块锁和方法锁 
	* 3.使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多的子类）
* **优先使用顺序：** 
	* Lock----同步代码块（已经进入了方法体，分配了相应资源）----同步方法（在方法体之外）
### 优缺点
* 线程同步的好处 
	* 解决了线程安全问题 
* 线程同步的缺点 
	* 性能下降 
	* 会带来死锁 
* 死锁
	* 当两个线程相互等待对方释放“锁”时就会发生死锁 
	* 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续 
	* 多线程编程时应该注意避免死锁的发生
## 线程通信
> 线程通信：多个线程因为在同一个进程中，所以互相通信比较容易的。
* 线程通信的经典模型：
	* 生产者与消费者问题。
    * 生产者负责生成商品，消费者负责消费商品。
    * 生产不能过剩，消费不能没有。
* 注意
	* 线程通信一定是多个线程在操作同一个资源才需要进行通信。
   * 线程通信必须先保证线程安全，否则毫无意义，代码也会报错！

> Java提供了3个方法解决线程之间的通信问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404171150626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
* 线程通信的核心方法：
	* public void wait(): 让当前线程进入到等待状态 此方法必须锁对象调用.
    * public void notify() : 唤醒当前锁对象上等待状态的某个线程  此方法必须锁对象调用
     * public void notifyAll() : 唤醒当前锁对象上等待状态的全部线程  此方法必须锁对象调用
* 小结：
     * 是一种等待唤醒机制。
     * 必须是在同一个共享资源才需要通信，而且必须保证线程安全。
## 线程控制方法
> Java提供一个线程调度器来监控程序中启动后进入就绪状态的所有线程。线程调度器 按照线程的优先级决定应调度哪个线程来执行。
* 
	* 线程的优先级用数字表示，范围从1到10 
		* Thread.MIN_PRIORITY = 1 
		* Thread.MAX_PRIORITY = 10 
		* Thread.NORM_PRIORITY = 5 
	* 使用下述方法获得或设置线程对象的优先级。 
		* int getPriority(); 
		* void setPriority(int newPriority); 
* 注意：`优先级低只是意味着获得调度的概率低。并不是绝对先调用优先级高后调用 优先级低的线程。`
-----------
* `join ()`
	* 阻塞指定线程等到另一个线程完成以后再继续执行 
* `sleep ()`
	* 使线程停止运行一段时间，将处于阻塞状态 
	* 如果调用了sleep方法之后，没有其他等待执行的线程，这个时候当前线程不会马上恢复执行 
* `yield ()` 
	* 让当前正在执行线程暂停，不是阻塞线程，而是将线程转入就绪状态 
	* 如果调用了yield方法之后，没有其他等待执行的线程，这个时候当前线程就会马上恢复执行！ 
* `setDaemon()` 
	* 可以将指定的线程设置成后台线程 
	* 创建后台线程的线程结束时，后台线程也随之消亡 
	* 只能在线程启动之前把它设为后台线程 
* `interrupt()` 
	* 并没有直接中断线程，而是需要被中断线程自己处理
* `stop()` 
	* 结束线程，不推荐使用
##  线程的生命周期
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200404165134478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
* `初始状态`
	* 用new关键字建立一个线程对象后，该线程对象就处于初始状态。
	* 处于初始状态的线程有自己的内存空间，通过调用start进入就绪状态 
* `就绪状态`
	* 处于就绪状态线程具备了运行条件，但还没分配到CPU，处于线程就绪队列，等待系统为其分配CPU 
	* 当系统选定一个等待执行的线程后，它就会从就绪状态进入执行状态，该动作称之为“cpu调度”。 
* `运行状态`
	* 在运行状态的线程执行自己的run方法中代码，直到等待某资源而阻塞或完成任务而死亡。 
	* 如果在给定的时间片内没有执行结束，就会被系统给换下来回到等待执行状态。 
* `阻塞状态`
	* 处于运行状态的线程在某些情况下，如执行了sleep（睡眠）方法，或等待I/O设备等资源，将让出CPU并暂时停止自己的运行，进 入阻塞状态。 
	* 在阻塞状态的线程不能进入就绪队列。只有当引起阻塞的原因消除时，如睡眠时间已到，或等待的I/O设备空闲下来，线程便转入 就绪状态，重新到就绪队列中排队等待，被系统选中后从原来停止的位置开始继续运行。 
* `终止状态`
	* 终止状态是线程生命周期中的最后一个阶段。线程终止的原因有三个。一个是正常运行的线程完成了它的全部工作；另一个是线 程被强制性地终止，如通过执行stop方法来终止一个线程[不推荐使用】，三是线程抛出未捕获的异常
## 线程池
### 什么是线程池 
> 线程池：其实就是一个容纳多个线程的容器，其中的线程可以反复使用，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源。

* 创建和销毁对象是非常耗费时间的 
* 创建对象：需要分配内存等资源 
* 销毁对象：虽然不需要程序员操心，但是垃圾回收器会在后台一直跟踪并销毁 
* 对于经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大。 
* `思路`：创建好多个线程，放入线程池中，使用时直接获取引用，不使用时放回池中。可以避 免频繁创建销毁、实现重复利用 
* `技术案例`：线程池、数据库连接池 
* JDK1.5起，提供了内置线程池
###  线程池的工作原理
* **当提交一个新任务到线程池时，线程池的处理流程如下**：
	* 线程池判断核心线程池里的线程是否都在执行任务。如果不是，则创建一个新的工作线程来执行任务。如果核心线程池里的线程都在执行任务，则进入下个流程。线程池判断工作队列是否已经满。如果工作队列没有满，则将新提交的任务存储在这个工作队列里。如果工作队列满了，则进入下个流程。线程池判断线程池的线程是否都处于工作状态。如果没有，则创建一个新的工作线程来执行任务。如果已经满了，则交给饱和策略来处理这个任务。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405182218140.bmp?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70#pic_center)
### 线程池的好处 
* 提高响应速度（减少了创建新线程的时间） 
* 降低资源消耗（重复利用线程池中线程，不需要每次都创建） 
* 提高线程的可管理性：避免线程无限制创建、从而销耗系统资源，降低系统稳定性，甚至内存溢出或者CPU耗尽 
###   线程池的使用
* `Executor`：线程池顶级接口，只有一个方法 
* `ExecutorService`：真正的线程池接口 
	* void execute(Runnable command) ：执行任务/命令，没有返回值，一般用来执行Runnable
	* <T> Future<T> submit(Callable<T> task)：执行任务，有返回值，一般又来执行Callable 
	* void shutdown() ：关闭连接池 
* `AbstractExecutorService`：基本实现了ExecutorService的所有方法 
* `ThreadPoolExecutor`：默认的线程池实现类 
 * `ScheduledThreadPoolExecutor`：实现周期性任务调度的线程池 
* `Executors`：工具类、线程池的工厂类，用于创建并返回不同类型的线程池 
	* Executors.newCachedThreadPool()：创建一个可根据需要创建新线程的线程池 
	* Executors.newFixedThreadPool(n); 创建一个可重用固定线程数的线程池 
	* Executors.newSingleThreadExecutor() ：创建一个只有一个线程的线程池 
	* Executors.newScheduledThreadPool(n)：创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。
	* 要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，很有可能配置的线程池不是较优的，因此在`java.util.concurrent.Executors`线程工厂类里面提供了一些静态工厂，生成一些常用的线程池。官方建议使用Executors工程类来创建线程池对象。
	* Executors类中有个创建线程池的方法如下：
		- `public static ExecutorService newFixedThreadPool(int nThreads)`：返回线程池对象。(创建的是有界线程池,也就是池中的线程个数可以指定最大数量)
* 获取到了一个线程池ExecutorService 对象，那么怎么使用呢，在这里定义了一个使用线程池对象的方法如下：
	- `public Future<?> submit(Runnable task)`:获取线程池中的某一个线程对象，并执行Future接口：用来记录线程任务执行完毕后产生的结果。

**使用线程池中线程对象的步骤：**

1. 创建线程池对象。
2. 创建Runnable接口子类对象。(task)
3. 提交Runnable接口子类对象。(take task)
4. 关闭线程池(一般不做)。
### 线程池的应用场合 
* 需要大量线程，并且完成任务的时间端 
* 对性能要求苛刻
* 接受突发性的大量请求

### 线程池参数 
* corePoolSize：核心池的大小 
	* 默认情况下，创建了线程池后，线程数为0，当有任务来之后，就会创建一个线程去执行任务。
	* 但是当线程池中线程数量达到corePoolSize，就会把到达的任务放到队列中等待。
* maximumPoolSize：最大线程数。 
	* corePoolSize和maximumPoolSize之间的线程数会自动释放，小于等于corePoolSize的不会释放。当大于了 这个值就会将任务由一个丢弃处理机制来处理。
* keepAliveTime：线程没有任务时最多保持多长时间后会终止
	* 默认只限于corePoolSize和maximumPoolSize之间的线程 
* TimeUnit： 
	* keepAliveTime的时间单位 
* BlockingQueue： 
	* 存储等待执行的任务的阻塞队列，有多中选择，可以是顺序队列、链式队列等。
* ThreadFactory 
	* 线程工厂，默认是DefaultThreadFactory，Executors的静态内部类
* RejectedExecutionHandler： 
	* 拒绝处理任务时的策略。如果线程池的线程已经饱和，并且任务队列也已满，对新的任 务应该采取什么策略。
	* 比如抛出异常、直接舍弃、丢弃队列中最旧任务等，默认是直接抛出异常。 
		* 1、CallerRunsPolicy：如果发现线程池还在运行，就直接运行这个线程 
		* 2、DiscardOldestPolicy：在线程池的等待队列中，将头取出一个抛弃，然后将当前线程放进去。
		* 3、DiscardPolicy：什么也不做 
		* 4、AbortPolicy：java默认，抛出一个异常

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
