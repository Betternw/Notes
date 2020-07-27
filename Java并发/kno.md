题：https://www.jianshu.com/p/3e88a5fe75f0
## 一 使用线程
1.  任务是通过线程驱动而执行的。
2.  使用线程的方法：
* 实现runnable接口——任务，通过线程调用
* 实现callable接口——任务，通过线程调用
* 继承Thread类
#### 实现Runnable接口
1. 定义一个类，实现runnable接口，重写run方法
2. 使用runnable实例创建Thread实例，调用Thread的start的方法来启动线程。
3. 无返回值，无法捕获异常处理。
```java
   public class MyRunnable implements Runnable {
       public void run() {
       // ...
       }
   }
   public static void main(String[] args) {
       MyRunnable instance = new MyRunnable();
       Thread thread = new Thread(instance);
       thread.start();
   }
```
#### 实现Callable接口
1. 定义一个类，实现Callable接口，重写run方法，有返回值
2. 使用FutureTask封装返回值对象，然后用该对象创建Thread实例，最后调用Thread的start方法启动线程。
```java
   public class MyCallable implements Callable<Integer> {
       public Integer call() {
           return 123;
       }
   }
   public static void main(String[] args) throws ExecutionException, InterruptedException {
       MyCallable mc = new MyCallable();
       FutureTask<Integer> ft = new FutureTask<>(mc);
       Thread thread = new Thread(ft);
       thread.start();
       System.out.println(ft.get());
   }
```
#### 继承Thread类
1. 继承Thread类，实现run方法
2. start方法启动线程
```java
   public class MyThread extends Thread {
       public void run() {
       // ...
       }
   }
   public static void main(String[] args) {
       MyThread mt = new MyThread();
       mt.start();
   }
```
#### 线程池
1. Executors提供方法，实现ExecutorService接口。
```java
public class MyRunnable implements Runnable {
    public void run() {
    // ...
    }
}
public static void main(String[] args) {
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    MyRunnable runnableTest = new MyRunnable();
    for (int i = 0; i < 5; i++) {
        executorService.execute(runnableTest);
    }
    executorService.shutdown();
}
```
#### run()和start()
1. start()用于启动线程让线程进入就绪状态，只能调用一次，会自动执行run()；
2. run()用于执行线程内部代码，类似main()下的普通方法，可重复调用，依赖于线程start()。
#### 实现接口vs继承Thread
实现接口会更好，因为可以继承多个接口，但是继承类，不仅不支持多重继承，还会使因为继承整个类而开销过大

## 二 基础线程机制
#### 线程调度模型
  * 分时调度：让所有线程轮流获得CPU使用权，平均分配时间片。
  * 抢占式调度：让优先级高的线程抢占CPU，直到有更高优先级线程进入或线程任务运行。JVM默认。

#### Executor
1. 管理多个异步任务的执行，无需显式地管理线程的生命周期。
2. CachedThreadPool：一个任务创建一个线程；
3. FixedThreadPool：所有任务只能使用固定大小的线程；
4. SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。

#### Daemon
1. 守护线程
2. 当程序运行时在后台提供服务，不可或缺
3. 非守护程序结束时，守护线程也会被杀死
4. main（）属于非守护线程
5. 在线程启动之前使用setDaemon（）方法将一个线程设置为守护线程

#### sleep（）锁——暂停执行

1. 休眠正在执行的线程，单位为毫秒。时间结束后退出。
2.  可能会抛出 InterruptedException，因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理
3.  sleep使线程陷入睡眠状态，调用中断方法会使线程结束阻塞状态
4.  sleep和wait方法的最大区别:sleep睡眠时会保持对象锁，仍然占有该锁，wait睡眠时释放对象锁。锁：控制同步访问。
5.  Thread类的方法，让线程进入有限期等待休眠，之后自动苏醒。休眠不释放锁。常用于暂停执行。

#### yield（）——cpu
1. 静态方法，当线程重要部分完成后，调用yield方法就可以切换给其他线程执行
2. 让线程放弃当前获得的CPU，使线程仍处于Runnable，随时可以再获得CPU。只给相同优先级或更高优先级的线程机会。
3.  sleep 方法使当前运行中的线程睡眼一段时间，进入不可运行状态，这段时间的长短是由程序设定的，yield 方法使当前线程让出 CPU 占有权，但让出的时间是不可设定的。
4.  实际上，yield()方法对应了如下操作：先检测当前是否有相同优先级的线程处于同可运行状态，如有，则把 CPU  的占有权交给此线程，否则，继续运行原来的线程。所以yield()方法称为“退让”，它把运行机会让给了同等优先级的其他线程。

## 三 线程之间的协作
当多个线程一起工作时，存在线程的先后问题，因此需要协调
#### join（）——全部
1. 当前线程调用，其他线程全部停止，等待当前线程执行结束再执行

#### wait（）——锁
1. Object类的方法，与synchronized一起使用，
2. 线程进入有限期或无限期等待，被notify方法调用才能解除阻塞，
3. 只有重新占用互斥锁才能进入Runnable。休眠释放互斥锁。常用于线程间通信交互。
4. wait(long timeout)超时后也会自动苏醒。
#### wait（）notify（）notifyAll（）
1. 调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程
2. 属于object类，不属于线程类
3. 只能在同步方法或者同步控制块中使用
4. 使用wait方法时，会释放锁，不然其他线程无法进入对象的同步方法或者同步控制块中，从而无法唤醒。
5. 对比：wait是object的方法，sleep是thread的方法。wait会释放锁，sleep不会释放锁。
6. notify()：唤醒一个线程。
7. notifyAll()：唤醒所有线程，参与锁竞争，失败就留在池中等待下次唤醒。
 #### await（）signal（）signalAll（）
 1. Condition类可以实现线程之间的协调
 2. await方法使线程等待，其他线程调用signal或signalAll来唤醒等待的线程
 3. 对比：await这种等待方式可以指定等待的条件

## 四 中断
在运行过程中发生异常会提前结束
#### 停止运行线程方法
1. 退出标志，让线程正常退出；
2. stop()/suspend()强制终止；
3. interrupt()中断线程，但仅是把逻辑状态设置为中断，不会停止线程，需要后续处理。
#### InterruptedException
1. 阻塞状态——线程失去CPU使用权，直到进入就绪状态才有机会转到运行状态
* 等待阻塞：运行的线程执行wait（）方法，jvm将线程放入等待池中
* 同步阻塞：运行的线程在获取对象的同步锁时，若锁被别的线程占用，则jvm将线程放入锁池中
* 其他阻塞：运行的线程执行sleep（）或者join（）方法，或者发出了IO请求时，jvm将线程设置为阻塞。
2. interrupt(): 不要以为它是中断某个线程！它只是线线程发送一个中断信号，让线程在无限等待时（如死锁时）能抛出，从而结束线程，但是如果你吃掉了这个异常，那么这个线程还是不会中断的！中断线程。调用该方法后，线程状态被置为中断，不会停止线程，抛出中断异常。
3. 通过调用一个线程的 interrupt() 来中断该线程，如果该线程处于阻塞、限期等待或者无限期等待状态，那么就会抛出 InterruptedException，从而提前结束该线程。但是不能中断 I/O 阻塞和 synchronized 锁阻塞（死锁只有在获得锁才能结束阻塞）
4. 如果一个线程处于了阻塞状态（如线程调用了thread.sleep、thread.join、thread.wait、1.5中的condition.await、以及可中断的通道上的 I/O 操作方法后可进入阻塞状态），则在线程在检查中断标示时如果发现中断标示为true，则会在这些阻塞方法（sleep、join、wait、1.5中的condition.await及可中断的通道上的 I/O 操作方法）调用处抛出InterruptedException异常
#### interrupted（）
1. 如果run方法无限循环，并且没有执行sleep方法等可以抛出异常的操作，interrupt方法就无法使线程提前结束
2. 调用 interrupt() 方法会设置线程的中断标记，此时调用 interrupted() 方法会返回 true。因此可以在循环体中使用 interrupted() 方法来判断线程是否处于中断状态，从而提前结束线程。
#### 对比
1. interrupt方法用于中断线程，调用该方法将线程的状态设置为“中断”状态，仅仅是设置中断状态位，不会中断，设置中断状态位后悔抛出中断异常.中断线程。调用该方法后，线程状态被置为中断，不会停止线程，抛出中断异常。
2. interrupt（）是用来设置中断状态的。返回true说明中断状态被设置了而不是被清除了。我们调用sleep、wait等此类可中断（throw InterruptedException）方法时，一旦方法抛出InterruptedException，当前调用该方法的线程的中断状态就会被jvm自动清除了，就是说我们调用该线程的isInterrupted 方法时是返回false。如果你想保持中断状态，可以再次调用interrupt方法设置中断状态。这样做的原因是，java的中断并不是真正的中断线程，而只设置标志位（中断位）来通知用户。如果你捕获到中断异常，说明当前线程已经被中断，不需要继续保持中断位
3. interrupt方法：设置一个中断状态，线程仍然会继续运行。对于正在运行的普通代码，不会抛出异常，对于处于wait/sleep/join的线程，调用interrupt方法会打破线程的暂停状态抛出异常，用户可以选择return来结束线程。
4. interrupted：测试当前是否被中断（检查中断状态），返回一个boolean并清除中断状态.是静态方法，检查当前中断状态，并清除中断状态。如果一个线程被中断了，第一次调用 interrupted 则返回 true，第二次和后面的就返回 false 了。
5. isinterrupted：查看当前线程的中断状态，不清除状态。


## 五 互斥同步
多个线程对共享资源的互斥访问。
*  jvm实现的synchronized
* jdk实现的ReentrantLock
#### 线程同步方式
* 同步方法。用synchronized修饰的方法。
* 同步代码块。用synchronized修饰的语句块。
* 用volatile修饰变量。
* 可重入锁。
* ThreadLocal管理变量副本。
* 阻塞队列。
* 原子类。
#### synchronized（不可中断、不公平）
1. 同步一个代码块
* 只作用于一个对象，如果调用两个对象上的同步代码块，不会进行同步
* 使用ExecutorService 执行两个线程，调用同一个对象的同步代码，即使是两个线程，也可以实现同步。当一个线程进入同步语句块时，另一个线程就必须等待。
* 使用ExecutorService 执行两个线程，调用两个对象的同步代码块，这两个线程不会实现同步，结果交替执行
2. 同步一个方法——作用于同一个对象
3. 同步一个类——两个线程调用同一个类的不同对象的同步语句时，会进行同步
4. 同步一个静态方法——作用于整个类

#### ReentrantLock（可中断、默认非公平）
是java.util.concurrent（J.U.C）包中的锁。
#### 比较
1. 锁的实现：synchronized 是 JVM 实现的，而 ReentrantLock 是 JDK 实现的。
2.  等待可中断当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。ReentrantLock 可中断，而 synchronized 不行。
3.  公平锁：公平锁是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁。synchronized 中的锁是非公平的，ReentrantLock 默认情况下也是非公平的，但是也可以是公平的。
4.  锁绑定多个条件一个 ReentrantLock 可以同时绑定多个 Condition 对象。
#### 使用选择
优先使用synchronized，因为jvm原生支持，并且不用担心死锁问题，因为jvm会确保锁的释放
## 六 线程状态
线程状态指的是java虚拟机中的线程状态

1. 新建（New）——创建后尚未启动
2. 可运行（Runable）——虚拟机中：在运行。os中——运行或者等待调度中，完成资源调度后进入运行状态
3. 阻塞等待
   * 阻塞（Blocked）——属于被动，线程进入同步块中，需要申请一个同步锁而进行的等待当获得锁后可以进入运行状态
   * 无限期等待(Waiting)——属于主动，调用Object.wait方法（Object.notify）或者join方法（执行完毕）进入等待状态，然后无限期等待其他线程来唤醒
   * 限期等待（Timed_Waiting）——调用Thread.sleep（时间到）进入等待状态，不等待其他线程显式的唤醒，在一定时间后被系统自动唤醒。等待时间是明确的
5. 死亡（Terminated）——线程任务结束后自己退出或者产生异常结束。
## 七 JUC—AQS
java.util.concurrent（J.U.C）大大提高了并发性能，AQS 被认为是 J.U.C 的核心
#### CountDownLatch
1. 用来控制一个或者多个线程等待多个线程
2. 维护了一个计数器 cnt，线程每次调用 countDown() 方法会让计数器的值减 1，减到 0 的时候，那些因为调用 await() 方法而在等待的线程就会被唤醒
3. 计数器初始值为线程的数量。当每一个线程完成自己任务后，计数器的值就会减一。当计数器的值为0时，表示所有的线程都已经完成一些任务，然后在CountDownLatch上等待的线程就可以恢复执行接下来的任务
4. 

#### CyclicBarrier
1. 控制多个线程相互等待，当多个线程都到达时这些线程才会继续执行
2. 维护计数器，当调用await方法后，计数器减1并进行等待，直到计数器为0，结束等待继续执行。
3. 可以调用reset方法进行循环使用。
4. 参数parties指让多少个线程或者任务等待至barrier状态；参数barrierAction为当这些线程都达到barrier状态时会执行的内容
5. 当线程到达栅栏位置时候，执行await，内部变量count减1，如果count！= 0，说明有线程还未到屏障处，则在锁条件变量trip上等待。当count == 0时，说明所有线程都已经到屏障处，执行条件变量的signalAll方法唤醒等待的线程。

#### Semaphore
1. 类似于信号量，控制对互斥资源的访问线程数

## 八 JUC其他组件
#### FutureTask
1. Callable接口有返回值，通过Future进行封装。而FutureTask实现了RunnableFuture接口，该接口继承自Runnable和Future接口。
2. 用于异步获取执行结果或者取消执行任务的场景。当一个计算任务需要执行很长时间，可以用FurureTask封装这个任务，主线程在完成自己的任务之后再去偶去结果。

#### BlockingQueue
1. 该接口可以实现阻塞队列：FIFO队列和优先级队列
2. take（）方法：队列为空时，take（）将阻塞直到队列中有内容
3. put（）方法：队列满时，put（）阻塞，直到对流有空闲位置
4. 可以实现生产者消费者问题

#### ForkJoin
1. 用于并行计算，把大的计算任务拆分成多个小的任务进行计算
2. ForkJoin 使用 ForkJoinPool 来启动，它是一个特殊的线程池，线程数量取决于 CPU 核数。
3. 每个线程维护一个双端队列
4. 工作窃取算法：允许空闲的线程从其他线程的双端队列中窃取一个任务来执行，窃取的任务必须是最晚的任务，避免发生竞争

## 九 线程不安全示例
多个线程对同一个共享数据进行访问而不采取同步操作会造成操作的结果不一致

## 十 Java内存模型
#### 主内存和工作内存
1. 处理器的读写速度大于内存的读写速度，为了解决这种速度矛盾，加入了高速缓存
2. 缓存会存在缓存一致性问题：多个缓存共享一块内存区域，数据会不一致。因此需要使用协议。
3. 线程有自己的工作内存，储存在高速缓存中。变量储存在主内存中，工作内存中有变量的副本拷贝。
4. 线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存完成。

#### 内存间交互操作
1. 主内存将变量的值从主内存传输到工作内存中，
2. 将得到的值放入工作内存的变量副本中
3. 将工作内存中一个变量的值传给执行引擎
4. 把执行引擎接收到的值赋给工作内存的变量
5. 把工作内存的一个变量的值传送到主内存中
6. 将得到的值放入主内存的变量中
![4c0f7d15f8e088ea1e4d837e9a3fd6fe.png](en-resource://database/4119:1)

#### 内存模型三大特性
1. 原子性
* 指一个操作不可中断，要么成功要么失败
* read、load、use、assign、store、write、lock 和 unlock 操作具有原子性
* 允许虚拟机将没有被 volatile 修饰的 64 位数据（long，double）的读写操作划分为两次 32 位的操作来进行，即 load、store、read 和 write 操作可以不具备原子性
* int 类型读写操作满足原子性只是说明 load、assign、store 这些单个操作具备原子性。
* 使用原子类AtomicInteger可以保证操作的原子性——CAS
* 使用 synchronized 互斥锁来保证操作的原子性。它对应的内存间交互操作为：lock 和 unlock，在虚拟机实现上对应的字节码指令为 monitorenter 和 monitorexit。
2. 可见性
* 指当一个变量修改了共享变量的值，其他线程能够得知这个修改
* 可见性实现： 在变量修改后，将新值同步回主内存，在读取变量前，在主内存刷新变量值。
* volatile实现可见性，不能保证操作的原子性。没有被 volatile 修饰的 64 位数据（long，double）的读写操作可以划分为两次 32 位的操作来进行，即 load、store、read 和 write 操作可以不具备原子性。（没有被修饰的可以没有原子性，被修饰的也不一定具有原子性）
* synchronized实现可见性：对一个变量执行 unlock 操作之前，必须把变量值同步回主内存。
* final实现可见性，被 final 关键字修饰的字段在构造器中一旦初始化完成，并且没有发生 this 逃逸（其它线程通过 this 引用访问到初始化了一半的对象），那么其它线程就能看见 final 字段的值。
3. 有序性
* 在本线程内观察所有操作都是有序的。观察另一个操作都是无序的
* 编译器和处理器会对指令进行重排序，这样会影响多线程并发执行的正确性
* volatile可以添加内存屏障，即重排序不能把后面的指令放到内存屏障之前
* 通过 synchronized 来保证有序性，它保证每个时刻只有一个线程执行同步代码，相当于是让线程顺序执行同步代码

#### 先行发生原则
一个操作无需控制就能先于另一个操作完成

1. 单一线程原则——在一个线程内，在程序前面的操作先行发生于后面的操作
2. 管程锁定原则——一个unlock操作先行于后面对同一个锁的lock操作
3. volatile变量原则——对一个volatile变量的写操作先行发生于后面的读操作
4. 线程启动原则——start（）方法调用先行于每一个动作
5. 线程加入机制——对象的结束在join方法返回之前。（因为是挂起另一个线程，另一个结束后，join才返回）
6. 线程中断规则——对线程 interrupt() 方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过 interrupted() 方法检测到是否有中断发生
7. 对象终结原则——一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始
8. 传递性——操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C。
## 十一 线程安全
#### 不可变
1. 不可变的对象绝对线程安全
2. final类型
3. String
4. 枚举类型
5. Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。但同为 Number 的原子类 AtomicInteger 和 AtomicLong 则是可变的

#### 互斥同步

synchronized 和 ReentrantLock。

#### 非阻塞同步
* 互斥同步的线程阻塞和唤醒会有性能问题，因此也成为阻塞同步
* 互斥同步是一种悲观的并发策略，不论是否会有竞争都会加锁。
* 乐观并发=非阻塞同步：先进行操作，如果没有其他线程争用共享数据就成功，如果有就采取补偿措施
1. CAS
* 需要操作和冲突检测这两个步骤具备原子性
* 靠硬件完成
* 比较并交换（Compare-and-Swap，CAS）。CAS 指令需要有 3 个操作数，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B
* CAS的语义是“我认为V的值应该为A，如果是，那么将V的值更新为B，否则不修改并告诉V的值实际为多少”，CAS是项乐观锁技术，当多个线程尝试使用CAS同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。
* 当我所认为的A与实际上内存的V相同的时候，就把新的值B给V，否则就失败重来。 
* CAS 用于同步的方式是从地址 V 读取值 A，执行多步计算来获得新 值 B，然后使用 CAS 将 V 的值从 A 改为 B。如果 V 处的值尚未同时更改，则 CAS 操作成功
* CAS 的指令允许算法执行读-修改-写操作，而无需害怕其他线程同时 修改变量，因为如果其他线程修改变量，那么 CAS 会检测它（并失败），算法 可以对该操作重新计算
2. AtomicInteger
* 整数原子类 AtomicInteger 的方法调用了 Unsafe 类的 CAS 操作
3. ABA
* 如果一个变量的值从A变到B变到A，CAS会以为没有变过
* 带有标记的原子引用类 AtomicStampedReference 通过控制变量值的版本来保证CAS的正确性
#### 无同步方案
如果一个方案不涉及共享操作，那么就不需要同步操作
1. 栈封闭（局部变量）——多个线程访问同一个方法的局部变量时，不会出现线程安全问题。因为局部变量存储在虚拟机栈中，属于线程私有的。
2. 线程本地存储
* 如果需要共享数据，看是否能将需要共享数据的代码保证在同一个线程中执行。
* 从本质来讲，就是每个线程都维护了一个map，而这个map的key就是threadLocal，而值就是我们set的那个值，每次线程在get的时候，都从自己的变量中取值，既然从自己的变量中取值，那肯定就不存在线程安全问题，总体来讲，ThreadLocal这个变量的状态根本没有发生变化，他仅仅是充当一个key的角色，另外提供给每一个线程一个初始值。
* 使用后需要手动remove（）
3. 可重用代码
* 纯代码（Pure Code），可以在代码执行的任何时刻中断它，转而去执行另外一段代码（包括递归调用它本身），而在控制权返回后，原来的程序不会出现任何错误

## 十二 锁优化
指 JVM 对 synchronized（同步锁） 的优化。
#### 自旋锁

1. 在请求锁的时候自旋一段时间，如果在这段时间可以获得锁，就可以避免进入阻塞状态
2. 循环会占用CPU时间，适用于共享数据锁定状态很短的场景。
3. 旋的次数由前一次在同一个锁上的自旋次数及锁的拥有者的状态来决定。
#### 锁消除
1. 对检测出不可能存在竞争的共享数据的锁进行消除
2. 通过逃逸分析来支持，如果堆上的共享数据不可能逃逸出去被其它线程访问到，那么就可以把它们当成私有数据对待，也就可以将它们的锁进行消除
#### 锁粗化
1. 如果对同一个对象进行频繁的加锁，就会扩大锁的范围，比如扩大这个操作开始前到结束后，就不需要每次执行都加锁解锁。
#### 轻量级锁
1. 锁拥有四个状态：无锁状态（unlocked）、偏向锁状态（biasble）、轻量级锁状态（lightweight locked）和重量级锁状态（inflated）
2. 轻量级锁是相对于传统的重量级锁而言，它使用 CAS 操作来避免重量级锁使用互斥量的开销。先采用 CAS 操作进行同步，如果 CAS 失败了再改用互斥量进行同步
3. 当尝试获取一个锁对象时，如果锁对象标记为 0 01，说明锁对象的锁未锁定（unlocked）状态。此时虚拟机在当前线程的虚拟机栈中创建 Lock Record，然后使用 CAS 操作将对象的 Mark Word 更新为 Lock Record 指针。如果 CAS 操作成功了，那么线程就获取了该对象上的锁，并且对象的 Mark Word 的锁标记变为 00，表示该对象处于轻量级锁状态。
#### 偏向锁
1. 偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至连 CAS 操作也不再需要
2. 当锁对象第一次被线程获得的时候，进入偏向状态，标记为 1 01。同时使用 CAS 操作将线程 ID 记录到 Mark Word 中，如果 CAS 操作成功，这个线程以后每次进入这个锁相关的同步块就不需要再进行任何同步操作
3. 当有另外一个线程去尝试获取这个锁对象时，偏向状态就宣告结束，此时撤销偏向（Revoke Bias）后恢复到未锁定状态或者轻量级锁状态
 ## 十三 多线程开发良好的实践

*  给线程起个有意义的名字，这样可以方便找 Bug。
*  缩小同步范围，从而减少锁争用。例如对于 synchronized，应该尽量使用同步块而不是同步方法。
*  多用同步工具少用 wait() 和 notify()。首先，CountDownLatch, CyclicBarrier, Semaphore 和 Exchanger 这些同步类简化了编码操作，而用 wait() 和 notify() 很难实现复杂控制流；其次，这些同步类是由最好的企业编写和维护，在后续的 JDK 中还会不断优化和完善。
*  使用 BlockingQueue 实现生产者消费者问题。
*  多用并发集合少用同步集合，例如应该使用 ConcurrentHashMap 而不是 Hashtable。
*  使用本地变量和不可变类来保证线程安全。
*  使用线程池而不是直接创建线程，这是因为创建线程代价很高，线程池可以有效地利用有限的线程来启动任务。