### SpringBoot日志管理

> SprinigBoot日志管理主要是在application.properties文件中配置日志配置。

* 这条配置是打印包路径为com.fiberhome.dao执行的SQL语句
```
#日志
logging.level.com.fiberhome.dao=DEBUG
```

* 日志写到相对路径
```
#文件输出 相对路径存储
logging.file=my.log
```

* 日志写到文件夹路径

```
#文件输出 硬盘存储
#logging.path=/var/log
logging.path=D://SpringBootLog//Spring-Summary-tx
```