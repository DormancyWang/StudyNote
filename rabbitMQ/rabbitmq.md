# 消息中间件

## 消息中间件中的协议
1. JMS Java Message Service
2. AMQP
	- Broker
	- Virtual host
	- Connection
	- Channel
	- Exchange
	- queue
	- Binding: Exchange 和 Queue 之间的虚拟连接，binding 中可以包含 routing key，Binding 信息被保存到 Exchange 中的查询表中，作为 Message 的分发依据。

3. MQTT   
	MQTT（Message Queuing Telemetry Transport，消息队列遥测传输）是 IBM 开发的一个即时通讯协议，目前看来算是物联网开发中比较重要的协议之一了，该协议支持所有平台，几乎可以把所有联网物品和外部连接起来，被用来当做传感器和 Actuator（比如通过Twitter让房屋联网）的通信协议，它的优点是格式简洁、占用带宽小、支持移动端通信、支持 PUSH、适用于嵌入式系统。
4. XMPP    

5. AMQP和JMS的区别
![](imgs/1.awebp)

## 主要产品
1. ActiveMQ  
	- ActiveMQ Classic
	- ActiveMQ Artemis:后者不仅支持 JMS 协议，还支持 AMQP 协议、STOMP 以及MQTT，可以说后者的玩法相当丰富。
2. RabbitMQ
	- RabbitMQ 算是 AMQP 体系下最为重要的产品了，它基于 Erlang 语言开发实现，
	![](imgs/2.awebp)

3. RocketMQ
	- 比较重要，特点很多，很专一

4. Kafka
5. ZeroMQ
	- ZeroMQ 号称最快的消息队列系统，它专门为高吞吐量/低延迟的场景开发，在金融界的应用中经常使用，偏重于实时数据通信场景

### RabbitMQ 的后台管理页面
### RabbitMQ 的其中消息收发方式
![](imgs/4.awebp)

1. 设计的一些概念
	- Publisher
	- Exchange
	- Consumer
	- Queue
	- Routes:交换机转发消息到队列的规则。

2. 分发形式    
在 RabbitMQ 中，所有的消息生产者提交的消息都会交由 Exchange 进行再分配，Exchange 会根据不同的策略将消息分发到不同的 Queue 中。    

3. 六种分发模式
