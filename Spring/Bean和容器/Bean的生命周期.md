### Bean的生命周期

Spring Bean的完整生命周期从创建Spring容器开始，直到最终Spring容器销毁Bean，这其中包含了一系列关键点。

初始化顺序为BeanNameAware, BeanFactoryAware, InitializingBean, BeanFactoryPostProcessor, BeanPostProcessor


```
/**
 * SpringSimpleMultiBean类实现了BeanFactoryAware, BeanNameAware, BeanFactoryPostProcessor, BeanPostProcessor, InitializingBean接口
 * 最后验证初始化顺序为:
 * BeanNameAware, BeanFactoryAware, InitializingBean, BeanFactoryPostProcessor, BeanPostProcessor
 * @author DELL
 *
 */
public class SpringSimpleMultiBean
		implements BeanFactoryAware, BeanNameAware, BeanFactoryPostProcessor, BeanPostProcessor, InitializingBean {

	private Long id;
	private String name;

	public SpringSimpleMultiBean(Long id, String name) {
		this.id = id;
		this.name = name;
	}

	public void say() {
		System.out.println("hello I am " + name);
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("InitializingBean接口的具体实现 begin and id is " + id + " and name is " + name);
	}

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("BeanPostProcessor初始化， postProcessBeforeInitialization。beanName=" + beanName);
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		System.out.println("BeanPostProcessor初始化， postProcessAfterInitialization。beanName=" + beanName);
		return bean;
	}

	@Override
	public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
		System.out.println("BeanFactoryPostProcessor初始化，beanFactory=" + beanFactory);
	}

	@Override
	public void setBeanName(String name) {
		System.out.println("BeanNameAware初始化，name=" + name);

	}

	@Override
	public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
		System.out.println("BeanFactroyAware初始化,beanFactory=" + beanFactory);

	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
```

```
/**
 * SpringSimpleMultiBeanTest类上面的注解是SpringBoot1.4采用JunitTest的写法。
 * 
 * 该测试用例主要是用来测试SpringSimpleMultiBean实现BeanFactoryAware，BeanNameAware，BeanFactoryPostProcessor,BeanPostProcessor,InitialingBean接口。
 * 初始化的先后顺序。
 * @author DELL
 *
 */
@RunWith(SpringRunner.class)
@SpringBootTest(classes=App.class)
@WebAppConfiguration
public class SpringSimpleMultiBeanTest {

	@Autowired
	private ApplicationContext applicationContext;

	@Test
	public void test() throws Exception {
		Object obj = applicationContext.getBean(SpringSimpleMultiBean.class);
		((SpringSimpleMultiBean) obj).say();

	}

}
```

```
BeanNameAware初始化，name=springSimpleMultiBean
BeanFactroyAware初始化,beanFactory=org.springframework.beans.factory.support.DefaultListableBeanFactory
InitializingBean接口的具体实现 begin and id is 1 and name is zsj
BeanFactoryPostProcessor初始化，beanFactory=org.springframework.beans.factory.support.DefaultListableBeanFactory
BeanPostProcessor初始化 postProcessBeforeInitialization。beanName=org.springframework.context.event.internalEventListenerProcessor
BeanPostProcessor初始化， postProcessAfterInitialization。beanName=org.springframework.context.event.internalEventListenerProcessor
```