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