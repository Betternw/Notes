## 目录
### * [性能优化总结](#1)
### * [内存泄露汇总](#2)
### * [UI基础](#3)     
### * [常用组件](#4)
 * #### [Activity](#5)
 * #### [Menu](#6)
 * #### [Dialog](#7)
 * #### [Fragment](#8)
 * #### [ViewPager](#9)
 * #### [事件分发](#10)
 * #### [网络操作](#11)
 * #### [Handler通信](#12)
 * #### [AsyncTask异步任务](#13)
### * [高级控件](#14)
 * #### [动画总结](#15)
 * #### [进程优先级](#16)
 * #### [屏幕适配](#17)
### * [数据存储](#18)
 * #### [本地文件操作](#19)
 * #### [数据库操作](#20)
 * #### [Context详解](#21)
 * #### [BroadcastReceiver](#22)
 * #### [Application全局应用](#23)
### * [框架](#24)
* #### [OkHttp](#25)
* #### [EventBus](#26)
* #### [RecyclerView](#27)
* #### [Mvp模式](#28)
* #### [<include merge>和viewStub](#29)
* #### [极光推送](#30)
* #### [WebView浏览器组件](#31)
* #### [ButterKnife实现View注入](#32)
### * [高级应用](#33)
* #### [Service](#34)
* #### [Binder机制和AIDL使用](#35)
* #### [ContentProvider](#36)
* #### [Bitmap压缩策略](#37)
* #### [HandlerThread](#38)
* #### [IntentService](#39)
* #### [LRUCache](#40)
* #### [Window、Activity、DecorView以及ViewRoot之间的关系](#41)
* #### [View测量布局及绘制原理](#42)
* #### [Android虚拟机及编译过程](#43)
* #### [进程间通信方式](#44)
## <span id = "1"> 性能优化总结</span>
### 一 布局优化
1. 删除布局中无用的控件和层次，选择性能比较低的viewFroup
2. 采用标签，viewStub
   * 标签主要用于布局重用，可以减少布局的层级
   * viewStub可以进行按需加载，当需要时才会将VIewStub的布局加载到内存，提高了初始化效率
3. 避免过度绘制
   * 移除不必要的背景
   * 自定义控件使用 clipRect() 和 quickReject() 优化。即设置一个绘制区域，

### 二 绘制优化
1. onDraw中不要创建新的局部对象，因为此方法会被频繁调用，一瞬间会产生大量的临时对象，不仅占用了过多的内存还会导致系统更加频繁gc，降低了程序的执行效率
2. onDraw方法中不要做耗时任务，也不要进行循环操作

### 三 内存泄露优化
### 四 ListView/RecycleView以及bitmap优化
ListView/RecycleView的优化：
1. 使用ViewHolder——缓存了显示数据的视图，如果convertView值为null时，会根据xml为进行赋值，并且生成一个viewholder来绑定converView中的每个view控件，然后将viewHolder设置到Tag中。当convertView不为空的时候，就会直接用convertView的getTag来获得一个ViewHolder。
2. 异步加载：耗时的操作放在异步线程中

bitmap优化:
对图片进行压缩

### 五 线程优化
1. 采用线程池，重用其中的线程
## <span id = "2">第一章</span>
#### 一 集合类泄露
集合中的元素只添加不删除

#### 二 单例造成的内存泄露
context对象被单例引用持有，当Activity退出时引用还被持有。
改进：使用Application的context

#### 三 非静态内部类创造静态实例造成内部泄露
1. 非静态内部类默认持有外部类的实例，而非静态内部类有又创建了一个静态的实例，导致该静态实例会一直持有该Activity的引用
2. 改进：将该内部类设为静态内部类或者将内部类封装为一个单例，如果需要使用Context可以使用Application的Context。在启动Activity和展示Dialog的时候不能使用Application的context，其他都可以

#### 四 匿名内部类
1. 继承实现Activity/Fragment/View的类中使用了匿名类，这个匿名类的实现对象会持有外部类的引用，如果将匿名类的对象引用传入一个异步线程，这个线程和Activity生命周期不一致的时候，就会造成泄漏

#### 五 Handler造成的泄露
1.  Handler 发送的 Message 尚未被处理，则该 Message 及发送它的 Handler 对象将被线程 MessageQueue 一直持有。当Activity被finish掉的时候，延迟执行任务的 Message 还会继续存在于主线程中，它持有该 Activity 的 Handler 引用（Handler是非静态内部类，持有外部类的引用），所以此时 finish() 掉的 Activity 就不会被回收了从而造成内存泄漏
2.  改进：将 Handler 声明为静态的；通过弱引用的方式引入 Activity，避免直接将 Activity 作为 context 传进去

#### 总结
使用静态内部类和弱引用
## <span id = "3">UI基础</span>
1. Activity：可视化界面
2. setContentView：设置内容视图，传入的参数就是要加载的布局。
3. R：为每一个资源文件按类别分配索引。通过R.类别名.资源名去操作对应的资源。
4. Manifest.xml文件：每创建一个activity，就会自动注册。Manifest.xml——进入到activity——oncreate——setcontentview
5. 布局
* 线性布局：linearout  新建xml文件。从上至下或者从左至右，与控件添加顺序有关。id常用命名：例如TextView  写tv_功能
* 相对布局：RelativeLayout  控件默认左上角  指定属性 layout_below/above/toRightof = 某id  指定在某id的下面或者右边
* 帧布局 ： FrameLayout 一层一层显示
* 表格布局：TableLayout  一个tabrow就代表一行
* 绝对布局 AbsoluteLayout
* 约束布局
6. 添加布局
* 利用xml文件
* 利用java代码
7. 布局重要属性
* android:layout_width：宽度
* android:layout_height：高度
* android:layout_padding：内边距
* android:layout_margin：外边距
* android:background：背景色
8. 线性布局
* res——layout——new 进行创建，文件名全小写。
* android:orientation：方向  vertical：垂直  horizontal：水平
* android:layout_weight：权重  针对控件。
比如一行两个文本框，但是是不同的 大小。android:layout_weight = “ 1” 。如果三个控件，只有一个设置了权重。那么优先分配其他的控件，剩下的空间都给设置了权重的控件。
按比例分配空间：width调为0，所有控件weight设置数字，就是控件之间的大小宽度比例。
* android:layout_gravity：重力。设置靠上下左右中间。（top bottom center 等）
9. 相对布局
* 以某个位置作为参照的一些位置
* 相对于父容器：android:layout_alignParentRight（上下左右居中）=true/false
* 相对于其他控件：android:layout_toRightOf/above/below="@id/其他id”——要用到其他控件的id。
android:layout_alignTop：和上边线对齐
### 基础控件
1. 控件：View
2. 通用属性
* android:layout_width：match—填充 wrap—根据内容确定 dp：精确大小
*  android:layout_height
*  android:id—@id/valname 使用已存在id @+id/valname：添加新的id
*  android:layout_margin：dp 和相邻控件或边缘的距离
*  android:padding：dp 控件内容距离控件边缘的距离
*  android:background：颜色作为背景或者图片作为背景（@mipmap/resourceId）
*  android:layout_gravity：自己相对于父容器的偏向
*  android:gravity：控件内容的偏向
*  android:visibility： 可见性
3. TextView
* 继承自View，子类有Button和EditText
* 对长文本进行显示处理：
* `strings.xml：
<string name = "abc">    这是ABC</string>
        android:text="@string/abc"`
* 支持html代码，内容有样式和链接效果。
* `android:textSize="22sp"
android:textColor="00ffff"
android:lineSpacingMultiplier="2"—行距
android:lineSpacingMultiplier="15sp"—倍距`
*滚动条控件内只能放一个控件。想放多个就在多个控件外面放一个布局，然后布局外面加滚动条。

```java
<ScrollView</ScrollView>
```
* 单行显示设置
* `android:singleLine="true"
android:lines="1"
android:ellipsize="start" //设置省略号位置
android:ellipsize="marquee"//跑马灯
android:focusable="true"//设置可以获取焦点，一个屏幕只能有一个跑马灯因为只有一个焦点。
android:focusableInTouchMode="true"//设置触摸时可以获取焦点
android:marqueeRepeatLimit="marquee_forever"//设置跑马灯的重复次数`
4. EditText：输入框

```java
android:inputType="textPassword" //设置输入类型，比如密码数字等
android:hint="Name"//提示文字
android:maxLength="12"//最大长度
```
5. Button
* 自定义内部类

```java
    //1. 根据id获取按钮
    Button btn1 = findViewById(R.id.btn1);
    //点击事件，被点击时触发的事件.括号内参数是接口
    MyClickListener mc1 = new MyClickListener();
    btn1.setOnClickListener(mc1);//2. 为按钮注册点击事件监听器
}
public  class MyClickListener implements View.OnClickListener{

    @Override
    public void onClick(View v) {
        //在控制台输出语句
        Log.e("TAG","内部类");
    }
}
```

* 匿名内部类
```java
    Button btn2 = findViewById(R.id.btn2);
    btn2.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Log.e("TAG","匿名内部类");
            
        }
    });
}
```
匿名内部类可以准确知道每个按钮需要执行的功能。
内部类适用于按钮的操作都类似
* 当前Activity去实现事件接口

```java
public class ButtonActivity extends AppCompatActivity implements View.OnClickListener {
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_button2);
Button btn3 = findViewById(R.id.btn3);
btn3.setOnClickListener(this);
@Override
public void onClick(View v) {
    Log.e("TAG","用本类实现");

}
```
* 在布局文件中添加点击事件属性

```java
<Button
    android:id="@+id/btn4"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="myClick"/>
<Button
    android:id="@+id/btn5"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="myClick"/>
//点击时被回调
//参数：被点击的控件对象
//点击时被回调
//参数：被点击的控件对象
public void myClick(View v){
    switch (v.getId()){
        case R.id.btn4:
            Log.e("Tag","btn4");
            break;
        case R.id.btn5:
            Log.e("Tag","btn5");
            break;
    }

}
```
 6.  ImageView
 - android：src="@minmap或者drawable/名字"
 - android：background=“” 设置背景
 - 图片名字命名是英文命名
 - mipmap：缩放程度会自己改变，
 - drawable：也可以放图片
 
 7. ImageButton
 有src和background的属性
 8. ProgressBar

```java
<ProgressBar
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    style="?android:attr/progressBarStyleHorizontal" //设置风格（水平进度条）
    android:progress="30"//设置进度
    android:max="200"//设置最大值，默认100
    android:indeterminate="true"/> //设置进度条一直滚动
```

```java
final ProgressBar progressBar = findViewById(R.id.progress);
progressBar.setProgress(80);//设置进度条进度为百分之80
//不能直接线程中操作控件，进度条是个特例。
//进度条从0——100增长，用线程实现
new Thread(){
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            progressBar.setProgress(i);
            try {
                Thread.sleep(30);// 增加的间隔时间
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}.start();
```
9. 约束布局
* 移动控件四个边的边线来确定相对于父容器或者其他控件的位置。 
* 添加约束
* Guidelines：水平对称添加垂直guidelines
10. CheckBox——继承于button，多选控件
* setChecked（）
```java
//设置是否选中
checkBox.setChecked(false);
```
* isChecked（）
```java
//获取状态（是否选中）
boolean isChecked = checkBox.isChecked();
```
*setOnCheckedChangeListener
```java
//监听状态，当状态被改变的时候可以处理很多数据和UI
checkBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
    checkBox = isChecked;

    }
});
```
11. RadioButton——单选空间
* 与Radiogroup一起使用，其中放几个button
```java
// 设置单选框listener
mSexRadioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup group, int checkedId) {
        switch (checkedId){
            case R.id.maleRadioButton:
                mPerson.setSex("男");
                break;
            case R.id.femaleRadioButton:
                mPerson.setSex("女");
                break;
        }
    }
});
```

12. ToggleButton
* 切换程序中的状态
* android：textOn：设置打开时的文字
* android：textOff：设置关闭时的文字
* setChecked（boolean）：设置是否被选中
13. SeekBar
* 使用进度条
* max：最大值
* progress：进度
* setProgress：设置进度
```java
SeekBar seekBar = findViewById(R.id.seek);
seekBar.setProgress(40);
seekBar.setMax(100);
```
* setOnSeekBarChangeListener监听器
```java
seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        //监督进度改变，会打印出过程

    }
    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {
        //开始的进度值

    }
    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {
        //结束的进度值
        mPrice = seekBar.getProgress();
        Toast.makeText(MainActivity.this, "价格： " + mPrice, Toast.LENGTH_SHORT).show();

    }
});
```
14. 综合案例的实现
* 初始化控件——findBiewById
* 初始化数据
* 为控件添加监听器

### <span id = "4">常用组件</span>
### <span id = "5"> Activity</span>
#### 一. 
1. 一个页面就是一个Activity
2. 启动那个Activity，哪个注册intent-filter
3. Activity跳转：使用Intent
```java
//Activity跳转
Intent intent = new Intent(ButtonActivity.this,ProgressBarActivity.class);
startActivity(intent);
```
#### 二. 启动模式：
##### 1. 标准模式(standard)
1. 默认是这个，按照顺序，每启动一次Activity就会创建一个新的实例并置于栈顶。
2. 谁启动了新的Activity，就会运行在启动该Activity的栈中。例如：Activity A启动了Activity B，则就会在A所在的栈顶压入一个新的Activity。（5.0后如果是跨进程启动会放入新的栈中，因为属于不同的程序）
3. 特殊情况，如果在Service或Application中启动一个Activity，其并没有所谓的任务栈，可以使用标记位Flag来解决。解决办法：为待启动的Activity指定FLAG_ACTIVITY_NEW_TASK标记位，创建一个新栈。
4. 应用场景： 绝大多数Activity。如果以这种方式启动的Activity被跨进程调用，在5.0之前新启动的Activity实例会放入发送Intent的Task的栈的顶部，但是属于不同的程序，在5.0之后，上述情景会创建一个新的Task，新启动的Activity就会放入刚创建的Task中。
##### 2. 栈顶复用模式（singleTop）
1. 如果需要新建的Activity位于栈顶，不需要重新新建这个Activity以及回调其他声明周期方法，会重用栈顶实例，并回调onNewIntent方法。
2. 如果栈顶不是新建的Activity,就会创建该Activity新的实例，并放入栈顶。
3. 应用场景：在通知栏点击收到的通知，然后需要启动一个Activity，这个Activity就可以用singleTop，否则每次点击都会新建一个Activity。同standard模式，如果是外部程序启动singleTop的Activity，在Android 5.0之前新创建的Activity会位于调用者的Task中，5.0及以后会放入新的Task中。
##### 3. 栈内复用模式（singleTask）
1. 单例模式，一个栈内只有一个该Activity实例。
2. 在AndroidManifest文件的Activity中指定该Activity需要加载到那个栈中，即singleTask的Activity可以指定想要加载的目标栈。singleTask和taskAffinity配合使用，指定开启的Activity加入到哪个栈中。
   ``` java
        <activity android:name=".Activity1"
            android:launchMode="singleTask"
            android:taskAffinity="com.lvr.task"
            android:label="@string/app_name">
        </activity>
   ```
3. taskAffinity：每个Activity都有taskAffinity属性，指明了需要进入的Task。如果一个Activity没有显式的指明该Activity的taskAffinity，那么它的这个属性就等于Application指明的taskAffinity，如果Application也没有指明，那么该taskAffinity的值就等于包名。
4. 执行逻辑：
* 如果Activity指定的栈不存在，则创建一个栈并将创建的Activity压入栈中。
* 如果指定的栈存在，且其中没有该Activity实例，会创建并压入栈顶。
* 如果指定的栈存在，但是其中有该Activity实例，则把该Activity实例之上的Activity杀死清除出栈，重用该实例并让该Activity实例处在栈顶，然后调用onNewIntent()方法。
1. 应用场景：大多数App的主页。当我们第一次进入主界面之后，主界面位于栈底，以后不管我们打开了多少个Activity，只要我们再次回到主界面，都应该将主界面Activity上所有的Activity移除，来让主界面Activity处于栈顶，而不是往栈顶新加一个主界面Activity的实例，通过这种方式能够保证退出应用时所有的Activity都能报销毁。在跨应用Intent传递时，如果系统中不存在singleTask Activity的实例，那么将创建一个新的Task，然后创建SingleTask Activity的实例，将其放入新的Task中。
##### 4. 单例模式（singleInstance）
1. 打开该Activity时，会直接创建一个新的任务栈，并创建该Activity实例放入新栈中。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例。
2. 应用场景： 呼叫来电界面。这种模式的使用情况比较罕见，在Launcher中可能使用。或者你确定你需要使Activity只有一个实例。建议谨慎使用。
##### 5. 特殊情况——前台栈和后台栈的交互
假如目前有两个任务栈。前台任务栈为AB，后台任务栈为CD，这里假设CD的启动模式均为singleTask,现在请求启动D，那么这个后台的任务栈都会被切换到前台，这个时候整个后退列表就变成了ABCD。当用户按back返回时，列表中的activity会一一出栈——D,C,B,A

如果不是请求启动D而是启动C，那么情况又不一样——C,B,A

调用SingleTask模式的后台任务栈中的Activity，会把整个栈的Actvity压入当前栈的栈顶。singleTask会具有clearTop特性，把之上的栈内Activity清除。

##### 6. Activity的Flags
用于设定Activity的启动模式。可以在启动Activity时，通过Intent的addFlags()方法设置。

(1)FLAG_ACTIVITY_NEW_TASK 其效果与指定Activity为singleTask模式一致。

(2)FLAG_ACTIVITY_SINGLE_TOP 其效果与指定Activity为singleTop模式一致。

(3)FLAG_ACTIVITY_CLEAR_TOP 具有此标记位的Activity，当它启动时，在同一个任务栈中所有位于它上面的Activity都要出栈。
* 如果和singleTask模式一起出现，若被启动的Activity已经存在栈中，则清除其之上的Activity，并调用该Activity的onNewIntent方法。
* 如果被启动的Activity采用standard模式，那么该Activity连同之上的所有Activity出栈，然后创建新的Activity实例并压入栈中。
#### 三. 生命周期 
#### 1. 典型生命周期
   (1). on Create() on Start() onResume() onPause() onStop() onDestory() onRestart()
   (2). on Create() ：当 Activity 第一次创建时会被调用。做一些初始化工作：比如调用setContentView去加载界面布局资源，初始化Activity所需的数据，借助onCreate()方法中的Bundle对象来回复异常情况下Activity结束时的状态
   (3). onRestart()：表示Activity正在重新启动——Activity从不可见重新变为可见状态时。这种情形一般是用户行为导致的，比如用户按Home键切换到桌面或打开了另一个新的Activity，接着用户又回到了这个Actvity。
   (4). onStart(): 表示Activity正在被启动，即将开始，但是还没有出现在前台，无法与用户交互。这个时候可以理解为Activity已经显示出来，但是我们还看不到。——此时在后台
   (5). onResume():表示Activity已经可见了，并且出现在前台并开始活动。onStart的时候Activity在后台，onResume的时候Activity才显示到前台。
   (6). onPause():表示 Activity正在停止，仍可见，正常情况下，紧接着onStop就会被调用。在特殊情况下，如果这个时候快速地回到当前Activity，那么onResume就会被调用（极端情况）。onPause中不能进行耗时操作，会影响到新Activity的显示。因为onPause执行完，新的Activity的onResume才会执行。
   (7). onStop():表示Activity即将停止，不可见，位于后台。可以做稍微重量级的回收工作，同样不能太耗时。
   (8). onDestory():表示Activity即将销毁，这是Activity生命周期的最后一个回调，可以做一些回收工作和最终的资源回收。
#### 2. 生命周期的普通和特殊情况
##### 2.1 生命周期的普通情况
(1) 针对一个特定的Activity，第一次启动: create-start-resume
(2) 用户打开新的Activiy的时候，上述Activity: pause-stop
(3) 再次回到原Activity时: restart-start-resume
(4) 按back键回退时: pause-stop-destroy
(5) 按Home键切换到桌面后又回到该Actitivy: pause-stop-restart-start-resume
(6) 调用finish()方法后，回调如下：onDestory()(以在onCreate()方法中调用为例，不同方法中回调不同，通常都是在onCreate()方法中调用)
##### 2.2 生命周期的特殊情况
1. 横竖屏切换：
   * Activity会被终止后再重建，状态保存在bundle中。其中onCreate和onRestoreInstanceState方法来恢复Activity的状态的区别： onRestoreInstanceState回调则表明其中Bundle对象非空，不用加非空判断。onCreate需要非空判断。建议使用onRestoreInstanceState。
   * onPause()->onSaveInstanceState()-> onStop()->onDestroy()->onCreate()->onStart()->onRestoreInstanceState->onResume()
   * 避免横竖屏切换的重建：在AndroidManifest文件的Activity中指定configChanges属性，从而横竖屏切换的时候会直接调用onCreate方法中的onConfigurationChanged方法，而不会重新执行onCreate方法，那当然如果不配置这个属性的话就会重新调用onCreate方法了
   ```java
    android:configChanges = "orientation| screenSize"
   ```
2. 资源内存不足导致优先级低的Activity被杀死
* 前台Activity——正在和用户交互的Activity，优先级最高。
* 可见但非前台Activity——比如Activity中弹出了一个对话框，导致Activity可见但是位于后台无法和用户交互。
* 后台Activity——已经被暂停的Activity，比如执行了onStop，优先级最低。
* 当系统内存不足时，会按照上述优先级从低到高去杀死目标Activity所在的进程。这种情况下数组存储和恢复过程和上述情况一致，生命周期情况也一样。

#### 四. Activity之间的数据传递
* 使用intent
``` java
//MainActivity中：
  private void initViews() {
        findViewById(R.id.buttonActivity).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //提取intent
                final Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                intent.putExtra(BUTTON_TITLE,getString(R.string.imooc_title));
                startActivity(intent);

//SecondActivity中：
 protected void onCreate(@Nullable Bundle savedInstanceState) {
     //接收数据
             if (getIntent() != null) {
                String buttonTitle = getIntent().getStringExtra(MainActivity.BUTTON_TITLE);
                button.setText(buttonTitle);        

 }
```
* 使用Bundle
``` java
//MainActivity中：
  private void initViews() {
        findViewById(R.id.buttonActivity).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                            //Bundle类似HashMap
                Bundle bundle = new Bundle();
                bundle.putString(BUTTON_TITLE, getString(R.string.imooc_title));
                intent.putExtra(BUTTON_TITLE, bundle);
                startActivity(intent);}
//SecondActivity中：
 protected void onCreate(@Nullable Bundle savedInstanceState) {
             //接收数据
        if (getIntent() != null) {
            Bundle bundle = getIntent().getBundleExtra(MainActivity.BUTTON_TITLE);
            if (bundle != null) {
                String buttonTitle = bundle.getString(MainActivity.BUTTON_TITLE);
                button.setText(buttonTitle);
            }

 }

```
* 序列化
``` java
//MainActivity中：
  private void initViews() {
        findViewById(R.id.buttonActivity).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
             //序列化
                intent.putExtra(BUTTON_TITLE, new User());
                startActivity(intent);
            }
//User中：
//必须继承Serializable
public class User implements Serializable{
    public String title="这是第三种方式默认标题";
}
 //SecondActivity中：
 protected void onCreate(@Nullable Bundle savedInstanceState) {
            User user=(User) (getIntent().getSerializableExtra(MainActivity.BUTTON_TITLE));
            button.setText(user.title);*/
        }



```
* 第二个页面结束时传值回第一个页面
``` java
 //SecondActivity中：
   protected void onCreate(@Nullable Bundle savedInstanceState) {
        //结束时传回
        final Button button = (Button) findViewById(R.id.buttonFinish);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.putExtra("imooc", "imooc+慕课网");
                setResult(RESULT_OK, intent);
                finish();
            }
        });
//MainActivity中：
//1. startActivityForResult
//2. 第二个页面setResult

  Bundle bundle = new Bundle();
                bundle.putString(BUTTON_TITLE, getString(R.string.imooc_title));
                intent.putExtra(BUTTON_TITLE, bundle);
                startActivityForResult(intent, 999);
//3. onActivityResult
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        //请求数据与返回的匹配
        if (requestCode == 999 && resultCode == RESULT_OK) {
             if(data!=null){
                //setTitle("前一个页面回来了");
                setTitle(data.getStringExtra("imooc"));
                
            }
        }
    }
```
### <span id = "6">Menu</span>
1. 选项菜单（OptionMenu）
* 操作栏中间
* 创建菜单和加载菜单资源
```java
<item android:title="保存"/>
<item android:title="设置"/>
<item android:title="更多操作" >
    <menu >
        <item android:title="子菜单1" />
        <item android:title="子菜单2" />
        <item android:title="子菜单3" />
    </menu>
</item>
```
```java
//创建optionMenu
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    //加载菜单资源
    getMenuInflater().inflate(R.menu.option,menu);
    return true;
}
```
* 属性：

```java
app:showAsAction="always"/> //显示，其余在三个点里
```
```java
android:icon="@mipmap/people"
app:showAsAction="always|withText"/> //当图标是图片，并且还想显示文本时
```
* 点击方法：
```java
@Override
public boolean onOptionsItemSelected(@NonNull MenuItem item) {
    switch (item.getItemId()){
        case R.id.save:
            Toast.makeText(this,"保存",Toast.LENGTH_LONG).show();
            break;
        case R.id.setting:
            Toast.makeText(this,"设置",Toast.LENGTH_LONG).show();
            break;
    }
    return super.onOptionsItemSelected(item);
}
```

2. 上下文菜单（ContextMenu）
* 长按某个view不放时会弹出
* 注册
```java
registerForContextMenu(findViewById(R.id.ctx_btn));
```
* 创建
```java
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
    getMenuInflater().inflate(R.menu.context,menu);
}
```
* 菜单项操作
```java
@Override
public boolean onContextItemSelected(@NonNull MenuItem item) {
    return super.onContextItemSelected(item);
}
```
* 为按钮设置上下文操作模式：上下文菜单的操作会在菜单栏中进行，不在页面中间进行
```java
ActionMode.Callback cb = new ActionMode.Callback() {
    //创建，启动上下文模式时候调用
    @Override
    public boolean onCreateActionMode(ActionMode mode, Menu menu) {
        getMenuInflater().inflate(R.menu.context,menu);
        return false;
    }

    //创建方法后调用
    @Override
    public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
        return false;
    }

    //switch语句
    @Override
    public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
        return false;
    }
    //上下文操作模式结束时调用
    @Override
    public void onDestroyActionMode(ActionMode mode) {

    }
};
```

```java
//view的长按事件中启动上下文操作模式
findViewById(R.id.ctx_btn).setOnLongClickListener(new View.OnLongClickListener() {
    @Override
    public boolean onLongClick(View v) {
        startActionMode(cb);
        return false;
    }
});
```

3. 弹出菜单（PopupMenu）
* 绑定在View的下方
* 实例化popupmenu对象(参数2：被绑定的view）
```java
final Button popopBtn = findViewById(R.id.popup_btn);
popopBtn .setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        //1 实例化popupmenu对象(参数2：被绑定的view）
        PopupMenu menu = new PopupMenu(ProgressBarActivity.this,popopBtn);
```
* 加载菜单资源 将第一个参数加载到第二个参数中
```java
menu.getMenuInflater().inflate(R.menu.popup,menu.getMenu());
```
*设置监听器
```java
menu.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
    @Override
    public boolean onMenuItemClick(MenuItem item) {
        //switch操作
        return false;
    }
});
```
* 显示
```java
menu.show();
```
4. onCreateOptionsMenu()方法必须返回true
5. onOptionsItemSelected方法返回true
6. onOptionsItemSelected最后需要 调用父类的默认实现  default：super.onOptionsItemSelected(item)

###  <span id = "7">Dialog</span>
1. AlertDialog——弹出式对话框
* 创建对象
```java
//设置button点击方法
android:id="@+id/normal"
android:onClick="myClick"/>
public void myClick(View view){
    switch (view.getId()){
        case R.id.normal:
            //AlertDialog的构造方法是protected，所以需要使用builder
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.setTitle("提示");
            builder.setMessage("确定吗");
            builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
                @Override
                public void onClick(DialogInterface dialog, int which) {
                    finish();
                }
            });
            //点击yes退出程序，点取消退出对话框
            builder.setNegativeButton("取消",null);
            builder.show();
            break;
```

2. 自定义Dialog
* 设置布局
新建一个布局，表示弹出的自定义dia的样式
* 设置style
在styles.xml中设置样式
去掉标题栏，背景设置为透明
```java
<style name="mydialog" parent="android:style/Theme.Dialog">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowBackground">@android:color/transparent</item>
</style>
```
* 自定义Dialog
新建类
```java
public class MyDialog extends Dialog {

    public MyDialog(@NonNull final Context context, int themeResId) {
        super(context, themeResId);
        //为对话框设置布局
        setContentView(R.layout.dialog_layout);

        findViewById(R.id.yes_btn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                System.exit(0); //退出程序
            }
        });

        findViewById(R.id.no_btn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                dismiss();//对话框消失
            }
        });
    }

```
* 显示

```java
case R.id.diy:
    MyDialog md = new MyDialog(this,R.style.mydialog);
    md.show();
```
3. PopupWindow
```java
case R.id.popup_btn:
    showPopupWindow(view);
    break;
```
* 创建实例
```java
//将布局当成视图生成，准备弹窗需要的视图对象
View v = LayoutInflater.from(this).inflate(R.layout.popup_layout,null);
//1. 实例化对象（参数1：用在弹窗中的view 参数2，3：弹窗的宽高 参数4：能否获取焦点）
final PopupWindow window = new PopupWindow(v,190,35,true);
```
* 设置背景、注册事件监听器和添加动画
```java
//2. 设置背景动画
//设置背景  设置背景为透明色
window.setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
//设置能相应外部的点击事件
window.setOutsideTouchable(true);
//设置弹窗能相应点击事件
window.setTouchable(true);
```
动画操作：
```java
//1). 创建动画资源 新建anim文件
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="0"
        android:toXDelta="0"
        android:fromYDelta="300"
        android:toYDelta="0"
        android:duration="2000"
        ></translate>
</set>
```
```java
//2). 创建style应用动画资源
<style name="translate_anim">
    <item name="android:windowEnterAnimation">@anim/translate</item>
</style>
```
```java
//3). 对当前弹窗的动画风格设置为第二步的资源索引
window.setAnimationStyle(R.style.translate_anim);
```
* 显示
```java
//3. 在参照物控件的下方显示 （参数1：锚，参照物 参数2 3：相对于锚在xy方向的偏移量）
window.showAsDropDown(view,-190,0);

//为弹窗中文本添加点击事件 注意是popup里的视图调用
v.findViewById(R.id.choose).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this,"选择",Toast.LENGTH_SHORT).show();
        window.dismiss();//点完消失
    }
});

v.findViewById(R.id.choose_all).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this,"全选",Toast.LENGTH_SHORT).show();
        window.dismiss();
    }
});

v.findViewById(R.id.copy).setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(MainActivity.this,"复制",Toast.LENGTH_SHORT).show();
        window.dismiss();
    }
});
```
4. ArrayAdapter——数组适配器，只能显示单一的文本
* id注册
```java
case R.id.arrayAdapter_btn:
                showArrayDialog();
                break;
```
* 设置ArrayDialog
```java
 private void showArrayDialog() {
        final String[] items = {"Java","Mysql","Android","HTML","C","JavaScript"};
        //数组适配器
        //参数1：环境
        //参数2：布局资源索引，指的是每一项数据所呈现的样式
        //参数3：数据源，集合或者数组
//        ArrayAdapter adapter = new ArrayAdapter(this,android.R.layout.simple_dropdown_item_1line,items);
        //参数3：int textviewid 指定文本你需要放在布局中对应id文本控制的位置
        ArrayAdapter adapter = new ArrayAdapter(this,R.layout.array_item_layout,R.id.item_txt,items);
        //实例化Builder
        AlertDialog.Builder builer = new AlertDialog.Builder(this)
                .setTitle("请选择")
                //参数1：适配器对象（对数据显示样式的规则制定器）
                //参数2：监听器
                .setAdapter(adapter, new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        Toast.makeText(MainActivity.this,items[i],Toast.LENGTH_SHORT).show();
                        //对话框消失
                        dialogInterface.dismiss();
                    }
                });
        builer.show();
    }
```

###  <span id = "8">Fragment</span>
### 一 什么Fragment
   * 一个Activity中放置运行多个Fragment，可以根据不同的分辨率进行页面的拆分
   * Fragment必须放在Activity中
   * 拥有自己的生命周期，可以在Activity中动态的添加、替换、移除不同的Fragment
### 二 生命周期
onAttach() -> onCreate() -> onCreateView() -> onActivityCreated() ->(这里对应Activity的created)
onAttach(): Fragment与Activity发生关联的时候调用
onCreateView(LayoutInflater, ViewGroup, Bundle)：创建该Fragment的视图
nActivityCreated(Bundle)：当Activity的onCreated方法返回时调用
onStart() -> onResume -> onPause -> onStop() -> 
onDestroyView() -> onDestroy() -> onDetach()
onDestroyView(): 与onCreateView方法相对应，当该Fragment的视图被移除时调用
onDetach(): 与onAttach方法相对应，当Fragment与Activity取消关联时调用
### 三 使用方式
#### 1. 静态加载：xml文件
* 创建一个类继承Fragment，重写onCreateView方法，来确定Fragment要显示的布局
* 在Activity中声明该类，与普通的View对象一样
```java
//1. MyFragment对应的布局文件item_fragment.xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:background="@color/colorAccent"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:src="@mipmap/ic_launcher" />

    </RelativeLayout>

//2. 继承Frgmanet的类MyFragment【请注意导包的时候导v4的Fragment的包】
public class MyFragment extends Fragment {
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        /*
        * 参数1：布局文件的id
        * 参数2：容器
        * 参数3：是否将这个生成的View添加到这个容器中去
        * 作用是将布局文件封装在一个View对象中，并填充到此Fragment中
        * */
        View v = inflater.inflate(R.layout.item_fragment, container, false);
        return v;
    }
}

//3. Activity对应的布局文件

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context="com.usher.fragment.MainActivity">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:gravity="center"
            android:text="Good Boy" />

        <fragment
            android:id="@+id/myfragment"
            android:name="com.usher.fragment.MyFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

    </LinearLayout>
```
#### 2. 动态加载：java文件（比如在本页面设置两个fragment）  
```java
// 1. Fragment对应的布局文件item_fragment.xml和Fragment2对应的布局文件item_fragment2.xml
// 2. 继承Frgmanet和Frgmanet1的类MyFragment和MyFragment1
// 3. MainActivity对应的布局文件
  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.usher.fragment.MainActivity">

    <FrameLayout
        android:id="@+id/myframelayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

  </LinearLayout>
// 4. MainActivity类

public class MainActivity extends AppCompatActivity {

    private Button bt_red;
    private Button bt_blue;
    private FragmentManager manager;
    private MyFragment fragment1;
    private MyFragment2 fragment2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();

        fragment1 = new MyFragment();
        fragment2 = new MyFragment2();

        //初始化FragmentManager对象
        manager = getSupportFragmentManager();

        //使用FragmentManager对象用来开启一个Fragment事务
        FragmentTransaction transaction = manager.beginTransaction();

        //默认显示fragment1
        transaction.add(R.id.myframelayout, fragment1).commit();

        //对bt_red设置监听
        bt_red.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FragmentTransaction transaction = manager.beginTransaction();
                transaction.replace(R.id.myframelayout, fragment1).commit();
            }
        });

        //对bt_blue设置监听
        bt_blue.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                FragmentTransaction transaction = manager.beginTransaction();
                transaction.replace(R.id.myframelayout, fragment2).commit();
            }
        });
    }

    private void initView() {
        bt_red = (Button) findViewById(R.id.bt_red);
        bt_blue = (Button) findViewById(R.id.bt_blue);
    }

 }
```
通过Fragment管理器对象调用beginTransaction()方法，实例化FragmentTransaction对象,transaction的方法主要有以下几种：
* transaction.add() 向Activity中添加一个Fragment
* transaction.remove() 从Activity中移除一个Fragment，如果被移除的Fragment没有添加到回退栈，这个Fragment实例将会被销毁
* transaction.replace() 使用另一个Fragment替换当前的，实际上就是remove()然后add()的合体
* transaction.hide() 隐藏当前的Fragment，仅仅是设为不可见，并不会销毁
* transaction.show() 显示之前隐藏的Fragment
* detach() 会将view从UI中移除,和remove()不同,此时fragment的状态依然由FragmentManager维护
* attach() 重建view视图，附加到UI上并显示
* ransatcion.commit() 提交事务
注意：在add/replace/hide/show以后都要commit其效果才会在屏幕上显示出来

### 四 回退栈
1. FragmentTransaction.addToBackStack(String)： 把当前事务的变化情况添加到回退栈
2. 调用replace -> 调用addToBackStack，使得被替换的Fragment实例不会被销毁，被添加到了回退栈。但是视图层次依然会被销毁，即会调用onDestoryView和onCreateView。因此返回前一个Fragment时候会进行重新绘制。
3. 调用hide -> 调用add -> 调用addToBackStack，不调用replace，当返回时视图不会进行重新绘制。
4. 原因：调用replace的生命周期是pause--stop--destroy，create--start，需要销毁前一个，耗时；而调用hide的生命周期是create--start，不耗时，但是占用内存。一般情况下如果Fragment不是很多就可以使用方式2来进行切换，这样能提高切换时的效率，保证app的流畅性（空间换时间）；如果Fragment比较多，并且对内存要求较高时，就用方式1来进行切换，保证app不会内存溢出（时间换空间）。


### 五 Fragment与Activity之间的通信
* 如果Activity中包含自己管理的Fragment的引用，可以通过引用直接访问所有的Fragment的public方法
* 如果Activity中未保存任何Fragment的引用，每个Fragment都有一个唯一的TAG或者ID,可以通过getFragmentManager.findFragmentByTag()或者findFragmentById()获得任何Fragment实例，然后进行操作
* Fragment中可以通过getActivity()得到当前绑定的Activity的实例，然后进行操作。

### 六 Fragment与Activity通信的优化
1. 最好不要Fragment之间相互操作，Activity担任的是Fragment间类似总线一样的角色，应当由它决定Fragment如何操作
2. Fragment不能响应Intent打开，但是Activity可以，Activity可以接收Intent，然后根据参数判断显示哪个Fragment。

### 七 处理运行时配置发生变化
1. 屏幕旋转时，视图进行了重新绘制。Activity和Fragment都进行了重新创建，很耗费内存资源。
2. Activity的onCreate的参数Bundle savedInstanceState中会存储一些数据，包括Fragment的实例，因此可以判断只有在savedInstanceState==null时，才进行创建Fragment实例，无论进行多次旋转都只会有一个Fragment实例在Activity中，
3. 重新绘制时，Fragment发生重建，原本的数据如何保持？ 和Activity类似，Fragment也有onSaveInstanceState的方法，在此方法中进行保存数据，然后在onCreate或者onCreateView或者onActivityCreated进行恢复都可以。

###  <span id = "9"> ViewPager</span>
1. 应用：引导界面、相册多图片预览；多Tab页面；广告播放展示（左滑右滑）
2. support包：compile引用
3. List<View> :上下滑动页面  PageAdapter:左右滑动切换页面
4. 功能实现
* 实现引导页和下滑点
```java
public class ImageViewPagerAdapter extends AppCompatActivity {

    public static final int INIT_POSITION = 1;
    private ViewPager mViewPager;

    //2. 创建三个页面用作切换用
    private int[] mLayoutIDs = {
            R.layout.view_first,
            R.layout.view_second,
            R.layout.view_thrid
    };
    private List<View> mViews;

    ////7 添加显示页面的布局（下面三个点，页面改变时点会随着改变）
    //7.1 定义变量
    private ViewGroup mDotViewGroup;
    private List<ImageView> mDotViews = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_image_view_adapter);

        //1. 获取当前页面
        mViewPager = (ViewPager) findViewById(R.id.view_pager);
        //4.6 获取点布局
        mDotViewGroup = (ViewGroup) findViewById(R.id.dot_layout);

        // 3. 初始化数据。将界面加到list（view）中
        mViews = new ArrayList<>();
        for (int index = 0; index < mLayoutIDs.length; index++) {
            //4.1 图片加到view中，下面将mview装入adapter中
            ImageView imageView = new ImageView(this);
            imageView.setImageResource(R.mipmap.ic_launcher);
            //4. 将页面layout加到view中，下面将view装入adapter中
            //普通添加页面
            //final View view  = getLayoutInflater().inflate(mLayoutIDs[index],null);
            //mViews.add(view);
            //4.2 放入mview
            mViews.add(imageView);

            //7.2 设置点的图片样式
            ImageView dot = new ImageView(this);
            dot.setImageResource(R.mipmap.ic_launcher);
            dot.setMaxWidth(100);
            dot.setMaxHeight(100);

            //7.3 设置点布局
            LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(80,80);
            layoutParams.leftMargin = 20;
            dot.setLayoutParams(layoutParams);
            dot.setEnabled(false);

            //7.4 将点添加到视图组和视图中
            mDotViewGroup.addView(dot);
            mDotViews.add(dot);

        }

        // 5. 设置adapter 需要填充数据——页面。其中adapter获取前面mview的数据
        mViewPager.setAdapter(mPagerAdapter);
        //4.3 滑动当前，左右两个都保存。
        mViewPager.setOffscreenPageLimit(4);
        //7.6 设置初始界面状态和位置
        mViewPager.setCurrentItem(INIT_POSITION);
        setDotViews(INIT_POSITION);

        //4.9 页面改变的设置
        mViewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {

                setDotViews(position);
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });
    }

    //7.5 设置，随着页面的改变，点的样式发生改变
    private void setDotViews(int position) {
        for (int index = 0; index < mDotViews.size(); index++) {
            mDotViews.get(index).setImageResource(position == index ? R.mipmap.diglett : R.mipmap.ic_launcher);

        }
    }


    //6. 创建mPagerAdapter对象，重写方法
    PagerAdapter mPagerAdapter = new PagerAdapter() {

        @Override
        //页数
        public int getCount() {
            return mLayoutIDs.length;
        }

        @Override
        public boolean isViewFromObject(View view, Object object) {

            return view == object;
        }

        @Override
        //将视图添加进去
        public Object instantiateItem(ViewGroup container, int position) {
            View child = mViews.get(position);
            container.addView(child);
            return child;
        }

        @Override
        //释放视图
        public void destroyItem(ViewGroup container, int position, Object object) {
            container.removeView(mViews.get(position));
        }
    };
}
```
```java
    //新建viewpager控件需要引包
    <android.support.v4.view.ViewPager
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />

    //4.4 添加显示页面的布局
    <LinearLayout
        android:id="@+id/dot_layout"
        android:layout_width="120dp"
        android:layout_height="30dp"
        android:gravity="center"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="30dp"
        android:orientation="horizontal">

    </LinearLayout>
```

* Fragment+viewpager(每一页不使用视图，使用fragment)
```java
TestFragment.java:
public class TestFragment extends Fragment {


    public static final String TITLE = "title";
    private String mTitle;

    //3 设置TestFragment 可以设置位置或者title，类型要改变
    public static TestFragment newInstance(String title) {
        TestFragment fragment = new TestFragment();
        Bundle bundle = new Bundle();
        bundle.putString(TITLE, title);
        fragment.setArguments(bundle);
        return fragment;
    }


    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //4 设置接收参数
        if (getArguments() != null) {
            mTitle = getArguments().getString(TITLE);
        }
    }

    @Nullable
    @Override
    // 1.创建Fragment并建立视图接收返回
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_test, null);
        // 5. 传入参数
        TextView textView = (TextView) view.findViewById(R.id.text_view);

        textView.setText(mTitle);

        return view;

    }

```
```java
TabViewpagerActivity.java:
//11.2.2 实现TabHost接口
public class TabViewPagerActivity extends AppCompatActivity implements TabHost.TabContentFactory{


    private TabHost mTabHost;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_tab_viewpager);

        //9 设置底部导航栏布局
        // 10 初始化总布局
        mTabHost = (TabHost) findViewById(R.id.tab_host);
        mTabHost.setup();

        // 11 三个Tab 做处理

        // 11.1. 初始化数据（要抽象到string中）
        int[] titleIDs = {
                R.string.home,
                R.string.message,
                R.string.me
        };
        int[] drawableIDs = {
                R.drawable.main_tab_icon_home,
                R.drawable.main_tab_icon_message,
                R.drawable.main_tab_icon_me
        };
        // 11.2 数据填充到布局中 data < -- > view
        for (int index = 0; index < titleIDs.length; index++) {

            //提取视图为view
            View view = getLayoutInflater().inflate(R.layout.main_tab_layout, null, false);
            //提取对象
            ImageView icon = (ImageView) view.findViewById(R.id.main_tab_icon);
            TextView title = (TextView) view.findViewById(R.id.main_tab_txt);
            View tab = view.findViewById(R.id.tab_bg);
            //设置
            icon.setImageResource(drawableIDs[index]);
            title.setText(getString(titleIDs[index]));
            tab.setBackgroundColor(getResources().getColor(R.color.white));
            //将tab加入到view中
            mTabHost.addTab(
                    mTabHost.newTabSpec(getString(titleIDs[index]))
                    .setIndicator(view)
                    .setContent(this) //11.2.1 当前视图的内容
            );

        }


        // 8. 三个fragment组成的viewpager，是底部导航栏的样式
        final Fragment[] fragments = new Fragment[]{
                TestFragment.newInstance("home"),
                TestFragment.newInstance("message"),
                TestFragment.newInstance("me")
        };
        //2. 创建vierpager
        final ViewPager viewPager = (ViewPager) findViewById(R.id.view_pager);
        viewPager.setOffscreenPageLimit(fragments.length);
        //3. 设置adapter
        viewPager.setAdapter(new FragmentPagerAdapter(getSupportFragmentManager()) {
            @Override
            public Fragment getItem(int position) {
                // 6. 传入要设置的参数
                // 传入pisition到TestFragment——setArguments——getArguments()，得到参数——进入oncreate进行设置
                return fragments[position];
            }

            @Override
            public int getCount() {

                return fragments.length;
            }
        });

        // 12 滑动时选择不同的tab
        viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {
                  if(mTabHost != null){
                      mTabHost.setCurrentTab(position);
                  }
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });

        // 13 tab切换时候页面也改变
        mTabHost.setOnTabChangedListener(new TabHost.OnTabChangeListener() {
            @Override
            public void onTabChanged(String s) {
                if (mTabHost != null) {
                    int position = mTabHost.getCurrentTab();
                    viewPager.setCurrentItem(position);
                }

            }
        });



    }

    //11.2.3 设置view，因为已经有了viewpager所以不需要再使用
    @Override
    public View createTabContent(String s) {
        View view = new View(this);
        view.setMinimumHeight(0);
        view.setMinimumWidth(0);
        return view;
    }
```
```java
fragment.xml:
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <TextView
        android:id="@+id/text_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:textSize="36sp"
        android:layout_centerInParent="true"
        android:text="@string/app_name"
        android:gravity="center"/>
</RelativeLayout>
```
```java
  // 7. 设置底部导航栏 Tabhost-TabWidget-FrameLayout-view(画线）
<TabHost
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/tab_host"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#f8f8f8"
    android:orientation="vertical">

      <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <android.support.v4.view.ViewPager
            android:id="@+id/view_pager"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_above="@+id/tab_divider"
            />

        <FrameLayout
            android:id="@android:id/tabcontent"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:visibility="gone"
            android:layout_above="@+id/tab_divider"
            >

        </FrameLayout>

        <View
            android:id="@+id/tab_divider"
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_above="@android:id/tabs"
            android:background="#dfdfdf"
            />

        <TabWidget
            android:id="@android:id/tabs"
            android:layout_width="match_parent"
            android:layout_height="60dp"
            android:layout_alignParentBottom="true"
            android:showDividers="none"
            >

        </TabWidget>

    </RelativeLayout>


</TabHost>

```
```java
main_tab_layout.xml:
 // 9. 设置底部导航栏样式 一个图标是一个layout
    <LinearLayout
        android:id="@+id/main_content"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:layout_centerInParent="true"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/main_tab_icon"
            android:layout_width="30dp"
            android:layout_height="30dp"
            android:layout_marginTop="4dp"
            android:scaleType="centerInside"
            android:src="@drawable/main_tab_icon_home"/>

        <TextView
            android:id="@+id/main_tab_txt"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="5dp"
            // 9.1 设置选中时图标颜色的变化
            android:textColor="@color/color_main_tab_txt"
            android:text="@string/home"/>
    </LinearLayout>

    <ImageView
        android:visibility="gone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

```
```java
新建color文件：color_main_tab_text.xml:
    // 9.1 设置选中时文本颜色的变化
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="#cccccc"/>
    <item android:state_selected="true" android:color="#4dd0c8"/>
    <item android:state_pressed="true" android:color="#4dd0c8"/>
</selector>
```
```java
drawable文件图片切换，三个文件，6种样式
main_tab_icon_home.xml:
//9.2 设置所有图片的变化前后效果。顺序是按照语句顺序的。希望状态切换样式改变，那么需要将状态变化设置放在前面
    <item android:state_pressed="true"  android:drawable="@drawable/tabbar_home_pressed" />
    <item android:state_selected="true" android:drawable="@drawable/tabbar_home_pressed" />
    <item android:drawable="@drawable/tabbar_home" />
```
### <span id = "10">事件分发</span>
### 一 基础认知
#### 1. 事件分发的对象
1. 事件分发的对象是事件
2. 当用户触摸屏幕时（View或ViewGroup派生的控件），将产生点击事件（Touch事件）。Touch事件相关细节（发生触摸的位置、时间、历史记录、手势动作等）被封装成MotionEvent对象
3. Touch主要有四种：
   * MotionEvent.ACTION_DOWN：按下View（所有事件的开始）
   * MotionEvent.ACTION_MOVE：滑动View
   * MotionEvent.ACTION_CANCEL：非人为原因结束本次事件
   * MotionEvent.ACTION_UP：抬起View（与DOWN对应）

#### 2. 事件分发的本质
1. 将点击事件MotionEvent传递给某一个view去处理，这个事件传递过程就是分发过程

#### 3. 事件在哪些对象之间进行传递
1. 传递顺序：Activity（window） -> viewGroup -> view
2. View是所有UI组件的基类; ViewGroup是容纳UI组件的容器，即一组View的集合, 比起View，它多了可以包含子View和定义布局参数的功能。

#### 4. 由哪些方法协作完成
1. dispatchTouchEvent()：分发传递点击事件，当点击事件能够被传递给当前view时会调用
2. onInterceptTouchEvent()：判断是否拦截了某个事件，在dispatchTouchEvent内部调用
3. onTouchEvent()：处理点击事件，在dispatchTouchEvent内部调用

### 二 事件分发机制方法&流程介绍
```java
    // 点击事件产生后，会直接调用dispatchTouchEvent（）方法
    public boolean dispatchTouchEvent(MotionEvent ev) {

        //代表是否消耗事件
        boolean consume = false;


        if (onInterceptTouchEvent(ev)) {
        //如果onInterceptTouchEvent()返回true则代表当前View拦截了点击事件
        //则该点击事件则会交给当前View进行处理
        //即调用onTouchEvent (）方法去处理点击事件
        consume = onTouchEvent (ev) ;

        } else {
        //如果onInterceptTouchEvent()返回false则代表当前View不拦截点击事件
        //则该点击事件则会继续传递给它的子元素
        //子元素的dispatchTouchEvent（）就会被调用，重复上述过程
        //直到点击事件被最终处理为止
        consume = child.dispatchTouchEvent (ev) ;
        }

        return consume;
    }
```

### 三 事件分发场景介绍
#### 1. 默认情况
1. 从Activity A---->ViewGroup B--->View C，从上往下调用dispatchTouchEvent()
2. 再由View C--->ViewGroup B --->Activity A，从下往上调用onTouchEvent()
3. 虽然ViewGroup B的onInterceptTouchEvent方法对DOWN事件返回了false，后续的事件（MOVE、UP）依然会传递给它的onInterceptTouchEvent()

#### 2. 处理事件
1. 假设View C希望处理这个点击事件，DOWN事件被传递给C的onTouchEvent方法，该方法返回true，表示处理这个事件
2. 因为C正在处理这个事件，那么DOWN事件将不再往上传递给B和A的onTouchEvent()；
3. 该事件列的其他事件（Move、Up）也将传递给C的onTouchEvent()

#### 3. 拦截事件
1. 假设ViewGroup B希望处理这个点击事件，即B覆写了onInterceptTouchEvent()返回true、那么事件不再往下传递自己进行处理
2. 调用onTouchEvent()处理事件（DOWN事件将不再往上传递给A的onTouchEvent()）
3. 该事件列的其他事件（Move、Up）将直接传递给B的onTouchEvent()
4. 该事件列的其他事件（Move、Up）将不会再传递给B的onInterceptTouchEvent方法，该方法一旦返回一次true，就再也不会被调用了。

#### 4. 拦截Down的后续事件
1. 假设ViewGroup B没有拦截DOWN事件（还是View C来处理DOWN事件），但它拦截了接下来的MOVE事件。DOWN事件传递到C的onTouchEvent方法，返回了true。
2. MOVE事件，B的onInterceptTouchEvent方法返回true拦截该MOVE事件，但该事件并没有传递给B的onTouchEvent方法
3. 这个MOVE事件将会被系统变成一个CANCEL事件传递给C的onTouchEvent方法
4. 后续又来了一个MOVE事件，该MOVE事件才会直接传递给B的onTouchEvent()、
5. C再也不会收到该事件列产生的后续事件。

#### 注意：
1. 如果ViewGroup A 拦截了一个半路的事件（如MOVE），这个事件将会被系统变成一个CANCEL事件并传递给子View；
2. 该事件不会再传递给ViewGroup A的onTouchEvent()
3. 只有再到来的事件才会传递到ViewGroup A的onTouchEvent()

### 四 Android事件分发机制源码分析
#### 1. Activity的事件分发机制
1. 当一个点击事件发生时，事件最先传到Activity的dispatchTouchEvent()进行事件分发
2. 调用Window类实现类PhoneWindow的superDispatchTouchEvent()
3. 调用DecorView的superDispatchTouchEvent()
4. 最终调用DecorView父类的dispatchTouchEvent()，即ViewGroup的dispatchTouchEvent()
5. 这样事件就从 Activity 传递到了 ViewGroup

#### 2. ViewGroup事件的分发机制
1. onInterceptTouchEvent方法返回true代表拦截事件，即不允许事件继续向子View传递；
2. 返回false代表不拦截事件，即允许事件继续向子View传递；（默认返回false）
3. 子View中如果将传递的事件消费掉，ViewGroup中将无法接收到任何事件。

#### 3. View事件的分发机制
1. onTouch（）的执行高于onClick（）
2. 每当控件被点击时：
   1. 如果在回调onTouch()里返回false，就会让dispatchTouchEvent方法返回false，那么就会执行onTouchEvent()；如果回调了setOnClickListener()来给控件注册点击事件的话，最后会在performClick()方法里回调onClick()。————onTouch()返回false（该事件没被onTouch()消费掉） = 执行onTouchEvent() = 执行OnClick()
   2. onTouch()返回true（该事件被onTouch()消费掉） = dispatchTouchEvent()返回true（不会再继续向下传递） = 不会执行onTouchEvent() = 不会执行OnClick()

### 五 思考点
#### 1，onTouch()和onTouchEvent()的区别
1. 有些控件是非enable的，即不能点击， 那么给它注册onTouch事件将永远得不到执行。对于这一类控件，如果我们想要监听它的touch事件，就必须通过在该控件中重写onTouchEvent方法来实现。
2. onTouch能够得到执行需要两个前提条件： mOnTouchListener的值不能为空，当前点击的控件必须是enable的。

#### 2. Touch事件的后续事件（MOVE、UP）层级传递
1. 如果给控件注册了Touch事件，每次点击都会触发一系列action事件（ACTION_DOWN，ACTION_MOVE，ACTION_UP等）
2. 当dispatchTouchEvent在进行事件分发的时候，只有前一个事件（如ACTION_DOWN）返回true，才会收到后一个事件（ACTION_MOVE和ACTION_UP）
3. 如果在某个对象（Activity、ViewGroup、View）的dispatchTouchEvent()消费事件（返回true），那么收到ACTION_DOWN的函数也能收到ACTION_MOVE和ACTION_UP
4. 如果在某个对象（Activity、ViewGroup、View）的onTouchEvent()消费事件（返回true），那么ACTION_MOVE和ACTION_UP的事件从上往下传到这个View后就不再往下传递了，而直接传给自己的onTouchEvent()并结束本次事件传递过程。
###  <span id = "11"> 网络操作</span>
1. 基础知识
* 服务端为客户端提供数据。
* 协议
   * HTTP：应用层。输入网址——DNS找到ip地址——通过HTTP请求转发到web服务器——从数据库中得到数据——转发回服务器——将HTTP解析的html文档等传输回web服务器，转发回客户端。
   * TCP：传输层。客户端和服务器端进行联系（三次握手）
   * URL：协议+主机域名+顶级域名+路径（虚拟目录）+文件名字+？+xxx&xxx&xxx（参数，通过参数返回不同的内容）
   * Http提供了封装或者显示数据的具体形式，Socket提供了网络通信的能力
* 网络请求
   * 从服务器获取数据
      * 实例化URL对象
      * 获取HttpURLConnection对象
      * 设置请求连接属性
      * 获取相应码，判断连接结果码
      * 读取输入流并解析
   * get请求
      * 从server获取数据
      * 将参数放在问号后面
      * 比如：http://example.com?data=3e
      ```java
        // 2. 在manifext.xml文件中申请网络权限
        <uses-permission android:name="android.permission.INTERNET"/> //允许程序打开网络套接字
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/> //允许程序访问网络连接信息
      ```

      ```java
        public void onClick(View v) {
        switch (v.getId()) {

            case R.id.getButton:
                // 1. 建立线程。主线程全部用来跑网络连接会超时崩溃
                // 2. 在manifext.xml文件中申请网络权限
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        String urlString = "http://www.imooc.com/api/teacher?type=2&page=1";
                        // 3. 构建url对象 获取数据
                        mResult = requestDataByGet(urlString);
                        // 4 请求网络在子线程，更新ui在主线程。子更改主线程UI方法：（handler消息机制）
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                // 4.1 转为utf-8格式
                                mResult = decode(mResult);
                                // 4.2 设置文本框内显示数据
                                mTextView.setText(mResult);
                            }
                        });
                    }
                }).start();
                break;
               // 3. 构建url对象 获取数据
        private String requestDataByGet(String urlString) {
            String result = null;
            try {
                URL url = new URL(urlString);
                //打开connection连接
                HttpURLConnection connection = (HttpURLConnection) url.openConnection();
                //超时时间 默认毫秒
                connection.setConnectTimeout(30000);
                //请求的方法类型
                connection.setRequestMethod("GET");  // GET POST
                //请求属性 参数1：数据类型 参数2：json————设置拿到的数据类型是json格式的
                connection.setRequestProperty("Content-Type", "application/json");
                //设置数据集的格式格式
                connection.setRequestProperty("Charset", "UTF-8");
                //设置接收的数据集格式
                connection.setRequestProperty("Accept-Charset", "UTF-8");
                //发起连接
                connection.connect();
                //获取请求码  知道连接状态
                int responseCode = connection.getResponseCode();
                if (responseCode == HttpURLConnection.HTTP_OK) {
                    //输入流数据
                    InputStream inputStream = connection.getInputStream();
                    // 3.1 将输入流转换为字符串
                    result = streamToString(inputStream);
                } else {
                    //请求消息
                    String responseMessage = connection.getResponseMessage();
                    Log.e(TAG, "requestDataByPost: " + responseMessage);
                }
            } catch (MalformedURLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
            return result;
        }

        /**
        * 将输入流转换成字符串
        *
        * @param is 从网络获取的输入流
        * @return 字符串
        */
        //3.1 将输入流转换为字符串
        public String streamToString(InputStream is) {
            try {
                //管道
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                byte[] buffer = new byte[1024];
                int len;
                while ((len = is.read(buffer)) != -1) {
                    baos.write(buffer, 0, len);
                }
                baos.close();
                is.close();
                byte[] byteArray = baos.toByteArray();
                return new String(byteArray);
            } catch (Exception e) {
                Log.e(TAG, e.toString());
                return null;
            }
        }
        // 4.1 转为utf-8格式
        public static String decode(String unicodeStr) {
            if (unicodeStr == null) {
                return null;
            }
            StringBuilder retBuf = new StringBuilder();
            int maxLoop = unicodeStr.length();
            for (int i = 0; i < maxLoop; i++) {
                if (unicodeStr.charAt(i) == '\\') {
                    if ((i < maxLoop - 5)
                            && ((unicodeStr.charAt(i + 1) == 'u') || (unicodeStr
                            .charAt(i + 1) == 'U')))
                        try {
                            retBuf.append((char) Integer.parseInt(unicodeStr.substring(i + 2, i + 6), 16));
                            i += 5;
                        } catch (NumberFormatException localNumberFormatException) {
                            retBuf.append(unicodeStr.charAt(i));
                        }
                    else {
                        retBuf.append(unicodeStr.charAt(i));
                    }
                } else {
                    retBuf.append(unicodeStr.charAt(i));
                }
            }
            return retBuf.toString();
        }
                         
      ```
   * post请求
      * 提交数据
      * 比如：http://example.com? 用数据封装起来，再传给server
      * 设置post并解析数据（将json映射为对象）
        ```java
            // 5 设置post数据方法
        private String requestDataByPost(String urlString) {
        String result = null;
        try {
            URL url = new URL(urlString);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setConnectTimeout(30000);
            connection.setRequestMethod("POST");

            // 设置运行输入,输出:
            connection.setDoOutput(true);
            connection.setDoInput(true);
            // Post方式不能缓存,需手动设置为false
            connection.setUseCaches(false);
            connection.connect(); //发起连接

            // 请求的数据打包:
            String data = "username=" + URLEncoder.encode("imooc", "UTF-8")
                    + "&number=" + URLEncoder.encode("15088886666", "UTF-8");
            // 获取输出流
            OutputStream out = connection.getOutputStream();
            //将封装好的数据提交
            out.write(data.getBytes());
            out.flush();
            out.close();

            int responseCode = connection.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) {
                InputStream inputStream = connection.getInputStream();
                Reader reader = new InputStreamReader(inputStream, "UTF-8");
                char[] buffer = new char[1024];
                reader.read(buffer);
                result = new String(buffer);
                reader.close();
            } else {
                String responseMessage = connection.getResponseMessage();
                Log.e(TAG, "requestDataByPost: " + responseMessage);
            }

            final String finalResult = result;
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    mTextView.setText(finalResult);
                }
            });

            connection.disconnect();
            return result;
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
        }
        ```
        ```java
            case R.id.parseDataButton:
        // 6. 处理json数据，将数据映射为对象
        if (!TextUtils.isEmpty(mResult)) {
            handleJSONData(mResult);
        }
        break;
        

        // 6. 处理json数据，将数据映射为对象
        private void handleJSONData(String json) {
            try {
                //6.1 一个大括号是一个对象
                JSONObject jsonObject = new JSONObject(json);
                // 6.1.1 读status
                int status = jsonObject.getInt("status");
                //6.1.2 读数组
                JSONArray lessons = jsonObject.getJSONArray("data");
                // 6.2 新建对象和列表  用于存放拿出的数据
                LessonResult lessonResult = new LessonResult();
                List<LessonResult.Lesson> lessonList = new ArrayList<>();
                //循环放数据
                if (lessons != null && lessons.length() > 0) {
                    for (int index = 0; index < lessons.length(); index++) {
                        //6.1.3 拿出数组中每一项数据
                        JSONObject item = (JSONObject) lessons.get(0);
                        int id = item.getInt("id");
                        String name = item.getString("name");
                        String smallPic = item.getString("picSmall");
                        String bigPic = item.getString("picBig");
                        String description = item.getString("description");
                        int learner = item.getInt("learner");
                        //6.2.1 每拿出一个数据，就放到对象中
                        LessonResult.Lesson lesson = new LessonResult.Lesson();
                        lesson.setID(id);
                        lesson.setName(name);
                        lesson.setSmallPictureUrl(smallPic);
                        lesson.setBigPictureUrl(bigPic);
                        lesson.setDescription(description);
                        lesson.setLearnerNumber(learner);
                        lessonList.add(lesson);
                    }
                    //6.2.2 将数组和状态放入对象中
                    lessonResult.setStatus(status);
                    lessonResult.setLessons(lessonList);
                    // 6.2.3 设置显示
                    mTextView.setText("data is : " + lessonResult.toString());
                }

            } catch (JSONException e) {
                e.printStackTrace();
            }
        }

        ```
      
* 9.0 新特性  
   * 创建安全配置文件 
      * 在res文件下创建xml/network_security_config文件
      * 增加cleartextTrafficPermitted属性
    ```java
        <network-security-config>
        <base-config cleartextTrafficPermitted = "true"/> // 允许http请求的加载
        </network-security-config>
    ```    
   * 添加安全配置文件
      * manifast.xml中的Application申明  
    ```java
    <application
    <action android:name="android.intent.action.MAIN"/>
    <category android:name="android.intent.category.LAUNCHER"/>       
    </application>               
    ```
###  <span id = "12"> Handler通信——消息机制</span>
### 一 消息机制概述
#### 1. 简介
   * 主线程=UI线程
   * new Thread创建子线程
   * 子线程不能更新UI线程的东西，需要使用Handler
   * 消息循环机制：进入主线程后开始循环，处理事件
#### 2. Handler作用
   * 定时任务
   * 在不同的线程之间执行动作，在子线程中进行耗时操作，执行完操作后发送消息通知主线程更新UI。
#### 3. 消息机制的模型
   * Message：需要传递的消息，可以传递数据
   * MessageQueue：消息队列，内部是单链表，因为单链表在插入和删除上比较有优势。存储message和runnable，在线程里，向线程池里行投递消息和从线程池里取走消息：Looper将Message从队列中取出，抽给handler，其中的handlerMessage方法对消息进行处理；或者Looper将runable从队列中取出，直接run。
   * Handler：主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)；
   * Looper：不断循环执行(Looper.loop)，从MessageQueue中读取消息，按分发机制将消息分发给目标处理者。

#### 4. 消息机制的架构
1. 运行流程
    * 子线程执行完耗时操作，Handler发送消息，调用MessageQueue.enqueueMessage向消息队列中添加消息
    * 通过Looper.loop开启循环，调用MessageQueue.next从消息队列中读取消息
    * 调用目标Handler（即发送该消息的Handler）的dispatchMessage方法传递消息，并返回到Handler所在线程
    * 目标Handler收到消息，调用handleMessage方法，接收消息，处理消息。
2. MessageQueue，Handler和Looper三者之间的关系
    * 每个线程只有一个Looper，保存在ThreadLocal中
    * 在主线程中不需要再创建Looper，但是在其他线程中需要创建Looper。
    * 每个线程有多个Handler，一个Looper可以处理来自多个Handler的消息。
    * Looper中维护一个MessageQueue，来维护消息队列，消息队列中的Message可以来自不同的Handler。
    * Looper有一个MessageQueue消息队列；MessageQueue有一组待处理的Message；Message中记录发送和处理消息的Handler；Handler中有Looper和MessageQueue。

### 二 消息机制的源码解析
#### Looper
1. 初始化Looper
    * 创建Looper,并保存在ThreadLocal。其中ThreadLocal是线程本地存储区（Thread Local Storage，简称为TLS），每个线程都有自己的私有的本地存储区域，不同线程之间彼此不能访问对方的TLS区域。
2. 开启Looper
    * 不断进行：读取消息队列中的下一条message（next（）），并将message分发给相应的target
    * 当队列中没有消息时，next会无限循环产生阻塞。等待MessageQueue中加入消息，然后重新唤醒。
    ```java
        public static void loop() {
            final Looper me = myLooper();  //获取TLS存储的Looper对象 
            if (me == null) {
                throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
            }
            final MessageQueue queue = me.mQueue;  //获取Looper对象中的消息队列

            Binder.clearCallingIdentity();
            final long ident = Binder.clearCallingIdentity();

            for (;;) { //进入loop的主循环方法
                Message msg = queue.next(); //可能会阻塞,因为next()方法可能会无限循环
                if (msg == null) { //消息为空，则退出循环
                    return;
                }

                Printer logging = me.mLogging;  //默认为null，可通过setMessageLogging()方法来指定输出，用于debug功能
                if (logging != null) {
                    logging.println(">>>>> Dispatching to " + msg.target + " " +
                            msg.callback + ": " + msg.what);
                }
                msg.target.dispatchMessage(msg); //获取msg的目标Handler，然后用于分发Message 
                if (logging != null) {
                    logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
                }

                final long newIdent = Binder.clearCallingIdentity();
                if (ident != newIdent) {
                
                }
                msg.recycleUnchecked(); 
            }
        }
    ```

#### Handler
1. 创建Handler
```java
    public Handler() {
        this(null, false);
    }

    public Handler(Callback callback, boolean async) {
    .................................
        //必须先执行Looper.prepare()，才能获取Looper对象，否则为null.
        mLooper = Looper.myLooper();  //从当前线程的TLS中获取Looper对象
        if (mLooper == null) {
            throw new RuntimeException("");
        }
        mQueue = mLooper.mQueue; //消息队列，来自Looper对象
        mCallback = callback;  //回调方法
        mAsynchronous = async; //设置消息是否为异步处理方式
    }
```

#### 发送消息
1. 在子线程中通过Handler的post、send方式发送消息，最终都是调用了sendMessageAtTime方法；或者调用Runnable对象的run方法
2. sendMessageAtTime方法:就是调用MessageQueue的enqueueMessage()方法，往消息队列中添加一个消息。
3. MessageQueue是按照Message触发时间的先后顺序排列的，消息按时间顺序插入到MessageQueue。队头的消息是将要最早触发的消息。当有消息需要加入消息队列时，会从队列头开始遍历，直到找到消息应该插入的合适位置，以保证所有消息的时间顺序。

#### 获取消息
1. 调用了queue.next()方法,通过该方法来提取下一条信息。
2. nextPollTimeoutMillis代表下一个消息到来前，还需要等待的时长；当nextPollTimeoutMillis = -1时，表示消息队列中无消息，会一直等待下去。
3. next()方法根据消息的触发时间，获取下一条需要执行的消息,队列中消息为空时，则会进行阻塞操作。

#### 分发消息
1. 使用dispatchMessage()方法
2. 当Message的msg.callback不为空时，则回调方法msg.callback.run()，此方法优先级最高
3. 当Handler的mCallback不为空时，则回调方法mCallback.handleMessage(msg)，此方法优先级第二
4. 最后调用Handler自身的回调方法handleMessage()，该方法默认为空，Handler子类通过覆写该方法来完成具体的逻辑，此方法优先级最低
   
### 三 死循环
1. 主线程希望一直运行，死循环可以保证主线程一直运行。循环退出那么app也就退出了。
2. 即使有死循环但是可以通过创建新线程的方法来处理其他的事务，比如ActivityThread类
3. 真正会卡死主线程的操作是在回调方法onCreate/onStart/onResume等操作时间过长，会导致掉帧，甚至发生ANR，looper.loop本身不会导致应用卡死。
4. 在主线程的MessageQueue没有消息时，便阻塞在loop的queue.next()中的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。
###  <span id = "13"> AsyncTask异步任务</span>
### 一、简介 
1. 同步任务：一步一步进行  异步任务：同时进行，互不干扰，多个线程
2. 线程、多线程
   * ANR（Application Not Responding）：应用程序无响应
   * 耗时操作时使用多线程
   * 多线程操作UI事件，其他线程处理后到主线程进行展示
   * 线程具有安全问题
   * 子线程更新主线程的东西使用handler，将要更改的信息传给主线程
3. AsyncTask
   * 目的：在线程池中执行后台任务，然后把执行的进度和最终进度传递给主线程并在主线程并在主线程中更新UI
4. 组成
    * 封装了两个线程池(SerialExecutor和THREAD_POOL_EXECUTOR)和一个Handler(InternalHandler)
    * SerialExecutor线程池用于任务的排队，让需要执行的多个耗时任务，按顺序排列，THREAD_POOL_EXECUTOR线程池才真正地执行任务，InternalHandler用于从工作线程切换到主线程。
5. Handler异步消息处理机制
6. 泛型参数
   * Params ：开始异步任务执行时传入的参数类型；
   * Progress：异步任务执行过程中，返回下载进度值的类型；
   * Result：异步任务执行完成后，返回的结果类型；
7. 核心方法
   * 执行前——onPreExecute()，在主线程执行，用于进行一些界面上的初始化操作，比如显示一个进度条对话框等等
   * 执行中—— doInBackground，子线程中，处理耗时任务，通过return语句将任务的执行结果返回，此方法中不可以进行UI操作
   * 执行后——onPostExecute 主线程，利用参数中的数值就可以对界面元素进行相应的更新
   * 处理进度——onProgressUpdate，返回的数据会作为参数传递到此方法中，利用返回的数据进行操作，在主线程中进行，比如提醒任务执行的结果，关闭进度条对话框等。
   * 调用顺序：
     *  onPreExecute() --> doInBackground() -> onProgressUpdate() --> onPostExecute()
     *  如果不需要执行更新进度则为onPreExecute() --> doInBackground() --> onPostExecute(),
   * onCancelled方法：主线程中执行，当异步任务取消时，会被调用。此时onPostExecute不会被调用。此方法将任务设置为取消状态，需要在doInBackGround判断终止任务。就好比想要终止一个线程，调用interrupt()方法，只是进行标记为中断，需要在线程内部进行标记判断然后中断线程。
8. 简单实用——执行下载
```java
    class DownloadTask extends AsyncTask<Void, Integer, Boolean> {  
        @Override  
        protected void onPreExecute() {  
            progressDialog.show();  
        }  

        @Override  
        protected Boolean doInBackground(Void... params) {  
            try {  
                while (true) {  
                    int downloadPercent = doDownload();  
                    publishProgress(downloadPercent);  
                    if (downloadPercent >= 100) {  
                        break;  
                    }  
                }  
            } catch (Exception e) {  
                return false;  
            }  
            return true;  
        }  

        @Override  
        protected void onProgressUpdate(Integer... values) {  
            progressDialog.setMessage("当前下载进度：" + values[0] + "%");  
        }  

        @Override  
        protected void onPostExecute(Boolean result) {  
            progressDialog.dismiss();  
            if (result) {  
                Toast.makeText(context, "下载成功", Toast.LENGTH_SHORT).show();  
            } else {  
                Toast.makeText(context, "下载失败", Toast.LENGTH_SHORT).show();  
            }  
        }  
    }
```
在doInBackground()方法中去执行具体的下载逻辑，在onProgressUpdate()方法中显示当前的下载进度，在onPostExecute()方法中来提示任务的执行结果。如果要启动任务，执行：
```java
 new DownloadTask().execute();
```
9. 注意事项
 * AsyncTask对象必须在UI线程中创建。
 * execute(Params... params)方法必须在UI线程中调用。
 * 不要手动调用onPreExecute()，doInBackground(Params... params)，onProgressUpdate(Progress... values)，onPostExecute(Result result)这几个方法。
 * 不能在doInBackground(Params... params)中更改UI组件的信息。（在子线程中执行的方法）
 * 一个任务实例只能执行一次，如果执行第二次将会抛出异常。

### 二 源码分析
1. 构造函数
```java
    public AsyncTask() {
            mWorker = new WorkerRunnable<Params, Result>() {
                public Result call() throws Exception {
                    mTaskInvoked.set(true);
                    Result result = null;
                    try {
                        Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND);
                        //noinspection unchecked
                        result = doInBackground(mParams);
                        Binder.flushPendingCommands();
                    } catch (Throwable tr) {
                        mCancelled.set(true);
                        throw tr;
                    } finally {
                        postResult(result);
                    }
                    return result;
                }
            };

            mFuture = new FutureTask<Result>(mWorker) {
                @Override
                protected void done() {
                    try {
                        postResultIfNotInvoked(get());
                    } catch (InterruptedException e) {
                        android.util.Log.w(LOG_TAG, e);
                    } catch (ExecutionException e) {
                        throw new RuntimeException("An error occurred while executing doInBackground()",
                                e.getCause());
                    } catch (CancellationException e) {
                        postResultIfNotInvoked(null);
                    }
                }
            };
        }
```
* 初始化mWorker和mFuture变量，并在初始化mFuture的时候将mWorker作为参数传入。
* mWorker是一个Callable对象，mFuture是一个FutureTask对象，这两个变量会暂时保存在内存中，稍后才会用到它们。 FutureTask实现了Runnable接口。
* mWorker中的call()方法执行了耗时操作，即result = doInBackground(mParams);,然后把执行得到的结果通过postResult(result);,传递给内部的Handler跳转到主线程中。

2. exec方法
* 调用了SerialExecutor 类的execute方法。
* SerialExecutor 内部维持了一个队列，通过锁使得该队列保证AsyncTask中的任务是串行执行的，即多个任务需要一个个加到该队列中，然后执行完队列头部的再执行下一个，以此类推。
* 在这个方法中，有两个主要步骤。 
  * 向队列中加入一个新的任务，即之前实例化后的mFuture对象。
  * 调用 scheduleNext()方法，调用THREAD_POOL_EXECUTOR执行队列头部的任务。
  * 可见SerialExecutor 类仅仅为了保持任务执行是串行的，实际执行交给了THREAD_POOL_EXECUTOR。
  * InternalHandler是一个静态类，为了能够将执行环境切换到主线程，因此这个类必须在主线程中进行加载。所以变相要求AsyncTask的类必须在主线程中进行加载。
  * ，默认情况下AsyncTask的执行效果是串行的，因为有了SerialExecutor类来维持保证队列的串行。如果想使用并行执行任务，那么可以直接跳过SerialExecutor类，使用executeOnExecutor()来执行任务。 

### 三 使用不当的后果
1. 生命周期：AsyncTask不与任何组件绑定生命周期，所以在Activity/或者Fragment中创建执行AsyncTask时，最好在Activity/Fragment的onDestory()调用 cancel(boolean)；
2. 内存泄露：如果AsyncTask被声明为Activity的非静态的内部类，那么AsyncTask会保留一个对创建了AsyncTask的Activity的引用。如果Activity已经被销毁，AsyncTask的后台线程还在执行，它将继续在内存里保留这个引用，导致Activity无法被回收，引起内存泄露。
3. 结果丢失：屏幕旋转或Activity在后台被系统杀掉等情况会导致Activity的重新创建，之前运行的AsyncTask（非静态的内部类）会持有一个之前Activity的引用，这个引用已经无效，这时调用onPostExecute()再去更新界面将不再生效。
## <span id = "14">高级控件</span>
###  <span id = "15"> 动画总结</span>
#### 一 属性动画
对于对象属性的动画
#### 属性动画原理
1. 使用了ObjectAnimator类，继承自ValueAnimator，可以对任意对象的任意属性进行动画操作
2.  只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的时长，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放次数、播放模式、以及对动画设置监听器等。
3. TypeEvaluator 决定了动画如何从初始值过渡到结束值
4. TimeInterpolator 决定了动画从初始值过渡到结束值的节奏

#### 属性动画自定义实现
举例：小球运动
1. 用TypeEvaluator 确定运动轨迹
```java
    public class PointSinEvaluator implements TypeEvaluator {

        @Override
        public Object evaluate(float fraction, Object startValue, Object endValue) {
            Point startPoint = (Point) startValue;
            Point endPoint = (Point) endValue;
            float x = startPoint.getX() + fraction * (endPoint.getX() - startPoint.getX());

            float y = (float) (Math.sin(x * Math.PI / 180) * 100) + endPoint.getY() / 2;
            Point point = new Point(x, y);
            return point;
        }
    }
```
继承该接口，实现evaluate方法，三个参数分别为：当前动画完成的百分比，动画的初始值，动画的结束值。根据最后的xy值产生一个新的点（point对象）返回。
使用这个PointSinEvaluator 生成属性动画的实例：
```java
 Point startP = new Point(RADIUS, RADIUS);//初始值（起点）
        Point endP = new Point(getWidth() - RADIUS, getHeight() - RADIUS);//结束值（终点）
        final ValueAnimator valueAnimator = ValueAnimator.ofObject(new PointSinEvaluator(), startP, endP);
        valueAnimator.setRepeatCount(-1);
        valueAnimator.setRepeatMode(ValueAnimator.REVERSE);
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                currentPoint = (Point) animation.getAnimatedValue();
                postInvalidate();
            }
        });
```
完成了动画轨迹的定义，调用valueAnimator.start() 方法，就会绘制出一个正弦曲线的轨迹。

2. 颜色及半径动画实现
首先需要定义属性，比如颜色和半径，并实现各自的set和get方法。
```java
// 使用ObjectAnimator 实现对color 属性的值按照ArgbEvaluator 这个类的规律在给定的颜色值之间变化，这个ArgbEvaluator 和我们之前定义的PointSinEvaluator一样，都是决定动画如何从初始值过渡到结束值的，只不过这个类是系统自带的，我们直接拿来用就可以，他可以实现各种颜色间的自由过渡。
 ObjectAnimator animColor = ObjectAnimator.ofObject(this, "color", new ArgbEvaluator(), Color.GREEN,
                Color.YELLOW, Color.BLUE, Color.WHITE, Color.RED);
        animColor.setRepeatCount(-1);
        animColor.setRepeatMode(ValueAnimator.REVERSE);

// 实现半径动态的变化，添加动画变化的监听器，在属性值更新的时候，将变化的结果赋值，实现半径动态的变化
// 这里radius 也可以使用和color相同的方式，只需要把ArgbEvaluator 替换为FloatEvaluator，同时修改动画的变化值即可；使用添加监听器的方式，只是为了介绍监听器的使用方法而已
        ValueAnimator animScale = ValueAnimator.ofFloat(20f, 80f, 60f, 10f, 35f,55f,10f);
        animScale.setRepeatCount(-1);
        animScale.setRepeatMode(ValueAnimator.REVERSE);
        animScale.setDuration(5000);
        animScale.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                radius = (float) animation.getAnimatedValue();
            }
        });
```
组合使用属性动画， 将三个动画同时播放
```java
 animSet = new AnimatorSet();
        animSet.play(valueAnimator).with(animColor).with(animScale);
        animSet.setDuration(5000);
        animSet.setInterpolator(interpolatorType);
        animSet.start();
```

3. TimeInterPolator介绍
控制动画变化的节奏，通过setInterpolator 设置不同的Interpolator
###  <span id = "16"> 进程优先级</span>
当系统内存不足时，安卓系统将根据进程的优先级选择杀死一些不太重要的进程，优先级低的先杀死。
进程优先级从高到低：前台进程、可视进程、服务进程、后台进程、空进程
#### 前台进程
* 处于正在与用户交互的activity
* 与前台activity绑定的service
* 调用了startForeground方法的service
* 正在执行oncreate（），onstart（），ondestroy方法的 service
* 进程中包含正在执行onReceive（）方法的BroadcastReceiver
系统中的前台进程并不会很多，而且一般前台进程都不会因为内存不足被杀死。特殊情况除外。当内存低到无法保证所有的前台进程同时运行时，才会选择杀死某个进程。

#### 可视进程
* 为处于前台，但仍然可见的activity（例如：调用了onpause（）而还没调用onstop（）的activity）。典型情况是：运行activity时，弹出对话框（dialog等），此时的activity虽然不是前台activity，但是仍然可见
* 可见activity绑定的service。（处于上诉情况下的activity所绑定的service）

#### 服务进程
* 已经启动的service

#### 后台进程
* 不可见的activity（调用onstop（）之后的activity）
后台进程不会影响用户的体验，为了保证前台进程，可视进程，服务进程的运行，系统随时有可能杀死一个后台进程。当一个正确实现了生命周期的activity处于后台被杀死时，如果用户重新启动，会恢复之前的运行状态。

#### 空进程
* 任何没有活动的进程
空进程的存在无非为了一些缓存，以便于下次可以更快的启动。

#### 二 传统动画和属性动画
1. 属性动画实现了view的真正移动，补间动画对view的移动类似在不同的地方绘制了一个影子，实际的对象还是处于原来的地方
2. 当我们把动画的repeatCount设置为无限循环时，如果在Activity退出时没有及时将动画停止，属性动画会导致Activity无法释放而导致内存泄漏，而补间动画却没有问题。因此，使用属性动画时切记在Activity执行 onStop 方法时顺便将动画停止。

###  <span id = "17"> 屏幕适配</span>
1. 屏幕尺寸：屏幕对角线的长度，单位是影讯
2. 屏幕分辨率：单位是px
3. 屏幕像素密度：每英寸上的像素点数，单位是dpi。相同尺寸，分辨率越高，像素密度越高（屏幕越清晰）
4. px：像素点  sp：字体大小 dip、dp：定义尺寸
5. idpi：mdpi：中密度 hdpi：高密度 xhdpi-xxhdpi-xxxhdpi：密度依次升高
6. 屏幕适配：禁用绝对布局、少用px、使用wrap_content,match_content,layout_weight、重建布局
7. 图片适配：使用自动拉审图、提供不同分辨率的备用位图
## <span id = "18">数据存储</span>
###  <span id = "19">本地文件操作</span>
1. SharedPreferences基本概念
   * 本质是一个xml文件，通过键值对的方式存放信息
   * 用于存放一些类似登录的配置信息
2. 操作模式
   * MODE_APPEND：追加方式存储
   * MODE_PRIVATE：私有方式存储
   * MODE_WORLD_READABLE：可被其他应用读取
   * MODE_WORLD_WRITEABLE：可被其他应用写入
   ```java
        //3. SharePreference的读取（进入界面后有之前输入的数据）
        //①获取SharePreference对象(参数1：文件名  参数2：模式)
        SharedPreferences share = getSharedPreferences("myshare",MODE_PRIVATE);
        //②根据key获取内容(参数1：key   参数2：当对应key不存在时，返回参数2的内容作为默认值)
        String accStr = share.getString("account","");
        String pwdStr = share.getString("pwd","");

        accEdt.setText(accStr);
        pwdEdt.setText(pwdStr);

        findViewById(R.id.login_btn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //1.获取两个输入框的内容
                String account = accEdt.getText().toString();
                String pwd = pwdEdt.getText().toString();
                //2.验证(admin  123)
                    if(account.equals("admin") && pwd.equals("123")){
                        //2.1 信息正确则存储信息到SharePreference
                        //①获取SharePreference对象(参数1：文件名  参数2：模式)
                        SharedPreferences share = getSharedPreferences("myshare",MODE_PRIVATE);
                        //②获取Editor对象
                        SharedPreferences.Editor edt = share.edit();
                        //③存储信息   键值对的方式存储信息
                        edt.putString("account",account);
                        edt.putString("pwd",pwd);
                        //④指定提交操作
                        edt.commit();

                        Toast.makeText(ShareActivity.this,"登录成功",Toast.LENGTH_SHORT).show();
                    }else {
                        //2.2验证失败，提示用户
                        Toast.makeText(ShareActivity.this,"账号或密码错误",Toast.LENGTH_SHORT).show();
                    }
            }
        });
   ```
3. 外部存储ExternalStorage
   * 写入并存入文件夹进行读取
   ```java
   public class ExternalActivity extends AppCompatActivity {

    EditText infoEdt;
    TextView txt;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_external);

        infoEdt = findViewById(R.id.info_edt);
        txt = findViewById(R.id.textView);

        // 6. 动态权限请求
        int permisson = ContextCompat.checkSelfPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE);
        //如果没有这个权限就申请权限
        if(permisson!=PackageManager.PERMISSION_GRANTED){
            //动态去申请权限
            ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},1);
        }

    }

    //7. 申请码的处理  在申请的过程中要处理的事
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if(requestCode == 1){
            //xxxxxxxxxxxxx
        }
    }

    public void operate(View v){
        // 1. 获取位置  得到根目录的绝对路径并创建目录文件
        String path = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Download/imooc.txt";
        Log.e("TAG",path);
        //判断内存卡是否存在
        //if(Environment.getExternalStorageState().equals("mounted"))
        switch (v.getId()){
            case R.id.save_btn:
                // 2. 创建文件
                File f = new File(path);
                try {
                    if (!f.exists()) {
                        f.createNewFile();
                    }

                    FileOutputStream fos = new FileOutputStream(path,true);
                    String str = infoEdt.getText().toString();
                    // 3. 将输入框内容写入文件
                    // 4. 创建权限
                    fos.write(str.getBytes());
                }catch (IOException ioe){
                    ioe.printStackTrace();
                }
                break;
            case R.id.read_btn:
                try {
                    // 5. 读取文件内容并显示
                    FileInputStream fis = new FileInputStream(path);
                    byte[] b = new byte[1024];
                    int len = fis.read(b);
                    String str2 = new String(b,0,len);
                    txt.setText(str2);
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                break;
        }
    }
   ```
   * 私有目录 卸载文件时数据也会清除掉
      * Context.getExternalFilesDir(String type)：删除时清除数据
      * Context.getExternalCacheDir()：删除时清除缓存
4. 内部存储——data文件夹
      * Context.getFilesDir(String type)：删除时清除数据
      * Context.getCacheDir()：删除时清除缓存
      ```java
        public void operate(View v){

        //  data/data/包名/files
        //   getCacheDir()    data/data/包名/cache
        File f = new File(getFilesDir(),"getFilesDirs.txt");
        switch (v.getId()){
            case R.id.save_btn:
                try {
                    if (!f.exists()) {
                        f.createNewFile();
                    }
                    FileOutputStream fos = new FileOutputStream(f);
                    fos.write(edt.getText().toString().getBytes());
                    fos.close();
                }catch (IOException e){
                    e.printStackTrace();
                }
                break;
            case R.id.read_btn:
                try {
                    FileInputStream fis = new FileInputStream(f);
                    byte[] b = new byte[1024];
                    int len = fis.read(b);
                    String str2 = new String(b,0,len);
                    txt.setText(str2);
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                break;
        }
      ```  
###  <span id = "20">SQLite数据库操作</span>
1. 开源的关系型数据库、轻量级
2. 以行和列的形式存储数据，以便于理解。
3. 本质是一个二进制文件，由核心部分、编译器、后端和附件合作。
4. 数据库操作
   * 查询：产生的是一个虚拟的结果集，并不是把数据取出来传给客户端  select * from 表名
   * 添加
   * 删除
   * 修改
5. 使用sql语句操作数据
```java
public class MainActivity extends Activity {

    private EditText nameEdt , ageEdt , idEdt;
    private RadioGroup genderGp;
    private ListView stuList;
    private RadioButton malerb;
    private String genderStr = "男";
    private SQLiteDatabase db;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //添加操作
        //1. 数据库名称，创建数据库
        //如果只有一个数据库名称，那么这个数据库的位置会是在私有目录中
        //如果带SD卡路径，那么数据库位置则在指定的路径下
        String path = Environment.getExternalStorageDirectory() + "/stu.db";
        //2. SQLiteOpenHelper  用于创建或打开数据库并对数据库的创建和版本进行管理
        SQLiteOpenHelper helper = new SQLiteOpenHelper(this,path,null,2) {
            @Override
            public void onCreate(SQLiteDatabase sqLiteDatabase) {
                //创建
                Toast.makeText(MainActivity.this,"数据库创建",Toast.LENGTH_SHORT).show();
                //如果数据库不存在，则会调用onCreate方法，那么我们可以将表的创建工作放在这里面完成
                        /*
                        String sql = "create table test_tb (_id integer primary key autoincrement," +
                                "name varhcar(20)," +
                                "age integer)";
                        sqLiteDatabase.execSQL(sql);*/
            }

            @Override
            public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
                //升级
                Toast.makeText(MainActivity.this,"数据库升级",Toast.LENGTH_SHORT).show();
            }
        };

        //3. 获取数据库库对象，返回值是SQLiteDatabase 对象
        //4. SQLiteDatabase  数据库对象类，用来管理和操作数据库，所有的操作都由这个类的对象完成
         //       db.rawQuery();        查询    select * from 表名
          //     db.execSQL();         添加、删除、修改、创建表
        //3.1 数据库存在，则直接打开数据库
        //3.2 数据库不存在，则调用创建数据库的方法，再打开数据库
        //3.3 数据库存在，但版本号升高了，则调用数据库升级方法
        db = helper.getReadableDatabase();

        // 5 获取控件 并为单选按钮设置点击事件
        nameEdt = (EditText) findViewById(R.id.name_edt);
        ageEdt = (EditText) findViewById(R.id.age_edt);
        idEdt = (EditText) findViewById(R.id.id_edt);
        malerb = (RadioButton) findViewById(R.id.male);

        genderGp = (RadioGroup) findViewById(R.id.gender_gp);
        genderGp.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                if(i == R.id.male){
                    //“男”
                    genderStr = "男";
                }else{
                    //"女"
                    genderStr = "女";
                }
            }
        });

        stuList = (ListView) findViewById(R.id.stu_list);
    }



    // 6. 绑定点击操作
    public void operate(View v){

        String nameStr = nameEdt.getText().toString();
        String ageStr = ageEdt.getText().toString();
        String idStr = idEdt.getText().toString();
        switch (v.getId()){
            //添加
            case R.id.insert_btn:
                //6.1 添加数据到数据库中
                //String sql = "insert into info_tb (name,age,gender) values ('"+nameStr+"',"+ageStr+",'"+genderStr+"')";
                //db.execSQL(sql);
                // 6.2 第二种方式添加数据  单选和年龄通过文本框获得，年龄通过监听点击事件获得
                String sql = "insert into info_tb (name,age,gender) values (?,?,?)";
                db.execSQL(sql,new String[]{nameStr,ageStr,genderStr});
                Toast.makeText(this,"添加成功",Toast.LENGTH_SHORT).show();
                break;



                //查询
            case R.id.select_btn:
                //SQLiteOpenHelper
                //select * from 表名 where _id = ?
                // 6.3 查询所有信息的sql语句
                String sql2 = "select * from info_tb";
                //根据id查询
                if(!idStr.equals("")){
                    sql2 += " where _id=" + idStr;
                }
                //6.4 查询结果并保存到cursor对象中，类似一张表
                Cursor c = db.rawQuery(sql2,null);

                // 6.5 将结果展示在listview中，需要创建适配器
                //SimpleCursorAdapter  游标适配器
                //SimpleAdapter a = new SimpleAdapter()
                //参数3：数据源
                SimpleCursorAdapter adapter = new SimpleCursorAdapter(
                        this, R.layout.item,c,
                        //注意要写 _id
                        new String[]{"_id","name","age","gender"},
                        new int[]{R.id.id_item,R.id.name_item,R.id.age_item,R.id.gender_item});
                stuList.setAdapter(adapter);
                break;



                //删除
            case R.id.delete_btn:
                //' '   "23"   23
                //6.6 删除的sql语句
                String sql3 = "delete from info_tb where _id=?";
                db.execSQL(sql3,new String[]{idStr});
                Toast.makeText(this,"删除成功",Toast.LENGTH_SHORT).show();
                break;


                //修改
            case R.id.update_btn:
                // 6.6 修改的sql语句
                String sql4 = "update info_tb set name=? , age=? , gender=?  where _id=?";
                db.execSQL(sql4,new String[]{nameStr,ageStr,genderStr,idStr});
                Toast.makeText(this,"修改成功",Toast.LENGTH_SHORT).show();
                break;
        }
        // 6.7 操作后清空输入栏
        nameEdt.setText("");
        ageEdt.setText("");
        idEdt.setText("");
        malerb.setChecked(true);
    }
}
```
6. 使用Api操作数据
```java
public class MainActivity2 extends Activity {

    private EditText nameEdt , ageEdt , idEdt;
    private RadioGroup genderGp;
    private ListView stuList;
    private RadioButton malerb;
    private String genderStr = "男";
    private SQLiteDatabase db;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //添加操作
        //数据库名称
        //如果只有一个数据库名称，那么这个数据库的位置会是在私有目录中
        //如果带SD卡路径，那么数据库位置则在指定的路径下
        String path = Environment.getExternalStorageDirectory() + "/stu.db";
        SQLiteOpenHelper helper = new SQLiteOpenHelper(this,path,null,2) {
            @Override
            public void onCreate(SQLiteDatabase sqLiteDatabase) {
                //创建
                Toast.makeText(MainActivity2.this,"数据库创建",Toast.LENGTH_SHORT).show();
                //如果数据库不存在，则会调用onCreate方法，那么我们可以将表的创建工作放在这里面完成
                        /*
                        String sql = "create table test_tb (_id integer primary key autoincrement," +
                                "name varhcar(20)," +
                                "age integer)";
                        sqLiteDatabase.execSQL(sql);*/
            }

            @Override
            public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
                //升级
                Toast.makeText(MainActivity2.this,"数据库升级",Toast.LENGTH_SHORT).show();
            }
        };

        //用于获取数据库库对象
        //1.数据库存在，则直接打开数据库
        //2.数据库不存在，则调用创建数据库的方法，再打开数据库
        //3.数据库存在，但版本号升高了，则调用数据库升级方法
        db = helper.getReadableDatabase();

        nameEdt = (EditText) findViewById(R.id.name_edt);
        ageEdt = (EditText) findViewById(R.id.age_edt);
        idEdt = (EditText) findViewById(R.id.id_edt);
        malerb = (RadioButton) findViewById(R.id.male);

        genderGp = (RadioGroup) findViewById(R.id.gender_gp);
        genderGp.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                if(i == R.id.male){
                    //“男”
                    genderStr = "男";
                }else{
                    //"女"
                    genderStr = "女";
                }
            }
        });

        stuList = (ListView) findViewById(R.id.stu_list);
    }

//    SQLiteOpenHelper
//    SQLiteDatabase
    public void operate(View v){

        String nameStr = nameEdt.getText().toString();
        String ageStr = ageEdt.getText().toString();
        String idStr = idEdt.getText().toString();
        switch (v.getId()){
            case R.id.insert_btn:
                //在SqliteDatabase类下，提供四个方法
                //insert（添加）、delete（删除）、update（修改）、query（查询）
                //都不需要写sql语句
                //参数1：你所要操作的数据库表的名称
                //参数2：可以为空的列.  如果第三个参数是null或者说里面没有数据
                //那么我们的sql语句就会变为insert into info_tb () values ()  ，在语法上就是错误的
                //此时通过参数3指定一个可以为空的列，语句就变成了insert into info_tb (可空列) values （null）
                ContentValues values = new ContentValues();
                //insert into 表明(列1，列2) values（值1，值2）
                values.put("name",nameStr);
                values.put("age",ageStr);
                values.put("gender",genderStr);
                long id = db.insert("info_tb",null,values);
                Toast.makeText(this,"添加成功，新学员学号是：" + id,Toast.LENGTH_SHORT).show();
                break;
            case R.id.select_btn:
                //select 列名 from 表名 where 列1 = 值1 and 列2 = 值2
                //参数2：你所要查询的列。{”name","age","gender"},查询所有传入null/{“*”}
                //参数3：条件（针对列）
                //参数5:分组
                //参数6：当 group by对数据进行分组后，可以通过having来去除不符合条件的组
                //参数7:排序
                Cursor c = db.query("info_tb",null,null,null,null,null,null);

                //SimpleCursorAdapter
                //SimpleAdapter a = new SimpleAdapter()
                //参数3：数据源
                SimpleCursorAdapter adapter = new SimpleCursorAdapter(
                        this, R.layout.item,c,
                        new String[]{"_id","name","age","gender"},
                        new int[]{R.id.id_item,R.id.name_item,R.id.age_item,R.id.gender_item});
                stuList.setAdapter(adapter);
                break;
            case R.id.delete_btn:
                int count = db.delete("info_tb","_id=?",new String[]{idStr});
                if(count > 0) {
                    Toast.makeText(this, "删除成功", Toast.LENGTH_SHORT).show();
                }
                break;
            case R.id.update_btn:
                ContentValues values2 = new ContentValues();
                //update info_tb set 列1=xx , 列2=xxx where 列名 = 值
                values2.put("name",nameStr);
                values2.put("age",ageStr);
                values2.put("gender",genderStr);
                int count2 = db.update("info_tb",values2,"_id=?",new String[]{idStr});
                if(count2 > 0) {
                    Toast.makeText(this, "修改成功", Toast.LENGTH_SHORT).show();
                }
                break;
        }
        nameEdt.setText("");
        ageEdt.setText("");
        idEdt.setText("");
        malerb.setChecked(true);
    }
```
7. SQLite操作数据的两套方法：
   * rawQuery（） 查询  execSql（）——需要些sql语句
   * insert delete update query 不用写语句，根据参数操作数据库
8. 数据库操作封装
app1
```java
public class MainActivity3 extends Activity {

    private EditText nameEdt , ageEdt , idEdt;
    private RadioGroup genderGp;
    private ListView stuList;
    private RadioButton malerb;
    private String genderStr = "男";
    private StudentDao dao;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        dao = new StudentDao(this);

        nameEdt = (EditText) findViewById(R.id.name_edt);
        ageEdt = (EditText) findViewById(R.id.age_edt);
        idEdt = (EditText) findViewById(R.id.id_edt);
        malerb = (RadioButton) findViewById(R.id.male);

        genderGp = (RadioGroup) findViewById(R.id.gender_gp);
        genderGp.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                if(i == R.id.male){
                    //“男”
                    genderStr = "男";
                }else{
                    //"女"
                    genderStr = "女";
                }
            }
        });

        stuList = (ListView) findViewById(R.id.stu_list);
    }

    public void operate(View v){

        String nameStr = nameEdt.getText().toString();
        String ageStr = ageEdt.getText().toString();
        String idStr = idEdt.getText().toString();
        switch (v.getId()){
            case R.id.insert_btn:
                Student stu = new Student(nameStr,Integer.parseInt(ageStr),genderStr);
                dao.addStudent(stu);
                Toast.makeText(this,"添加成功",Toast.LENGTH_SHORT).show();
                break;
            case R.id.select_btn:
                String key="",value="";
                if(!nameStr.equals("")){
                    value = nameStr;
                    key = "name";
                }else if(!ageStr.equals("")){
                    value = ageStr;
                    key = "age";
                }else if(!idStr.equals("")){
                    value = idStr;
                    key = "_id";
                }
                Cursor c;
                if(key.equals("")){
                   c = dao.getStudent();
                }else {
                   c = dao.getStudent(key, value);
                }

                SimpleCursorAdapter adapter = new SimpleCursorAdapter(
                        this, R.layout.item,c,
                        new String[]{"_id","name","age","gender"},
                        new int[]{R.id.id_item,R.id.name_item,R.id.age_item,R.id.gender_item});
                stuList.setAdapter(adapter); /**/
                break;
            case R.id.delete_btn:

                String[] params = getParams(nameStr,ageStr,idStr);

                dao.deleteStudent(params[0],params[1]);
                Toast.makeText(this,"删除成功",Toast.LENGTH_SHORT).show();
                break;
            case R.id.update_btn:
                Student stu2 = new Student(Integer.parseInt(idStr),nameStr,Integer.parseInt(ageStr),genderStr);
                dao.updateStudent(stu2);
                Toast.makeText(this,"修改成功",Toast.LENGTH_SHORT).show();
            break;
        }
        nameEdt.setText("");
        ageEdt.setText("");
        idEdt.setText("");
        malerb.setChecked(true);
    }

    public String[] getParams(String nameStr,String ageStr,String idStr){
        String[] params = new String[2];
        if(!nameStr.equals("")){
            params[1] = nameStr;
            params[0] = "name";
        }else if(!ageStr.equals("")){
            params[1] = ageStr;
            params[0] = "age";
        }else if(!idStr.equals("")){
            params[1] = idStr;
            params[0] = "_id";
        }
        return params;
    }
```

```java
public class Student{
        //私有属性
        private int id;
        private String name;
        private int age;
        private String gender;
        //无参构造
        public Student(){

        }

        public Student(String name, int age, String gender) {
            this.name = name;
            this.age = age;
            this.gender = gender;
        }

    //有参构造
        public Student(int id, String name, int age, String gender) {
            super();
            this.id = id;
            this.name = name;
            this.age = age;
            this.gender = gender;
        }
        //创建的setter和getter方法
        public int getId() {
            return id;
        }
        public void setId(int id) {
            this.id = id;
        }
        public String getName() {
            return name;
        }
        public void setName(String name) {
            this.name = name;
        }
        public int getAge() {
            return age;
        }
        public void setAge(int age) {
            this.age = age;
        }
        public String getGender() {
            return gender;
        }
        public void setGender(String gender) {
            this.gender = gender;
        }


}


```

```java
public class StudentDao {
    private SQLiteDatabase db;

    public StudentDao(Context context){
        String path = Environment.getExternalStorageDirectory() + "/stu.db";
        SQLiteOpenHelper helper = new SQLiteOpenHelper(context,path,null,2) {
            @Override
            public void onCreate(SQLiteDatabase sqLiteDatabase) {
            }

            @Override
            public void onUpgrade(SQLiteDatabase sqLiteDatabase, int i, int i1) {
            }
        };
        db = helper.getReadableDatabase();
    }

    public void addStudent(Student stu){
        String sql = "insert into info_tb (name,age,gender) values(?,?,?)";
        db.execSQL(sql,new Object[]{stu.getName(),stu.getAge()+"",stu.getGender()});
    }

    public Cursor getStudent(String... strs){
        //1.查询所有(没有参数)
        String sql = "select * from info_tb ";
        //2.含条件查询（姓名/年龄/编号）（参数形式：第一个参数指明条件，第二个参数指明条件值）
        if(strs.length != 0){
            sql += " where " + strs[0] + "='" + strs[1] + "'";
        }
        Cursor c = db.rawQuery(sql,null);
        return c;
    }

    public ArrayList<Student> getStudentInList(String... strs){
        ArrayList<Student> list = new ArrayList<>();
        Cursor c = getStudent(strs);
        while (c.moveToNext()){
            int id = c.getInt(0);
            String name = c.getString(1);
            int age = c.getInt(2);
            String gender = c.getString(3);
            Student s = new Student(id,name,age,gender);
            list.add(s);
        }
        return list;
    }

    public void deleteStudent(String... strs){
        String sql  = "delete from info_tb where " + strs[0] + "='" + strs[1] + "'";
        db.execSQL(sql);
    }

    public void updateStudent(Student stu){
        String sql = "update info_tb set name=?,age=?,gender=? where _id=?";
        db.execSQL(sql,new Object[]{stu.getName(),stu.getAge(),stu.getGender(),stu.getId()});
    }

```
###  <span id = "21">Context详解</span>
#### 一 Context定义
1. 指上下文环境，理解为当前对象在程序中所处的一个环境，一个与系统交互的过程。
2. Context在加载资源、启动Activity、获取系统服务、创建View等操作都要参与
3. Context的具体实现子类就是：Activity，Service，Application
4. Context数量=Activity数量+Service数量+1

#### 二 Context作用及作用域
1. Broadcast Receiver，Content Provider并不是Context的子类，他们所持有的Context都是其他地方传过去的。弹出Toast、启动Activity、启动Service、发送广播、操作数据库等等都需要用到Context
2. 弹出dialog是需要在一个Activity上弹出，因此这种情况只能使用Activity类型的Context，不能使用Application和Service类型的
3. layout inflate：在Application和Service中也是合法的，但是会使用系统默认的主题样式，如果你自定义了某些样式可能不会被使用
4. start an Activity：非Activity类型的Context没有所谓的任务栈，待启动的Activity找不到栈。如果指定标记位那么会在启动的时候创建一个新的任务栈，这样不推荐。
5. 总结：与UI相关的，应该使用Activity做为Context来处理（否则是默认样式）；其他的一些操作，Service,Activity,Application等实例都可以

#### 三 获取Context
1. View.getContext()：获取当前view对象的Context对象，即当前正在展示的Activity对象
2. Activity.getApplicationContext()：获取当前Activity所在的应用进程的Context对象
3. ContextWrapper.getBaseContext()
4. Activity.this 返回当前的Activity实例，如果是UI控件需要使用Activity作为Context对象，但是默认的Toast实际上使用ApplicationContext也可以。
##### getApplicaion和getApplicationContext
1. 打印出的内存地址是相同的，是同一个对象，因为Application本身就是一个Context
2. 区别：getApplication（）只有在Activity和Service中才能调用到，如果像BroadCastReceiver中也想获得Application的实例，就需要使用ApplicationContext。

#### 四 Context引起的内存泄露
##### 错误的单例模式
```java
    public class Singleton {
        private static Singleton instance;
        private Context mContext;

        private Singleton(Context context) {
            this.mContext = context;
        }

        public static Singleton getInstance(Context context) {
            if (instance == null) {
                instance = new Singleton(context);
            }
            return instance;
        }
    }
```
instance作为静态对象生命周期比普通的对象包括Activity都要长，Activity如果getInstance获得该对象，那么这个单例就会持有传入的Activity对象，当Activiy被销毁掉，但是他的引用还存在与一个单例中，不可能被gc掉，会导致内存泄露。

##### view持有Activity引用
```java
    public class MainActivity extends Activity {
        private static Drawable mDrawable;

        @Override
        protected void onCreate(Bundle saveInstanceState) {
            super.onCreate(saveInstanceState);
            setContentView(R.layout.activity_main);
            ImageView iv = new ImageView(this);
            mDrawable = getResources().getDrawable(R.drawable.ic_launcher);
            iv.setImageDrawable(mDrawable);
        }
    }
```
静态的drawable对象被设置入imageview中，而new ImageView时传入的this是Activity的Context，相当于这个activity是静态变量drawable的间接引用，当activity被销毁时也不能被gc掉。

##### 正确使用Context
1. 当Application的Context能搞定的情况下，并且生命周期长的对象，优先使用Application的Context。因为Application的Context是随着进程存在的
2. 不要让生命周期长于Activity的对象持有到Activity的引用
3. 尽量不要在Activity中使用非静态内部类，因为非静态内部类会隐式持有外部类实例的引用，如果使用静态内部类，将外部实例引用作为弱引用持有
###  <span id = "22">BroadcastReceiver 广播接收器</span>
### 一、定义
1. 四大组件之一，属于一个全局的监听器
2. 分为：广播发送者、广播接收者

### 二、作用
1. 用于监听 / 接收 应用发出的广播消息，并做出响应
2. 应用场景
* 不同组件之间通信（包括应用内 / 不同应用之间）
* 与 Android 系统在特定情况下的通信——如当电话呼入时、网络可用时
* 多线程通信

### 三、实现原理
1. 观察者模式：基于消息的发布/订阅事件模型。Android将广播的发送者 和 接收者 解耦，使得系统方便集成
2. 三个角色：
 * 消息订阅者——广播接收
 * 消息发布者——广播发布
 * 消息中心（AMS，即Activity Manager Service）
3. 原理描述：
   * 接收者在AMS注册（Binder机制）
      * 静态注册 Manifest.xml中
      * 动态注册 运行期间注册
   * 发布者通过Binder机制向AMS发送广播
   * AMS 根据 广播发送者 要求，在已注册列表中，寻找合适的广播接收者
     * 寻找依据：IntentFilter / Permission
   * AMS将广播发送到合适的广播接收者相应的消息循环队列中
   * 广播接收者通过 消息循环 拿到此广播，并回调 onReceive()
#### 广播发送者 和 广播接收者的执行 是 异步的，发出去的广播不会关心有无接收者接收，也不确定接收者到底是何时才能接收到；

### 四、具体使用
1. 自定义广播接收者BroadcastReceiver
2. 注册
   ```java
   // 1. 自定义广播接收者BroadcastReceiver
    public class ImoocBroadcastReceiver extends BroadcastReceiver {
        @Override
        //1.1 接收广播时回调的方法
        // 广播接收器接收到相应广播后，会自动回调onReceive()方法
        // 一般情况下，onReceive方法会涉及与其他组件之间的交互，如发送Notification、启动service等
         //默认情况下，广播接收器运行在UI线程，因此，onReceive方法不能执行耗时操作，否则将导致ANR。
        public void onReceive(Context context, Intent intent) {
            // 接收广播
            if(intent != null){
                // 接收到的是什么广播
                String action  = intent.getAction();
                Log.d(TAG, "onReceive: " + action);

                // 判断是什么广播（是不是我们自己发送的自定义广播）
                if(TextUtils.equals(action, MainActivity.MY_ACTION)){
                    // 获取广播携带的内容， 可自定义的数据
                    String content = intent.getStringExtra(MainActivity.BROADCAST_CONTENT);
                    if(mTextView != null){
                        mTextView.setText("接收到的action是："+ action + "\n接收到的内容是：\n" + content);
                    }
                }
            }
   
   ```
   ```java
     // 1. 创建广播
        //2. 注册静态广播
        //在AndroidManifest.xml里通过 <receive> 标签声明
        <!-- 静态注册广播接收器 -->
        <receiver
            android:name=".ImoocBroadcastReceiver">

            <!-- 接收哪些广播 -->

            <intent-filter>
                <!-- 开机广播 -->
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <!-- 电量低广播 -->
                <action android:name="android.intent.action.BATTERY_LOW"/>
                <!-- 应用被安装广播 -->
                <action android:name="android.intent.action.PACKAGE_INSTALL"/>
                <!-- 应用被卸载广播 -->
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <!-- 数据类型 -->
                <data android:scheme="package"/>

                <!-- 自定义广播 -->
                <action android:name="com.imooc.demo.afdsabfdaslj"/>

            </intent-filter>

        </receiver>
   ```
   * 动态注册：在代码中通过调用Context的*registerReceiver（）*方法进行动态注册BroadcastReceiver，
   ```java
   @Override
    protected void onResume() {
        super.onResume();

        //实例化BroadcastReceiver子类 &  IntentFilter
        mBroadcastReceiver mBroadcastReceiver = new mBroadcastReceiver();
        IntentFilter intentFilter = new IntentFilter();

        //设置接收广播的类型
        intentFilter.addAction(android.net.conn.CONNECTIVITY_CHANGE);

        //调用Context的registerReceiver（）方法进行动态注册
        registerReceiver(mBroadcastReceiver, intentFilter);
    }


    //注册广播后，要在相应位置记得销毁广播
    //即在onPause() 中unregisterReceiver(mBroadcastReceiver)
    //当此Activity实例化时，会动态将MyBroadcastReceiver注册到系统中
    //当此Activity销毁时，动态注册的MyBroadcastReceiver将不再接收到相应的广播。
    @Override
    protected void onPause() {
        super.onPause();
        //销毁在onResume()方法中的广播
        unregisterReceiver(mBroadcastReceiver);
    }
   ```
   ### 注意：
   * 动态广播最好在Activity的onResume()注册、onPause()注销。
   * 原因：对于动态广播，有注册就必然得有注销，否则会导致内存泄露
 #### 两种注册方式的区别：
 1. 静态注册（常驻广播）：不受组件的生命周期影响，耗电占用内存，需要时刻监听广播
 2. 动态注册（非常驻广播）：灵活，跟随组件的生命周期变化，组件结束=广播结束，在组件结束前，必须移除广播接收器。需要在特定时刻监听广播。

3. 广播发送者向AMS发送广播 
   1. 广播发送者将此广播的”intent“通过sendBroadcast()方法发送出去
   2. 普通广播：
      * intent.setAction(BROADCAST_ACTION);进行定义
      * 若被注册了的广播接收者中注册时intentFilter的action与上述匹配，则会接收此广播（即进行回调onReceive()）。如下mBroadcastReceiver则会接收上述广播
   3. 系统广播
      * 每个广播都有特定的Intent - Filter
      * 当使用系统广播时，只需要在注册广播接收者时定义相关的action即可，并不需要手动发送广播，当系统有相关操作时会自动进行系统广播
   4. 有序广播
      * 发送出去的广播被广播接收者按照先后顺序接收，有序是针对广播接收者而言的
      * 广播接受者接收广播的顺序规则：按照Priority属性值从大-小排序；Priority属性相同者，动态注册的广播优先；
      * 先接收的广播接收者可以对广播进行截断，即后接收的广播接收者不再接收到此广播；
      * 先接收的广播接收者可以对广播进行修改，那么后接收的广播接收者将接收到被修改后的广播
      * 发送方式是：sendOrderedBroadcast(intent);
   5. App应用内广播
       * App应用内广播可理解为一种局部广播，广播的发送者和接收者都同属于一个App。
       * 相比于全局广播（普通广播），App应用内广播优势体现在：安全性高 & 效率高
       * 使用方法1：将全局广播设置成局部广播
         * 注册广播时将exported属性设置为false，使得非本App内部发出的此广播不被接收；
         * 在广播发送和接收时，增设相应权限permission，用于权限验证；
         * 发送广播时指定该广播接收器所在的包名，此广播将只会发送到此包中的App内与之相匹配的有效广播接收器中。通过 intent.setPackage(packageName) 指定报名
       * 使用方法2：使用封装好的LocalBroadcastManager类 使用方式上与全局广播几乎相同，只是注册/取消注册广播接收器和发送广播时将参数的context变成了LocalBroadcastManager的单一实例
4. 应用广播：应用发送广播，其他应用接收
   * 自定义广播动态注册（自己发送自己接收）
   ```java
   // 1. 创建广播
    public class ImoocBroadcastReceiver extends BroadcastReceiver {

        TextView mTextView;
        public ImoocBroadcastReceiver() {
        }

        public ImoocBroadcastReceiver(TextView textView) {
            mTextView = textView;
        }

        private static final String TAG = "ImoocBroadcastReceiver";
        @Override
        //接收广播时回调的方法
        public void onReceive(Context context, Intent intent) {
            // 6.2 接收广播
            if(intent != null){
                // 接收到的是什么广播
                String action  = intent.getAction();
                Log.d(TAG, "onReceive: " + action);

                // 判断是什么广播（是不是我们自己发送的自定义广播）
                if(TextUtils.equals(action, MainActivity.MY_ACTION)){
                    // 获取广播携带的内容， 可自定义的数据
                    String content = intent.getStringExtra(MainActivity.BROADCAST_CONTENT);
                    if(mTextView != null){
                        mTextView.setText("接收到的action是："+ action + "\n接收到的内容是：\n" + content);
                    }
                }
            }
   ```
   * 两个应用，一个发送一个接收
      * 建立两个包（包名在string.xml和build.gradle中改）
      * 两个包建立相同的action，一个发送一个接收     public static final String MY_ACTION = "com.imooc.demo.afdsabfdaslj";
   ```java
     protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //设置页面的名字
        // 用包名做title
        setTitle(getPackageName());
5. 生命周期
   * 广播类中的onReceive方法在主线程中，如果操作十秒，会阻塞主线程
   * 需要onReceive中建立子线程进行处理，接到消息后分类给子线程进行处理
###  <span id = "23">Application全局应用</span>
1. Application是维护全局状态的基类，在启动应用进程时会创建一个Application对象。每一个应用进程都有一个。
2. 可以扩展Application类，让系统使用扩展的类的对象
3. 组件通过intent传递数据
4. 作用：共享全局状态，初始化应用需要的服务
5. Application对象和静态单例
   * 静态单例模块化程度更好，更加灵活
   * Application就是一个context，有访问资源的能力；静态单例能接受context参数
   * Application对象能接收系统回调，生命周期由系统控制
6. 生命周期——诞生于其他任何组件对象之前并且一直存活，直到应用进程结束
7. 对象由系统管理，回调函数都运行于UI线程。常见的回调函数：onCreate，onConfigurationChanged（配置变更），onLowMemory
## <span id = "24">框架</span>
###  <span id = "25">OkHttp</span>
1. Okio
   * 访问存储处理数据
   * ByteString类——字节串，将字节集结成串成为一个统一的值，就可以对这个值进行处理。【处理数据】
      * 自动返回编码值
      * 返回校验值  byteString.md5
      * 写入输出流中
      * ByteString.Base64  将图片等二进制数据放入json文件中
      * ByteString.encodeUtf8 
      * byteString1.sha1().hex() 获取校验值并转化为字符串
   * Okio—Buffer——读取存储数据
      * 流向程序的为输入流，Source为数据源接口，提供read（）、timeout（）、close（）
      * 程序生成数据通过输出流进行输出：Sink方接口，write（）、flush（）缓存数据写到目的地、timeout（）超时处理、close（）
      * BufferedSource和BufferedSink   Buffer中的输入输出，写入缓存区或者从缓存区读出，提高读写效率
      ```java
        BufferedSource source = Okio.buffer(Okio.source(new File("in.txt")));

        BufferedSink sink = Okio.buffer(Okio.sink(new File("out.txt")));

        source.readAll(sink);

        source.close();
        sink.close();
      ```
      * 移动电源类似Buffer
      * Buffer内部类似ArraList
2. OkHttp
   * Http Client ：和服务器进行交互，处理网络请求。处理APP的请求并给APP响应
   * 创建请求Request：URL、方法（get post）、headers（请求头）、body（信息主体）
      * Get
        ```java
                Request.Builder builder = new Request.Builder();
                builder.url(GET_URL);
                Request request = builder.build();
                Call call = mClient.newCall(request);
                Response response = call.execute(); // 同步方法
        ```
      * Post：
        ```java
        Request.Builder builder = new Request.Builder();
            builder.url(POST_URL);
            builder.post(RequestBody.create(MEDIA_TYPE_MARKDOWN, "Hello world github/linguist#1 **cool**, and #1!"));
            Request request = builder.build();
            Call call = mClient.newCall(request);
        ```
   * Response:请求的相应。返回码（code）（200-ok）、headers、body（字符串或者数据流）
###  <span id = "26">EventBus</span>
1. EventBus
   * 发送方将事件发送到Bus中，进行传递，订阅方订阅和处理事件。EventBus相当于一个黑盒子。
   * 从而实现组件间的相互通信
2. 常规使用监听器和本地广播
3. 建立通信
   * 定义事件：java对象
   * 订阅事件：在BUs中注册、处理（回调函数，参数类型是定义的事件类型）
   * 发布事件：实现发布
4. 线程模式
   * 线程切换：handler或者asnayc task
   * ThreadMode：控制具体回调函数运行于哪个线程
      * POSTing
         * 发布和订阅处理运行于同一线程。默认方式
      * Main
         * 订阅函数运行于主线程
         * 发布在主，订阅在主
         * 发布在子，订阅在主
      * Main_Ordered
         * 订阅在主
         * 在Main中，发布在主线程执行后订阅回调会立即执行，会导致发布的后续指令阻塞
         * 在MainOrder中，发布在主线程执行后后续会立即执行，不用等待时事件处理方的处理，从而不会被订阅回调阻塞
      * Background
         * 发布在子，订阅回调运行在子，同一线程
         * 发布在主，订阅回调运行在子
      * Async
         * 发布在某一线程，订阅回调运行在新开的线程中。
5. Sticky——粘性事件 发布时将事件缓存起来，实现先发布再订阅
###  <span id = "27">RecyclerView</span>
1. 有限的窗口展示大量的数据
2. 灵活可配置、可自定义和重复利用的item、高度解耦的控件。
3. 回收和复用View，将功能交给别的类来操作
   * LayoutManager：布局管理器，进行展示样式
      * LinearLayoutManager：线性布局
      * GridLayoutManager：网格布局
      * StaggeredGridLayoutManager：瀑布流布局
   * Adapter：处理视图和数据的关系
      * onCreateViewHolder：创建ViewHolder并返回，每一个ViewHolder容纳一个item实例
      * onBindViewHolder：将数据放入对应的ViewHolder
      * getItemCount：返回int，是item的数量
   * ViewHolder：容纳itemView的实例（在列表或者网格中单独的view）
      * 一个holder是一个itemView。
4. 流程
   * 准备被展示的数据
   * 适配器连接数据和视图（展示的逻辑等 Adapter+ViewHolder）
   * LayoutManager控制视图的展示形式（抽象类，有实例化的类）
###  <span id = "28">Mvp模式</span>
#### 一 MVP概述
1. MVC：M—业务逻辑和实体模型  V—view，布局文件 C—Controllor，对应于Activity。关于布局中的数据绑定的操作在Activity中，View处理的很少，造成了Activity既像View又像Controller，使得Activity变得臃肿
2. MVP：M—业务逻辑和实体模型，V—Activity，负责view的绘制和用户交互，P—presenter，负责V和M之间的交互
3. 总结：
* MVP模式通过Presenter实现数据和视图之间的交互，简化了Activity的职责。同时即避免了View和Model的直接联系，又通过Presenter实现两者之间的沟通。
* 减少了Activity的职责，简化了Activity中的代码，将复杂的逻辑代码提取到了Presenter中进行处理，模块职责划分明显，层次清晰。与之对应的好处就是，耦合度更低，更方便的进行测试
* Presenter中同时持有View层的Interface的引用以及Model层的引用，View层持有Presenter层引用
* 当View层某个界面需要展示某些数据的时候，首先会调用Presenter层的引用，然后Presenter层会调用Model层请求数据，当Model层数据加载成功之后会调用Presenter层的回调方法通知Presenter层数据加载情况，最后Presenter层再调用View层的接口将加载后的数据展示给用户。
###  <span id = "29"><include merge>和viewStub</span>
#### 一 include
1. 解决重复定义布局的问题

#### 二 merge
减少层级，与其他view防止平级

#### 三 ViewStub
1. 很少使用但是布局复杂的view在需要的时候再显示出来。
2. 加载viewStub：使用inflate()方法；使用setVisibility(View.VISIBLE)
###  <span id = "30">极光推送</span>
1. 推送：使应用程序即时收到由服务器端发起的通知，客户端接收并进行展示
2. 实现推送的方式
   * 客户端定时轮询
   * 客户端与服务器建立长连接
      * 短连接：数据交互连接，交互完成则关闭
      * 客户端与服务器之间始终保持通信连接
3. 推送实现原理：客户端与服务器建立长连接，服务器主动向客户端推送消息，客户端收到后进行展示
4. 常用的推送平台——考虑推送到达率
   * 谷歌GCM
   * 极光推送（jPush）
   * 友盟推送（UPush）
   * 个推
5. 集成极光推送
   * 注册极光账号
   * 创建应用并开通推送功能
   * 集成SDK
      * 下载sdk
      * 添加jar、so、资源文件，
      * 配置AndroidManifest
      * 添加继承自BroadcastReceiver的类
      * 激活jpush插件
###  <span id = "31">WebView浏览器组件</span>
1. WebView
   * 一种ui组件
   * 基于webakkitnei内核（Chromium）
   * 可以用来展示网页并且可以和网页进行交互
2. 网页组成： HTML CSS JS
3. WebView常用方法
   * 加载网页的四种方式
      * loadUrl（String url）
      * loadUrl(String url,Map<String,String> additionalHttpHeaders)
      * loadData(String data,String mimeType,String encoding)
      * loadDataWithBaseURL(String baseUrl,String data,String mimeType,String encoding,String historyUrl)
    * 控制网页的前进和后退
       * boolean canGoBack()
       * boolean canGoForward()
       * boolean canGoBackOrForward(int steps)
       * void goBack()
       * void goForward()
       * void goBackOrForward(int steps)
       * void clearHistory()
    * 状态管理（在Activity结束的时候webView也结束，与Activity的生命周期做相同的处理）
       * onPause()：通知内核暂停所有动作
       * pauseTimers()
       * on Resume()：激活为活跃状态
       * resumeTimers()
       * destory()：销毁webview
4. Web常用类
   * WebSettings：对webView配置和管理，比如控制网页缩放，jsp的代码运行等
      * 控制js代码运行
      ```java
              WebSettings webSettings = mWebView.getSettings();
              webSettings.setJavaScriptEnabled(true);
      ```
      * 控制缩放
      ```java
        //是否支持缩放
        webSettings.setSupportZoom(true);
        //设置内置缩放控件
        webSettings.setBuiltInZoomControls(true);
        //是否隐藏原生的缩放控件
        webSettings.setDisplayZoomControls(true);
      ```
      * 控制对网页的缓存
      ```java
        //永远不使用网络只有本地缓存，没有本地缓存则不会加载
       webSettings.setCacheMode(WebSettings.LOAD_CACHE_ONLY);
        //只要本地有缓存，无论是否过期，都使用本地缓存，没有缓存才去加载网络
        webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
        //默认，根据cache-control决定是否从网络获取
       webSettings.setCacheMode(WebSettings.LOAD_DEFAULT);
        //永不使用缓存，只从网络获取
        webSettings.setCacheMode(WebSettings.LOAD_NO_CACHE);
      ```
   * WebViewClient：处理网页加载时的各种回调通知
      * 默认使用系统浏览器加载，传入参数就可以在本地app加载
      * 常用方法
      ```java
      @Override
            //进行资源请求时回调
            public WebResourceResponse shouldInterceptRequest(WebView view, WebResourceRequest request) {
                Log.e("WebViewActivity","webview-》shouldInterceptRequest请求了(android5.0之上调用)url：" + request.getUrl().toString());
                return super.shouldInterceptRequest(view, request);
            }

            @Override
            //网页开始加载的时候回调
            public void onPageStarted(WebView view, String url, Bitmap favicon) {
                super.onPageStarted(view, url, favicon);
                Log.e("WebViewActivity","webview-》onPageStarted 网页开始进行加载url：" + url);
            }

            @Override
            //加载网页之前回调
            public void onLoadResource(WebView view, String url) {
                super.onLoadResource(view, url);
                Log.e("WebViewActivity","webview-》onLoadResource 网页开始加载资源url：" + url);
            }

            @Override
            //网页加载完成时回调
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                Log.e("WebViewActivity","webview-》onPageFinished 网页已经加载完成url：" + url);
            }
      ```    
   * WebChromeClient：辅助获取网页标题、对话框、进度等
      * ```java
            mWebView.setWebChromeClient(new WebChromeClient() {
                @Override
                //获取网页加载进度
                public void onProgressChanged(WebView view, int newProgress) {
                    super.onProgressChanged(view, newProgress);
                    Log.e("webViewActivity", "newProgress:" + newProgress);
                }

                @Override
                //获取网页标题
                public void onReceivedTitle(WebView view, String title) {
                    super.onReceivedTitle(view, title);
                    Log.e("webViewActivity", "title:" + title);
                }
                     @Override
            //在网页将要打开一个alert警告对话框的时候回调
            public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
                boolean res = super.onJsAlert(view, url, message, result);
                res = true;
                Log.e("webViewActivity", "onJsAlert - url : " + url + " - message : " + message + "  - res : " + res);
                Toast.makeText(WebViewActivity.this, message, Toast.LENGTH_SHORT).show();
                result.confirm();
                return res;
            }

            @Override
            //在网页将要打开一个confirm警告对话框的时候回调
            public boolean onJsConfirm(WebView view, String url, String message, final JsResult result) {
                boolean res = super.onJsConfirm(view, url, message, result);
                res = true;
                Log.e("webViewActivity", "onJsConfirm - url : " + url + " - message : " + message + "  - res : " + res);
                AlertDialog.Builder builder = new AlertDialog.Builder(WebViewActivity.this);
                builder.setMessage(message);
                builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        result.confirm();
                    }
                });

                builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        result.cancel();
                    }
                });
                builder.create().show();
                return res;
            }

            @Override
            //在网页将要打开一个prompt警告对话框的时候回调
            public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, final JsPromptResult result) {
                boolean res = super.onJsPrompt(view, url, message, defaultValue, result);
                res = true;
                Log.e("webViewActivity", "onJsConfirm - url : " + url + " - message : " + message + " - defaultValue : " + defaultValue + "  - res : " + res);
                AlertDialog.Builder builder = new AlertDialog.Builder(WebViewActivity.this);
                builder.setMessage(message);
                builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        result.confirm("这是点击了确定按钮之后的输入框内容");
                    }
                });

                builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        result.cancel();
                    }
                });
                builder.create().show();
                return res;
            }
        ```
5. Android端调用JS代码
   * loadUrl('javascript:方法名（参数...)'),但是无法获取返回值
   ```java
       public void onShowAlertFromloadUrl (View v) {
        mWebView.loadUrl("javascript:showAlert()");
      }
   ```
   * evaluateJavascript('javascript:方法名（参数...)',ValueCallback<String> resultCallback）
   ```java
       public void onSumFromEVJS (View v) {
        mWebView.evaluateJavascript("javascript:sum(2, 3)", new ValueCallback<String>() {
            @Override
            public void onReceiveValue(String s) {
                Toast.makeText(WebViewActivity.this, "evaluateJavascript - " + s, Toast.LENGTH_SHORT).show();
            }
        });
       }
   ```
6. JS调用Android代码
   * 拦截js请求的回调方法
   ```java
    //js协议内容需要设置为Android
                Uri uri = Uri.parse(url);
                if ("android".equals(uri.getScheme())) {
                    String functionName = uri.getAuthority();
                    if ("print".equals(functionName)) {
                        String msg = uri.getQueryParameter("msg");
                        print(msg);
                        return true;
                    }
                }
   ```
   * 对象映射
   ```java
    mWebView.addJavascriptInterface(new DemoJsObject(), "android");
   ```
   ```java
    public class DemoJsObject {

        @JavascriptInterface
        public String print (String msg) {
            Log.e("DemoJsObject", "msg ：" + msg);
            return "这是android的返回值";
        }
    }

   ```
###  <span id = "32">ButterKnife实现View注入</span>
* 对一个成员变量使用@BindView注解，并传入一个View ID， ButterKnife 就能够帮你找到对应的View，并自动的进行转换（将View转换为特定的子类）
1. 注入View、资源和事件
```java

    //注入View
    @BindView(R.id.id_tv)
    //不能用private和static进行修饰，默认public        
     TextView mTv;
    @BindView(R.id.id_btn1)
    Button mBtn1;
    @BindView(R.id.id_btn2)
    Button mBtn2;

    // 注入资源
    @BindString(R.string.hello_world)
    String str;

    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ButterKnife.bind(this);

        mTv.setText(str);
        mBtn1.setText("Hello");
        mBtn2.setText("Imooc");

    }

    //注入事件
    @OnClick({R.id.id_btn1, R.id.id_btn2})
    public void btnClick(View view)
    {
        switch (view.getId())
        {
            case R.id.id_btn1:
                Toast.makeText(this, "Btn1 Clicked!", Toast.LENGTH_SHORT).show();
                break;
            case R.id.id_btn2:
                Toast.makeText(this, "Btn2 Clicked!", Toast.LENGTH_SHORT).show();
                break;
        }

    }
```
2.   注入ListView
```java
public class CategoryAdapter extends ArrayAdapter<String>
{
    private LayoutInflater mInflater;

    public CategoryAdapter(Context context, List<String> objects)
    {
        super(context, -1, objects);

        mInflater = LayoutInflater.from(context);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent)
    {
        ViewHolder holder = null;
        if (convertView == null)
        {
            convertView = mInflater.inflate(R.layout.item_category, parent, false);
            holder = new ViewHolder(convertView);
            convertView.setTag(holder);
        } else
        {
            holder = (ViewHolder) convertView.getTag();
        }

        holder.mTextView.setText(getItem(position));
        return convertView;
    }

    static class ViewHolder
    {
        @BindView(R.id.id_title_tv)
        TextView mTextView;

        public ViewHolder(View view)
        {

            ButterKnife.bind(this, view);
        }

    }
```

```java
public class CategoryActivity extends AppCompatActivity
{

    @BindView(R.id.id_listview)
    ListView mListView;

    //listview的数据
    private List<String> mData = new ArrayList<>(Arrays.asList("Simple Use", "RecyclerView Use"));


    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_category);

        ButterKnife.bind(this);

        mListView.setAdapter(new CategoryAdapter(this, mData));

    }

    @OnItemClick(R.id.id_listview)
    public void itemClicked(int position)
    {
        Toast(...);
    }

```
3. 注入RecuclerView
```java
  @OnItemClick(R.id.id_listview)
    public void itemClicked(int position)
    {
        Intent intent = null;
        switch (position)
        {
            case 0:
                intent = new Intent(this, MainActivity.class);
                break;
            case 1:
                intent = new Intent(this, RecyclerViewActivity.class);
                break;
        }
        if (intent != null)
            startActivity(intent);
    }
```
```java
public class RecyclerViewActivity extends AppCompatActivity
{
    @BindView(R.id.id_recyclerview)
    RecyclerView mRecyclerView;

    private List<String> mDatas = new ArrayList<>(Arrays.asList("Simple Use", "RecyclerView Use"));


    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycler_view);

        ButterKnife.bind(this);

        mRecyclerView.setAdapter(new RvAdapter(this, mDatas));
        mRecyclerView.setLayoutManager(new LinearLayoutManager(this));
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu)
    {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_recycler_view, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item)
    {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings)
        {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
  }
```
```java
public class RvAdapter extends RecyclerView.Adapter<RvAdapter.ViewHolder>
{
    private LayoutInflater mInflater;
    private List<String> mDatas;

    public RvAdapter(Context context, List<String> datas)
    {
        mInflater = LayoutInflater.from(context);
        mDatas = datas;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int i)
    {
        return new RvAdapter.ViewHolder(mInflater.inflate(R.layout.item_category, viewGroup, false));
    }

    @Override
    public void onBindViewHolder(ViewHolder viewHolder, int i)
    {
        viewHolder.mTextView.setText(mDatas.get(i));
    }

    @Override
    public int getItemCount()
    {
        return mDatas.size();
    }

    static class ViewHolder extends RecyclerView.ViewHolder
    {
        @BindView(R.id.id_title_tv)
        TextView mTextView;

        public ViewHolder(View itemView)
        {
            super(itemView);
            ButterKnife.bind(this, itemView);
        }
    }
```
4. ButterKnife Zelezny插件
   * 导入后，右键 Generate ButterKnife Injections，可以自动声明控件和onclick方法
   * 导入后，右键 Generate ButterKnife Injections，可以自动声明viewholder方法
## <span id = "33">高级应用</span>
###  <span id = "34">Service</span>
### 一. Service简介
1. 相当于没有界面的Activity，四大组件之一。
2. 用于在后台处理耗时操作，比如下载、音乐播放
3. 不受Activity生命周期影响
4. 运行在UI线程中，不要在Service中执行耗时的操作，除非你在Service中创建了子线程来完成耗时操作。

### 二. Service种类
#### 按运行地点分类：
1. 本地服务（Local Service）：依附在主线程上，节约资源。通信不需要IPC和AIDL，bindService方便；主线程被kill后，服务终止；比如音乐播放器等不需要常驻的服务
2. 远程服务（remote service）：独立进程，进程名为包名+android：process字符串。服务不受其他进程kill影响；独立进程占用资源；比如常驻的提供系统服务的service

#### 按运行类型分类：
1. 前台服务：会在通知栏显示；服务终止时，通知栏的notification会消失，比如音乐播放服务
2. 后台服务：不会在通知栏显示；用户看不到终止提示，比如日期同步、邮件同步等

#### 按使用方式分类：
1. startService启动的服务：启动一个服务进行后台服务，不进行通信。停止服务用stopService
2. bindService启动的服务：启动的服务要进行通信。停止服务要unbindService
3. 同时使用startService和bindService启动的服务：停止服务使用stopService和unbindService

### 三. 生命周期
### 生命周期介绍
   * onCreate()
   * onStartCommand():如果服务已经创建了，后续重复启动，操作的都是同一个服务，不会再重新创建了，除非你先销毁它。每次客户端调用startService()方法启动该Service都会回调该方法（多次调用）。一旦这个方法执行，service就启动并且在后台长期运行。通过调用stopSelf()或stopService()来停止服务。
   ```java 
       public void operate(View v){
        switch (v.getId()){
            case R.id.start:
                //启动服务:创建-->启动-->销毁
                //如果服务已经创建了，后续重复启动，操作的都是同一个服务，不会再重新创建了，除非你先销毁它
                Intent it1 = new Intent(this,MyService.class);
                startService(it1);
                break;
   ```
   * onBind():绑定.先创建再绑定，但是不会开始运行。先解绑再停止服务；
     * 会随着Actiity结束而结束。调用bindService()方法进行绑定，会调用onbind方法，但是只会调用一次,下次再调用bindservice不会回调该方法。
     * 在实onBind实现中返回一个IBinder来使客户端与service进行通信，如果不允许绑定，返回null
   ```java
       //IBinder
    //ServicerConnection:用于绑定客户端和Service
    //进度监控
    private ServiceConnection conn = new ServiceConnection() {
        //当客户端正常连接着服务时，执行服务的绑定操作会被调用
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        }
        @Override
        public void onServiceDisconnected(ComponentName componentName) {

        }
    };
   ```
   ```java
   case R.id.bind:
                //绑定服务：最大的 作用是用来实现对Service执行的任务进行进度监控
                //如果服务不存在： onCreate-->onBind-->onUnbind-->onDestory
                // （此时服务没有再后台运行，并且它会随着Activity的摧毁而解绑并销毁）
                //服务已经存在：那么bindService方法只能使onBind方法被调用，而unbindService方法只能使onUnbind被调用
                Intent it3 = new Intent(this,MyService.class);
                bindService(it3, conn,BIND_AUTO_CREATE);
   ```
   * onUnbind()：解绑。
      * 如果服务不存在： onCreate-->onBind-->onUnbind-->onDestory。
      * 服务已经存在：那么bindService方法只能使onBind方法被调用，而unbindService方法只能使onUnbind被调用
      * 当前组件调用unbindService()，想要解除与service的绑定时系统调用此方法（一次调用，一旦解除绑定后，下次再调用unbindService()会抛出异常）
   ```java
   case R.id.unbind:
                //解绑服务
                unbindService(conn);
                break;
   ```
   * onDestory()：service不再被使用时并销毁时使用，在此方法中释放资源，
   ```java
               case R.id.stop:
                Intent it2 = new Intent(this,MyService.class);
                stopService(it2);
                break;
   ```
### 三种不同情况下Service的生命周期情况。
  1. 启动操作:
     * onCreate()--onStartCommand()--服务运行--onDestory()--服务停止。进行执行确实要执行的耗时操作
     * Context.startService方法启动，那么不管是否有Activity使用bindService绑定或unbindService解除绑定到该Service，该Service都在后台运行，直到被调用stopService，或自身的stopSelf方法。
     * 如果系统资源不足，android系统也可能结束服务，
     * 还有一种方法可以关闭服务，在设置中，通过应用->找到自己应用->停止。
     * 第一次 startService 会触发 onCreate 和 onStartCommand，以后在服务运行过程中，每次 startService 都只会触发 onStartCommand
     * 不论 startService 多少次，stopService 一次就会停止服务
  2. 绑定操作：onCreate()--onBind()--绑定--onUnbind()--onDestory()--服务停止。
     * 会随着Activity的结束而结束。因此进行进度监控
     * 如果一个Service在某个Activity中被调用bindService方法启动，不论bindService被调用几次，Service的onCreate方法只会执行一次，同时onStartCommand方法始终不会调用。
     * 当建立连接后，Service会一直运行，除非调用unbindService来解除绑定、断开连接或调用该Service的Context不存在了（如Activity被Finish——即通过bindService启动的Service的生命周期依附于启动它的Context），系统在这时会自动停止该Service。
     * 第一次 bindService 会触发 onCreate 和 onBind，以后在服务运行过程中，每次 bindService 都不会触发任何回调 
     * 绑定操作的作用——Activity对后台service执行的任务进行进度监控
      * IBinder：用于远程操作对象的一个基本接口
   ```java
   //1. startService(本类对象)
    public class MyService extends Service {
        public MyService() {
        }

        private int i;
        //2. 创建子线程
        @Override
        public void onCreate() {
            super.onCreate();
            Log.e("TAG","服务创建了");
            //开启一个线程（从1数到100），用于模拟耗时的任务
            new Thread(){
                @Override
                public void run() {
                    super.run();
                    try {
                        for (i = 1; i <= 100; i++) {
                            sleep(1000);
                        }
                    }catch (Exception e){
                        e.printStackTrace();
                    }
                }
            }.start();
        }

        //启动
        @Override
        public int onStartCommand(Intent intent, int flags, int startId) {
            Log.e("TAG","服务启动了");
            return super.onStartCommand(intent, flags, startId);
        }

        //3. 绑定
        //IBinder：在android中用于远程操作对象的一个基本接口
        @Override
        public IBinder onBind(Intent intent) {
            Log.e("TAG","服务绑定了");
            //实现IBinder类所有方法的空实现，但是可以定义自己的方法，从而实现自己想要的方法
            return new MyBinder();
        }

        //对于onBind方法而言，要求返回IBinder对象
        //实际上，我们会自己定义一个内部类，继承Binder类

        //4. 定义MyBinder
        class MyBinder extends Binder{
            //定义自己需要的方法（实现进度监控）
            public int getProcess(){
                return i;
            }
        }
   ```
      * ServicerConnection
   ```java
    //IBinder
    //5. ServicerConnection:用于绑定客户端和Service
    //进度监控
    //Ibinder返回值会传入这个方法
    private ServiceConnection conn = new ServiceConnection() {
        //当客户端正常连接着服务时，执行服务的绑定操作会被调用
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            Log.e("TAG","慕课");

            MyService.MyBinder mb = (MyService.MyBinder) iBinder;
            int step = mb.getProcess();
            Log.e("TAG","当前进度是：" + step);

        }

        //当客户端和服务的连接丢失了
        @Override
        public void onServiceDisconnected(ComponentName componentName) {

        }
      };
   ```
  3. 混合型
      * service在被启动（startservice）的时候又被绑定（bindservice）， 该service会一直在后台运行
      * onCreate方法始终只调用一次，onstartcommand调用次数和startservice调用次数一致
      * 调用unBindService将不会停止Service，必须调用stopService或Service自身的stopSelf来停止服务。
  4. 在什么情况下使用 startService 或 bindService 或 同时使用startService 和 bindService？
      * 启动一个后台服务长期进行某项任务——startservice
      * 与正在运行的service取得联系——broadCast/bindService。
        * broadCast：交流比较频繁，容易造成性能上的问题，并且 BroadcastReceiver 本身执行代码的时间是很短的（也许执行到一半，后面的代码便不会执行）
        * 因此选择使用bindService方法，同时使用startService和bindService
      * 如果服务只是公开一个远程接口，供连接上的客服端（android 的 Service 是C/S架构）远程调用执行方法。这个时候可以不让服务一开始就运行，而只用 bindService ，这样在第一次 bindService 的时候才会创建服务的实例运行它，这会节约很多系统资源，特别是如果服务是Remote Service，那么该效果会越明显（当然在 Service 创建的时候会花去一定时间）。
### 四、Service的几种典型使用实例
#### 1. 不可交互的后台服务
1. 通过StartService方式开启，生命周期方法是onCreate、onStartCommand、onDestroy
2. Service类重写上述生命周期方法，并配置该服务类，在前台调用startService方法，就会启动耗时操作（在startCommend方法中开启子线程）
3. service执行在ui线程中，可以在service中创建子线程来完成耗时操作。
4. Service关闭后，需要在onDestory()方法中关闭线程。因此，在onDestory()方法中要进行必要的清理工作。

#### 2. 可交互的后台服务
1. 可交互的后台服务是指前台页面可以调用后台服务的方法，通过bindService()方式开启。Service的生命周期分别为onCreate、onBind、onUnBind、onDestroy。
2. 需要返回一个ibinder对象，返回给 前台进行获取，从而前台可以获取该代理对象执行后台服务的方法，
3. 前台调用：bindService(mIntent,con,BIND_AUTO_CREATE);con是ServiceConnection 对象，ServiceConnection 类的onServiceConnected方法传入的参数是后台返回的ibinder，从而可以通过binder对象调用后台服务类的方法。
4. 当调用unbindService()停止服务，同时要在onDestory()方法中做好清理工作。
5. 通过bindService启动的Service的生命周期依附于启动它的Context。因此当前台调用bindService的Context销毁后，那么服务会自动停止。

#### 3. 混合型后台服务
将上面两种启动方式结合起来就是混合性交互的后台服务了，即可以单独运行后台服务，也可以运行后台服务中提供的方法，其完整的生命周期是：onCreate->onStartCommand->onBind->onUnBind->onDestroy

#### 4. 前台服务
1. 前台服务提升了服务所在的进程级别，当系统内存不足的时候后台服务会被回收掉，前台服务不会被回收掉
2. 前台服务需要在service的基础上创建一个Notification，然后调用Service的startForeground()方法来启动为前台服务。然后调用startService方法启动前台服务
3. 点击notification的跳转不能使用PendingIntent的原因。会在程序退出后，notification仍然存在，点击后再back会回退到home界面而不是应用程序的界面，因此增加使用taskStackBuilder来进行跳转。
4. 用TaskStackBuilder.create(this)创建一个stackBuilder实例，然后stackBuilder.addParentStack(NotificationShow.class);含义为：为跳转后的activity添加一个父activity，在activity中的manifest中添加parentActivityName。也就是说在NotificationShow这个界面点击回退时，会跳转到MainActivity这个界面，而不是直接回到了home菜单。
5. 通过 stopForeground()方法可以取消通知，即将前台服务降为后台服务。此时服务依然没有停止。通过stopService()可以把前台服务停止。
###  <span id = "35">Binder机制和AIDL使用</span>
### Binder原理
#### 1. 概述
用于进程间通信。Binder数据拷贝只需要一次，而管道、消息队列、Socket都需要2次，共享内存方式一次内存拷贝都不需要，但实现方式又比较复杂。并且Binder机制从协议本身就支持对通信双方做身份校检，从而大大提升了安全性
#### 2. Binder
注册服务(addService)： 在Android开机启动过程中，Android会初始化系统的各种Service，并将这些Service向ServiceManager注册（即让ServiceManager管理）。这一步是系统自动完成的。

获取服务(getService)： 客户端想要得到具体的Service直接向ServiceManager要即可。客户端首先向ServiceManager查询得到具体的Service引用，通常是Service引用的代理对象，对数据进行一些处理操作。

使用服务： 通过这个引用向具体的服务端发送请求，服务端执行完成后就返回。

### AIDL使用
使用服务:
1. binder对象的获取
 * Binder对象在服务端和客户端是共享的，是同一个Binder对象。客户端通过binder对象获取实现了interface接口的对象来调用远程服务，然后通过binder来实现参数传递
 * 服务端获取binder对象并保存interface接口对象：binder具有跨进程传输能力是因为实现了IBinder接口，系统会为每个实现了该接口的对象提供跨进程传输。
 * 客户端获取binder对象并获取interface接口对象：通过bindService获得binder对象，再通过binder对象获得interface对象（ServiceConnection ）
2. 调用服务端方法
   * 调用服务端方法时应开启子线程防止UI线程堵塞导致ANR
#### 1. 简介
一种接口定义语言，用于生成可以在安卓设备上两个进程之间进程间通信的代码。如果在一个进程中调用另一个进程中对象的操作，可以使用AIDL生成可序列化的参数来完成进程间通信

#### 2. 使用
1. 服务端首先要创建一个Service来监听客户端的连接请求，
###  <span id = "36">ContentProvider</span>
### 一 作用
1. 四大组件之一——为存储和获取数据提供统一的接口。也就是将自身通过接口将数据提供给外部。
2. 进程间进行数据交互&共享，即跨进程通信
3. 实现不同的应用程序之间共享数据（比如微信可以访问联系人数据）
4. 属于数据的搬运工，真正存储和操作数据的数据源还是原来存储数据的方式（数据库、文件、xml或网络）。

### 二 原理
1. 将数据存表，然后以操作数据库的形式去操作数据。
2. 底层采用Android中的Binder机制

### 三 使用
#### 1. 统一资源标识符（URI）
* 唯一标识contentprovider以及其中的数据
* 外界进程通过URI找到对应的contentprovider和其中的数据，再进行数据操作。
* 分为系统预置和自定义，自定义对应自定义数据库
* 自定义URI = content://（主题名）com.carson.provider（授权信息）/user（表名）/1(记录)
  * 主题：URI前缀
  * 授权信息：唯一标识符
  * 表名：指向数据库中的某个表名
  * 记录：表中的某个记录，若无指定就返回全部记录
#### 2. MIME数据类型
* 指设定某种扩展名的文件用一种应用程序来打开的方式类型，多用于指定一些客户端自定义的文件名以及一些媒体文件打开方式
* 作用：指定某个扩展名的文件用某种应用程序来打开 如指定.html文件采用text应用程序打开、指定.pdf文件采用flash应用程序打开
* 使用：
  * contentprovider根据URL返回MIME类型
  * MIME由两部分组成 类型/子类型
#### 3. Contentprovider类
* 以表格的方式组织数据，也支持文件数据。
* 共享数据：添加、删除、获取&修改更新数据
* 核心方法：
```java
    public Uri insert(Uri uri, ContentValues values) 
  // 外部进程向 ContentProvider 中添加数据

  public int delete(Uri uri, String selection, String[] selectionArgs) 
  // 外部进程 删除 ContentProvider 中的数据

  public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs)
  // 外部进程更新 ContentProvider 中的数据

  public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs,  String sortOrder)　 
  // 外部应用 获取 ContentProvider 中的数据

  // 注：
  // 1. 上述4个方法由外部进程回调，并运行在ContentProvider进程的Binder线程池中（不是主线程）
 // 2. 存在多线程并发访问，需要实现线程同步
   // a. 若ContentProvider的数据存储方式是使用SQLite & 一个，则不需要，因为SQLite内部实现好了线程同步，若是多个SQLite则需要，因为SQL对象之间无法进行线程同步
  // b. 若ContentProvider的数据存储方式是内存，则需要自己实现线程同步
  
    <-- 2个其他方法 -->
    public boolean onCreate() 
    // ContentProvider创建后 或 打开系统后其它进程第一次访问该ContentProvider时 由系统进行调用
    // 注：运行在ContentProvider进程的主线程，故不能做耗时操作

    public String getType(Uri uri)
    // 得到数据类型，即返回当前 Url 所代表数据的MIME类型
```
* 常见的数据如通讯录等有内置的默认的Contentprovder，自定义需要重写上面六个方法
* contentprovider主要通过contentresolver与外部进行交互
#### 4. 辅助工具类——contentResolver类
* 通过URI操作不同的contentprovider中的数据；外部进程通过 ContentResolver类 从而与ContentProvider类进行交互
* 为什么要使用通过ContentResolver类从而与ContentProvider类进行交互，而不直接访问ContentProvider类？
  * 一款应用要使用多个ContentProvider，若需要了解每个ContentProvider的不同实现从而再完成数据交互，操作成本高 & 难度大，所以再ContentProvider类上加多了一个 ContentResolver类对所有的ContentProvider进行统一管理。
* 具体使用
```java
    // 外部进程向 ContentProvider 中添加数据
    public Uri insert(Uri uri, ContentValues values)　 

    // 外部进程 删除 ContentProvider 中的数据
    public int delete(Uri uri, String selection, String[] selectionArgs)

    // 外部进程更新 ContentProvider 中的数据
    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs)　 

    // 外部应用 获取 ContentProvider 中的数据
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)

    // 使用ContentResolver前，需要先获取ContentResolver
    // 可通过在所有继承Context的类中 通过调用getContentResolver()来获得ContentResolver
    ContentResolver resolver =  getContentResolver(); 

    // 设置ContentProvider的URI
    Uri uri = Uri.parse("content://cn.scu.myprovider/user"); 

    // 根据URI 操作 ContentProvider中的数据
    // 此处是获取ContentProvider中 user表的所有记录 
    Cursor cursor = resolver.query(uri, null, null, null, "userid desc");
```
#### 5. 辅助工具类——contentUris
* 操作URI
* 具体使用 核心方法有两个：withAppendedId（） &parseId（）
```java
    // withAppendedId（）作用：向URI追加一个id
    Uri uri = Uri.parse("content://cn.scu.myprovider/user") 
    Uri resultUri = ContentUris.withAppendedId(uri, 7);  
    // 最终生成后的Uri为：content://cn.scu.myprovider/user/7

    // parseId（）作用：从URL中获取ID
    Uri uri = Uri.parse("content://cn.scu.myprovider/user/7") 
    long personid = ContentUris.parseId(uri); 
    //获取的结果为:7
```
#### 6. 辅助工具类——UriMatcher类
* 在ContentProvider 中注册URI；根据 URI 匹配 ContentProvider 中对应的数据表
* 具体使用：
```java
// 步骤1：初始化UriMatcher对象
 UriMatcher matcher = new UriMatcher(UriMatcher.NO_MATCH);
 // 步骤2：在ContentProvider 中注册URI（addURI（））
 matcher.addURI("cn.scu.myprovider", "user1", URI_CODE_a);
// 步骤3：根据URI 匹配 URI_CODE，从而匹配ContentProvider中相应的资源（match（））
public String getType (Uri uri) {
 }
```
#### 7. 辅助工具类——ContentObserver类
* 内容观察者，观察 Uri引起ContentProvider 中的数据变化 & 通知外界（即访问该数据访问者）
* 当ContentProvider 中的数据发生变化（增、删 & 改）时，就会触发该 ContentObserver类
* 使用
```java
// 步骤1：注册内容观察者ContentObserver
getContentResolver().registerContentObserver（uri）；
// 步骤2：当该URI的ContentProvider数据发生变化时，通知外界（即访问该ContentProvider数据的访问者）
public class UserContentProvider extends ContentProvider {
    public Uri insert(Uri uri, ContentValues values) {
        db.insert("user", "userid", values);
        getContext().getContentResolver().notifyChange(uri, null);
        // 通知访问者
    }
}

// 步骤3：解除观察者
getContentResolver().unregisterContentObserver（uri）；
// 同样需要通过ContentResolver类进行解除
```
### 四 实例说明
#### 1. 进程内通信
1. 创建数据库类 DBHelper.java
2. 自定义 ContentProvider 类,要重写6个方法，
3. 注册 创建的 ContentProvider类 在AndroidManifest.xml中
4. 进程内访问数据
```java
       // 设置URI
        Uri uri_user = Uri.parse("content://cn.scu.myprovider/user");

        // 插入表中数据
        ContentValues values = new ContentValues();
        values.put("_id", 3);
        values.put("name", "Iverson");
         // 获取ContentResolver
        ContentResolver resolver =  getContentResolver();
        // 通过ContentResolver 根据URI 向ContentProvider中插入数据
        resolver.insert(uri_user,values);
         // 通过ContentResolver 向ContentProvider中查询数据
        Cursor cursor = resolver.query(uri_user, new String[]{"_id","name"}, null, null, null);
        while (cursor.moveToNext()){
            System.out.println("query book:" + cursor.getInt(0) +" "+ cursor.getString(1));
            // 将表中数据全部输出
        }
        cursor.close();
        // 关闭游标
```
#### 2. 进程间数据共享
1. 需要创建2个进程，进程1创建contentprovider并存储数据，进程2访问contentprovider中的存储数据。
2. 进程1：
   1. 创建数据库类
   2. 自定义 ContentProvider 类,要重写6个方法，
   3. 注册 创建的 ContentProvider类 在AndroidManifest.xml中
    * 需要声明外界可访问的权限，可以戏份为读写
    * 设置此provider是否可以被其他进程使用
3. 进程2:
   1. AndroidManifest.xml中声明权限，与进程1中的相对应
   2. 访问数据，基本同上。

### 五 优点
1. 应用间数据交互可以根据需求将自己的数据开放给其他应用进行增删改查，不用担心因为直接开发数据库权限而带来安全问题。
2. 采用不同的存储方式存储数据，数据的访问方式也会不同。比如文件操作读写文件方式存储的数据，sharedpreferences API读写Sharedpreferences 共享数据。而采用ContentProvider方式，，使得无论底层数据存储采用何种方式，外界对数据的访问方式都是统一的。
###  <span id = "37">Bitmap压缩策略</span>

### 一 为什么bitmap需要高效加载
1. 高清大图的加载很容易造成内存溢出，因此需要高效加载
2. Imageview有时无法显示原始图片，可以按照一定的采样率来将图片缩小后再加载进来。

### 二 bitmap高效加载的具体方式
#### 1. 加载bitmap的方式
bitmapFactory提供四类方法，分别从文件系统、资源、输入流和字节数组中加载bitmap对象
#### 2. BitmapFactory.Options的参数
1. inSampleSize参数
   * 采样率，设置图片的像素的宽和高
   * 值 = 1，即为原始大小
   * 值 > 1,图片缩小，缩放比例为1/（值的二次方）
   * 关于inSampleSize取值的注意事项： 通常是根据图片宽高实际的大小/需要的宽高大小，分别计算出宽和高的缩放比。但应该取其中最小的缩放比，避免缩放图片太小，到达指定控件中不能铺满，需要拉伸从而导致模糊。
2. inJustDecodeBounds参数
   * 不加载图片就想获取到图片的宽高信息的时候，通过inJustDecodeBounds=true，然后加载图片就可以实现只解析图片的宽高信息，并不会真正的加载图片，
   * 当获取了宽高信息，计算出缩放比后，然后在将inJustDecodeBounds=false,再重新加载图片，就可以加载缩放后的图片。
3. 高效加载bitmap的流程
   * 将BitmapFactory.Options的inJustDecodeBounds参数设为true并加载图片。
   * 从BitmapFactory.Options中取出图片的原始宽高信息，它们对应于outWidth和outHeight参数。
   * 根据采样率的规则并结合目标View的所需大小计算出采样率inSampleSize。
   * 将BitmapFactory.Options的inJustDecodeBounds参数设为false，然后重新加载图片。

###  <span id = "38">HandlerThread</span>
### 一 使用场景
1. 执行耗时操作需要开启子线程，线程的开启和销毁很消耗性能，因此可以使用线程池或者使用HandlerThread
2. 可以用来执行多个耗时操作，不需要多次开启线程，里面是采用Handler和Looper实现的.
3. 相当于是在子线程中创建了looper，即在子线程中执行耗时的、可能有多个任务的操作。它就是一个帮我们创建 Looper 的线程，让我们可以直接在线程中使用 Handler 来处理异步任务。
4. 在子线程中直接创建Looper是很复杂的，因此使用HandlerThread，它包含了Looper，可以直接使用这个Looper创建的Handler
### 二 使用方法
1. 创建HandlerThread的实例对象
```java
    HandlerThread handlerThread = new HandlerThread("myHandlerThread");
```
2. 启动创建的HandlerThread线程
```java
    handlerThread.start();
```
当调用start后，子线程中的Looper开始工作，会按顺序取出消息队列中的队列处理，然后调用子线程的 Handler 处理
3. 将HandlerThread与Handler绑定在一起，其实就是将HandlerThread的Looper和Handler绑定在一起
```java
    mThreadHandler = new Handler(mHandlerThread.getLooper()) {
        @Override
        public void handleMessage(Message msg) {
            checkForUpdate();
            if(isUpdate){
                mThreadHandler.sendEmptyMessage(MSG_UPDATE_INFO);
            }
        }
    };
```
### 三 源码解析
1. 构造方法
```java
public HandlerThread(String name) {
    super(name);
    mPriority = Process.THREAD_PRIORITY_DEFAULT;
}

public HandlerThread(String name, int priority) {
    super(name);
    mPriority = priority;
}
```
name表示当前线程的名称，priority为线程的优先级别

2. run方法：初始化Looper获取当前线程的Looper并设置线程的优先级别
```java
    public void run() {
        mTid = Process.myTid();
        Looper.prepare();
        //持有锁机制来获得当前线程的Looper对象
        synchronized (this) {
            mLooper = Looper.myLooper();
            //发出通知，当前线程已经创建mLooper对象成功，这里主要是通知getLooper方法中的wait
            notifyAll();
        }
        //设置线程的优先级别
        Process.setThreadPriority(mPriority);
        //这里默认是空方法的实现，我们可以重写这个方法来做一些线程开始之前的准备，方便扩展
        onLooperPrepared();
        Looper.loop();
        mTid = -1;
    }
```
   * 使用HandlerThread的时候必须调用start()方法，接着才可以将HandlerThread和handler绑定在一起。原因就是run()方法才开始初始化looper，而调用HandlerThread的start()方法的时候，线程会交给虚拟机调度，由虚拟机自动调用run方法。即调用start方法后对于内部才是调用了run方法
   * 这里我们为什么要使用锁机制和notifyAll();，原因我们可以从getLooper()方法中知道：
   ```java
    public Looper getLooper() {
        if (!isAlive()) {
            return null;
        }
        // 直到线程创建完Looper之后才能获得Looper对象，Looper未创建成功，阻塞
        synchronized (this) {
            while (isAlive() && mLooper == null) {
                try {
                    wait();
                } catch (InterruptedException e) {
                }
            }
        }
        return mLooper;
    }
   ```
   总结：在获得mLooper对象的时候存在一个同步的问题，只有当线程创建成功并且Looper对象也创建成功之后才能获得mLooper的值。这里等待方法wait和run方法中的notifyAll方法共同完成同步问题。
1. quit和quitSafe方法
    * quit方法退出Looper消息循环及退出循环
    * quitSafe方法调用安全的退出线程
    * 最终调用到MessageQueue的quit方法
    * quit方法：调用removeAllMessagesLocked();就是遍历Message链表，移除所有信息的回调，并重置为null。
    * quitSafe方法：调用removeAllFutureMessagesLocked();这个方法，它会根据Message.when这个属性，判断我们当前消息队列是否正在处理消息，没有正在处理消息的话，直接移除所有回调，正在处理的话，等待该消息处理处理完毕再退出该循环。因此说quitSafe()是安全的，而quit()方法是不安全的，因为quit方法不管是否正在处理消息，直接移除所有回调。
  

   


### <span id = "39">IntentService</span>    
### 一 定义
IntentService是Android的一个封装类，继承自Service

### 二 作用
处理异步请求，实现多线程

### 三 工作流程
1. 调用startService方法传递请求IntentService ——> IntentServie在onCreate方法中通过HandlerThread单独开启一个线程，来依次处理所有Intent请求对象所对应的任务 ——>当执行完所有的Intent对应的任务并且没有新的Intent到达 ——> Intent自动停止
2. 若启动IntenrService多次，那么每个耗时操作以队列的方式在IntentService的onHandleIntent回调方法中依次执行，执行完自动结束

### 四 具体实例
1. 定义IntentService的子类：传入线程名称、复写onHandleIntent()方法，onHandleIntent()中根据Intent的不同进行不同的事务处理，onStartCommand方法默认实现将请求的Intent添加到工作队列中
2. 在Manifest.xml中注册服务
3. 在Activity中开启Service服务
```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

            //同一服务只会开启一个工作线程
            //在onHandleIntent函数里依次处理intent请求。

            Intent i = new Intent("cn.scu.finch");
            Bundle bundle = new Bundle();
            bundle.putString("taskName", "task1");
            i.putExtras(bundle);
            startService(i);

            Intent i2 = new Intent("cn.scu.finch");
            Bundle bundle2 = new Bundle();
            bundle2.putString("taskName", "task2");
            i2.putExtras(bundle2);
            startService(i2);

            startService(i);  //多次启动
        }
    }
```
4. 工作总结
 * 本质是采用Handler和HandlerThread方式
 * 通过HandlerThread单独开启一个名叫IntentService的线程
 * 创建一个名叫ServiceHandler的内部Handler
 * 把内部Handler和HandlerThread对应的子线程进行绑定
 * 通过onStartCommand()将intent依次插入到工作队列中，并逐个发送给onHandleIntent()
 * 通过onHandleIntent()来依次处理所有Intent请求对象所对应的任务
 * 因此通过复写方法onHandleIntent()，再在里面根据Intent的不同进行不同的线程操作就可以了

5. 注意事项
工作任务队列是顺序执行的，原因：
* 由于onCreate() 方法只会调用一次，所以只会创建一个工作线程
* 多次调用 startService(Intent) ，即多次调用onstartCommand方法，但是不会创建新的工作线程，只是把消息加入消息队列中等待执行，所以，多次启动 IntentService 会按顺序执行事件；
* 如果服务停止，会清除消息队列中的消息，后续的事件得不到执行

### 五 使用场景
1. 线程任务需要按顺序、在后台执行的使用场景——离线下载
2. 由于所有的任务都在同一个Thread looper里面来做，所以不符合多个数据同时请求的场景。

### 六 对比
1. IntentService与Service的区别
* Service依赖于主线程，不能编写耗时的操作，否则会ANR
* IntentService：创建一个工作线程来处理多线程任务
* Service需要主动调用stopSelft()来结束服务，而IntentService不需要（在所有intent被处理完后，系统会自动关闭服务）
2. IntentService与其他线程的区别
* IntentService内部采用了HandlerThread实现，作用类似于后台线程；
* 与后台线程相比，IntentService是一种后台服务，优势是：优先级高（不容易被系统杀死），从而保证任务的执行。
  * 对于后台线程，若进程中没有活动的四大组件，则该线程的优先级非常低，容易被系统杀死，无法保证任务的执行


### <span id = "40">LRUCache</span>
### 一 安卓中的缓存策略
1. 缓存策略主要包括缓存的添加、获取和删除三类操作
2. LRU：最近最少使用算法，当缓存满时，会优先淘汰最近最少使用的缓存对象。
   * LrhCache：内存缓存
   * DisLruCache：硬盘缓存

### 二 LruCache的使用
#### 1. 介绍
原理：把最近使用的对象，用强引用，存储在LinkedHashMap中，当缓存满时，把最近最少使用的对象从内存中移除。并提供了get和put方法来完成

#### 2. 使用
```java
    int maxMemory = (int) (Runtime.getRuntime().totalMemory() / 1024);
    int cacheSize = maxMemory / 8;
    mMemoryCache = new LruCache<String, Bitmap>(cacheSize) {
        @Override
        protected int sizeOf(String key, Bitmap value) {
            return value.getRowBytes() * value.getHeight() / 1024;
        }
    };
```
1. 设置LruCache缓存的大小，一般为当前进程可用容量的1/8
2. 重写sizeof方法，计算出要缓存的大小

### 三 实现原理
维护一个缓存对象列表，排列方式按照访问顺序实现，即一直没访问的对象放在队尾，最近访问的对象放在队头。
#### 1. LinkedHashMap
1. 数组+双向链表
2. accessOrder设置为true按照访问顺序，最近访问的最后输出。

#### 2. put方法
```java
    public final V put(K key, V value) {
            //不可为空，否则抛出异常
        if (key == null || value == null) {
            throw new NullPointerException("key == null || value == null");
        }
        V previous;
        synchronized (this) {
                //插入的缓存对象值加1
            putCount++;
                //增加已有缓存的大小
            size += safeSizeOf(key, value);
            //向map中加入缓存对象
            previous = map.put(key, value);
                //如果已有缓存对象，则缓存大小恢复到之前
            if (previous != null) {
                size -= safeSizeOf(key, previous);
            }
        }
            //entryRemoved()是个空方法，可以自行实现
        if (previous != null) {
            entryRemoved(false, key, previous, value);
        }
            //调整缓存大小(关键方法)
        trimToSize(maxSize);
        return previous;
    }
``` 
在添加过缓存对象后，调用trimToSize方法来判断缓存是否已满，如果满了就删除最近最少使用的。

#### 3. timeToSize
不断地删除LinkedHashMap中队尾的元素，即近期最少访问的，直到缓存大小小于最大值。

#### 4. get方法
当调用LruCache的get()方法获取集合中的缓存对象时，就代表访问了一次该元素，将会更新队列，将访问的元素更新到队列头部，保持整个队列是按照访问顺序排序。最终会调用到recordAccess方法:先删除再移动到队列的头部
```java
    void recordAccess(HashMap<K,V> m) {
        LinkedHashMap<K,V> lm = (LinkedHashMap<K,V>)m;
        //判断是否是访问排序
        if (lm.accessOrder) {
            lm.modCount++;
            //删除此元素
            remove();
            //将此元素移动到队列的头部
            addBefore(lm.header);
        }
    }
```
#### 5. 总结
LruCache维护了一个集合，LinkedHashMap。以访问顺序进行排序，当调用put方法时，就会在集合中添加元素并调用trimToSize方法判断缓存是否已经满，如果满了就用LinkedHashMap的迭代器删除队尾元素，即近期最少访问的元素。当调用get()方法访问缓存对象时，就会调用LinkedHashMap的get()方法获得对应集合元素，同时会更新该元素到队头。


















### <span id = "41">Window、Activity、DecorView以及ViewRoot之间的关系</span>
#### 一 职能简介
#### Activity
1. 不负责视图控制，控制生命周期和处理事件
2. Activity就像一个控制器，统筹视图的添加与显示，以及通过其他回调方法，来与Window、以及View进行交互。

#### window
1. 视图的承载器，内部持有一个DecorView，是view的根布局
2. Window是一个抽象类，实际在Activity中持有的是其子类PhoneWindow。
3. PhoneWindow中有个内部类DecorView，通过创建DecorView来加载Activity中设置的布局R.layout.activity_main。
4. Window 通过WindowManager将DecorView加载其中，并将DecorView交给ViewRoot，进行视图绘制以及其他交互

#### DecorView
1. 是安卓视图树的根节点视图，
2. 顶级view，内部包含一个竖直方向的LinearLayout，有上下三个部分，上面是个ViewStub，延迟加载的视图（应该是设置ActionBar，根据Theme设置），中间的是标题栏(根据Theme设置，有的布局没有)，下面的是内容栏。
3. 在Activity中通过setContentView所设置的布局文件其实就是被加到内容栏之中的，成为其唯一子View

#### ViewRoot
1. view的绘制和事件分发由viewRoot来执行
2. ViewRoot对应ViewRootImpl类，它是连接WindowManagerService和DecorView的纽带，View的三大流程（测量（measure），布局（layout），绘制（draw））均通过ViewRoot来完成。
3. ViewRoot并不属于View树的一份子。从源码实现上来看，它既非View的子类，也非View的父类，但是，它实现了ViewParent接口，这让它可以作为View的名义上的父视图。
4. RootView继承了Handler类，可以接收事件并分发，Android的所有触屏事件、按键事件、界面刷新等事件都是通过ViewRoot进行分发的。

#### 二 DecorView的创建
1.  先在PhoneWindow中创建了一个DecroView，其中创建的过程中可能根据Theme不同，加载不同的布局格式，例如有没有Title，或有没有ActionBar等，
2.  然后再向mContentParent中加入子View，即Activity中设置的布局。到此位置，视图一层层嵌套添加上了。mContentParent就是@android:id/content所对应的FrameLayout。

#### 三 DecorView的显示
1. 通过setContentView()设置的界面，在onResume()之后才对用户可见
2. ActivityThread中的handleLaunchActivity方法，调用handleResumeActivity()方法，其中执行了Activity.makeVisible()方法之后，界面才对我们是可见的。
3. Activity.makeVisible()中的addView()方法，最终调用到setview方法，然后进行view的绘制，视图进行显示。


#### 四 总结
Activity就像个控制器，不负责视图部分。Window像个承载器，装着内部视图。DecorView就是个顶层视图，是所有View的最外层布局。ViewRoot像个连接器，负责沟通，通过硬件的感知来通知视图，进行用户之间的交互。

### <span id = "42">View测量布局及绘制原理</span>
#### 一 view绘制的流程框架
1. view的绘制是从上往下一层层迭代下来的，自上而下遍历DecorView-->ViewGroup（--->ViewGroup）-->View ，按照这个流程从上往下，依次measure(测量),layout(布局),draw(绘制)。

#### 二 Measure流程
在某些情况下，需要多次测量才能确定view最终的宽高，在该情况下，measure过程后得到的宽高可能不准确，此时建议在layout过程中onLayout去获取最终的宽高
1. performMeasure -> measure -> onMeasure -> Measure
2. measure，就是测量每个控件的大小
3. 调用measure()方法，进行一些逻辑处理，然后调用onMeasure()方法，在其中调用setMeasuredDimension()设定View的宽高信息，完成View的测量操作。
```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
                getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
    }
```
在获取widthMeasureSpec, heightMeasureSpec这两个参数信息后，调用setMeasureDimension方法，指定view的宽高，完成测量工作

#### MeasureSpec的确定
1. 组成
   * MeasureSpec(测量规格，32位的int值) = mode（测量模式 高2位即31、32位） + size（具体测量大小 底30位）
   * mode模式
     * UNSPECIFIED ：（不限制）不对View进行任何限制，要多大给多大，一般用于系统内部
     * EXACTLY：（精确数值）对应LayoutParams中的match_parent和具体数值这两种模式。检测到View所需要的精确大小，这时候View的最终大小就是SpecSize所指定的值，
     * AT_MOST ：（占满父容器）对应LayoutParams中的wrap_content。View的大小不能大于父容器的大小。
2. MeasureSpec的确定
    * DecorView：根据LayoutParams的布局格式（match_parent，wrap_content或指定大小），将自身大小和屏幕大小相比，设置一个不超过屏幕大小的宽高。—— 通过屏幕的大小，和自身的布局参数LayoutParams。
    * 其他view：通过父布局的MeasureSpec和自身的布局参数LayoutParams
      * 父布局是EXACTLY：子布局为EXACTLY时为自身具体大小，其他模式是父view的剩余尺寸
      * 父布局是AT_MOST：子布局为EXACTLY时为自身具体大小，其他模式是父view的剩余尺寸
        * wrapparent和matchparent都是父view的剩余尺寸，为了显示区别，在自定义View时，需要重写onMeasure方法，处理wrap_content时的情况，进行特别指定。
      * 父布局是UNSPECIFIED：子布局为EXACTLY时为自身具体大小，剩下Matchparent和wrapparent都是0
    * 可以看出MeasureSpec的指定也是从顶层布局开始一层层往下去，父布局影响子布局
3. view的测量流程
开始测量 ——> measure ——> onMeasure(在viewGroup中需要重写此方法，在此方法中定义自己的测量方式，一般的测量步骤：先遍历测量group中各个子view，根据子view的测量值定义自己的宽高信息。通过计算整个ViewGroup中各个View的属性，从而最终确定整个ViewGroup的属性；最后setMeasuredDimension()方法存储测量后view宽高的值) ——> measureChildren(循环遍历每个子view测量，不需要重写) ——> 如果是viewGroup再回到measure处，直到为view时 ——> measure ——> onMeasure(可能会重写onMeasure方法，对wrapcontent情况设置)


#### 三 Layout流程
测量完view后，需要将view布局在window中，view的布局主要通过确定上下左右四个点来确定的。
布局也是自上而下，不同的是ViewGroup先在layout()中确定自己的布局，然后在onLayout方法中再调用子view的Layout方法，确定子view布局。在Measure过程中，ViewGroup一般是先测量子View的大小，然后再确定自身的大小。
```java
public void layout(int l, int t, int r, int b) {  

    // 当前视图的四个顶点
    int oldL = mLeft;  
    int oldT = mTop;  
    int oldB = mBottom;  
    int oldR = mRight;  

    // setFrame（） / setOpticalFrame（）：确定View自身的位置
    // 即初始化四个顶点的值，然后判断当前View大小和位置是否发生了变化并返回  
 boolean changed = isLayoutModeOptical(mParent) ?
            setOpticalFrame(l, t, r, b) : setFrame(l, t, r, b);

    //如果视图的大小和位置发生变化，会调用onLayout（）
    if (changed || (mPrivateFlags & PFLAG_LAYOUT_REQUIRED) == PFLAG_LAYOUT_REQUIRED) {  

        // onLayout（）：确定该View所有的子View在父容器的位置     
        onLayout(changed, l, t, r, b);      
  ...

}
```
onLayout():确定子View的布局，如果当前view是一个单一的view没有子view就不需要实现该方法。如果当前view是一个viewgroup，就需要实现onLayout方法。

view的布局流程
开始布局 ——> layout ——> setFrame(确定自己在布局中的位置) ——> onLayout(当前是viewgroup此方法需要重写。其中关键逻辑循环调用child.layout方法，来确定子view在当前viewGroup中的位置布局) ——> child.Layout ——> 当不是viewgroup时，——> setFrame ——> onLayout（没有子view就是空实现）——> 布局完毕

#### 四 Draw过程
1. 绘制过程
   * 绘制背景 background.draw(canvas)
   * 绘制自己（onDraw）
   * 绘制Children(dispatchDraw)
   * 绘制装饰（onDrawScrollBars）
```java
public void draw(Canvas canvas) {
// 所有的视图最终都是调用 View 的 draw （）绘制视图（ ViewGroup 没有复写此方法）
// 在自定义View时，不应该复写该方法，而是复写 onDraw(Canvas) 方法进行绘制。
// 如果自定义的视图确实要复写该方法，那么需要先调用 super.draw(canvas)完成系统的绘制，然后再进行自定义的绘制。
    ...
    int saveCount;
    if (!dirtyOpaque) {
          // 步骤1： 绘制本身View背景
        drawBackground(canvas);
    }

        // 如果有必要，就保存图层（还有一个复原图层）
        // 优化技巧：
        // 当不需要绘制 Layer 时，“保存图层“和“复原图层“这两步会跳过
        // 因此在绘制的时候，节省 layer 可以提高绘制效率
        final int viewFlags = mViewFlags;
        if (!verticalEdges && !horizontalEdges) {

        if (!dirtyOpaque) 
             // 步骤2：绘制本身View内容  默认为空实现，  自定义View时需要进行复写
            onDraw(canvas);

        ......
        // 步骤3：绘制子View   默认为空实现 单一View中不需要实现，ViewGroup中已经实现该方法
        dispatchDraw(canvas);

        ........

        // 步骤4：绘制滑动条和前景色等等
        onDrawScrollBars(canvas);

       ..........
        return;
    }
    ...    
}

```
自定义View一般要重写onDraw()方法，在其中绘制不同的样式。
在ViewGroup中，实现了 dispatchDraw()方法，而在单一子View中不需要实现该方法。

2. 流程
开始绘制 ——> draw ——> drawBackgroud ——> onDraw ——>(绘制自己是空方法，无论是单一view还是viewgroup都需要自定义实现)  ——> dispatchDraw(将绘制事件分发给子view让子view进行绘制) ——> onDrawScrollBars（绘制装饰） ——> 绘制完毕

#### 五 总结
onMeasure()方法：单一View，一般重写此方法，针对wrap_content情况，规定View默认的大小值，避免于match_parent情况一致；ViewGroup，若不重写，就会执行和单子View中相同逻辑，不会测量子View。一般会重写onMeasure()方法，循环测量子View。（先测量子view再确定viewgroup）
onLayout()方法: 单一View，不需要实现该方法。ViewGroup必须实现，该方法是个抽象方法，实现该方法，来对子View进行布局。（先确定group布局再确定子view布局）
onDraw()方法：无论单一View，或者ViewGroup都需要实现该方法，因其是个空方法。自定义view需要自己在其中进行绘制。


### <span id = "43">Android虚拟机及编译过程</span>
### 一 什么是Dalvik虚拟机
1. 是用于安卓平台的java虚拟机，运行在安卓的运行时库层
2. Dalvik作为面向Linux、为嵌入式操作系统设计的虚拟机，主要负责完成对象生命周期管理、堆栈管理、线程管理、安全和异常管理，以及垃圾回收等。另外，Dalvik早期并没有JIT编译器，直到Android2.2才加入了对JIT的技术支持。

### 二 Dalvik虚拟机的特点
1. 体积小，占用内存空间小
2. 专有的DEX可执行文件格式，体积更小，执行速度更快
3. 常量池采用32位索引值，寻址类方法名，字段名，常量更快
4. 基于寄存器架构，并拥有一套完整的指令系统；
5. 提供了对象生命周期管理，堆栈管理，线程管理，安全和异常管理以及垃圾回收等重要功能；
6. 所有的Android程序都运行在Android系统进程里，每个进程对应着一个Dalvik虚拟机实例。

### 三 Dalvik虚拟机和java虚拟机的区别
1. Java虚拟机运行的是Java字节码，Dalvik虚拟机运行的是Dalvik字节码
   * 传统的java程序经过编译，生成java字节码保存在class文件中，然后java虚拟机通过解码class文件中的内容来运行程序。而Dalvik字节码由java字节码转换而来，并被打包到一个Dex可执行文件中，Dalvik虚拟机通过解释DEX文件来执行这些字节码
2. Dalvik可执行文件体积小，安卓SDK中有一个叫dx的工具负责将java字节码转换为Dalvik字节码
   * dx工具对Java类文件重新排列，消除在类文件中出现的所有冗余信息，避免虚拟机在初始化时出现反复的文件加载与解析过程。
   * 一般java文件中有不同的方法签名，类方法被反复引用、字符串常量大量使用，都是属于冗余信息，影响解析文件的效率
   * dx会消除其中的冗余信息，重新组合形成一个常量池，所有的类文件共享同一个常量池。由于dx工具对常量池的压缩，使得相同的字符串，常量在DEX文件中只出现一次，从而减小了文件的体积。
   * java的class文件存储是一个混合常量池，dex格式文件时特定的不同的常量池，比如字符串常量池、方法池、类池等
   * 总结：dex格式文件就是通过dx工具将多个class文件中公有的部分进行统一存放，去除冗余信息。
3. java虚拟机和Dalvik虚拟机架构不同
   * java虚拟机基于栈架构，程序运行时虚拟机需要频繁的从栈上读取或者写入数据，这个过程需要很多的指令分派和内存访问次数，很消耗cpu时间，不利于像手机这种设备资源有限的设备
   * Dalvik虚拟机基于寄存器架构。数据的访问通过寄存器间直接传递，这样的访问方式比栈快很多。

### 四 Dalvik虚拟机的结构
1. class文件经过dx工具转换为dex文件
2. 类加载器加载原生类和java类
3. 解释器根据指令集对Dalvik字节码进行解释执行
4. 最后，根据dvm_arch参数选择编译的目标机体系结构。

### 五 安卓APK编译打包流程
1. Java编译器对工程本身的java代码进行编译，这些java代码有三个来源：app的源代码，由资源文件生成的R文件(aapt工具)，以及有aidl文件生成的java接口文件(aidl工具)。产出为.class文件。
   * 用AAPT编译R.java文件
   * 编译AIDL的java文件
   * 把java文件编译成class文件
2. class文件和依赖的三方库文件通过dex工具生成Delvik虚拟机可执行的.dex文件，包含了所有的class信息，包括项目自身的class和依赖的class。产出为.dex文件。
3. apkbuilder工具将.dex文件和编译后的资源文件生成未经签名对齐的apk文件。这里编译后的资源文件包括两部分，一是由aapt编译产生的编译后的资源文件，二是依赖的三方库里的资源文件。产出为未经签名的.apk文件。
4. 分别由Jarsigner和zipalign对apk文件进行签名和对齐，生成最终的apk文件。
总结：编译 ——> DEX ——> 打包 ——> 签名和对齐

### 六 ART虚拟机和Dalvik虚拟机的区别
#### 1. 什么是ART
1. ART代表Android Runtime，其处理应用程序执行的方式完全不同于Dalvik，
2. Dalvik是依靠一个Just-In-Time (JIT)编译器去解释字节码。开发者编译后的应用代码需要通过一个解释器在用户的设备上运行，这一机制并不高效，但让应用能更容易在不同硬件和架构上运行 —— 需要一个解释器来解释字节码
3. ART在应用安装时就预编译字节码到机器语言，这一机制叫Ahead-Of-Time (AOT）编译

#### 2. ART优点
1. 系统性能的显著提升
2. 应用启动更快、运行更快、体验更流畅、触感反馈更及时
3. 更长的电池续航能力
4. 支持更低的硬件

#### 3. ART缺点
1. 更大的存储空间占用，大约10%-20%
2. 更长的应用安装时间

#### 4. ART虚拟机相对于Dalvik虚拟机的提升
1. 预编译
   * 使用了AOT直接在安装时将其翻译成机器语言，提高了虚拟机的指令执行速度

2. 垃圾回收机制
2.1 Dalvik的垃圾回收机制
   1. 当gc被触发时候，其会去查找所有活动的对象，这个时候整个程序与虚拟机内部的所有线程就会挂起，这样目的是在较少的堆栈里找到所引用的对象。这个回收动作和应用程序不是并发的、
   2. gc对符合条件的对象进行标记；
   3. gc对标记的对象进行回收；
   4. 恢复所有线程的执行现场继续运行
2.2 好处
    当pause后，gc是快速的，但是如果出现GC频繁并且内存吃紧势必会导致UI卡顿、掉帧、操作不流畅等。
2.3 ART的垃圾回收机制——将其非并发过程改成了部分并发，还有就是对内存的重新分配管理。
   1. gc锁住java堆，扫描并进行标记
   2. 标记完毕后释放掉java堆的锁，并挂起所有线程
   3. gc对标记的对象进行回收
   4. 恢复所有线程的执行现场继续运行
   5. 重复2-4直到结束
2.4 总结
    Dalvik会挂起所有的线程，但是ART中gc会要求程序在分配空间的时候标记自身的堆栈，在标记的时候不会挂起线程，是间断性的挂起的。ART通过标记时机的变更使中断和阻塞的时间更短.

3. 提高内存使用，减少碎片化 
    * Dalvik内存管理特点是:内存碎片化严重，当然这也是Mark and Sweep算法带来的弊端。
    * ART的解决：在ART中，它将Java分了一块空间命名为Large-Object-Space，这块内存空间的引入用来专门存放large object。LOS专供Bitmap使用,los解决了大对象的内存分配和存储问题。
    * ART又引入了moving collector的技术，即将不连续的物理内存块进行对齐。将内存压缩后使内存空间更加紧凑。
    * Large-Object-Space的引入是因为moving collector对大块内存的位移时间成本太高。根官方统计，ART的内存利用率提高10倍了左右，大大提高了内存的利用率。




### <span id = "44">进程间通信方式</span>
### 一 使用intent
1. Activity，Service，Receiver 都支持在 Intent 中传递 Bundle 数据，而 Bundle 实现了 Parcelable 接口，可以在不同的进程间进行传输
2. 在一个进程中启动了另一个进程的 Activity，Service 和 Receiver ，可以在 Bundle 中附加要传递的数据通过 Intent 发送出去。

### 二 使用文件共享
1. Windows 上，一个文件如果被加了排斥锁会导致其他线程无法对其进行访问，包括读和写；而 Android 系统基于 Linux ，使得其并发读取文件没有限制地进行，甚至允许两个线程同时对一个文件进行读写操作，尽管这样可能会出问题
2. 可以在一个进程中序列化一个对象到文件系统中，在另一个进程中反序列化恢复这个对象（注意：并不是同一个对象，只是内容相同
3. SharedPreferences 是个特例，系统对它的读 / 写有一定的缓存策略，即内存中会有一份 ShardPreferences 文件的缓存，系统对他的读 / 写就变得不可靠，当面对高并发的读写访问，SharedPreferences 有很多大的几率丢失数据。因此，IPC 不建议采用 SharedPreferences

### 三 使用Messager
1. Messenger 是一种轻量级的 IPC 方案，它的底层实现是 AIDL ，可以在不同进程中传递 Message 对象，它一次只处理一个请求
2. 服务端进程：服务端创建一个 Service 来处理客户端请求，同时通过一个 Handler 对象来实例化一个 Messenger 对象，然后在 Service 的 onBind 中返回这个 Messenger 对象底层的 Binder 
3. 客户端进程：首先绑定服务端 Service ，绑定成功之后用服务端的 IBinder 对象创建一个 Messenger ，通过这个 Messenger 就可以向服务端发送消息了，消息类型是 Message 。如果需要服务端响应，则需要创建一个 Handler 并通过它来创建一个 Messenger（和服务端一样），并通过 Message 的 replyTo 参数传递给服务端。服务端通过 Message 的 replyTo 参数就可以回应客户端了
4. 客户端和服务端是通过拿到对方的 Messenger 来发送 Message 的。只不过客户端通过 bindService onServiceConnected 而服务端通过 message.replyTo 来获得对方的 Messenger 。Messenger 中有一个 Hanlder 以串行的方式处理队列中的消息。不存在并发执行，因此不用考虑线程同步的问题

### 四 使用ALDL
1. Messenger 是以串行的方式处理客户端发来的消息，如果大量消息同时发送到服务端，服务端只能一个一个处理，所以大量并发请求就不适合用 Messenger ，而且 Messenger 只适合传递消息，不能跨进程调用服务端的方法
2. AIDL 可以解决并发和跨进程调用方法的问题，要知道 Messenger 本质上也是 AIDL ，只不过系统做了封装方便上层的调用而已。
3. 支持的数据类型：string、charsequence、ArrayList、hashmap、parcelable、aldl接口
4. 服务端：服务端创建一个 Service 用来监听客户端的连接请求，然后创建一个 AIDL 文件，将暴露给客户端的接口在这个 AIDL 文件中声明，最后在 Service 中实现这个 AIDL 接口即可
5. 客户端：绑定服务端的 Service ，绑定成功后，将服务端返回的 Binder 对象转成 AIDL 接口所属的类型，然后就可以调用 AIDL 中的方法了。客户端调用远程服务的方法，被调用的方法运行在服务端的 Binder 线程池中，同时客户端的线程会被挂起，如果服务端方法执行比较耗时，就会导致客户端线程长时间阻塞，导致 ANR 。客户端的 onServiceConnected 和 onServiceDisconnected 方法都在 UI 线程中

### 五 使用ContentProvider
1. 用于不同应用间数据共享，和 Messenger 底层实现同样是 Binder 和 AIDL， 系统预置了许多 ContentProvider ，如通讯录、日程表，需要跨进程访问。
2. 使用方法：继承 ContentProvider 类实现 6 个抽象方法，这六个方法均运行在 ContentProvider 进程中，除 onCreate 运行在主线程里，其他五个方法均由外界回调运行在 Binder 线程池中。
3. ContentProvider 的底层数据，可以是 SQLite 数据库，可以是文件，也可以是内存中的数据

### 六 使用Socket
1. 常用的 Socket 类型有两种：流式 Socket（SOCK_STREAM）和数据报式 Socket（SOCK_DGRAM）。流式是一种面向连接的 Socket，针对于面向连接的 TCP 服务应用；数据报式 Socket 是一种无连接的 Socket ，对应于无连接的 UDP 服务应用。
2. Tcp/Ip五层网络模型：应用层、传输层（端到端）、网络层（主机到主机）、数据链路层、物理层
3. socket连接应用层和传输层
4. Socket：
   * 通过，ip定位电脑，端口号定位程序。
   * UDP：封装发送。不安全的通信协议。
   * TCP：持续性输送消息。流。
5. Http和Socket的区别
   * Http用于应用层，无状态协议
   * Socket用于传输层：可以自己定义协议，灵活性更高，实现Client和Server的通信。
6. https：更加安全