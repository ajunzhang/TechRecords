### 通过volume挂载配置到docker容器中

* 首先在宿主机上新建配置文件如在/opt/001hosts文件中添加配置.
```
新建文件：vi 001hosts
进入insert模式：i
调整模式：ESC
保存退出：:wq!
查看文件：cat 001hosts
```
* 打好镜像
```
docker build -t hub.skyinno.com/cuss/business         ./cus-business
```

* 运行镜像
如下命令实现的效果是通过--link连接容器scgo 并且给它取别名为 sc。 通过--name给这个容器取名为business。通过-p给容器的端口8080映射到宿主机的9982，从而可以直接通过宿主机ip+9982+url来访问该容器的接口服务。通过-v 将宿主机上配置文件目录为 /opt/hosts 挂载到容器内部 /etc/hosts,从而保证镜像在run的时候能加载这些配置，其中 -v 宿主机路径/容器路径。
```
docker run -i -d -e JAVA_OPTS="-Xmx512m -Xms512m" --link=scgo:sc --name=business -p 9982:8080 -v /opt/001hosts:/etc/hosts  hub.skyinno.com/cuss/business
```

* 进入容器查看配置
```
docker exec -it 2a6f0e007672 /bin/sh
ls
cd etc
cat hosts
```

* 退出容器
```
CTRL+D
```