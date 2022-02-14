## java language
### java 语法
1. final 关键字定义常量。constant 为保留字 不建议使用  
2. 枚举类型  enum NAME{}  
3. String类，以及相关函数。  
String 不可修改，只能新建  
String 判断相等用equals（） 不可用 ==，因为只有字符串常量是共享的。  
代码点和代码单元： 常用的Unicode字符可用一个代码单元表示，辅助字符需要一对代码单元表示。length() 方法是获取代码单元的方法。codePointCount()获取代码单元数量。Character.isSupplementaryCodePoin()  
4. 文件的输入输出   
5. java 数组允许为0，与null不同 
6. 数组就是对象 [java 数组详解][https://dunwu.github.io/javacore/basics/java-array.html] 
### 对象和类
1. 面向对象程序设计  
2. 类，封装，状态(state，实例域的集合)  
3. 隐式参数，显式参数。this  
4. 基于类的访问权限  同一个类的不同对象可以引用对方的实例域  
5. final：初始化后不可修改
6. Factory* 构造方法，由方法确定构造什么样的对象并返回给你。  
7. java的方法总是采用值调用
8. **重载 overloading**
9. 方法签名 signature = 参数类型 + 方法名  
10. 调用另一构造器 this()  
11. 初始化块(会在构造器之前运行)，静态域的初始化块  
12. finalize方法，释放对象占用的资源。会在垃圾回收器清除对象之前使用，但是不知道确切的时候，所以不要用它来处理任何短缺资源。  
对此可以人工管理，设置dispose(),close() 并且人工调用  
#### 包：
13. 包名是唯一的。从编译器的角度来看，嵌套的包之间没有任何关系。例如：java.util 和 java.util.jar包之间无任何关系，他们是独立的。  
14. 一个类可以使用本包内的类，和其他包内的public 类  
15. 类名冲突是为了编译器识别，在字节码中是使用 全名 的  
16. import 不仅可以导入类，还添加了导入**静态方法** 和 **静态域** 的功能  
17. 将类放入包中: 需要将 包名 声明在源文件开头 package com.exmple.exmple...   
18. 没有声明的放在 默认包(default package) 中。默认包没有名字
19. 编译的时候需要写全包名，编译器在编译的时候不检查目录结构。所以你写错了也能编译，但是运行的时候会出错，因为 虚拟机 会找不到类。  
20. 编译对文件进行操作，所以加路径。虚拟机对类 所以加包名  
```
javac com\exmple\
java com.exmple.
```  
21. default 和 什么都不写 对类，对 域 的访问控制  
22. jar (java archive)文件 可以存放多个压缩形式的类和子目录文件，第三方库文件通常以此种形式提供。使用的是zip的压缩格式。  
23. java注释简略 @param @return @throws @author @version @deprecated @see reference
### 继承 is-a
1. 覆盖(override)  
2. 子类不能访问父类的私有对象(private)，只有父类的相同的类可以
3. super关键字，特指父类的方法。与this不一样。只是一种标记，而this是一个对象引用  
4. 继承层次，继承链  
### 多态  一个父类对象可以是一个父类也可以是子类对象
1. 但是不能用子类方法。。。因为他被看为一个父类对象  
2. 对方法的调用来说，有动态绑定和静态绑定（构造器，static,final方法-不能被修改的方法）两种。使用 方法表 来解决动态绑定中搜索方法造成的时间消耗问题。  
3. final方法-不能被修改的方法，final类-不允许扩展继承的类
4. 对象的强制类型转换。只能子类转父类（默认的，也不需要标出），子类还原成子类。错误信息ClassCastException异常  
5. instanceof() 运算符  
>instanceof 严格来说是Java中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法为：
>boolean result = obj instanceof Class
>　　其中 obj 为一个对象，Class 表示一个类或者一个接口，当 obj 为 Class 的对象，或者是其直接或间接子类，或者是其接口的实现类，结果result 都返回 true，否则返回false。
6. 抽象类,抽象方法 abstract - 只将他作为派生其他类的模板，而不产生实例  
7. >private  
>default(本包)  
>protected(本包加子类)  
>public  
>(访问控制逐渐开放)  
8. *Object*-所有类的超类
9. object的方法：  
>equals()  
>HashCode() *(重写完equals方法必须重写hashcode，Case Override only equals: two same object will have different hashcode = same objects go in different bucket(duplication). ），so equals must act the same as HashCode---when equals show two object are same,their HashCode must be the same* 
toString()  
Class getClass()  
Object clone()  
java.lang.Class  
	String getName()  
	Class getSuperclass()  
10. ArrayList<>()，泛型，ensureCapacity(),trimToSize()  
11. 对象包装器和自动打包autoboxing,自动拆包。比如在自增的时候是先拆包再自增再打包的。这样容易造成包装器和值相同的假象。比如 判断相等，包装器在一定数值内(-128-127)是使用对象池的 所以可以用 == 判断是否相等，其实是判断是不是一个对象，但是数值太大的时候一定要用.equals()    Void,Boolean。基本数据类型的包装类都是继承自公共超类Number. 1.他们是不可改变的 2.不能继承或更改  
12. //todo org.omg.CORBA holder,intHolder  
13. 包装器内有一些常见的处理对应数值的方法  
14. 形参数量可变的方法 eg: public PrintStream printf(Sring fmt,Object... args){return format(fmt,args)}  
15. 枚举类 enum 
#### 反射
1. 反射库(reflection library),动态操纵java程序，应用于JavaBeans中。  
2. 能够分析类能力的程序被称为反射  
3.  反射要用到的基本类 java.lang.reflect: Method,Field,Constractor  
					 java.lang.Object: Class  
获取Class: 1. Object.getClass() 
           2. Class.forName("className");
           3. T.class  
jvm为每个类型管理一个Class类，所以可以用== 判断是否相等  
Class.newInstance(),Constructor.newInstance()  
类AccessibleObject   
Method.invoke()  
#### 接口
1. 接口中的所有方法都自动的属于public，不必声明public，在实现的时候要声明  
2. 接口之间可以继承  
3. 接口中可以声明常量，与方法一样自动为public static final  
4. 一个类可以实现多个接口   
5. 由于只能继承一个抽象类，所以产生了接口来实现多继承  
6. Object.clone() 只是浅拷贝，原始对象和克隆对象会共享子对象引用信息  
7. Cloneable接口在这里是标记接口(tagging interfacce)，只是告诉编译器，该类需要实现clone()方法。异常(checked exception)  
#### 接口与回调
1. 例子：定时器，回调函数。 传入的对象需要实现java.awt.event ActionListener接口  
#### 内部类
1. 可以调用所在作用域的数据 2. 可以对包中其他类隐藏起来 3. 可以简化代码  
2. java中的内部类有一个指向外围类的引用，static内部类没有  
3. 内部类是类与类的关系，不是对象与对象的关系。外部类的对象并不包含内部类的对象。  
**参数内部类**
4. 内部类是编译器现象，与虚拟机无关。 编译器会将内部类翻译成  外部类$内部类 的一个普通类文件，虚拟机对此一无所知  
5. 内部类内会有一个指向外部类的引用，外部类会有一个提供访问的静态方法。全部为编译器提供。
6. 局部内部类，匿名内部类，静态内部类  
7. 代理
#### 异常
1. Throwable:  
             error
             Exception
                      IOException
                      Runtime Exception
2. Error 和　Runtime Exception  叫未检查异常(unchecked)，其他的叫检查异常(checked)  
#### 断言
assert  

#### 泛型
1. 泛型程序设计  
2. 泛型类，泛型方法：public static <T> T getMiddle(T[] a)  , 调用 middle = ArrayAlg.<String>getMiddle(names);<String> 可以省略  
3. 类型变量的限定<T extends ... & ...> extends只是一个符号，后面的... 可以是类或者接口 &用来连接多个emmm.Bounding Types
4. 不能用基本类型初始化  

#### 集合
1. 接口和实现分离  
2. Abstract开头的类，为类库实现者而设计  
3. interface Collection 和 Iterator  
4. 一些具体的集合  
有序表：
ArrayList : 底层维护了一个可扩容的数组
LinkedList : 链表是双向链接的

ArrayDeque ： 数组双向队列
*LinkedList* ： 链表双向队列

HashSet : 链表数组实现，每个列表被称为桶bucket，
TreeSet : 有序集合，过程为 插入排序输出，红黑树完成，比数组，链表快。
EnumSet
LinkedHashSet ： 可以记住插入元素项的顺序
PriorityQueue : 最小堆实现 

HashMap : 无排序
TreeMap : 有排序
EnumMap
LinkedHashMap : 可以记住插入元素项的顺序
WeakHashMap : 弱引用，不用的对象会被gc回收
IdentityHashMap : hashcode 是使用 System.identityHashCode 方法计算，两个对象比较的时候使用 ==  

Hashtable : 和HashMap作用一样 ，线程安全的
Vecotr : 和 ArrayList 作用一样 ， 线程安全的

#### 具体实现
1. ArrayList : 底层数组，允许放入null，没有实现同步，会自动扩容，数组扩容通过一个公开的方法ensureCapacity(int minCapacity)来实现。在实际添加大量元素前，我也可以使用ensureCapacity来手动增加ArrayList实例的容量，以减少递增式再分配的数量。 数组进行扩容时，会将老数组中的元素重新拷贝一份到新的数组中，每次数组容量的增长大约是其原容量的1.5倍。这种操作的代价是很高的，因此在实际使用时，我们应该尽量避免数组容量的扩张。当我们可预知要保存的元素的多少时，要在构造ArrayList实例时，就指定其容量，以避免数组扩容的发生。或者根据实际需求，通过调用ensureCapacity方法来手动增加ArrayList实例的容量。  
ArrayList还给我们提供了将底层数组的容量调整为当前列表保存的实际元素的大小的功能。它可以通过trimToSize方法来实现。
Fail-Fast机制  

2. LinkedList : 使用栈或者队列时最先考虑ArrayDeque，性能更好。没有实现同步  
底层实现，双向链表  

3. stack queue   

4. PriorityQueue ： 优先队列的作用是能保证每次取出的元素都是队列中权值最小的  
其通过堆实现，具体说是通过完全二叉树(complete binary tree)实现的小顶堆(任意一个非叶子节点的权值，都不大于其左右子节点的权值)
也就意味着可以通过数组来作为PriorityQueue的底层实现。

5. HashMap 和 HashSet ： HashSet 里有一个HashMap的成员变量，所以使用HashMap实现的。下面是hashMap 的分析  
1. HashMap实现了Map接口，即允许放入key为null的元素，也允许插入value为null的元素；除该类未实现同步外，其余跟Hashtable大致相同  
2. 关于扩容
3. 冲突处理方式为冲突链模式
4. hashCode()和equals()。hashCode()方法决定了对象会被放到哪个bucket里，当多个对象的哈希值冲突时，equals()方法决定了这些对象是否是“同一个对象”。
5. Java8 对 HashMap 进行了一些修改，最大的不同就是利用了红黑树，所以其由 数组+链表+红黑树 组成。超过8会变成红黑树  


6. TreeMap底层通过红黑树(Red-Black tree)实现，也就意味着containsKey(), get(), put(), remove()都有着log(n)的时间复杂度


7. 它的特殊之处在于 WeakHashMap 里的entry可能会被GC自动删除，即使程序员没有调用remove()或者clear()方法
*java.util.Property:Properties is a subclass of Hashtable. It is used to maintain lists of values in which the key is a String and the value is also a String.
The Properties class is used by many other Java classes. For example, it is the type of object returned by System.getProperties( ) when obtaining environmental values.Properties define the following instance variable. This variable holds a default property list associated with a Properties object.*

5. 框架(framework) 是一个类的集

### 多线程
Concurrency 并发
1. 可见性(cpu缓存)，原子性(时分复用)，有序性(重拍序)  
2. Synchronized :

1. 例子：
class MyRunnable implements Runnable{
	pubic void run(){
		task code
	}
}
Runnable r = new MyRunnable();
Thread t = new Thread(r);
t.start();nm  
2. native : allows you to  call a compiled dynamically loaded library with arbitrary assembly code from java.  

#### 注解
1. 作用： 生成文档，编译检查，编译动态处理，运行动态处理
2. 注解的分类 ： java自带注解，元注解（用于定义注解的注解），自定义注解（可用元注解解释）  
>1. java自带注解 ：@Override,@Deprecated,@SuppressWarnings(关闭编译器警告信息)  
>2. 元注解
>？上述内置注解的定义中使用了一些元注解（注解类型进行注解的注解类），在JDK 
>1.5中提供了4个标准的元注解：@Target，@Retention，@Documented，@Inherited, 
>在JDK 1.8中提供了两个元注解 @Repeatable和@Native。
>
>
>Target注解的作用是：描述注解的使用范围（即：被修饰的注解可以用在什么地方）,它的取值范围定义在ElementType 枚举中  
>
>Reteniton注解的作用是：描述注解保留的时间范围（即：被描述的注解在它所修饰的类中可以被保留到何时）   Reteniton注解用来限定那些被它所注解的注解类在注解到其他类上以后，可被保留到何时，一共有三种策略，定义在RetentionPolicy枚举中。  
>
>Documented注解的作用是：描述在使用 javadoc工具为类生成帮助文档时是否要保留其注解信息。  
>
>Inherited注解的作用：被它修饰的Annotation将具有继承性。如果某个类使用了被@Inherited修饰的Annotation，则其子类将自动具有该注解。  
>
>Repetable 允许在同一申明类型(类，属性，或方法)的多次使用同一个注解  
>
>使用 @Native 注解修饰成员变量，则表示这个变量可以被本地代码引用，常常被代码生成工具使用。对于 @Native 注解不常使用，了解即可  

3. 定义注解后，如何获取注解中的内容呢？反射包java.lang.reflect下的AnnotatedElement接口提供这些方法  
