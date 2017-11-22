### @Bean注解

一般@Bean注解要和@Configuration配合使用。表明显示装配一个bean。
bean的Id为方法名称，也可以指定别名用@Bean(name="")。

```
@Configuration
public class Config {
	
	@Bean(name="myBeanFactoryAware")
	public MyBeanFactoryAware myBeanFactoryAware(){
		return new MyBeanFactoryAware();
	}

}
```

@Bean注解表明提供了bean定义信息