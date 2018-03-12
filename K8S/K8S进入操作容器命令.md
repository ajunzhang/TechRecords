###

* 进入容器
```
kubectl exec -ti business-mp622 sh -n cuss
```

* 退出容器

```
CTRL+d
```

* 查看所有的configMap
```
kubectl get configmaps -n cuss
```

* 用yaml文件形式查看某个具体的configMap （Volume）
```
kubectl get configmaps hosts -o yaml -n cuss
```

* 查看pod带查询
```
kubectl get pod -n cuss|grep busin
```
