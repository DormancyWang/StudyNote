### 多线程  
#### 多线程的使用场景

多线程方法汇总  
java.lang.Thread    
java.lang.Runnable    
Thread.start();    
Thread.run();    
static Thread.currentThread();    
Thread.getName();  
Thread.setName();   
Thread.yeild(); : 释放cpu的执行权    
Thread.join(); :  抢占cpu的执行权   
static Thread.sleep(long milltime) 休眠   
Thread.isAlive()    
Thread.getPriority()    
Thread.setPriority()    


1. 需要后台执行任务  
2. 需要实现一些需要等待的任务  

#### java.lang.Thread & java.lang.Runnable
There are two ways to create a new thread of execution. One is to declare a class to be a subclass of Thread.  
One is to declare a class to be a subclass of Thread. This subclass should override the run method of class Thread. overwrite run() method;
The other way to create a thread is to declare a class that implements the Runnable interface. pass to a thread constructor as paramter.

1. 不可以直接调用run方法，这样不会启动一个新线程  
2. 实际开发中可以使用匿名对象来进行简化代码
```java
class MyThread1 extends Thread{
    @Override
    public void run() {
        for(int i=0;i<100;i++){
            if(i%2==1) System.out.println(Thread.currentThread().getName()+ ":\t\t" + i);
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
    	// use independent class
//        MyThread1 t1 = new MyThread1();
//        MyThread2 t2 = new MyThread2();
//        t1.start();
//        t2.start();
    	//use anonymous class
        Thread t1 = new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i % 2 == 1) System.out.println(Thread.currentThread().getName() + ":\t\t" + i);
                }
            }
        };

        t1.start();
    }
}
```

3. java线程调度的方法是，同优先级使用fifo，时间片轮转，高优先级，使用抢占式  
MAX_PRIORITY 10  
MIN_PRIORITY 1  
NORM_PRIROITY 5  
getPriority()  
setPriority()  

4. java中的线程状态   
NEW   新建   
RUNNABLE 可执行   
BLOCKED   阻塞  
WAITING   等待   
TIMED_WAITING    暂停     
TERMINATED     结束   

#### 线程同步
1. 同步代码块 synchronized(同步监视器){}  
同步监视器,俗称锁：任何一个类的对象都可以充当锁。注意锁要使用一样的。  
类也是一个对象   类名.class    
3. 同步方法，如果操作共享数据的代码全部在一个方法中，可以将此方法声明为同步的   
同步方法用的同步监视器是默认的： 就是this 

懒汉式的线程安全问题  

4. lock锁
java.util.concurrent.locks.ReentrantLock

1. 与synchronize的区别，Lock是显式锁（手动开启和关闭锁，别忘记关闭锁），synchronized是隐式锁，出了作用域自动释放  
2. Lock只有代码块锁，synchronized有代码块锁和方法锁  
3. 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多的子类）  



#### wait() 与notify() 和notifyAll()

wait()：令当前线程挂起并放弃CPU、同步资源并等待，使别的线程可访问并修改共享资源，而当前线程排队等候其他线程调用notify()或notifyAll()方法唤醒，唤醒后等待重新获得对监视器的所有
权后才能继续执行。

方法与锁要相互对应,notify。。。 都定义在object当中  

notify()：唤醒正在排队等待同步资源的线程中优先级最高者结束等待
notifyAll()：唤醒正在排队等待资源的所有线程结束等待.

这三个方法只有在synchronized方法或synchronized代码块中才能使用，否则会报
java.lang.IllegalMonitorStateException异常。

因为这三个方法必须有锁对象调用，而任意对象都可以作为synchronized的同步锁，
因此这三个方法只能在Object类中声明。

sleep() 和 wait()  
相同： 都可以使当期线程进入阻塞状态  
不同点：定义位置不同，不同类 Thread 和 Object  
调用要求不同  
是否释放锁  

#### JKD5.0新增创建方式
Callable接口      重写call    
使用线程池    

与使用Runnable相比，Callable功能更强大些
相比run()方法，可以有返回值
方法可以抛出异常
支持泛型的返回值
需要借助FutureTask类，比如获取返回结果



volatile变量修饰符


#### java常用类
String  字面量全在常量池中，new出来的有指向常量池的指针    
intern() 方法   
常用方法  

StringBuffer  
扩容的原理  
StringBuilder  


#### 日期时间api
1. java.lang.System  
currentTimeMillis() nano(); 一般用于计算程序运行时间  

2. java.util.Date  
java.sql.Date util.Date的子类  

3. SimpleDateFormate
格式化  将日期转换为字符串   
解析  将字符串转化为日期    

4. java.util.Calendar(日历)类
Calendar是一个抽象基类，主用用于完成日期字段之间相互操作的功能。
获取Calendar实例的方法
使用Calendar.getInstance()方法
调用它的子类GregorianCalendar的构造器


一个Calendar的实例是系统时间的抽象表示，通过get(int field)方法来取得想
要的时间信息。比如YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY 、
MINUTE、SECOND
public void set(int field,int value)
public void add(int field,int amount)
public final Date getTime()
public final void setTime(Date date)
注意:
获取月份时：一月是0，二月是1，以此类推，12月是11
获取星期时：周日是1，周二是2，。。。。周六是7


-------------------------------------------------
以上是java8之前的api 有很多不足，偏移量，可变性，格式化，线程不安全  

java8 之后引入了Joda-Time
LocalDate   
LocalTime  
LocalDateTime  
ZonedDateTime  
Duration  

java.time – 包含值对象的基础包
java.time.chrono – 提供对不同的日历系统的访问
java.time.format – 格式化和解析时间和日期
java.time.temporal – 包括底层框架和扩展特性
java.time.zone – 包含时区支持的类


java.time包通过值类型Instant提供机器视图，不提供处理人类意义上的时间   
单位。Instant表示时间线上的一点，而不需要任何上下文信息，例如，时区。   
概念上讲，它只是简单的表示自1970年1月1日0时0分0秒（UTC）开始的秒数。因为java.time包是基于纳秒计算的，所以Instant的精度可以达到纳秒级。   
(1 ns = 10^-9s)   1秒= 1000毫秒=10^6微秒=10^9纳秒   


#### System类
一些基本的参数信息  
一些基本的方法  

#### BigInteger和BigDecimal

#### 枚举类
自定义枚举类  
enum  其定义的父类是java.lang.Enum
和普通Java 类一样，枚举类可以实现一个或多个接口
若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只
要统一实现该方法即可。
若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式, 
则可以让每个枚举值分别来实现该方法


#### 注解Annotation
注解就是接口，可以带参数  https://docs.oracle.com/javase/tutorial/java/annotations/repeating.html  

四种原注解  
jdk8 添加了 可重复注解  和类型注解  


#### java 集合
	Collection->List->ArrayList,LinkedList,Vector
			  ->Set->HashSet,LinkedHashSet,TreeSet
	Map->HashMap,LinkedHashMap,TreeMap,HashTable,Properties
这里需要注意 equals() 和 HashCode() 这两个方法  


1. Collection 里的方法   
2. List里的方法  

ArrayList的源码分析： 
	1. 初始化大小为10
	2. 扩容倍数为1.5
	3. 结论，开发中使用带参的构造器  


3. Set接口
HashSet: 主要实现类，线程不安全的，可以存储null值  
LinkedHashSet: 作为HashSet的子类出现，遍历其内部数据的时候可以按照添加的顺序遍历
TreeSet：使用红黑树存储，可排序的。

添加元素的过程，以HashSet为例
首先调用HashCode()方法，得到哈希值。  
用此hash值来算出在数组的存放位置  
如果没有元素就直接添加成功  
如果已经存在了其他元素b，首先比较元素a与元素b的hash值：不相同添加成功，如果hash值相同，再调用equals方法。来确定究竟是否是一样的元素  

向Set中添加数据，一定要重写HashCode和equals方法，且尽可能的保持一致性  

用Set 来过滤重复的数据  

4. Map接口 
Map->HashMap,LinkedHashMap
	->TreeMap,
	->HashTable,Properties

1. HashMap 主要实现类，线程不安全，效率高，可存储null的key和value
2. LinkedHashMap： 添加了前后指针，可以按插入顺序遍历，对于频繁的遍历操作，效率比HashMap高
3. TreeMap ： 底层红黑树，可以排序
4. Hashtable： 老旧实现类，线程安全，基本不用
5. Properties： key 和 value都是String，处理配置文件的类  

比较顺序 第一 取hash加映射算法，第二取hash比较，第三用equals

HashMap.Entry/Node 看源码就好  

5. Properties
是Hashtable 的子类   
Key和Value都是字符串类型  


6. Collections工具类  
一个操作Set，List，Map的工具类。都是静态方法  
排序操作：（均为static方法）  
reverse(List)：反转List 中元素的顺序  
shuffle(List)：对List集合元素进行随机排序  
sort(List)：根据元素的自然顺序对指定List 集合元素按升序排序  
sort(List，Comparator)：根据指定的Comparator 产生的顺序对List 集合元素进行排序  
swap(List，int，int)：将指定list 集合中的i 处元素和j 处元素进行交换查找、替换  
Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素  
Object max(Collection，Comparator)：根据Comparator 指定的顺序，返回给定集合中的最大元素  
Object min(Collection)  
Object min(Collection，Comparator)  
int frequency(Collection，Object)：返回指定集合中指定元素的出现次数  
void copy(List dest,Listsrc)：将src中的内容复制到dest中  
booleanreplaceAll(List list，Object oldVal，Object newVal)：使用新值替换List 对象的所有旧值  

#### 泛型
1. 为什么有泛型
jdk1.5之前是设计为object的，之后使用泛型来解决  
	1. 解决元素存储的安全性问题   
	
	2. 解决需要强制转换的问题  
	
	3. Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生ClassCastException异常。同时，代码更加简洁、健壮。  

2. 在集合中的泛型使用

3. 自定义的泛型结构

	自定义泛型类: class GenTest<K,V> interface List<T>    

	使用泛型的主要优点是能够在编译时而不是在运行时检测错误  
	
	泛型不同的引用不能相互赋值。尽管在编译时ArrayList<String>和ArrayList<Integer>是两种类型，但是，在运行时只有一个ArrayList被加载到JVM中。  
	
	泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。经验：泛型要使用一路都用。要不用，一路都不要用  
	class Son1 extends Father {// 等价于class Son extends Father<Object,Object>{}

	不能使用new E[]。但是可以：E[] elements = (E[])new Object[capacity];参考：ArrayList源码中声明：Object[] elementData，而非泛型参数类型数组

	在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。但在静态方法中不能使用类的泛型。

4. 泛型在继承上的实现
5. 通配符的使用

	<?>
允许所有泛型的引用调用
通配符指定上限
上限extends：使用时指定的类型必须是继承某个类，或者实现某个接口，即<= 
通配符指定下限
下限super：使用时指定的类型不能小于操作的类，即>=
举例：
<?extends Number>     (无穷小, Number]
只允许泛型为Number及Number子类的引用调用
<? super Number>      [Number , 无穷大)
只允许泛型为Number及Number父类的引用调用
<? extends Comparable>
只允许泛型为实现Comparable接口的实现类的引用调用

6. 泛型应用的举例



#### IO 流
##### File类

final static String seperator 通用分隔符  


三种构造器
public File(String pathname)  

public File(String parent,String child)  

public File(File parent,String child)  


File类的获取功能
public String getAbsolutePath()：获取绝对路径
public String getPath()：获取路径
public String getName()：获取名称
public String getParent()：获取上层文件目录路径。若无，返回null
public long length() ：获取文件长度（即：字节数）。不能获取目录的长度。
public long lastModified() ：获取最后一次的修改时间，毫秒值
public String[] list()：获取指定目录下的所有文件或者文件目录的名称数组
public File[] listFiles()：获取指定目录下的所有文件或者文件目录的File数组  

File类的重命名功能  
publicbooleanrenameTo(Filedest):把文件重命名为指定的文件路径  


File类的判断功能
public boolean isDirectory()：判断是否是文件目录
public boolean isFile()：判断是否是文件
public boolean exists()：判断是否存在
public boolean canRead()：判断是否可读
public boolean canWrite()：判断是否可写
public boolean isHidden() ：判断是否隐藏

File类的创建功能
public boolean createNewFile()：创建文件。若文件存在，则不创建，返回false  
public boolean mkdir() ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。  
public boolean mkdirs()：创建文件目录。如果上层文件目录不存在，一并创建注意事项：如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目路径下。  

##### IO流

按操作数据单位不同分为：字节流(8 bit)，字符流(16 bit)  
按数据流的流向不同分为：输入流，输出流  
按流的角色的不同分为：节点流，处理流  


(抽象基类) 字节流 字符流
输入流 InputStream Reader
输出流 OutputStream Writer

![IO 流体系](image/IOStream.png)

不要用throws 因为不能关闭  


缓冲流

外层关闭的时候，自动关闭了内层流  


转换流提供了在字节流和字符流之间的转换
Java API提供了两个转换流：
InputStreamReader：将InputStream转换为Reader
OutputStreamWriter：将Writer转换为OutputStream
字节流中的数据都是字符时，转成字符流操作更高效。
很多时候我们使用转换流来处理文件乱码问题。实现编码和
解码的功能


#### 网络编程



### Java 反射机制
api：  
```java
java.lang.Class
java.lang.reflect.Method
java.lang.reflect.Field
java.lang.reflect.Constructor
```
Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接**操作**任意对象的内部属性及方法。  


1、动态语言
是一类在运行时可以改变其结构的语言：例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。主要动态语言：Object-C、C#、JavaScript、PHP、Python、Erlang。

2、静态语言
与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C、C++。

补充：动态语言vs 静态语言
Java不是动态语言，但Java可以称之为“准动态语言”。即Java有一定的动态性，我们可以利用反射机制、字节码操作获得类似动态语言的特性。Java的动态性让编程的时候更加灵活！

可以用反射做什么？  
在运行时判断任意一个对象所属的类
在运行时构造任意一个类的对象
在运行时判断任意一个类所具有的成员变量和方法
在运行时获取泛型信息
在运行时调用任意一个对象的成员变量和方法
在运行时处理注解
生成动态代理

1. 理解Class类并获取Class实例

Object类中定义了getClass()方法，返回Class类  这就是反射的源头  

Class本身也是一个类
Class 对象只能由系统建立对象
一个加载的类在JVM 中只会有一个Class实例
一个Class对象对应的是一个加载到JVM中的一个.class文件
每个类的实例都会记得自己是由哪个Class 实例所生成
通过Class可以完整地得到一个类中的所有被加载的结构
Class类是Reflection的根源，针对任何你想动态加载、运行的类，唯有先获得相应的
Class对象

1）前提：若已知具体的类，通过类的class属性获取，该方法最为安全可靠，程序性能最高  
实例：Class clazz= String.class;  
2）前提：已知某个类的实例，调用该实例的getClass()方法获取Class对象  
实例：Class clazz= “www.atguigu.com”.getClass();  
3）前提：已知一个类的全类名，且该类在类路径下，可通过Class类的静态方法forName()获取，可能抛出ClassNotFoundException  
实例：Class clazz= Class.forName(“java.lang.String”);  
4）其他方式(不做要求)  
ClassLoadercl = this.getClass().getClassLoader();  
Class clazz4 = cl.loadClass(“类的全类名”);  
获取Class类的实例(四种方法)  

可以有class对象的类型 class，interface,[],enumtation,primitive type,  void  




通过反射可以调用类的私有结构




2. 类的加载与ClassLoader的理解
3. 创建运行时类的对象
4. Java反射机制概述
5. 获取运行时类的完整结构
6. 调用运行时类的指定结构
7. 反射的应用：动态代理 7

##### 动态代理


##### java8的新特性
1. 速度快  

2. 代码少 

3. Stream API  

4. 便于并行

5. 最大化减少空指针异常 Optional

6. Nashorn 引擎，允许jvm上运行js应用


1. Lambda表达式
Lambda表达式的本质是对象，就是一个函数式接口的实例  
无参，无返回值  



2. 函数式接口： 接口里面只有一个抽象方法   @FunctionalInterface()  
 

#### Stream api
Stream API ( java.util.stream)把真正的函数式编程风格引入到Java中。这
是目前为止对Java类库最好的补充，因为Stream API可以极大提供Java程
序员的生产力，让程序员写出高效率、干净、简洁的代码



Stream到底是什么呢？
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
“集合讲的是数据，Stream讲的是计算！”
注意：
①Stream 自己不会存储元素。
②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行

Stream 的操作三个步骤
 1-创建Stream
一个数据源（如：集合、数组），获取一个流
 2-中间操作
一个中间操作链，对数据源的数据进行处理
 3-终止操作(终端操作)
一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用



#### jvm
java虚拟级规范  
sun->HotSpot  
BEA->JRockit  
IBM-> J9 VM  


tips：父类实现一个接口，子类又重复实现同一个接口的目地
存粹是为了提高代码的可读性
如果子类不再单独实现接口，java.lang.Class直接获取子类的接口为空数组，这样要做一些动态代理操作的时候无法操作，所以也有可能是这样的原因而重新继承了接口。