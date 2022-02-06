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

