---
title: Zookeeper的高可用集群与leader选举策略
date: 2019-07-17 14:23:02
tags:
    - 分布式
---


# Zookeeper的高可用集群与leader选举策略

### 一、概述

+ zookeeper作为分布式系统协调者和管理者，承担着联结分布式系统的各组件来组成一个完整服务的职责，如kafka作为一个分布式集群，在kafka的内部体系结构中包含消息生产者，消费消费者，消息存储broker三个核心组件，kafka通过在zookeeper中维护这三大组件的运作信息，并且这三大组件分别通过zookeeper获取其他组件的运作信息或者获取组件内部其他子组件的运作状态来协同工作，如消息消费者节点将其所消费的消息主题的分区的消息offset上传到zookeeper中，当该消费者机器节点宕机时，另外一个消费者可以基于zookeeper中保存的消息消费情况，继续接着消费数据，避免数据重复，从而实现了不同消费者节点之间的协作。
+ 所以zookeeper自身需要保证高可用，这样才能作为分布式系统的大管家，保证分布式系统的稳定运作。zookeeper的高可用也是通过集群的方式来实现的，即多个zookeeper实例节点共同组成一个集群来避免单点问题。
+ 是一种分布式协调服务，用于管理大型主机。数据模型类似于数据结构当中的树，树是由节点所组成，Zookeeper 的数据存储也同样是基于节点，这种节点叫做 Znode
  
  #### Znode
  - Znode包含哪些元素
  	- data：Znode 存储的数据信息。
  	- ACL：记录 Znode 的访问权限，即哪些人或哪些 IP 可以访问本节点。
  	- stat：包含 Znode 的各种元数据，比如事务 ID、版本号、时间戳、大小等等。
  	- child：当前节点的子节点引用 Zookeeper的设计初衷就是为了实现分布式锁
  - Znode 分为四种类型：
  	- 持久节点（PERSISTENT）
  	- 持久节点顺序节点（PERSISTENT_SEQUENTIAL）
  	- 临时节点（EPHEMERAL）
  	- 临时顺序节点（EPHEMERAL_SEQUENTIAL）

### 二、高可用集群

- zookeeper作为分布式系统协调者，在数据存储方面，zookeeper将数据组织成目录树的结构，然后在每个目录节点存放部分数据。从zookeeper集群角度而言，集群内每个zookeeper节点存放的都是相同的数据，这个跟Redis的集群是不一样的，Redis的集群主要用于实现一个分布式数据库来进行海量数据存储，所以每个节点存放不同的数据。而zookeeper集群的主要目的是实现高可用，所以在设计层面是基于主从来设计的，这样当主节点宕机时，可以将某个从节点升级为主节点，从而避免主节点的单点问题，实现高可用。

- 在数据读写方面，由于zookeeper集群是一个主从结构集群，故主节点负责处理写请求，然后将写请求同步给从节点，从而实现整个集群的数据一致性。所以zookeeper也是适合于读多写少的应用场景的。

#### 机器节点角色

  - 在zookeeper集群中，通过给节点定义角色来区分不同节点的功能。由以上分析可知，zookeeper集群是一个主从结构集群，其中角色定义包括三种：leader，follower，observer。
#### leader：领导者

- leader节点是zookeeper集群的主节点，主要负责所有的写请求，也可以处理读请求。除此之外，需要维持与从节点，即follower节点和observer节点的心跳，从而实时监测从节点的运作情况和通知从节点自身的运作情况，如假如leader节点宕机了，则不会再发心跳包给follower节点和observer节点，此时follower节点才能发现主节点挂了，从而进行新一轮leader选举实现崩溃恢复。
####  follower：跟随者
- follower节点是zookeeper集群的从节点，主要负责处理客户端的读请求，对于写请求则首先统一转发给leader节点，然后在leader节点执行写请求时，会将写请求以proposal的方式发送给向所有follower节点，投票来决定是否需要执行此次写请求，所以follower节点需要ack响应leader节点来进行投票。如果投票通过，则leader向所有的follower和observer节点发送提交commit请求，从而在所有节点执行本次数据写操作。
- 除了对leader节点的写请求进行投票外，follower节点还需要对leader节点的选举进行投票，从而选举出leader节点。
#### observer：观察者
- observer节点跟follower节点差不多，也是主要处理读请求，对于写请求则统一转发给leader节点。与follower节点不同的是，observer节点没有投票权，即不参与leader节点发起的写请求的投票和leader选举的投票，只是从leader节点同步数据，处理读请求，所以observer节点主要是对zookeeper集群的读请求的拓展，所以说zookeeper是适合读多写少的应用场景。
#### 机器节点状态与心跳包
- 每个节点的状态跟节点的角色类似，主要包含LOOKING，LEADING, FOLLOWING和OBSERVING四种。
- LOOKING：不确定leader节点的状态，此时该节点会认为当前集群不存在leader节点，故会主动发起一次leader选举，广播选举包，即投给自己，给其他节点。此时leader节点收到后会将自己的LEADING状态告诉该节点并投票给leader自身，其他follower节点收到后，则是将自己的FOLLOWING状态告诉该节点并投票给leader节点，该节点收到leader和其他follower的状态和投票后，知道当前leader节点的信息，并通过其他follower的投票确认该leader节点确实是leader节点，则设置自身状态为FOLLOWING，成为当前leader的follower节点。
- FOLLOWING：跟随者状态，即自身角色是follower。
- LEADING：领导状态，自身角色为leader，并且维持着与follower和observer的心跳。
- OBSERVING：观察者状态，即自身角色是observer。
### 三、Zab协议
- Zab协议，即原子广播协议，主要定义了zookeeper集群的数据同步方式，从而实现集群数据的最终一致性和定义了在leader节点宕机时的leader选举方式，从而实现集群的高可用。

#### 1. 写请求：原子广播

- 由上面的分析可知，zookeeper集群的每个节点的数据是一致的，并且由leader节点负责处理所有的写请求，然后再同步给follower和observer节点。
- leader节点执行一次写请求的过程如下：
	- 1. 客户端向leader节点发送写请求，或者follower节点或者observer节点向leader节点转发写请求；
	- 2. leader节点收到写请求，首先持久化到本地文件，然后将写请求以proposal的方式，注意proposal是包含写请求的修改操作的内容的，通过与follower节点维持的长连接（即心跳包来维持），广播给所有的follower节点并等待follower节点的ack；
	- 3. follower节点和observer接收到该proposal，将写操作持久化到本地，然后follower节点会响应ack给leader；
	- 4. leader只要收到超过半数follower节点的ack，即n/2+1个（leader自身也算一个ack），其中n为follower节点的个数，则向所有的follower和observer发送提交请求commit。注意这个过程有点像两阶段提交2PC，不同之处是2PC需要收到所有节点的ack才执行提交操作，而zab协议则只需要收到超过半数即提交，故性能相对2PC较好。不过这里说收到超过半数即提交不是说其他还没收到的节点就可以不同步，其他还没发送ack或者ack还没传到leader节点的follower还会继续同步数据，除非这个节点宕机了；
	- 5. leader节点，follower节点，observer节点收到commit请求后，则执行写操作，修改节点的内存数据，从而保证了数据持久性。
#### 消息有序性：顺序一致性
- 由以上过程可知，leader节点可能同时收到多个写请求，而写操作的过程又是异步非阻塞的，即leader可以同时处理多个写请求，而不需要等待前一个写请求完成以上过程才能继续下一个写请求，所以这里就存在一个消息有序性问题。
- zookeeper主要是利用一个全局有序消息id和FIFO队列来实现的。
- 全局有序id：leader节点每处理一个写请求都会为该写请求分配一个全局有序的消息id，称为zxid。zxid是一个64位的数字，其中前32位选举轮次epoch，后32位本轮次处理事务次数。即每经过一次leader选举，则epoch递增，处理事务次数从0开始。故在整个运行过程中，每个写请求都是全局唯一的。
- FIFO队列：leader节点会发送多个写请求对应的proposal给follower，leader节点会为每个follower节点都维护一个FIFO队列，从而实现对该follower的所有proposal按照FIFO的顺序进行ack处理，保证写请求的先后顺序一致。
#### 最终一致性
- zookeeper集群是一个高可用集群，实现的是数据的最终一致性，即集群各节点的数据最终会得到一致，而不是强一致性。由分布式的CAP理论可知，任何时候只能存在CP或者AP，即高可用+分区容忍性，或者强一致性和分区容忍性，CA，即高可用和强一致性是不可能同时存在的，因为数据需要通过网络在不同节点之间传播，由于网络的不稳定性，总是存在延迟或者数据丢失，所以不同节点的数据可能不一致。
####  崩溃恢复与leader选举：FastLeaderElection机制

- 由于leader节点基于心跳机制维持与follower和observer节点的长连接，故当leader节点宕机之后，该连接就断开了，follower和observer就不会再继续接收到leader的心跳包，或者leader节点失去大多数的followers，则会进入崩溃恢复阶段，重新进行leader选举。
#### 选举的投票数据
- leader选举时，每个follower节点的投票包的相关核心数据如下：
	- 1. logicalClock：该节点发起的第几轮投票，每选举一次加1；
	- 2. self_id：当前节点自身的myid；
	- 3. self_zxid：当前节点自身所保存的数据的最大zxid，越大说明处理了越多数据写请求；
	- 4. vote_id：当前节点投票给的节点的myid；
	- 5. vote_zxid：当前节点投票给的节点的数据的最大zxid。
    
#### leader选举的过程

- 1. leader选举开始时，每个follower节点首先清空自己的投票箱，然后投票给自己，并通过广播的方式通知所有其他节点给自己投票；
- 2. 每个follower节点接收到其他follower节点的选票，将该外部选票与自己的选票进行对比，对比主要是基于以上5个核心数据项来展开：
	- 1. 选举轮次：比较logicalClock，如果外部选票的logicalClock大于自己的，则说明自己的选举轮次落后于该外部节点了，则清空自己的投票箱，并将自己的投票更新为当前轮次后重新广播投票出去；小于则忽略该外部选票；等于则进入下面步骤继续比较其他数据；
	- 2. vote_zxid大小比较：将外部选票的vote_zxid与自己的投票的vote_zxid进行对比，如果外部的大，则将自己的（vote_myid，vote_zxid）更新为该外部选票的vote_myid和vote_zxid并广播出去，即投给这个vote_zxid更大的vote_myid；并更新自身的投票箱，即添加或者更新该外部投票对应的vote_myid的选票情况。因为在每个节点的投票箱中，对于集群中的所有参与投票follower节点只能存在一张投票，即当当前节点收到某个节点的多次投票时，则需要进行覆盖，如节点A刚开始收到节点B投给B自己的投票，A放入投票箱为（B,B），后来又收到B投给C，则更新为（B,C），此时A的投票箱不再存在（B，B）的这种选票了，而是更新为了（B,C）；
	- 3. vote_myid大小比较：如果vote_zxid相同，则投票给vote_myid更大的节点；
	- 4. 重复以上过程，当某个节点发现过半数的follower节点都投给了自己，则更新自己的状态为LEADING，其他节点则更新自己的状态为FOLLOWING，投票结束。接下来进入数据同步阶段。
	- 5. 数据同步阶段：主要是当前新的leader节点将自己的已经commit的数据同步给其他follower节点。




