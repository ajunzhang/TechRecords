### 理解Spring装配

> Spring框架的核心是Spring容器，(spring有两个核心接口：BeanFactory和ApplicationContext，其中ApplicationContext是BeanFactory的子接口。他们都可代表Spring容器，Spring容器是生成Bean实例的工厂，并且管理容器中的Bean).容器负责管理应用中组件的生命周期，它会创建这些组件并保证它们的依赖能够得到满足，这样组件才能完成预定的任务。

> 在Spring中，对象无需自己查找或创建与其关联的其他对象。相反容器会负责把需要互相协作的对象引用赋给各个对象。创建对象与对象之间协作关系的行为称为装配(wiring)，这也是依赖注入(DI)的本质。如下是Spring配置的三种方案：

* 1. 隐式的Bean发现机制和自动装配

> Spring从两个角度来实现自动化装配：

- 组件扫描：Spring会自动发现应用上下文中所创建的Bean

> @Component注解表明标记的类会作为组件(Component)类，并且告知Spring要为这个类创建Bean。

> 不过，组件扫描默认是不启用的，我们要显示配置一下Spring命令其寻找带有@Component注解的类，然后为这些类创建Bean。

> 有两种方式启用ComponentScan

    @Configuration
    @ComponentScan("aaa/bbb")
    public class A{
        
    }
    
或者

    <context:component-scan base-package="aaa/bbb"/>

- 自动装配：Spring自动满足Bean直接的依赖

> 自动装配就是让Spring自动满足Bean依赖的一种方法，在满足依赖的过程中，会在Spring应用上下文中寻找匹配某个Bean需求的其他Bean。我们使用Spring的@Autowired注解声明要进行自动装配。

* 2. 在Java中进行显示配置

> 创建JavaConfig类的关键在于为其添加@Configuration注解，该注解表明这个类是一个配置类。

> 在JavaConfig中声明Bean，编写一个方法，为这个方法添加@Bean注解。@Bean注解会告诉Spring这个方法将会返回一个对象，改对象要注册为Spring应用上下文中的Bean，而方法体则包含最终产生Bean实例的逻辑。

    @Bean
    public AAA functionA(BBB bbb){
        return new AAA(bbb);
    }

> 在这个方法中，请求了一个BBB类型的实例作为参数，当Spring调用functionA这个方法去创建AAA类型的Bean时，会自动装配一个BBB类型的Bean。当然前提是在容器中能找到BBB类型的Bean。

> 默认情况下，Spring中的bean都是单例的。而且bean的name默认是方法名称。可以通过@Bean(name="***")自定义名称。

* 3. 在XML中进行显示配置

> 要在基于XML的Spring配置汇总声明一个bean，要使用Spring-beans模式的一个元素:<bean>

    <bean id="classa" class="aaa.classA"/>