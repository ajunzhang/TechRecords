### 线程池Executors

> 在Java5之后，并发线程这块发生了根本的变化，最重要的莫过于新的启动、调度、管理线程的一大堆API了。在Java5以后，通过Executor来启动线程比用Thread的start()更好。在新特征中，可以很容易控制线程的启动、执行和关闭过程，还可以很容易使用线程池的特性。

> 通过java.util.concurrent.ExecutorService接口对象来执行任务，该接口对象通过工具类java.util.concurrent.Executors的静态方法来创建。Executors此包中所定义的 Executor、ExecutorService、ScheduledExecutorService、ThreadFactory 和 Callable 类的工厂和实用方法。ExecutorService提供了管理终止的方法，以及可为跟踪一个或多个异步任务执行状况而生成 Future 的方法。 可以关闭 ExecutorService，这将导致其停止接受新任务。关闭后，执行程序将最后终止，这时没有任务在执行，也没有任务在等待执行，并且无法提交新任务。

> 代码实例如下：

```
public class ExecutorsTest1 {
	public static void main(String[] args) {
		ExecutorService executor = Executors.newCachedThreadPool();
		executor.execute(new Runnable() {
			@Override
			public void run() {
				// TODO Auto-generated method stub
				System.out.println("code in executoe");
			}
		});
		executor.shutdown();
	}
}
```

> 通过工具类java.util.concurrent.Executors的静态方法来创建。Executors此包中所定义的 Executor、ExecutorService、ScheduledExecutorService、ThreadFactory 和 Callable 类的工厂和实用方法。比如，创建一个ExecutorService的实例，ExecutorService实际上是一个线程池的管理工具：
*        ExecutorService executorService = Executors.newCachedThreadPool();
*        ExecutorService executorService = Executors.newFixedThreadPool(3);
*        ExecutorService executorService = Executors.newSingleThreadExecutor();