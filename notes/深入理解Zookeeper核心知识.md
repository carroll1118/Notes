@[toc]
# 推荐阅读
* [什么是分布式协调技术？](https://blog.csdn.net/qq_40722827/article/details/102993621)
* [什么是分布式锁？](https://blog.csdn.net/qq_40722827/article/details/102993655)
* [Zookeeper如何实现分布式锁？](https://blog.csdn.net/qq_40722827/article/details/102995302)
* [Linux环境下搭建zookeeper 环境](https://blog.csdn.net/qq_40722827/article/details/102540595)
* [ZooKeeper面试题（2020最新版）](https://blog.csdn.net/ThinkWon/article/details/104397719)
---------------
# 什么是ZooKeeper?
> ZooKeeper 是一种分布式协调服务，用于管理大型主机。在分布式环境中协调和管理服务是一个复杂的过程。ZooKeeper 通过其简单的架构和 API 解决了这个问题。ZooKeeper 允许开发人员专注于核心应用程序逻辑，而不必担心应用程序的分布式特性。
### ZooKeeper提供的常见服务
* 命名服务 - 按名称标识集群中的节点。它类似于DNS，但仅对于节点。
* 配置管理 - 加入节点的最近的和最新的系统配置信息。
* 集群管理 - 实时地在集群和节点状态中加入/离开节点。
* 选举算法 - 选举一个节点作为协调目的的leader。
* 锁定和同步服务 - 在修改数据的同时锁定数据。此机制可帮助你在连接其他分布式应用程序（如Apache HBase）时进行自动故障恢复。
* 高度可靠的数据注册表 - 即使在一个或几个节点关闭时也可以获得数据。
### Zookeeper 的数据模型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329200050260.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
* 它很像数据结构当中的树，也很像文件系统的目录。
* 树是由节点所组成，Zookeeper 的数据存储也同样是基于节点，这种节点叫做 Znode。但是，**不同于树的节点，Znode 的引用方式是路径引用，类似于文件路径**。这样的层级结构，让每一个 Znode 节点拥有**唯一**的路径，就像命名空间一样对不同信息作出清晰的隔离。
### Znode 包含哪些元素
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329200347208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
* data：Znode 存储的数据信息。
* ACL：记录 Znode 的访问权限，即哪些人或哪些 IP 可以访问本节点。
* stat：包含 Znode 的各种元数据，比如事务 ID、版本号、时间戳、大小等等。
* child：当前节点的子节点引用

**注意：Zookeeper 是为读多写少的场景所设计。Znode 并不是用来存储大规模业务数据，而是用于存储少量的状态和配置信息，每个节点的数据最大不能超过 1MB。**
### Znode的类型
* Znode被分为持久（persistent）节点，顺序（sequential）节点和临时（ephemeral）节点。
	* 持久节点  - 即使在创建该特定znode的客户端断开连接后，持久节点仍然存在。默认情况下，除非另有说明，否则所有znode都是持久的。
	* 临时节点 - 客户端活跃时，临时节点就是有效的。当客户端与ZooKeeper集合断开连接时，临时节点会自动删除。因此，只有临时节点不允许有子节点。如果临时节点被删除，则下一个合适的节点将填充其位置。临时节点在leader选举中起着重要作用。
	* 顺序节点 - 顺序节点可以是持久的或临时的。当一个新的znode被创建为一个顺序节点时，ZooKeeper通过将10位的序列号附加到原始名称来设置znode的路径。例如，如果将具有路径 /myapp 的znode创建为顺序节点，则ZooKeeper会将路径更改为 /myapp0000000001 ，并将下一个序列号设置为0000000002。如果两个顺序节点是同时创建的，那么ZooKeeper不会对每个znode使用相同的数字。顺序节点在锁定和同步中起重要作用。

# ZooKeeper的架构
*  ZooKeeper的“客户端-服务器架构”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329202044309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)
* 每个组件的描述

|部分	|描述|
----------|-----------
Client	|客户端，我们的分布式应用集群中的一个节点，从服务器访问信息。对于特定的时间间隔，每个客户端向服务器发送消息以使服务器知道客户端是活跃的。类似地，当客户端连接时，服务器发送确认码。如果连接的服务器没有响应，客户端会自动将消息重定向到另一个服务器。
Server |	服务器，我们的ZooKeeper总体中的一个节点，为客户端提供所有的服务。向客户端发送确认码以告知服务器是活跃的。
Ensemble|	ZooKeeper服务器组。形成ensemble所需的最小节点数为3。
Leader	|服务器节点，如果任何连接的节点失败，则执行自动恢复。Leader在服务启动时被选举。
Follower|	跟随leader指令的服务器节点。
# Zookeeper 的基本操作
* 创建节点   `create`
* 删除节点	```delete```
* 判断节点是否存在`exists`
* 获得一个节点的数据`getData`
* 设置一个节点的数据`setData`
* 获取节点下的所有子节点`getChildren`

**exists，getData，getChildren 属于读操作。**

* Zookeeper 客户端在请求读操作的时候，可以选择是否设置 Watch。
### Watches（监视）
* 监视是一种简单的机制，使客户端收到关于ZooKeeper集合中的更改的通知。客户端可以在读取特定znode时设置Watches。Watches会向注册的客户端发送任何znode（客户端注册表）更改的通知。
* Znode更改是与znode相关的数据的修改或znode的子项中的更改。只触发一次watches。如果客户端想要再次通知，则必须通过另一个读取操作来完成。当连接会话过期时，客户端将与服务器断开连接，相关的watches也将被删除。
### Sessions（会话）
* 会话对于ZooKeeper的操作非常重要。会话中的请求按FIFO顺序执行。一旦客户端连接到服务器，将建立会话并向客户端分配会话ID 。
* 客户端以特定的时间间隔发送心跳以保持会话有效。如果ZooKeeper集合在超过服务器开启时指定的期间（会话超时）都没有从客户端接收到心跳，则它会判定客户端死机。
* 会话超时通常以毫秒为单位。当会话由于任何原因结束时，在该会话期间创建的临时节点也会被删除。
# Zookeeper 工作流
* 一旦ZooKeeper集合启动，它将等待客户端连接。客户端将连接到ZooKeeper集合中的一个节点。它可以是leader或follower节点。一旦客户端被连接，节点将向特定客户端分配会话ID并向该客户端发送确认。如果客户端没有收到确认，它将尝试连接ZooKeeper集合中的另一个节点。 一旦连接到节点，客户端将以有规律的间隔向节点发送心跳，以确保连接不会丢失。
	* 如果客户端想要读取特定的znode，它将会向具有znode路径的节点发送读取请求，并且节点通过从其自己的数据库获取来返回所请求的znode。为此，在ZooKeeper集合中读取速度很快。
	* 如果客户端想要将数据存储在ZooKeeper集合中，则会将znode路径和数据发送到服务器。连接的服务器将该请求转发给leader，然后leader将向所有的follower重新发出写入请求。如果只有大部分节点成功响应，而写入请求成功，则成功返回代码将被发送到客户端。 否则，写入请求失败。绝大多数节点被称为 Quorum 。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329203940729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNzIyODI3,size_16,color_FFFFFF,t_70)

|组件	|描述|
------|----------
写入（write）|	写入过程由leader节点处理。leader将写入请求转发到所有znode，并等待znode的回复。如果一半的znode回复，则写入过程完成。
读取（read）|	读取由特定连接的znode在内部执行，因此不需要与集群进行交互。
复制数据库（replicated database）|	它用于在zookeeper中存储数据。每个znode都有自己的数据库，每个znode在一致性的帮助下每次都有相同的数据。
Leader	|Leader是负责处理写入请求的Znode。
Follower	|follower从客户端接收写入请求，并将它们转发到leader znode。
请求处理器（request processor）|	只存在于leader节点。它管理来自follower节点的写入请求。
原子广播（atomic broadcasts）|	负责广播从leader节点到follower节点的变化。
# Zookeeper 的一致性
> Zookeeper 身为分布式系统协调服务，如果自身挂了如何处理呢？

* 为了防止单机挂掉的情况，Zookeeper 维护了一个集群。如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200329201028523.png)
* Zookeeper Service 集群是一主多从结构。
	* 在更新数据时，首先更新到主节点（这里的节点是指服务器，不是 Znode），再同步到从节点。
	* 在读取数据时，直接读取任意从节点。
	* 为了保证主从节点的数据一致性，Zookeeper 采用了 ZAB 协议，这种协议非常类似于一致性算法 Paxos 和 Raft。
# Zookeeper 的应用场景
### 分布式锁
* 这是雅虎研究员设计 Zookeeper 的初衷。利用 Zookeeper 的临时顺序节点，可以轻松实现分布式锁。

### 服务注册和发现
* 利用 Znode 和 Watcher，可以实现分布式服务的注册和发现。最著名的应用就是阿里的分布式 RPC 框架 Dubbo。
### 共享配置和状态信息
* Redis 的分布式解决方案 Codis，就利用了 Zookeeper 来存放数据路由表和 codis-proxy 节点的元信息。同时 codis-config 发起的命令都会通过 ZooKeeper 同步到各个存活的 codis-proxy。
* 此外，Kafka、HBase、Hadoop，也都依靠Zookeeper同步节点信息，实现高可用。

**你知道的越多，你不知道的越多。
有道无术，术尚可求，有术无道，止于术。
如有其它问题，欢迎大家留言，我们一起讨论，一起学习，一起进步**
