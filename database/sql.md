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

show databases;

select database();

create database [if not exists] *name* [default charset *name*] [collate 排序规则];

drop dadtabase [if exists] *name*;

