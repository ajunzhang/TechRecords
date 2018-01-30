### AOP监测SQL执行时间

> 通过Spring AOP机制，统计dao方法的调用时间，超过一定阈值，会打印到日志中。后面可以接入邮件系统，每天统计慢sql，了解系统的健康状况，及时优化各种潜在的风险。

#### 在pom文件引入AOP Starter 

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

#### 定义切面执行监测

```
/**
 * Description:定义一个切面，用于监测执行超过50毫秒的SQL。打印到日记中。
 * @author sjZhang
 * @date 2018年1月30日上午10:50:09
 */
@Aspect
@Component
public class DaoRTLogAspect {

    private static final Logger logger = LoggerFactory.getLogger("daoRTLog");

    /**定义一个切点，用表达式匹配哪些类切入操作。**/
    @Pointcut("execution(public * com.fiberhome.dao..*.*(..))")
    public void daoLog() {
    }

    /**环绕通知，具体逻辑实现。**/
    @Around("daoLog()")
    public Object profile(ProceedingJoinPoint pjp) throws Throwable {
        String method = pjp.getSignature().toString();
        Long _startTime = System.currentTimeMillis();
        try {
            return pjp.proceed();
        } finally {
            Long _wasteTime = System.currentTimeMillis() - _startTime;
            if (_wasteTime > 50) {
                StringBuilder sb = new StringBuilder();
                sb.append("method=").append(method).append(",wasteTime=").append(_wasteTime);
                logger.info(sb.toString());
            }
        }
    }

}
```

#### 执行效果

```
2018-01-30 10:46:21.824 DEBUG 7444 --- [nio-8080-exec-1] com.fiberhome.dao.UserDao.getUser        : ==>  Preparing: SELECT * FROM USER1 
2018-01-30 10:46:21.848 DEBUG 7444 --- [nio-8080-exec-1] com.fiberhome.dao.UserDao.getUser        : ==> Parameters: 
2018-01-30 10:46:21.893 DEBUG 7444 --- [nio-8080-exec-1] com.fiberhome.dao.UserDao.getUser        : <==      Total: 3
2018-01-30 10:46:21.921  INFO 7444 --- [nio-8080-exec-1] daoRTLog                                 : method=List com.fiberhome.dao.UserDao.getUser(),wasteTime=120
```