### maven-shade-plugin解决Jar包冲突

> 问题描述：在SpringBoot中引入Hbase-client1.1.2整理了要排除的jar包，然后运行良好。pom文件如下。然后整合到MS框架中启服务报错，找了报错原因发现是Hbase-client1.1.2引用了google的jar包guava12(这是一个低版本)，而MS框架在多个starter中引入了guava18及以上版本。因为guava18不向下兼容guava12导致版本冲突。

```
<dependencies>
    <!-- SpringBoot -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>1.5.4.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>1.5.4.RELEASE</version>
    </dependency>
    <!-- hbase -->
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
            <exclusion>
                <groupId>javax.servlet</groupId>
                <artifactId>javax.servlet-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

#### 解决方案

* 第一个解决方法为临时解决方法，主要思路是在排除hbase-client1.1.2的guava12版本，然后在外面引入guava12版本的依赖。pom文件如下.结果能够正确跑起来，观察依赖树发现项目中确实引入的是guava12版本。然后测试了其它涉及到guava18的地方如fegin，consul服务发现都能正确工作。但是始终有隐患，只能作为临时解决方案。

```
<!-- 因为guava版本在Hbase中是12，在MS中多处用到都是18，版本18不兼容12导致Hbase报错。所以在此处引用12版本保证Hbase正常运行 -->
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>12.0</version>
</dependency> 
<!-- hbase -->
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.2</version>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
        <exclusion>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
        </exclusion>
        <exclusion>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

* 第二个方案就是网上博客出现的用maven插件<code>maven-shade-plugin</code>用自定义的包名替换冲突所在jar包的包名，然后重新编译打一个jar包，再去引用这个新基础依赖jar包。

> 我的思路是重新编译hbase-client1.1.2jar包，步骤如下：

* 新建一个maven项目cus-hbase-client,主要是pom文件的配置.首先是在dependency中将要重新编译的jar包引入进来，然后在build标签中引入插件<code>maven-shade-plugin</code>。基本都是模板代码，要修改的地方就是包名替换的位置。

```
<groupId>com.fiberhome</groupId>
<artifactId>cus-hbase-client</artifactId>
<version>1.0.0</version>
<packaging>jar</packaging>

<name>cus-hbase-client</name>
<url>http://maven.apache.org</url>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <java.version>1.8</java.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
            <exclusion>
                <groupId>javax.servlet</groupId>
                <artifactId>javax.servlet-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.4.1</version>
            <configuration>
                <createDependencyReducedPom>false</createDependencyReducedPom>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <relocations>
                            <relocation>
                                <pattern>com.google.guava</pattern>
                                <shadedPattern>com.fiberhome.guava</shadedPattern>
                            </relocation>
                            <relocation>
                                <pattern>org.joda</pattern>
                                <shadedPattern>com.fiberhome.joda</shadedPattern>
                            </relocation>
                            <relocation>
                                <pattern>com.google.common</pattern>
                                <shadedPattern>com.fiberhome.common</shadedPattern>
                            </relocation>
                            <relocation>
                                <pattern>com.google.thirdparty</pattern>
                                <shadedPattern>com.fiberhome.thirdparty</shadedPattern>
                            </relocation>
                        </relocations>
                        <transformers>
                            <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer" />
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

<repositories>
    <repository>
        <id>elasticsearch-releases</id>
        <url>http://maven.elasticsearch.org/releases</url>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>daily</updatePolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

* 运行mvn clean install 去本地仓库找cus-hbase-client1.0.0.jar包是否在文件夹内。找到之后测试我们重新编译的这个jar包能否工作。

* 首先在SpringBoot项目中测试，新建一个maven项目，pom文件为.注意要排除hbase-client1.1.2至于为什么会存在其中暂时还未搞明白，可能是maven在重新编译的时候哪里没有配置好吧。然后运行测试，发现没问题可以正常访问hbase数据库。

```
<dependencies>
    <!-- SpringBoot -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>1.5.4.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>1.5.4.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>com.fiberhome</groupId>
        <artifactId>cus-hbase-client</artifactId>
        <version>1.0.0</version>
        <exclusions>
            <exclusion>
                <groupId>org.apache.hbase</groupId>
                <artifactId>hbase-client</artifactId>
                <version>1.1.2</version>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

* 然后把这个重新编译之后的cus-hbase-client1.0.0.jar包整合到MS框架中去，pom文件类似如上，启动报错，无法创建consulConfig Bean.暂时还未能找出原因。(待续)


