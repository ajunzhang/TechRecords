### SpringBootTest注解

如果是用SpringBoot构建服务，在跑测试用例的时候要指定启动类。注意@SpringBootTest注解是Spring Boot 1.4开始启用的，之前的版本要用@SpringApplicationConfiguration

```
@RunWith(SpringRunner.class)
@SpringBootTest(classes=App.class)
@WebAppConfiguration
public class SpringSimpleMultiBeanTest{

}
```


>Spring Boot 1.3 recap

Of course, often you need to move a little further up the stack and start to write integration tests that do involve Spring. Luckily, Spring Framework has the spring-test module to help here, unluckily there a lot of different ways to use it with Spring Boot 1.3.
You might be using the @ContextConfiguration annotation in combination with SpringApplicationContextLoader:
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes=MyApp.class, loader=SpringApplicationContextLoader.class)
public class MyTest {

    // ...

}
```

或者

```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(MyApp.class)
public class MyTest {

    // ...

}
```