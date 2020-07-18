1. 什么是ANR 如何避免它？
   * 在Android上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应 用程序无响应（ANR：Application NotResponding）对话框。
   * 不同的组件发生ANR的时间不一样，Activity是5秒，BroadCastReceiver是10秒，Service是20秒（均为前台）
   * 主线程中存在耗时的计算、BroadcastReceiver未在10秒内完成相关的处理、Service在特定的时间内无法处理完成 20秒
   * 将所有耗时操作，比如访问网络，Socket通信，查询大 量SQL 语句，复杂逻辑计算等都放在子线程中去，使用AsyncTask、handler.sendMessage等方式更新UI
2. Activity和Fragment生命周期有哪些？https://camo.githubusercontent.com/85eb508cd8e5d6077f3a9f0fe9e513182e2d8474/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f323839333133372d643633353337373033313933613664312e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f
3. 横竖屏切换时候Activity的生命周期
   * 不设置Activity的android:configChanges时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。
   * 设置Activity的android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次
   * 设置Activity的android:configChanges=”orientation |keyboardHidden”时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法
4. AsyncTask的缺陷和问题，说说他的原理
   * AsyncTask是什么？
      * AsyncTask是一种轻量级的异步任务类，它可以在线程池中执行后台任务，然后把执行的进度和最终结果传递给主线程并在主线程中更新UI。
      * AsyncTask是一个抽象的泛型类，它提供了Params、Progress和Result这三个泛型参数，其中Params表示参数的类型，Progress表示后台任务的执行进度和类型，而Result则表示后台任务的返回结果的类型，如果AsyncTask不需要传递具体的参数，那么这三个泛型参数可以用Void来代替。

   * 关于线程池：
      * AsyncTask对应的线程池ThreadPoolExecutor都是进程范围内共享的，且都是static的，所以是Asynctask控制着进程范围内所有的子类实例。由于这个限制的存在，当使用默认线程池时，如果线程数超过线程池的最大容量，线程池就会爆掉(3.0后默认串行执行，不会出现个问题)。针对这种情况，可以尝试自定义线程池，配合Asynctask使用。

   * 关于默认线程池：
      * AsyncTask里面线程池是一个核心线程数为CPU + 1，最大线程数为CPU * 2 + 1，工作队列长度为128的线程池，线程等待队列的最大等待数为28，但是可以自定义线程池。线程池是由AsyncTask来处理的，线程池允许tasks并行运行，需要注意的是并发情况下数据的一致性问题，新数据可能会被老数据覆盖掉。所以希望tasks能够串行运行的话，使用SERIAL_EXECUTOR。

   * 一些问题：

1.生命周期
AsynTask会一直执行，直到doInBackground()方法执行完毕，然后，如果cancel(boolean)被调用,那么onCancelled(Result result)方法会被执行；否则，执行onPostExecute(Result result)方法。如果我们的Activity销毁之前，没有取消AsyncTask，这有可能让我们的应用崩溃(crash)。因为它想要处理的view已经不存在了。所以，我们是必须确保在销毁活动之前取消任务。总之，我们使用AsyncTask需要确保AsyncTask正确的取消。

2.内存泄漏

如果AsyncTask被声明为Activity的非静态内部类，那么AsyncTask会保留一个对Activity的引用。如果Activity已经被销毁，AsyncTask的后台线程还在执行，它将继续在内存里保留这个引用，导致Activity无法被回收，引起内存泄漏。

3.结果丢失

屏幕旋转或Activity在后台被系统杀掉等情况会导致Activity的重新创建，之前运行的AsyncTask会持有一个之前Activity的引用，这个引用已经无效，这时调用onPostExecute()再去更新界面将不再生效。

4.并行还是串行

在Android1.6之前的版本，AsyncTask是串行的，在1.6之后的版本，采用线程池处理并行任务，但是从Android 3.0开始，为了避免AsyncTask所带来的并发错误，又采用一个线程来串行执行任务。可以使用executeOnExecutor()方法来并行地执行任务。

* AsyncTask原理
AsyncTask中有两个线程池和一个Handler，其中线程池SerialExecutor用于任务的排队，而线程池THREAD_POOL_EXECUTOR用于真正地执行任务，InternalHandler用于将执行环境从线程池切换到主线程。
sHandler是一个静态的Handler对象，为了能够将执行环境切换到主线程，这就要求sHandler这个对象必须在主线程创建。由于静态成员会在加载类的时候进行初始化，因此这就变相要求AsyncTask的类必须在主线程中加载，否则同一个进程中的AsyncTask都将无法正常工作。

5. onSaveInstanceState() 与 onRestoreIntanceState()
* Activity的 onSaveInstanceState() 和 onRestoreInstanceState()并不是生命周期方法，并不一定会被触发。
* 当应用遇到意外情况（如：内存不足、用户直接按Home键）由系统销毁一个Activity时，onSaveInstanceState() 会被调用。但是当用户主动去销毁一个Activity时，例如在应用中按返回键，onSaveInstanceState()就不会被调用。因为在这种情况下，用户的行为决定了不需要保存Activity的状态。通常onSaveInstanceState()只适合用于保存一些临时性的状态，而onPause()适合用于数据的持久化保存。 
* 这个方法在一个activity被杀死前调用，当该activity在将来某个时刻回来时可以恢复其先前状态。 例如，如果activity B启用后位于activity A的前端，在某个时刻activity A因为系统回收资源的问题要被杀掉，A通过onSaveInstanceState将有机会保存其用户界面状态，使得将来用户返回到activity A时能通过onCreate(Bundle)或者onRestoreInstanceState(Bundle)恢复界面的状态
6. android中进程的优先级？
    1. 前台进程：
    即与用户正在交互的Activity或者Activity用到的Service等，如果系统内存不足时前台进程是最晚被杀死的

    2. 可见进程：
    可以是处于暂停状态(onPause)的Activity或者绑定在其上的Service，即被用户可见，但由于失了焦点而不能与用户交互

    3. 服务进程：
    其中运行着使用startService方法启动的Service，虽然不被用户可见，但是却是用户关心的，例如用户正在非音乐界面听的音乐或者正在非下载页面下载的文件等；当系统要空间运行，前两者进程才会被终止

    4. 后台进程：
    其中运行着执行onStop方法而停止的程序，但是却不是用户当前关心的，例如后台挂着的QQ，这时的进程系统一旦没了有内存就首先被杀死

    5. 空进程：
    不包含任何应用程序的进程，这样的进程系统是一般不会让他存在的
7. Bunder传递对象为什么需要序列化？Serialzable和Parcelable的区别？
* 因为bundle传递数据时只支持基本数据类型，所以在传递对象时需要序列化转换成可存储或可传输的本质状态（字节流）
* Serializable（Java自带）：序列化，表示将一个对象转换成存储或可传输的状态。序列化后的对象可以在网络上进传输，也可以存储到本地。在硬盘上。
* Parcelable（android专用）：使用Parcelable也可以实现相同的效果，不过不同于将对象进行序列化，Parcelable方式的实现原理是将一个完整的对象进行分解，而分解后的每一部分都是Intent所支持的数据类型，这也就实现传递对象的功能了。在内存中
8. Context相关
* Activity和Service以及Application的Context是不一样的,Activity继承自ContextThemeWraper.其他的继承自ContextWrapper。
* 每一个Activity和Service以及Application的Context是一个新的ContextImpl对象。
* getApplication()用来获取Application实例的，但是这个方法只有在Activity和Service中才能调用的到。比如BroadcastReceiver中也想获得Application的实例，这时就可以借助getApplicationContext()方法，getApplicationContext()比getApplication()方法的作用域会更广一些，任何一个Context的实例，只要调用getApplicationContext()方法都可以拿到我们的Application对象。
* 创建对话框时不可以用Application的context，只能用Activity的context。
* Context的数量等于Activity的个数 + Service的个数 +1，这个1为Application。
9. 更新UI方式：Activity.runOnUiThread(Runnable)、Handler、AsyncTask
10. ContentProvider使用方法。
* 跨进程通信，实现进程间的数据交互和共享。
* 通过Context 中 getContentResolver() 获得实例，通过 Uri匹配进行数据的增删改查。ContentProvider使用表的形式来组织数据，无论数据的来源是什么，ConentProvider 都会认为是一种表，然后把数据组织成表格
11. Merge、ViewStub 的作用。
Merge: 减少视图层级，可以删除多余的层级。

ViewStub: 按需加载，减少内存使用量、加快渲染速度、不支持 merge 标签。
12. Asset目录与res目录的区别
* assets：不会在 R 文件中生成相应标记，存放到这里的资源在打包时会打包到程序安装包中。（通过 AssetManager 类访问这些文件）
* res：会在 R 文件中生成 id 标记，资源在打包时如果使用到则打包到安装包中，未用到不会打入安装包中。
13. Handler机制
* message：消息。
* MessageQueue：消息队列，负责消息的存储与管理，负责管理由 Handler 发送过来的 Message。读取会自动删除消息，单链表维护，在其next()方法中会无限循环，不断判断是否有消息，有就返回这条消息并移除。
* Looper：消息循环器，负责关联线程以及消息的分发，在该线程下从 MessageQueue获取 Message，分发给Handler
Looper创建的时候会创建一个 MessageQueue，调用loop()方法的时候消息循环开始，其中会不断调用messageQueue的next()方法，当有消息就处理，否则阻塞在messageQueue的next()方法中。当Looper的quit()被调用的时候会调用messageQueue的quit()，此时next()会返回null，然后loop()方法也就跟着退出。
* Handler：消息处理器，负责发送并处理消息
* 1、Handler通过sendMessage()发送消息Message到消息队列MessageQueue。
* 2、Looper通过loop()不断提取触发条件的Message，并将Message交给对应的target handler来处理。
* 3、target handler调用自身的handleMessage()方法来处理Message。
* Handler 发送的消息由 MessageQueue 存储管理，并由 Looper 负责回调消息到 handleMessage()。
* Handler 引起的内存泄露原因以及最佳解决方案——当Handler消息队列 还有未处理的消息 / 正在处理消息时，存在引用关系： “未被处理 / 正处理的消息Message持有Handler实例的引用，handler又持有外部类的引用。——将 Handler 定义成静态的内部类，在内部持有 Activity 的弱引用，
14. 程序A能否接收到程序B的广播？
* 能，使用全局的BroadCastRecevier能进行跨进程通信，但是注意它只能被动接收广播。LocalBroadCastRecevier只限于本进程的广播间通信。
15. 数据加载更多涉及到分页，你是怎么实现的？
* 可以手动调用接口，重新刷新RecyclerView。
16. JSON的结构？对象和数组
17. Oom 是否可以try catch ？
* 在try语句中声明了很大的对象，导致OOM，并且可以确认OOM是由try语句中的对象声明导致的，那么在catch语句中，可以释放掉这些对象，解决OOM的问题，继续执行剩余语句。
18. 强引用置为null，会不会被回收？
*对象的引用被置为null，断开了当前线程栈帧中对该对象的引用关系，垃圾收集器是运行在后台的线程，只有当用户线程运行到安全点(safe point)或者安全区域才会扫描对象引用关系，扫描到对象没有被引用则会标记对象，这时候仍然不会立即释放该对象内存，因为有些对象是可恢复的（在 finalize方法中恢复引用 ）。只有确定了对象无法恢复引用的时候才会清除对象内存
19. Bundle传递数据为什么需要序列化？
* 可存储或可传输。1.永久性保存对象，保存对象的字节序列到本地文件中 2.对象在网络中传递；3.对象在IPC间传递（跨进程）
20. 广播传输的数据是否有限制，是多少，为什么要限制？
* 限制在1MB之内。传递大数据，不应该用Intent；考虑使用ContentProvider或者直接匿名共享内存
30. 直接在Activity中创建一个thread跟在service中创建一个thread之间的区别？
* 在Activity中被创建：该Thread的就是为这个Activity服务的，完成这个特定的Activity交代的任务，主动通知该Activity一些消息和事件，Activity销毁后，该Thread也没有存活的意义了。

在Service中被创建：只要整个Service不退出，Thread就可以一直在后台执行，一般在Service的onCreate()中创建，在onDestroy()中销毁。所以，在Service中创建的Thread，适合长期执行一些独立于APP的后台任务，比较常见的就是：在Service中保持与服务器端的长连接。
31. activty和Fragmengt之间怎么通信，Fragmengt和Fragmengt怎么通信？
* Handler、广播、事件总线：EventBus、、接口回调、Bundle和setArguments(bundle)
32. 服务启动一般有几种，服务和activty之间怎么通信，服务和服务之间怎么通信
* startService：
onCreate()--->onStartCommand() ---> onDestory()


如果服务已经开启，不会重复的执行onCreate()， 而是会调用onStartCommand()。一旦服务开启跟调用者(开启者)就没有任何关系了。 开启者退出了，开启者挂了，服务还在后台长期的运行。 开启者不能调用服务里面的方法。
* bindService：


onCreate() --->onBind()--->onunbind()--->onDestory()
bind的方式开启服务，绑定服务，调用者挂了，服务也会跟着挂掉。 绑定者可以调用服务里面的方法。
33. 说说Activity、Intent、Service 是什么关系
* Activity 和 Service 都是 Android 四大组件之一。Activity 负责用户界面的显示和交互，Service 负责后台任务的处理。Activity 和 Service 之间可以通过 Intent 传递数据，因此 可以把 Intent 看作是通信使者。
34. Handler、Thread和HandlerThread的差别
* Handler：在android中负责发送和处理消息，通过它可以实现其他支线线程与主线程之间的消息通讯。
* Thread：Java进程中执行运算的最小单位，亦即执行处理机调度的基本单位。某一进程中一路单独运行的程序。
* HandlerThread：继承自Thread的类HandlerThread，H本质就是个Thread。在内部直接实现了Looper的实现，这是Handler消息机制必不可少的。有了自己的looper，可以在自己的线程中分发和处理消息。
35. ThreadLocal的原理
* ThreadLocal相当于线程内的内存，一个局部变量。每次可以对线程自身的数据读取和操作，并不需要通过缓冲区与 主内存中的变量进行交互。并不会像synchronized那样修改主内存的数据，再将主内存的数据复制到线程内的工作内存。ThreadLocal可以让线程独占资源，存储于线程内部，避免线程堵塞造成CPU吞吐下降。
* 实现单个线程单例以及单个线程上下文信息存储，比如交易id等。实现线程安全，非线程安全的对象使用ThreadLocal之后就会变得线程安全，因为每个线程都会有一个对应的实例。 承载一些线程相关的数据，避免在方法中来回传递参数。
36. MVP，MVVM，MVC
* MVC
   * 
   视图层(View) 对应于xml布局文件和java代码动态view部分
   
   控制层(Controller) MVC中Android的控制层是由Activity来承担的，Activity本来主要是作为初始化页面，展示数据的操作，但是因为XML视图功能太弱，所以Activity既要负责视图的显示又要加入控制逻辑，承担的功能过多。
   
   模型层(Model) 针对业务模型，建立数据结构和相关的类，它主要负责网络请求，数据库处理，I/O的操作。
* MVP
   * 通过引入接口BaseView，让相应的视图组件如Activity，Fragment去实现BaseView，实现了视图层的独立，在MVP中View并不直接使用Model，它们之间的通信是通过Presenter(MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会从直接Model中读取数据而不是通过Controller。MVP彻底解决了MVC中View和Controller分不清楚的问题，在该模式中，视图通常由表示器初始化，它呈现用户界面（UI）并接受用户所发出命令，但不对用户的输入作任何逻辑处理，而仅仅是将用户输入转发给表示器。通常每一个视图对应一个表示器，但是也可能一个拥有较复杂业务逻辑的视图会对应多个表示器，每个表示器完成该视图的一部分业务处理工作，降低了单个表示器的复杂程度，但是随着业务逻辑的增加，一个页面可能会非常复杂，UI的改变是非常多，会有非常多的case，这样就会造成View的接口会很庞大。
* MVVM
   * UI的改变多的情况下，通过双向绑定的机制，实现数据和UI内容，只要想改其中一方，另一方都能够及时更新的一种设计理念，这样就省去了很多在View层中写很多case的情况，只需要改变数据就行。

37. SharedPrefrences
* 在键值对中存储私有原始数据。
* apply和commit有什么区别？
   * apply没有返回值而commit返回boolean表明修改是否提交成功。
   * apply方法不会提示任何失败的提示。 由于在一个进程中，sharedPreference是单实例，一般不会出现并发冲突，如果对提交的结果不关心的话，建议使用apply，当然需要确保提交成功且有后续操作的话，还是需要用commit的。
38. Base64、MD5是加密方法么？
* Base64是用文本表示二进制的编码方式，它使用4个字节的文本来表示3个字节的原始二进制数据。它将二进制数据转换成一个由64个可打印的字符组成的序列
* 使用MD5加密的方式进行存储，将任意数据产生出一个128位（16字节）的散列值
40. HttpClient和HttpConnection的区别？
* Http Client适用于web浏览器，替换成OkHttp
* HttpURLConnection对于大部分功能都进行了包装，增加了一些Https方面的改进
41. ActivityA跳转ActivityB然后B按back返回A，各自的生命周期顺序，A与B均不透明。
* 程序正常启动：onCreate()->onStart()->onResume();
* 正常退出：onPause()->onStop()->onDestory()
* 一个Activity启动另一个Activity: onPause()->onStop(), 再返回：onRestart()->onStart()->onResume()
* 程序按back 退出： onPause()->onStop()->onDestory(),再进入：onCreate()->onStart()->onResume();
* 程序按home 退出： onPause()->onStop(),再进入：onRestart()->onStart()->onResume();
* ActivityA跳转到ActivityB：
Activity A：onPause
Activity B：onCreate
Activity B：onStart
Activity B：onResume
Activity A：onStop
* ActivityB返回ActivityA：
Activity B：onPause
Activity A：onRestart
Activity A：onStart
Activity A：onResume
Activity B：onStop
Activity B：onDestroy
42. BroadcastReceiver，LocalBroadcastReceiver 区别？
* BroadcastReceiver用于应用之间的传递消息；而LocalBroadcastManager用于应用内部传递消息，比broadcastReceiver更加高效。
* BroadcastReceiver是以 Binder（进程间通讯） 通讯方式为底层实现的机制，LocalBroadcastManager 的核心实现实际还是 Handler
43. 单例实现线程的同步的要求：
* 确保自己只有一个实例、必须自己创建自己的实例、必须为其他对象提供唯一的实例
44. ContentProvider、ContentResolver、ContentObserver 之间的关系？
* ContentProvider：管理数据，提供数据的增删改查操作，数据源可以是数据库、文件、XML、网络等，ContentProvider为这些数据的访问提供了统一的接口，可以用来做进程间数据共享。
* ContentResolver：ContentResolver可以为不同URI操作不同的ContentProvider中的数据，外部进程可以通过ContentResolver与ContentProvider进行交互。
* ContentObserver：观察ContentProvider中的数据变化，并将变化通知给外界。
44. HandlerThread
* HandlerThread有自己的内部Looper对象，可以进行loopr循环。当有耗时任务进入队列时，则不需要开启新线程，在原有的线程中执行耗时任务即可
45. Android中跨进程通讯的几种方式
* Content Provider 
* 广播
* ALDL服务
46. 显示Intent与隐式Intent的区别
* 明确指出了目标组件名称的Intent，我们称之为“显式Intent” 没有明确指出目标组件名称的Intent，则称之为“隐式 Intent”。