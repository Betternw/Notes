## 一 概览
容器：collection（一个对象） map（键值对）
#### Collection
1. Set
* TreeSet：基于红黑树，有序操作
* HashSet：基于哈希表实现，支持快速查找，不支持有序性操作
* LinkedHashSet：内部使用双向链表
2. List
* ArrayList：基于动态数组，支持随机访问
* Vector：和ArrayList类似，但是线程安全
* LinkedList：基于双向链表，顺序访问。可以用作栈、队列、双向队列
3. Queue
* LinkedList：实现啥双向队列
* PriorityQueue：基于堆实现，可以用来实现优先队列

#### Map
* TreeMap：基于红黑树实现。
* HashMap：基于哈希表实现。
* HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致。使用 ConcurrentHashMap 来支持线程安全，ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
* LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

## 二 容器中的设计模式
#### 迭代器模式
使用foreach方法实现iterable接口的对象、
for (String item : list) {
    System.out.println(item);
}
#### 适配器模式
1. 使用asList（包装类型数组）可以数组类型转换成List类型
2. Integer[] arr = {1, 2, 3};
List list = Arrays.asList(arr);
或者：
List list = Arrays.asList(1, 2, 3);

## 三 源码分析
#### ArrayList
1. 概览
* 基于数组实现，支持快速随机访问
* RandomAccess接口表示该类支持快速随机访问。
2. 扩容
* 使用ensuerCapacityInternal（）方法保证容量足够。如果不够时，用grow方法进行扩容。新容量大小是原本的1.5倍，且需要将原数组复制到整个新数组中。
3. 删除元素
* 使用Arraycopy方法将要删除元素后面的元素都复制到前面，操作复杂度是O（N）
4. 序列化
* 序列化：对象变为byte数组
*  保存元素的数组 elementData 使用 transient 修饰，该关键字声明数组默认不会被序列化
*  writeObject() 和 readObject() 来控制只序列化数组中有元素填充那部分内容
*  序列化时需要使用 ObjectOutputStream 的 writeObject() 将对象转换为字节流并输出。writeObject() 方法在传入的对象存在 writeObject() 的时候会去反射调用该对象的 writeObject() 来实现序列化反序列化使用的是 ObjectInputStream 的 readObject() 方法
5. Fail-Fast
记录数组结构发生变化的次数。在进行序列化或者迭代等操作时，需要比较操作前后 modCount 是否改变，如果改变了需要抛出 ConcurrentModificationException

#### Vector
1. 同步——使用synchronize进行同步
2. 扩容——构造函数可以传入 capacityIncrement 参数，它的作用是在扩容时使容量 capacity 增长 capacityIncrement。如果这个参数的值小于等于 0，扩容时每次都令 capacity 为原来的两倍
3. 比较——vector是同步的，访问速度更慢；扩容请求是两倍，ArrayList是1.5倍

#### CopyOnWriteArrayList
1. 读写分离
* 写操作在一个复制的数组上进行，读操作在原始操作上进行，读写分离
* 写操作需要加锁
* 写操作结束后需要把原始操作指向新的复制数组
2. 适用场景
* 在写的时候允许读，适合读多写少的场景
* 但是会占用内存，并且数据不能实时性被读数组进行读取。

#### LinkedList 
1. 基于双向链表实现
2. 与ArrayList相比，数组支持随机访问，但是插入删除的代价很高，需要移动大量元素
3. 链表不支持随机访问，但是插入删除只需要改变指针
#### HashMap
1. 存储结构
* 内部有一个Entry类型的数组。Entry存储键值对。数组中每个位置中存放一个链表。
* 使用拉链法来解决冲突。

2. 拉链法工作原理
* 新建一个HashMap，默认大小为k
* 插入键值对<K,V>,计算k的hashcode，再计算 桶下标等于hashcode%桶大小。
* 链表的插入是头插法。
* 查找时：先计算键值对所在的桶。然后在链表上进行顺序查找。时间复杂度和链表的长度成正比
3. put操作
* 允许插入键为null 的键值对，使用第0个桶存放键为null 的键值对
* 使用头插法进行插入

【
* 通过hash() 计算出键的hashcode， 再通过indexFor() 方法计算出对应的索引
* 如果散列表为空, 调用resize()初始化散列表
* 添加元素到散列表中 
如果没有发生hash碰撞, 直接添加到散列表中
如果发生碰撞 
** 无论是红黑树还是链表, 如果equals相同, 则替换旧的键值对
** equals不同 
****    链表, 循环遍历整个链表, 知道链表的某个节点为空(jdk1.8尾插法, 但是在jdk1.7采用的是头插法会引起并发死锁), 查看链表长度是否达到8以及容量是否大于64, 如果满足条件, 将链表转化成红黑树
****  红黑树插入方法
* 如果元素个数大于临界值(数组容量* loadFactor), 则通过resize()进行扩容】

4. 确定桶下标
* 计算hash值
* 取模：令 x = 1<<4，即 x 为 2 的 4 次方。
令一个数 y 与 x-1 做与运算，可以去除 y 位级表示的第 4 位以上数。这个性质和 y 对 x 取模效果是一样的。
5. 扩容基本原理
* HashMap 的 table 长度为 M，需要存储的键值对数量为 N，如果哈希函数满足均匀性的要求，那么每条链表的长度大约为 N/M，因此查找的复杂度为 O(N/M)
* 需要保证M尽可能大。采用动态扩容来调整。
* 当需要扩容时，容量变为原来的两倍。默认是16，必须是2的n次方
* 扩容用resize（）实现。需要将旧表的所有键值对重新插入到新的中。

【扩容resize():
1.初始化数组table
2.当数组table的size达到阙值时即++size > load factor * capacity 时，也是在putVal函数中
具体实现
1.通过判断旧数组的容量是否大于0来判断数组是否初始化过
否：进行初始化
判断是否调用无参构造器， 
是:使用默认的大小和阙值
否:使用构造函数中初始化的容量，当然这个容量是经过tableSizefor计算后的2的次幂数
是，进行扩容，扩容成两倍(小于最大值的情况下)，之后在进行将元素重新进行与运算复制到新的散列表中】

6. 扩容重新计算桶下标
* 原本为0，扩容后桶位置与原来一致
* 原本为1，扩容后是原位置+16

7. 计算数组容量
先求出掩码

8. jdk1.8后，当一个桶存储的链表长度大于等于8时会将链表转换成红黑树。
9. 与HashTable的比较
* Hashtable 使用 synchronized 来进行同步。
* HashMap 可以插入键为 null 的 Entry。
* HashMap 的迭代器是 fail-fast 迭代器。
* HashMap 不能保证随着时间的推移 Map 中的元素次序是不变的。

#### ConcurrentHashMap
1. 存储结构
*  与HashMap类似。采用了分段锁（segment），每个分段锁维护着几个桶（HashEntry），多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高（并发度就是 Segment 的个数）
* 默认是16

2. size操作
* 每个 Segment 维护了一个 count 变量来统计该 Segment 中的键值对个数
* 执行 size 操作时，需要遍历所有 Segment 然后把 count 累计起来

3. jdk1.8
使用了 CAS 操作来支持更高的并发度，在 CAS 操作失败时使用内置锁 synchronized

#### LinkedHashMap
1. 存储结构
* 继承自 HashMap，因此具有和 HashMap 一样的快速查找特性
* 内部维护了一个双向链表，用来维护插入顺序或者 LRU 顺序
* afterNodeAccess和afterNodeInsertion用于维护顺序
2. afterNodeAccess（）
如果一个节点被访问时，该值为true，就会将该节点移到链表尾部。保证链表尾部都是最近访问的节点。链表首部是最近最久未使用的节点
3. afterNodeInsertion（）
* 在put操作后执行，当 removeEldestEntry() 方法返回 true 时会移除最晚的节点，也就是链表首部节点 first
* removeEldestEntry() 默认为 false，如果需要让它为 true，需要继承 LinkedHashMap 并且覆盖这个方法的实现。
5. LRU缓存

* 设定最大缓存空间 MAX_ENTRIES 为 3；
* 使用 LinkedHashMap 的构造函数将 accessOrder 设置为 true，开启 LRU 顺序；
* 覆盖 removeEldestEntry() 方法实现，在节点多于 MAX_ENTRIES 就会将最近最久未使用的数据移除。
#### WeakHashMap
1. 存储结构：用来实现缓存，通过使用 WeakHashMap 来引用缓存对象，由 JVM 对这部分缓存进行回收
2. ConcurrentCache
ConcurrentCache 采取的是分代缓存：

* 经常使用的对象放入 eden 中，eden 使用 ConcurrentHashMap 实现，不用担心会被回收（伊甸园）；
* 不常用的对象放入 longterm，longterm 使用 WeakHashMap 实现，这些老对象会被垃圾收集器回收。
* 当调用 get() 方法时，会先从 eden 区获取，如果没有找到的话再到 longterm 获取，当从 longterm 获取到就把对象放入 eden 中，从而保证经常被访问的节点不容易被回收。
* 当调用 put() 方法时，如果 eden 的大小超过了 size，那么就将 eden 中的所有对象都放入 longterm 中，利用虚拟机回收掉一部分不经常使用的对象。