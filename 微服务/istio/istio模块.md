### istio模块

Istio是Google、IBM和Lyft联合开源的微服务Service Mesh框架，旨在解决大量微服务的发现、连接、管理、监控以及安全等问题。

> Istio的主要特性包括：
* HTTP、gRPC和TCP网络流量的自动负载均衡
* 丰富的路由规则，细粒度的网络流量行为控制
* 流量加密、服务间认证，以及强身份声明
* 全范围（Fleet-wide）策略执行
* 深度遥测和报告


> Istio从逻辑上可以分为数据平面和控制平面：
* 数据平面主要由一系列的智能代理（Envoy）组成，管理微服务之间的网络通信
* 控制平面负责管理和配置这些智能代理，并动态执行策略

Istio架构可以如下图所示

![istio模块](/images/istio1.jpg)


* Envoy：Lyft开源的高性能代理总线，支持动态服务发现、负载均衡、TLS终止、HTTP/2和gPRC代理、健康检查、性能测量等功能。Envoy以sidecar的方式部署在相关的服务的Pod中。

* Mixer：负责访问控制、执行策略并从Envoy代理中收集遥测数据。Mixer支持灵活的插件模型，方便扩展

* Pilot：用户和Istio的接口，验证用户提供的配置和路由策略并发送给Istio组件，管理Envoy示例的生命周期

* Istio-Auth：提供服务间和终端用户的认证机制