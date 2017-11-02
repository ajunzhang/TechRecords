### kubectl命令
 > kubectl是K8s的客户端工具，其中pod是对容器封装了一层，pod中的容器共享一组资源。K8S为每个pod分配了一个IP地址。而pod的生命周期是由RC来管理的，RC用于定义副本的数量，在MASTER内，Controller Manager进程通过RC的定义来完成pod的创建、监控、启停等操作。kubectl工具可用delete命令来完成一次性删除RC和RC控制的全部pod.

 > 在部署的时候通过deploy.exe工具直接创建RC,如果存在已有的RC直接删除RC同时也删除掉了RC所控制的全部pod,重启pod直接用kubectl delete pod <podID>命令即可，会自动重启一个pod的。

 > namespace是一个重要的概念，通过系统内部的对象分配到不同namespace中，形成逻辑上分组的不同项目、小组和用户组，便于不同的分组在共享使用整个集群的资源的同时还能被分别管理。

 * 1：查看namespace为cuss的所有rc

    kubectl get rc -n <cuss>
    或者查看所有的
    kubectl get rc --all-namespaces

 * 2: 查看namespace为cuss的所有pod
    
    kubectl get pod -n <cuss>
    或者查看所有命名空间下的pod
    kubectl get pod --all-namespaces

 * 3: 查看日志
    
    kubectl logs -f cashform-controller-2tmfh  -n cuss

* 4: 删除pod
    
    kubectl delete pod -n cuss cusweb-controller-m4qnp

* 5: 查看namespace
    
    kubectl get namespaces
