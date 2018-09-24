### 显式锁ReentrantLock

> 在jdk1.5之前，协调共享对象的访问机制只有Synchronized(内置锁)和volatile。jdk1.5之后增加了一种新的机制ReentrantLock(显式锁).显式锁并不是用来替代内置锁，而是当内置锁不适用的时候，作为一种可选择的高级功能。

> 内置锁的局限？

* 无法中断一个正在等待获取锁的线程。
* 无法在请求获取一个锁时无限的等待下去。
* 内置锁必须在获取该锁的代码中释放，无法实现非阻塞结构的加锁规则。

> ReentrantLock的用法

显式锁的用法比内置锁要复杂，必须在finally中释放锁，还需要考虑抛出异常的情况对象处于的状态。

```
public class ReadWriteMap<K, V> {
    private Map<K, V> map;
    private final ReadWriteLock lock = new ReentrantReadWriteLock();
    private final Lock r = lock.readLock();
    private final Lock w = lock.writeLock();

    public ReadWriteMap(Map<K, V> map) {
        this.map = map;
    }

    /**
     * 写操作加锁
     *
     * @param key
     * @param value
     * @return
     */
    public V put(K key, V value) {
        w.lock();
        try {
            return map.put(key, value);
        } finally {
            w.unlock();
        }
    }
}
```

> 