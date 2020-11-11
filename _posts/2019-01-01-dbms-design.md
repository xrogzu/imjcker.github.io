---
layout: post
title: 数据库设计方案
category: DB
tags: oracle mysql 
---

1. 多数据源动态切换

同一服务对应多个数据库，根据登录信息的不同，切换数据库。各院可以动态增加自己的数据库；每个数据源都维护自己的连接池配置，从而分担数据库的压力

**流程图**

```flow
start=>start: 请求
end=>end: 结束
op_api_req=>operation: xxAPI
op_switch_db=>operation: 根据用户信息切换数据源
cond_is_switch=>condition: 是否启用默认DB?
io_api_response=>inputoutput: 请求结果

start->op_api_req->cond_is_switch
cond_is_switch(yes)->io_api_response->end
cond_is_switch(no)->op_switch_db(right)->op_api_req
```

**问题**

- [ ] 一致性该如何解决
- [x] 连接池问题

2. 所有服务共用一个库

多个服务对应一个库，即所有服务使用同一个数据库，这无疑是增加了数据库的压力，也不是一个真正的微服务规范做法。

**问题**

- [ ] 主要是连接数不够的问题

同一个库情况下，连接数不够的因素：

随着业务逻辑的拆分和新业务的加入，服务拆分粒度越细，服务个数太多，如果采用默认的连接池配置，加上多节点的配置，导致连接数成倍增长（默认个数x节点个数x拆分个数）；假如拆分出10个服务，默认连接池大小为10，最大连接100，每个服务部署3个节点 ，最后将产生至少300个连接；最大session数量也是有待解决的问题。

一些数据库的默认最大连接数：

> 1. postgreSQL 100
> 2. MySQL （Windows < 2048）,（Linux/Solaris 视环境内存等因素而定）
> 3. Oracle 

3. 不同服务使用不同数据库服务

微服务提倡各个服务独立，互不依赖，甚至提倡各个微服务之间的数据库、抽象接口、utility 都不共享，

1. 主从分离，读写分离
2. 提供公共的数据访问服务，其他服务少访问数据库或不进行数据库访问
3. 程序上，优化代码，尽量减少数据库的读写，逻辑尽量不要放到数据库层，尽早释放连接，能重用时尽量避免重新获取新的连接等等。
4. 数据库上，优化数据库配置，采用一主多从、多主多从、分数据库服务器等方法，通过增加数据库服务器分担单台的压力。
5. 重构现有逻辑，将不同的数据分别存储到不同的数据库服务器上。
6. 采用MQ解决一致性问题。

7. 附录

**AKF**拆分原则

![img](https://images2018.cnblogs.com/blog/668104/201803/668104-20180315151827430-1071777557.png)

AKF扩展立方体(参考《The Art of Scalability》)，是一个叫AKF的公司的技术专家抽象总结的应用扩展的三个维度。理论上按照这三个扩展模式，可以将一个单体系统，进行无限扩展。
**X 轴 ：**指的是水平复制，很好理解，就是讲单体系统多运行几个实例，做个集群加负载均衡的模式。
**Z 轴 ：**是基于类似的数据分区，比如一个互联网打车应用突然火了，用户量激增，集群模式撑不住了，那就按照用户请求的地区进行数据分区，北京、上海、四川等多建几个集群。
**Y 轴 ：**就是我们所说的微服务的拆分模式，就是基于不同的业务拆分。
场景说明：比如打车应用，一个集群撑不住时，分了多个集群，后来用户激增还是不够用，经过分析发现是乘客和车主访问量很大，就将打车应用拆成了三个服务：乘客服务、车主服务、支付服务。三个服务的业务特点各不相同，独立维护，各自都可以再次按需扩展。


# 事务

**事务的基本特性**

1. 原子性

2. 一致性

3. 隔离性

4. 持久性

**隔离级别**

1. 读取未提交

2. 读取已提交

3. 可重复读取

4. 可串行化

**传播行为**



# 读写分离

## 什么是读写分离

随着一个网站的业务不断扩展，数据不断增加，数据库的压力也会越来越大，对数据库或者SQL的基本优化可能达不到最终的效果，我们可以采用读写分离的策 略来改变现状。读写分离现在被大量应用于很多大型网站，这个技术也不足为奇了。

​     读写分离简单的说是把对数据库读和写的操作分开对应不同的数据库服务器，这样能有效地减轻数据库压力，也能减轻IO压力。主数据库提供写操作，从数据库提供读操作，其实在很多系统中，主要是读的操作。当主数据库进行写操作时，数据要同步到从的数据库，这样才能有效保证数据库完整性。使用读写分离最大的作用无非是缓解服务器压力。



## 为什么要用读写分离

​	在实际的生产环境中，对数据库的读和写都在同一个数据库服务器中，是不能满足实际需求的。无论是在安全性、高可用性还是高并发等各个方面都是完全不能满足实际需求的。因此，通过主从复制的方式来同步数据，再通过读写分离来提升数据库的并发负载能力。

**保证数据安全**

​	读写分离架构可通过主从复制的方式可增加冗余，从而达到数据备份的目的，防止数据的丢失，保证了数据的安全性。

**提升并发和吞吐**

​	读写分离主要是为了解决数据库的“写入”问题，从而达到提升“读出”的效率。减轻数据库服务的负载，从而提升并发和吞吐，增加了系统的处理能力和效率。

总体上说，做读写分离是为了提高系统的高可用性、健壮性、数据的安全性和处理能力以及效率。



## 部署应用

1. 不同MQ的代码是不是要重写
2. 对象存储问题
3. 目前在做阿里平台的测试，我们负责产出DOCKER，航天，华为、阿里、腾讯，上去部署我们的DOCKER，针对不同平台，我们的应用需要做什么配置修改、程序修改、所有的调整内容，记录操作步骤、操作过程、形成过程文档（遇见的问题、如何解决，如何配置等关联），如果可以给出横向对比的结论。
4. OSS NAS 调用操作也要进行测试，形成过程文档。
5. 自动填录的应用需要继续做。
6. 设计系统与系统之间的关系与边界在设计文档中未体现，目前RPC 未定，DUBBO的的开发是否高耦合，RPC RSETFUL是否都兼容。
7. 在部署模式上需要支持多级部署，要整体考虑 分库、数据交换、协同、数据同步等，需要确认是否基于容器能够兼容所有平台部署

