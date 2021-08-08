1. Set和List对比：
    * List特点：（解决顺序问题）元素有放入顺序，元素可重复;和数组类似，List可以动态增长；List集合默认按元素的添加顺序设置元素的索引，例如第一个添加的元素索引为0，第二个添加的元素索引为1......
    * Set特点：（元素独一无二）元素无放入顺序，元素不可重复，重复元素会盖掉，（注意：元素虽然无放入顺序，但是元素在set中位置是由该元素的HashCode决定的，其位置其实是固定，加入Set 的Object必须定义equals()方法;
    * Map（Key值搜索）：键值对存储，Key值不能重复，但能引用相同的对象，实现类有HashMap、ConcurrentHashMap等。
2. 线程安全集合类与非线程安全集合类
    * LinkedList、ArrayList、HashSet是非线程安全的，Vector是线程安全的;
    * HashMap是非线程安全的，HashTable是线程安全的;
    * StringBuilder是非线程安全的，StringBuffer是线程安的。
3. ArrayList与LinkedList的区别和适用场景
    * Arraylist：基于动态数组、当需要对数据进行对此访问的情况下选用ArrayList
    * LinkedList：基于链表的数据结构、当要对数据进行多次增加删除修改时采用LinkedList。
    * 正常使用中，不会频繁增删，不会多线程，所以选ArrayList。频繁增删就用LinkedList。线程安全就用Vector。
    * 底层数据结构：ArrayList底层是Object动态数组；LinkedList底层使用双向链表
    * 随机访问和插入删除元素：ArrayList支持随机访问（相当于get(int index)方法）但删除插入元素慢；LinkedList不支持随机访问但支持随机插入删除元素。
    * 内存空间占用：ArrayList必须预留一定的空间；LinkedList要存储前驱后继和节点信息，开销大。
4. ArrayList和LinkedList怎么动态扩容的吗？
    * ArrayList 初始化大小是 10 。 扩容规则是，新增的时候发现容量不够用了，就去扩容 扩容大小规则是，扩容后的大小= 原始大小+原始大小/2 + 1。（例如：原始大小是 10 ，扩容后的大小就是 10 + 5+1 = 16）
    * linkedList 是一个双向链表，没有初始化大小，也没有扩容的机制，就是一直在前面或者后面新增就好。
5. HashSet与TreeSet的区别和适用场景
    * TreeSet 是二叉树（红黑树的树据结构）实现的，数据是自动排好序的，不允许放入null值。
    * HashSet 是哈希表实现的，放入的对象必须实现HashCode()方法，放的对象，是以hashcode码作为标识的，数据是无序的，只能放入一个null值。
    * 快速查找使用HashSet，排序使用TreeSet
6. HashMap与TreeMap、HashTable的区别及适用场景
    * HashMap：基于哈希表(散列表)实现。使用HashMap要求的键类明确定义了hashCode()和equals()[可以重写hasCode()和equals()]，其中散列表的冲突处理主分两种，一种是开放定址法，另一种是链表法。HashMap实现中采用的是链表法。
    * TreeMap：非线程安全基于红黑树实现。
7. set集合从原理上如何保证不重复？
    * 在往set中添加元素时，如果指定元素不存在，则添加成功。
    * 当向HashSet中添加元素的时候，首先计算元素的hashcode值，然后用这个（元素的hashcode）%（HashMap集合的大小）+1计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加。
8. ConcurrentHashMap 1.8原理：
    * CAS：如果obj内的value和expect相等，就证明没有其他线程改变过这个变量，那么就更新它为update，如果这一步的CAS没有成功，那就采用自旋的方式继续进行CAS操作。
    * put方法
      * 根据 key 计算出 hashcode 。
      * 判断是否需要进行初始化。
      * 如果当前 key 定位出的 Node为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
      * 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。
      * 如果都不满足，则利用 synchronized 锁写入数据。
      * 最后，如果数量大于 TREEIFY_THRESHOLD 则要转换为红黑树。
    * get方法
      * 根据计算出来的 hashcode 寻址，如果就在桶上那么直接返回值。
      * 如果是红黑树那就按照树的方式获取值。
      * 不满足那就按照链表的方式遍历获取值。
9.   HashMap何时扩容：
    * 当向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值---即大于当前数组的长度乘以加载因子的值的时候，就要自动扩容。
10.  扩容的算法是什么：
    * 扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。方法是使用一个新的数组代替已有的容量小的数组。
11.  Hashmap如何解决散列碰撞（必问）？
    * HashMap使用链表来解决碰撞问题，当碰撞发生了，对象将会存储在链表的下一个节点中。hashMap在每个链表节点存储键值对对象。当两个不同的键却有相同的hashCode时，他们会存储在同一个bucket位置的链表中。
12. Map接口
* HashMap
  * HashMap是基于Hash算法实现的由数组和链表组合的数据结构，允许使用null值和null键。
  * 用数组存储，将冲突的key对象放入链表中，再发生冲突就在链表中顺序做对比。
  * 插入元素
     * 利用key的hashCode计算出当前对象的元素在数组中的下标；
     * 如果位置上没有元素，就直接插入；
     * 如果位置上有元素，则比较key，若key相同，则覆盖原始值；若key不同，先判断是否是树节点，是就用putTreeVal() 添加元素，不是就拉链法插入链表。
     * 若插入后链表长度 > 8，转成红黑树或者扩容。
     * java8前用头插法，新来的值代替原有值，原有值被往后推。问题是，扩容后容易形成环形链表。
     * java8后用尾插法。在扩容同时保证链表元素原来的顺序，避免形成环。
     * 对key的hashCode()做hash，然后再计算index;
     * 如果没碰撞直接放到bucket里；
     * 如果碰撞了，以链表的形式存在buckets后；
     * 如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD)，就把链表转换成红黑树；
     * 如果节点已经存在就替换old value(保证key的唯一性)
     * 如果bucket满了(超过load factor*current capacity)，就要resize。
  * 读取元素
     * get元素时，找hash值对应的下标，在进一步判断key是否相同，从而找到对应值。
     * 首先将 key hash 之后取得所定位的桶。
     * 如果桶为空则直接返回 null 。
     * 否则判断桶的第一个位置(有可能是链表、红黑树)的 key 是否为查询的 key，是就直接返回 value。
     * 如果第一个不匹配，则判断它的下一个是红黑树还是链表。
     * 红黑树就按照树的查找方式返回值。
     * 不然就按照链表的方式遍历匹配返回值。
     * 若为树，则在树中通过key.equals(k)查找，O(logn)；
     * 若为链表，则在链表中通过key.equals(k)查找，O(n)。
  * 多线程
    * hashmap线程不安全。put/get方法没有同步锁，容易出现上一秒put值，下一秒get的还是原值。
  * 初始化
    * new HashMap() 不传值，默认为16，负载因子为0.75。
    * 传k，则为 大于k的2的整数次方。比如传10，大小为16。
  * hash函数设计
    * 拿到 key 的32位 hashCode，将hashCode高16位与低16位异或。
    * 降低哈希碰撞，混合高半区和低半区的特征，加大低区随机性；
    * 之后与 length-1，进行与运算，获得最低的 x 位，即下标。
* HashMap和HashTable的区别
  * 线程安全：HashMap线程不安全；HashTable内部使用synchronized关键字，线程安全。
  * Null Key支持：HashMap中null可作为Key；HashTable中null不能作key也不能作value。
  * 效率：HashMap比HashTable效率高一点，而且HashTable基本淘汰了。
  * 初始容量：HashMap是16，HashTable是11。
  * 扩容机制：HashMap是当前容量翻倍，Hashtable是当前容量翻倍+1。
  * 为什么HashTable的null不能做key和value
    * HashTable在put空值时会抛空指针异常。HashMap做了三目运算的处理，null就设0。
    * 安全失败机制。让此次读取的数据不一定是最新，同时key为null就无法判断key是不存在还是空。
  * 线程不安全
    * HashMap线程不安全。因为多线程环境下扩容，导致hash规则变化，可能形成环形链表，死循环。
    * 多线程下数据覆盖问题，A线程判断index为空挂起，B线程写入index，这时A线程恢复线程，进行赋值操作，把B线程数据覆盖。
    * HashTable线程安全。因为内部实现put和remove方法时使用synchronized同步，所以对单个方法的使用是线程安全的。但对多个方法复合操作时，无法保证安全性。
* HashMap的底层结构
  * JDK1.8之前，用数组+链表用链地址法实现。所谓拉链法，就是数组链表，数组的每一格就是一个链表，遇到哈希冲突，则将冲突值加入链表。
  * JDK1.8之后使用数组+链表+红黑树实现，解决链表过长而查询速度变慢。
  * hash后算出下标，没有冲突就直接放进node；
  * 有冲突，链地址法，如果产生冲突（碰撞）用链表链接相同hash值的数据；
  * 链表长度>8且数组长度<64，先进行扩容；
  * 链表长度>8且数组长度>64，转为红黑树，加速遍历。
* 扩容
   * HashMap的初始容量16，加载因子0.75，扩容增量是原容量的1倍。HashMap中的元素个数超过 初始容量16 * 加载因子0.75 = 12 后进行扩容。扩容分为两步。
   * 创建新数组：创建原来HashMap大小两倍的bucket数组。
   * ReHash：遍历原数组，将对象重新hash后放入新数组中。
   * 为啥要rehash
      * 因为长度扩大后，hash规则也改变。key & (length-1)
      * 1.8 前，需要重新hash计算在新数组的位置；
      * 1.8 后，只需要逻辑判断，位置不变或者索引+原容量大小。
      * 扩容是两倍大小，所以相当于 01111 -> 11111，高位为0，则hash值不变，高位为1，则hash值为原索引+16。

   * 长度为什么是2的幂/初始长度为什么是16
     * 首先使用位运算来减少碰撞次数，提高查询效率。，而要使用位运算，就需要数组-1然后与hash值保证其在数组范围内，只有当数组长度为2的指数次幂时，其计算得出的值才能和取模算法的值相等，并且保证能取到数组的每一位，减少哈希碰撞，不浪费大量的数组资源
     * hash%length == hash&(length-1)的前提是 length 是2^n。
     * 2的幂时，length-1所有位都是1，哈希结果是hashCode后四位的值，只要hashcode本身均匀，hash结果就均匀。即实现均匀分布。
     * eg.length为15，则length-1为14，对应二进制为1110，进行与操作后，最后一位为0，则最后一位为1的位置都不能存放元素。
     * 当hashmap的容量是2的幂时，n-1的2进制与hash值进行与运算时，能够进行充分的散列，即不同的hash值与n-1进行位运算后都能得到不同的值，使得添加的元素能够均匀分布在集合不同的位置上，避免hash碰撞。而不是2的幂时，n-1与hash值会有重复，碰撞概率很高。
   * 加载因子为什么是0.75
       * 从结果来推导，加载因子为0.75时，能保证和任何2的幂乘积结果都是整数，即负载因子*容量的结果是一个整数，
* ConcurrentHashMap——CAS+自旋+synchronized锁
   * 底层数据结构：ConcurrentHashMap是数组+链表+红黑树
   * 实现线程安全：
      * 在JDK1.8之前，ConcurrentHashMap使用分段锁将Hash表分割为16个桶，每个分段锁维护着几个桶，多线程访问不同分段锁上的桶，提高并发度。(并发度就是分段锁个数)
      * 在JDK1.8之后，使用Node数组+链表+红黑树的数据结构，并发控制用synchronized和CAS操作。
   * synchronized锁定当前链表/红黑树的头节点，只要hash不碰撞，就不会并发，效率大幅提升。
   * put操作流程
      * 根据 key 计算出 hashcode 。
      * 判断是否需要进行初始化。
      * 即为当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
      * 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。
      * 如果当前桶不为空，则利用 synchronized 锁写入数据。
      * 如果数量大于 TREEIFY_THRESHOLD 则要转换为红黑树。
   * get操作流程
      * 根据算出的hashcode寻址，在桶上就直接返回值。
      * 如果是红黑树，就按树的方式获取值。
      * 都不满足，就是按链表遍历获取值。
   * 为什么用synchronized
      * JAVA8优化了同步锁，最初都是轻量级锁慢慢升级为重量级锁。
      * 先用偏向锁，优先同一线程获取锁。
      * 失败，就升级为CAS，失败后短暂自旋。
      * 都失败，就升级为重量级锁。
* LinkedHashMap和LinkedHashSet
    * LinkedHashMap能记录元素的插入顺序和访问顺序。
    * 内部通过双向链表，保证元素的插入顺序。
    * LinkedHashSet底层使用LinkedHashMap实现。
    * 二者关系类似HashMap和HashSet。
* HashSet,HashMap和TreeSet区别
    * HashMap底层使用Hash表实现，通过元素的hashCode值和equals()方法保证元素唯一性；
    * TreeSet底层使用红黑树实现，通过comparable或comparator接口保证元素唯一性；
    * HashSet底层是基于HashMap实现的，基本都是调用hashmap的方法。
* TreeMap/红黑树
    * TreeMap存储键值对，底层是红黑树，可以实现元素的自动排序。了解 TreeMap 必须理解红黑树！
    * 红黑树是一种二叉搜索树，也是均衡二叉树，当不满足红黑树规则时，自动调整节点平衡。
    * 规则有：
        * 节点分为红色和黑色；
        * 根节点必为黑色；
        * 叶子节点都为黑色，且都为null；
        * 连接红色节点的两个子节点都为黑色，即不出现相邻的红色节点；
        * 从任意节点出发，到任意叶子节点的路径中包含相同数量的黑色节点；
        * 新加入红黑树的节点都是红色节点。
        * 维持平衡的方式：变色，左旋，右旋。具体操作
1.  List接口
   * 多线程下的ArrayList
      * ArrayList不是线程安全的。需要通过Collections.synchronizedList()方法转换成线程安全容器。
   * 扩容
      * 新建一个原始长度 * 1.5的数组；oldCapacity + (oldCapacity >> 1)
      * 把原数组的数据复制到新数组中；
      * 最后更新地址。
   * 新增元素
      * 判断长度，不够就扩容；
      * 新建数组，复制原数组，在index位开始把元素放进index+1位；
      * 最后在index放入新增元素。
   * 删除元素
      * 新建数组，复制原数组，跳过index位，把index+1位放在index位；
      * 相当于被覆盖。
   * CopyOnWriteArrayList
      * 读写分离，写操作在复制的数组上，读操作在原始数组上。
      * 写操作因为是并发所以需要加锁，结束后指针指向复制数组。
      * 在写操作的同时允许读操作，适合读多写少的场景。
      * 缺点：
      * 内存敏感，因为写操作要复制数组；
      * 数据不一致：读操作不能读到最新数据，因为写操作还未同步到数组中。
15. Set和Queue
   * HashSet原理
      * 基于HashMap实现，HashSet值存放在HashMap的key上，value统一为PRESENT。HashSet通过比较hash值和equals()判断key是否重复。底部基本都是调用HashMap的方法。
16. 关于扩容
  * HashMap：16 0.75，大于16*0.75进行扩容，新建一个两倍容量的数组，将原数组元素rehash后放入
  * HashTable：初始为11，扩容为double+1
  * ArrayList：新建一个长度是1.5倍的数组，将原数据复制，最后更新地址
17. 集合和数组的区别
  * 数组的长度在初始化后指定，集合的长度可以动态增加。
  * 数组存储的元素是基本类型的值或者是对象，集合存储的类型是对象，基本类型的数据要转换成相应的包装类才能放入集合中。
18. ArrayMap总结
    * 数据格式：
        * 内部是两个数组。一个是记录所有key的哈希值的数组从小到大排列mHashes。一个是记录key—value键值对的数组mArray，是前一个数组的两倍
        * mBaseCache：用于缓存大小为4的ArrayMap，mBaseCacheSize记录着当前已缓存的数量，超过10个则不再缓存
        * mTwiceBaseCache：用于缓存大小为8的ArrayMap，mTwiceBaseCacheSize记录着当前已缓存的数量，超过10个则不再缓存。
    * 缓存机制
        * 两个缓存池，分别缓存大小为4和8的ArrayMap对象
        * 内存释放时机：
          * 移除最后一个元素
          * clear进行清理
          * put容量满时，先分配内存再释放内存
        * 内存释放操作：
          * 如果同时满足释放的array大小等于4或者8，且相对应的缓冲池个数未达上限，则会把该mArray数组加入到缓存池中
          * array[0]指向原来的缓存池，mBaseCache或者mTwiceBaseCache
          * array[1]指向mHashes的数组的地址，
          * 第二个元素后的数据全部置为null
          * 缓存池的头部指向最新的array的位置，并将缓存池的大小加1
        * 分配内存时机：
          * 执行构造函数
          * 当执行put()在容量满的情况下, 先执行allocArrays, 再执行freeArrays
          * 当执行ensureCapacity()在当前容量小于预期容量的情况下, 先执行allocArrays,再执行freeArrays
        * 分配内存操作：
          * 如果所需要分配的大小等于4或者8，且相对应的缓冲池不为空，则会从相应缓存池中取出缓存的mArray和mHashes。
          * 将当前缓存池赋值给mArray
          * 将缓存池指向上一条缓存地址
          * 将缓存池的第1个元素赋值为mHashes
          * 再把mArray的第0和第1个位置的数据置为null
          * 并将该缓存池大小执行减1操作
    * 扩容机制
        * 容量扩张：
          * 当mSize大于或等于mHashes数组长度时则扩容，完成扩容后需要将老的数组拷贝到新分配的数组，并释放老的内存。
          * 当map个数满足条件 osize<4时，则扩容后的大小为4
          * 当map个数满足条件 4<= osize < 8时，则扩容后的大小为8
          * 当map个数满足条件 osze>=8时，则扩容后的大小为原来的1.5倍
        * 容量收紧：
          * 当数组内存的大小大于8，且已存储数据的个数mSize小于数组空间大小的1/3的情况下，需要收紧数据的内容容量，分配新的数组，老的内存靠虚拟机自动回收
          * 如果mSize<=8，则设置新大小为8
          * 如果mSize> 8，则设置新大小为mSize的1.5倍
19. HashMap和ConcurrentHashMap
  * ConcurrentHashMap有一个同步竞争机制。存在一个被volatile修饰的变量，当值为负数的时候说明某个线程正在操作这个map，想要去操作这个Map的线程就要一直去竞争这个变量，没有得到这个变量的值就要一直自旋等待这个变量。
20. SpareArray优化
  * key保存在int mKeys[]数组中，相对于HashMap不再对key进行自动装箱，避免资源消耗。但是vaule是保存在Object[] mValues数组中还是需要自动装箱的。
  * 相对于HashMap，不再使用额外的Entry对象来存储数据，减少了内存开销。
  * 数据量小的情况下，随机访问效率更高。（千级以内）
  * 插入流程：
    * key是一个int有序数组，用二分查找法查找元素的key。
    * 如果插入的数据冲突了，则直接覆盖原则。
    * 如果当前插入的key上的数据为DELETE，则直接覆盖。
    * 如果前面几步都失败了，则检查是否需要gc()并且在索引上插入数据。