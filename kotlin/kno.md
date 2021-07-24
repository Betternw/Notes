### * [协程](#1)

## <span id = "1"> 协程</span>
### 一 协程介绍
#### 1. 功能点
1. 轻量：单个线程可以运行多个协程，协程支持挂起，不会使正在运行协程的线程阻塞，挂起比阻塞更加节省内存，且支持多个并行操作
2. 内存泄露更少：使用结构化并发机制在一个作用域内执行多个操作
3. 内置取消支持：会自动通过正在运行的协程层次结构传播

#### 2. 协程的建立
```java
    fun main() {
        GlobalScope.launch(context = Dispatchers.IO) {
            //延时一秒
            delay(1000)
            log("launch")
        }
        //主动休眠两秒，防止JVM过快退出
        Thread.sleep(2000)
        log("end")
    }
```

#### 3. 协程和线程的区别
1. 线程的子线程和主线程切换需要进行反复切换和回调，协程实际是一个线程框架，底层是通过线程池来实现的，但是比线程更加灵活。因为比线程多了一个上下文，所以后台任务执行完毕后可以自动切换会上下文协程中。
线程:
```java
    override fun initData() {
            thread {
                ioCode1()  //执行耗时操作1
                activity?.runOnUiThread {
                    uiCode1()  //更新UI1
                    thread {
                        ioCode2() //执行耗时操作2
                        activity?.runOnUiThread {
                            uiCode2()  //更新UI2
                            thread {
                                ioCode3()  //执行耗时操作3
                                activity?.runOnUiThread {
                                    uiCode3()  //更新UI3
                                }
                            }
                        }
                    }
                }
            }
        }
```

协程：
```java
    override fun initData() {
            GlobalScope.launch(Dispatchers.Main) {
                ioCode1() //耗时操作1
                uiCode1() //更新UI操作1
                ioCode2() //耗时操作2
                uiCode2() //更新UI操作2
                ioCode3() //耗时操作3
                uiCode3() //更新UI操作3
            }
        }


        suspend fun  ioCode1(){
            withContext(Dispatchers.IO){
                println("我是IO线程1==${Thread.currentThread().name}")
            }
        }

        suspend fun ioCode2(){
            withContext(Dispatchers.IO) {
                println("我是IO线程2==${Thread.currentThread().name}")
            }
        }

        suspend fun ioCode3(){
            withContext(Dispatchers.IO) {
                println("我是IO线程3==${Thread.currentThread().name}")
            }
        }
```

#### 4. 协程的四个基本概念
1. suspend function。即挂起函数，delay 函数就是协程库提供的一个用于实现非阻塞式延时的挂起函数
2. CoroutineScope。即协程作用域，GlobalScope 是 CoroutineScope 的一个实现类，用于指定协程的作用范围，可用于管理多个协程的生命周期，所有协程都需要通过 CoroutineScope 来启动
3. CoroutineContext。即协程上下文，包含多种类型的配置参数。Dispatchers.IO 就是 CoroutineContext 这个抽象概念的一种实现，用于指定协程的运行载体，即用于指定协程要运行在哪类线程上
4. CoroutineBuilder。即协程构建器，协程在 CoroutineScope 的上下文中通过 launch、async 等协程构建器来进行声明并启动。launch、async 等均被声明 CoroutineScope 的扩展方法

#### 5. suspend function
1. 用suspend修饰的函数就是挂起函数，将协程挂起，在特定的时候再回复协程
2. 例如，当在 ThreadA 上运行的 CoroutineA 调用了delay(1000L)函数指定延迟一秒后再运行，ThreadA 会转而去执行 CoroutineB，等到一秒后再来继续执行 CoroutineA。所以，ThreadA 并不会因为 CoroutineA 的延时而阻塞，而是能继续去执行其它任务，所以挂起函数并不会阻塞其所在线程，这样就极大地提高线程的并发灵活度，最大化线程的利用效率。
3. 而如果是使用Thread.sleep()的话，线程就真的只是白白消耗 CPU 时间片而不会去执行其它任务
4. suspend 函数只能由其它 suspend 函数调用，或者是由协程来调用
5. retrofit：当调用网络请求时，会暂停协程，在io线程池中运行代码，当网络请求完成后，恢复暂停的协程，使得主线程协程可以直接拿到网络请求结果而不用会用回调来通知主线程。get函数就是类似网络请求：
```java
    suspend fun fetchDocs() {                             // Dispatchers.Main
        val result = get("https://developer.android.com") // Dispatchers.IO for `get`
        show(result)                                      // Dispatchers.Main
    }

    suspend fun get(url: String) = withContext(Dispatchers.IO) { /* ... */ }

```
5. kotlin使用堆栈帧管理，当暂停协程时，会复制并保存当前的堆栈帧以供稍后使用。恢复时，会将堆栈帧从其保存位置复制回来，然后函数再次开始运行。
6. 在主线程进行的暂停协程和恢复协程的两个操作，既实现了将耗时任务交由后台线程完成，保障了主线程安全，又以同步代码的方式完成了实际上的多线程异步调用。解决了：处理耗时任务 (Long running tasks)，这种任务常常会阻塞住主线程、保证主线程安全 (Main-safety) ，即确保安全地从主线程调用任何 suspend 函数

#### 6. CoroutineScope
1. 协程作用域
2. GlobalScope。即全局协程作用域，在这个范围内启动的协程可以一直运行直到应用停止运行。GlobalScope 本身不会阻塞当前线程，且启动的协程相当于守护线程，不会阻止 JVM 结束运行
3. runBlocking。一个顶层函数，只有当内部相同作用域的所有协程都运行结束后，声明在 runBlocking 之后的代码才能执行，即 runBlocking 会阻塞其所在线程。但是整个线程其实不会阻塞，因为是早于GlobalScope进行输出的。
4. 自定义 CoroutineScope。可用于实现主动控制协程的生命周期范围，对于 Android 开发来说最大意义之一就是可以避免内存泄露

#### 7. CoroutineBuilder
1. launch：在不阻塞当前线程的情况下启动一个协程，并返回对该协程任务的引用。三个参数：协程的上下文、协程的启动方式（立即执行、延迟启动）、执行体，即协程执行的任务
2. async：可以返回协程的执行结果
#### 8. CoroutineContext
1. job：控制协程的生命周期
2. CoroutineDispatcher：指运行于哪个线程：Dispatchers.Main、Dispatchers.IO、Dispatchers.Default 
3. CoroutineName：为协程指定一个名字
4. CoroutineExceptionHandler：处理未捕获的异常
5. withContext：创造在指定线程中运行的代码块，一般配合 CoroutineDispatcher使用

### 二 取消协程
1. job.cancel()用于取消协程，job.join()用于阻塞等待协程运行结束

#### 1. 协程无法取消
1. 协程正在执行计算任务并且未检查是否已处于取消状态的话，就无法取消协程
2. 需要判断是否还处于可运行状态，当不可运行时候退出，isActive方法

#### 2. 用finally释放资源 —— try catch
#### 3. 父协程和子协程
1. 父协程等待所有子协程完成后再结束自身

