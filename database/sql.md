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
