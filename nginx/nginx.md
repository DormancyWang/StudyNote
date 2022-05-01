## nginx

1. 反向代理
	- 正向代理
	- 反向代理
		+ 客户端对代理没有感知
		+ 反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器的地址，隐藏了真实服务器ip
2. 负载均衡
	- 分摊请求
3. 动静分离
	- 动态页面和静态页面分开请求

4. 配置文件/usr/local/conf/nginx.conf
	- 全局块
	- events块 ： 配置服务器与用户网络的部分
	- http块 ： 配置最频繁的部分 http全局块和server块

1. 配置反向代理
	```
	server {
        listen       80;
        proxy_pass 	目的地址加端口
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }
	```

2. 负载均衡
	- 轮询
	- weight权重
	- ip_hash: 每个访问的ip按照hash算出一个服务器，这样每个访客固定访问一台服务器，可以解决session问题
	- fair按照后端服务器的响应 时间进行分配，响应时间短的先分配

3. 动静分离
	- 把动态请求和静态请求分开

4. 高可用
	- nginx自己本身的备份





##
1. nginx的版本
	- 开源版： http://nginx.org/
	- plus商业版： www.nginx.com
	- openresty:
	- tengine
2. configure检查依赖
3. make , make install
4. stop,quit静默关闭，会处理没有完成的内容,reload 重载配置
5. nginx是主进程和子进程工作

### 配置文件
1. worker_processes: 开启的进程数量，一般与cup核心数一样
2. events{} 每一个进程可以开起多少链接
3. Http
	- include : 引入文件 
	- defalut_type : 无法处理的类型，按照默认类型返回
	- 