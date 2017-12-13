### Bean的装配过程
>教科书上总结为：
#### 一、Spring装配Bean的过程
* 1. 实例化;
* 2. 设置属性值;
* 3. 如果实现了BeanNameAware接口,调用setBeanName设置Bean的ID或者Name;
* 4. 如果实现BeanFactoryAware接口,调用setBeanFactory 设置BeanFactory;
* 5. 如果实现ApplicationContextAware,调用setApplicationContext设置ApplicationContext
* 6. 调用BeanPostProcessor的预先初始化方法;
* 7. 调用InitializingBean的afterPropertiesSet()方法;
* 8. 调用定制init-method方法；
* 9. 调用BeanPostProcessor的后初始化方法;

#### Spring容器关闭过程
* 1. 调用DisposableBean的destroy();
* 2. 调用定制的destroy-method方法;


#### Spring容器中的Bean几种初始化方法和销毁方法的先后顺序
Spring 容器中的 Bean 是有生命周期的，Spring 允许 Bean 在初始化完成后以及销毁前执行特定的操作。下面是常用的三种指定特定操作的方法：
* 通过实现InitializingBean/DisposableBean 接口来定制初始化之后/销毁之前的操作方法；
* 通过<bean> 元素的 init-method/destroy-method属性指定初始化之后 /销毁之前调用的操作方法；
* 在指定方法上加上@PostConstruct或@PreDestroy注解来制定该方法是在初始化之后还是销毁之前调用。
这几种配置方式，执行顺序是怎样的呢？示例如下：

定义bean
```
/**
 * Description:测试三种Bean的初始化方法和销毁方法
 * 
 * @author sjZhang
 * @date 2017年12月13日下午2:18:00
 */
public class BikeBean implements InitializingBean, DisposableBean {

	public void init() {
		System.out.println("=== BikeBean ===init ====");
	}

	public void des() {
		System.out.println("===== BikeBean ===destroy ====");
	}

	@Override
	public void destroy() throws Exception {
		System.out.println("容器关闭的时候调用BikeBean销毁方法");
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("实例化之后调用BikeBean初始化方法");
	}

	@PostConstruct
	public void initial() {
		System.out.println("......initial......");
	}

	@PreDestroy
	public void close() {
		System.out.println("......close.........");
	}

}
```
装配bean
```
@Configuration
public class BikeConfig {
	@Bean(initMethod = "init", destroyMethod = "des")
	public BikeBean bikeBean() {
		return new BikeBean();
	}

}
```

测试
```
/**
 * Description:
 * 
 * @author sjZhang
 * @date 2017年12月13日下午2:27:07
 */
@RunWith(SpringRunner.class)
@SpringBootTest(classes = App.class)
@ContextConfiguration(classes = BikeConfig.class)
public class BikeBeanTest {

	@Test
	public void test1() {
		AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(BikeConfig.class);
		Object obj = context.getBean("bikeBean");
		context.close();
	}

}
```

运行结果如下：
```
......initial......
实例化之后调用BikeBean初始化方法
=== BikeBean ===init ====

......close.........
容器关闭的时候调用BikeBean销毁方法
===== BikeBean ===destroy ====
```

可见先后顺序为
Bean在实例化的过程中：Constructor > @PostConstruct >InitializingBean > init-method
Bean在销毁的过程中：@PreDestroy > DisposableBean > destroy-method