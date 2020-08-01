#### 二. java内存区域与内存溢出异常
##### 概述
1. 编译器将java源代码转换为.class文件，虚拟机加载并运行.class文件
2. 运行时数据区：

* 堆: 存放Java **对象**， 线程之间共享的；
* 栈: **方法**运行，每一个方法对应一个栈帧，每一个线程对应一个栈；每个栈由包括 操作数、局部变量表、指向运行时常量池的引用，方法返回地址、附加位区；所以是线程不共享 (这个就是栈的宽度)(栈的栈帧个数决定了栈的深度)；
* 方法区 (静态区) : 被虚拟机加载的类信息、静态 (static) 变量，常量 (final) ，即时编译器编译后的代码等**数据**。
* 程序计数器: 指出某一个时候执行某一个指令、执行完毕之后要返回的**位置**，当执行的Java方法的时候，这里保存的当前执行的地址，如果执行的是本地方法，那么程序计数器为空。线程不共享。
3. java程序运行：编译为.class文件——JVM初始分配方法区和堆（**线程共享，与java程序生命周期相同**），加载运行——为每一个线程分配程序计数器、虚拟机栈、本地方法栈。程序终止时三者释放（**非线程共享，生命周期与所属线程相同，垃圾回收发生场所**）
4. （非）程序计数器：当前线程所执行的字节码的行号指示器，每个线程独立拥有。java方法有本地native方法无。
5. （非） java虚拟机栈：java方法执行的线程内存模型。栈帧: 每个方法执行都会创建一个栈帧。请求深度过大：StackOverowError异常。扩展时无法申请到足够的内存：OutOftMemoruError异常
6. （非）本地方法栈：本地方法执行的服务。也会存在上述异常。
7. （共）java堆：所有对象保存在java堆中，所有线程共享java堆。
8. （共）方法区：存储已被虚拟机加载的类信息、常亮、静态变量等数据
9. 运行时常量池：存放编译器生成的各种字面量和符号引用，在类加载后进入方法区的运行常量池中。常量可以在编译器中产生，也可以在运行期间放入。
##### 对象

1. 对象的创建：new类名——在常量池中定位一个类的符号引用——如果没有找到这个符号引用，进行类的加载解析和初始化—虚拟机为对象在堆中分配内存——将分配的内存初始化为零值——调用对象的init方法
* 内存分配：指针碰撞——指针作为内存是否分配的分界点，需要分配内存时指针移动；空闲列表：分配与否的内存空间相互交错，列表+记录哪些是可用的
* 线程安全问题
* 初始化对象：根据对象头中储存的信息
* 调用对象的构造方法：init初始化
2. 对象的内存布局：对象头（自身运行数据和类型指针）、实例数据、对齐填充
3. 对象的访问定位：
* 句柄：栈中的reference来操作堆上的具体对象，包含了对象的实例数据和地址信息。栈中的refenerce—句柄池——实例数据和类型数据
* 直接指针：reference中存储的就是对象地址

#### 三 垃圾回收基本原理和算法
##### 1.概述

1. 垃圾回收不针对线程私有的程序计数器、虚拟机栈和本地方法栈。而是针对**java堆和方法区**。
##### 2. 如何判定对象为垃圾对象

###### 引用计数法

1. 在对象中添加一个引用计数器
2. 当有地方引用这个对象的时候，引用计数器的值就+1，当引用失效的时候，计数器的值就-1.
3. 当某个对象的引用计数器的值为0的时候，就回收这个对象
4. 当堆内部的对象互相指向的时候就不会回收，但是也是垃圾对象

###### 可达性分析算法

1. 解决上面引用计数法不能判定堆内部垃圾对象的问题
2. 以“gc roots”为起点进行搜索，路径叫做引用链。如果一个对象到“gc roots”没有引用链，那么就是垃圾对象
###### 引用分类

1. 强引用: StrongReference: 引用指向对象，类似"Object obj = new Object();"这类引用，gc(Garbage Collection)运行时不回收；
###### finalize（）方法

1. 类似析构函数，用来做关闭外部资源等工作。
2. 当对象被回收时，如果需要执行该对象的finalize（）方法，那么可能在该方法中让对象被重新引用。从而实现对象的自救。
3. 自救只能进行一次。
4. 在可达性分析算法中，要在finalize中拯救自己，需要重新与引用链上的任何一个对象建立关联，比如this关键字赋值
###### 回收方法区

1. 方法区中回收：废弃常量和无用的类
2. 如果一个常量进入常量池中，但是没有对象引用该常量，就会被清理出常量池。
3. 如果一个类：该类的所有实例被回收、加载该类的classloader被回收、该类对象的对象没有被引用。那么这个类就可以被回收。
##### 3. 垃圾回收算法
###### 标记清除算法

1. 首先标记处所有需要回收的对象，在标记完成后统一回收所有被标记的对象。
2. 产生的问题：
* 标记和清除的效率不够高——效率问题
* 标记清除后会产生大量不连续的内存碎片——空间问题
###### 复制算法

1. 将内存容量划分为大小相等的两块，每次只使用其中的一块。
2. 当这一快的内存用完就将还存活着的对象复制到另外一块上面。
3. 然后将已使用过的内存空间一次性清理掉。
4. 每次都是对半个区域进行内存回收。但是将内存缩小到一半，代价很高。
5. 将内存分为一块较大的 Eden 空间和两块较小的 Survivor 空间，每次使用 Eden 和其中一块 Survivors。当回收时，将 Eden 和 Survivor 中还存活着的对象一次性地复制到另外一块Survivor空间上，最后清理掉 Eden 和刚才用过的 Survivor 空间。
6. 默认 Eden和 Survivor 的大小比例是 8 : 1。只有 10% 的内存会被“浪费”。
7. 新生代：大多数对象在新生代中创建，生命周期普遍短，gc后只有少量对象存活，采用赋值算法。分三个区：一个Eden区，两个Survivor区（一般而言），大部分对象在Eden区中生成。当Eden区满时，还存活的对象将被复制到两个Survivor区（中的一个）。对象每经历一次Minor GC，年龄加1。这个过程属于晋升。到达阈值时，会被放到老年代。——当new一个对象时就会放到新生代
8. 老年代：在新生代中经历了n次gc后仍然存活的对象，就会被放到老年代。通常采用标记清理或者标记整理算法。
9. 永久代：存放元数据。垃圾回收对象关系不大。——即常说的方法区
###### 标记整理算法

1. 根据老年代的特点，标记整理算法，先标记，然后将存活的对象向一端进行移动，然后清理掉端边界以外的内存。

###### 分代收集算法
将java堆划分为新生代和老年代，根据各个年代的特点采用最适当的收集算法。
##### 4. 垃圾收集器

1. 单线程指的是垃圾收集器只使用一个线程进行收集，而多线程使用多个线程
2. 串行指的是垃圾收集器与用户程序交替执行，在执行垃圾收集的时候需要停顿用户程序；并发指的是垃圾收集器和用户程序同时执行。除了 CMS 和 G1 之外，其它垃圾收集器都是以串行的方式执行
3. Serial：**单线程串行**。现在依然是虚拟机运行在 Client 模式下的默认新生代收集器
4. ParNew：是 Serial 收集器的**多线程**版本。只有它能与 CMS 收集器配合工作。
5. 并行 (Parallel): 指多条垃圾收集线程并行工作，但此时用户线程仍然处于等待状态
6. 并发(Concurrent) : 指用户线程与垃圾收集线程同时执行，(但不一定是并行的，可能会交替执行)，用户程序在继续运行，而垃圾收集程序运行于另一个CPU 上
7. Parallel Scavenge收集器
* 是新生代复制算法，多线程收集器、吞吐量优先的收集器。
* **多线程并行**。
* 其他收集器的关注点是缩短垃圾收集时用户线程的停顿时间，而本收集器的目标是达到一个可控制的吞吐量（运行代码时间/运行代码时间+垃圾收集时间。）
* 适合在后台运算而不需要太多交互的任务
* MaxGCPauseMillis 参数允许的值是一个大于 0 的毫秒数。最大垃圾收集停顿时间。
* GCTimeRatio：垃圾收集时间占总时间的比率，相当于是吞吐量的倒数
* -XX:+UseAdaptiveSizePolicy。虚拟机会根据当前系统的运行情况收集性能监控信息，动态调整这些参数以提供最合适的停顿时间或者最大吞吐量，这种调节方式成为GC自适应的调节策略
8. Serial Old收集器：**单线程**，使用标记整理。是Serial的老年代版本。
9. Paralell Old收集器：Parallel Scavenge 收集器的老年代版本，使用**多线程**和 ”标记-整理“ 算法
10. Cms收集器：
* 标记 - 清除 算法。**并发收集**、低停顿。
* 初始标记——并发标记（并发）——重新标记——并发清除（并发）
* 吞吐量低、无法处理浮动垃圾、标记 - 清除算法导致的空间碎片
11. G1收集器
* 初始标记——并发标记——最终标记——筛选回收
* 面向服务端应用的垃圾收集器
* 并行与并发
* 分代收集
* 空间整合：整体基于标记整理，局部基于复制
* 可预测停顿
* 取消新生代老年代划分
##### 5. 内存分配与回收策略

1. 自动内存管理：
* 给对象分配内存——堆上分配，主要位于新生代
* 回收分配给对象的内存——回收算法和垃圾收集器
2. 内存分配原则
* 对象优先在Eden分配，年龄为0
* 大对象和生命周期长的对象直接进入老年代
* 一次新生代GC后Eden中还存活的对象就进入S0或者S1，年龄加1
* 当年龄大于15岁时，进入老年代
* 15岁的原因：分配的空间是4位，最大就是15

###### 5.1 垃圾回收——清理对象整理内存
1. 新生代 GC (Minor GC)：一般直接分配到Eden区域，但是如果Eden区域不够，就进行Minor GC和分配担保。一般回收速度度也比较快。采用的是复制算法。对于较大的对象，在 Minor GC 的时候可以直接进入老年代
2. 老年代 GC (Major GC / Full GC）：老年代没有足够空间发生Full GC。较慢。采用的是标记 - 清除 / 整理算法。Full GC 发生的次数不会有 Minor GC 那么频繁，并且 Time(Full GC)>Time(Minor GC)


###### 5.2 空间分配担保
* 在发生minor gc之前，虚拟机会检测 : 老年代最大可用的连续空间 > 新生代all对象总空间？

1.  满足，minor gc是安全的，可以进行minor gc。
2. 不满足，虚拟机查看HandlePromotionFailure参数：
* （1）为true，允许担保失败，会继续检测老年代最大可用的连续空间 > 历次晋升到老年代对象的平均大小? minor gc ：full gc。
* （2）为false，则不允许，要进行full gc。

#### 三 类加载机制
##### 1 类加载时机

1. 类从被加载到虚拟机内存中开始，到出内存为止，它的整个生命周期包括 : 加载 、验 证—准 备 —解 析（连接） 、初始化、使用 和卸载 7 个阶段。
2. 对类的使用方式分为两类：主动引用 被动引用
3. 所有的java虚拟机必须在每个类或接口被程序首次主动使用时才初始化他们。
###### 主动引用

1. 使用 new 关键字实例化对象的时候，读取或设置一个类的静态字段，调用一个类的静态方法
2. 通过反射，调用reflect包的方法对类进行反射调用的时候需要进行初始化。
3. 初始化一个类，先触发父类的初始化。
4. 虚拟机启动，先初始化主类。
5. 方法句柄对应的类需要初始化。
###### 被动引用

1. 被动引用不会导致类的初始化，只有首次主动引用才会初始化。
2. 通过子类引用父类的静态字段，不会导致子类初始化。
3. 通过数组来定义类，不会触发此类的初始化。会对数组类进行初始化。
4. 常量在编译阶段会存入调用类的常量池中，没有直接应用定义常量的类，不会触发定义常量的类的初始化。但是如果一个类的值没有给出确定的值，在程序运行调用这个常量时，就会主动使用这个常量所在的类，导致这个类被初始化。
5. 当一个接口在初始化时，只有在用到父接口的时候，如引用接口中定义的常量，才会初始化。
###### 类加载顺序

类的加载从上到下进行。
##### 2 类加载过程
加载、验证、准备、解析、初始化
 ###### 加载

1.  通过一个类的全限定名来获取定义此类的**二进制字节流**，即将类的.class文件中的**二进制数据读入到内存中**(这个就是类加载器做的事情

获取二进制字节流：

* 文件：zip包
* 网络
* 计算机生成二进制流，动态代理技术
* 由其他文件生成，jsp应用
* 数据库：从数据库中读取
2.  将这个字节流所代表的静态存储结构转化为方法区的**运行时存储结构**
3.  内存中生成一个代表这个类的 Class **对象**
 ###### 验证

1.  保证class文件的字节流包含的信息符合当前虚拟机的要求。
2.  文件格式验证
3.  元数据验证
4.  字节码验证
5.  符号引用验证
###### 准备

1. 为类变量分配内存并设置变量的初始值，但在到达初始化之前，类变量都没有初始化为真正的初始值
2. 变量使用的内存都在方法区中进行分配。
3. 实例变量不会在这阶段分配内存，它将会在对象实例化时随着对象一起分配在堆中
4. 实例化不是类加载的一个过程，类加载发生在所有实例化操作之前，并且类加载只进行一次，实例化可以进行多次
5. public static int value = 123; // 只有在初始化的<clinit>中才会是123，在准备阶段只是0
6. static final型的变量会在准备的阶段就附上正确的值:public static final int value = 123;
###### 解析

1. 虚拟机将常量池的符号引用替换为直接引用

* 类或接口的解析
* 字段解析
* 类方法解析
* 接口方法解析
2. 符号引用是一组符号来描述所引用的目标对象
3. 直接引用可以是直接指向目标对象的指针、相对偏移量或是一个能间接定位到目标的句柄
4. 符号引用就是字符串，包含足够的信息。
5. 当第一次运行的时候，根据字符串的内容搜索方法，当运行一次后符号引用就会被替换为直接引用，不用再搜索。
###### 初始化

1. 准备阶段，赋值系统要求的初始值。在初始化阶段赋值是主观的。为类的静态变量赋予正确的初始值。即虚拟机执行类构造器 <clinit>() 方法的过程
2. 父类中定义的静态语句块要优先于子类的变量赋值。
3. 如果一个类中不包含静态语句块，也没有对类变量的赋值操作，编译器可以不为该类生成<clinit>()方法。
4. 接口中有类变量的初始化操作。只有当父接口的定义的变量使用时，父窗口才会初始化。
5. 如果多个线程同时初始化一个类，只会有一个线程执行这个类的<clinit>()方法，其它线程都会阻塞等待，直到活动线程执行<clinit>() 方法完毕。
##### 3 类加载器

1. 实现：通过一个类的全限定名来获取描述此类的二进制字节流
2. 类加载器就是加载其他的类，负责将字节码文件加载到内存，创建class对象。
3. Java运行时，会根据类的完全限定名寻找并加载类，寻找的方式基本就是在系统类和指定的类路径中寻找，如果是class文件的根目录，则直接查看是否有对应的子目录及文件，如果是jar文件，则首先在内存中解压文件，然后再查看是否有对应的类。
4. 负责加载类的类就是类加载器，它的输入是完全限定的类名，输出是Class对象。
#####  类与类加载器
两个类相等，需要类本身相等并且使用同一个类加载器进行加载。
#####  类加载器分类

1. 启动类加载器：用c++实现，是虚拟机自身的一部分。加载java的基础类。日常用的Java类库比如String，ArrayList等都位于该包内。
2. 扩展类加载器：加载一些扩展类。
3. 应用程序加载器：加载应用程序的类，包括自己写的和引入的第三方法类库。即所有在类路径中指定的类。
#####  双亲委派模型

1. 应用程序加载器的父亲是扩展类加载器。扩展类的加载器是启动类加载器。在子加载器加载类时，一般会首先通过父加载器加载。
2. 加载过程：判断是否加载过，加载过就返回class对象。一个类只加载一次——没有加载，父加载器加载，如果成功返回class对象——如果父加载器没有加载成功，自己尝试加载类。
3. 父类先加载可以避免java类库被覆盖的问题。
#####  理解classloader

1. public ClassLoader getClassLoader()  获取实际加载它的ClassLoader
2. public final ClassLoader getParent()  获取它的父ClassLoader
#####  自定义类加载器

* 定义一个类，继承 ClassLoader；
* 重写 loadClass 方法；
* 实例化 Class 对象；

自定义类加载器的优势

* 类加载器是 Java 语言的一项创新，也是 Java 语言流行的重要原因之一，它最初的设计是为了满足 java applet 的需求而开发出来的；
* 高度的灵活性；
* 通过自定义类加载器可以实现热部署；
* 代码加密；
#### 四 java虚拟机性能监控工具

* jps (Java Process Status Tool): 虚拟机进程状态工具；
* stat (JVM Statistics Monitoring Tool): 虚拟机统计信息监视工具；
* jinfo (Configuration Info for Java): Java配置信息工具；
* jmap (Memory Map for Java): Java内存映像工具；
* jhat (JVM Heap Dump Browser): 虚拟机堆转存储快照工具；
* jstack (Stack Trace for Java): Java堆栈跟踪工具；
1. jps虚拟机进程状态工具
* 可以列出正在运行的虚拟机进程，并显示虚拟机执行主类名称，以及这些进程的本地虚拟机唯一ID
2. jstat : 虚拟机统计信息监视工具
* 用于监视虚拟机各种运行状态信息。可以显示本地或者远程虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据，
3. jinfo ：Java配置信息工具
* 实时地查看和调整虚拟机各项参数
4. jmap : Java内存映像工具
* 生成堆转存的快照
* 查询 finalize 执行队列，java 堆和永久代的详细信息，
5. jhat : 虚拟机堆转存储快照工具
6. jstack ： Java堆栈跟踪工具
* 查看虚拟机当前时刻的线程快照