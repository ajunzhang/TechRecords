### FactoryBean

以Bean结尾，表示它是一个Bean，不同于普通Bean的是：它是实现了FactoryBean<T>接口的Bean，根据该Bean的Id从BeanFactory中获取的实际上是FactoryBean的getObject()返回的对象，而不是FactoryBean本身， 如果要获取FactoryBean对象，可以在id前面加一个&符号来获取。


通过前缀引用来区分本身和factoryBean，举例说明，如果一个bean叫做myJndiObject,那么getBean("myJndiObject")将获取该bean，如果getBean("&myJndiObject")将获取FactoryBean


```
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("factory-bean.xml");  
FactoryBean factoryBean =  applicationContext.getBean("&proxyDB",FactoryBean.class);  
DBOperation db = (DBOperation)factoryBean.getObject();  
MysqlDBEntity dbEntity = new MysqlDBEntity();  
db.save(dbEntity); 
```
自定义了一个CarFactoryBean实现了FactoryBean接口

```
@Component("CarFactoryBean")
public class CarFactoryBean implements FactoryBean<Car> {

	private Logger logger = LoggerFactory.getLogger(CarFactoryBean.class);

	@Override
	public Car getObject() throws Exception {
		logger.info("实例化Car时，添加逻辑");
		return new Car("Benze", "red", 10000);
	}

	@Override
	public Class<?> getObjectType() {
		return Car.class;
	}

	@Override
	public boolean isSingleton() {
		return false;
	}
	
}
```
测试用例如下测试：
```
@Autowired
private ApplicationContext context;

@Test
public void test(){
    Object obj1 = context.getBean("CarFactoryBean");//获取的是CarFactoryBean勒种getObjectType()返回的Car实例
    Object obj2 = context.getBean(CarFactoryBean.class);
    Object obj3 = context.getBean("&CarFactoryBean");//获取CarFactoryBean实例
    try {
        Car car2 = ((CarFactoryBean)obj2).getObject();
        
        System.out.println(car2.getBrand());
        System.out.println(((CarFactoryBean)obj3).getObjectType());
        
        System.out.println(obj1.getClass());
        System.out.println(obj2.getClass());
        System.out.println(obj3.getClass());
    } catch (Exception e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
}
```
运行结果如下：
```
2017-11-22 10:17:00.559  INFO 14012 --- [           main] c.f.J.spring.CarFactoryBean              : 实例化Car时，添加逻辑
2017-11-22 10:17:00.560  INFO 14012 --- [           main] c.f.J.spring.CarFactoryBean              : 实例化Car时，添加逻辑
Benze
class com.fiberhome.JsencyptResource.entity.Car
class com.fiberhome.JsencyptResource.entity.Car
class com.fiberhome.JsencyptResource.spring.CarFactoryBean
class com.fiberhome.JsencyptResource.spring.CarFactoryBean
```