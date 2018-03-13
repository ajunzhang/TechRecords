### ETCD

> Etcd是CoreOS基于Raft开发的分布式key-value存储，可用于服务发现、共享配置以及一致性保障（如数据库选主、分布式锁等）。

#### Etcd主要功能
- 基本的key-value存储
- 监听机制
- key的过期及续约机制，用于监控和服务发现
- 原子CAS和CAD，用于分布式锁和leader选举

#### 选举方法
-  1) 初始启动时，节点处于follower状态并被设定一个election timeout，如果在这一时间周期内没有收到来自 leader 的 heartbeat，节点将发起选举：将自己切换为candidate 之后，向集群中其它 follower 节点发送请求，询问其是否选举自己成为leader。
-  2) 当收到来自集群中过半数节点的接受投票后，节点即成为 leader，开始接收保存client 的数据并向其它的 follower 节点同步日志。如果没有达成一致，则candidate随机选择一个等待间隔（150ms ~ 300ms）再次发起投票，得到集群中半数以上follower接受的candidate将成为leader
-  3) leader节点依靠定时向 follower 发送heartbeat来保持其地位。
-  4) 任何时候如果其它 follower 在 election timeout 期间都没有收到来自 leader 的heartbeat，同样会将自己的状态切换为 candidate 并发起选举。每成功选举一次，新 leader 的任期（Term）都会比之前 leader 的任期大1。

#### Etcd，Zookeeper，Consul 比较

> Etcd 和 Zookeeper 提供的能力非常相似，都是通用的一致性元信息存储，都提供watch机制用于变更通知和分发，也都被分布式系统用来作为共享信息存储，在软件生态中所处的位置也几乎是一样的，可以互相替代的。

> 二者除了实现细节，语言，一致性协议上的区别，最大的区别在周边生态圈。Zookeeper 是apache下的，用java写的，提供rpc接口，最早从hadoop项目中孵化出来，在分布式系统中得到广泛使用（hadoop, solr, kafka, mesos 等）。Etcd 是coreos公司旗下的开源产品，比较新，以其简单好用的rest接口以及活跃的社区俘获了一批用户，在新的一些集群中得到使用（比如kubernetes）。虽然v3为了性能也改成二进制rpc接口了，但其易用性上比 Zookeeper 还是好一些。

> 而Consul 的目标则更为具体一些，Etcd 和 Zookeeper 提供的是分布式一致性存储能力，具体的业务场景需要用户自己实现，比如服务发现，比如配置变更。而Consul 则以服务发现和配置变更为主要目标，同时附带了kv存储。