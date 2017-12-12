### SpringBoot集成WS-Starter

* 1 SpringBoot 提供了WS的starter,pom文件添加依赖。

```
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
```

* 2 编写xsd文件，Spring在官网中提供了规范<link>https://docs.spring.io/spring-ws/docs/2.4.1.BUILD-SNAPSHOT/reference/htmlsingle/#tutorial.xsd</link>

```
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:tns="http://www.fiberhome.com/ws"
        targetNamespace="http://www.fiberhome.com/ws" elementFormDefault="qualified">
    <xs:element name="getCountryRequest">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="name" type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
    <xs:element name="getCountryResponse">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="country" type="tns:country"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
    <xs:complexType name="country">
        <xs:sequence>
            <xs:element name="name" type="xs:string"/>
            <xs:element name="population" type="xs:int"/>
            <xs:element name="capital" type="xs:string"/>
            <xs:element name="currency" type="tns:currency"/>
        </xs:sequence>
    </xs:complexType>
    <xs:simpleType name="currency">
        <xs:restriction base="xs:string">
            <xs:enumeration value="GBP"/>
            <xs:enumeration value="EUR"/>
            <xs:enumeration value="PLN"/>
        </xs:restriction>
    </xs:simpleType>
</xs:schema>
```

* 3 通过Maven添加自动生成服务端代码的插件jaxb2-maven-plugin
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>jaxb2-maven-plugin</artifactId>
            <version>1.6</version>
            <executions>
                <execution>
                    <id>xjc</id>
                    <goals>
                        <goal>xjc</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <schemaDirectory>${project.basedir}/src/main/resources//schema</schemaDirectory>
                <outputDirectory>${project.basedir}/src/main/java</outputDirectory>
                <clearOutputDir>false</clearOutputDir>
            </configuration>
        </plugin>
    </plugins>
</build>
```
用mvn install生成服务端代码。

* 4 为了支持用Spring-ws注解(主要是@Endpoint注解)需要在继承类WsConfigurerAdapter，装配如下三个bean
```
@EnableWs
@Configuration
public class Config extends WsConfigurerAdapter {
	@Bean
	public ServletRegistrationBean messageDispatcherServlet(ApplicationContext applicationContext) {
		MessageDispatcherServlet servlet = new MessageDispatcherServlet();
		servlet.setApplicationContext(applicationContext);
		servlet.setTransformWsdlLocations(true);
		return new ServletRegistrationBean(servlet, "/ws/*");
	}

	@Bean(name = "countries")
	public DefaultWsdl11Definition defaultWsdl11Definition(XsdSchema countriesSchema) {
		DefaultWsdl11Definition wsdl11Definition = new DefaultWsdl11Definition();
		wsdl11Definition.setPortTypeName("CountriesPort");
		wsdl11Definition.setSchema(countriesSchema);
		return wsdl11Definition;
	}

	@Bean
	public XsdSchema countriesSchema() {
		return new SimpleXsdSchema(new ClassPathResource("schema/countries.xsd"));
	}

}
```

* 5 用Spring-ws注解声明服务端发布的类和方法
```
@Endpoint
public class CountryEndpoint {
	private static final String NAMESPACE_URI = "http://www.fiberhome.com/ws";

	@PayloadRoot(namespace = NAMESPACE_URI, localPart = "getCountryRequest")
	@ResponsePayload
	public GetCountryResponse getCountry(@RequestPayload GetCountryRequest request) {
		GetCountryResponse response = new GetCountryResponse();
		Country poland = new Country();
		poland.setName("Poland-" + request.getName());
		poland.setCapital("Warsaw");
		poland.setCurrency(Currency.PLN);
		poland.setPopulation(38186860);
		response.setCountry(poland);
		return response;
	}

}
```

* 6 启动SpringBoot服务
