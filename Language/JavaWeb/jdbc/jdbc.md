#JDBC 
1. 数据的持久化  
	将数据保存到可掉电式存储设备中以供使用  
2. java数据存储技术  
	1. jdbc    
	2. jdo  
	3. 第三方mybatis，hibernate  
	4. 维服务  
3. jdbc  
	独立于数据库管理系统的api公共的接口 / java.sql / javax.sql  
	两个层次 java api 供开发人员使用 java driver api 供开发商开发数据库驱动使用
	odbc是微软的sql server定义的api / 所以连sqlserver用odbc-jdbc桥

### 获取链接
1. Driver
	1. 使用反射的方式来构建Driver来获得更好的移植性
		
		 使用DriverManager来获取链接
	
		``` java
		Class clazz = Class.forName("com.mysql.jdbc.Driver");
		Driver driver = (Driver) clazz.getDeclaredConstructor().newInstance();
		DriverManager.registerDriver(driver);
        Connection conn = DriverManager.getConnection(url,user,password);
		```
		
		mysql的driver实现类更新了 放在了com.mysql.cj.jdbc.Driver并且自动调用了DriverManager的注册方法
		
		```	java
		static {
			try {
				DriverManager.registerDriver(new Driver());
			} catch (SQLException var1) {
				throw new RuntimeException("Can't register driver!");
			}
		}
		```
		
		使用配置文件的方式
		```java
			@Test
	    	public void testConnection4() throws Exception {
				//Class clazz =
				//Class.forName("com.mysql.cj.jdbc.Driver");
				// Driver driver = (Driver) clazz.getDeclaredConstructor().newInstance();

				//DriverManager.registerDriver(driver);
				Properties properties = new Properties();
				properties.load(new FileInputStream("jdbc.properties"));

				Class.forName(properties.getProperty("driverClass"));

				Connection conn = DriverManager.getConnection(properties.getProperty("url"),properties);

				System.out.println(conn);
			}
		```

### 使用PreparedStatement实现CRUD

1. java.sql 包中定义了三种对数据库的调用方式
	1. statement : 执行静态的sql语句
	2. PreparedStatement : 预编译，可多次高效的执行语句
	3. CallableStatement : 用于执行存储过程

2. Statement 会造成sql注入的问题所以引用PreparedStatement替换Statement 就可以了
3. PreparedStatement
	? 占位符
	setString 设置占位符
	
4. 封装获取链接的方法，和关闭资源的方法来减少代码量

5. 查询结果的处理
	1. ORM(object relational mapping) 编程思想
		一个数据表对应一个java类
		表中的一条记录对应java类的一个对象
		表中的一个字段对应java类中的一个属性
	
	2. java和sql对应数据类型转换表
	3. 如果数据库中的字段名与类中的属性名不一样，可以使用ResultSetMetaData.getColumnLabel 并且在sql中起好别名

6. 使用反射编写通用的方法
7. PreparedStatement 的特点
	1. 可以操作Blob数据 而Statement做不到
	2. 可以更高效的批量操作
	



### 数据库链接池

1 JDBC数据库连接池的必要性  ：
	
	1. 在使用开发基于数据库的web程序时，传统的模式基本是按以下步骤：
		1. 在主程序（如servlet、beans）中建立数据库连接
		2. 进行sql操作
		3. 断开数据库连接
	
	问题：
		普通的JDBC数据库连接使用 DriverManager 来获取，每次向数据库建立连接的时候都要将 Connection
		加载到内存中，再验证用户名和密码(得花费0.05s～1s的时间)。需要数据库连接的时候，就向数据库要求
		一个，执行完成后再断开连接。这样的方式将会消耗大量的资源和时间。数据库的连接资源并没有得到很
		好的重复利用。若同时有几百人甚至几千人在线，频繁的进行数据库连接操作将占用很多的系统资源，严
		重的甚至会造成服务器的崩溃。
	
		对于每一次数据库连接，使用完后都得断开。否则，如果程序出现异常而未能关闭，将会导致数据库系统
		中的**内存泄漏（链接创建没有被回收）**，最终将导致重启数据库。（回忆：何为Java的内存泄漏？）
	
		这种开发不能控制被创建的连接对象数，系统资源会被毫无顾及的分配出去，如连接过多，也可能导致内
		存泄漏，服务器崩溃。
	
2 数据库连接池技术
	为解决传统开发中的数据库连接问题，可以采用数据库连接池技术。
		
	数据库连接池的基本思想：就是为数据库连接建立一个“缓冲池”。预先在缓冲池中放入一定数量的连接，当需要
	建立数据库连接时，只需从“缓冲池”中取出一个，使用完毕之后再放回去。
		
	数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是重
	新建立一个。
		
	数据库连接池在初始化时将创建一定数量的数据库连接放到连接池中，这些数据库连接的数量是由最小数据库
	连接数来设定的。无论这些数据库连接是否被使用，连接池都将一直保证至少拥有这么多的连接数量。连接池
	的最大数据库连接数量限定了这个连接池能占有的最大连接数，当应用程序向连接池请求的连接数超过最大连
	接数量时，这些请求将被加入到等待队列中。
		
		
3. 数据库连接池技术的优点
	1. 资源重用
	由于数据库连接得以重用，避免了频繁创建，释放连接引起的大量性能开销。在减少系统消耗的基础上，另一
	方面也增加了系统运行环境的平稳性。
	
	2. 更快的系统反应速度
	数据库连接池在初始化过程中，往往已经创建了若干数据库连接置于连接池中备用。此时连接的初始化工作均
	已完成。对于业务请求处理而言，直接利用现有可用连接，避免了数据库连接初始化和释放过程的时间开销，
	从而减少了系统的响应时间
	
	3. 新的资源分配手段
	对于多应用共享同一数据库的系统而言，可在应用层通过数据库连接池的配置，实现某一应用最大可用数据库
	连接数的限制，避免某一应用独占所有的数据库资源
	
	4. 统一的连接管理，避免数据库连接泄漏
	在较为完善的数据库连接池实现中，可根据预先的占用超时设定，强制回收被占用连接，从而避免了常规数据
	库连接操作中可能出现的资源泄露

4. 多种开源的数据库连接池  
	JDBC 的数据库连接池使用 javax.sql.DataSource 来表示，DataSource 只是一个接口，该接口通常由服务器
	(Weblogic, WebSphere, Tomcat)提供实现，也有一些开源组织提供实现：
		
		DBCP 是Apache提供的数据库连接池。tomcat 服务器自带dbcp数据库连接池。速度相对c3p0较快，但因
		自身存在BUG，Hibernate3已不再提供支持。
		
		C3P0 是一个开源组织提供的一个数据库连接池，速度相对较慢，稳定性还可以。hibernate官方推荐使用
		
		Proxool 是sourceforge下的一个开源项目数据库连接池，有监控连接池状态的功能，稳定性较c3p0差一
		点
		
		BoneCP 是一个开源组织提供的数据库连接池，速度快
		
		Druid 是阿里提供的数据库连接池，据说是集DBCP 、C3P0 、Proxool 优点于一身的数据库连接池，但是
		速度不确定是否有BoneCP快
	
	DataSource 通常被称为数据源，它包含连接池和连接池管理两个部分，习惯上也经常把 DataSource 称为连接
	池
	
	DataSource用来取代DriverManager来获取Connection，获取速度快，同时可以大幅度提高数据库访问速
	度。
	
```
	数据源和数据库连接不同，数据源无需创建多个，它是产生数据库连接的工厂，因此整个应用只需要一个
	数据源即可。
	
	当数据库访问结束后，程序还是像以前一样关闭数据库连接：conn.close(); 但conn.close()并没有关闭数
	据库的物理连接，它仅仅把数据库连接释放，归还给了数据库连接池
```
	数据库链接池需要使用单例模式
	
	

	

