

- k8s的负载均衡是基于service实现的，如下图架构图所示。


 ![service](/images/k8s_1.png)


>Service有四种类型：
- ClusterIP：默认类型，自动分配一个仅cluster内部可以访问的虚拟IP
- NodePort：在ClusterIP基础上为Service在每台机器上绑定一个端口，这样就可以通过 <NodeIP>:NodePort 来访问该服务
- LoadBalancer：在NodePort的基础上，借助cloud provider创建一个外部的负载均衡器，并将请求转发到 <NodeIP>:NodePort
- ExternalName：将服务通过DNS CNAME记录方式转发到指定的域名（通过 spec.externlName 设定）。需要kube-dns版本在1.7以上。


