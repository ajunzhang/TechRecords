### web.xml中Filter标签的说明

在Spring2以前filter的配置是在web.xml中配置的，如下为标签说明

```
<!-- 定义Filter -->
<filter>
<!-- Filter的名字 -->
<filter-name>log</filter-name>
<!-- Filter的实现类 -->
<filter-class>lee.LogFilter</filter-class> 
<!--初始化参数-->
<init-param>
    <param-name>param</param-name>
    <param-value>PUT</param-value>
</init-param>
</filter>
<!-- 定义Filter拦截的URL地址 -->
<filter-mapping>
<!-- Filter的名字 -->
<filter-name>log</filter-name>
<!-- Filter负责拦截的URL 全部以/的请求,如果<url-pattern>/*.action </>,将会以拦截*.action的请求-->
<url-pattern>/*</url-pattern>
</filter-mapping>
```

> 自定义写的一个demoFilter配置示例。
```
<filter>
    <filter-name>demoFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
<init-param>
    <param-name>param</param-name>
    <param-value>PUT</param-value>
</init-param>
</filter>
<filter-mapping>
    <filter-name>demoFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

>然后我们也可以在自定义的filter中获取到初始化的值
```
@Override
public void init(FilterConfig cfg) throws ServletException {
    try {
        pageSize = Integer.parseInt(cfg.getInitParameter("pageSize"));
        System.out.println("ljm:pageSize:"+pageSize);
    } catch (NumberFormatException e) {
        pageSize = 10;
    }
}
```

### SpringBoot中配置filter
通过@WebFilter注解配置

```
@WebFilter(filterName="extFilter",urlPatterns="/*")
public class ExtFilter implements Filter{
	@Override
	public void destroy() {
		// TODO Auto-generated method stub
		System.out.println("销毁过滤器");
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		// TODO Auto-generated method stub
		System.out.println("过滤器执行中");
		chain.doFilter(request, response);
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub
		System.out.println("过滤器初始化");
	}
}
```