## 一 数据类型
#### 基本类型

* byte/8：1字节
* char/16：2字节
* short/16：2字节
* int/32：4字节
* float/32：4字节
* long/64：8字节
* double/64：8字节
* boolean/~：1字节
#### 包装类型
1. 基本类型与其对应的包装类型之间使用自动装箱和拆箱完成
2. 装箱：Integer.valueOf
3. 拆箱：X.intValue
#### 缓存池
1. new Integer（123）：会新建一个对象
2. Integer.valueOf（123）：先判断值是否在缓存池中，如果存在，会使用缓存池中的对象，多次调用会取得同一个对象的引用。
3. 缓存池大小为-128~127

## 二 String
#### 概览
1. String属于final，不可以被继承
2. Integer也不可以被继承
3. java8中，String内部使用char数组存储数据
4. java9之后，String内部用byte数组存储字符串。数组的存储被声明为final，string内部也没有改变数组的方法，因此可以保证String不可变
#### 不可变的好处
1. 可以缓存hash值。
2.  如果一个string对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。
3.  作为参数，string不变可以保证参数不变，具有安全性。
4.  在线程中使用具有安全性。

#### String，StringBuffer，StringBuilder
1. String不可变，其余可变
2. String不可变，线程安全。StringBuffer内部同步，线程安全。StingBuilder不是线程安全的。
3. 如果字符串需要更改，且是单线程，使用StringBuilder

#### String Pool
1. 字符串常量池
2. 保存着所有字符串字面量。使用intern（）方法在运行过程中将字符串添加到String Pool中
3. 当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

#### new String（“abc”）
1. 使用new方法会在堆中创建一个字符串对象，属于字符串字面量
2. 在编译期间会在String Pool中创建一个字符串对象指向字面量。
3. 也就是说使用new方法会创建两个字符串对象（pool中之前没有）

## 三 运算
#### 参数传递
1. java的参数是以**值传递**的形式传入方法中
2. 在将一个参数传入一个方法时，本质上是将对象的地址以值的方式传递到形参中。
3. 方法外调用，方法内改变调用的值，那么对象的值会改变，因为是引用的同一个对象

public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    Dog oldDog = aDog;
    foo(aDog);
    aDog.getName().equals("Fifi"); // true
    // but it is still the same dog:
    aDog == oldDog; // true}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true
    d.setName("Fifi");}
4. 方法外调用，方法内调用（新建了别的对象，将指针引用了其他对象），方法外对象的值不会改变，因为此时方法内外的两个指针指向了不同的对象。

public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    Dog oldDog = aDog; 
    aDog.getName().equals("Max"); // true
    aDog.getName().equals("Fifi"); // false
    aDog == oldDog; // true}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true
    d = new Dog("Fifi");
    d.getName().equals("Fifi"); // true}
    
 5. 也就是说，方法内外的改变，对象的值都会改变。但是方法内新建对象，那么对原来对象的值没有影响

#### float和double
1. double不能到float进行向下转型
2. float f = 1.1  错误，因为1.1属于double，不能向下转型到float，因此不能直接赋值
3. float f = 1.1f 1.1f属于float类型

#### 隐式类型转换
1. 精度高的类型不能隐式的往精度低的类型转换
2. 可以使用+= 或者 ++ 进行向下转型
short x = 3;
x += 4.6;
等价于：
short x = 3;
x = (short)(x + 4.6);  x的结果是7

#### switch
1. switch中的判断条件可以使用string：switch（s）

## 四 关键字
#### final
1. 数据被声明成final类型后不再改变
2. 方法被声明成final后不能被子类重写
3. 类被声明成final后不允许被继承

#### static
1. 静态变量=类变量。可以直接通过类名访问。不用通过对象来访问。
2. 静态方法：必须有内容。只能访问类的静态字段和静态方法。因此和main方法在一个类的方法都必须是static类型的
3. 静态语句块：在类初始化时运行一次
4. 静态内部类：静态内部类可以直接创建：StaticInnerClass staticInnerClass = new StaticInnerClass();  而非静态内部类必须通过外部类的实例来创建： OuterClass outerClass = new OuterClass();
        InnerClass innerClass = outerClass.new InnerClass();
5. 静态导包
6. 初始化顺序：静态变量和静态语句块优先于其他。静态的初始化顺序取决于代码顺序

## 五 Object通用方法
1. 基本类型在栈中存储数据
2. 引用类型在栈中存放引用，数据存放在堆中。栈中的指针指向堆中的实体。
#### equals（）
1. 等价关系，具有自反性、对称性、传递性、一致性，与null比较为false
2. **基本类型**用==判断值是否相等。没有equals方法
3. **引用类型**用==判断是否引用同一个对象，用**equals**判断引用的值是否相等。

#### hashcode（）
1. 返回哈希值。等价的两个对象散列值一定相同
2. HashSet和HashMap使用这个方法计算对象应该储存的位置。
3. 不相等的对象应当均匀分布到所有可能的哈希值上。要把所有域的值都考虑进来，将每个域都当成 R 进制的某一位，然后组成一个 R 进制的整数，R 一般取 31

#### toString（）
1. 默认返回 ToStringExample@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示

#### clone（）
1. 显式重写clone方法，其他类才能调用该类实例的clone方法
2. 重写clone方法需要继承cloneable接口
3. 赋值、浅拷贝、深拷贝：
![728c95250020cbeb11b4e246719295ff.png](en-resource://database/2785:1)
4. 浅拷贝复制对象的指针不复制本身，新旧对象共享同一块内存。栈中的内存空间号不相同，指向同一块堆空间
![d712f7fff70bef494dade93671a36cd3.png](en-resource://database/2789:1)
其中，如果修改属性中的引用数据类型，两者都会变。如果修改基础数据类型，不会影响。
因为基本数据类型是值传递，直接将值赋值。各自的栈中有各自的基本数据。互相改变不影响。
而引用数据类型是引用传递，中间会存在地址问题。这两个数据指向同一块堆应用，因此修改会影响，
看图就是基本数据类型和引用数据类型存放位置的不同
5. 深拷贝会创造一个一模一样的对象，和原对象不共享内存，修改新对象不会改到原对象
![efc2c1103042cd2d3ab6c6325f2f82ae.png](en-resource://database/2791:1)

6. 赋值，赋的是栈中的地址，两个对象指向同一个存储空间，是联动的。栈中的内存空间号相同，指向的也是同一块堆空间。
![0380339ca6bdd94f3c19d7851d4a51c3.png](en-resource://database/2787:1)
7. 最好使用拷贝构造函数和拷贝工厂来拷贝对象。

## 六 继承
#### 访问权限
1. private public ptotected
#### 抽象类与接口
1. 一个类包含抽象方法那么这个类必须是抽象类
2. 抽象类不能被实例化只能被继承
3. 从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。
4. 使用接口，需要让不相关的类都实现一个方法
5. 使用抽象类需要继承重写抽象方法，继承非静态和非常量字段

#### super
1. 访问父类的构造函数
2. 访问父类的成员：如果子类重写了父类的某个方法

#### 重写和重载
1. 重写：子类实现了一个与父类在方法声明上完全相同的方法

* 子类方法的访问权限必须大于等于父类方法；
* 子类方法的返回类型必须是父类方法返回类型或为其子类型。
* 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。
2. 重载：存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。返回值必须相同

## 七 反射
1. 每个类都有一个class对象，类在第一次使用时动态加载到jvm中。
2. 反射可以提供运行时的类信息
* Field ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
* Method ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
* Constructor ：可以用 Constructor 的 newInstance() 创建新的对象。
3. 反射可以在运行时判断任意一个对象所属的类、构造任意一个类的对象、判断一个类具有的成员变量和方法、运行时调用任意一个对象的方法、生成动态代理
4. 通过反射生成并操作对象
```java
    // 生成新的对象：用newInstance()方法
    Object obj = class1.newInstance();
    //首先需要获得与该方法对应的Method对象
    Method method = class1.getDeclaredMethod("setAge", int.class);
    //调用指定的函数并传递参数
    method.invoke(obj, 28);
```
5. 代理模式
 * 客户端不直接操控原对象，而是通过代理对象间接的操控原对象
 * 静态代理实现思路
   * 代理对象和原对象实现一个共同的接口，并具体实现接口逻辑
   * 代理类的构造函数中实例化一个原对象，并调用原对象的行为接口
   * 客户端通过调用代理类来实现调用目标对象的行为接口
 * 动态代理实现思路（运行时动态生成代理类）
   * Proxy类和InvocationHandler类

## 八 异常
1. Throwable 可以用来表示任何可以作为异常抛出的类，分为两种： Error 和 Exception。其中 Error 用来表示 JVM 无法处理的错误

## 九 泛型
public class Box<T> {
    // T stands for "Type"
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}

1. Java中的泛型是什么 ? 使用泛型的好处是什么? —— 在集合中存储对象并在使用前进行类型转换不方便，泛型提供了编译期的类型安全，确保将正确类型的对象放在集合中
2. Java的泛型是如何工作的 ? 什么是类型擦除  ——泛型是通过类型擦除来实现的，编译器在编译时擦除了所有类型相关的信息，所以在运行时不存在任何类型相关的信息。例如List<String>在运行时仅用一个List来表示。
3.  什么是泛型中的限定通配符和非限定通配符——限定通配符对类型进行了限制。有两种限定通配符，一种是<? extends T>它通过确保类型必须是T的子类来设定类型的上界，另一种是<? super T>它通过确保类型必须是T的父类来设定类型的下界。泛型类型必须用限定内的类型来进行初始化，否则会导致编译错误。另一方面<?>表示了非限定通配符，因为<?>可以用任意类型来替代。
4.  List<? extends T>和List <? super T>之间有什么区别——List<? extends T>可以接受任何继承自T的类型的List，而List<? super T>可以接受任何T的父类构成的List。例如List<? extends Number>可以接受List<Integer>或List<Float>
5.  如何编写一个泛型方法，让它能接受泛型参数并返回泛型类型?—— public V put(K key, V value) {   return cache.put(key, value);  }
6.  可以把List<String>传递给一个接受List<Object>参数的方法吗——因为List<Object>可以存储任何类型的对象包括String, Integer等等，而List<String>却只能用来存储Strings
7.   Array中可以用泛型吗——因为List可以提供编译期的类型安全保证，而Array却不能，不能直接用数组创建泛型

## 十 注解
Java 注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助性的作用。

## 十一 特性
1.  [java8新特性](https://blog.csdn.net/sanri1993/article/details/101176712)
2. java与c++的区别

* Java 是纯粹的面向对象语言，所有的对象都继承自 java.lang.Object，C++ 为了兼容 C 即支持面向对象也支持面向过程。
* Java 通过虚拟机从而实现跨平台特性，但是 C++ 依赖于特定的平台。
* Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针。
* Java 支持自动垃圾回收，而 C++ 需要手动回收。
* Java 不支持多重继承，只能通过实现多个接口来达到相同目的，而 C++ 支持多重继承。
* Java 不支持操作符重载，虽然可以对两个 String 对象执行加法运算，但是这是语言内置支持的操作，不属于操作符重载，而 C++ 可以。
* Java 的 goto 是保留字，但是不可用，C++ 可以使用 goto。

3. JRE和JDK
* JRE：Java Runtime Environment，Java 运行环境的简称，为 Java 的运行提供了所需的环境。它是一个 JVM 程序，主要包括了 JVM 的标准实现和一些 Java 基本类库。
* JDK：Java Development Kit，Java 开发工具包，提供了 Java 的开发及运行环境。JDK 是 Java 开发的核心，集成了 JRE 以及一些其它的工具，比如编译 Java 源码的编译器 javac 等。