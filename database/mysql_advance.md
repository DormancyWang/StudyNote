### MySql的数据目录
1. mysql8
	1. 数据库文件的存放目录: /var/lib/mysql  'datadir'
	2. 相关命令: /usr/bin   /usr/sbin
	3. 配置文件的目录： /usr/share/mysql-8.0  /etc/my.cnf
2. 数据库与文件系统
	1. 系统数据库四个 mysql,information_schema,sys,performance
	![](image/database.jpg)
	![](image/database2.jpg)
	2. 数据库在文件系统之中表示为文件夹
	3. 表在文件系统之中表示
		1. inoodb引擎
			1. 系统表空间
			![](image/systemTablespace.jpg)
			2. 独立表空间(file-per-table table space)
			.frm/.ibd
			3. innodb_file_per_table 参数
			4. general tablespace/tempory tablespace
		2. MyISAM
			1. 表结构相同 .frm
			2. 数据索引分开放的 .MYD/.MYI
3. 总结
![](image/conclusion.jpg)

### 用户权限和管理
1. 用户管理
	1. 创建用户 
	2. 修改用户
	3. 删除用户
	4. 修改当前用户密码
	5. 修改其他用户密码
2. 权限管理
	1. 权限列表 show privileges
	2. 授予权限 grant all privileges on \*.\* to user; 唯独不包含 权限授予 权限 。权限的不可传递性 with grant option 有赋权权限
	3. 查看权限 show grants for user;
	4. 收回权限
3. 权限表
	1. user
	2. db
	3. tables_priv , columns_priv
	4. procs_priv
4. 角色管理

### 配置文件


### 逻辑架构
![](image/mysql_architecture.jpg)  
1. sql的执行流程  
	1. @@profiling , show profiles;show profile for 
	
2. 缓冲池：存储在内存的表  
	1. ![](image/innodb-architecture.png)  
	2. 相关设置变量  
	innoodb_buffer_pool_size  
	innoodb_buffer_pool_instance  


### 存储引擎
1. 概述
	1. 
		![](image/engines.jpg)
	2. 默认引擎参数 storage_engine
2. 详细介绍
	1. innodb
		![](image/innodb.jpg)
	2. MyISAM
		![](image/MyISAM.jpg)
	3. Archive 归档索引，仅允许插入和查询
	4. Blackhole 黑洞索引
	5. csv引擎 数据存储在csv文件中，可以和excel的软件交互
	6. Memory，存储在内存中的表，使用hash索引
		！[](image/Memory.jpg)
	7. Federated 引擎：访问远程表Federated引擎是访问其他MySQL服务器的一个代理	，器的灵活性，但也经常带来问题，因此默认是禁用的。
 	8. Merge引擎：管理多个MyISAM表构成的
 	9. NDB引擎：MySQL集群专用存储引擎
 

### 索引的数据结构
1. 索引定义
2. 索引的分类
	1. 聚集索引：索引的叶子节点存储的是完整的用户记录
	2. 二级索引（辅助索引，非聚集索引）： 叶子节点存放了部分的用户记录。 *回表查询*
	3. 联合索引：多个字段组成的索引，也是一种二级索引。排序方式可以有升序和降序
	4. b树，b+树，二叉排序树： 二叉排序树是鼻祖，b树是为了磁盘查找来设计的，因为磁盘的特殊性，读取数据需要一块一块读，并且io时间极长，所以减少io是b树的设计目标。 由于一块一块读，一个块block就需要存储更多的可以用来查找的信息。由此，b树一个块就不放一个节点了（那就是二叉树了），放很多个节点，你自己找你所在的区间。 但是做的还不够绝， 所以b+树干脆，只放索引定义的值（主键值），其他的都放到叶子节点去。这样不仅降低了树的高度，而且宽度也同样增加了不少。数据存储能力变大，查找时间还少了，并且查找每个数据的时间一样，很稳定。甚好。
	5. 页分裂的点
3. Innodb的b+树索引的注意事项
	1. 根页面位置不动
	2. 内节点中目录项记录的唯一性
	会自动加上主键值
	3. 一个 页面最少存储两条记录
4. MyISAM中
	1. 数据和索引分离
	2. 数据按插入顺序，有定长和不定长之分，
	3. 索引算是二级索引，存标号或者直接偏移地址
	4. 回表极快
5. innodb两个例子
	1. 主键不要太长
	2. 主键最好单调，不然老是要调整树结构
	

