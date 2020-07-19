## 概述
#### 基本特征
1. 并发：一段时间内同时运行多个程序——进程和线程
2. 并行：同一时刻运行多个指令。如分布式操作系统、多流水线。
3. 共享：资源可以被并发进程共同使用——互斥共享、同时共享
4. 虚拟：时分复用、空分复用、时分复用：多个进程在同一个cpu上并发执行，每个进程轮流占用处理器每次只执行一小个时间片并快速切换。空分复用：虚拟内存。将物理内存抽象为地址空间，每个进程都有各自的地址空间。地址空间的页被映射到物理内存，地址空间的页并不需要全部在物理内存中，当使用到一个没有在物理内存的页时，执行页面置换算法，将该页置换到内存中。
5. 异步：进程以不可知的速度向前推进。
#### 基本功能
1. 进程管理：进程控制、进程同步、进程通信、死锁处理、处理机调度
2. 内存管理：内存分配、地址映射、内存保护与共享、虚拟内存
3. 文件管理：文件存储空间的管理、目录管理、文件读写管理和保护
4. 设备管理：完成用户的 I/O 请求。
#### 系统调用
1. 进程由用户态需要使用内核态的功能时，就进行系统调用从而陷入内核。
#### 大内核和微内核
1. 大内核：将os作为一个紧密结合的整体放到内核。各模块共享信息，具有很高的性能。
2. 微内核：部分操作系统功能移出内核，只有微内核一个模块运行在内核态。需要频繁的在用户态和和心态之间进行切换。
#### 中断分类
1. 外中断：由cpu之外的事件引起。比如IO完成中断、时钟中断、控制台中断。
2. 异常：cpu内部事件引起，如非法操作码、地址越界、算术溢出等
3. 限入：用户程序中使用系统调用

## 进程管理
#### 进程与线程
1. 进程：资源分配的基本单位。PCB进程控制块。
2. 线程：独立调度的基本单位，一个进程有多个线程，线程并发执行可以使在浏览器中点击一个新链接从而发起 HTTP 请求时，浏览器还可以响应用户的其它事件
3. 区别：进程拥有资源，线程访问资源；线程是独立调度的基本单位，同一进程的线程切换不会引起进程切换；进程的创建和撤销的系统开销都比线程大；线程之间可以直接通信，进行通信需要借助IPC（进程间通信）
#### 进程状态的切换
1. 就绪状态、运行状态、阻塞状态
2. 就绪状态和运行状态可以相互转换。就绪状态通过**进程调度算法**获得cpu时间转为运行状态。运行状态在**cpu时间片用完**后转为就绪状态。
3. 运行状态在**缺少资源**时会转为阻塞状态。
#### 进程调度算法
1. 批处理系统
* FCFS：先来先服务。按照请求顺序进行调度。
* SJF：短作业优先
* SRTN：最短作业优先的抢占式版本。
2. 交互式系统
* 时间片轮转：按照FCFS的原则排成一个队列。每个进程轮流执行一个时间片长度，然后发出时钟中断，送往末尾，进行下个进程的执行。
* 优先级调度：按照优先级进行调度。可以随着时间的推移增加等待进程的优先级。
* 多级反馈队列：时间片和优先级调度的结合，设置了多个队列，当一个进程在第一个队列没有执行完，就会移到下个队列。每个队列的优先权不同。
3. 实时系统：要求在一个确定时间内得到相应。硬实时：必须满足绝对的截止时间。软实时：可以容忍一定的超时。
#### 进程同步
1. 临界区：临界资源进行访问的代码。临界资源需要互斥的访问。
2. 同步与互斥：互斥：同一时刻只有一个进程能进入临界区。同步：因为合作产生的制约关系。
3. 信号量
4. 管程：独立出控制的代码，使客户端调用代码更容易。在一个时刻只能有一个进程使用管程。
#### 进程通信
1. 进程同步：控制多个进程按一定顺序执行
2. 进程通信：进程间传输信息。
3. 为了达到进程同步的目的，需要让进程进行通信。
4. 管道：
* pipe（）函数
* 半双工通信，单向交替传输。
* 只能在父子进程和兄弟进程中使用
5. FIFO
* 命名管道
* 没有只能使用与父子进程中使用的限制。
* 用于客户=服务器应用程序中，作为汇聚点，在客户进程和服务器进程之间传输数据。
6. 消息队列：
* 独立于读写进程存在
* 避免同步阻塞问题，不需要进程提供同步方法
* 可以有选择的接收消息
7. 信号量：为多个进程提供对共享数据对象的访问。
8. 共享存储：
* 允许多个进程共享一个存储区——最快的一种IPC
* 使用信号量同步访问
* 多个进程可以将同一个文件映射到地址空间实现共享内存。
9. 套接字：用于不同机器之间的进程通信。

## 死锁
#### 必要条件
* 互斥：每个资源要么已经分配给了一个进程，要么就是可用的
* 占有和等待：已经得到了某个资源的进程可以再请求新的资源
* 不可抢占
* 环路等待
#### 处理方法
* 鸵鸟策略
* 死锁检测和死锁恢复
* 死锁预防
* 死锁避免
##### 弱鸟策略——忽略
##### 死锁检测与死锁恢复
不阻止死锁。而是发生死锁时，采取措施进行恢复。

1. 每种类型一个资源的死锁检测——检测有向图是否存在环，从一个节点进行dfs，检测是否存在环。
2. 每种类型多个资源的死锁检测：
* 寻找一个没有标记的进程 Pi，它所请求的资源小于等于 A。
* 如果找到了这样一个进程，那么将 C 矩阵的第 i 行向量加到 A 中，标记该进程，并转回 1。
* 如果没有这样一个进程，算法终止。
3. 死锁恢复：
* 抢占恢复
* 回滚恢复
* 杀死进程恢复
##### 死锁预防
1. 破坏互斥条件
2. 破坏占有和等待条件——在开始前请求全部资源
3. 破坏不可抢占
4. 破坏环路等待——编号，按照顺序
##### 死锁避免
在程序运行时避免发生死锁
1. 安全状态
2. 银行家算法
## 内存管理
#### 虚拟内存
1. 让物理内存扩充成更大的逻辑内存
2. 将内存抽象成地址空间，每个程序拥有自己的地址空间。
3. 程序的**地址空间**被分割成许多块，每一块称为一页。映射到**物理内存**中。不需要连续映射也不需要全部映射。
4. 因此一个程序不用全部调用内存就可以运行。
#### 分页系统地址映射
1. 内存管理单元：地址空间和物理内存的转换
2. 页表：地址空间和内存空间的映射表
3. 虚拟地址：存储页面号+存储偏移量
#### 页面置换算法
如果要访问的页不在内存中，发成缺页中断，需要将页面调入内存中。
当内存满时，需要调出一个页面到磁盘对换区来腾出空间。
1. 最佳：替换出最长时间不再被访问的页面。实现难，因为不知道一个页面多长时间不再被访问。
2. 最近最久未使用：维护链表，页面被访问时移到链表表头，链表表尾就是最久未访问的。
3. 先进先出：换出最先进入的
4. 第二次机会：当页面被访问 (读或写) 时设置该页面的 R 位为 1。需要替换的时候，检查最老页面的 R 位。如果 R 位是 0，那么这个页面既老又没有被使用，可以立刻置换掉；如果是 1，就将 R 位清 0，并把该页面放到链表的尾端，修改它的装入时间使它就像刚装入的一样，然后继续从链表的头部开始搜索。
5. 时钟：使用环形链表将页面连接起来，再使用一个指针指向最老的页面。
#### 分段
分页随着页面内容的增加，会产生覆盖的问题。
分段可以动态增长
（一个横着分一个竖着分）
#### 段页式
先划分成多个段，在每个段上再划分成大小相同的页。
#### 分页分段的比较

* 对程序员的透明性：分页透明，但是分段需要程序员显式划分每个段。
* 地址空间的维度：分页是一维地址空间，分段是二维的。（每个页面的地址连续，每个段之间地址不连续）
* 大小是否可以改变：页的大小不可变，段的大小可以动态改变。
* 出现的原因：分页主要用于实现虚拟内存，从而获得更大的地址空间；分段主要是为了使程序和数据可以被划分为逻辑上独立的地址空间并且有助于共享和保护。

## 设备管理
#### 磁盘结构

* 盘面（Platter）：一个磁盘有多个盘面；
* 磁道（Track）：盘面上的圆形带状区域，一个盘面可以有多个磁道；
* 扇区（Track Sector）：磁道上的一个弧段，一个磁道可以有多个扇区，它是最小的物理储存单位，目前主要有 512 bytes 与 4 K 两种大小；
* 磁头（Head）：与盘面非常接近，能够将盘面上的磁场转换为电信号（读），或者将电信号转换为盘面的磁场（写）；
* 制动手臂（Actuator arm）：用于在磁道之间移动磁头；
* 主轴（Spindle）：使整个盘面转动。

#### 磁盘调度算法——降低平均寻道时间
影响因素：

* 旋转时间——磁头移动到合适的扇区
* 寻道时间——磁头移动到合适的磁道
* 实际的数据传输时间
1. FCFS：先来先服务——时间长
2. SSTF：最短寻道时间优先——容易饥饿
3. SCAN：电梯算法——按一个方向进行调度，直到该方向没有请求，改变方向。——解决饥饿
## 链接
#### 编译系统

* 预处理阶段：处理以 # 开头的预处理命令；
* 编译阶段：翻译成汇编文件；
* 汇编阶段：将汇编文件翻译成可重定位目标文件；
* 链接阶段：将可重定位目标文件和 printf.o 等单独预编译好的目标文件进行合并，得到最终的可执行目标文件。
#### 静态链接

1. 输入可重定位目标文件，输出可执行目标文件
2. 符号解析：将每一个符号引用与符号定义关联起来
3. 重定位：将符号定义与一个内存位置关联起来，让引用指向内存位置
4. 当静态库更新时那么整个程序都要重新进行链接；
#### 目标文件

1. 在给定的文件系统中一个库只有一个文件，所有引用该库的可执行目标文件都共享这个文件，它不会被复制到引用它的可执行文件中
2. 在内存中，一个共享库的 .text 节（已编译程序的机器代码）的一个副本可以被不同的正在运行的进程共享
3. 解决了静态库的：当静态库更新时那么整个程序都要重新进行链接 和 对于 printf 这种标准函数库，如果每个程序都要有代码，这会极大浪费资源