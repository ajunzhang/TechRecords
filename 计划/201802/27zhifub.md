
- 讲解一个最能体现你的能力的项目？描述你在这个项目中起的作用？

> answer: 澳门海关项目，立项的时候负责框架的搭建和开发测试环境的搭建维护。在开发中推动微服务化概念和开发流程以及负责解答组员开发的细节。负责整理文档记录常见问题和bug。负责解决本地开发的时候和服务器部署的服务进行联调。以及服务部署。中期负责系统管理相关的模块开发，以及对接客户的原有系统，对将用到的技术进行预研。后期上生产环境的时候，负责服务的增量部署，及时解决服务部署中遇到的问题，如通过Service对外暴露服务端口，通过ConfigMap加载host配置到容器中。

- 讲解Ms框架在项目中做了什么，在业务方面举例说你是如何实现一个接口的，讲一讲三层架构？

> 基于Ms框架，可以利用框架提供的一系列Starter简化业务的开发。比如pom文件引入Cache-Starter依赖则可以在代码中对查询接口使用redis缓存。这些Starter包括：

```
<modules>
    <module>smartms-spring-boot-starter-cache</module>
    <module>smartms-spring-boot-starter-dao</module>
    <module>smartms-spring-boot-starter-service</module>
    <module>smartms-spring-boot-starter-session</module>
    <module>smartms-spring-boot-starter-ui</module>
    <module>smartms-spring-boot-starter-download</module>
    <module>smartms-spring-boot-starter-upload</module>
    <module>smartms-spring-boot-starter-security</module>
    <module>smartms-spring-boot-starter-client</module>
    <module>smartms-spring-boot-starter-message</module>
    <module>smartms-spring-boot-starter-web-support</module> 
    <module>smartms-spring-boot-starter-export</module> 
    <module>smartms-spring-boot-starter-import</module> 
    <module>smartms-spring-boot-starter-solr</module>
    <module>smartms-spring-boot-starter-log</module>
    <module>smartms-spring-boot-starter-workflow</module>
    <module>smartms-spring-boot-starter-mail</module>
</modules>
```

比如 Starter-dao 集成了Mybatis。
Starter-client 集成了 Fegin。
Starter-service 封装了一些框架常用的方法，如getByIds single insert等操作。
Starter-ui 封装了一些常用的rest接口。
Starter-session 集成了redis，用户登录的时候会创建session，存储到redis中。
Starter-log 采集日志。

- java项目部署到K8S上的流程和需要注意的事项？

> answer: 基于Hub仓库进行镜像的管理，需要注意的是版本的管理，即打tag。k8s客户端kubectl基于yaml文件create pod。

- 服务与服务之间如何通信？

> answer:使用Fegin进行声明式的接口调度。内部是基于Ribbon进行请求转发的。而且 Ribbon 和 Hystrix 是在微服务架构中实现客户端负载均衡的服务调用以及用断路器来保护微服务应用。Fegin对这两者进行了封装。简化使用。

- 在K8S上服务对外怎样暴露接口，服务对内端口改变了如何进行服务调度？

- answer: 通过Service对外暴露一组pod实例的端口。对于网关gateway来说所有服务的实例端口默认都是8080.

- Tomcat和DOCKER的本质区别是什么？

> answer: Tomcat是web服务器，java应用打成war包放在tomcat上面启，就是部署应用了。DOCKER是一种轻量级的虚拟机，是单个进程启的。可以将docker类比linux系统，我们可以在docker中安装mysql redis nginx java应用 golang应用等等。所以docker的本质是platform。

- 职业规划是什么？

> answer: 职业规划工作前期是打基础，扩展技术面，选择核心技术如容器+微服务 深入学习。这个阶段希望能在一个较大的平台，技术氛围浓厚的环境工作。中期着重提升自己的竞争力，对自己进行准确的定位。充分去了解市场和行业。

- 你有什么想问我的？

> answer:
是从事框架研发还是产品研发或者还是交付型项目的开发？或者说偏重于研发还是业务方向？
我应聘的这个岗位在平时的工作中主要用到的技术栈有哪些？ 对哪方面的技能要求最高？
根据我的面试回答我的工作经验和技术是否和该岗位对口？
开发团队是否固定？
您觉得我这次面试有哪些不足？

