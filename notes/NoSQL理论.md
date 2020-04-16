# 1.1 NoSQL概述
###  NoSQL数据库
关系型数据库的补充
### NoSQL数据库优点
1. 灵活的可扩展性
2. 灵活的数据模型
3. 和云计算的精密结合

Web2.0的到来，传统关系数据库无法满足海量数据的管理需求。

### 传统数据库的性能的缺陷
1. 无法满足海量数据的管理需求
2. 无法满足高并发的需求
3. 无法满足可扩展性和高可用性的需求

### MySQL集群的缺陷
1. 复杂性
2. 延迟性
3. 扩容问题

# 1.2 NoSQL与关系数据库比较
### NoSQL与关系数据库比较
一、数据库原理方面
二、数据规模方面
三、数据库模式方面
四、查询效率方面
五、在事务一致性方面
六、数据完整性方面
七、可扩展性方面
八、可用性方面
# 1.3NoSQL的四大类型
**典型的NoSQL数 据库通常包括**键值数据库、列族数据库、文档数据库和图形数据库。

### 键值数据库
Redis、Riak、SimpleDB、Chordless、Scalaris、Memcached
##### 数据模型 
键/值对 
键是一个字符串对象 
值可以是任意类型的数据，比如整型、字符型、数组、列表、集合等
##### 优点 
扩展性好，灵活性好，大量写操作时性能高 
##### 缺点 
无法存储结构化信息，条件查询效率较低
### 列族数据库
BigTable、HBase、Cassandra、HadoopDB、GreenPlum、 PNUTS
##### 数据模型 
列族
##### 优点 
查找速度快，可扩展性强，容易进行分布式扩展，复杂性低 
##### 缺点 
功能较少，大都不支持强事务一致性
### 文档数据库
MongoDB、CouchDB、Terrastore、ThruDB、RavenDB、SisoDB、 RaptorDB、CloudKit、Perservere、Jackrabbit
##### 数据模型 
键/值 
值（value）是版本化的文档
##### 优点 
性能好（高并发），灵活性高，复杂性低，数据结构灵活 提供嵌入式文档功能，将经常查询的数据存储在同一个文档中 既可以根据键来构建索引，也可以根据内容构建索引 
##### 缺点 
缺乏统一的查询语法
### 图形数据库
Neo4J、OrientDB、InfoGrid、Infinite Graph、GraphDB
##### 数据模型 
图结构 
##### 优点 
灵活性高，支持复杂的图形算法，可用于构建复杂的关系图谱 
##### 缺点 
复杂性高，只能支持一定的数据规模
# 1.4NoSQL的三大基石
CAP 
BASE 
最终一 致性
### CAP
C（Consistency）：一致性
A:（Availability）：可用性
lP（Tolerance of Network Partition）：分区容忍性
注意:三者不能同时满足，最多只能同时满足其中两个。（三选二，只能同时满足两个）  
（NOSQL的P必须实现，所以NoSQL必须在C和A之间权衡）
### BASE
基本可用（Basically Availble）
软状态（Soft- state）
最终一致性（Eventual consistency）
### 最终一致性
一致性的类型包括强一致性和弱一致性

二者的主要区别在于高并发的数据 访问操作下，后续操作是否能够获取最新的数据。

##### ACID（关系型数据库）
 原子性(Atomicity) 
一致性(Consistency) 
隔离性(Isolation) 
持久性 (Durable)

