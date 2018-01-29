### SpringBoot集成Mybatis

> SpringBoot集成Mybatis只要在pom文件添加如下即可依赖,其实就是引入Mybatis基于SpringBoot的Starter。第二个依赖表示引入了数据库连接池HikariCP。第三个依赖是指定sqlserver驱动。

```
<!--mybatis Starter  -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
<!-- HikariCP连接池 -->
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <!-- 版本号可以不用指定，Spring Boot会选用合适的版本 -->
</dependency>
<!-- sqlserver驱动 -->
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>sqljdbc4</artifactId>
    <version>4.0</version>
</dependency>
```

> 在application.properties文件中，配置DataSource信息
```
mybatis.mapper-locations=classpath*:com/fiberhome/dao/*Dao.xml
mybatis.type-aliases-package=com.fiberhome.entity

spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
spring.datasource.url=jdbc:sqlserver://10.110.*.*:1433;DatabaseName=TEST-2
spring.datasource.readOnly=false
spring.datasource.connectionTimeout=30000
spring.datasource.idleTimeout=600000
spring.datasource.maxLifetime=1800000
spring.datasource.maximumPoolSize=15
spring.datasource.username=*
spring.datasource.password=*

#日志
logging.level.com.fiberhome.dao=DEBUG
```

> 这是最简洁的SpringBoot集成Mybatis。如果不用连接池hikari，SpringBoot默认集成的是Tomcat-jdbc连接池