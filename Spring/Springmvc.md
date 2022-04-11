就是Spring实现mvc来简化web开发 === Spring web    
**采用了松散耦合可插拔组件结构**，比其他 MVC 框架更具扩展性和灵活性。    

### Spring 中的mvc
DispatherServlet Spring自带的分发器，最前面的servlet  
load-on-startup标签，在服务器启动的时候创建对象，默认是访问的时候创建servlet，  
值越小优先级越高

### Spring开始的一些配置以及概念

拦截url的配置：  
	/ 拦截所有请求 包括静态资源和动态请求 但是不拦截jsp  
	/\* 拦截所有请求 包括静态资源和动态请求 也拦截jsp  
	区别就在于/ 不拦截jsp /\*拦截jsp
	
	/\*\* 的意思是所有文件夹及里面的子文件夹
	/\* 是所有文件夹，不含子文件夹

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class MyFirstController {

		// /可以省略
    @RequestMapping("/hello")
    public String myFirstRequest(){
        System.out.println("request get processing....");
		
		//规则1
        return "/WEB-INF/page/success.jsp";
    }
}
```
```xml
springmvc.xml

<!-- 配置分发处理器（前端控制器）  就他一个servlet -->
 	<servlet>
        <servlet-name>springDispatherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        
        // 配置地址 如果不指定文件位置他会去webinf下面找一个默认的配置文件
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        //配置servlet是访问的时候创建呢，还是启动的时候创建，不写就是访问创建
        <load-on-startup>1</load-on-startup>
    </servlet>

    //配置拦截url，具体看上面
    <servlet-mapping>
        <servlet-name>springDispatherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>



<!-- 配置一个视图解析器 ，能帮我们拼接页面地址-->
	
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"></property> 	 	前缀
        <property name="suffix" value=".jsp"></property>   					后缀
    </bean>

```


```xml
tomcat config目录下面的自己的xml
所有的小webxml都是继承于大xml，记不住了去看web.xml里面的注释

小webxml里面的配置会覆盖大webxml里面的配置
  <!--The default servlet for all web applications, that serves static     -->
  <!-- resources.  It processes all requests that are not mapped to other   -->
  <!-- servlets with servlet mappings (defined either here or in your own   -->
  <!-- web.xml file). -->


  <!-- The mapping for the default servlet -->
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- The mappings for the JSP servlet -->
    <servlet-mapping>
        <servlet-name>jsp</servlet-name>
        <url-pattern>*.jsp</url-pattern>
        <url-pattern>*.jspx</url-pattern>
    </servlet-mapping>
```


1. @RequestMapping注解：
	可以标在类上也可以标在方法上
其他属性
	String[] value() default {};

    RequestMethod[] method() default {};

    String[] params() default {};

    String[] headers() default {};

    String[] consumes() default {};

    String[] produces() default {};
	
	@RequestMapping注解的属性有六种，分别是value、method、produces、consumes、header、params。这六种属性是与的关系，联合使用会使请求地址的映射更加精确。

value： 指定请求的实际地址，像/action/info这样的。

method：指定请求的method类型， GET、POST、PUT、DELETE等。

produces：它的作用是指定返回内容的类型，只有当request请求头中Accept属性包含该produces指定的类型才能返回数据成功。注意：请求头中Accept代表发送端（客户端）希望接受的数据类型，比如：Accept：text/xml（application/json），代表客户端希望接受的数据类型是xml（json）类型。

consumes：它的作用是指定request请求提交的内容类型(Content-Type)，例如application/json, text/html。注意：Content-Type代表发送端（客户端|服务器）发送的实体数据的数据类型，比如：Content-Type：text/html（application/json），代表发送端发送的数据格式是html（json）。

params：指定request请求地址中必须包含某些参数值，才让该方法处理。当使用params属性时，你可以让多个处理方法处理同一个URL请求地址，只需要设置请求地址的参数不同即可。支持简单的表达式：**不可以加空格**
    ----params = "params1"：表示请求必须包含名为params1的请求参数;
    ----params = "!params1"：表示请求不能包含名为params1的请求参数;
    ----params = "params1 != value1"：表示请求必须包含名为params1的请求参数，但是其值不能是value1;
    ----params = {"params1 = value1", "param2"}：表示请求必须包含名为params1和params2两个请求参数，且params1的值必须为value1;

header：指定request请求中必须包含某些指定的请求头header的值，才能让该方法处理请求。

Ant风格的url
路径上可以有占位符{name:}在形参上面可以用
**@PathVariable("id")** 来引用

#### REST
1. Representational State Transfer
2. 网页中只能发get post 请求，利用filter转化成规定形式的请求,在表单里面加一个隐藏域 名字叫_method

```xml
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
```java
//过滤方法
public class HiddenHttpMethodFilter extends OncePerRequestFilter {
	public static final String DEFAULT_METHOD_PARAM = "_method";
	private String methodParam = "_method";
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
	        String paramValue = request.getParameter(this.methodParam);
	        if ("POST".equals(request.getMethod()) && StringUtils.hasLength(paramValue)) {
	            String method = paramValue.toUpperCase(Locale.ENGLISH);
	            HttpServletRequest wrapper = new HiddenHttpMethodFilter.HttpMethodRequestWrapper(request, method);
	            filterChain.doFilter(wrapper, response);
	        } else {
	            filterChain.doFilter(request, response);
	        }

	    }
}
```

#### SpringMVC 如何获取请求带来的各种信息
默认获取请求参数，形参名字来匹配就好

@RequestParam 获取请求参数明确指定 和上面@PathVariable一样

@RequestHeader 获取请求头中的信息

@CookieValue 获取cookie的值

对象可以自动封装

springMVC也可以在参数上面写原生的api (HttpSession 等)
Principal 安全相关
Locale 国际化相关
Request
Response
Session
InputStream  request的输入流
OutputStream response的输出流
Reader 字节流
Writer 字节流

#### 乱码的解决
1. 分类
	请求乱码:
		get:改tomcat配置文件就好
		post: 配置characterEncodingFilter
	响应乱码：setContentType("text/html;charset='utf-8'")
	
	
#### 如何将数据输出到页面
在方法出传入Map		Model	ModelMap 请求域中
BindingAwareModelMap 都是这个类传进来的

还可以有返回值ModelAndView类型,还是放在Request域中
```java
public ModelAndView handle4(){
    ModelAndView view = new ModelAndView("success");
    //view 
    view.addObject("msg","hello");
    return view;
}
```

给session保存数据
@SessionAttributes 只能标在类上
给BindingAwareModelMap 中保存数据的时候同时保存一份在session中

String[] value() default {}; 按名字保存 **用的多**
Class<?>[] types() default {}; 按类型保存
不推荐使用sessionAttributes 会引发异常，推荐使用原生的api

还有一个@ModelAttribute 现在基本不用，因为有了mybatis
```java
在处理器中使用(@ModelAttribute("book") Book book，Model model)
来获取
上下两个model是一个对象，一个类型。
@ModelAttribute
    public void myModelAttribute(Map<String ,Object> map){
        Book book = new Book();
        map.put("book",book);
        
    }
```

给BindingAwareModelMap 从页面请求的开始一致可以用到结束，叫做**隐含模型**

#### DispatherServlet的源码分析
DispatherServlet extends
FrameworkServlet extends
HttpServletBean extends
HttpServlet

```java
protected final void processRequest(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		long startTime = System.currentTimeMillis();
		Throwable failureCause = null;

		LocaleContext previousLocaleContext = LocaleContextHolder.getLocaleContext();
		LocaleContext localeContext = buildLocaleContext(request);

		RequestAttributes previousAttributes = RequestContextHolder.getRequestAttributes();
		ServletRequestAttributes requestAttributes = buildRequestAttributes(request, response, previousAttributes);

		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
		asyncManager.registerCallableInterceptor(FrameworkServlet.class.getName(), new RequestBindingInterceptor());

		initContextHolders(request, localeContext, requestAttributes);

		try {
			doService(request, response);
		}
		catch (ServletException ex) {
			failureCause = ex;
			throw ex;
		}
		catch (IOException ex) {
			failureCause = ex;
			throw ex;
		}
		catch (Throwable ex) {
			failureCause = ex;
			throw new NestedServletException("Request processing failed", ex);
		}

		finally {
			resetContextHolders(request, previousLocaleContext, previousAttributes);
			if (requestAttributes != null) {
				requestAttributes.requestCompleted();
			}

			if (logger.isDebugEnabled()) {
				if (failureCause != null) {
					this.logger.debug("Could not complete request", failureCause);
				}
				else {
					if (asyncManager.isConcurrentHandlingStarted()) {
						logger.debug("Leaving response open for concurrent processing");
					}
					else {
						this.logger.debug("Successfully completed request");
					}
				}
			}

			publishRequestHandledEvent(request, startTime, failureCause);
		}
	}
	
	
	protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
		if (logger.isDebugEnabled()) {
			String requestUri = urlPathHelper.getRequestUri(request);
			String resumed = WebAsyncUtils.getAsyncManager(request).hasConcurrentResult() ? " resumed" : "";
			logger.debug("DispatcherServlet with name '" + getServletName() + "'" + resumed +
					" processing " + request.getMethod() + " request for [" + requestUri + "]");
		}

		// Keep a snapshot of the request attributes in case of an include,
		// to be able to restore the original attributes after the include.
		Map<String, Object> attributesSnapshot = null;
		if (WebUtils.isIncludeRequest(request)) {
			logger.debug("Taking snapshot of request attributes before include");
			attributesSnapshot = new HashMap<String, Object>();
			Enumeration<?> attrNames = request.getAttributeNames();
			while (attrNames.hasMoreElements()) {
				String attrName = (String) attrNames.nextElement();
				if (this.cleanupAfterInclude || attrName.startsWith("org.springframework.web.servlet")) {
					attributesSnapshot.put(attrName, request.getAttribute(attrName));
				}
			}
		}

		// Make framework objects available to handlers and view objects.
		request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, getWebApplicationContext());
		request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
		request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
		request.setAttribute(THEME_SOURCE_ATTRIBUTE, getThemeSource());

		FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
		if (inputFlashMap != null) {
			request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
		}
		request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
		request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);

		try {
			doDispatch(request, response);
		}
		finally {
			if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
				return;
			}
			// Restore the original attribute snapshot, in case of an include.
			if (attributesSnapshot != null) {
				restoreAttributesAfterInclude(request, attributesSnapshot);
			}
		}
	}
	
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;

		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

		try {
			ModelAndView mv = null;
			Exception dispatchException = null;

			try {

				//检查是否是文件上传请求
				processedRequest = checkMultipart(request);
				multipartRequestParsed = processedRequest != request;

				// Determine handler for the current request.
				//就是找到处理的类
				mappedHandler = getHandler(processedRequest);
				if (mappedHandler == null || mappedHandler.getHandler() == null) {
					
					//抛出404
					noHandlerFound(processedRequest, response);
					return;
				}

				// Determine handler adapter for the current request.
				// 选择需要调用的方法，强大的反射对象  AnnotationMethodHandlerAdapter
				HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

				// Process last-modified header, if supported by the handler.
				String method = request.getMethod();
				boolean isGet = "GET".equals(method);
				if (isGet || "HEAD".equals(method)) {
					long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
					if (logger.isDebugEnabled()) {
						String requestUri = urlPathHelper.getRequestUri(request);
						logger.debug("Last-Modified value for [" + requestUri + "] is: " + lastModified);
					}
					if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
						return;
					}
				}

				if (!mappedHandler.applyPreHandle(processedRequest, response)) {
					return;
				}

				try {
					// Actually invoke the handler.
					//执行后的返回值放在ModelAndView中
					mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
				}
				finally {
					if (asyncManager.isConcurrentHandlingStarted()) {
						return;
					}
				}


				//设置默认的视图名字
				applyDefaultViewName(request, mv);
				mappedHandler.applyPostHandle(processedRequest, response, mv);
			}
			catch (Exception ex) {
				dispatchException = ex;
			}


			//转发到页面的方法，数据都在Request域中，所以也传入了request
			processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
		}
		catch (Exception ex) {
			triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
		}
		catch (Error err) {
			triggerAfterCompletionWithError(processedRequest, response, mappedHandler, err);
		}
		finally {
			if (asyncManager.isConcurrentHandlingStarted()) {
				// Instead of postHandle and afterCompletion
				mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
				return;
			}
			// Clean up any resources used by a multipart request.
			if (multipartRequestParsed) {
				cleanupMultipart(processedRequest);
			}
		}
	}



1. 找到处理器
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
		
			handlerMapping里面保存了支持的url地址
	有 BeanNameUrlHandlerMapping xml配置的
	   DefaultAnnotationHandlerMapping  基于注解的


		for (HandlerMapping hm : this.handlerMappings) {
			if (logger.isTraceEnabled()) {
				logger.trace(
						"Testing handler map [" + hm + "] in DispatcherServlet with name '" + getServletName() + "'");
			}
			HandlerExecutionChain handler = hm.getHandler(request);
			if (handler != null) {
				return handler;
			}
		}
		return null;
	}

2. 找到适配器
protected HandlerAdapter getHandlerAdapter(Object handler) throws ServletException {
		for (HandlerAdapter ha : this.handlerAdapters) {
			if (logger.isTraceEnabled()) {
				logger.trace("Testing handler adapter [" + ha + "]");
			}
			if (ha.supports(handler)) {
				return ha;
			}
		}
		throw new ServletException("No adapter for handler [" + handler +
				"]: The DispatcherServlet configuration needs to include a HandlerAdapter that supports this handler");
	}



```
handlerMappings
handlerAdapters 是什么时候赋值的？

Springmvc 的九大组件以及初始化
```java
	/** MultipartResolver used by this servlet */
	//多部件解析器 文件上传相关
	private MultipartResolver multipartResolver;

	/** LocaleResolver used by this servlet */
	//区域信息解析器
	private LocaleResolver localeResolver;

	/** ThemeResolver used by this servlet */
	//用来解析样式、图片及它们所形成的显示效果的集合。
	private ThemeResolver themeResolver;

	/** List of HandlerMappings used by this servlet */

	//保存Url和逻辑处理的映射关系，
	private List<HandlerMapping> handlerMappings;

	/** List of HandlerAdapters used by this servlet */
	//动态参数适配器，让固定的Servlet处理方法调用Handler来进行处理
	private List<HandlerAdapter> handlerAdapters;

	/** List of HandlerExceptionResolvers used by this servlet */
	//Handler处理异常解析器
	private List<HandlerExceptionResolver> handlerExceptionResolvers;

	/** RequestToViewNameTranslator used by this servlet */
	private RequestToViewNameTranslator viewNameTranslator;

	/** FlashMap Manager used by this servlet */
	//允许重定向携带数据
	private FlashMapManager flashMapManager;

	/** List of ViewResolvers used by this servlet */
	//视图解析器
	private List<ViewResolver> viewResolvers;
```

如何确定参数以及调用

#### 视图和视图解析器
一定要加/ 不然一般容易出问题
"forward:/url"
"redirect:/url"

#### 视图渲染
```java
//入口函数
processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);

private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {
		//异常，先不管
		boolean errorView = false;
		if (exception != null) {
			if (exception instanceof ModelAndViewDefiningException) {
				logger.debug("ModelAndViewDefiningException encountered", exception);
				mv = ((ModelAndViewDefiningException) exception).getModelAndView();}
			else {
				Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
				mv = processHandlerException(request, response, handler, exception);
				errorView = (mv != null);}}
		// Did the handler return a view to render?
		if (mv != null && !mv.wasCleared()) {
			//渲染，上面判断mv是否是空
			render(mv, request, response);
			if (errorView) {
				WebUtils.clearErrorRequestAttributes(request);}}
		else {
			if (logger.isDebugEnabled()) {
				logger.debug("Null ModelAndView returned to DispatcherServlet with name '" + getServletName() +
						"': assuming HandlerAdapter completed request handling");}}
		if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
			// Concurrent handling started during a forward
			return;}
		if (mappedHandler != null) {
			mappedHandler.triggerAfterCompletion(request, response, null);}
	}

	//渲染函数
	protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
		// Determine locale for request and apply it to the response.
		Locale locale = this.localeResolver.resolveLocale(request);
		response.setLocale(locale);
		View view;
		if (mv.isReference()) {
			// We need to resolve the view name.
			//根据视图名得到视图对象
			view = resolveViewName(mv.getViewName(), mv.getModelInternal(), locale, request);
			if (view == null) {
				throw new ServletException(
						"Could not resolve view with name '" + mv.getViewName() + "' in servlet with name '" +
								getServletName() + "'");}}
		else {
			// No need to lookup: the ModelAndView object contains the actual View object.
			view = mv.getView();
			if (view == null) {
				throw new ServletException("ModelAndView [" + mv + "] neither contains a view name nor a " +
						"View object in servlet with name '" + getServletName() + "'")}}

		// Delegate to the View object for rendering.
		if (logger.isDebugEnabled()) {
			logger.debug("Rendering view [" + view + "] in DispatcherServlet with name '" + getServletName() + "'");}
		try {
			//看最下面
			view.render(mv.getModelInternal(), request, response);}
		catch (Exception ex) {
			if (logger.isDebugEnabled()) {
				logger.debug("Error rendering view [" + view + "] in DispatcherServlet with name '"
						+ getServletName() + "'", ex);}
			throw ex;}
	}


	protected View resolveViewName(String viewName, Map<String, Object> model, Locale locale,
			HttpServletRequest request) throws Exception {
		//遍历所有的viewResolver
		for (ViewResolver viewResolver : this.viewResolvers) {
			View view = viewResolver.resolveViewName(viewName, locale);
			if (view != null) {
				return view;}}
		return null;}


	public View resolveViewName(String viewName, Locale locale) throws Exception {
		if (!isCache()) {
			return createView(viewName, locale);}
		else {
			Object cacheKey = getCacheKey(viewName, locale);
			View view = this.viewAccessCache.get(cacheKey);
			if (view == null) {
				synchronized (this.viewCreationCache) {
					view = this.viewCreationCache.get(cacheKey);
					if (view == null) {
						// Ask the subclass to create the View object.
						//创建view对象
						view = createView(viewName, locale);
						if (view == null && this.cacheUnresolved) {
							view = UNRESOLVED_VIEW;}
						if (view != null) {
							this.viewAccessCache.put(cacheKey, view);
							this.viewCreationCache.put(cacheKey, view);
							if (logger.isTraceEnabled()) {
								logger.trace("Cached view [" + cacheKey + "]");}}}}}
			return (view != UNRESOLVED_VIEW ? view : null);}}


	protected View createView(String viewName, Locale locale) throws Exception {
		// If this resolver is not supposed to handle the given view,
		// return null to pass on to the next resolver in the chain.
		if (!canHandle(viewName, locale)) {
			return null;
		}
		// Check for special "redirect:" prefix.
		if (viewName.startsWith(REDIRECT_URL_PREFIX)) {
			String redirectUrl = viewName.substring(REDIRECT_URL_PREFIX.length());
			RedirectView view = new RedirectView(redirectUrl, isRedirectContextRelative(), isRedirectHttp10Compatible());
			return applyLifecycleMethods(viewName, view);
		}
		// Check for special "forward:" prefix.
		if (viewName.startsWith(FORWARD_URL_PREFIX)) {
			String forwardUrl = viewName.substring(FORWARD_URL_PREFIX.length());
			return new InternalResourceView(forwardUrl);
		}
		// Else fall back to superclass implementation: calling loadView.
		return super.createView(viewName, locale);
	}

	@Override
	public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
		if (logger.isTraceEnabled()) {
			logger.trace("Rendering view with name '" + this.beanName + "' with model " + model +
				" and static attributes " + this.staticAttributes);
		}

		Map<String, Object> mergedModel = createMergedOutputModel(model, request, response);

		prepareResponse(request, response);
		//渲染要给页面输出的所有数据
		renderMergedOutputModel(mergedModel, request, response);
	}

	protected void renderMergedOutputModel(
			Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		// Determine which request handle to expose to the RequestDispatcher.
		//不知道啥意思，选择一个请求控制器来暴露？//todo
		HttpServletRequest requestToExpose = getRequestToExpose(request);
		
		// Expose the model object as request attributes.
		//将 隐含模型 数据取出来全部放在request域中，很重要的函数
		exposeModelAsRequestAttributes(model, requestToExpose);
		
		// Expose helpers as request attributes, if any.
		exposeHelpers(requestToExpose);
		
		// Determine the path for the request dispatcher.
		//拿到转发路径
		String dispatcherPath = prepareForRendering(requestToExpose, response);
		
		// Obtain a RequestDispatcher for the target resource (typically a JSP).
		//拿到转发器
		RequestDispatcher rd = getRequestDispatcher(requestToExpose, dispatcherPath);
		
		if (rd == null) {
			throw new ServletException("Could not get RequestDispatcher for [" + getUrl() +
					"]: Check that the corresponding file exists within your web application archive!");}
		// If already included or response already committed, perform include, else forward.
		if (useInclude(requestToExpose, response)) {
			response.setContentType(getContentType());
			if (logger.isDebugEnabled()) {
				logger.debug("Including resource [" + getUrl() + "] in InternalResourceView '" + getBeanName() + "'");}
			rd.include(requestToExpose, response);}

		else {
			// Note: The forwarded resource is supposed to determine the content type itself.
			if (logger.isDebugEnabled()) {
				logger.debug("Forwarding to resource [" + getUrl() + "] in InternalResourceView '" + getBeanName() + "'");}
			//进行转发
			rd.forward(requestToExpose, response);}}
```
得到view对象流程
1. 遍历所有配置的视图解析器来尝试解析view对象，如果不是空就直接返回了，是空就继续遍历
2. 调用render方法

视图解析器只能得到视图对象，视图对象里的方法才能调用原始的转发方法，并且把 隐含模型 里的数据处理好
视图对象才能够执行转发

视图对象：
作用是渲染数据， 