#### Restful框架是基于RESTEasy的，使用ExceptionMapper来实现一个通用异常处理类。
> Restful框架通过ExceptionMapper处理通用异常，ExceptionMapper是provider的一个协议，它会将Java的异常映射到Response对象。所以要进行通用异常处理，我们只需要写一个类来实现ExceptionMapper接口，并把它声明为一个provider即可。

```
@Provider
public class BusinessAccessExceptionMapper
    extends AbstractExceptionMapper<BusinessAccessException> {
```

```
/**
 * 异常处理抽象父类
 * @param <E> 处理的异常类型
 */
public abstract class AbstractExceptionMapper<E extends Throwable> extends WebContentGenerator
    implements
      ExceptionMapper<E> {
```