# Zookeeper



# 初识Zookeeper

Zookeeper是Apache hadoop项目（大数据框架）下面的一个子项目，是一个树形目录服务

简称zk，叫动物园管理员，管理大数据里的Hadoop（大象）Hive（蜜蜂）Pig（小猪）

是一个分布式的，开源的**分布式应用程序的协调服务**

主要功能：

* 配置管理
* 分布式锁
* 集群管理（做注册中心）





# Zookeeper安装与配置

详情请见[Zookeeper安装.md](.\Zookeeper安装.md)





# Zookeeper命令操作



## 1. Zookeeper的数据模型



是一个树形目录服务，数据模型和Unix文件系统目录类似，层次化结构

每一个节点被称为：ZNode，**每个节点都会保存自己的数据和节点信息**

节点可以拥有子节点，同时允许少量（小于1MB）数据存储在该节点下

节点分为四大类：

* PERSISTENT：持久化节点（默认）
* EPHEMERAL：临时节点（-e）
* PERSISTENT_SEQUENTIAL：持久化顺序节点（-s）
* EPHEMERAL_SEQUENTIAL：临时顺序节点（-es）





## 2. Zookeeper服务端常用命令



zkServer.sh start/stop/status/restart





## 3. Zookeeper客户端常用命令



连接Zookeeper服务端：

Windows系统双击zkCli.cmd

Linux系统中：

zkCli.sh -server localhost:2181（IP:端口）



* 断开连接：quit
* 查看命令帮助：help
* 显示指定目录下节点：ls 目录
  * 查询节点详细信息：ls -s /节点路径
* 创建节点：create /节点路径 节点数据
  * create -e /节点路径 节点数据（临时节点，退出后就没了）
  * create -s /节点路径 节点数据（顺序节点，自动生成编号）
* 获取节点数据：get /节点路径
* 设置节点数据：set /节点路径 节点数据
* 删除单个节点：delete /节点路径
* 删除带有子节点的节点：deleteall /节点路径







# Zookeeper的JavaAPI操作

> API操作，详情请见IDEA的Zookeeper项目中展示



## Curator

Apache ZooKeeper的Java客户端

简化了原生ZooKeeper Java API，简化ZooKeeper客户端的使用



## Curator API

建立连接

添加

删除

修改

查询

**Watch事件监听**：Zookeeper允许用户在指定节点上注册一些Watcher，并且在一些特定的事件触发后，Zookeeper服务端会将事件通知到感兴趣的客户端上，该机制是Zookeeper实现分布式协调服务的重要特性

Zookeeper中引入了Watcher机制来实现发布/订阅模式。能够让多个订阅者同时监听某一个对象，当一个对象自身状态变化时，会通知所有订阅者

**Zookeeper原生支持通过注册Watcher来进行事件监听**，但是其使用的时候不是很方便

**Curator引入了Cache来实现对Zookeeper服务端事件的监听**

Zookeeper的三种Watcher：

* NodeCache：只监听某一节点
* PathChildrenCache：监控一个节点的所有子节点
* TreeCache：可以监控整个树上的所有节点



**分布式锁**



## 分布式锁

在单机应用开发时，涉及到的并发同步问题，往往采用synchronized或者Lock的方式来解决多线程之间的代码同步问题，这时候的多线程都是在一个JVM中的

但是在分布式集群的环境下工作时，这是属于多JVM的环境，跨JVM之间已经无法通过多线程的锁来解决同步问题

所以，**分布式锁**：就是来**解决跨机器进程之间的数据同步问题**

分布式锁的实现：

* 缓存实现分布式锁：
  * Redis
  * Memcache
* Zookeeper实现分布式锁：Curator
* 数据库实现分布式锁：拿一张空表。性能巨低



Zookeeper分布式锁原理：

* 核心思想：当客户端要获取锁时，则创建节点，使用完锁后，则删除该节点

1. 客户端获取锁时，在指定的一个锁节点下创建临时顺序节点（临时节点的意义在于防止客户端使用锁的时候宕机，造成死锁无法释放资源）
2. 然后获取锁节点下所有的子节点，客户端获取到所有的子节点后，如果发现自己创建的子节点序号是最小的，那么就认为该客户端获取到了锁。使用完锁之后，将该节点删除
3. 如果发现自己创建的节点不是锁节点中最小的，那么就说明自己没有获取到锁，此时的客户端需要找到比自己小的那个节点，同时对其注册事件监听，监听删除事件
4. 如果发现比自己小的节点被删除，则客户端的Watcher就会收到通知，此时再判断自己创建的子节点是否是锁节点的子节点中最小的，如果是那么就获取到了锁，如果不是，那么就继续获取比自己小的节点注册监听删除事件。



Curator实现分布式锁的API：

Curator的五种锁：

* InterProcessSemaphoreMutex：分布式非可重入排它锁（可重入：对于一个资源，当前线程访问过后，是否可以再次访问)
* InterProcessMutex：分布式可重入排它锁
* InterProcessReadWriteLock：分布式读写锁
* InterProcessMultiLock：将多个锁作为单个实体管理的容器
* InterProcessSemaphoreV2：共享信号量





## 案例：模拟12306售票

> 详情请见IDEA的Zookeeper项目中展示



也就是利用分布式锁模拟解决并发问题



# Zookeeper集群搭建



集群介绍：

Leader：Leader服务器在 ZooKeeper 中的作主要是处理事务性的会话请求以及管理 ZooKeeper 集群中的其他角色服务器

Leader选举：

* Serverid：服务器ID，编号越大在选择算法中权重就越大
* Zxid：数据ID，服务器中存放的最大数据ID，值越大说明数据越新，在选举算法中数据越新权重越大
* 在Leader的选举中，某台Zookeeper获取了超过半数的得票就会成为Leader





集群搭建：

伪集群：以端口进行区分

详情请见[Zookeeper集群搭建.md](.\Zookeeper集群搭建.md)



集群异常：

* 3个节点的集群，从服务器挂掉，集群正常
* 3个节点的集群，2个从服务器都挂掉，主服务器也无法运行。因为可运行的机器没有超过集群总数量的半数
* 当集群中的主服务器挂了，集群中的其他服务器会自动进行选举状态，然后产生新得leader
* 当领导者产生后，再次有新服务器加入集群，不会影响到现任领导者





# Zookeeper核心理论



集群角色：

* Leader 领导者：
  1. 处理事务请求
  2. 集群内部各服务器的调度者（同步各服务器数据）
* Follower 追随者：
  1. 处理客户端非事务请求（查询），转发事务请求给Leader服务器
  2. 参与Leader的选举投票
* Observer 观察者：
  1. 分担Follower的处理非事务请求的压力，处理非事务请求，转发事务请求给Leader服务器





> ©iWyh2





