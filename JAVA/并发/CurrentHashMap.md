

- ConcurrentHashMap使用了分段锁来保证线程安全。分段锁是一种粒度更细的加锁机制来实现更大程度的共享。通过这种机制任意数量的线程可以并发访问Map和修改Map。所以使用ConcurrentHashMap在并发环境实现更高的吞吐量，在单线程环境损失非常小的性能。

- Segment的get操作是不需要加锁的。因为volatile修饰的变量保证了线程之间的可见性.

```
/**
* The array of bins. Lazily initialized upon first insertion.
* Size is always a power of two. Accessed directly by iterators.
*/
transient volatile Node<K,V>[] table;
```


- Segment的put操作是需要加锁的，在插入时会先判断Segment里的HashEntry数组是否会超过容量(threshold),如果超过需要对数组扩容，翻一倍。然后在新的数组中重新hash，为了高效，CocurrentHashMap只会对需要扩容的单个Segment进行扩容

