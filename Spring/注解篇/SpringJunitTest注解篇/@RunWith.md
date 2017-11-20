### RunWith注解

```
@RunWith(SpringJUnit4ClassRunner.class) // SpringJUnit支持，由此引入Spring-Test框架支持！ 
@SpringBootTest(classes = App.class) // 指定我们SpringBoot工程的Application启动类
@WebAppConfiguration // 由于是Web项目，Junit需要模拟ServletContext，因此我们需要给我们的测试类加上@WebAppConfiguration。 
```

或者

```
@RunWith(SpringRunner.class)
@SpringBootTest(classes=App.class)
@WebAppConfiguration
```