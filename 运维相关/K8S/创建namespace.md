### 创建namespace

> 新建yaml文件 000-cusstest-namespace.yaml

```
apiVersion: v1  
kind: Namespace  
metadata:  
   name: cusstest  
   labels:  
     name: cusstest
```

> 新建namespace
```
kubectl create -f 000-cusstest-namespace.yaml
```

> 查看namespace
```
kubectl get namespace
```