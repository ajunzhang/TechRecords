### 通过Volume挂载host文件到pod中
----
* 首先要新建一个configMap，通过yaml文件来建configMap.如下yaml文件所示，主要是挂载data:hosts: | 的数据。

命名为001configMap.yaml
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: hosts
  namespace: cuss
data:
  hosts: |
    172.20.64.15 bigdata-test1.novalocal
    172.20.64.16 bigdata-test2.novalocal
    172.20.64.17 bigdata-test3.novalocal
    172.20.64.18 bigdata-test4.novalocal
```
----
* 然后在该yaml文件所在目录执行新建configMap命令

```
kubectl create -f configMap 001configMap.yaml
```
----

* 这个时候不能通过deploy工具新建RC了，要通过yaml文件配置来新建RC.观察这个yaml文件发现是通过volumes来挂载配置文件的。
命名为002business-rc.yaml

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: business
  namespace: cuss
  labels:
    app: business
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: business
    spec:
      containers:
      - env:
        - name: JAVA_OPTS
          value: "-Xmx1024m -Xms1024m"
        name: business
        image:  172.16.66.2/cuss/business:latest
        ports:
        - containerPort: 8080
        volumeMounts:
        - name: hosts
          mountPath: /etc/hosts
          subPath: hosts
        imagePullPolicy: Always
      volumes:
      - name: hosts
        configMap:
          name: hosts
```

* 执行002business-rc.yaml文件

```
kubectl create -f --2business-rc.yaml
```

* RC新建好之后，会自动根据副本数新建pod.进入pod容器查看一下该容器的路径/etc/hosts内容

用kubectl exec进入容器
```
 kubectl exec -it business-mjc7m sh -n cuss
```
查看/etc/hosts的配置。
```
cat /etc/hosts
```

退出容器
```
CTRL+d
```
