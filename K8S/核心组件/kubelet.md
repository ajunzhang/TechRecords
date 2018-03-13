### kubelet

> 每个节点上都运行一个kubelet服务进程，默认监听10250端口，接收并执行master发来的指令，管理Pod及Pod中的容器。每个kubelet进程会在API Server上注册节点自身信息，定期向master节点汇报节点的资源使用情况，并通过cAdvisor监控节点和容器的资源。

#### 节点管理
> 节点管理主要是节点自注册和节点状态更新：
- Kubelet可以通过设置启动参数 --register-node 来确定是否向API Server注册自己；
- 如果Kubelet没有选择自注册模式，则需要用户自己配置Node资源信息，同时需要告知Kubelet集群上的API Server的位置；
- Kubelet在启动时通过API Server注册节点信息，并定时向API Server发送节点新消息，API Server在接收到新消息后，将信息写入etcd


#### Pod管理

- 获取pod清单

- 主要有扫描指定配置目录下的文件/etc/kubernetes/mainifests/
- 通过 API Server监听 etcd目录，同步pod清单