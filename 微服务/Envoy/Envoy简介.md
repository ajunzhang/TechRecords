### Envoy简介

> Envoy是以C++开发的高性能代理，用于调解服务网格中所有服务的所有入站和出站流量。Envoy有许多内置功能，例如动态服务发现，负载均衡，TLS termination，HTTP/2&gRPC代理，熔断器，健康检查，基于百分比流量拆分的分段推出，故障注入和丰富的metrics。

> Envoy实现了过滤和路由、服务发现、健康检查，提供了具有弹性的负载均衡。它在安全上支持TLS，在通信方面支持gRPC.

> 概括说，Envoy提供的是服务间网络通讯的能力，包括(以下均可支持TLS)：

* HTTP／1.1
* HTTP/2
* gRPC
* TCP
* 以及网络通讯直接相关的功能：
* 服务发现：从Pilot得到服务发现信息
* 过滤
* 负载均衡
* 健康检查
* 执行路由规则(Rule): 规则来自Polit,包括路由和目的地策略
* 加密和认证: TLS certs来自 Istio-Auth
* 此外, Envoy 也吐出各种数据给Mixer:
* Metrics
* Logging
* Distribution Trace: 目前支持 Zipkin

#### Envoy和Istio的关系

总结: Envoy是Istio中负责”干活”的模块,如果将整个Istio体系比喻为一个施工队,那么 Envoy 就是最底层负责搬砖的民工，所有体力活都由Envoy完成。所有需要控制，决策，管理的功能都是其他模块来负责，然后配置给Envoy