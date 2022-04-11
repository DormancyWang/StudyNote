- Spring是一个分层的(一站式) 轻量级开源框架 Spring的核心是控制反转（IoC）和面向切面（AOP）

- 为什么说分层一站式呢？
	- javaEE分三层开发 WEB层，业务层，持久层。在ssh整合框架中s == Struts2， s == spring，h ==Hibernate ,spring 的一站式开发就是不用struts2和hibernate，在spring中有SpringMvc可以替代Struts2，springJDBC可以替代Hibernate。等于一个spring框架可以快速开发JavaEE应用。关于轻量级就不太多说了，spring整个框架打包出来也才1M多的内存大小。spring运行中的消耗也不大。那肯定是轻量级的框架。

- spring的模块
	- Spring core
		+ 提供spring框架的基本功能。主要组件是BeanFactory。
	- spring Context
		+ Spring 上下文：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
	- Spring Web 模块
		+ Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
	- Spring MVC 框架
		+ MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。
