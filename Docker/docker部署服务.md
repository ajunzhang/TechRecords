### docker部署服务
> 本地服务打docker镜像

* 0: 在linux中启动docker
> systemctl start docker

* 1：基于maven打jar包
> mvn clear install

* 2：命令模式连接docker环境
> set DOCKER_HOST=tcp://10.110.200.104:2375

* 3：打docker镜像
> docker build -t hub.skyinno.com/cuss/distressedgoods         ./cus-distressedgoods

* 4: 运行镜像以Container的形式在docker中运行
> docker run -i -d -e JAVA_OPTS="-Xmx512m -Xms512m" --link=scgo:sc --name=distressedgoods  hub.skyinno.com/cuss/distressedgoods

* 5: 推送镜像到HUB仓库中去，如果有私有仓库就推送到私有仓库。登录hub账号
> docker login -p Shijun20094142 -u ajunzhang hub.skyinno.com/cuss

* 6：推送镜像到HUB仓库
> docker push hub.skyinno.com/cuss/distressedgoods

* 7：或者直接从HUB仓库pull镜像
> docker pull hub.skyinno.com/cuss/distressedgoods:latest

* 8：查看镜像如果带条件查看
> docker images 或者 docker images | grep distressedgoods

* 9: 删除镜像，首先要stop掉对应容器。
> docker rmi -f <imagesId>

* 10: 删除none镜像
> docker rmi $(docker images -qf dangling=true)

* 11: 停掉容器
> docker stop <containId | containName>

* 12: 删除容器 -f 是强制删除
> docker rm -f <containId | containName>

* 13：重启容器
> docker restart <containId | containName>

* 14：查看容器日志
> docker logs -f <containId | containName>

* 14: 日志很多查看最近100条
> docker logs -f --tail=100 <containId | containName>

* 15: 查看容器信息
> docker inspect <containId | containName>

* 16：查看容器内存消耗情况
> docker stats

* 17: 给镜像打tag
> docker tag hub.skyinno.com/cuss/identification hub.skyinno.com/cuss/identification:v1.2
或者
> docker tag <imageID> hub.skyinno.com/cuss/identification:v1.2

* 18: 进入容器
> docker exec -t -i <containId> /bin/bash
> 退出时，使用[ctrl + D]，这样会结束docker当前线程，容器结束，可以使用[ctrl + P][ctrl + Q]退出而不终止容器运行

* 19：用docker Save 命令保存镜像的tar文件
> docker save 172.16.66.2/cuss/ selflogin:latest> selflogin.tar

* 20：用docker load命令从tar文件恢复成docker镜像
> docker load< selflogin.tar

* 21：查看所有容器
> docker ps -a | grep disstressedgoods