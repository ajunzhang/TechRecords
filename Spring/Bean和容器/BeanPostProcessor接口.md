### BeanPostProcessor接口的作用

> BeanPostProcessor，这个接口，是Spring初始化bean时对外暴露的扩展点。

BeanPostProcessor，可以在spring容器实例化bean之后，在执行bean的初始化方法前后，添加一些自己的处理逻辑。这里说的初始化方法，指的是下面两种：
1 bean实现了InitializingBean接口，对应的方法为afterPropertiesSet
2 在bean定义的时候，通过init-method设置的方法
注意：BeanPostProcessor是在spring容器加载了bean的定义文件并且实例化bean之后执行的。BeanPostProcessor的执行顺序是在BeanFactoryPostProcessor之后。


> 在smartAs框架中,ThisAwareBeanPostProcessor处理器添加了自己代理自己的逻辑。

```
/**
 * 框架注入 实现了ThisAware接口的服务对象
 *
 */
public class ThisAwareBeanPostProcessor implements BeanPostProcessor {

  @Override
  public Object postProcessBeforeInitialization(Object bean, String beanName)
      throws BeansException {
    if (bean instanceof ThisAware) {
      ((ThisAware) bean).setThis(bean);
    }
    return bean;
  }

  @Override
  public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    return bean;
  }
}
```