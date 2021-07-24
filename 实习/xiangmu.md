### 一 缓存
#### 一 下载功能UI重构
1. 背景：之前的下载只能下载单集，只有两个清晰度选择，点击下载前台没有任何反馈。最终是要做一个交互面板，能够让用户感知到清晰度的选择、展示下载视频的信息（缩略图，标记，下载状态——开始下载、正在下载、下载完成）、缓存toast、显示当前下载视频大小/总内存空间。
2. 逻辑和UI逻辑：adapter：中间下载逻辑。 bottombar：底部信息，视频大小/可用内存。选择清晰度面板。三个部分放在三个类里。这三个通过一个implement联系在一起（外观者模式）。
3. 容器：list的view、dialog是详情页的底部弹窗，全屏的底部弹窗是三个框，
4. 逻辑的复用实现：三个场景的展示的信息和功能是相同的，然后分别塞到三个容器里。三个容器中间的连接也是implement。因为使用了外观者模式，容器只需要装在implement不需要装在三个具体的类。
5. 外观模式：为多个复杂的子系统提供一个一致的接口，只需要和这个接口进行交互。

#### 二 视频下载UI更新
1. 有网下提供倍速点赞评论，无网提供倍速功能  
2. 踩坑：启播时候进行网络请求点赞评论数据，请求返回以后再进行异步刷新UI。如果同步的话请求到了再起播就阻塞，子线程网络异步请求article信息，通过globalhandler推送到主线程中进行刷新UI。同步的时候bug：网络好ok，网络不好ANR。
3. 点击开始下载，放置视频宽高URL等信息，
4. 下载的实现：DownloadManager单例非线程安全的下载器，TaskInfo数据结构封装了下载任务相关信息，然后封装成DownloadRask，丢到DownloadManeger里。下载状态告知前台：将监听器（进行下载状态的监听），通过offlineService（接口功能），塞到downloadManager里。

#### 三 踩坑总结
1. 在Activity之间传数据，一开始使用了intent，但是article太大了，
   * 解决：采用单例的数据存储类来存储数据，map存储<id, article>，在activity之间进行传递。弹Activity之前即点击下载的时候塞进去。弹的时候再get到。
     * 安卓是有一个线程，因此使用恶汉或者懒汉模式就行。这个操作只会在主线程进行，不会出现子线程取数据。
     * 这个HashMap是一个weakHashMap，弱引用，不会内存泄露：Article很大，一直塞不回收会内存泄漏，   
   * 使用EventBus， 
   * 将Article保存到Application中，整个应用都能读写到这个数据
2. 不要再主线程中进行网络请求，不要再子线程刷新UI
3. 监听Activity的声明周期：
   * 面板退出上报埋点，但是Activity被destroy掉没有来得及监听到destroy状态进行上报。使用lifecycle进行状态监听，getLifrCycle.removeObserver方法。当Activity或者Fragment的生命周期发生变化的时候得到通知。

### 二 首帧优化
#### 一 layer延迟添加
1. 背景：目前西瓜/头条播放器的 Layer 没有区分场景添加，不论某个 Layer 是否在当前视频场景下是否使用，都会添加在一个通用的场景上。而 Layer 在视频起播之前，就会去初始化所持有的 View，在没有 RecyclerView 的 ViewHolder 复用的情况下，初始化 View 会消耗大量的时间，占用首帧时长，降低播放体验。
2. 技术方案：继承 BaseVideoLateInitLayer，layerEventListener进行监听，所有的layer继承该layer，然后重写构造方法，传入需要激活这个layer的一个或多个事件，在当前 Layer 未被激活前，不会响应所有事件，其所持有的View也未被初始化，如果当前 Layer 并非以某个事件进行激活，或在接受到某个事件并且还要满足其他情况才需要被使用，可以重写 boolean BaseVideoLateInitLayer#needInitLayer(IVideoLayerEvent)  方法，自行判断事件和条件是否满足初始化，返回 true/false 来决定是否初始化。通常这种情况可以继续收敛 Layer 的初始化，制造更精确的条件来避免不必要的初始化浪费；
3. 分成：触发播放时、调用点播play后，renderStart后，全屏半屏改变后，其他event或者行为触发，不触发不进行添加
4. 实验结果：feed可感知首帧降低，提升了feed的播放vv
5. 首帧时间：App层View初始化相关的UI逻辑 + 播放器创建、拉流、解码、渲染所花费的时间 = 可感知首帧

#### 二 耗时方法优化
1. 使用trace看了在正式起播前的耗时方法
2. 去除耗时的方法：  
   * VideoShop 无用Log，获取点播最大音量和当前音量方法。
   * 耳机注册耗时：每次播放都会注册一次耳机，现在改成单例的形式
   * 详情页优化toolbar 设置Pre和next 设置时机，不需要在进入详情后立即设置。在点击屏幕展示pre和next时再显示
   * 添加post runnable
     * Layer 的try_play 和prepared和release 的event(播放流程的部分event)
     * 调用engine play和renderStart之后延迟添加layer
3. 实验结果：显著降低feed可感知首帧
