### SpringBoot引入ws-starter依赖客户端的实现

* 1 修改pom文件,添加对应依赖主要是ws-starter,以及添加插件jaxb2生成代码。
```
<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
			<version>1.5.4.RELEASE</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<version>1.5.4.RELEASE</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web-services -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web-services</artifactId>
			<version>1.5.4.RELEASE</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/wsdl4j/wsdl4j -->
		<dependency>
			<groupId>wsdl4j</groupId>
			<artifactId>wsdl4j</artifactId>
			<version>1.6.2</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.jvnet.jaxb2.maven2</groupId>
				<artifactId>maven-jaxb2-plugin</artifactId>
				<version>0.12.3</version>
				<executions>
					<execution>
						<goals>
							<goal>generate</goal>
						</goals>
					</execution>
				</executions>
				<configuration>
					<schemaLanguage>WSDL</schemaLanguage>
					<generatePackage>com.fiberhome.ws</generatePackage>
					<generateDirectory>${basedir}/src/main/java</generateDirectory>
					<schemas>
						<schema>
							<fileset>
								<!-- Defaults to schemaDirectory. -->
								<directory>${basedir}/src/main/resources/schemas</directory>
								<!-- Defaults to schemaIncludes. -->
								<includes>
									<include>*.wsdl</include>
								</includes>
								<!-- Defaults to schemaIncludes -->
								<!--<excludes> -->
								<!--<exclude>*.xs</exclude> -->
								<!--</excludes> -->
							</fileset>
							<!--<url>http://localhost:8080/ws/countries.wsdl</url> -->
						</schema>
					</schemas>
				</configuration>
			</plugin>
		</plugins>
	</build>
```

* 2 生成客户端代码
执行<code>mvn install</code>命令插件jaxb2生成客户端代码。

* 3 装配需要的bean

```
@Configuration
public class WsConfig {
	@Bean
	public Jaxb2Marshaller marshaller() {
		Jaxb2Marshaller marshaller = new Jaxb2Marshaller();
		marshaller.setContextPath("com.fiberhome.ws");
		return marshaller;
	}

	@Bean
	public WsClient wsClient(Jaxb2Marshaller marshaller) {
		WsClient client = new WsClient();
		client.setDefaultUri("http://localhost:8080/ws/countries.wsdl");
		client.setMarshaller(marshaller);
		client.setUnmarshaller(marshaller);
		return client;
	}

}
```

* 4 写客户端调用服务端的方法,要继承类<code>WebServiceGatewaySupport</code>
```
public class WsClient extends WebServiceGatewaySupport {
	public GetCountryResponse getCountry(String name) {
		GetCountryRequest request = new GetCountryRequest();
		request.setName(name);
		GetCountryResponse response = (GetCountryResponse) getWebServiceTemplate()
				.marshalSendAndReceive("http://localhost:8080/ws/countries.wsdl", request);
		return response;
	}

}
```

* 5 可以测试了，即可以用Junit也可以对外暴露接口在浏览器测试接口。
```
@RestController
public class IndexController {
	@Autowired
	private WsClient wsClient;

	@RequestMapping("callws")
	public Object callWs() {
		GetCountryResponse response = wsClient.getCountry("hello");
		return response.getCountry();
	}
}
```

* 6 上述内容可以在Spring官网如下路径找到内容。
```
https://docs.spring.io/spring-ws/docs/2.4.1.BUILD-SNAPSHOT/reference/htmlsingle/#overview
```
