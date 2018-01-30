### SpringBoot 声明式事物

* 在入口类上添加注解<code>@EnableTransactionManagement</code>,开启注解事务管理，等同于xml配置文件中的 <tx:annotation-driven />。

* 在需要事务管理的地方加@Transactional注解。@Transactional 注解可以被应用于接口定义和接口方法、类定义和类的 public 方法上。

* @Transactional注解只能应用到 public 可见度的方法上。 如果你在 protected、private 或者 package-visible 的方法上使用 @Transactional 注解，它也不会报错， 但是这个被注解的方法将不会展示已配置的事务设置。

* Spring团队建议在具体的类（或类的方法）上使用 @Transactional 注解，而不要使用在类所要实现的任何接口上。在接口上使用 @Transactional 注解，只能当你设置了基于接口的代理时它才生效。因为注解是 不能继承 的，这就意味着如果正在使用基于类的代理时，那么事务的设置将不能被基于类的代理所识别，而且对象也将不会被事务代理所包装。

*  @Transactional 的事务开启，是基于接口的或者是基于类的代理被创建。所以在同一个类中一个方法调用另一个方法是有事务的方法，事务是不会起作用的。

* Spring使用声明式事务处理，默认情况下，如果被注解的数据库操作方法中发生了unchecked异常，所有的数据库操作将rollback；如果发生的异常是checked异常，默认情况下数据库操作还是会提交的。


#### @Transactional()参数说明

* readOnly : 该属性用于设置当前事务是否为只读事务，设置为true表示只读，false则表示可读写，默认值为false。例如：@Transactional(readOnly=true)

* rollbackFor : 该属性用于设置需要进行回滚的异常类数组，当方法中抛出指定异常数组中的异常时，则进行事务回滚。例如：

```
指定单一异常类：@Transactional(rollbackFor=RuntimeException.class)
指定多个异常类：@Transactional(rollbackFor={RuntimeException.class, Exception.class})
```


#### 什么是只读事务。
* 概念：从这一点设置的时间点开始（时间点a）到这个事务结束的过程中，其他事务所提交的数据，该事务将看不见！（查询中不会出现别人在时间点a之后提交的数据）。
> 应用场合：
* 如果你一次执行单条查询语句，则没有必要启用事务支持，数据库默认支持SQL执行期间的读一致性； 
* 如果你一次执行多条查询语句，例如统计查询，报表查询，在这种场景下，多条查询SQL必须保证整体的读一致性，否则，在前条SQL查询之后，后条SQL查询之前，数据被其他用户改变，则该次整体的统计查询将会出现读数据不一致的状态，此时，应该启用事务支持。
* 【注意是一次执行多次查询来统计某些信息，这时为了保证数据整体的一致性，要用只读事务】


#### try catch后事务不回滚解决方案
> springaop异常捕获原理：被拦截的方法需显式抛出异常，并不能经任何处理，这样aop代理才能捕获到方法的异常，才能进行回滚.

#### 解决方案1-不使用try catch
> @Transactional所在函数不进行try catch捕获，而是放到上层函数进行异常捕获。比如@Transactional放在service层，我们在service层不进行异常处理，只抛出，而在controller层进行异常捕获。

####  解决方案2-在catch中throw
> catch后再throw，显示回滚.

```
try {
    int a = 1/0;
} catch (Exception e) {
    // TODO: handle exception
    e.printStackTrace();
    throw e;
}
return userDao.insert(user);
```

####  解决方案3-手动回滚
> TransactionAspectSupport手动回滚事务：

```
try {
    int a = 1/0;
} catch (Exception e) {
    // TODO: handle exception
    e.printStackTrace();
    TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
}
return userDao.insert(user);
```