### Replication Controller来启Pod
> 通过新建yaml文件来配置RC,下面这个yaml文件的配置表明了新建RC副本数量为3,且命名空间为cusstest
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
  namespace: cusstest
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

> 新建完之后查看rc

```
kubectl get rc -n cusstest
```

> 也可以用edit命令修改rc的配置如：
```
kubectl edit rc nginx -n cusstest
```

> 最好不要越过RC直接创建Pod，因为Replication Controller会通过RC管理Pod副本，实现自动创建、补足、替换、删除Pod副本，这样就能提高应用的容灾能力，减少由于节点崩溃等意外状况造成的损失。即使应用程序只有一个Pod副本，也强烈建议使用RC来定义Pod。