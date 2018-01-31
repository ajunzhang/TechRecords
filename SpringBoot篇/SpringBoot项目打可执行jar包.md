### SpringBoot项目打可执行jar包

> 问题背景为打可执行jar包，java -jar执行。

* pom文件添加parent标签

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.6.RELEASE</version>
    <relativePath /> <!-- lookup parent from repository -->
</parent>
```

* pom文件添加build标签

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

* maven clean install即可

* 如果要打docker镜像，新建Dockerfile文件。

* 执行 docker build -t 命令

* 通过docker run -p 对外暴露端口.可以不走网关直接访问接口。

```
docker build -t hub.skyinno.com/cuss/wsclient         ./cus-ws-103-client
docker run -i -d -e JAVA_OPTS="-Xmx512m -Xms512m" --name=wsclient -p 9669:9669 hub.skyinno.com/cuss/wsclient
```