#JDBC 
1. 数据的持久化  
	将数据保存到课掉电式存储设备中以供使用  
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

2. 