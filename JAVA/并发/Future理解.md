

- Executor框架使用Runnable作为基本的任务表示形式，有很大的局限，因为它不能返回一个值或者抛出一个checked Exception.所以提出了Callable作为更好的抽象，能返回一个值。

- Future表示一个任务的生命周期，提供了相应的方法来判断是否已经完成或者取消，以及获取任务的结果和取消任务等。

- Future规范的含义是，任务的生命周期只能前进不能后退。

- get()方法的行为取决于任务的状态，还未开始，正在运行，已完成。如果任务已完成get()则返回值或者抛出异常。如果正在运行，get()则会阻塞并直到任务完成。get()方法还可以在主线程中捕获子线程的异常。

- ExecutorService中的所有submit方法都将返回一个Future.

```
ExecutorService executorServer = Executors.newCachedThreadPool();
Future<String> future = executorServer.submit(new ChildThread());
```

- FutureTask 实际上就是一个Runnable,Future对象

```
public class FutureTask<V> implements RunnableFuture<V>
public interface RunnableFuture<V> extends Runnable, Future<V>
```

> FutureTask的使用如下：发现FutureTask构造方法里面new了一个Callable对象。

```
FutureTask<String> future = new FutureTask<>(new Callable<String>() {
    @Override
    public String call() throws Exception {
        // TODO Auto-generated method stub
        System.out.println("call function");
        return searcher.search(target);
    }
});
executor.execute(future);
```