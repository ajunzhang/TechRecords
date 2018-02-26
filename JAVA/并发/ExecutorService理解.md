
- ExecutorService的生命周期有三种状态，运行 关闭 和 终止。

- ExecutorService在初始创建的时候处于运行状态。shutdown方法将执行平缓的关闭操作，（即不再接受新的任务，等待原有的任务完成）。

- ExecutorService submit()和execute()方法区别。

- execute()方法不返回值。submit()方法返回future对象，通过future.get()来获取值和取消操作，以及捕获异常。