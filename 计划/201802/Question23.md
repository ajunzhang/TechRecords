
- 客户端负载均衡是通过Ribbon来实现的，通过@Loadbance注解标识。MS框架的客户端负载均衡是如何实现的？没有用到@Loadbance注解。

- 可不可以这么理解通过网关转发请求 这里是服务端负载均衡。 而在服务内部之间的请求通过Ribbon或者Fegin则是客户端负载均衡？