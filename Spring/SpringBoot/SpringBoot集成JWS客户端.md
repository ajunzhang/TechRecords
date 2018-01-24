### SpringBoot集成WS-starter客户端

* 1 首先会获取一个wsdl的URL路径，因为服务端是用JDK自带的JWS实现，所以客户端根据wsdl文件代码的生成是基于wsimport.exe.该文件在路径<code>E:\Java\jdk1.8.0_66\bin</code>下面。新建一个文件夹，进入cmd模式，输入命令

```
wsimport -s . http://localhost:8088/ccv3/RmsAgent?wsdl
```
生成指定编码格式utf-8
```
wsimport -encoding utf-8 -s . http://10.110.200.104:9980/ccv3/RmsAgent?wsdl
```
会生成客户端代码，包括pojo,service Interface,service impl.

* 2 如果客户端要用Spring框架实现，就要借助一个重要的类<code>JaxWsProxyFactoryBean</code>,使用该类可以在Spring中装配Spring web服务，它是一个工厂 bean 能生成一个知道如何和SOAP Web服务交互的代理。其中set的这些属性都能从wsdl文件中找到，注意一下setNamespaceUri()方法对应的属性为targetNamespace，其他的属性很明显可以找到。

```
@Bean
public JaxWsPortProxyFactoryBean rmsClient(){
    JaxWsPortProxyFactoryBean proxy = new JaxWsPortProxyFactoryBean();
    try {
        proxy.setWsdlDocumentUrl(new URL("http://localhost:8088/ccv3/RmsAgent?wsdl"));
        proxy.setServiceName("RmsAgentServiceMockImplService");
        proxy.setPortName("RmsAgentServiceMockImplPort");
        proxy.setServiceInterface(IRmsAgentService.class);
        proxy.setNamespaceUri("http://mock.fiberhome.com/");
    } catch (MalformedURLException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
    return proxy;
    
}
```

* 3 如果我们要测试下是否能正常调用服务端的方法，用restful对外暴露一个接口。IRmsAgentService是根据wsdl生成的一个接口。客户端要做的就是调用服务端的实现方法，至于客户端必要的接口方法都是通过自动生成代码生成的。
```
@RestController
public class RmsController {

	@Autowired
	private IRmsAgentService service;

	@RequestMapping(value = "/test")
	public Object test() {
		RequestValue requestValue = new RequestValue();
		requestValue.setMovementId("aaa");
		ResultValue value=new ResultValue();
		try {
			value = service.checkPermit(requestValue);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return value;
	}

}
```

* 4 springBoot工程启动可能要指定一个端口,实现接口<code>EmbeddedServletContainerCustomizer</code>，重写方法设置下端口即可。
```
@SpringBootApplication
public class App implements EmbeddedServletContainerCustomizer{
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}

	@Override
	public void customize(ConfigurableEmbeddedServletContainer container) {
		// TODO Auto-generated method stub
		container.setPort(8081);
	}
}
```

tips:-Ddebug调式时打印信息
