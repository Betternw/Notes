1. Set和List对比：
    * List特点：元素有放入顺序，元素可重复;和数组类似，List可以动态增长
    * Set特点：元素无放入顺序，元素不可重复，重复元素会盖掉，（注意：元素虽然无放入顺序，但是元素在set中位置是由该元素的HashCode决定的，其位置其实是固定，加入Set 的Object必须定义equals()方法;
2. 线程安全集合类与非线程安全集合类
    * LinkedList、ArrayList、HashSet是非线程安全的，Vector是线程安全的;
    * HashMap是非线程安全的，HashTable是线程安全的;
    * StringBuilder是非线程安全的，StringBuffer是线程安的。
3. ArrayList与LinkedList的区别和适用场景
    * Arraylist：基于动态数组、当需要对数据进行对此访问的情况下选用ArrayList
    * LinkedList：基于链表的数据结构、当要对数据进行多次增加删除修改时采用LinkedList。
4. ArrayList和LinkedList怎么动态扩容的吗？
    * ArrayList 初始化大小是 10 。 扩容规则是，新增的时候发现容量不够用了，就去扩容 扩容大小规则是，扩容后的大小= 原始大小+原始大小/2 + 1。（例如：原始大小是 10 ，扩容后的大小就是 10 + 5+1 = 16）
    * linkedList 是一个双向链表，没有初始化大小，也没有扩容的机制，就是一直在前面或者后面新增就好。
5. HashSet与TreeSet的区别和适用场景
    * TreeSet 是二叉树（红黑树的树据结构）实现的，
    * HashSet 是哈希表实现的，放入的对象必须实现HashCode()方法，放的对象，是以hashcode码作为标识的，
6. HashMap与TreeMap、HashTable的区别及适用场景
    * HashMap：基于哈希表(散列表)实现。使用HashMap要求的键类明确定义了hashCode()和equals()[可以重写hasCode()和equals()]，其中散列表的冲突处理主分两种，一种是开放定址法，另一种是链表法。HashMap实现中采用的是链表法。
    * TreeMap：非线程安全基于红黑树实现。
7. set集合从原理上如何保证不重复？
    * 在往set中添加元素时，如果指定元素不存在，则添加成功。
    * 当向HashSet中添加元素的时候，首先计算元素的hashcode值，然后用这个（元素的hashcode）%（HashMap集合的大小）+1计算出这个元素的存储位置，如果这个位置为空，就将元素添加进去；如果不为空，则用equals方法比较元素是否相等，相等就不添加，否则找一个空位添加。
8. HashMap 1.8的原理：
    * 当 Hash 冲突严重时，在桶上形成的链表会变的越来越长，这样在查询时的效率就会越来越低；时间复杂度为 O(N)，因此 1.8 中重点优化了这个查询效率。
    * 与7的区别：TREEIFY_THRESHOLD 用于判断是否需要将链表转换为红黑树的阈值。HashEntry 修改为 Node。
    * put 方法：
      * 对key的hashCode()做hash，然后再计算index;
      * 如果没碰撞直接放到bucket里；
      * 如果碰撞了，以链表的形式存在buckets后；
      * 如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD)，就把链表转换成红黑树；
      * 如果节点已经存在就替换old value(保证key的唯一性)
      * 如果bucket满了(超过load factor*current capacity)，就要resize。
    * get方法
      * 首先将 key hash 之后取得所定位的桶。
      * 如果桶为空则直接返回 null 。
      * 否则判断桶的第一个位置(有可能是链表、红黑树)的 key 是否为查询的 key，是就直接返回 value。
      * 如果第一个不匹配，则判断它的下一个是红黑树还是链表。
      * 红黑树就按照树的查找方式返回值。
      * 不然就按照链表的方式遍历匹配返回值。
      * 若为树，则在树中通过key.equals(k)查找，O(logn)；
      * 若为链表，则在链表中通过key.equals(k)查找，O(n)。
1. ConcurrentHashMap 1.8原理：
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
2.  HashMap何时扩容：
    * 当向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值---即大于当前数组的长度乘以加载因子的值的时候，就要自动扩容。
3.  扩容的算法是什么：
    * 扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。方法是使用一个新的数组代替已有的容量小的数组。
4.  Hashmap如何解决散列碰撞（必问）？
    * HashMap使用链表来解决碰撞问题，当碰撞发生了，对象将会存储在链表的下一个节点中。hashMap在每个链表节点存储键值对对象。当两个不同的键却有相同的hashCode时，他们会存储在同一个bucket位置的链表中。

