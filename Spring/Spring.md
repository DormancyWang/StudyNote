## 导言
1. 框架
	高度抽取可重用代码的一种设计；高度的通用性
	多个可充用模块的集合，形成一个整体的解决方案

2. Spring框架
	容器框架    
	EJB(enterprise java bean)---->Spring  
	IOC(思想) 和 AOP <span style="color: red;font-size: 20px">DI(具体的实现方法) 就是控制反转IOC 的最经典的实现 </span>

	<br/>Spring的优良特性<br/>
	非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API<br/>
	依赖注入：DI——Dependency Injection，反转控制(IOC)最经典的实现。<br/>
	面向切面编程：Aspect Oriented Programming——AOP<br/>
	容器：Spring是一个容器，因为它包含并且管理应用对象的生命周期<br/>
	组件化：Spring实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。<br/>
	一站式：在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring 自身也提供了表述层的SpringMVC和持久层的Spring JDBC）。<br/>


## Spring

3. spring框架信息
	![](imgs/spring-overview.png)

4. IOC(inversion of Control)
	获取资源的方式：
		主动式、被动式  复杂对象的创建是比较庞大的功能

	容器：管理所有的组件（有特定功能的类）

![](imgs/ApplicationHer.jpg)
	ClassPathXmlApplicationContext ioc容器的配置问价在类的路径之下<br>
	FileSystem							在磁盘路径下面<br>
	bean在容器创建的时候就创建好了，单实例，property赋值是调用get，set方法。<br>

bean->    name value type ref index

名称空间：防止标签重复 p:...
a:name ---->  b:name
```xml
实验6：通过继承实现bean配置信息的重用 
parent
<bean id="person01" class="com.guo.learningspring.Person">
        <property name="name" value="张三"/>
        <property name="age" value="34"/>
        <property name="gender" value="男"/>
        <property name="email" value="asdfasd@emil.com"/>
    </bean>
    <bean class="com.guo.learningspring.Person" id="person06" parent="person01">
        
    </bean>
实验7：通过abstract属性创建一个模板bean   
abstract

实验8：bean之间的依赖 
bean的创建顺序是按照文件读的顺序
使用depends-on来改变创建顺序

实验9：测试bean的作用域，分别创建单实例和多实例的bean★
scope: prototype 多实例,request,session,singleton 单实例
多实例在容器启动的时候不会默认创建，单实例则相反

实验5：配置通过静态工厂方法创建的bean、实例工厂方法创建的bean、FactoryBean★
静态工厂：工厂本身不需要创建对象，通过静态方法调用
实例工厂：工厂本身需要创建对象
factory-bean factory-method
FactoryBean(是Spring规定的一个接口)只要实现了一个这个接口的类，Spring都认识



实验10：创建带有生命周期方法的bean
声明周期：	bean的创建到销毁；
			ioc容器中注册的bean：
				1. 单实例bean，容器启动的时候就创建好
				2. 多实例的bean，在获取的时候才创建
			可以为bean自定义一些生命周期的方法，Spring在创建或者销毁的时候会调用指定的方法
			bean-->destory-method,init-method

			scope="prototype" 容器关闭不会销毁，单实例会调用destory-method


实验11：测试bean的后置处理器
BeanPostProcessor
在bean的初始化前后调用方法；


实验12：引用外部属性文件★
数据库连接池作为单实例是最好的，可以让Spring来帮我们创建数据链接池对象
依赖context的名称空间


实验13：基于XML的自动装配（自定义类型自动赋值）
javaBean  bean --> autowire = default 不自动装配
							  byName  按照名字去寻找
							  byType  按照类型去寻找
							  constructor 按照构造器的类型进行赋值 不会报错
							  no   


实验14：[SpEL测试I]
    在SpEL中使用字面量、
    引用其他bean、
    引用其他bean的某个属性值、
    调用非静态方法
    调用静态方法、
    使用运算符



实验15：通过注解分别创建Dao、Service、Controller★
Spring有四个注解
	@Controller
	@Service
	@Repository (数据库层，持久化层，dao)
	@Component
	1) 给要添加的组件上标四个注解的任何一个
	2）告诉spring，自动扫描添加了注解的组件，依赖context名称空间
	3） id就是默认类名首字母小写，一定要导入aop包，其实现了支持注解
	@Scope可以调整作用域


实验16：使用context:include-filter指定扫描包时要包含的类
实验17：使用context:exclude-filter指定扫描包时不包含的类
type：	annotation
		assignable
		aspectj
		custom
		regex


实验18：使用@Autowired注解实现根据类型实现自动装配★




实验19：如果资源类型的bean不止一个，
        默认根据@Autowired注解标记的成员变量名作为id查找bean，进行装配★


实验20：如果根据成员变量名作为id还是找不到bean，
        可以使用@Qualifier注解明确指定目标bean的id★



实验21：在方法的形参位置使用@Qualifier注解


实验22：@Autowired注解的required属性指定某个属性允许不被设置
@Resource，@Inject 只有autowire最强大


实验23：测试泛型依赖注入★
	

```

### Spring测试
1. @ContextConfiguration(locations="")
	@RunWith(SpringJUnit4ClassRunner.class)//使用spring的单元测试方法


### AOP
Aspect Oriented Programming
新的编程思想，基于oop的
<span style="color: red">在程序运行期间，将某段代码动态的切入到指定方法的指定位置进行运行的这种编程方式</span>

例子： 日志记录  <br>
	1. 写在代码里
	2. 在运行期间动态加上
	代理的设计模式 <span style="color: red;font-size: 20px">Proxy</span>
	动态代理写起来太麻烦，jdk默认的动态代理，如果对象没有接口，则创建不出来

AOP底层就是动态代理
可以用spring一句也不写来创建动态代理，没有强制要求必须实现接口



1. AOP专业术语

2. 注解和切入点表达式
@Before：前置通知，在方法执行之前执行
@After：后置通知，在方法执行之后执行
@AfterRunning：返回通知，在方法返回结果之后执行
@AfterThrowing：异常通知，在方法抛出异常之后执行
@Around：环绕通知，围绕着方法执行
	最强大的通知方法，就是手写的动态代理
	就是因为有JoinPoint 可以获得参数，调用方法。

execution([权限修饰符] [返回值类型] [简单类名/全类名] [方法名] （参数列表))


1. ..,任意参数，任意层路径 
2. \*，权限位置不行，不写就是匹配所有权限

3. ioc里面存的是代理对象 java.sun.proxy,所以不能通过子类class获取

4. 没有接口就是自己的类型，cglib ，内部类

5. JoinPoint

6. 切入点表达式的重用 重新定义一个方法

7. 



### IOC 源码分析

