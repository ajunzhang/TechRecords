### servlet中的filter总结

---
> filter功能，它使用户可以改变一个 request和修改一个response. Filter 不是一个servlet,它不能产生一个response,它能够在一个request到达servlet之前预处理request,也可以在离开 servlet时处理response.换种说法,filter其实是一个”servlet chaining”(servlet 链).

> Servlet 3.0提出了注解@WebFilter。直接声明在类上面。用法简单。

```
/**
 * Description:Servlet 3.0用注解@WebFilter声明过滤器
 * @author sjZhang
 * @date 2018年2月7日下午1:59:51
 */
@WebFilter("/user/access")
public class LogFilter implements Filter {
	private final Logger log = LoggerFactory.getLogger(LogFilter.class);

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		log.debug("income log filter:" + request.getRemoteHost());
		chain.doFilter(request, response);
	}

	@Override
	public void destroy() {
	}

}
```


