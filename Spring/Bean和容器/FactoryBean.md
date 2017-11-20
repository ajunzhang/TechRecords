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
