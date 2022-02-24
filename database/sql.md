## mysql实用
1. 相关配置文件
```
/etc/my.inf
/var/lib/mysql/auto.cnf

/log/mysql.log

/etc/resolv.conf
```
2. 相关命令
```
firewall --zone=public --port=3306/tcp --permanent
firewall --reload
```
3. 加密验证算法和ssl
mysql-native....






# 关系数据库设计理论
## 重要术语
属性（attribute）：列的名字，上图有学号、姓名、班级、兴趣爱好、班主任、课程、授课主任、分数。 依赖（relation）：列属性间存在的某种联系。 元组（tuple）：每一个行，如第二行 （1301，小明，13班，篮球，王老师，英语，赵英，70） 就是一个元组 

表（table）：由多个属性，以及众多元组所表示的各个实例组成。 模式（schema）：这里我们指逻辑结构，如 学生信息（学号，姓名，班级，兴趣爱好，班主任，课程，授课主任，分数） 的笼统表述。

域（domain）：数据类型，如string、integer等，上图中每一个属性都有它的数据类型（即域）。

键（key）：由关系的一个或多个属性组成，任意两个键相同的元组，所有属性都相同。需要保证表示键的属性最少。一个关系可以存在好几种键，工程中一般从这些候选键中选出一个作为主键（primary key）。

候选键（prime attribute）：由关系的一个或多个属性组成，候选键都具备键的特征，都有资格成为主键。

超键（super key）：包含键的属性集合，无需保证属性集的最小化。每个键也是超键。可以认为是键的超集。

外键（foreign key）：如果某一个关系A中的一个（组）属性是另一个关系B的键，则该（组）属性在A中称为外键。 

主属性（prime attribute）：所有候选键所包含的属性都是主属性。 

投影（projection）：选取特定的列，如将关系学生信息投影为学号、姓名即得到上表中仅包含学号、姓名的列 

选择（selection）：按照一定条件选取特定元组，如选择上表中分数>80的元组。 

笛卡儿积（交叉连接Cross join）：第一个关系每一行分别与第二个关系的每一行组合。 
  
自然连接（natural join）：第一个关系中每一行与第二个关系的每一行进行匹配，如果得到有交叉部分则合并，若无交叉部分则舍弃。 
连接（theta join）：即加上约束条件的笛卡儿积，先得到笛卡儿积，然后根据约束条件删除不满足的元组。 

外连接（outer join）：执行自然连接后，将舍弃的部分也加入，并且匹配失败处的属性用NULL代替。 

除法运算（division）：关系R除以关系S的结果为T，则T包含所有在R但不在S中的属性，且T的元组与S的元组的所有组合在R中。

## sql
1. SQL(Structured Query Language)，标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

2. 数据库创建与使用:

```
CREATE DATABASE test;
USE test;
```

### 分类
DDL definition
DML manipulation
DQL	qurey
DCL	control 创建数据库用户，控制数据库的访问权限

#### DDL

##### 操作数据库
show databases;

select database();

create database [if not exists] *name* [default charset *name*] [collate 排序规则];

drop dadtabase [if exists] *name*;

use name;

utf8mb4;
utf8 一般存储是三个字节。使用mb4是自个字节

##### 操作表

show tables;

desc 表名;

show create table 表名；

创建表：
create table 表名{
	字段1 字段1类型[comment 字段解释],
	字段1 字段1类型[comment 字段解释],
	字段1 字段1类型[comment 字段解释],
	字段1 字段1类型[comment 字段解释],
	字段1 字段1类型[comment 字段解释],
	字段1 字段1类型[comment 字段解释]
}[comment 表注释];


create table tb_user(
	id int comment '编号',
	name varchar(50) comment '姓名',
	age int comment '年龄',
	gender varchar(1) comment '性别'
) comment '用户表';

*数据类型   SIGNED UNSIGNED
TINYINT 1B
SMALLINT 2B
MEDIUMINT 3B
INT 4B
BIGINT 8B
FLOAT 4B
DOUBLE 8B
DECIMAL 精确描述小数 ，M,D 精度，标度*

score double(4,1)

*字符串类型*

|类型|大小|描述|
|----|--|---------|
|char|255|定长字符串|
|varchar|65535|变长字符串|
|tinyblob|255|二进制数据|
|tinytext|255|短文本|
|blob|65535|二进制长文本|
|text|65535|长文本
|mediumbolb|16777215|
|mediumtext|16777215|
|longblob|4294967295
|longtext|4294967295


char（可存储的最大长度） 效率高
varchar 

*日期类型*
data
time
year
datatime
timestamp   max_value 2038 less use;




##### 修改表
添加字段
alter table 表名 add 字段名 类型 \[comment\] \[约束];

alter table emp add nikename varchar(20) comment "昵称";

修改字段数据类型
alter table 表名 modify 字段名，新数据类型

修改字段
alter table 表名 change 旧字段名 新字段名 类型 【comment】【约束】

删除字段
ALTER TABLE  表名  DROP 字段名

修改表名
ALTER TABLE 表名 RENAME TO 新名字



##### 删除表
drop table [if exists] 表名


truncate table 表名： 用于快速删除表中所有内容，比delete快


#### DML
manipulation

insert
update
delete

insert into 表名(字段1，字段2) values （值1，值2）,（值1，值2）,（值1，值2）;

insert into 表名 values (值1，值2),（值1，值2）,（值1，值2）;

update 表名 set 字段名=值1，字段名=值2，... 【where条件】

delete from 表名 【where 条件】

#### DQL
query

select 字段列表  
from 表名列表  
where 条件列表   
group by 分组条件  
having 分组后的条件列表  
order by 排序字段列表  
limit 分页参数  

设置别名
select 字段1[as 别名1]，。。。 from

去除重复记录
select distinct 字段列表，from 表名


**条件**

|比较运算符|功能|
|------|-----|
|>|
|>=|
|<|
|<=|
|=|
|<> 或!=|
|BETWEEN...AND...|含最大最小值
|IN(...)|在in之后的列表多选1
|LIKE 占位符| 模糊匹配（_匹配单个，%匹配多个）
|IS NULL|

|逻辑运算符|功能|
|------|-----|
|and 或者 &&|并且
|or \|\|
|not ！


聚合函数
count 统计数量
max
min
avg
sum 求和

select 聚合函数（字段） form 表名

分组查询
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件]

执行顺序 where> 聚合函数> having


排序查询
order by

select 字段列表 from 表名 order by 字段1 排序方式1，字段2 排序方式2；
									先用他排，再用他排 
asc 升序 ascend
desc 降序 descend

分页查询
select 字段列表 from 表名 limit 起始索引，查询记录数；

查询页码-1 * 每页的记录数 = 起始索引
分页查询是数据库的方言，不同数据库有不同的实现 

select * from emp limit 11#开始位置,10#长度;


*DQL* 的执行顺序

select 字段列表  
from 表名列表    
where 条件列表     
group by 分组条件    
having 分组后的条件列表  
order by 排序字段列表  
limit 分页参数  

from/ where/ group by having /select/ order by / limit

#### DCL
control 

**用户创建**
查询用户 
use mysql  
select * from user;

创建用户
create user '用户名'@'主机名' identified by '密码'

修改用户密码
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码'；

drop user 用户名@主机名

**权限控制**

|权限|说明|
|-|-|
|ALL,ALL PRIVILEGES|所有权限|
|select|插入数据|
|insert
|update
|delete
|alter
|drop| 删除数据库，表，视图
|create

查询权限
show grants for 用户名

GRANT 权限列表 on 数据库.表名 to 用户

revoke 权限列表 on 数据库.表名 from 用户


## 函数

字符串函数

| 函数 | 功能 |
|-|-|
|concat|链接
|lower
|upper
|lpad(str,n,pad)|左填充，达到n个字符串长度
|rpad(str,n,pad)|右填充
|trim
|substring


数值函数

| 函数 | 功能 |
|-|-|
|ceil(x)|向上取整
|floor(x)|向下
|mod(x,y)
|rand()|0-1
|round(x,y)|四舍五入，保留y位小数

生成六位随机验证码

日期函数
curdate()
curtime()
now()
year()
month()
day()
date()\_add
datediff()

流程控制函数
if(value,t,f)
ifnull(value1,value2)
case when [val1] then [res1] ....else [default] end;
case [exper] when [val1] then [res1] ... else [default] end;

## 约束
作用于表中字段的规则，限制存储在表中的数据
目的是保证数据库中的数据的正确有效性完整性

not null  
unique  
primary key  
default  
check  
foreign key   两张表之间建立链接 保证数据的一致性和完整性 

创建外键
create table 表名(
	字段名 数据类型
	[constriant] 外键名称 foreign key(外键字段名) references 主表(主表列名)
);
修改外键
alter table 表名 add constaint 外键名称 foreign key (外键字段名) references 主表(主表列名);

去除外键
alter table 表名 drop foreign key 外键名称;


删除更新行为
no action  
restrict  上面两个相等，删除父表中的记录时，判断有没有外键与之相连，有的的话就不允许删除更新  

cascade  有的话也把子表中的记录删除或者更新  
set null  有的话将子表设置为null  (前提外键允许被设为null)
set default  有的话将子表设置为default值 （Innodb不支持）

alter talbe 表名 add constraint 外键名称 foreign key 外键字段 references 主表名(主表字段名) on update cascade on delete cascade;

#### 多表查询

##### 多表关系
一对多  
多对多  通过中间表来实现关系  
多对一  
一对一  用于单表的拆分，基础字段放在一张表中，其他字段放在另一张表中。

##### 多表查询
select * from emp, dept ; 笛卡尔积 所有的组合情况

在笛卡尔积上进行取舍就有了所谓内连接，外链接，自连接。。。
链接查询
	内连接 select ... from ... [inner join] ... on ...
	外链接
		左外链接 select ... from ... [left outer join] ... on ...
		右外链接 select ... from ... [right outer join] ... on ...
	自连接 select 字段列表 from 表A 别名A join 表A 别名B on 条件  查询自己的领导是谁

联合查询-union,union all
select ... from A ...
union [all]
select ... from B ...;
列数要匹配

子查询-嵌套查询
select * from t1 where column1 = (select column1 from t2);

标量  
列  in/not in /any满足任何一个就可以 / some满足一些 / all都满足
行  =/<>/in /not in
表  


	
隐式内连接 select 字段列表 from 表1，表2 where 条件

显示内连接 select 字段列表 from 表1 [inner] join 表2 on 链接条件...;


## 事务
操作的集合，要么都做，要么都不做 
mysql的事务是自动提交的

select @@autocommit
set @@autocommit = 0;

commit

rollback

//不修改自动提交
start transaction / beign;
commit
rollback

事务的四大特性 ：
	原子性 
	一致性 所有数据应该保持一致
	隔离性 事务之间互相不影响
	持久性 改变是永久的
	acid

并发事务问题

脏读 一个事务读取到另外一个事务没有提交的数据
不可重复读 两个事务读取的数据不同
幻读 查询的时候没有，插入的时候有了

[一个小疑问][https://segmentfault.com/q/1010000014365001]

事务的隔离界别

|隔离级别|脏读|不可重复读|幻读|
|-|-|-|-
|
|read uncommited| y| y |y
|read committed(orcal default) |n |y |y| 
|repeatable read(default) |n| n |y |
serializable |n |n |n|

并发性能递减
select @@transaction_isolation
set [session|global] transaction isolation level {read uncommited|read committed | repeatable read|serializable}




## 中级
1. 存储引擎  
2. 索引  
3. sql优化  
4. 视图、存储过程，触发器
5. 锁
6. innodb
7. mysql管理
 
### 存储引擎
不同的引擎有不同的适用场景，没有好坏之分
存储引擎 就是 存储数据 建立索引 更新/查询数据等技术的实现方式 引擎是基于表的，而不是基于库的，所以存储引擎也被成为表引擎

默认innodb/ engin = innodb

show engines
几个需要知道的



myisam : 早期默认，不支持事务，不支持外键，支持表锁，不支持行锁，访问所读快
.MYD数据/.MYI索引/.sdi结构

innodb : 高可靠，高性能。 dml 支持acid 支持事务 行级锁， foreign key
	文件 xxxx.ibd 存放了，表结构(frm -> sdi)，数据，索引  innodb_file_per_talbe  

									逻辑存储结构
	TableSpace：
	Segment
	Extent :  1M 
	Page : 基本单元,16k
	Row ： trx id,roll pointer,col...



memory ： 存储在内存当中，临时表，或者缓存
内存存放 hash索引



innodb myisam（nosql mangodb redis) 区别： 事务 外键 锁 

## 索引

帮助mysql高效获取数据的 数据结构
提高了检索效率 降低了io成本    索引也占用空间
降低数据排序成本，抵消CPU消耗     大大提高了查询效率，降低了更新表的速度

索引在存储引擎结构层实现 ，引擎不同索引不同

B+ Tree              最常见
Hash 　　　　　：　　　精确匹配才有效，不支持查询范围
R-tree (空间索引)　　特殊索引MyISAM 索引，主要用于地理空间数据类型
Full-text （全文索引）    倒排索引 ，lucene，solr，es


二叉树 容易退化
红黑树 层级也较深
b树 多路（一个节点可以包含多个数据）平衡查找树

hash索引是memory引擎的东西，innodb有自适应的hash功能，hash索引根据b+tree索引在特定条件下构建而成。

索引分类
主键索引
唯一索引
常规索引
全文索引  
https://stackoverflow.com/questions/707874/differences-between-index-primary-unique-fulltext-in-mysql  



聚集索引clustered index 保存的具体的存储位置
二级索引secondary index 保存者聚集索引
聚集索引的创建规则
主键->第一个唯一索引-> rowid 作为隐藏的聚集索引

回表查询
innodb指针占用6个字节

create [unique|fulltext] index name on table_name (index_col_name...)

show index from table_name

drop index name on table_name

#### sql性能优化
1. sql执行频率
show session|global status Com_...;

2. 慢查询日志
show variables like 'slow_query_log'
long_query_time
/var/lib/mysql/localhost-slow.log

3. show profiles
have_profiling参数
@@profiling

show profiles;
show profile for query query_id
show profile cup for query query_id

4. explain执行计划
  
explain / desc 获取命令执行的信息
explain select ... from ... where ...

id: select操作顺序，嵌套查询，多表查询啥的分开给你显示，id大的先执行   
select_type : simple ， primary 主查询，外层查询 /union，subquery
type： null（不访问表）,system（系统表）,const（主键）（唯一索引访问）,eq_ref,ref（非唯一性索引）,range,index（用了索引，但是遍历所有索引树）,all 由好到差   
possible_key  ： 可能用到的索引

key：实际用到的索引  

key_len: 使用到索引的字节数，并非实际的使用长度，越短越好

row：必须查询的行数，预估值

filtered ： 返回行数占总读取行数的百分比 

id相同从上到下  

索引的使用原则
1. （复合索引）最左前缀法则： 索引了多列 需要遵守，查询不能跳列，跳过的列的后面的索引会失效  
2. 范围查询（复合索引）：范围查询右边的索引会失效 >=不会失效
3. 索引运算会失效
4. 字符串类型不加单引号索引失效
5. 模糊查询： 仅尾部进行模糊匹配索引不会失效
6. or链接条件，都有索引才会生效
7. 数据分布影响，mysql会评估使用索引慢就用全表扫描
8. sql提示 多个索引都匹配，可以指定索引

use index 建议
ignore index  
force index 强迫

9. 覆盖索引 ，尽量不要用select * 。 选择的尽量在索引里。是否需要回表查询
10. 前缀索引，对于字符串，文本的设计的索引，大大节省索引空间。
create index idx_xxxx on table_name(column(n));
关于长度 选择性，不重复的值
select count(distinct name)/ count(\*) from tb_user;
select count(distinct substring(email,1,5))/count(\*) from tb_user;

11. 单列索引和联合索引的选择问题  
 最好使用单列索引


索引的设计原则
1. 数据量大，查询比较频繁的表
2. 常作为 where,order,group by 操作的字段
3. 尽量选择区分度高，尽量建立唯一索引。
4. 根据字段特点，建立前缀索引
5. 尽量建立联合索引，减少单列索引，查询时联合索引很多时候可以覆盖索引，节省 存储空间。
6. 需要控制索引数量，并不是越多越好，会影响增删改的速度
7. 索引列不能存储null值，创建表的时候应该标明


### 其他sql语句的优化
1. 使用批量插入，几千条
2. 手动提交事务
3. 主键顺序插入
4. 大批量的数据插入 Load
mysql --local-infile -u root -p
set global local_infile = 1;
load data local infile (path) fields terminated by '' lines terminated by ''  

#### 主键优化
索引组织表(index organized table IOT)

一页至少包含两行数据/行溢出
底层类似数组，不是顺序存入需要频繁移动   

页分裂
页合并  页中删除的行数据达到特定值 (merge_threshold 自己设置或者创建表或者索引的时候设置) 会合并

主键设计原则：
1. 降低主键长度
2. 顺序插入
3. 不要使用 数据本身的unique id 来当主键 
4. 避免对主键的修改

#### order by优化
1. using filesort 不是通过索引直接返回排序
2. using index 效率高，不需要额外排序
show variable like 'sort_buffer_size'

#### group by 优化

#### limit 优化
大数据情况下，limit耗时太长
覆盖索引，加子查询

#### count 优化
myisam 将表的长度存在了磁盘上
innodb 没有存，只是一个一个的计数

自己维护一个计数器

count的几种方式
cout(\*) 不取值，直接累加，做了优化 ,count(主键),count(字段),count(1) 

#### update 语句的注意事项
避免行锁上升为表锁 更新一定要加索引

## 视图，存储过程，触发器，存储函数 存储对象

1. 视图   
		 虚拟存在的的表，基表。view。视图中的数据是动态生成的。保存的是sql的逻辑，不保存数据  

--创建视图  
create [or replace] view 视图名称[(列名列表)] AS select语句 [with[cascaded|local] check option]  

查询视图  
show create view 视图名称  
select * from 视图名称....;  

修改视图  
create [or replace] view  视图名称[(列名列表)] As select语句 ...
alter view 视图名称 as select 。。。  

drop view [if exists] 视图名称,  

视图检查选项 local cascaded  
通过检查选项来检查视图中的每一行。mysql允许基于一个视图创建另一个视图。  
cascade（默认值）会检查所有依赖，local  

##### 视图的更新
1. 视图中的行与表中的行具有一对一的关系  
聚合函数或者窗口函数sum,min,max(),count  
distinct,group by,having,union或者union all
  
##### 视图的作用
1. 操作简单，将复杂的查询条件打包。
2. 安全： 数据库可以授权，但是表中字段没法授权，所以可以通过视图实现。

##### 存储过程
 多条sql语句的集合。  
 简化应用开发人员的工作  
 特点: 封装，复用，可以接受参数，也可以返回数据，可以减少网络交互，提升效率  

 基本语法： 
 		create procedure 名称([参数列表，输入返回])
 		begin
 			sql语句
 		end

 		调用
 		call 名称（参数）

 		查看
 		Select * from information_schema.routines where routine_schema='xxx'
 		show create procedure 存储过程的名称

 		删除
 		drop procedure 名称


 		命令行中遇到分号就结束了，所以需要用delimiter 来指定语句结束符号
 		delimiter $$



存储过程的语法结构  

###### 变量： 系统，用户自定义，局部

系统变量：  mysql服务器提供，不许哟啊用户定义，全局global，会话变量session   

查看系统变量：  
show [session|global] variables;  
show [session|global] variables like '...';  
select @@[session|global] 系统变量名;  

设置系统变量  
set [session|global] 变量名 =   
set @@变量名=  

系统重启会重置  
如果想要永久修改，需要修改配置文件  

用户自定义变量：  
不需要提前声明  用@就好（@@是系统）session作用域  没有赋值的变量获得就是null  
set @var_name = expr,  
set @var_name := expr,  
select @var_name:= expr;  
select 字段名 into @varname from 表名 从表中查数据赋值给变量  

使用  
select @varname  

局部变量  
declare声明，可作为存储过程内的局部变量或者输入参数   begin end 块之内  
declare 变量名，变量类型 [default...];  

set/select 赋值  

###### if条件判断
if condition1 then
elseif condition2 then
else
end if;

存储过程的参数：  
in  输入参数 默认  
out  输出参数  
inout  即可作为输出也可作为输入  

create procedure 名字([in/out/inout 参数名 参数类型])  
begin  
  
end;  

###### case


case (case_value)
		when when_value1 then statement_list1  
		...  
		....  
		else   
ens case;  


###### 循环控制
while 条件 do  
  
end while；  


repeat 当满足条件退出循环  
repeat  
	sql  
 	until  
end repeat  

loop: leave/iterate

游标： 用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环处理。  
open / fetch / close  
delclare 名字 cursor for sql语句
open 名字  
fetch 游标名称 into 变量\[,变量...\];  
close  名字   

游标变量应该在最后声明  

条件处理程序 handler   
```
declare handler_action handler for condition_value[,condition_value]... statement;  
handler_action:  
		continue;  
		exit;  
```

```
condition_value:  
	sqlstate sqlstate_value  状态码 如 0200  
	sqlwarning 所有01 开头的sqlstate 的简写  
	not found : 所有02 开头的sqlstate的简写  
	sqlexception： 所有没有被sqlwarning，notfound捕获的sqlstate的简写  
```

存储函数： 有返回值的存储过程，参数只能是in类型 具体语法如下:  

```create function 名字（参数列表）  
returns type \[characteristic 特性值，deterministic/no sql/ reads sql data\]  
begin
	sql
	return ...;
end;
```

二级日志开启就必须要加如特性字段    

触发器  
与表有关，在insert/update/delete之前或者之后，触发并执行触发器中定义的sql语句的集合。可以帮助保证数据的完整性，日志记录，数据校验等操作。  
old，new 引用了原来的记录内容和新的记录内容，与其他数据库类似。只支持行级触发，不支持语句触发。  

1. 创建  
create trigger trigger_name  
before/after insert/update/delete  
on tbl_name for each row  行触发器  
begin  
	trigger_stmt;  
end;  

2. 查看
show triggers;

drop trigger [schema_name.]trigger_name;
now()函数

### 锁
1. 概述  
一种机制  
2. 分类   
全局锁  锁住所有表  
表级锁  每次操作锁住整张表  
行级锁  每次操作锁住处理的行数据  

全局锁：对整个数据库实例加锁，加锁后整个实例出于只读状态，后续的dml和ddl语句  
应用场景：左全库的逻辑备份，对所有的表进行锁定，从而保证一致性的视图保证数据的完整性  
flush tables with read lock  
unlock tables;
会导致主从延迟  
--single-transaction 不加锁的备份，底层通过快照实现  

2. 表级锁
锁表，锁定粒度大，冲突高，并发低 myisam,innodb,dbd  
表锁/ 元数据锁 / 意向锁

表锁， 读，写  
lock tables 表名 read/write  
unlock tables  或者断开链接   

元数据锁，不允许更改表结构，自动加上

意向锁
select ... lock in share mode
共享锁 与 表锁共享锁read兼容，与表锁排他锁互斥

insert,update,delete,select ... for update
意向排他锁ix 与表锁共享锁及排他锁都互斥，意向锁之间不会互斥

select * from performance_schema.data_locks;


#### 行级锁
主要用在innodb存储引擎中。myisam和innodb区别，事务，外键，行级锁  
innodb 数据基于索引组织，航所通过对索引上的索引项加锁实现的，而不是对记录加锁。  
1. 行锁（record lock） 锁定单行记录，防止事务进行update和delete，rc rr隔离级别支持  
2. 间隙锁（gap lock）：防止其他事务对间隙进行insert，产生幻读现象，rr级别下支持
3. 临键锁： （next-key lock） 行锁和间隙锁的组合，同时锁住数据，并锁住数据前面的间隙gap 在rr级别下支持  

1. S 共享锁： 允许读，阻止其他事务获得相同数据集的排他锁
2. X 排他锁： 允许获取排他锁的事务更新数据，阻止其他事务获得排他锁和共享锁  

https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html  


#### 一些机制


对唯一索引进行检索时，对   已存在的记录  进行等值匹配时，会优化为行锁  
不通过 索引条件 检索数据，innodb会对所有记录加锁，此时就会升级为表锁  


默认情况下，innodb在RR 事务隔离级别运行，使用next-key 锁进行搜索和扫描，防止幻读  
1. 索引上的等值查询（唯一索引），给不存在的记录加锁的时候，优化为间隙索引  
2. 索引上的等值查询（普通索引）， 向右遍历时最后一个值不满足查询需求时，next-key lock退化为间隙锁  
3. 索引上的范围查询（唯一索引） -- 会访问到不满足条件的第一个值为止  

目的是防止其他事务插入间隙，间隙锁可以共存，一个事务采用的间隙锁，不会阻止另一个事务在同一间隙上采用间隙锁



### innodb 引擎

#### 逻辑结构

表空间TableSpace->段Segment->区Extent->页Page->行Row

TableSpace :  .ibd file.
Segement : leaf node Segment 数据段，non-leaf node segment 索引段，rollback segment 回滚段  
区 ：　表空间的单元结构，１M 大小，页大小16k 所以有64页  
页 ： 磁盘管理的最小单元，每页16k，为了保证页的连续性 每次申请4-5 个区  
行 ： 按行进行存放  

#### 架构
innodb 擅长事务处理，具有崩溃回复的特性  


##### 内存结构
1. 缓冲池 buffer pool  
页为单位，链表数据结构管理，页类型 free / clean / dirty  



2. change buffer 更改缓冲区   针对于非唯一二级索引来说的  
执行dml的时候，如果数据没有在buffer pool 内时，不会操作磁盘，而是会存在change buffer中，在未来数据被读取的时候，合并到buffer pool 中，再刷新到磁盘上  
二级索引非唯一  插入的位置也比较随机，有了更改缓冲区可以减少磁盘io  

3. adaptive Hash index ----> buffer pool 自适应的hash索引  
参数 adaptive_hash_index  

4. log buffer(redo log,undo log) 16mb 如果需哟啊更新和删除多行数据，增加日志缓冲区的大小可以节省磁盘io  
innodb_log_buffer_size;  
innodb_flush_log_at_trx_commit 日志刷新到磁盘的时机   
1：日志在每次事务提交的时候刷新到磁盘当中  
0：每秒刷新一次  
2： 事务提交+每秒  


##### 磁盘结构
1. System Tablespace 系统表空间： 存放change buffer 的区域 innodb_file_pre_table 打开，就不存表文件，如果关闭了就也会存储表文件  
参数 innodb_data_file_path


2. File_per_table tablespace 每个表的独立表空间  
参数innodb_file_per_talbe (default on)  

3. General Tablespace：通用表空间  create tablespace 语法创建，在创建表的时候可以指定该表空间  
create tablespace xxx add detafile 'file_name' engine = engine_name  

4. Undo Tablesapce：撤销表空间。初始化会创建两个Undo表空间默认16m 存储undo日志  

5. Temporary Talbesapce临时表空间：临时回话，全局。存储用户创建的临时表等数据  
6. doublewrite Buffer files 双写缓冲区： innodb将数据页从buffer pool刷新到磁盘前，会放在这里  便于系统异常的时候恢复数据  

7. redo log重做日志：  事务的持久性。redo log buffer，redo log 内存，磁盘  
用于将脏页刷新到磁盘中 发生错误时，进行数据恢复  
以循环方式写入 ib_logfile0,1

##### 后台线程
1. Master Thread
核心后台线程，调度其他线程，将缓冲池中的数据   异步刷新到磁盘当中。脏页的刷新， 合并插入缓存，undo页的回收  

2. IO Thread： 使用AIO来处理 异步非堵塞 的io，可以极大提高数据库的性能。
线程： 
Read thread 4个
Write thread 4个
Log thread 1
Insert buffer thread 1

3. Purge Thread : 回收事务已经提交了的undo log
4. Page Cleaner Thread : 协助master Thread 刷新脏页到磁盘的线程，减轻Master Thread 的压力，减少阻塞  



#### 事务原理
acid
原子性
一致性
隔离性
持久性
acd： redo log，undo log
i： 锁，mvcc

1. redo log :　记录事务提交时数据页的修改，用来实现事务的持久性。
redo log buffer，redo log file
WAL write-ahead-log

2. undo log : 回滚日志，用于记录数据被修改之前的信息。作用： 提供回滚，mvcc（多版本并发控制）
undo log 逻辑日志，记录反向的操作 例如：delete->insert 
undo log 的销毁，一般事务提交就销毁了，但有例外，还在被mvcc用
undo log 的存储，段，存放在rollback segment中，内部包含1024个unod log segment

##### mvcc 多版本并发控制
1.  当前读  
读取的记录是最新的，并且要保证其他的并发事务 不能修改当前记录。会加锁  
比如 select ... lock in share mode;

2. 快照读
读取的是记录数据的可见版本，普通的select 就是快照读，不加锁，是非阻塞读  
Read Committed：每次select 都会生成一个快照读
Repeatable Read: 开启事务后的第一个select 语句才是快照读
serializable： 快照读会退化为当前读

3. mvcc
Multi-Version Concurrency Control,维护一个数据的多个版本，使读写操作没有冲突 ，快照读为mysql实现mvcc提供了一个非阻塞读的功能  

实现依赖，数据库记录中的三个隐式字段，undo log,readView。
DB_TRX_ID / DB_ROLL_PTR /DB_ROW_ID
最近修改事务的id
回滚指针，指向记录的上一个版本，配合undo log使用  
隐式主键，没有主键的时候才有。  

undo log补充：insert的时候，产生的只在回滚的时候需要，在事务提交以后可以被立即删除  
update和delete的时候产生的undo log在读快照的时候也需要。不会立即删除。
undo log 版本链   

readview ： mvcc提取数据的依据，
m_ids：当前活跃的事务id集合
min_trx_id：最小活跃事务id
max_trx_id：预分配事务id，当前事务id+1
creator_trx_id:readview创建者的事务id  
版本链的访问规则  

生成时机：    
rc：事务每执行一次快照读时生成readView    
rr： 仅在事务第一次执行快照读的时候生成readView 后续复用该ReadView   

#### mysql管理
1. 系统数据库
information_schema ：　提供访问数据库元数据的各种表视图，包含数据库，表，字段类型及访问权限等
mysql ： 存储mysql服务器正常运行所需要的各种信息（时区，主从，用户，权限）
performance_schema：为ｍｙｓｑｌ服务器运行是状态提供了一个底层监控的功能，主要用于手机数据库服务器性能参数　　
sys：

##### 常用工具
mysql -e 
mysqladmin 
mysqlbinlog 二进制日志管理
mysqlshow --count -i  
mysqldump  
数据导入  
mysqlimport  
source  


### 运维篇
日志/主从复制/分库分表/读写分离

#### 日志

1. 错误日志：
/var/log     show varialbes like '%log_error%'

2.  二进制日志
binlog 记录了所有ddl 和dml语句 不包括select 和show
灾难时的数据恢复；mysql的主从复制；
show variables like '%log_bin%'
statement/row/mixed 基于sql，基于行，混合
reset master/purge master logs to 'binlog.xxxx'
purge master logs before 
show varibles like '%binlog_expire_logs_seconds%'

3. 查询日志
所有的增删改查都会被记录
默认关闭
general_log
general_log_file 

4. 慢查询日志
语句效率低的语句  
log_qurey_time<  < min_exmained_row_limit

#### 主从复制
1. 可以预防意外
2. 降低主库访问压力，读写分离
3. 可以在从库备份，以免影响主库
relay log
三部  

主库配置 my.cnf
server-id = 1
read-only = 0
binlog-ignore-db = mysql
binlog-do-db = db01
创建用户，赋予权限
show master status

从库配置 /etc/my.cnf
server-id = 2
read-only = 1

https://dev.mysql.com/doc/refman/8.0/en/replication.html



#### 分库分表
将数据分散存储，是的单一数据库表的数据量变小，解决性能问题
单数据库存储会有两个问题
io瓶颈：大量磁盘io 网络io瓶颈
cpu瓶颈：排序分组，聚合统计，cpu会出现瓶颈

拆分策略：
垂直 分库，分表
水平 分库，分表

实现
1. shardingjdbc 基于aop原理，在应用程序中对本地执行的sql进行、拦截、解析、改写 路由处理。需哟啊自行编码配置实现，只支持java语言，性能比较高
2. mycat：数据库分库分表的中间件，支持多种语言，性能不如sharding jdbc

mycat 数据库中间件  
schema.xml 以及相关配置  
server.xml 配置用户信息
mycat配置文件详解：
schema.xml
逻辑库和表  
1. schema标签  
2. table标签  <table name='TB ORDER' dataNode='' rule='' primaryKey  type/>
3. <dataNode>
4. <dataHost>

rule.mxl
 

server.xml 

Mycat 分片
垂直拆分
                
水平分片的规则：
范围分片
取模分片
一致性hash
枚举分片
substring
固定分片hash算法
字符串hash解析

mycat的管理与监控
底层运行原理： 
解析sql->分片->路由->读写分离  
分页<-排序<-聚合<-结果合并  


监控平台
Zookeeper配置中心
MyCat-Web
server.xml 里 定义了 8066,9066端口

#### mycat主从复制
也就是用的mycat的路由 寻路实现的  
1. <dataHost> 内的balance属性 0：不开启读写分离，所有操作发送到writehost上。1. 全部的readhost和备用的writehost都参与select 语句的负载均衡（双主双从） 2.所有的读写随机分发 3.所有的读请求随机分发到writehost对应的readhost ， writehost不负担读压力  


