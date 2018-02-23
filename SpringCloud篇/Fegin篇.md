### feign

- feign出现的背景

- Ribbon 和 Hystrix 是在微服务架构中实现客户端负载均衡的服务调用以及用断路器来保护微服务应用。在实际应用中这两个的使用几乎是同时出现的，既然如此如何来封装这两个基础工具来简化开发。于是feign就出现了。

- feign基于Netfix Feign实现。除了整合了Ribbon 和 Hystrix。还提供了一种声明式的Web服务客户端定义方式。

