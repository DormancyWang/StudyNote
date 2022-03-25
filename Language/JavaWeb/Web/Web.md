### servlet 用来处理访问请求的接口
1. servlet api 是由tomcat来实现的扩展类 需要找apache的文档查看 

2. xml配置 url和类名映射
	1. servlet 标签
	2. servlet Mapping 标签
	
3. servlet 的生命周期
	1. 构造器
	2. init
	//程序加载的时候调用
	3. service
	//所有后来的访问，都是调用这个方法
	4. destroy
	//最后服务器销毁所调用的方法

4. serverlet是最基本的类，一般不实现 而是用HttpServlet类来实现

5. servlet类的继承体系
	|servlet   | GenericServlet   |    HttpServlet|
	|----------|------------------|---------------|
	只是定义了servlet规范
		
	实现了servlet接口，做了很多空实现。持有一个servletconfig的类的引用，
	并对servletconfig做了一些方法
	重写了service方法，调用了dopost 和doget方法 跑出错误
	实现了请求的分发处理
	
6. ServletConfig类
	
	1. 配置信息类
	
	2. 可以获取Servlet程序的别名
	
	3. 获取初始化参数init-param
	
	4. 获取ServletContext对象


7. 重写了init方法需要调用父类的init方法 ，给类中hold的conifg对象赋值

8. ServletContext
	
	1. ServletContext是一个接口，它表示一个Servlet上下文对象
	
	2. 一个web工程只有一个ServletContext对象实例

	3.  ServletContext对象是一个域对象
		域对象是可以向map一样存取数据的对象
		域值得是可以存储数据的操作范围
	
	4. 一些方法
		setAttribute(),getAttribute(),removeAttribute;
		
	5. 作用
		
		1. 获取web.xml中配置的上下文参数context-param
		
		2. 获取当前的工程路径，格式 :/工程路径
		
		3. 获取工程部署后在服务器硬盘上的绝对路径
		
		4. 存储环境变量的参数
	
	6. ServletContext 里的参数和 initparam 完全不一样,没有交叉
	



### HTTP 协议
1. GET请求头
	请求行，请求头
	重要报文字段
	1. Connection:  告诉服务器请求应该如何处理
		keep-alive : 不哟啊马上关闭，保持一小段链接时间
		closed	： 马上关闭


	请求行，请求头
2. POST请求头
	1. Content-Type: application/x-www-form-urlencoded
					
					 multipart/form-data
	2. Cache-Control: max-age=0

3. Get请求有哪些
	1. form标签method=get
	2. a标签
	3. link标签引入css
	4. script标签引入js文件
	5. img标签引入图片
	6. iframe引入html页面
	7. 地址栏输入地址回车
	
4. post请求
	form标签method=post
	
5. 响应http协议格式
	1. 响应行
		1. 响应协议，和版本号
		2. 响应状态码 : 
			200 ：请求成功
			302 ：请求重定向 
			404 ：请求服务器收到了，但是数据不存在
			500 ：表示服务器已经收到了请求但是服务器内部错误
		3. 响应状态描述符 : 对状态的解释
	
	2. 响应头
		1. key：vlaue 不同的响应头有不同的含义
		空行
	
	3. 响应体
		回传给客户端的内容

6. MIME类型说明
HTTP协议中的数据类型multipurpose internet Mail extensions
格式 大类型/小类型 并与某种文件的扩展名相对应
常见的MIME类型
	.html	text/html
	.txt	txt/plain
	.rtf	application/rtf
	.gif	image/gif
	.jpg	image/jpeg
	.au 	audio/basic
	.mid	audio/midi
	.ra.ram	audio/x-pn-realaudio
	.mpg	video/mpeg
	.avi	video/x-msvideo
	.gz		application/x-gzip
	.tar	application/x-tar
	
	
7. HTTPServletRequest类
	只要有请求进入tomcat服务器，tomcat服务器就会吧请求来的http协议信息解析好封装到Request对象中
	然后就传递给service方法，我们可以通过这个对象来获得请求的所有信息
	
	1. 基本方法
		getRequestURL()
		getRequestURI()
		getRemoteHost()
		getHeader()
		getParameter()
		getParameterValues()
		getMethod()
		setAttribute(key,value)
		getAttribute(key)
		getRequestDispather()
	
8. 请求的转发
	服务器收到请求后从一个服务器资源跳到另一个资源的操作叫做请求转发
	1. 转发可以转发到WEB-INF 目录中
	2. 不可以转发到工程以外的资源
	
9. base标签
	1. 所有的相对路径工作的时候都参照浏览器地址栏的地址
	2. 设置当前页面中的相对路径工作是，参照哪个路径进行跳转
	base标签最后的斜杠不能省略
	
10. / 的不同含义
	1. 被浏览器解析，得到的是http://ip:port/
	2. 被服务器解析，得到的是http://ip:port/工程路径
		特殊情况，response.sendRediect("/"),把斜杠发送给浏览器解析，得到http://ip:port/

### HttpServletResponse 类
和Request类一样，每一次请求进来，tomcat服务器都会创建一个Response对象，传递给Servlet程序使用 

	1. 两个输出流的说明
		
		1. 字节流 getOutputStream() 常常用于下载（传递二进制数据）
		
		2. 字符流 getWrite() 		常常用于回传字符串		
		
		3. 两个流只能同时使用一个,否则会报错
		
		
		
### 请求重定向
特点：
	1. 浏览器地址会改
	2. 两次请求
	3. 不会共享域
	4. 可以重定向到站外地址
	5. 使用sendRedirect方法可以实现直接跳转
	
	


### JAVAEE 服务器三层架构
1. Web层： 		视图展现层
2. service层： 	业务层
3. Dao层： 		持久层	

![](image/1.jpg)
	
![](image/2.jpg)
	
开发流程
	1. 创建数据库模块和表
	2. 编写与数据库表对应的javabean对象
	3. 编写DAO 持久层 
	4. BaseDao,dao接口，dao实现
	5. 编写userService和测试
	6. 编写servlet层
	

### JSP
java server page java的服务器页面
jsp主要作用是代替servlet程序回传html页面的数据，因为用servlet回传机器的麻烦

1. jsp页面本质上是一个servlet程序
	本质就是获取输出流，然后写入html的代码

2. jsp头部的page指令
	用来修改jsp页面的一些重要的属性，或者行为
	1. 	language		支持语言
	2. 	contentType		返回数据类型
	3. 	pageEncoding	jsp页面本身的字符集
	4. 	import			导入包，或者类
	5.	autoFlush		out输出流使用，设置缓冲区是否自动刷新
	6. 	buffer			设置缓冲区大小默认8kb
	7. 	errorPage		出错的时候跳转的页面
	8. 	isErrorPage		设置当前的jsp页面是否是jsp页面
	9. 	session			访问当前的jsp页面是否会创建session对象，默认为true
	10.	extends			设置jsp翻译出来的java类默认继承谁

3. 常用的jsp脚本
	
	1. 声明脚本 <%! 声明java代码 %> 可以给jsp翻译出来的java类定义属性和方法，甚至是静态代码块，内部类等
		1. 声明属性
		2. 声明静态代码块
		3. 声明方法
		4. 声明内部类
		
	2. 表达式脚本 <%= 在jsp页面上输出数据%>
		会被放在out.print() 方法里面
		
	3. 代码脚本<%java语句%>
		可以编写自己需要的功能
		
	4. 脚本可以组合使用，随便用，非常灵活
	
	5. jsp的九大内置对象
		1. request 		请求对象
		2. response		响应对象
		3. pageContext	jsp的上下文对象
		4. session		会话对象
		5. application	servletContext对象
		6. config		servletConfig对象
		7. out			jsp输出流对象
		8. page 		指向当前jsp的对象
		9. exception	异常对象
		
	6. 四大域对象
		1. PageContext,Request,Session,Application
		2. 分别是，当前jsp范围内有效，一次请求有效，一次会话有效（打开浏览器访问服务器，知道关闭浏览器），整个web工程范围内都有效（只要web工程不销毁，都在）
	
	7. out和response的区别
		1. out缓冲区不是直接输出到页面而是放在response缓冲区的末尾 out.flush()
		
	8. jsp的常用标签
		1. 静态包含 <%@include file="" %> : 直接把被包含页面的内容拷贝过来
		2. 动态包含
		3. jsp转发 <jsp:forward page=""></>
		
### Listener监听器
监听某种事务的变化，然后通过回调函数，反馈给客户程序去做一些响应的处理	
1. ServletContextListener可以监听servletContext对象的创建和销毁
	会调用响应的方法
	
	
### EL 表达式 与 JSTL 标签库
1. EL expression Language  表达式语言
	主要用来代替jsp页面张的表达式脚本
	
2. EL  表达式的格式为${表达式} 

3.EL表达式主要是在jsp页面中输出数据。主要是输出域对象中的数据。




### Cookie 和 Session
1. 什么是cookie 

2. response.setCookie，通过响应头的set-cookie字段来设置的

3. Cookie的生命控制 
	setMaxAge()
		正数，表示在指定秒数后过期
		负数，默认情况，表示浏览器关闭后就消失
		0，表示删除这个cookie 

4. Path属性
	可以有效的过滤，哪些Cookie可以发送给服务器，哪些不发，
	path属性是通过请求的地址来进行有效过滤的
	
2. session 是一个接口HttpSession
	session就是会话，用来维护一个客户端和服务器之间关联的一种技术
	每个客户端都有自己的一个session会话
	session中我们经常用来保存用户登录之后的信息

3. 如何创建和获取session
	创建和获取session的api是一样的 request.getsession()
	第一此调用是创建session会话，之后调用都是获取前面创建好的session会话
	isNew() 来判断是否为刚创建出来的session
	getId()来获取session会话的id

4. session 的生命周期的设置
	删除session使用session.invalidate();

5. 浏览器和服务器的session的技术关联
	JSESSIONID=996EFAEE0D64874249792D00710B48F8;（id） Path=/cs; HttpOnly
	session是存在服务器内存之中的，但是不保存id，浏览器访问的时候将id传入

