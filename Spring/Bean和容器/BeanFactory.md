### BeanFactory容器
BeanFactory是面向Spring框架的，称作IOC容器。BeanFactory体系结构是典型的工厂方法模式，即什么样的工厂生产什么样的产品。BeanFactory是最基本的抽象工厂，而其他的IOC容器只不过是具体的工厂，对应着各自的Bean定义方法。但同时，其他容器也针对具体场景不同，进行了扩充，提供具体的服务。


概括起来：

>1.BeanFactory是Spring容器的Root Interface

>2.BeanFactory的作用是持有一定数量的Bean Definition，每一个都有一个独有的String名字。BeanFactory可以返回单例或多例的对象，取决于Bean定义文件。

>3.通过setters,constructors进行依赖注入更好，其实这也是常用的方法

>4.BeanFactory通过载入配置源文件(XML文件)的方式，来配置Bean。

>5.最后一大段是BeanFactory支持的bean生命周期的顺序。但是其实BeanFactory是没有给出抽象方法的。

BeanFactory作为最顶层的一个接口类，它定义了IOC容器的基本功能规范，BeanFactory 有三个子类接口：ListableBeanFactory、HierarchicalBeanFactory 和AutowireCapableBeanFactory