# NoSQL
<p>
NoSQL databases (aka "not only SQL") are non-tabular databases and store data differently than relational tables. NoSQL databases come in a variety of types based on their data model. The main types are document, key-value, wide-column, and graph. They provide flexible schemas and scale easily with large amounts of data and high user loads.
</p>

1. 解决功能性问题：java,jsp,RDBMS,tomcat,html,linux....
2. 解决扩展性问题：spring,springmvc,mybatis
3. 解决性能问题: NoSQL,java线程，hadoop，Nginx，MQ，ElasticSearch

NoSQL = not only sql,通常只带非关系型数据库  

特点

- 不遵循sql标准
- 不支持ACID
- 远超sql性能

## Nosql使用场景

- 高并发
- 海量数据读写
- 对数据高可扩展性

## 不适用的场景

- 需要事务

## 常见nosql数据库
- memcache
- redis : 支持持久化，支持较多的数据类型
- mongodb：文档性数据库，形式更加多样
- Hbase
- Cassandra
- Neo4j: 图关系数据库

	
## 行式数据库，列式数据库

## Redis

1. 开源key-value
2. 数据类型多
3. 操作都是原子性的
4. 可以排序
5. 内存数据库
6. 周期性持久化
7. 在6的基础上实现了主从同步

...

### redis的安装

1. 下载源码
2. make
3. make install 
4. 默认放在/usr/local/bin中
5. 复制配置文件在源码包里，到外面来，该deamonize
6. 使用redis-server 配置文件 来后台启动
7. 使用redis-cli来进行链接

### 基础知识

1. 默认16个数据库，从0开始
2. 单线程+多路io复用 vs 串行 vs 多线程+锁（memcache）

### redis里的数据类型

1. 一些基本操作
	- keys
	- exists key
	- type key
	- del key
	- unlink key 根据value选择非阻塞删除
	- expire key 设置过期时间
	- ttl key 查看是否过期
	- select 切换库
	- dbsize 查看当前库有多少key
	- flushdb 清空当前库
	- flushall 清空所有库

1. String 基本类型
	- 二进制安全的，String可以包含任何数据，比如图片或者是可序列化的对象
	- value值最多可以是512M
	- set，get，append，strlen，setnx,incr,decr,incrby,decrby
	- 原子性操作：不会被线程调度机制打断的操作
	- mset,mget,msetnx,getrange,setrange,setex,getset

	- 数据结构：   
		简单的动态字符串 类似与ArrayList，capacity1m 大于1m会扩容，每次扩1m
	
2. List 基本类型
	- 单键多值
	- 简单的字符串列表，按照插入顺序排序，可以头部添加或者尾部添加
	- 底层就是一个双向链表：对两端操纵效率很高，对中间操作不行
	- lpush，rpush 加值
	- lpop，rpop 取值
	- rpoplpush
	- lrange 取值
	- lindex
	- llen
	- linsert,lrem(remove),lset
	- quickList数据结构    
		分块链表，块内是顺序存储。相对于链表，节省了部分指针的空间   
		![](imgs/QuickList.png)

3. Set 基本数据类型
	- 类似的一个列表功能，但是是可以自动排重
	- String类型的无序集合，底层是一个value为null的hash表.
	- sadd,smembers,sismember是否含有对应的值,scard返回个数
	- srem，spop,srandmember随机取n个值不会删除,smove,sinter,sunion,sdiff
	- dict 字典   
		使用hash表实现的

4. Hash
	- 键值对集合
	- String 类型的field，和value的映射表，特别适合存储对象类似于Map<String,Object>
	- 对象存储的三种方式：   
		- key - 序列化对象
		- key:value - 值
		- key - map
	- hset,hget,hmset,hexists,hkeys,hvals,hincrby,hsetnx

5. Zset集合有序集合
	- 没有重复元素，有序
	- 每一个成员都关联了一个评分score，从最低分到最高分排序集合中的成员，集合的成员是唯一的，但是评分是可重复的
	- zadd , zrange,zrangebyscore,zincrby,zrem,zcount,zrank
	- 数据结构，一个是hash表，一个是跳表

### 配置文件详解

1. Units 单位
2. include 包含
3. modules
4. network

### redis 的发布和订阅

1. 消息的通信方式，redis客户端可以订阅任意数量的频道

### redis新数据类型

1. BitMaps
	- setbit 
	- getbit,bitcount,bitop
	- 可以极大的节约空间

2. HyperLogLog
 	+ 不重复数据的个数
 	+ 传统的解决方案会随着时间增加，会占用大量的空间
 	+ HyperLogLog只需要话费12kb计算出更多的不同元素的基数
 	+ pfadd，pfcount,pfmerge,
 
3. Geographic 地理信息的缩写
 	+ 基于该类型，提供了经纬度设置，查询，范围查询，距离查询，，经纬度Hash等常见操作
 	+ geoadd,geopos,geodist,georadius,
 
### Jedis操作Redis6

### redis的事务

1. redis的事务是一个单独的隔离操作，事务中的所有命令都会序列化，按顺序地执行，事务在执行过程中，不会被其他客户端发送的命令打断    
	串联多个命令防止别的命令插队
2. multi,exec,discard,watch
3. 单独的隔离性，没有隔离级别，没有原子性
4. 利用lua脚本的原子性来实现悲观锁串行执行


### Redis的持久化操作
1. RDB(Redis DataBase)
	- 在指定的时间间隔中，将内存中的**数据集快照**写入到磁盘中 
	- 单独创建一个子进程来进行之就花，会现将数据写入到一个临时文件，等待持久化过程都结束了，再用这个临时文件来代替上次持久化号了的文件，缺点就是最后一次持久化后的数据可能会丢失，因为放在内存里面掉电就没得了
	- 写时复制技术：就是写到临时文件，然后再替换
	- 备份可以自动恢复
	- 适合大规模的数据恢复，对数据完整性和一致性要求不高的时候可以使用，节省磁盘空间，速度块
	
	- redis.conf : dump.rdb 名称，dir ./配置位置,
	- save 来启动快照保存，lastsave
	- rdbcompression启动磁盘文件压缩，需要消耗cpu，默认关闭
	
	- rdbchecksum 检查完整性在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能
	
	- 



	
2. AOF(Append Only File)
	- 以日志的形式来记录每个写操作，增量保存，跟mysql的redo日志一样
	- aof和rdb同时开启默认读取的是aof
	- 同步频率：alawys，second，no
	- Rewrite压缩
	- AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；
	AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，redis4.0版本后的重写，是指上就是把rdb 的快照，以二级制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。

3. 两个都启用

### redis主从复制

1. 读写分离
2. 容灾，快速恢复

配置主从复制
slaveof

3. 复制原理  
	+ Slave启动成功连接到master后会发送一个sync命令
	+ Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
	+ 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
	+ 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
	+ 但是只要是重新连接master,一次完全同步（全量复制)将被自动执行

### 哨兵模式(sentinel)

1. 新建sentinel.conf文件
2. 填写内容sentinel monitor mymaster ip port  
```
sentinel monitor mymaster 127.0.0.1 6379 2 
sentinel down-after-milliseconds mymaster 60000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1

就是说多少个哨兵确定访问不到了才会启动failover 程序
坏处没看懂
1. 必须要达到那个值才可以failover，也就是说如果大多数哨兵掉线，failover就没法执行？

2. no failover in the minority partition？？、？

哨兵掉线不需要解决么。。。。。这算什么问题，掉线才是问题吧。。。。
```

redis做压测可以用自带的redis-benchmark工具

哪个从机会被选举为主机呢？根据优先级别：slave-priority 

3. 由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。

### 集群操作

原来是由代理服务器来解决的


1. Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。   
2. Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

```
include /home/bigdata/redis.conf
port 6379
pidfile "/var/run/redis_6379.pid"
dbfilename "dump6379.rdb"
dir "/home/bigdata/redis_cluster"
logfile "/home/bigdata/redis_cluster/redis_err_6379.log"
cluster-enabled yes //打开集群
cluster-config-file nodes-6379.conf //节点配置文件名
cluster-node-timeout 15000 //节点失联时间，超过该时间进行主从切换

cmd
redis-cli --cluster create 127.0.0.1:7000 127.0.0.1:7001 \
127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 \
--cluster-replicas 1

```

3. slots 
	- 16384个插槽 hash slot
	- 集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。
4. 可以使用redis-cli -c 来自动跳转，可以通过{}来定义组的概念   
	```
	cluster getkeysinslot <slot><count>  返回 count 个 slot 槽中的键
	```

5. 多键操作是不被支持的 
多键的Redis事务是不被支持的。lua脚本不被支持
由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大

### redis的问题

1. 缓存穿透： 查询不存在的数据，缓存里当然没有喽，所以就直接访问数据库了，造成数据库压力大   
	- 对空值缓存：如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这		个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟
	- 设置可访问的名单（白名单）：
	使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmap里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问。
	+ 采用布隆过滤器：(布隆过滤器（Bloom Filter）是1970年由布隆提出的。它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。
	布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。)
	将所有可能存在的数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被 这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力。
	- 进行实时监控：当发现Redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务

2. 缓存击穿： key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题。   
	- 预先设置热门数据
	- 及时调整
	- 使用锁

3. 缓存雪崩：key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。   
	- 缓存雪崩与缓存击穿的区别在于这里针对很多key缓存，前者则是某一个key
	- 构建多级缓存架构
	- 使用锁或者队列
	- 设置过期标志，更新缓存
	- 将缓存失效时间分散开




