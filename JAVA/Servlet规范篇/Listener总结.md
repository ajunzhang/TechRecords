### Servlet中Listener总结

> 在Servlet规范中定义了多种类型的监听器，它们用于监听的事件源是三个域对象，分别为：

* ServletContext

* HttpSession

* ServletRequest

> Servlet规范针对这三个域对象上的操作，又把这多种类型的监听器划分为三种类型：

* 监听三个域对象的创建和销毁的事件监听器

* 监听三个域对象的属性(Attribute)的增加和删除的事件监听器

* 监听绑定到 HttpSession 域中的某个对象的状态的事件监听器。


#### 自定义Listener

```
/**
 * Description:Servlet3.0通过注解@WebListener声明监听器。
 * 
 * @author sjZhang
 * @date 2018年2月7日下午1:49:52
 */
@WebListener
public class MyListener implements ServletContextListener {
	private final Logger log = LoggerFactory.getLogger(MyListener.class);

	@Override
	public void contextInitialized(ServletContextEvent sce) {
		log.debug("MyListener start");
	}

	@Override
	public void contextDestroyed(ServletContextEvent sce) {
	}
}
```