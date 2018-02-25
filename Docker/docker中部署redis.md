
- 执行命令部署redis容器

```
docker run --name some-redis -d redis redis-server --appendonly yes
```

- 进入该容器内

```
docker exec -it **** /bin/bash
```

- 然后测试redis节点

```
root@zsj:/# docker exec -it cbf9943d5476 /bin/bash
root@cbf9943d5476:/data# redis-cli
127.0.0.1:6379> set master bc83
OK
127.0.0.1:6379> get master
"bc83"
127.0.0.1:6379> 
```

