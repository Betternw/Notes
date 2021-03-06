## 目录
### * [快捷键](#1)
### * [第一章 杂](#2)
### * [UI基础](#3)     
### * [常用组件](#4)
 * #### [Activity](#5)
 * #### [Menu](#6)
 * #### [Dialog](#7)
 * #### [Fragment](#8)
 * #### [ViewPager](#9)
### * [网络操作](#10)
 * #### [网络操作](#11)
 * #### [Handler通信](#12)
 * #### [AsyncTask异步任务](#13)
### * [高级控件](#14)
 * #### [ListView](#15)
 * #### [CardView](#16)
 * #### [屏幕适配](#17)
### * [数据存储](#18)
 * #### [本地文件操作](#19)
 * #### [数据库操作](#20)
 * #### [手风琴特效](#21)
 * #### [BroadcastReceiver](#22)
 * #### [Application全局应用](#23)
### * [框架](#24)
* #### [OkHttp](#25)
* #### [EventBus](#26)
* #### [RecyclerView](#27)
* #### [Glide](#28)
* #### [GreenDao](#29)
* #### [极光推送](#30)
* #### [WebView浏览器组件](#31)
* #### [ButterKnife实现View注入](#32)
### * [高级应用](#33)
* #### [Service](#34)
* #### [ALDL实现远程服务的通信](#35)
* #### [ContentProvider](#36)
* #### [Socket&Https通信](#37)
### * [项目](#38)
## <span id = "1">快捷键</span>
alt+enter：错误纠正
## <span id = "2">第一章</span>
1. android系统架构：Linux内核层、系统运行库层、应用框架层（API）、应用层）
2. adb指令：
* adb kill-server  杀死服务
* adb stsrt-server   开启服务
* cd 桌面 install xx.apk 快速安装apk
* adb uninstall 包名 快速卸载
* adb shell 进到终端 ls 显示所有目录
* adb push 新文件 /目标文件）
* adb pull 目标文件（写清楚目录结构） 
3. 项目运行：android-manifest.xml 文件中，会对activity进行注册。
4. 不在活动中直接编写界面。在布局中编写界面，然后在活动中引入进来。
5. res目录：drawable放图片。minmap放应用图标。values放字符串、样式、颜色等。layout放布局文件。
6. build.gradle：构建工具版本；项目包名；
7. 日志工具：
* Log.v（）打印嘴琐碎的意义最小的日志信息
* Log.d（“当前类名”，“想要打印的具体的内容”）打印调试信息 debug
* Log.i（）打印重要信息 info
* Log.w（）打印警告信息 warn
* Log.e（）打印错误信息 error
8. 一个简单地案例
* 画ui
* 根据ui写业务逻辑。mainactivity内。
* 加载布局（setContentView）找到控件（findViewById）要转型成本类类型，比如按钮就是Button而不是用View；
* 为按钮设置点击事件setOnClickListener
其中参数是一个接口，因此定义一个类实现方法需要的接口类型。
* 类中定义了点击事件所实现的功能。Toast：提示信息。意图定义拨打电话：设置动作——设置拨打的数据——开启意图
9.  Toast：消息提示.
Toast.makeText（this/父类.this（看传入的对象是在哪个类中），“文本内容”，1或0）.show（）
10. 按钮的四种点击事件
* 内部类：要覆盖重写抽象方法
* 匿名内部类：要覆盖重写抽象方法
* 参数使用this，即this调用的是所在类的对象，本类进行继承onclicklistener接口，重写onclick方法，具体判断是哪个按钮switch（v.getId（））。**适用于多个按钮的情况。**
* button属性，不要id。声明一个方法 方法名和你要点击的按钮在布局中的属性是一样的 android.oncLick=“selfDestruct” public void selfDestruct(**View v**){    } **适用于小demo**
11. 常用布局
* 线性布局：linearout  新建xml文件。id常用命名：例如TextView  写tv_功能
* 相对布局：RelativeLayout  控件默认左上角  指定属性 layout_below/above/toRightof = 某id  指定在某id的下面或者右边
* 帧布局 ： FrameLayout 一层一层显示
* 表格布局：TableLayout  一个tabrow就代表一行
* 绝对布局 AbsoluteLayout
12. 单位介绍
* px：像素，不会随着屏幕大小而改变，不用
* dp：会根据屏幕分辨率进行改变，都用这个
* sp：给一个textview文字设置大小
13. 一个活动对应一个布局。活动与用户界面进行交互。
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
2. 谁启动了新的Activity，就会运行在启动该Activity的栈中。例如：Activity A启动了Activity B，则就会在A所在的栈顶压入一个新的Activity。（5.0后会放入新的栈中，因为会属于不同的程序）
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
1. 设计思想
   * 一个Activity中放置运行多个Fragment，可以根据不同的分辨率进行页面的拆分
   * Fragment必须放在Activity中
2. 生命周期
3. 加载
   * 静态加载：xml文件
   ``` java
   //1. 在MainActiyity中设置点击事件，由MainActivity跳到StaticLoadFragmentActivity
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // find views on onclick event 静态加载
        findViewById(R.id.textView).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // static load fragment.
            startActivity(new Intent(MainActivity.this, StaticLoadFragmentActivity.class));
            }
        });
   ```
   
   
``` java
   //2. 创建StaticLoadFragmentActivity，加载布局
   public class StaticLoadFragmentActivity extends AppCompatActivity{
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_static_load_fragment);
    }
}
```
   
``` java
//3. 创建activity_static_load_fragment，用这个fragment加载一个Activity
<fragment
android:id="@+id/listFragment"
android:name="iom.imooc.fragmentdemo.ListFragment"
android:layout_width="100dp"
android:layout_height="100dp"/>

<fragment
android:id="@+id/detailFragment"
android:name="iom.imooc.fragmentdemo.ListFragment"
android:layout_centerInParent="true"
android:layout_width="100dp"
android:layout_height="100dp"/>
```
    
``` java
//4. 创建ListFragment和fragment_list.layout。每个fragment都可以加载一个fragment_list.layout
    public class ListFragment extends Fragment {
        public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        // 布局文件；当前所在group;绑定当前根布局
        //返回布局渲染的视图view
        View view = inflater.inflate(R.layout.fragment_list, container, false);
        //对控件的处理
        //必须是new出来的根视图view进行id的find
        TextView textView = view.findViewById(R.id.textView);
        textView.setText(mTitle);
        return view;
        }

         <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="TextView"
        android:textColor="#FFFFFF"
        android:textSize="20sp"/>
```  
   * 动态加载：java文件（比如在本页面设置两个fragment）  
    
   ``` java
   // 1.在要加载的页面设置container（用LinearLayout）
       <LinearLayout
        android:orientation="horizontal"
        android:id="@+id/listContainer"
        android:layout_width="150dp"
        android:layout_margin="1dp"
        android:layout_height="match_parent">

    </LinearLayout>

    <LinearLayout
        android:orientation="horizontal"
        android:id="@+id/detailContainer"
        android:layout_width="200dp"
        android:layout_margin="1dp"
        android:layout_height="match_parent">

    </LinearLayout>
   ```
   ```java
   //2. 编写fragment
   public class ListFragment extends Fragment {

   ```
   ```java
   //3. 将fragment装入container中
    // 1. container  2. fragment  3. fragment-->container
        // activity--->fragment value
        ListFragment listFragment = ListFragment.newInstance("list");
        getSupportFragmentManager()
                .beginTransaction()//开启事物，将fragment放在草container中
                .add(R.id.listContainer, listFragment)//add remove replace
                .commit();
        listFragment.setOnTitleClickListener(this);

        ListFragment detail = ListFragment.newInstance("detail");
        getSupportFragmentManager()
                .beginTransaction()
                .add(R.id.detailContainer, detail)//和上面不能是同一个fragment，一个fragment只能加载一个fragment
                .commit();

        detail.setOnTitleClickListener(this);

   ```
4. 传值
* Activity向Fragment传值
``` java
//ListFragment:
    //传值 传入
    public static ListFragment newInstance(String title){
        ListFragment fragment = new ListFragment();
        Bundle bundle = new Bundle();
        bundle.putString(BUNDLE_TITLE, title);
        fragment.setArguments(bundle);
        return fragment;
    }
    //接收数据
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if(getArguments() != null){
            mTitle = getArguments().getString(BUNDLE_TITLE);
        }
    }
     public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_list, container, false);
        TextView textView = view.findViewById(R.id.textView);
        //参数是接收传进的参数
        textView.setText(mTitle);...}
```
```java
//MainActivity:
       //传入对象 list和detail
       ListFragment listFragment = ListFragment.newInstance("list");
        getSupportFragmentManager()
                .beginTransaction()//开启事物，将fragment放在草container中
                .add(R.id.listContainer, listFragment)
                .commit();
        listFragment.setOnTitleClickListener(this);

        ListFragment detail = ListFragment.newInstance("detail");
        getSupportFragmentManager()
                .beginTransaction()
                .add(R.id.detailContainer, detail)//和上面不能是同一个fragment，一个fragment只能加载一个fragment
                .commit();
```
* Fragment向Activity传值——回调接口
   * 步骤：
     1. 类A：定义接口，定义接口类型变量（接受传值并设置事件），定义方法C（设置接口的方法），参数为接口类型变量并为接口变量赋值
     2. 类B继承接口，类A对象调用方法C，参数是this（传值），重写接口方法——最终呈现的效果
     3. 类A目标对象（触发事件的对象）当方法c的对象存在的时候，实现监听器，回调（实现）接口方法，将需要的参数传进去。
     * （接口，观察者，实现者：定义接口，实现者传值并定义方法的最终实现形式，观察者当接收到实现者的对象时，将参数传给方法，方法实现）
```java
//ListFragment:
    // 2. 设置参数为接口类型的函数
    public void setOnTitleClickListener(OnTitleClickListener onTitleClickListener) {
        this.mOnTitleClickListener = onTitleClickListener;
    }

    // 定义变量 接口对象
    private OnTitleClickListener mOnTitleClickListener;

    // 1. 定义接口
    public interface OnTitleClickListener{
        void onClick(String title);
    }

     public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        // 布局文件；当前所在group;绑定当前根布局
        //返回布局渲染的视图view
        View view = inflater.inflate(R.layout.fragment_list, container, false);

        //对控件的处理
        //必须是new出来的根视图view进行id的find
        TextView textView = view.findViewById(R.id.textView);

        //参数是接收传进的参数
        textView.setText(mTitle);

        //5. 回调实现的接口中的方法（textview被点击时）
        textView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(mOnTitleClickListener != null){
                    mOnTitleClickListener.onClick(mTitle);
                }
            }
        });

        return view;
    }
```
```java
//MainActivity:
//3. MainActivity实现接口
 public class MainActivity extends AppCompatActivity  implements ListFragment.OnTitleClickListener{

}
        ListFragment listFragment = ListFragment.newInstance("list");
        getSupportFragmentManager()
                .beginTransaction()//开启事物，将fragment放在草container中
                .add(R.id.listContainer, listFragment)
                .commit();
        //4. 调用（参数为接口类型的）函数，参数为自身
        listFragment.setOnTitleClickListener(this);

        ListFragment detail = ListFragment.newInstance("detail");
        getSupportFragmentManager()
                .beginTransaction()
                .add(R.id.detailContainer, detail)//和上面不能是同一个fragment，一个fragment只能加载一个fragment
                .commit();
        //接口回调
        detail.setOnTitleClickListener(this);


    }

//4 并实现方法
    @Override
    public void onClick(String title) {
        setTitle(title);
    }

```

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
## <span id = "10">网络操作</span>
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
###  <span id = "12"> Handler通信</span>
1. 关于线程
   * 主线程=UI线程
   * new Thread创建子线程
   * 子线程不能更新UI线程的东西，需要使用Handler
   * 消息循环机制：进入主线程后开始循环，处理事件
2. Handler作用
   * 定时任务
   * 在不同的线程之间执行动作
   * MessageQueue是消息队列，存储message和runnable，在线程里，Looper将Message从队列中取出，抽给handler，其中的handlerMessage方法对消息进行处理；或者Looper将runable从队列中取出，直接run。
3. Handler使用
   * 基本实现和消息发送
   创建handler——创建子线程，子线程中handler中做更改内容——通过handler将更改的message发送给主线程——主线程handler接收消息并处理
   ```java
      protected void onCreate(Bundle savedInstanceState) {
        /**
         * UI线程
         */
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final TextView textView = (TextView)findViewById(R.id.tv);

        //1. 创建Handler
        final Handler handler = new Handler(){
            @Override
            //3. 处理消息
            public void handleMessage(Message msg) {
                super.handleMessage(msg);
                /**
                 * 主线程接到子线程发出来的消息，处理
                 */
                //3.1 接收子线程更改消息并处理消息
                Log.d(TAG,"handleMessage:"+msg.what);
                if (msg.what == 1002){
                    textView.setText("imooc");
                    // Log.d(TAG,"handleMessage:"+msg.arg1); // 传入的参数是打包类型

                }

            }
        };

        // 2. button代表子线程运行
        findViewById(R.id.button).setOnClickListener(new View.OnClickListener(){
            @Override
            public void onClick(View v) {
                /**
                 * 子线程
                 */
                //全部在主线程中运行有可能做大量耗时操作
                // 2.1 创建子线程
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            Thread.sleep(2000);
                        }catch (InterruptedException e){
                            e.printStackTrace();
                        }

                        /**
                         * 通知UI更新
                         */
                        // 2.2. 子线程发送消息 参数：消息内容
                        handler.sendEmptyMessage(1001);
                        // 2.2.1 参数是消息对象
                        Message message = Message.obtain();
                        message.what = 1002;
                        message.arg1 = 1003;
                        message.arg2 = 1004;
                        message.obj = MainActivity.this;
                        handler.sendMessage(message);

                        /**
                         * 定时任务
                         */
                        // 2.2.2 消息发送时间——在某事发生后的几秒之后，相对时间
                        handler.sendMessageAtTime(message, SystemClock.uptimeMillis()+3000);
                        // 在两秒之后——绝对时间
                        handler.sendMessageDelayed(message,2000);

                        final Runnable runnable = new Runnable() {

                            @Override
                            public void run() {
                                int a = 1+2+3;
                            }
                        };
                        //2.3 参数是可执行对象 直接运行
                        handler.post(runnable);
                        runnable.run();
                        //2.3.1 定时任务
                        handler.postDelayed(runnable,2000);

                    }
                }).start();


            }
        });

        //不创建子线程直接发送消息
        handler.sendEmptyMessage(1001);


    }
   ```
   * 异步下载文件更新进度条
   ```java
   public class DownloadActivity extends Activity {

    private Handler mHandler;
    public static final int DOWNLOAD_MESSAGE_CODE = 100001;
    public static final int DOWNLOAD_MESSAGE_FAIL_CODE = 100002;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_download);

        final ProgressBar progressBar = (ProgressBar)findViewById(R.id.progressBar);

        /**
         * 主线程 -->start
         * 点击按钮 |
         * 发起下载 |
         * 开启子线程做下载 |
         * 下载完成后通知主线程 | -->主线程更新进度条
         */
        findViewById(R.id.button2).setOnClickListener(new View.OnClickListener() {
            @Override
            // 1. 点击按钮发起事件，并创建子线程
            public void onClick(View v) {
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        // 2.发起下载
                        download("http://download.sj.qq.com/upload/connAssitantDownload/upload/MobileAssistant_1.apk");
                    }
                }).start();

            }
        });

        // 2.6 创建handler并发送消息
        mHandler =  new Handler(){
            @Override
            public void handleMessage(Message msg) {
                super.handleMessage(msg);

                switch (msg.what){
                    case DOWNLOAD_MESSAGE_CODE:
                        if(msg.obj!=null)
                            //进度条进度设置
                        progressBar.setProgress((Integer) msg.obj);
                        break;
                    case DOWNLOAD_MESSAGE_FAIL_CODE:


                }
            }
        };
    }

    // 2.发起下载
    //3. 设置下载权限和读写文件权限 在manifest中
        private void download(String appUrl){
            try {
                URL url = new URL(appUrl);
                //2.1 开启连接
                URLConnection urlConnection = url.openConnection();
                //2.2 获得文件流
                InputStream inputStream = urlConnection.getInputStream();
                /**
                 * 获取文件的总长度
                 */
                int contentLength = urlConnection.getContentLength();
                //获取下载内容的存储地址  File.separator表示斜杠  放在imooc目录下
                String downloadFolderName = Environment.getExternalStorageDirectory()
                        + File.separator+"imooc"+File.separator;
                //2.3 创建文件
                File file = new File(downloadFolderName);
                if (!file.exists()){
                    file.mkdir();
                }
                String fileName = downloadFolderName + "imooc.apk";
                File apkFile = new File(fileName);
                if (apkFile.exists()){
                    apkFile.delete();
                }
                //下载进度
                int downloadSize = 0;
                byte[] bytes = new  byte[1024];

                int length = 0;
                //2.4 输入流读出数据，输出流将数据写入文件
                // 读出进度
                OutputStream outputStream = new FileOutputStream(fileName);
                while ((length = inputStream.read(bytes)) != -1){
                    outputStream.write(bytes,0,length);
                    downloadSize += length;
                    /**
                     * update UI
                     */
                    // 2.5 通过message发送更改内容，handler发送给主线程
                    Message message = Message.obtain();
                    message.obj = downloadSize * 100 / contentLength;
                    message.what = DOWNLOAD_MESSAGE_CODE;
                    // 2.6 创建handler并发送消息
                    mHandler .sendMessage(message);
                }
                //2.7 关闭流
                inputStream.close();
                outputStream.close();
            }catch (MalformedURLException e){
                //2.8 下载失败消息
                notifyDownloadFaild();
                e.printStackTrace();
            }catch (IOException e){
                notifyDownloadFaild();
                e.printStackTrace();
            }
    }

    //3. 设置下载权限和写文件权限
    private  void  notifyDownloadFaild(){
        Message message = Message.obtain();
        message.what = DOWNLOAD_MESSAGE_FAIL_CODE;
        mHandler .sendMessage(message);
    }
   ```
   ```java
       //3. 设置下载权限和读写文件权限
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
   ```
   * 实现倒计时并优化内存泄露
   ```java
   public class MainActivity extends AppCompatActivity {
    /**
     * 倒计时标记handle code
     */
    public  static  final  int COUNTDOWN_TIME_CODE = 100001;
    /**
     * 倒计时间隔
     */
    public  static  final  int  DELAY_MILLIS = 1000;
    /**
     * 倒计时最大值
     */
    public  static  final  int  MAX_COUNT = 10;
    private TextView mCountdownTimeTextView;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //1. 得到控件
        mCountdownTimeTextView= (TextView)findViewById(R.id.countdownTimeTextView);

        //2. 创建了一个handler,写成静态的handler
        CountdownTimeHandler handler = new CountdownTimeHandler(this);

        //3. 新建了一个message
        Message message = Message.obtain();
        message.what = COUNTDOWN_TIME_CODE;
        message.arg1 = MAX_COUNT;

        //3.1 第一次发送这个message 延迟发送消息
        handler.sendMessageDelayed(message,DELAY_MILLIS);

    }

    //2.1
    public  static  class  CountdownTimeHandler extends Handler{
          static  final  int  MIN_COUNT = 0;
          //2.2 建立弱引用
        final WeakReference<MainActivity> mWeakReference;

         CountdownTimeHandler(MainActivity activity){
            mWeakReference = new WeakReference<> (activity );
        }

        @Override
        // 2.2 接收信息并处理
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            //通过弱引用拿到activity
            MainActivity activity =mWeakReference.get();

            switch (msg.what){
                case COUNTDOWN_TIME_CODE:
                    int value = msg.arg1;
                    //通过弱引用的activity拿到textview，并设置值
                    activity.mCountdownTimeTextView.setText(String.valueOf(value --));

                     //循环发的消息控制
                    if (value >= MIN_COUNT) {
                        //再建立一个message，将更改后的数字这一消息发出，从而实现循环的消息控制
                        Message message = Message.obtain();
                        message.what = COUNTDOWN_TIME_CODE;
                        message.arg1 = value;
                        sendMessageDelayed(message, DELAY_MILLIS);
                    }

                    break;
            }
        }
    }

   ```
   * 打地鼠demo
   ```java
   public class DiglettActivity extends AppCompatActivity implements View.OnClickListener,View.OnTouchListener{

    public  static final int CODE = 123;

    private TextView mResultTextView;
    private ImageView mDiglettImageView;
    private  Button mStarrButton;

    public int[][] mPosition = new  int[][]{
            {342,180},{432,880},
            {521,256},{429,780},
            {456,976},{145,665},
            {123,678},{564,567},
    };

    private  int mTotalCount;
    private  int mSuccessCount;

    public static final int MAX_COUNT = 10;

    private DiglettHandler mHandler = new DiglettHandler(this);

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_diglett);

        //1. 初始化控件
        initView();

        setTitle("打地鼠");

    }
    //1. 初始化控件
    private void initView(){
        mResultTextView = (TextView)findViewById(R.id.text_view);
        mDiglettImageView = (ImageView)findViewById(R.id.image_view);
        mStarrButton = (Button)findViewById(R.id.start_button);

        mStarrButton.setOnClickListener(this);
        mDiglettImageView.setOnTouchListener(this);
    }

    @Override
    // 2. 设置点击事件
    public void onClick(View v){
        switch (v.getId()){
            case  R.id.start_button:
                start();
                break;
        }

    }
    //3. 设置提示
    private  void  start(){
        //发送消息 handler.sendmessagedelayer
        mResultTextView.setText("开始啦");
        mStarrButton.setText("游戏中.....");
        mStarrButton.setEnabled(false);
        next(0);
    }

    //5. 设置下一个的产生
    private  void  next(int delayTime){
        int position = new Random().nextInt(mPosition.length);

        Message message = Message.obtain();
        message.what =CODE;
        message.arg1 = position;

        //通过handler将下一个的消息发出
        mHandler.sendMessageDelayed(message,delayTime);
        mTotalCount ++;
    }

    @Override
    //5. 设置点击
    public boolean onTouch(View v, MotionEvent event) {
        v.setVisibility(View.GONE);
        mSuccessCount ++ ;
        mResultTextView.setText("打到了"+ mSuccessCount +"只，共" + MAX_COUNT +"只.");
        return false;
    }

    //4. 设置handler，接收到消息后进行位置更改，将更改信息通过handler传给下一个下一个位置值。
    public static class DiglettHandler extends Handler{
        public static final int RANDOM_NUBER = 500;
        public  final WeakReference<DiglettActivity> mWeakReference;

        public  DiglettHandler(DiglettActivity activity){
            mWeakReference = new WeakReference<>(activity);

        }

        @Override
        //接受到要产生下一个的消息，设置下一个的属性
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            DiglettActivity activity = mWeakReference.get();

            switch (msg.what){
                case CODE:
                    if (activity.mTotalCount >MAX_COUNT){
                        activity.clear();
                        Toast.makeText(activity,"地鼠打完了！",Toast.LENGTH_LONG).show();
                        return;
                    }

                    int position = msg.arg1;
                    activity.mDiglettImageView.setX(activity.mPosition[position][0]);
                    activity.mDiglettImageView.setY(activity.mPosition[position][1]);
                    activity.mDiglettImageView.setVisibility(View.VISIBLE);

                    int randomTime = new Random().nextInt(RANDOM_NUBER) + RANDOM_NUBER;

                    activity.next(randomTime);
                    break;
            }
        }
    }
    // 4.1 设置清除
    private  void clear(){
        mTotalCount = 0;
        mSuccessCount = 0;
        mDiglettImageView.setVisibility(View.GONE);
        mStarrButton.setText("点击开始");
        mStarrButton.setEnabled(true);

    }
   ```
###  <span id = "13"> AsyncTask异步任务</span> 
1. 同步任务：一步一步进行  异步任务：同时进行，互不干扰，多个线程
2. 线程、多线程
   * ANR（Application Not Responding）：应用程序无响应
   * 耗时操作时使用多线程
   * 多线程操作UI事件，其他线程处理后到主线程进行展示
   * 线程具有安全问题
   * 子线程更新主线程的东西使用handler，将要更改的信息传给主线程
3. AsyncTask
   * 目的：方便在后台线程操作后更新UI
   * Thread和Handler进行封装
   * 实质：Handler异步消息处理机制
   * 参数都是泛型
   * 常用方法
      * 执行前——onPreExecute()  主线程
      * 执行中—— doInBackground 另一个线程，抛出进度
      * 执行后——onPostExecute 主线程
      * 处理进度——onProgressUpdate 主线程
4. 实现下载功能
   * 网络上请求数据：申请网络权限 读写存储权限
   ```java
       <uses-permission android:name="android.permission.INTERNET"/>
       <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
       <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
   ```
   * 布局layout
   * 下载之前：UI
   * 下载中：数据
   * 下载后：UI
   ```java
   public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";
    public static final int INIT_PROGRESS = 0;
    public static final String APK_URL = "http://download.sj.qq.com/upload/connAssitantDownload/upload/MobileAssistant_1.apk";
    public static final String FILE_NAME = "imooc.apk";
    private ProgressBar mProgressBar;
    private Button mDownloadButton;
    private TextView mResultTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 1. 初始化视图
        initView();

        // 2. 设置点击监听
        setListener();

        // 3. 初始化UI数据
        setData();

        /*DownloadHelper.download(APK_URL, "", new DownloadHelper.OnDownloadListener.SimpleDownloadListener() {
            @Override
            public void onSuccess(int code, File file) {

            }

            @Override
            public void onFail(int code, File file, String message) {

            }

            @Override
            public void onStart() {
                super.onStart();
            }

            @Override
            public void onProgress(int progress) {
                super.onProgress(progress);
            }
        });*/


    }

    /**
     * 初始化视图
     */
    //1. 初始化视图
    private void initView() {

        mProgressBar = (ProgressBar) findViewById(R.id.progressBar);
        mDownloadButton = (Button) findViewById(R.id.button);
        mResultTextView = (TextView) findViewById(R.id.textView);

    }

    //2. 设置点击监听
    private void setListener() {

        mDownloadButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // TODO: 16/12/19 下载任务
                //5.1 传参
                DownloadAsyncTask asyncTask = new DownloadAsyncTask();
                asyncTask.execute(APK_URL);
            }
        });
    }

    // 3.初始化数据
    private void setData() {

        mResultTextView.setText(R.string.download_text);
        mDownloadButton.setText(R.string.click_download);
        mProgressBar.setProgress(INIT_PROGRESS);

    }


    /**
     * String 入参
     * Integer 进度
     * Boolean 返回值
     */
    public class DownloadAsyncTask extends AsyncTask<String, Integer, Boolean> {
        String mFilePath;
        /**
         * 在异步任务之前，在主线程中
         */
        @Override
        // 4 下载前操作
        protected void onPreExecute() {
            super.onPreExecute();
            // 可操作UI  类似淘米,之前的准备工作
            mDownloadButton.setText(R.string.downloading);
            mResultTextView.setText(R.string.downloading);
            mProgressBar.setProgress(INIT_PROGRESS);
        }

        /**
         * 在另外一个线程中处理事件
         *
         * @param params 入参  煮米
         * @return 结果
         */
        @Override
        // 5.下载中 另外一个线程处理
        // 5.1 传参
        protected Boolean doInBackground(String... params) {
            if(params != null && params.length > 0){
                String apkUrl = params[0];

                try {
                    // 构造URL
                    URL url = new URL(apkUrl);
                    // 构造连接，并打开
                    URLConnection urlConnection = url.openConnection();
                    InputStream inputStream = urlConnection.getInputStream();

                    // 获取了下载内容的总长度
                    int contentLength = urlConnection.getContentLength();

                    // 下载地址准备
                    mFilePath = Environment.getExternalStorageDirectory()
                            + File.separator + FILE_NAME;

                    // 对下载地址进行处理
                    File apkFile = new File(mFilePath);
                    if(apkFile.exists()){
                        boolean result = apkFile.delete();
                        if(!result){
                            return false;
                        }
                    }

                    // 已下载的大小
                    int downloadSize = 0;

                    // byte数组
                    byte[] bytes = new byte[1024];

                    int length;

                    // 创建一个输入管道
                    OutputStream outputStream = new FileOutputStream(mFilePath);

                    // 不断的一车一车挖土,走到挖不到为止
                    while ((length = inputStream.read(bytes)) != -1){
                        // 挖到的放到我们的文件管道里
                        outputStream.write(bytes, 0, length);
                        // 累加我们的大小
                        downloadSize += length;
                        // 发送进度  每次下载都更新进度
                        publishProgress(downloadSize * 100/contentLength);
                    }

                    inputStream.close();
                    outputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                    return false;
                }
            } else {
                return false;
            }

            return true;
        }

        // 6 下载后操作
        @Override
        protected void onPostExecute(Boolean result) {
            super.onPostExecute(result);
            // 也是在主线程中 ，执行结果 处理
            mDownloadButton.setText(result? getString(R.string.download_finish) : getString(R.string.download_finish));
            mResultTextView.setText(result? getString(R.string.download_finish) + mFilePath: getString(R.string.download_finish));

        }

        // 7 下载进度处理
        @Override
        protected void onProgressUpdate(Integer... values) {
            super.onProgressUpdate(values);
            // 收到进度，然后处理： 也是在UI线程中。
            if (values != null && values.length > 0) {
                mProgressBar.setProgress(values[0]);
            }
        }

    }

   ```
5. 封装方法
   * 创建调用类进行封装
      * 回调：封装类中定义接口（要执行的方法）、定义下载方法、执行异步任务（参数为接口），然后执行类使用封装类的对象调用异步任务，参数有接口，重写接口允许的方法。
      ```java
        public class DownloadHelper {


        // 执行异步任务 excute方法
        public static void download(String url, String localPath, OnDownloadListener listener){
            DownloadAsyncTask task = new DownloadAsyncTask(url, localPath, listener);
            task.execute();
        }


        public static class DownloadAsyncTask extends AsyncTask<String, Integer, Boolean> {

            String mUrl;
            String mFilePath;
            OnDownloadListener mListener;

            public DownloadAsyncTask(String url, String filePath, OnDownloadListener listener) {
                mUrl = url;
                mFilePath = filePath;
                mListener = listener;
            }

            /**
            * 在异步任务之前，在主线程中
            */
            @Override
            //1. 执行前
            protected void onPreExecute() {
                super.onPreExecute();
                // 可操作UI  类似淘米,之前的准备工作
                if(mListener != null){
                    mListener.onStart();
                }
            }

            /**
            * 在另外一个线程中处理事件
            *
            * @param params 入参  煮米
            * @return 结果
            */
            @Override
            //2.执行中， 在另外一个线程中处理事件 String... params可变参数，数量任意
            //抛出进度
            //在异步线程处理，其余都是主线程
            protected Boolean doInBackground(String... params) {
                    String apkUrl = mUrl;

                    try {
                        // 构造URL
                        URL url = new URL(apkUrl);
                        // 构造连接，并打开
                        URLConnection urlConnection = url.openConnection();
                        InputStream inputStream = urlConnection.getInputStream();

                        // 获取了下载内容的总长度
                        int contentLength = urlConnection.getContentLength();

                        // 对下载地址进行处理
                        File apkFile = new File(mFilePath);
                        if(apkFile.exists()){
                            boolean result = apkFile.delete();
                            if(!result){
                                if(mListener != null){
                                    mListener.onFail(-1, apkFile, "文件删除失败");
                                }
                                return false;
                            }
                        }

                        // 已下载的大小
                        int downloadSize = 0;

                        // byte数组
                        byte[] bytes = new byte[1024];

                        int length;

                        // 创建一个输入管道
                        OutputStream outputStream = new FileOutputStream(mFilePath);

                        // 不断的一车一车挖土,走到挖不到为止
                        while ((length = inputStream.read(bytes)) != -1){
                            // 挖到的放到我们的文件管道里
                            outputStream.write(bytes, 0, length);
                            // 累加我们的大小
                            downloadSize += length;
                            // 发送进度
                            publishProgress(downloadSize * 100/contentLength);
                        }

                        inputStream.close();
                        outputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                        if(mListener != null){
                            mListener.onFail(-2, new File(mFilePath), e.getMessage());
                        }
                        return false;
                    }

                if(mListener != null){
                    mListener.onSuccess(0, new File(mFilePath));
                }
                return true;
            }

            @Override
            // 3. 执行后
            protected void onPostExecute(Boolean result) {
                super.onPostExecute(result);
                // 也是在主线程中 ，执行结果 处理
            }

            @Override
            //4. 处理进度
            protected void onProgressUpdate(Integer... values) {
                super.onProgressUpdate(values);
                // 收到进度，然后处理： 也是在UI线程中。
                if (values != null && values.length > 0) {
                    if(mListener != null){
                        mListener.onProgress(values[0]);
                    }
                }

            }

        }


        public interface OnDownloadListener{

            void onStart();

            void onSuccess(int code, File file);

            void onFail(int code , File file, String message);

            void onProgress(int progress);


            //内部抽象类，其中定义的方法不是必须实现的，可以简化代码
            abstract class SimpleDownloadListener implements OnDownloadListener{
                @Override
                public void onStart() {

                }

                @Override
                public void onProgress(int progress) {}}}}

                
      ```
   * 调用封装方法
   ```java
   DownloadHelper.download(APK_URL, "", new DownloadHelper.OnDownloadListener.SimpleDownloadListener() {
            @Override
            public void onSuccess(int code, File file) {

            }

            @Override
            public void onFail(int code, File file, String message) {

            }

            @Override
            public void onStart() {
                super.onStart();
            }

            @Override
            public void onProgress(int progress) {
                super.onProgress(progress);
            }
        });
   ```
6. 注意
   * 主线程执行excute（）方法
   * Task实例必须在UI进程创建 
   * 注意防止内存泄漏——弱引用       
## <span id = "14">高级控件</span>
###  <span id = "15"> ListView</span>
1. 由多个item组成，每个item都是一个视图
2. Layout中设置ListView，和普通控件一样
3. adpter将数据适配到每个item中
4. 基本实现ListView
   * 在Layout中创建ListView
   * 创建每一行的layout
   * 创建每一行的数据
   * 用adapter将数据填充到每一行的视图中（adpter和数据，adapter和视图）
   * 优化缓存和读取速度
   ```java
   public class AppListActivity extends AppCompatActivity{

    private ListView mListView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // 1.得到控件
        // 2. 创建item的layout
        mListView = (ListView) findViewById(R.id.list_view_demo);
        //6. 创建头部图片Layout并转换为view
        LayoutInflater layoutInflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        View headerView = layoutInflater.inflate(R.layout.header_list_demo, null);
        // 7.header视图添加view
        mListView.addHeaderView(headerView);
        List<ResolveInfo> infos = getAppInfos();
        //5. 将数据和adapter连接
        mListView.setAdapter(new AppListAdapter(this, infos));
    }

    // 3. 获取所有数据 应用信息
    private List<ResolveInfo> getAppInfos() {
        Intent mainIntent = new Intent(Intent.ACTION_MAIN, null);
        mainIntent.addCategory(Intent.CATEGORY_LAUNCHER);
        return getPackageManager().queryIntentActivities(mainIntent, 0);
    }

    // 4. 创建adapter  将数据和视图进行适配
    public class AppListAdapter extends BaseAdapter{

        private Context mContext;
        //要填充的数据列表
        private List<ResolveInfo> mInfos;

        //构造器
        public AppListAdapter(Context context, List<ResolveInfo> infos) {
            mContext = context;
            mInfos = infos;
        }

        @Override
        // 数量
        public int getCount() {

            return mInfos.size();
        }

        @Override
        //返回当前position位置的这一条
        public Object getItem(int position) {

            return mInfos.get(position);
        }

        @Override
        //返回当前position位置的这一条的ID
        public long getItemId(int position) {

            return position;
        }

        @Override
        //处理view-data 填充数据的一个过程
        public View getView(final int position, View convertView, ViewGroup parent) {
            ViewHolder viewHolder;
            //将layout文件读成java的view文件
            LayoutInflater layoutInflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            if(convertView == null){
                viewHolder = new ViewHolder();
                //将layout读出 变成view文件
                convertView = layoutInflater.inflate(R.layout.item_demo_list, null);
                // 获取控件
                viewHolder.nameTextView = (TextView) convertView.findViewById(R.id.title_text_view);
                viewHolder.avatarImageView = (ImageView) convertView.findViewById(R.id.icon_image_view);
                //8.1  将view与holder进行绑定
                convertView.setTag(viewHolder);
            } else {
                //8.2 当view不为空，之前读过，曾保存在holder中，就可以直接读出view。
                //不需要每次读取view的时候都新建控件
                viewHolder = (ViewHolder) convertView.getTag();
            }
            // 将控件和数据之间进行绑定，设置控件的值
            viewHolder.nameTextView.setText(mInfos.get(position).activityInfo.loadLabel(mContext.getPackageManager()));
            viewHolder.avatarImageView.setImageDrawable(mInfos.get(position).activityInfo.loadIcon(mContext.getPackageManager()));

            //点击事件 设置点击进item后获取系统信息
            convertView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    ResolveInfo info = mInfos.get(position);

                    //该应用的包名
                    String pkg = info.activityInfo.packageName;
                    //应用的主activity类
                    String cls = info.activityInfo.name;

                    ComponentName componet = new ComponentName(pkg, cls);

                    Intent intent = new Intent();
                    intent.setComponent(componet);
                    startActivity(intent);
                }
            });
            return convertView;
        }

        // 8. 优化
        class ViewHolder {
            ImageView avatarImageView;
            TextView nameTextView;
        }
    }
   ```
5. 网络下载
```java
public class RequestDataActivity  extends AppCompatActivity {
    private ListView mListView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //1.获取view
        //2. 设置适配器（需要数据和item view）
        mListView = (ListView) findViewById(R.id.list_view_demo);
        //7 底部图片设置
        LayoutInflater layoutInflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        View view = layoutInflater.inflate(R.layout.header_list_demo, null);
        mListView.addFooterView(view);

        // 6 异步运行
        new AppAsyncTask().execute();
    }

    // 4. 创建Adapter
    public class AppListAdapter extends BaseAdapter {

        private Context mContext;
        private List<LessonInfo> mInfos;

        public AppListAdapter(Context context, List<LessonInfo> infos) {
            mContext = context;
            mInfos = infos;
        }

        @Override
        public int getCount() {
            return mInfos.size();
        }

        @Override
        public Object getItem(int position) {
            return mInfos.get(position);
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        @Override
        public View getView(final int position, View convertView, ViewGroup parent) {
            ViewHolder viewHolder;
            LayoutInflater layoutInflater = (LayoutInflater)getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            if(convertView == null){
                viewHolder = new ViewHolder();
                convertView = layoutInflater.inflate(R.layout.item_demo_list, null);
                // 获取控件
                viewHolder.nameTextView = (TextView) convertView.findViewById(R.id.title_text_view);
                viewHolder.avatarImageView = (ImageView) convertView.findViewById(R.id.icon_image_view);
                convertView.setTag(viewHolder);
            } else {
                viewHolder = (ViewHolder) convertView.getTag();
            }
            // 和数据之间进行绑定
            viewHolder.nameTextView.setText(mInfos.get(position).getName());
            viewHolder.avatarImageView.setVisibility(View.GONE);

            return convertView;
        }

        class ViewHolder {
            ImageView avatarImageView;
            TextView nameTextView;
        }
    }



    // 3. 异步获取并处理网络数据

    public class AppAsyncTask extends AsyncTask<Void,Integer, String> {

        @Override
        // 3.1 请求数据——request方法
        protected String doInBackground(Void... params) {

            return request("http://www.imooc.com/api/teacher?type=2&page=1");
        }

        @Override
        // 3.2 处理数据
        protected void onPostExecute(String result) {
            super.onPostExecute(result);
            //创建数据集对象
            LessonResult lessonResult = new LessonResult();
            //将json对象的数据放入数据集对象中
            try {
                JSONObject jsonObject = new JSONObject(result);
                int status = jsonObject.getInt("status");
                String message = jsonObject.getString("msg");
                lessonResult.setStatus(status);
                lessonResult.setMessage(message);

                List<LessonInfo> lessonInfos = new ArrayList<>();
                JSONArray dataArray = jsonObject.getJSONArray("data");
                for (int i = 0; i < dataArray.length(); i++) {
                    LessonInfo lessonInfo = new LessonInfo();
                    JSONObject tempJsonObject = (JSONObject) dataArray.get(i);
                    lessonInfo.setID(tempJsonObject.getInt("id"));
                    lessonInfo.setName(tempJsonObject.getString("name"));
                    lessonInfos.add(lessonInfo);
                }
                lessonResult.setLessonInfos(lessonInfos);

            } catch (JSONException e) {
                e.printStackTrace();
            }
            //5 将数据传入adapter中
            mListView.setAdapter(new AppListAdapter(RequestDataActivity.this, lessonResult.getLessonInfos()));
        }


        // 3.1.1 请求数据方法
        private String request(String urlString) {
            try {
                URL url = new URL(urlString);
                HttpURLConnection connection = (HttpURLConnection) url.openConnection();
                connection.setConnectTimeout(30000);//超时时间
                connection.setRequestMethod("GET");  // GET POST
                connection.connect();
                int responseCode = connection.getResponseCode();//状态码
                String responseMessage = connection.getResponseMessage();
                String result = null;
                if(responseCode == HttpURLConnection.HTTP_OK){
                    InputStreamReader inputStreamReader = new InputStreamReader(connection.getInputStream());
                    BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
                    StringBuilder stringBuilder = new StringBuilder();
                    String line;
                    //读取数据 当不为空  就继续读下一行
                    while ((line = bufferedReader.readLine()) != null) {
                        stringBuilder.append(line);
                    }
                    result = stringBuilder.toString();
                } else {
                  // TODO:
                }
                return result;
            } catch (IOException e) {
                e.printStackTrace();
            }
            return null;
        }
    }

}

```
6. 引用不同的行布局——根据行类型进行判断
```java
public class ChatActivity extends AppCompatActivity {

    private ListView mListView;
    List<ChatMessage> mChatMessages = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mListView = (ListView) findViewById(R.id.list_view_demo);
        // 2. 创建信息类
        ChatMessage chatMessage = new ChatMessage(1,2,"刘小明","8:20","你好吗","","",true);
        ChatMessage chatMessage2 = new ChatMessage(2,1,"小军","8:21","我很好","","",false);
        ChatMessage chatMessage3 = new ChatMessage(1,2,"刘小明","8:22","今天天气怎么样","","",true);
        ChatMessage chatMessage4 = new ChatMessage(2,1,"小军","8:23","热成狗了","","",false);

        mChatMessages.add(chatMessage);
        mChatMessages.add(chatMessage2);
        mChatMessages.add(chatMessage3);
        mChatMessages.add(chatMessage4);

        // 3. 将信息列表数据放入适配器
        mListView.setAdapter(new ChatMessageAdapter(this, mChatMessages));
    }

    // 1. 适配器
    public static class ChatMessageAdapter extends BaseAdapter {

        //信息数据类型 发出的和接收的
        public interface IMessageViewType {
            int COM_MESSAGE = 0;
            int TO_MESSAGE = 1;
        }

        private List<ChatMessage> mChatMessages;
        private LayoutInflater mInflater;

        //构造器
        public ChatMessageAdapter(Context context, List<ChatMessage> coll) {
            this.mChatMessages = coll;
            mInflater = LayoutInflater.from(context);
        }

        public int getCount() {
            return mChatMessages.size();
        }

        public Object getItem(int position) {
            return mChatMessages.get(position);
        }

        public long getItemId(int position) {
            return position;
        }

        //视图类型 根据不同的type（两种）返回不同的视图
        public int getItemViewType(int position) {
            ChatMessage entity = mChatMessages.get(position);
            if (entity.getMsgType()) {
                return IMessageViewType.COM_MESSAGE;
            } else {
                return IMessageViewType.TO_MESSAGE;
            }

        }

        public int getViewTypeCount() {
            return 2;
        }

        //根据当前类型生成不同视图
        public View getView(int position, View convertView, ViewGroup parent) {

            final ChatMessage entity = mChatMessages.get(position);
            boolean isComMsg = entity.getMsgType();

            ViewHolder viewHolder;
            if (convertView == null) {
                //根据不同的消息类型产生不同的视图
                if (isComMsg) {
                    convertView = mInflater.inflate(R.layout.chatting_item_msg_text_left, null);
                } else {
                    convertView = mInflater.inflate(R.layout.chatting_item_msg_text_right, null);
                }
                //根据视图得到控件，并设置数据
                viewHolder = new ViewHolder();
                viewHolder.mSendTime = (TextView) convertView.findViewById(R.id.tv_send_time);
                viewHolder.mUserName = (TextView) convertView.findViewById(R.id.tv_username);
                viewHolder.mContent = (TextView) convertView.findViewById(R.id.tv_chat_content);
                viewHolder.mTime = (TextView) convertView.findViewById(R.id.tv_time);
                viewHolder.mUserAvatar = (ImageView) convertView.findViewById(R.id.iv_user_head);
                viewHolder.mIsComMessage = isComMsg;
                convertView.setTag(viewHolder);
            } else {
                viewHolder = (ViewHolder) convertView.getTag();
            }

            viewHolder.mSendTime.setText(entity.getDate());
            viewHolder.mContent.setText(entity.getContent());
            viewHolder.mContent.setCompoundDrawablesWithIntrinsicBounds(0, 0, 0, 0);
            viewHolder.mTime.setText("");
            viewHolder.mUserName.setText(entity.getName());
            if (isComMsg) {
                viewHolder.mUserAvatar.setImageResource(R.drawable.avatar);
            } else {
                viewHolder.mUserAvatar.setImageResource(R.mipmap.ic_launcher);
//                ImageLoader.getInstance().displayImage(entity.getAvatarUrl(), viewHolder.mUserAvatar);
            }

            return convertView;
        }

        class ViewHolder {
            public TextView mSendTime;
            public TextView mUserName;
            public TextView mContent;
            public TextView mTime;
            public ImageView mUserAvatar;
            public boolean mIsComMessage = true;
        }
    }
```
###  <span id = "16"> CardView</span>
1. 使用时需要导入包，继承自FrameLayout
* File——project structure——dependcies—— + library dependency——cardview 记得调整版本
* build.gradle中：
```java
    implementation 'com.android.support:cardview-v7:26.1.0'
```
2. 常用属性
   * cardBackgroundColor：背景色
   * cardCoornerRadius：设置圆角半径
   * contentPadding：设置内部padding
   * cardElevation：设置阴影大小
   * cardUseCompatPadding：默认为false，true 添加额外的padding绘制阴影
   * cardPreventCornerOverlap：默认为true，添加额外的padding，防止内容和圆角重叠
3. 使用
   * 在控件外添加： android.support.v7.widget.CardView
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
###  <span id = "21">手风琴特效——ExpandableListView</span>
1. 基本介绍  ExpandableListView-Imooc
   * 和ListView区别
      * 继承自ListView
      * 可扩展可收缩可分组
   * 应用于按照关键字进行分组，比如联系人列表等
2. 常用属性
   * groupIndicator：分组指示器 一个item 
   * childIndicator
   * childDivider：设置分割线
3. 常用方法
   * setAdapter：适配器
   * setOnGroupClickListener：点击分组条目的回调
   * setOnChildClickListener：点击childItem的回调
   * setOnGroupCollapseListener：点击group收缩的回调
   * setOnGroupExpandListener：点击group展开的回调
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
###  <span id = "28">Glide 第三方组件加载库</span>
1. 加载图片的种类
   * 资源文件中的图片
   * 手机中的图片
   * 网络中的图片
2. 加载图片的步骤
   * 明确图片的地址
   * 将图片转换为可被加载的对象
   * 通过图片加载控件展示图片
3. 图片加载库
4. 使用Glide进行加载
```java
  /**
     * 使用Glide加载网络图片
     * @param img 网络图片地址
     */
    private void glideLoadImage (String img) {
//      4. 通过 RequestOptions 对象来设置Glide的配置
        RequestOptions options = new RequestOptions()
//                设置图片变换为圆角
                .circleCrop()
//                设置站位图
                .placeholder(R.mipmap.loading)
//                设置加载失败的错误图片
                .error(R.mipmap.loader_error);

//      1. Glide.with 会创建一个图片的实例，接收 Context、Activity、Fragment
        Glide.with(this)
//                2. 指定需要加载的图片资源，接收 Drawable对象、网络图片地址、本地图片文件、资源文件、二进制流、Uri对象等等
                .load(img)
//                指定配置
                .apply(options)
//                3. 用于展示图片的ImageView
                .into(mIv);
    }
```
5. Generated API全局配置——为glide提供自定义选项，将配置打包。
   * 引入Generated API支持库
   * 创建类，继承AppGlideModule并添加@GlideModule注解
   ```java
   @GlideModule
    public class MyAppGlideModule extends AppGlideModule {
    }

   ```
   * 创建类，添加@GlideExtension注解，并实现private构造函数
   ```java
   
    @GlideExtension
    public class MyGlideExtension{

        /**
        * 实现private的构造函数
        */
        private MyGlideExtension() {
        }

        @GlideOption
        public static void injectOptions (RequestOptions options) {
            options
    //                设置图片变换为圆角
                    .circleCrop()
    //                设置站位图
                    .placeholder(R.mipmap.loading)
    //                设置加载失败的错误图片
                    .error(R.mipmap.loader_error);

        }
    }
   ```
   ```java
     private void glideAppLoadImage (String img) {
        /**
         * 不想每次都通过 .apply(options) 的方式来进行配置的时候，可以使用GlideApp的方式来进行全局统一的配置
         * 需要注意以下规则：
         * 1、引入 repositories {mavenCentral()}  和 dependencies {annotationProcessor 'com.github.bumptech.glide:compiler:4.8.0'}
         * 2、集成 AppGlideModule 的类并且通过 @GlideModule 进行了注解
         * 3、有一个使用了 @GlideExtension 注解的类 MyGlideExtension，并实现private的构造函数
         * 4、在 MyGlideExtension 可以通过被 @GlideOption 注解了的静态方法来添加可以被GlideApp直接调用的方法，该方法默认接受第一个参数为：RequestOptions
         */
            GlideApp.with(this)
                    .load(img)
        //               调用在MyGlideExtension中实现的，被@GlideOption注解的方法，不需要传递 RequestOptions 对象
                    .injectOptions()
                    .into(mIv);
            }

   ```
###  <span id = "29">GreenDao</span>
1. ORM框架——在关系型数据库和对象之间做一个映射，就不需要操作sql语句。
2. GreenDao
   * 属于ORM框架
   * 为关系型数据库提供面向对象的接口
   * 简化数据库操作
   * 性能最高
   * 强大API，涵盖关系和链接
   * 内存消耗100kb大小
3. 概念以及步骤
   * 一个实体类对应一个表——创建实体类
   * 一个Dao对应一个数据访问对象（某表的操作）
   * DaoMaster——数据库连接对象
   * DaoSession——由连接生成的会话——创建Dao、DaoMaster、DaoSession
4. 使用
   * 创建数据库会话
   ```java
   // 引入注解，写出属性后点Build——Make Project。
    @Entity
    public class GoodsModel implements Parcelable {
        @Id(autoincrement = true)
        private Long id;
        @Index(unique = true)
        private Integer goodsId;
        private String name;
        private String icon;
        private String info;
        private String type;
   ```
   ```java
        public class MyApplication extends Application {

        public static DaoSession mSession;

        @Override
        public void onCreate() {
            super.onCreate();
            initDb();
        }

        //连接数据库并创建会话
        public void initDb () {
            //1. 获取需要连接的数据库
    //        获取SQLiteOpenHelper对象devOpenHelper
            DaoMaster.DevOpenHelper devOpenHelper = new DaoMaster.DevOpenHelper(this, "imooc.db");
    //        获取SQLiteDatabase
            SQLiteDatabase db = devOpenHelper.getWritableDatabase();
            // 2. 创建数据库链接
    //        加密数据库
    //        Database db = devOpenHelper.getEncryptedWritableDb("123");
    //        创建DaoMaster实例
    //        DaoMaster保存数据库对象（SQLiteDatabase）并管理特定模式的Dao类（而不是对象）。
    //        它具有静态方法来创建表或将它们删除。
    //        其内部类OpenHelper和DevOpenHelper是在SQLite数据库中创建模式的SQLiteOpenHelper实现。
            DaoMaster daoMaster = new DaoMaster(db);
            // 3. 创建数据库会话
    //        管理特定模式的所有可用Dao对象
            mSession = daoMaster.newSession();
        }
   ```
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
###  <span id = "35">ALDL实现远程服务的通信</span>
1. 安卓接口定义语言：进程间的通信接口
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
###  <span id = "37">Socket&Https通信</span>
1. Socket：
   * 通过，ip定位电脑，端口号定位程序。
   * UDP：封装发送。不安全的通信协议。
   * TCP：持续性输送消息。流。
2. Http和Socket的区别
   * Http用于应用层，无状态协议
   * Socket用于传输层：可以自己定义协议，灵活性更高，实现Client和Server的通信。
3. https：更加安全
##  <span id = "38">项目</span>
1. 模块化，用model作为基本单元
2. 全局唯一配置入口，单Activity，以Fragment作为基本容器
3. 核心model：路由架构、HTTP请求、通用UI、重复处理（ui绑定rv的处理网络请求的回调） 、业务相关的东西。
4. 业务model：只有第三方库、项目特有的个别功能、相应一类业务的特殊UI、相应一类业务的通用逻辑和特殊处理
5. 创建model：mall-library，存储自己的库等。业务无关的东西。
6. 全局配置搭建
* 配置管理器（mall.library/global/Configurator）
   * 获取全局的存储容器变量
   * 全局的单例模式
   * 访问服务器端API设置
   * 结束配置——进行一些回收动作，检查配置是否完成；配置完成后得到数据
* 存储配置的容器（mall.library/utilstorage/MemoryStore)
   * 以内存级别存储，当被序列化到xml或者数据库中后，在配置或者远程网络变化时也能及时更改响应
   * 全局的类最好是线程安全的单例模式，内部类或者双重校验锁
   * 以HashMap的数据结构存储数据
* 枚举类的类型存储数据变量（mall.library/global/GlobalKeys）
* 访问配置器的类（mall.library/global/Mall）
   * 获取MemoryStore，全局context，返回值Configurator（配置）
* 与app module进行连接（创建app.MallEXampleApp）
   * 通过mall.library/global/Mall类的init方法进行访问全局配置中的方法或变量
7. 网络框架搭建
* 将Retrofit封装成一个通用的网络请求库
   * 导入依赖“com.squareup.retrofit2:retrofit:2.9.0”
   * 创建枚举类，存放HTTP方法（mall.library/net/HttpMethod）
   * 创建Service接口，完成请求的映射（mall.library/net/RestService），对应RestfulAPI。写相应的get、post、delete、upload等方法
   * 创建Retrofit的各个实例的类。（mall.library/net/RestCreator）。
      * 创建OkHttpHolder用到了建造者模式：将一个复杂对象的构建与它的表示分离，使同样的构建过程可以创建不同的表示。
      * 使用多个简单的对象一步一步构建成一个复杂的对象:
      * 为了解析返回的字符串，使用了简单工厂模式
* 实现具体的请求
   * 在所有依赖mall-library的app中对外暴露直接使用的客户端，（mall.library/net/RestClient），————构建每一个restful请求，对API请求进行回应
   * 建造者模式构建参数和回调。Builder是静态内部类，独立出一个类。（mall.library/net/RestClientBuilder），构建RestClient并初始化参数和回调，辅助RestClient。
      * WeakHashMap存储请求参数——基于弱引用，随时可能被删除，在缓存时由于内存有限不能缓存所有对象，因此需要一定的删除机制淘汰掉一些对象。
      * 建立具体处理逻辑的回调类，作为参数传递给RestClient中的方法调用。（mall.library/net/callback/RequestCallBacks）
   * 建立（mall.library/callback）包，存放回调方法。
* 网络加载
   * 加载框
      * 导入 api 'com.wang.avi:library:2.1.3'库
      * 新建mall.library/ui/loader包，表示加载框，创建枚举类，ui/loader/LoaderStyles。样式的选择是通过加载类的名字以反射的形式加载实例，再以view渲染的方式进行展现
      * 处理loader：mall.library/ui/loader/MallLoader。本身loader是以view的形式进行展现的，需要实例化再展现。所以应该以dialog容器的形式承载loader 以弹出框的形式加载出，当网络请求加载结束后再cancel。
         * 获取屏幕宽高：导入工具类com.blankj:utilcode:1.29.0
         * 设置加载方法
      * 在RestClientBuilder、RestClient、RequestCallBacks中创建loader变量和初始化器
8. UI搭建
   * 基类Fragment实现
      * 单Activity==controcler
      * 导入Fragment包
      * 新建Fragment包。
      * mall.library/Fragment/BaseFragment，抽象类不实例化。
      * mall.library/Fragment/MallFragment,继承自BaseFragment，编写延迟时间等一些在底部实现的封装方法
      * 建立承载容器Activity：mall.library/activities/ProxyActivity。初始化容器并继承代理方法。
   * 电商主页
      * 封装底部Tab基类——mall.library/Fragment/bottom
         * mall.library/Fragment/bottom/BaseBottonFragment：基类，不需要初始化，抽象类。相当于底部容器（底部所有）。将tab和fragment封装到每一个item中，进行点击事件的处理onBindView，比如颜色的切换，图标的改变等
         * mall.library/Fragment/bottom/BaseItemFragment：实现基本功能。每一页tab的Fragment（每个tab点击的功能，和对应的每个tab点击后的Fragment）
         * mall.library/Fragment/bottom/BottomTabBean：容纳每个tab中承载的Fragment（切换时的每个tab的属性，比如名字和图标）
         * mall.library/Fragment/bottomItemBuilder：构建类，返回每个item（属性+功能）
    * 实现底部的Tab类
         * app/fragment/MallMainFragment:继承自BaseBottomFragment()，
            * 初始化实现，添加需要的主界面和tab对，框架会自动判断并渲染出正确的主界面。图标使用font aawesome中的图标，直接fa引用，并设置每个tab的名字。
            * 将字体库构造到constructor中。
            * 创建每个item对应的实例化Fragment。在每个fragment中设置功能就可以实现具体每页的功能。
9. 主页实现
   * 首页框架
      * 导入RecyclerView
   * 轮播图
      * 新建banner 初始化轮播图相关
      * 创建holder：新建glide对象，将缓存策略、动画等放入glide对象，将data/url插入到imageView中进行加载，让image显示网络图片
      * 创建creator：设置具体轮播，比如轮播时间和轮播样式等
   * recyler的创建抽象几何
      * 创建mall.library/ui/recyler
      * mall.library/ui/recyler/ItemType：枚举。每个item的ui类型。比如只有文字、只有图片、轮播图等
      * mall.library/ui/recyler/MultipleFields ：枚举类，数据加载要解析的字段与json转换的字段一一对应
      * mall.library/ui/recyler/MultipleItemEntity：存储MultipleFields的字段的数据并做相应的转换。通过json返回值类型放入itemType，放入MultipleFields的itemType中的属性。
      * mall.library/ui/recyler/MultipleItemEntityBuilder：构造器，item和itemType的连接工作
      * mall.library/ui/recyler/MultipleRecyclerAdapter和 MultipleViewHolder 。将数据映射到相应的view中。经过adapter的处理后，在view看来所有的数据映射过来都是一样的。
         * getSpanSize方法：将数据转换到MultipleItemEntity中通过方法获取到每一行的MultipleItemEntity，获取到spanSize，将该值又转换到getSpansize，判断一行能塞几个值——数据驱动ui通过
         * convert：通过itemtype方法获取布局的类型进行渲染——数据驱动UI
   * 数据驱动UI
       * 惰性加载RV的UI和数据。
       * 初始化数据
         * 导入com.alibaba:fastjson:1.1.71.android包
         * mall.library/ui/recyler/DataConverter：数据转换层，抽象方法
         * app/fragment/index/IndexDataConverter：继承DataConverter，通过json object将数据取出，放入adapter对象中，RecyclerView加载adapter。
10. 商品分类页面
    * 右边内容
       * app/fragment/sort/content/ContentFragment。点击list，通过contentFragment的静态方法newInstance（），将contentId传入，根据id得到content列表
       * app/fragment/sort/content/SectionDataConverter：json数据转换，用SectionEntity存储数据，不用之前通用的Entity，因为content部分是section组成的。
          * app/fragment/sort/content/SectionContentItemEntity ：存储section中的商品的数据类型
          * app/fragment/sort/content/SectionBean：存储一整个section的数据
          * 将SectionBean传入SectionDataConverter中，取出json数据并转换成对象
       * app/fragment/sort/content/SectionAdapter：将数据映射（转换）到UI上
       * app/fragment/sort/content/ContentFragment：初始化数据和view进行展示
    * 左边分类栏
       * app/fragment/sort/list/VerticalListFragment：初始化UI后加载数据
       * app/fragment/sort/list/VerticalListDataConverter：数据转换
    * list和content联动
       * app/fragment/sort/list/SortRecyclerAdapter：通用adapter类，通过layout和枚举类取出每个空间的id，设置点击事件，
       * app/fragment/sort/SortFragment：父类Fragment，加载布局进行初始化
11. 购物车
   * ContrainLayout布局实现购物车界面和item的布局
   * app/fragment/cart/ShopCartFragment：初始化控件、加载数据、响应商品增减
   * UI渲染
      * app/fragment/cart/ShopCartDataConverter：数据转换，获取数据对象
      * app/fragment/cart/ShopCartAdapter：将数据放入adapter，与UI进行交互渲染
   * 加减商品
      * adapter中添加加减事件，被点击后数目和价格的变化
      * ShopCartFragment获取adapter和总价
      * item价格变化改变外部fragment的总价：app/fragment/cart/ICartItemPriceListener接口，adapter中在加减方法中调用接口，将总价作为参数传递，fragment类中的adapter对象可以接收到变化的总价
      * 每次加减都请求服务器，传递id和count
   * 全选和清空（定义方法后要在bindView进行绑定）
      * 定义点击接口，选择不同的按钮实现不同的事件
      * 全选
        * 使用tag进行标记是否全选
        * 全选有颜色的更改，设置取消全选的下标范围
        * 没有全选有颜色的更改，设置全选的下标范围
      * 清空
        * 清空adapter数据
        * 清空改变
        * 通知用户购物车清空了
      * 删除选中条目
        * 将需要删除的item记录
        * adapter中的data中统一删除
        * notify通知recylerView发生了删除
      * ViewStub：随时可能出现的控件，会替代当前的layout。用作提醒购物车空需要添加。
12. 商品详情
    * app/fragment/details/GoodsDetailFragment
      * 获取goodsID
      * 通过API获取相应商品信息
      * 轮播图的初始化 传入json对象
      * 加载商品具体信息
      * 初始化TabLayout和ViewPager，并关联TabLayout和ViewPager
      * 上下粘性滑动
      * 添加购物车操作：加入购物车动作进行了restful API请求，向服务器请求一次API，把加购物车的动作通知服务器，得到服务器返回结果后，将结果体现在购物车上
    * app/fragment/index/IndexItemClickListener:点击事件的重写。点击首页的商品跳转到商品详情中。
    * app/fragment/details/GoodsInfoFragment：商品详细信息。将json数据赋给具体的对象
    * app/fragment/details/TabPagerAdapter：ViewPager的adapter。滑动到pager才加载数据。取出json数据放入对象中，做位置处理。
    * app/fragment/details/ImageFragment：图片的显示放在另外的Fragment中处理，实现解耦。使用RecyclerView实现
    * app/fragment/details/DetailImageAdapter：RecyclerView的adapter实现
13. 项目总结
   * 首页展示前，整个Ａｃｔｉｖｉｔｙ作为Ｆｒａｇｍｅｎｔ的调度站，负责Ｆｒａｇｍｅｎｔ的跳转和信息传递。这样的好处是，避免跳转流程复杂，导致自己开发时候很晕。一般Ｆｒａｇｍｅｎｔ的跳转需要Ａｐｐ的当前状态，用户的当前状态，通过这两状态去决定下一个Ｆｒａｇｍｅｎｔ是什么。
   * MVC
     * model：负责在数据库中存取数据
     * view：处理数据的显示，视图是根据模型数据创建的
     * controller：处理用户交互，从视图读取数据，并向模型发送数据。
     * 优点：耦合性低，更改视图层代码不用重新编译模型和控制器代码
     * 缺点：不能互相调用的时候就功能局限，独立重用功能很低。对数据可能会存在多次的频繁访问。
14.  OkHttp
     * 关于网络请求的第三方类库，其中封装了网络请求的get、post等操作的底层实现，
     * 创建OkHttpclient对象：http请求的客户端类，client对象作为全局的实例进行保存。只需要这一个实例对象。所有的http请求共用client实例对象的response缓存和线程池。
     * 创建request对象：存储了请求报文的信息（请求的URL地址、请求的方法和请求体，标志位），使用builder模式进行创建，将创建和表示分离。
     * new call 方法创建call 对象：代表一个实际的http请求，连接request和response的桥梁。这个对象可以同步、异步获取数据
       * execute（）： Response response = client.newCall(request).execute();。同步需要开启子线程，会阻塞进程获取数据，接收一个request对象，执行execute方法后返回response对象，也就是结果。
         * 首先使用new创建OkHttpClient对象。然后调用OkHttpClient对象的newCall方法生成一个Call对象，该方法接收一个Request对象，Request对象存储的就是我们的请求URL和请求参数。最后执行Call对象的execute方法，得到Response对象，这个Response对象就是返回的结果。
       * enqueue：client.newCall(request).enqueue(new Callback()。接收callback参数，不会阻塞，开启一个子线程，在子线程中操作。当异步请求成功时，回调onResponse方法，获取okHttp的response对象。当请求失败时，回调onFailture方法。这两个方法都在工作线程当中执行。
         * 首先使用new创建OkHttpClient对象。然后调用OkHttpClient对象的newCall方法生成一个Call对象，该方法接收一个Request对象，Request对象存储的是我们的请求URL和请求参数。最后执行Call对象的enqueue方法，该方法接收一个Callback回调对象，在回调对象的onResponse方法中拿到Response对象，这就是返回的结果。
     * post方法：在生成request方法的时候执行了post方法，get方法没有。因为request的builder时候，默认是get方法，所以需要post时就需要申明post方法。
15. retrofit
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
16. ButterKnife
    * 通过解析注解来实现辅助代码
    * 绑定一个View  注解@BindView
    * 给view添加点击事件
17. Glide
    * 传入context对象，调用load方法，使用into方法显示imageView
  

   
     