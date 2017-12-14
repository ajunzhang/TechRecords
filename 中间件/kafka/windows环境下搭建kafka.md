### windows环境下搭建kafka

* 1 下载安装zookeeper，注意下载binary，也就是编译之后的压缩文件。在conf目录下新建配置文件zoo.cfg,修改其中内容
```
dataDir=D:\\Apache\\zookeeper\\data
```
* 执行<code>D:\Apache\zookeeper-3.4.11\bin>zkServer.cmd</code>启动zookeeper服务端。
* 执行<code>D:\Apache\zookeeper-3.4.11\bin>zkCli.cmd</code>启动zookeeper客户端，默认占用端口2181

* 2 下载安装kafka，注意下载binary，src没编译执行不了。同样在config目录下修改配置文件server.properties文件
```
log.dirs=D:\\Apache\\kafka\\log
```
* 执行命令<code>D:\Apache\kafka_2.11-0.11.0.1>.\bin\windows\kafka-server-start.bat    .\config\server.properties</code>

* 执行如下命令 创建一个test1的topic
```
D:\Apache\kafka_2.11-0.11.0.1\bin\windows>kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1
```

* 执行命令<code>D:\Apache\kafka_2.11-0.11.0.1\bin\windows>kafka-console-producer.bat --broker-list localhost:9092 --topic test</code>服务端

* 执行命令<code>D:\Apache\kafka_2.11-0.11.0.1\bin\windows>kafka-console-consumer.bat --zookeeper localhost:2181 --topic test</code>客户端


#### kafka常用的topic命令
* 1 创建topic

```
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test1
```

* 2 查看topic列表命令
```
kafka-topics.bat --list --zookeeper localhost:2181
```

* 3 删除topic
```
D:\Apache\kafka_2.11-0.11.0.1\bin\windows>kafka-topics.bat --zookeeper localhost:2181 --delete --topic test1
```

* 4 查看某个topic
```
kafka-topics.bat --zookeeper localhost:2181 --describe --topic test
```