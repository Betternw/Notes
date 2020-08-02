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
      * AsyncTask是一种轻量级的异步任务抽象类，它可以在线程池中执行后台任务，然后把执行的进度和最终结果传递给主线程并在主线程中更新UI。
      * AsyncTask是一个抽象的泛型类，它提供了Params、Progress和Result这三个泛型参数，其中Params表示参数的类型，Progress表示后台任务的执行进度和类型，而Result则表示后台任务的返回结果的类型，如果AsyncTask不需要传递具体的参数，那么这三个泛型参数可以用Void来代替。

   * 关于线程池：
      * AsyncTask对应的线程池ThreadPoolExecutor都是进程范围内共享的，且都是static的，所以是Asynctask控制着进程范围内所有的子类实例。由于这个限制的存在，当使用默认线程池时，如果线程数超过线程池的最大容量，线程池就会爆掉(3.0后默认串行执行，不会出现个问题)。针对这种情况，可以尝试自定义线程池，配合Asynctask使用。

   * 关于默认线程池：
      * AsyncTask里面线程池是一个核心线程数为CPU + 1，最大线程数为CPU * 2 + 1，工作队列长度为128的线程池，线程等待队列的最大等待数为28，但是可以自定义线程池。线程池是由AsyncTask来处理的，线程池允许tasks并行运行，需要注意的是并发情况下数据的一致性问题，新数据可能会被老数据覆盖掉。所以希望tasks能够串行运行的话，使用SERIAL_EXECUTOR。

   * 一些问题：

       1.生命周期
       在activity的destroy中调用cancel方法才行，否则会提前崩溃。AsynTask会一直执行，直到doInBackground()方法执行完毕，然后，如果cancel(boolean)被调用,那么onCancelled(Result result)方法会被执行；否则，执行onPostExecute(Result result)方法。如果我们的Activity销毁之前，没有取消AsyncTask，这有可能让我们的应用崩溃(crash)。因为它想要处理的view已经不存在了。所以，我们是必须确保在销毁活动之前取消任务。总之，我们使用AsyncTask需要确保AsyncTask正确的取消。

       2.内存泄漏

       如果AsyncTask被声明为Activity的非静态内部类，那么AsyncTask会保留一个对Activity的引用。如果Activity已经被销毁，AsyncTask的后台线程还在执行，它将继续在内存里保留这个引用，导致Activity无法被回收，引起内存泄漏。

       3.结果丢失

       屏幕旋转或Activity在后台被系统杀掉等情况会导致Activity的重新创建，之前运行的AsyncTask会持有一个之前Activity的引用，这个引用已经无效，这时调用onPostExecute()再去更新界面将不再生效。

       4.并行还是串行

       在Android1.6之前的版本，AsyncTask是串行的，在1.6之后的版本，采用线程池处理并行任务，但是从Android 3.0开始，为了避免AsyncTask所带来的并发错误，又采用一个线程来串行执行任务。可以使用executeOnExecutor()方法来并行地执行任务。
          * 串行：一个线程顺序执行n个任务
          * 并行：N个线程分别执行N个任务，有多个cpu
          * 并发：一个cpu，分发时间片，不同时间段执行不同任务，其他线程就处于阻塞状态

     * AsyncTask原理
        * AsyncTask中有两个线程池和一个Handler，其中线程池SerialExecutor用于任务的排队，而线程池THREAD_POOL_EXECUTOR用于真正地执行任务，InternalHandler用于将执行环境从线程池切换到主线程sHandler是一个静态的Handler对象，为了能够将执行环境切换到主线程，这就要求sHandler这个对象必须在主线程创建。由于静态成员会在加载类的时候进行初始化，因此这就变相要求AsyncTask的类必须在主线程中加载，否则同一个进程中的AsyncTask都将无法正常工作。
    * 同步和异步
      * 同步：方法调用一旦开始，必须等到执行完成后才能继续后续的操作
      * 异步：方法调用一旦开始，方法调用就会返回，实际的执行会在另外一个线程中执行，整个过程不会阻碍调用者的工作。
    * 使用方法
      * 三个参数
        * Params：启动任务执行的输入参数，比如HTTP请求的URL。
        * Progress：后台任务执行的百分比。
        * Result：后台任务执行最终返回的结果，比如String，Integer等。
      * 五个方法
        * execute（Params... params）：执行一个异步任务，需要我们手动去调用此方法，触发异步任务的执行。
        * onPreExecute()：在execute(params... params)被调用后立即执行，一般用来在执行后台任务前对UI做一些标记。
        * doInBackground(Params... params)：在onPreExecute()完成后立即执行，用于执行耗时操作，在执行过程中可以调用publishProgress(Progress... values)来更新进度信息。
        * onProgressUpdate(Progress... values)：在调用publishProgress(Progress... values)时，此方法被执行，直接将进度信息更新到UI组件上。
        * onPostExecute(Result result)：当后台操作结束时，此方法将会被调用，计算结果将作为参数传递到此方法中，直接将结果显示到UI组件上。


5. onSaveInstanceState() 与 onRestoreIntanceState()
* 除了在栈顶的activity，其他的activity都有可能在内存不足的时候被系统回收，一个activity越处于栈底，被回收的可能性就越大。调用 onPause()和 onStop()方法后的 activity 实例仍然存在于内存中，activity 的所有信息和状态数据不会消失，当 activity 重新回到前台之后，所有的改变都会得到保留。当系统内存不足时， 调用onPause()和onStop()方法后的 activity可能会被系统摧毁,，此时内存中就不会存有该 activity 的实例对象了。如果之后这个 activity 重新回到前台，之前所作的改变就会消失。
* Activity的 onSaveInstanceState() 和 onRestoreInstanceState()并不是生命周期方法，并不一定会被触发。
* 当应用遇到意外情况（如：内存不足、用户直接按Home键）由系统销毁一个Activity时，onSaveInstanceState() 会被调用。但是当用户主动去销毁一个Activity时，例如在应用中按返回键，onSaveInstanceState()就不会被调用。因为在这种情况下，用户的行为决定了不需要保存Activity的状态。通常onSaveInstanceState()只适合用于保存一些临时性的状态，而onPause()适合用于数据的持久化保存。 
* onSaveInstanceState()方法接受一个 Bundle 类型的参数，开发者可以将状态数据存储到这个 Bundle 对象中，这样即使 activity 被系统摧毁，当用户重新启动这个 activity 而调用它的onCreate()方法时，上述的 Bundle 对象会作为实参传递给 onCreate()方法，开发者可以从 Bundle 对象中取出保存的数据，然后利用这些数据将 activity 恢复到被摧毁之前的状态。
* 这个方法在一个activity被杀死前调用，当该activity在将来某个时刻回来时可以恢复其先前状态。 例如，如果activity B启用后位于activity A的前端，在某个时刻activity A因为系统回收资源的问题要被杀掉，A通过onSaveInstanceState将有机会保存其用户界面状态，使得将来用户返回到activity A时能通过onCreate(Bundle)或者onRestoreInstanceState(Bundle)恢复界面的状态
1. android中进程的优先级？
    1. 前台进程：
    即与用户正在交互的Activity或者Activity用到的Service等，如果系统内存不足时前台进程是最晚被杀死的

    1. 可见进程：
    可以是处于暂停状态(onPause)的Activity或者绑定在其上的Service，即被用户可见，但由于失了焦点而不能与用户交互

    1. 服务进程：
    其中运行着使用startService方法启动的Service，虽然不被用户可见，但是却是用户关心的，例如用户正在非音乐界面听的音乐或者正在非下载页面下载的文件等；当系统要空间运行，前两者进程才会被终止

    1. 后台进程：
    其中运行着执行onStop方法而停止的程序，但是却不是用户当前关心的，例如后台挂着的QQ，这时的进程系统一旦没了有内存就首先被杀死

    1. 空进程：
    不包含任何应用程序的进程，这样的进程系统是一般不会让他存在的
2. Bunder传递对象为什么需要序列化？Serialzable和Parcelable的区别？
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
* HandlerThread：因为多次创建和销毁线程是很耗费资源的。继承自Thread的类HandlerThread，本质就是个Thread。在内部直接实现了Looper的实现，这是Handler消息机制必不可少的。有了自己的looper，可以在自己的线程中分发和处理消息，执行异步任务。它不需要我们去拿主线程的Looper，也不用手动调用Looper.prepare和Looper.loop，它已经封装了Looper，
1.  ThreadLocal的原理
* ThreadLocal相当于线程内的内存，一个局部变量。每次可以对线程自身的数据读取和操作，并不需要通过缓冲区与 主内存中的变量进行交互。并不会像synchronized那样修改主内存的数据，再将主内存的数据复制到线程内的工作内存。ThreadLocal可以让线程独占资源，存储于线程内部，避免线程堵塞造成CPU吞吐下降。
* 实现单个线程单例以及单个线程上下文信息存储，比如交易id等。实现线程安全，非线程安全的对象使用ThreadLocal之后就会变得线程安全，因为每个线程都会有一个对应的实例。 承载一些线程相关的数据，避免在方法中来回传递参数。
36. MVP，MVVM，MVC
* MVC
   * 视图层(View) 对应于xml布局文件和java代码动态view部分
   * 控制层(Controller) MVC中Android的控制层是由Activity来承担的，Activity本来主要是作为初始化页面，展示数据的操作，但是因为XML视图功能太弱，所以Activity既要负责视图的显示又要加入控制逻辑，承担的功能过多。
   * 模型层(Model) 针对业务模型，建立数据结构和相关的类，它主要负责网络请求，数据库处理，I/O的操作。
   * model：负责在数据库中存取数据，业务逻辑处理
   * view：处理数据的显示，视图是根据模型数据创建的，比如xml文件
   * controller：处理用户交互，从视图读取数据，并向模型发送数据。Activity处理用户交互问题
   * 优点：耦合性低，更改视图层代码不用重新编译模型和控制器代码。便于UI界面逻辑的显示和业务逻辑的分离。
   * 缺点：不能互相调用的时候就功能局限，独立重用功能很低。对数据可能会存在多次的频繁访问。
   * 适合于大型项目、业务逻辑复杂、需求变更频繁
* MVP
   * 在MVP中View并不直接使用Model，它们之间的通信是通过Presenter(MVC中的Controller)来进行的，所有的交互都发生在Presenter内部，而在MVC中View会从直接Model中读取数据而不是通过Controller。
   * MVP彻底解决了MVC中View和Controller分不清楚的问题（View功能有限，会放到controcller中）。在该模式中，视图通常由表示器初始化，但不对用户的输入作任何逻辑处理，而仅仅是将用户输入转发给表示器。通常每一个视图对应一个表示器，但是也可能一个拥有较复杂业务逻辑的视图会对应多个表示器，每个表示器完成该视图的一部分业务处理工作，降低了单个表示器的复杂程度，
   * 但是随着业务逻辑的增加，一个页面可能会非常复杂，UI的改变是非常多，会有非常多的case，这样就会造成View的接口会很庞大。
   * View的xml文件功能有限，Activity中不仅处理业务逻辑还要处理操作UI。因此controcller会很厚重
   * M：业务逻辑和实体逻辑
   * V：Activity，负责view的绘制和用户的交互
   * P：负责完成View与Model之间的交互
   * View和Model不直接交互。通过presenter层进行交互
   * 
* MVVM
   * view  viewmodel model，后两者双向交互
   * View：activity+xml。负责View的绘制与用户交互。Activity层不写业务逻辑，只写初始化控件等。更新UI通过数据绑定，在viewModel中来做。
   * model：数据模型，数据获取存储和变化，提供数据接口，供viewModel使用。
   * viewmodel： 数据交互，业务逻辑，负责交互view和model。
   * UI的改变多的情况下，通过双向绑定的机制，实现数据和UI内容，只要想改其中一方，另一方都能够及时更新的一种设计理念，这样就省去了很多在View层中写很多case的情况，只需要改变数据就行。

1.  SharedPrefrences
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
* 本质上是一个在子线程的handler (HandlerThread=Handler+Thread);
* 是通过获取thread的looper，来初始化Handler
* HandlerThread 的具体使用场景是 IntentService。IntentService 封装了 HandlerThread 和 Handler 。HandlerThread 主要用来处理需要顺序执行的异步操作。
```java
HandlerThread mHandlerThread = new HandlerThread("mHandlerThread");
mHandlerThread .start();
 Handler mHandler= new Handler( mHandlerThread.getLooper() ) {
           @Override
           public boolean handleMessage(Message msg) {
               //消息处理
               return true;
           }
     });
 Message  message = Message.obtain();
     message.what = “2”
     mHandler.sendMessage(message);
    mHandlerThread.quit()；
```
1.  通信
    * Android中跨进程通讯的几种方式
      * Content Provider 
      * 广播：使用intent携带数据
        * BroadcastReceiver 是跨应用广播，利用Binder机制实现，
        * LocalBroadcastReceiver 是应用内广播，利用Handler实现，
      * ALDL服务
    * Activity跳转：使用intent和bundle
       * 显示intent：Intent intent = new Intent(MainActivity.this,SecondActivity.class);startActivity(intent);
       * 隐式intent
         * 在manifests.xml配置文件中给SecondActivity设置<intent-filter>,绑定action，命名为“android.intent.action.MY_ACTION”
         *  intent.setAction("com.android.activity.MY_ACTION");  startActivity(intent);
    * Activity之间通信：intent、bundle、eventbus、广播、接口回调、
    * Activity与Fragment进行通信：Bundle（在activity中建一个bundle，把要传的值存入bundle，然后通过fragment的setArguments（bundle）传到fragment，在fragment中，用getArguments接收。）、广播、Handler
    * Fragment通信
       * 在fragment中调用activity的方法：getActivity
       * 在Activity中调用Fragment的方法：接口回调
       * 在Fragment中调用Fragment的方法：findFragmentByID
    * 跨进程调用自定义Service有两种方式：Messager和AIDL。要让两个不同的进程之间进行函数调用，就要使用进程间通信IPC，这两种方式都使用了IPC技术。在安卓系统当中，它实际上是由Binder来实现的。
       * ALDL：定义一个文件，将service要提供给其他进程使用的接口函数定义在里面。创建一个service类，实现刚才类定义的binder。另一个应用创建serviceconnection，绑定servicec后得到返回的binder。
2.  显示Intent与隐式Intent的区别
* 明确指出了目标组件名称的Intent，我们称之为“显式Intent” 没有明确指出目标组件名称的Intent，则称之为“隐式 Intent”。
47. Kotlin 特性，和 Java 相比有什么不同的地方?
   * 能直接与Java相互调用
   * 支持高阶函数
   * 语言层面解决空指针问题
48. activity的四种状态：running paused stopped  killed
50. viewpager实现页面滑动
51. service：运行在主线程中，不能做耗时操作。
52. service和thread区别；————service运行在主线程中，不能运行耗时操作，是执行在后台不依赖于activity的，与是否耗时没有关系。，与activity通信通过async。————thread是子线程，可以运行耗时操作。是分配cpu的基本单位。
53. 广播：应用程序之间传输信息，发送的内容是intent，intent携带数据；同一个app之间不同组件的消息通信、不同app之间的组件的相互通信；普通广播、系统广播、本地广播（只在app内传播）；静态注册：注册完成后一直运行，动态注册：跟随activity的生命周期。内部实现机制：binder机制
54. 启动service的方式：
  * startservice：activity销毁了不会影响service，传递的参数是intent。第一次调用startService方法时，onCreate方法、onStartCommand方法将依次被调用，而多次调用startService时，只有onStartCommand方法会被多次调用。
  * bindservice（通过ibinder接口与activity进行绑定），传递的参数是binder
55.  Service和IntentService的区别：
   * service不可以执行耗时操作，因此创建intentService，继承自service，在onCreate的时候，建立一个内部的线程去执行耗时操作。
56. onStartCommand四种返回值策略
  * start_sticky:service进程被kill掉，保留开始状态不保留intent数据。系统调用onStartCommand重新开始service，如果没有命令传入那么intent是null。默认情况
  * start_not_sticky:如果被kill掉，系统不会自动重启。
  * start_redeliver_intent:重启intent。如果服务被kill，会自动重启并传入intent
  * start_sticky_compatibility:第一种的兼容版本， 不保证服务被kill后一定能重启
57. 为啥不在Activity中开启子线程，而是在Service中开启子线程？
  * 因为在Activity中开启子线程，当Activity被销毁了，子线程是无法控制的，但是如果在Service中开启子线程，就无需担心这些，完全不用担心对子线程的控制，因为子线程都在Service中。
58. handler——发送处理消息
   * 子线程的操作通过handler传递给主线程；
   * Handler在子线程中怎么发送消息，
     * 拿到主线程的Looper或者通过手动调用Looper.prepare和Looper.loop，
     * HandlerThread：它不需要我们去拿主线程的Looper，也不用手动调用Looper.prepare和Looper.loop，它已经封装了Looper，
   * 使用方法：post（runnable）、sendmessage（message）；
      * message：消息。
      * MessageQueue：消息队列，负责消息的存储与管理，负责管理由 Handler 发送过来的 Message。读取会自动删除消息，单链表维护，在其next()方法中会无限循环，不断判断是否有消息，有就返回这条消息并移除。
      * Looper：消息循环器，负责关联线程以及消息的分发，在该线程下从 MessageQueue获取 Message，分发给Handler
      * Looper创建的时候会创建一个 MessageQueue，调用loop()方法的时候消息循环开始，其中会不断调用messageQueue的next()方法，当有消息就处理，否则阻塞在messageQueue的next()方法中。当Looper的quit()被调用的时候会调用messageQueue的quit()，此时next()会返回null，然后loop()方法也就跟着退出。
      * 过程
        * 主线程创建handler，复写handlemessage方法
        * 子线程通过message属性调用handlermeaasge方法，将子线程中消息传到主线程
        * 通过handlermessage的复写方法进行相应的ui线程的处理
      * Handler：消息处理器，负责发送并处理消息
      * 1、Handler通过sendMessage()发送消息Message到消息队列MessageQueue。
      * 2、Looper通过loop()不断提取触发条件的Message，并将Message交给对应的target handler来处理。
      * 3、target handler调用自身的handleMessage()方法来处理Message。
      * Handler 发送的消息由 MessageQueue 存储管理，并由 Looper 负责回调消息到 handleMessage()。
   * 内存泄露：
     * 非静态内部类引用了外部类的引用。handler没有被释放，持有的引用没有必要释放，会内存释放——设置为静态内部类，在类内持有外部类的弱引用（弱引用也是用来描述非必需对象的，当JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象）
     * Handler 引起的内存泄露原因以及最佳解决方案——当Handler消息队列 还有未处理的消息 / 正在处理消息时，存在引用关系： “未被处理 / 正处理的消息Message持有Handler实例的引用，handler又持有外部类的引用。——将 Handler 定义成静态的内部类，在内部持有 Activity 的弱引用，
   * Looper循环为什么不会导致应用卡死
     * 因为死循环的作用是保证不断循环，主线程可以一直处于运行状态。通过loop循环遍历MessageQueue,然后通过H handler类的handleMessage方法去处理activity相关的start、pause、stop、destroy等各类事件。真正卡死主线程操作的是在回调方法onCreate、onStart、onResume等操作时间过长，会导致掉帧甚至ANR，Looper.loop()本身不会导致应用卡死。
   * 主线程的死循环一直运行是不是特别消耗 CPU 资源呢？
     * 在messagequeue中没有消息时，主线程会释放CPU资源进入休眠状态，当有下个消息到达时候，通过往pipe管道写端口写入数据来唤醒休眠的线程。这里采用IO多路复用，同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。
59. 自定义View的绘制
   * View树的绘制流程：measure（是否需要计算视图大小）——layout（是否需要视图位置）——draw（是否需要重绘）
   * measure：树的递归。从上到下有序遍历。根据父容器对子容器的测量规格的参数，获取子容器的长宽高，并返回父容器，然后进行统一的测量。
   * 
59. listview——动态滚动展示view
   * 适配器模式：数据驱动UI，根据数据绘制UI
   * recylerBin：内部类。屏幕可见放在内存中，其余放在recylerBin
   * 优化：convertview重用（通过缓存convertView,这种利用缓存contentView的方式可以判断如果缓存中不存在View才创建View，如果已经存在可以利用缓存中的View）/viewholder（循环利用itemview。第一次加载item后，放入到内存中，下一次再加载的时候直接填充数据，不用再重新加载view。也就是不断地复用，只是更改了数据。）
60. Activity的启动模式
   * standard：每次打开一个Activity就创建一个新的实例
   * singleTop：栈顶有就复用，没有就重新创建一个实例
   * singleTask：栈中有就销毁上面的所有Activity，成为新的栈顶。
   * singleInstance：每创建一个Activity就新建一个栈。
61. 对于 Context，你了解多少?——application service activity 都是具体的context。初始化一些系统组件 如 dialog toast 要把这个context传入
62. IntentFilter直译：意图过滤器，可以给四大组件配置自己关心的action等，以免想打开A结果打开了B，比如Receiver需要指定intent-filter来表明自己关心什么广播等
63. 简单介绍下ContentProvider是如何实现数据共享的？
   * 可以通过ContenrResolver来操作ContentProvider暴露的数据
   * ContentProvider：对外提供了访问数据的接口
   * ContenrResolver：通过不同的URI操作不同的ContentProvider中的数据
64. OkHttp
     * 关于网络请求的第三方类库，其中封装了网络请求的get、post等操作的底层实现，
     * 创建OkHttpclient对象：http请求的客户端类，client对象作为全局的实例进行保存。只需要这一个实例对象。所有的http请求共用client实例对象的response缓存和线程池。
     * 创建request对象：存储了请求报文的信息（请求的URL地址、请求的方法和请求体，标志位），使用builder模式进行创建，将创建和表示分离。
     * new call 方法创建call 对象：代表一个实际的http请求，连接request和response的桥梁。这个对象可以同步、异步获取数据
       * execute（）： Response response = client.newCall(request).execute();。同步需要开启子线程，会阻塞进程获取数据，接收一个request对象，执行execute方法后返回response对象，也就是结果。
         * 首先使用new创建OkHttpClient对象。然后调用OkHttpClient对象的newCall方法生成一个Call对象，该方法接收一个Request对象，Request对象存储的就是我们的请求URL和请求参数。最后执行Call对象的execute方法，得到Response对象，这个Response对象就是返回的结果。
       * enqueue：client.newCall(request).enqueue(new Callback()。接收callback参数，不会阻塞，开启一个子线程，在子线程中操作。当异步请求成功时，回调onResponse方法，获取okHttp的response对象。当请求失败时，回调onFailture方法。这两个方法都在工作线程当中执行。
         * 首先使用new创建OkHttpClient对象。然后调用OkHttpClient对象的newCall方法生成一个Call对象，该方法接收一个Request对象，Request对象存储的是我们的请求URL和请求参数。最后执行Call对象的enqueue方法，该方法接收一个Callback回调对象，在回调对象的onResponse方法中拿到Response对象，这就是返回的结果。
     * post方法：在生成request方法的时候执行了post方法，get方法没有。因为request的builder时候，默认是get方法，所以需要post时就需要申明post方法。
65. retrofit
    * 创建一个接口，作为http请求的api接口。
      * 使用注解的方式描述和配置网络请求参数。实际上是运用动态代理的方式将注解翻译成一个Http请求，然后执行该请求。
      * 请求方法有GET、POST、PUT、HEAD、DELETE等
      * 方法的返回类型必须为Call，xxx是接收数据的类
      * User中字段的名字必须和后台Json定义的字段名是一致的，这样Json才能解析成功。
      * public interface GitHubService {
         @GET("getUserData")
           Call<User> getCall();
             }
    * 创建一个Retrofit对象
      * builder模式
      * baseUrl拼接Url+网络请求接口的注解设置——网络请求的地址
      * addConverterFactory(GsonConverterFactory.create())的作用是设置数据解析器，它可以解析Gson、JsonObject、JsonArray这三种格式的数据。
      * Retrofit retrofit = new Retrofit.Builder().baseUrl("http://10.75.114.138:8081/")
                .addConverterFactory(GsonConverterFactory.create()).build(); //设置网络请求的Url地址
                .addConverterFactory(GsonConverterFactory.create()) //设置数据解析器
                .build();
    * 创建网络请求接口实例
      * retrofit的create方法创建request对象
      * retrofit的getcall创建call对象
    * 发送网络请求——同步异步的请求和okHttp请求的一样。
    * retrofit在OkHttp的上面封装了一层，使请求接口和数据解析更加简洁明了。
66. ButterKnife
    * 通过解析注解来实现辅助代码
    * 绑定一个View  注解@BindView
    * 给view添加点击事件
67. Glide
    * 传入context对象，调用load方法，使用into方法显示imageView
68. Activity中onNewIntent方法的调用时机和使用场景？
   * 当启动模式为single Top的时候，如果栈顶有当前activity，那么复用这个activity，启动模式就是onnewintent（）方法。
69. binder和bundle
   * binder
     * 跨进程通信，客户端和服务端之间通信
     * Android使用Linux内核，通信方式有：socket、binder、管道
     * 性能更好，binder比socket更高效
     * 安全性更高，socket是ip地址手动填写，会有安全性问题。binder要进行通信双方信息校验，所以更安全
     * 通信模型步骤
       * 建立serviceManager，相当于通信录，
       * server进程进行注册，
       * 通信时，查询serviceManeger，将信息告诉client，client通过binder与server进行通信
     * 跨进程通信
       * server在manage中进行注册方法
       * client查询是否有目标方法，
       * manage返回方法的代理对象，这个方法内部是空的，client可以进行调用
       * 当client调用代理对象的时候，返回binder驱动，binder驱动就会去调用server的方法，将结果返回给manage再返回给client。
     * 对于server端来说，binder是本地对象。对于client来说，binder是代理对象
   * binder例子
     * ALDL
       * asInterface：如果AB在同一个进程就不会跨进程，如果不是就会返回proxy代理类的对象，通过这个对象来获取数据
       * onTransct：根据ALDL返回的方法编号进行调用相应的方法
   * bundle
     * 两个activity之间通信，是一种数据载体
     * Intent直接传值和通过Bundle包装后传值的比较
       * 若要从AActivity跳转到BActivity时需要写2个Intent，如果涉及的传值的话，Intent还要写两遍添加值的方法。如果用1个Bundle直接把值先存里边 然后再存到Intent中代码会显得更加简洁。
     * 应用场景
       * Activity：onCreat()、onSaveInstanceState()
       * Fragment： setArguments()
       * Message：setData()
70. Fragment
   * 为什么被称为第五大组件？
     * fragment比activity更节省内存，并且UI的切换更加舒适，使用replace等方法进行切换fragment，不会有很明显的效果，但是activity的切换会有明显的翻页效果。
   * 传值
     * Activity向Fragment传值——bundle
       * 将要传的值放入bundle对象中
       * 在activity中创建fragment对象，使用fragment.setArgument（）方法，将bundle传递到fragment中。
       * 在Fragment中调用getArgument（）方法。获得bundle对象，就能得到值。
     * Fragment向Activity传值：
       * 在Activity中调用getFragmentManager()得到fragmentManager,，调用findFragmentByTag(tag)或者通过findFragmentById(id)
         * FragmentManager fragmentmanager = getFragmentmanager();
         * Fragment fragment = fragmentmanager.findFragmentByTag(tag)
       * 回调接口
         * 在Fragment中定义一个接口，接口中有一个空的方法，在Fragmnt中调用该方法并把值作为参数放到方法中，在ACtivity中实现这个接口重写这个方法，这样值就传入到了Activity中。
     * Fragment之间传值
       * 接口回调
       * setArgument+bundle
       * 通过findFragmentByTag得到另一个的Fragment的对象，这样就可以调用另一个的方法了。
   * 加载
     * Fragment加载到Activity中
       * 添加Fragment到Activity的布局文件中
       * 动态地在activity中添加fragment
           * 添加一个FragmentTransction的实例
             * FragmentManager fragmentmanager = getFragmentmanager();
             * FragmentTransction transction = fragmentmanager.beginTransction（）
           * 用add方法加上Fragment的对象rightFragment
             * RightFragment rightFragment = new RightFragment（）
             * fragmentTransction.add(fragment的id)
           * 调用commit方法使得FragmentTransction实例的改变生效
             * transction.commit（）
   * Fragment跳转：使用activity的fragmenttransation的replace方法替换
   * Activity跳转：使用intent和bundle
       * 显示intent：Intent intent = new Intent(MainActivity.this,SecondActivity.class);startActivity(intent);
       * 隐式intent
         * 在manifests.xml配置文件中给SecondActivity设置<intent-filter>,绑定action，命名为“android.intent.action.MY_ACTION”
         *  intent.setAction("com.android.activity.MY_ACTION");  startActivity(intent);
   * 通信
     * 在fragment中调用activity的方法：getActivity
     * 在Activity中调用Fragment的方法：接口回调
     * 在Fragment中调用Fragment的方法：findFragmentByID
   * FragmentPagerAdapter与FragmentStatePagerAdapter的区别：
     * FragmentPagerAdapter：页面较少。因为在destroyitem的时候不会回收内存，只是将Fragment和UI进行分离
     * FragmentStatePagerAdapter：页面较多，在destroyitem的时候会回收内存
71. 广播
  * 使用intent包装数据
  * 本地广播使用handler实现
  * 程序之间广播使用binder机制
  * 实现广播-receiver
    * 静态注册：注册完成后就一直运行。写在xml文件中，用<receive>进行注册
    * 动态注册：跟随activity的生命周期。
  * 内部实现机制
    * 自定义广播接收者BroadcastReceiver，并复写onReceive（）方法
    * 通过binder机制向AMS注册（Activity Manager Service）
    * 广播发送者通过binder机制向AMS发送广播
    * AMS查找符合相应（IntentFilter）条件的BroadcastReceiver，将广播发送到相应的BroadcastReceiver（一般是Activity）的相应的消息循环队列中
    * 消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive方法
72. IntentService
  * 继承自service的一个抽象类
  * 内部通过HandlerThread和Handler来实现异步操作。
  * 在IntentService内有一个工作线程来处理耗时操作，主要是里面onHandleIntent()方法
  * 启动IntentService的方式和启动传统的Service一样，同时，当任务执行完后，IntentService会自动停止，而不需要我们手动去控制或调用stopSelf()。
  * 可以启动IntentService多次，而每一个耗时操作会以工作队列的方式在IntentService的onHandleIntent回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个。
  * 可以处理耗时操作的原因
    * 创建了HandlerThread，实例化一个Handler对象，传入的Looper是HandlerThread的Looper，所以可以执行耗时操作。


  



