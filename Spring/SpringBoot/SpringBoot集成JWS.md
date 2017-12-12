### SpringBoot集成JWS
项目有需求需要在SpringBoot中集成jws，将jws服务部署在SpringBoot中。目的是发布一个wsdl文件，通过浏览器可以查看wsdl文件，通过jdk自带的wsimport.exe能生成客户端代码。

#### 参考Spring实战第15章

* 1 创建基于Spring的JAX-WS端点
Spring提供了一个JAX-WS服务导出器<code>SimpleJaxWsServiceExporter</code>，该导出器要求JAX-WS运行时支持将端点发布到指定地址上。这里只能用JDK1.6自带的JAX-WS实现才符合要求。其他的JAX-WS实现可能并不能满足要求。

* 2 Spring自动装配JAX-WS端点bean
我们指定JDK自带的JWS实现用两个注解@WebService声明为Web服务的端点 和@WebMethod来声明为Web服务的操作。但是在应用中JAX-WS端点需要装配到IOC容器中去，Spring提供了类<code>SpringBeanAutowiringSupport</code>,标记了@WebService的类继承该类之后，可以用@Autowired将该类注入到业务类中去。
tips:但是这个不常用，当对象的生命周期不是由Spring管理的，而对象的属性又需要注入Spring所管理的bean时，<code>SpringBeanAutowiringSupport</code>是很有用的。常用的方式如下描述所示。

* 3 导出独立的JAX-WS端点
Spring 提供的JAX-WS服务导出器<code>SimpleJaxWsServiceExporter</code>，会将使用@WebService标记的所有bean发布为JAX-WS服务。使用的方式为：
这里调整了基本地址，如果不调整的话默认就是8080
```
@Bean
public SimpleJaxWsServiceExporter simpleJaxWsServiceExporter(){
    SimpleJaxWsServiceExporter sJaxWs = new SimpleJaxWsServiceExporter();
    sJaxWs.setBaseAddress("http://localhost:8088/ccv3/");
    return sJaxWs;
}
```

然后就和用Spring 平常的bean一样可以注入到业务类中。

* 4 浏览器访问wsdl文件
<code>http://localhost:8088/ccv3/RmsAgent?wsdl</code>

* 5 基于JDK自带的msimport.exe生成客户端代码
新建一个文件夹，进入cmd模式，输入命令
<code>wsimport -s . http://localhost:8088/ccv3/RmsAgent?wsdl</code>

在Spring中实现客户端代理JAX-WS服务待续。。。