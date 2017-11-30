### 基于Servlet的MVC架构
> Servelt API是1998年发布的，它的核心API一直变化不大，非常稳定，也是JavaEE众多API中最成功的一个。基于Servlet API产生了众多的框架，比较大家最熟悉的Structs，Spring MVC等。和REST对比，它们有如下特征

* 它们的核心理念是servelt与handler之前的一个mapping，利用一个配置文件（spring mvc可以是annotation式的配置）来处理servlet和handler之间的mapping关系。
* Servlet具有session状态，这也方便了服务器端实现一些带状态的逻辑。但同时这样也导致了servlet实现多服务器的架构带来了困难，就必须实现复杂的负载均衡、session复制、持久化机制。
* Servlet获取客户端信息的方式更多的是通过request parameters。想想你经常通过getParamter就可以明白。


### Restful架构
> 相对于Servlet，RESTful更多的是一种架构方式的改进，它强调以下几点：

* 通过请求URL来获取信息，路径即是信息，这也是HTTP的核心理念。
* 无状态，状态转而通过应用层或者数据库层来维护。
* 通过http的POST, DELTE, PUT, GET等方式来实现数据的增删改查。而不再是借以前servlet中我们经常定义的通过方法名来区别各个方式，比如getXXXByXXX，updateXXX等等。