### Mybatis与数据库进行交互，持久化层框架（sql映射框架）
1. 原始的工具  
JDBC-dbutils-jdbcTemplate
2. 框架要考虑的问题
	缓存，异常处理，部分字段的问题
3. Hibernate(orm框架)
	封装程度太高，反模式

4. 需求，可以定制sql，并且不许要硬编码到类中
mybatis：
	写sql在配置文件中。其他还是由mybatis自动执行
	mybatis底层就是对原生jdbc的一个封装