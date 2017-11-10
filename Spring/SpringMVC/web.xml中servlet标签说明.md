### SpringMVC中web.xml标签的说明

> 下面这段配置，DispatcherServlet其实就是个Servlet（它继承自HttpServlet基类），同样也需要在你web应用的web.xml配置文件下声明。你需要在web.xml文件中把你希望DispatcherServlet处理的请求映射到对应的URL上去。这就是标准的Java EE Servlet配置；下面的代码就展示了对DispatcherServlet和路径映射的声明：

```
<web-app>
    <servlet>
        <servlet-name>example</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>example</servlet-name>
        <url-pattern>/example/*</url-pattern>
    </servlet-mapping>
</web-app>
```

> 其中标签 <load-on-startup> 表示load-on-startup元素标记容器是否在启动的时候就加载这个servlet(实例化并调用其init()方法)。它的值必须是一个整数，表示servlet应该被载入的顺序。当值为0或者大于0时，表示容器在应用启动时就加载并初始化这个servlet；当值小于0或者没有指定时，则表示容器在该servlet被选择时才会去加载。正数的值越小，该servlet的优先级越高，应用启动时就越先加载。当值相同时，容器就会自己选择顺序来加载。所以，<load-on-startup>x</load-on-startup>，中x的取值1，2，3，4，5代表的是优先级，而非启动延迟时间。


> 配置DispatcherServlet表示，该工程将采用springmvc的方式。启动时也会默认在/WEB-INF目录下查找XXX-servlet.xml作为配置文件， XXX就是DispatcherServlet的名字，该文件中将配置两项重要的mvc特性：HandlerMapping,负责为DispatcherServlet这个前端控制器的请求查找Controller；ViewResolver,负责为DispatcherServlet查找ModelAndView的视图解析器。此处使用指定的配置文件spring-mvc.xml

```
<servlet>
    <servlet-name>web-context</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/config/spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>web-context</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```