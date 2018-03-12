### K8s新建service

> 前提：ccv3wsserver服务需要一个指定端口对外暴露wsdl文件。部署到k8s云平台上时，所以需要新建一个service做这个操作。

* 创建yaml文件

```
vi ccv3wsserver.yaml

apiVersion: v1
kind: Service
metadata:
  name: ccv3wsserver
  namespace: cuss
spec:
  ports:
  - port: 9980
    name: ccv3wsserver
    targetPort: 8088
    protocol: TCP
  selector:
    app: ccv3wsserver
  type: NodePort
```

* 创建service

```
kubectl create -f ccv3wsserver.yaml
```

* 创建rc和pod
```
deploy.exe rc  ccv3wsserver 172.16.66.2/cuss/ccv3wsserver:latest 9980
```


删除service

```
kubectl delete svc ccv3wsserver -n cuss
```

编辑service对应的yaml文件

```
kubectl edit svc ccv3wsserver -n cuss
```

