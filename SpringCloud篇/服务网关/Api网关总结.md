### Api服务网关

- 为何提出服务网关概念？

- 如果没有网关的概念，客户端发起请求会通过Nginx等进行路由转发到对应的服务实例上。为了保证路由的正确性，需要手动去修改维护这些路由规则和服务实例列表。当有实例增减或者IP地址变动的情况下，也需要手动去同步。显然这是很复杂和不合理的，所以需要一套机制来降低维护路由规则和服务列表的难度。


- 服务网关能做什么？

- 请求的统一入口，实现服务端的负载均衡。
- 统一做权限校验控制。
- 日记采集。


- Spring Cloud 如何实现网关？

- 基于Netflix Zuul实现API网关组件 Spring Cloud Zuul . 首先网关要注册到注册中心，这样可以保证同时获取其他微服务的实例信息。 
- 而维护路由规则 zuul则默认将通过 服务名 serviceId作为 ContextPath的方式来创建路由映射。这样的设置可以满足大部分的路由需求。
- 统一认证和校验，则是利用Zuul的一套Filter机制，我们可以利用Zuul来创建各种校验过滤器，然后指定哪些规则的请求需要执行校验逻辑，只有通过校验的请求才会路由到具体的微服务接口。


### 传统的路由方式

定义一组path和url的映射关系
```
zuul.routes.api-a-url.path=/services/**
zuul.routes.api-a-url.url=http://localhost:8080/
```
如上配置规则规则表示定义了发往API网关的请求中，所有符合/services/**规则的访问都将被路由转发到 http://localhost:8080/地址上。比如我们直接访问如下url。5555可以认为是网关暴露的端口。

```
http://localhost:5555/services/hello
```
将被API网关转发到
```
http://localhost:8080/hello
```

- 显然这种路由规则的配置是很不友好的，Zuul提出了面向服务的路由配置

### 面向服务的路由

```
zuul.routes.api-a.path=/api-a/**
zuul.routes.api-a.serviceId=a-service

zuul.routes.api-b.path=/api-b/**
zuul.routes.api-b.serviceId=b-service
```

