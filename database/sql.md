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
```CREATE DATABASE test;
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

#### 索引

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


聚集索引clustered index
二级索引secondary index
聚集索引的创建规则
主键->第一个唯一索引-> rowid 作为隐藏的聚集索引





