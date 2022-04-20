https://baomidou.com/pages/24112f/#%E7%89%B9%E6%80%A7

### mybatis-plus 的使用
1. 导入mybatis-plus的starter

2. 配置数据源信息

3. 配置包扫描

4. 写pojo和mapper 继承BaseMapper

### BaseMapper

1. insert   
	- insert(T) :传入对象插入即可
	
2. delete
	- int deleteById(Serializable id); 根据id删除
	- 根据map中封装的条件删除
	- 根据wrapper删除
	- 根据ids删除
	
3. update
	- 根据pojo
	- 根据wrapper条件更新
	
4. select
	- 根据id
	- 根据ids
	- 根据map
	- 查找一个
	- 查找技术
	- 查找出list
	- 查找出maps
	- 查找出objs
	- 查找页数并翻页


#### 自定义
以上是对单表的查询，而且表名什么的都是自动的。不够灵活。
```java
@ConfigurationProperties(
    prefix = "mybatis-plus"
)
public class MybatisPlusProperties {
 	private String[] mapperLocations = new String[]{"classpath*:/mapper/**/*.xml"};
}
```
可以直接写mapperxml来定制功能

#### Service
IService 接口   
serviceImpl 实现类    
一般是继承一下，复杂的逻辑自己写    

一个好用的工具类  
sqlHelper  


#### 常用注解

- TableName:指定表名
	+ 全局配置 mybatisPlus:global-config:db-config:table-prefix:

- TableId
	+ IdType:主键的生成策略
		* auto:数据库自增
		* none：跟随全局
		* input：insert前自动set之间值
		* assign_id：雪花算法
		* assign_uuid： uuid
	+ 全局配置文件中自动是assign_id,可以更改，位置：mybatis-plus:global-config:db-config:id-type: 

- 雪花算法
	+  背景   
		需要选择合适的方案去应对数据规模的增长，以应对逐渐增长的访问压力和数据量。
		数据库的扩展方式主要包括：业务分库、主从复制，数据库分表。
	+ 雪花算法   
		非常适合分布式的主键生成算法，可以保证不同表的主键的不重复性，相同表的主键有序性
	优点：整体上按照时间自增排序，并且整个分布式系统内不会产生ID碰撞，并且效率较高


- TableField字段注解
	
- TableLogic
	+ 逻辑删除
	

### Wrapper
- QureyWrapper
- UpdateWrapper
- condition
- LambdaQueryWrapper
- LambdaUpdateWrapper

组装查询条件，排序条件，删除条件，条件优先级，select子句，实现子查询

### 分页方法

### 乐观锁

