### Mybatis与数据库进行交互，持久化层框架（sql映射框架）
1. 原始的工具  
JDBC-dbutils-jdbcTemplate
2. 框架要考虑的问题
	缓存，异常处理，部分字段的问题
3. Hibernate(orm框架)
	封装程度太高，反模式

4. 需求，可以定制sql，并且不许要硬编码到类中
mybatis：
	写sql在配置文件中。其他还是由mybatis自动执行
	mybatis底层就是对原生jdbc的一个封装

### mybatis 配置
1. 写一个配置文件 mybatis-config.xml

2. 可以使用properties标签导入外部.properties文件

3. 查看官网创建sqlSessionFactory,获取链接

4. 写mapper，mybatis-config.xmll里面写命名空间

5. 日志依赖log4j

### ORM object relation mapping


### XML config file

- properties

- settings 一些设置都是在这里
	* useColumnLabel 驼峰命名转化
	* lazyLoadingEnabled 延迟加载

+ typeAliases： 设置类型别名,就是给类起一个简短的名字

- typeHandlers： 类型处理器

- objectFactory

- plugins ： 设置拦截器的

- environments ： 设置数据源环境的

- databasesIdProvider ： 设置数据厂商提供

- mappers ： 设置映射的


### XML mappingfile
\#{}是预编译，不需要自己加双引号
${}是字符串替换 需要自己加引号

- cache 为特定的命名空间设置缓存

- cache-ref 从别的命名空间中引用缓存设置

- resultMap 最强大的和复杂的元素，描述了如何处理返回对象
	+ 写法如下
	```xml
		<resultMap id="detailedBlogResultMap" type="Blog">
			# 构造器来进行初始话
		  <constructor>
		    <idArg column="blog_id" javaType="int"/>
		  </constructor>
		  //普通的resultmap元素
		  <result property="title" column="blog_title"/>
		  //级联元素
		  <association property="author" javaType="Author">
		    //指定id
		    <id property="id" column="author_id"/>
		    <result property="username" column="author_username"/>
		    <result property="password" column="author_password"/>
		    <result property="email" column="author_email"/>
		    <result property="bio" column="author_bio"/>
		    <result property="favouriteSection" column="author_favourite_section"/>
		    //分布查询
		    //select：设置分步查询，查询某个属性的值的sql的标识（namespace.sqlId//） column：将sql以及查询结果中的某个字段设置为分步查询的条件
		  <association property="dept" 
			select="com.atguigu.MyBatis.mapper.DeptMapper.getEmpDeptByStep" 
			column="did"/>

		  //指定集合
		  <collection property="posts" ofType="Post">
		    <id property="id" column="post_id"/>
		    <result property="subject" column="post_subject"/>
		    <association property="author" javaType="Author"/>
		    <collection property="comments" ofType="Comment">
		      <id property="id" column="comment_id"/>
		    </collection>
		    <collection property="tags" ofType="Tag" >
		      <id property="id" column="tag_id"/>
		    </collection>
		    //返回值来确定使用什么样的返回类型
		    <discriminator javaType="int" column="draft">
		      <case value="1" resultType="DraftPost"/>
		    </discriminator>
		  </collection>
		</resultMap>
	```
	- 分部查询与懒加载
		+ 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
		lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
		aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个
		属性会按需加载
		此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和
		collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加
		载)|eager(立即加载)"

- sql 公共sql抽取工具

- insert,update,delete
- select

查询返回多条数据：直接写集合内的属性就可以了

#### 动态sql

- if

- where

- trim

- choose，when，otherwise

- foreach

#### mybatis的缓存

- mybatis有两级缓存
	+ 一是sqlSession的缓存
	+ 二是SqlSessionFactory，需要手动开启 cacheEnabled=true 或者使用cache 标签
	+ 二级缓存必须在sqlsession关闭或者提交以后才有效，查询的数据所转换的的实体类类型必须实现序列化

### 逆向工程 和 分页工具
直接看文档了