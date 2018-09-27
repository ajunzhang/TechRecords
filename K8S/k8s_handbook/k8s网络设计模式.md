
### Kubernetes集群内部存在三类IP，分别是：

* Node IP：宿主机的IP地址
* Pod IP：使用网络插件创建的IP（如flannel），使跨主机的Pod可以互通
* Cluster IP：虚拟IP，通过iptables规则访问服务

> 在安装node节点的时候，节点上的进程是按照flannel -> docker -> kubelet -> kube-proxy的顺序启动的，主要关注flannel的网络划分和如何与docker交互，如何通过iptables访问service。

![flannel网络通信架构图](https://jimmysong.io/kubernetes-handbook/images/flannel-networking.png)


* lo：回环网络，127.0.0.1
* eth0：NAT网络，虚拟机创建时自动分配，仅可以在几台虚拟机之间访问
* eth1：bridge网络，使用vagrant分配给虚拟机的地址，虚拟机之间和本地电脑都可以访问
* eth2：bridge网络，使用DHCP分配，用于访问互联网的网卡
* docker0：bridge网络，docker默认使用的网卡，作为该节点上所有容器的虚拟交换机
* veth295bef2@if6：veth pair，连接docker0和Pod中的容器。veth pair可以理解为使用网线连接好的两个接口，把两个端口放到两个namespace中，那么这两个namespace就能打通。
