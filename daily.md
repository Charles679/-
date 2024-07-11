## 9.18

### 堆

- 定义：
  - 最大堆：每个节点都比子树节点的值要大
  - 最小堆：每个节点都比子树节点的值要小
- 一般基于完全二叉树实现，使用数组存储
- 用途：当我们只关心一堆数据的最大值或最小值时
- 对于有序数组而言，堆的主要优势在于插入和删除数据效率较高
- 插入：从基层做起，逐步向上
- 删除栈顶：删除后调整堆结构，完成堆化
  - 自底向上：空间浪费
  - 自顶向下：将最后一个元素移向堆顶，自顶向下调整，不会产生气泡
- **heapSort**
  - 初始化堆：对所有非叶子结点进行自顶向下的堆化
  - 排序

### K-Merge

合并k个有序链表

![image-20230918120609951](C:\Users\35114\AppData\Roaming\Typora\typora-user-images\image-20230918120609951.png)

- 方法1：以**2-merge**为原型，两两合并链表至结果

- 方法2: 使用priorityQueue

  初始化lists所有头结点到优先队列中

  将优先队列中的最小元素出队列，添加到res链表尾（使用一个指针p对链表尾进行跟踪）

  判断p.next是否为空，不为空则加入优先队列，也就是将出元素后的链表的下一个节点添加到优先队列中

  循环，直至优先队列为空

  

  **思路**：每次都将各个链表最小的元素添加到res链表中，因此我们只考虑**最小元素**，故考虑使用priorityQueue



## 9.19

### 缓存击穿

缓存击穿是指**热点key**在缓存中失效，导致请求大量打到数据库的情况

#### 互斥锁

- setnx 上锁，其他请求查询的进程便无法打到数据库上

  ```java
  setIfAbsent(key,value)
  ```

### 逻辑过期

object转json格式

```java
JsonUtil.toJsonStr(object)
```

### leetcode141 环形链表

判断链表是否成环。

- 使用一个set保存链表节点，将节点逐一添加到set中，如set中存在此节点说明成环。在遍历完成后，若不存在重复节点，说明不成环



## 9.20

### << >> >>>

### continue break return

### a++ ++a

### 8大基本数据类型

### 包装类和基本类的区别

- 包装类的缓存机制

- .equals()方法

- 自动拆装箱

  ```java
  从字节码中，我们发现装箱其实就是调用了 包装类的valueOf()方法，拆箱其实就是调用了 xxxValue()方法。
      因此，Integer i = 10 等价于 Integer i = Integer.valueOf(10)
      int n = i 等价于 int n = i.intValue();
  ```

### 浮点数运算的精度丢失

- 使用BigDecimal避免精度丢失

  - 创建方法

    ```java
     BigDecimal a=new BigDecimal("0.9"); //直接传double会导致精度丢失
     BigDecimal b=BigDecimal.valueOf(0.9);
    ```

  - 加减乘除

  - 比大小

    ```java
    a.compareTo(b);
    ```

  - 保留位数

    ```java
    BigDecimal m = new BigDecimal("1.255433");
    BigDecimal n = m.setScale(3,RoundingMode.HALF_DOWN);
    System.out.println(n);// 1.255
    ```

### 成员变量与局部变量的区别

- 语法
- 存储：
  - 对象：堆
  - 局部变量：栈
- 生存时间：
  - 成员变量：岁对象消亡
  - 局部变量：随方法调用消亡
- 默认值：
  - 成员变量：有默认值
  - 局部变量：无默认值

### 静态变量

static修饰，所有类公用一份，仅分配一次内存

### 静态方法为什么不能调用非静态成员

静态方法属于类，静态方法在类加载时就会分配内存。非静态成员属于对象，只有类实例化后才存在，需要通过对象去访问。

在类的非静态成员还没在内存中存在时，静态方法就已经存在了，因此静态方法调用在内存中还不存在的非静态成员属于非法行为

### 静态方法和非静态方法的异同

- 调用方式：类名.方法名
- 访问权限：静态方法只能访问静态成员

### 重载(overload)和重写(override)的区别

- 重载根据输入数据的不同，作出差异性回应，即参数不同
- 重写输入数据一样，子类作出与父类不同的响应 “两同两小一大”

### 可变长参数

优先匹配固定参数

```java
 public void printf(String... args){
        for(String arg:args){
            System.out.println(arg);
        }
    }
```



## 9.21

### leetcode 62 不同路径

给定m*n矩阵，只能向右或向下移动一格，求移动到最右下角的路径数

dp

### leetcode 121 买卖股票

贪心：第i天购入的最低成本

最大利润初始化为0，PriorityQueue记录第i天前购入股票的最小值，不断比较更新最大利润

**PriorityQueue**默认从小到大排序

### 面向对象和面向过程

### 对象实体与对象引用的区别

### 对象相等（内容）和引用相等（地址）

### 封装、继承、多态

- 多态：父类引用指向子类示例

### 接口和抽象类的异同

- 同：
  1. 无法被实例化
  2. 都可包含default方法
  3. 均可包含抽象方法
- 异：
  1. 只能继承1个类
  2. 抽象类强调所属关系，接口表示遵循某一类行为规范
  3. 接口的成员变量必须声明为public static final，在子类中不可修改

### 浅拷贝、深拷贝与引用拷贝

### ==和equals()

- 所有类都有equals方法，有的类重写了，有的类没重写，没重写相当于==
- ==比较值
- 创建字符串时，虚拟机会在常量池中查找有没有值与所需要创建的对象的值相同的对象，如有，直接进行引用拷贝；如没有就在常量池中新建一个对象

### 为什么hashcode()和equals()要同时重写

- hashcode相等，两对象不一定相等
- 两对象值相同，hashcode不用，则两对象不相等
- 值与hashcode值都相同，说明两对象相等

**hashcode与equals在不涉及散列表时毫不相干，重写hashcode是避免散列表出现对象相同，hashcode不同的情况，造成元素重复**

### String  StringBuffer StringBuilder

- String不可变：1.缓冲数组为private且未提供setter方法 2. final修饰该类，避免了子类破坏String的不可变性
- StringBuffer线程安全，对有关操作加了同步锁；StringBuilder线程非安全

### 字符串常量池



## 9.23

### Exception和Error的异同

- 都继承自Throwable类
- Exception可以被catch，Error不可以

### Checked Exception

### Throwable类的常用方法

### try-catch-finally中的返回值

### try-with-resources

### 项目中哪里用到了泛型？

### 反射

- 获取Class对象的四种方式

  - Apple.class
  - Class.forName("org.example.Apple")
  - apple.getClass()
  - ClassLoader.getSystemClassLoader.().loadClass("org.example.apple")

- 常用API

  ```java
  appleClass.getDeclaredMethods(); //declare获取本类全部，包括私有 method获取本类及超类的全部公有
  
  Apple apple=appleClass.getInstance();
  
  Method method=appleClass.getDeclaredMethod("methodName",String.class,...);
  
  method.invoke(apple,s1,...)
  ```

- 反射的优缺点

### leetcode 169 多数元素

- hashmap
- 摩尔投票：
  - 初始化nums[0]为候选人
  - 与nums[0]相同，票+1；不同，票-1
  - res票必大于0，在某候选人的票为0时，余下数组中必有票>0的，故众数一定存在余下数组元素中



## 10.7

### 全局唯一ID

策略

- UUID
- Redis incr
- 雪花算法
- 数据库自增表

Redis:

**符号位+时间戳+Redis生成的自增id**

```java
细节：
1.时间戳以秒为单位
2.Redis生成自增id:redisStringTemplate.opsForValue.increment(key) //key为日期，能起到统计该日订单数的作用
3.拼接数字：移位运算+或运算
```

### 秒杀下单中的并发问题——超卖

同一秒，多个并发请求，如果为“查询-修改”的线程，可能导致超卖

![image-20231007204444609](C:\Users\35114\AppData\Roaming\Typora\typora-user-images\image-20231007204444609.png)

解决方案：

1. **悲观锁**（syncronized)
   - 由并行变串行，效率低下
2. **乐观锁**(CAS):
   - 在作出数据修改时进行相应的线程安全的判断
   - 如秒杀优惠券时，判断库存数是否>0

## 10.8

### Spring

```java
通过@Component把类注册到容器中，容器包括BDRegistry和Object-cache
容器中存放了多个对象，在容器初始化时完成对象的创建(IOC)//通过反射创建对象
在创建某对象时，发现依赖关系，则进行递归调用创建对象(DI)

Spring中的简单工厂:
//BeanFactory
Car car=context.getBean("car");

Spring中的工厂方法：
//FactoryBean

```

容器是一个concurrentHashMap,k是BeanDefinition,v是Object

## 10.10

### java默认有几个线程

- main线程
- gc线程

线程是操作系统调度运算的最小单位

t1.join()是阻塞其他线程，等待t1执行完

### 线程安全

线程安全是指多线程并发执行情况下，对共享变量的访问是否能保证有效性和一致性

### 保证线程安全的3种方法

1. synchronized自动锁

   **总结：确保只有一个线程执行被同步的代码**

   ```JAVA
   //当多个并发线程访问同一对象中的synchronized代码块时，将只能有一个线程执行，其他线程均被阻塞
   
      //obj 锁定的对象
      synchronized(obj)
      {
         // todo
      }
   //上述代码中，我们用synchronized对obj加了锁这时，当一个线程访问obj对象时，其他试图访问obj对象的线程将会阻塞，直到该线程访问obj对象结束。
   
   //无论synchronized关键字加在方法上还是对象上，如果它作用的对象是非静态的，则它取得的锁是对象；如果synchronized作用的对象是一个静态方法或一个类，则它取得的锁是对类，该类所有的对象同一把锁。
   //每个对象只有一个锁（lock）与之相关联，谁拿到这个锁谁就可以运行它所控制的那段代码。
   //实现同步是要很大的系统开销作为代价的，甚至可能造成死锁，所以尽量避免无谓的同步控制。
   ```

   作用范围：

   1. 修饰代码块，作用在当前对象上
   2. 修饰非静态方法，作用在当前对象上
   3. 修饰静态方法，作用在类的所有对象上（也就是说ob1上锁后0b2不能访问)
   4. 修饰类`synchornized(xxx.class)`，同3

2. lock手动锁

3. volatile关键字

   如果我们将某变量声明为volatile，就说明该变量是共享且不稳定的，每次使用它都到主存中直接读取。

   volatile关键字的作用是保证共享变量在多个线程间的可见性

   **volatile关键字使线程在读取共享变量时直接读取主存中的值，而非工作内存中的值，使共享变量变得多线程间可见**

   **volatile不能保证原子性**

   JMM:

   ![img](https://pic4.zhimg.com/80/v2-a7616a8ed8f589fb8b384babdad4116b_1440w.webp![JMM(Java 内存模型)](https://oss.javaguide.cn/github/javaguide/java/concurrent/jmm.png)

每个线程都在工作内存中对变量值进行修改，而工作内存中的变量是主存的副本（执行run方法时将主存变量拷贝到工作内存中）

线程在对工作内存中的变量值进行修改后，由JMM将这种改动同步到主存

不同的线程间是无法访问对方的工作内存的，线程间的通信（传值）必须通过主存来完成

### 并发编程的三大特性

- 原子性

  一个或一系列操作，要么都得到执行且不会受任何因素的干扰中断，要么都不执行

- 可见性

  一个线程中对共享变量做出修改，其他线程能立即看见该变量被修改

- 有序性

  为提升性能，在执行代码时，会对指令进行重排序，这种重排序能保证串行时的语义一致性，但不能保证并发/多线性时的语义一致性

  volatile禁止指令重排序

### JMM

- happens-before原则
- 与JVM内存模型不同，JMM是并发编程中的抽象概念
- 主内存中保存共享变量（包括成员变量和局部变量），每个线程都有自己的内存区保存一份copy

### 双重校验锁实现单例模式

单例模式是指一个类仅提供一个实例进行全局的访问（共享资源、控制某些资源的特定访问）

有两种实现：

1. 懒汉模式，又称懒加载，只在第一次被引用时才会创建对象，可以通过同步方法和双重锁校验来实现

```java
public class LazySingleton {
    private static LazySingleton instance;
    
    private LazySingleton() {
        // 私有构造函数，防止外部实例化
    }
    
    public static synchronized LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}

```

2. 饿汉模式

   这种情况下，实例在类创建时就被加载，属于天然的线程安全

```java
public class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();
    
    private EagerSingleton() {
        // 私有构造函数，防止外部实例化
    }
    
    public static EagerSingleton getInstance() {
        return instance;
    }
}

```

```java
public class Singleton {
    private volatile Singleton singleton;

    public Singleton getSingleton(){
        if(singleton==null){
            synchronized (Singleton.class){
                singleton=new Singleton();
            }
        }
        return  singleton;
    }
}
```

- 其中synchronized关键字保证了1个线程在创建Singleton对象时其他线程不会执行创建对象的过程，确保了单例

- 而synchronized关键字则是禁止了指令重排列

  如果不禁止重排列的话，因为对象的创建分3阶段

  1. 为singleton分配内存空间

  2. 初始化singleton

  3. 将singleton指向内存空间

     重排列以后，可能执行顺序为1->3->2，此时如果线程t1完成1,3后，线程t2调用get方法，发现singleton不为空，则会返回没有初始化的实例，造成数据错误

## 10.11

### 悲观锁

悲观锁假定每次对共享变量的访问都会造成安全问题，因此每次涉及到共享变量访问时都会上锁。

高并发情况下，激烈的锁竞争会导致大量线程阻塞，从而导致频繁的上下文切换，增加系统性能开销。

悲观锁有可能陷入死锁且



### 乐观锁

乐观锁假定总是最好的情况，即每次对共享变量的访问都不会出现安全问题，因此不会出现阻塞等待的情况，也不会出现死锁，只在提交修改的时候验证**共享资源**是否被其他线程修改过



**乐观锁主要用于写较少场景，悲观锁主要用于写较多场景，悲观锁的开销是固定的**

 ### 乐观锁的实现

1. 版本号

   通常是在数据库中将数据添加一个version字段，每次修改数据后将version值+1，在最后进行提交时比较当前要提交的version值是否与数据库中的version值相同

2. CAS（compare and set）

   在每次对数据进行更新操作前，将此数据未更新时应该得到的值（即期望值）与数据的实际值进行对比，如果相等则继续操作，如果不等则当前线程放弃数据更新，但允许该线程多次重复尝试

​     

### 乐观锁的ABA问题

A->其他值->A,这样线程会以为A值没有修改过

解决方法是加版本号或时间戳



### Synchronized底层实现原理

- 同步代码块

  ```java
  monitorenter //获取对象监视器monitor的持有权，尝试获取锁
      //锁计数器为0则可获取，将锁计数器值+1，否则阻塞，直到其他线程释放锁
      ...
  monitorexit
      //释放锁，将所计数器置0
  ```

- 方法

  ```java
  ACC_SYNCHORNIZED
  ```

  **本质都是对对象监视器monitor的获取**

  

 ### 锁的升级

  基本概念：对象头，MarkWord

  CAS实际上是对MarkWord进行CAS

  锁的升级是单向的，即只能 无锁->偏向锁->轻量锁->重量锁

  1. 无锁

     无锁的mw记录的是hashcode

  2. 偏向锁
     偏向锁针对的是同一线程访问同一同步代码块；当该线程再次进行访问同步代码块时，会判断markword是否为当前线程的线程id，如果是，则获取锁成功，...
     偏向锁在没有别的线程竞争时，一直偏向当前线程，当前线程能一直进行；不涉及操作系统，仅涉及MarkWord

  3. 轻量级锁

     偏向锁在已有一个线程进入同步代码块的情况下，有另一线程想要访问同步代码块，此时偏向锁会升级为轻量锁，Markword指向栈顶

  4. 重量级锁：
     多线程场景下，MarkWord指向monitor指针

## 10.12

### 自旋锁

在多核CPU系统中，最重要的是时间分片，同一时间片只能有一个线程执行；那么其他的线程怎么办呢？

- 一种解决办法是让该线程循环等待，直至另一线程将锁资源释放，称为**自旋锁**
- 另一种解决办法是将其阻塞，等待重新调度请求，称为**互斥锁**

自旋锁避免了进程调度和线程切换

适应性自旋锁：规定了自旋时间

- 优点：在锁竞争较小，时间较短时，自旋锁的消耗小于线程阻塞并唤醒的消耗，避免了线程上下文的切换开销，能带来性能的大幅提升
- 缺点：锁竞争较大时，即多线程和线程占用cpu时间较长的情况，此时自旋的线程一直占用cpu资源却没有做任何动作，造成cpu的浪费，自旋的开销大于线程阻塞唤起的开销，性能降低，需要关闭自旋锁

**因此自旋锁适用于锁竞争不激烈，锁持有时间短的情况**

### AQS

java中，有两种锁，一种是synchronized修饰的内置锁/监视器锁，另一种是J2SE1.5后JUC包下的各种同步器。这些同步器的底层都是基于AQS来实现的，而AQS的核心是一种名为CLH锁的数据结构

### CLH

CLH将线程排成队列，让先来的线程先获取锁，避免了饥饿的发生

CLH的每个Node记录了线程和获取锁的情况，维护了一个队尾指针Tail,在每次线程尝试获取锁时，对Tail进行getAndSet操作，get到Tail后，对Tail的获取锁情况进行判断，如果该线程未持有锁，则此线程可以获取锁

CLH是一种隐式队列，其pre-next的实现通过getAndSet来完成

- 优点：
  - 公平锁，先来先获取
  - 性能优异，获取和释放锁开销小
- 缺点：
  - 有自旋操作，锁占有时间长时带来的cpu开销大
  - 功能单一

### AQS对CLH的改造

