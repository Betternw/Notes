1. 你所知道的设计模式有哪些？
   * 创建型模式：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
   * 结构型模式：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
   * 行为型模式：策略模式、模板方法模式、观者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。
2. 通过静态内部类实现单例模式有哪些优点？
    * 不用 synchronized ，节省时间。
    * 调用 getInstance() 的时候才会创建对象，不调用不创建，节省空间，这有点像传说中的懒汉式。
3. 静态代理和动态代理的区别，什么场景使用？
    * 静态代理与动态代理的区别在于代理类生成的时间不同，即根据程序运行前代理类是否已经存在，可以将代理分为静态代理和动态代理。
    * 静态代理使用场景：四大组件同AIDL与AMS进行跨进程通信
    * 动态代理使用场景：Retrofit使用了动态代理极大地提升了扩展性和可维护性。
4. 简单工厂、工厂方法、抽象工厂、Builder模式的区别？
    * 简单工厂模式：一个工厂方法创建不同类型的对象。
    * 工厂方法模式：一个具体的工厂类负责创建一个具体对象类型。
    * 抽象工厂模式：一个具体的工厂类负责创建一系列相关的对象。
    * Builder模式：对象的构建与表示分离，它更注重对象的创建过程。
5. 装饰模式和代理模式有哪些区别 ？与桥接模式相比呢？
    * 装饰模式是以客户端透明的方式扩展对象的功能，是继承关系的一个替代方案；而代理模式则是给一个对象提供一个代理对象，并由代理对象来控制对原有对象的引用。
    * 装饰模式应该为所装饰的对象增强功能；代理模式对代理的对象施加控制，但不对对象本身的功能进行增加。
6. 是否能从Android中举几个例子说说用到了什么设计模式 ？
    * AlertDialog使用了Bulider（建造者）模式完成参数的初始化：
      * 将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。即将配置从目标类中隔离出来，避免过多的setter方法。
      * 创建的对象较复杂，由多个部件构成，各部件面临着复杂的变化，但构件间的建造顺序是稳定的。
    * 日常开发的BaseActivity抽象工厂模式：
      * 抽象的UI元素和具体的UI元素之间的关系就是抽象工厂模式最好的体现。
    * AIDL代理模式：
      * 为其他对象提供一种代理以控制对这个对象的访问。当无法或不想直接访问某个对象或访问某个对象存在困难时可以通过一个代理对象来间接访问，为了保证客户端使用的透明性，委托对象与代理对象需要实现相同的接口。
    * ListView/RecyclerView/GridView的适配器模式：
    * 静态代理使用场景：四大组件同AIDL与AMS进行跨进程通信
    * 动态代理使用场景：Retrofit使用了动态代理极大地提升了扩展性和可维护性。
7. 单例模式
恶汉：类加载的时候就直接实例化好,浪费资源，线程不安全
```java
public class Singleton1 {
    private static final Singleton1 INSTANCE = new Singleton1();
    private Singleton1() {}
    public static Singleton1 getInstance() {
        return INSTANCE;
    }
}
```

懒汉式：线程不安全，因为当进行 new Singleton3()的时候，这个操作不是一个原子操作另一个线程因为instance此时仍然为空，也会进入if判断，那么两个线程继续执行就会产生两个不同的实例。
```java
public class Singleton3 {
    private static Singleton3 INSTANCE = null;
    private Singleton3() {}
    public static Singleton3 getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton3();
        }        
        return INSTANCE;
    }
}
```
懒汉式：线程安全，synchronized修饰方法，范围过大效率低。
```java
public class Singleton4 {
    private static Singleton4 INSTANCE = null;
    private Singleton4() {}
    public static synchronized Singleton4 getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton4();
        }        
        return INSTANCE;
    }
}
```
双重校验：synchronized关键字外部有一层判断，只要实例被创建了，就不会再进入同步代码块，而锁住方法的话每次都会进入同步代码块。
那么锁内部还加一次判断的原因：如果线程A到了new的步骤还没new完，此时变量还是null，就有线程B经过判断后来到syn锁这里，线程A创建完对象后释放锁，线程B没有经过判断又会执行初始化操作。
```java
public class Singleton{
  private volatile static Singleton instance;
  private Singleton(){

  }
  private static Singleton getInstance(){
    if(instance==null){
      synchronized(Singleton.class){
        if(instance==null){
          instance = new Singleton();
        }
      }
    }
    return instance;
  }

}
```
new对象：分配内存给这个对象、初始化这个对象、把INSTANCE变量指向初始化的对象，不是原子操作。使用volatile关键字可以解决重排序的问题
volatile：解决重排序，保证可见性，使得变量的修改能够及时被其他线程所知