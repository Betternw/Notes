## 快捷键
alt+enter：错误纠正
## 第一章
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
## UI基础
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
#### 基础控件
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

#### 常用组件
##### 组件一：Activity
1. 一个页面就是一个Activity
2. 启动那个Activity，哪个注册intent-filter
3. Activity跳转：使用Intent
```java
//Activity跳转
Intent intent = new Intent(ButtonActivity.this,ProgressBarActivity.class);
startActivity(intent);
```
4. Activity启动模式——standard
* 默认是这个，按照顺序
5. Activity启动模式——singleTop
* 顶部复用，新打开一个会在栈上面重新打开一个，底部的还留着
6. Activity启动模式——singleTask
* 如果复用Activity，会清除前面的
7. Activity启动模式——singleInstance
* 打开新的，会重新新建一个栈
#####  Menu
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

#####  Dialog
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
//3. 显示 （参数1：锚，参照物 参数2 3：相对于锚在xy方向的偏移量）
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
