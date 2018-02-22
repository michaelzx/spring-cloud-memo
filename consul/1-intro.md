# 介绍

Consul 是 HashiCorp 公司推出的开源工具，用于实现分布式系统的服务发现与配置。与其他分布式服务注册与发现的方案，Consul的方案更“一站式” ，内置了服务注册与发现框架

据说使用起来也较 为简单。Consul使用Go语言编写，因此具有天然可移植性(支持Linux、windows和Mac OS X)；安装包仅包含一个可执行文件，方便部署，与Docker等轻量级容器可无缝配合 。 基于 Mozilla Public License 2.0 的协议进行开源. Consul 支持健康检查,并允许 HTTP 和 DNS 协议调用 API 存储键值对. 一致性协议采用 Raft 算法,用来保证服务的高可用. 使用 GOSSIP 协议管理成员和广播消息, 并且支持 ACL 访问控制.

# Consul 四大特性

1. Service Discovery (服务发现)
2. Health Check (健康检查)
3. Multi Datacenter (多数据中心)
4. Key/Value Storage

# Consul Glossary(术语)

## Agent
- Agent 是一个守护进程
- 运行在Consul集群的每个成员上
- <font color=red>有Client 和 Server 两种模式</font>
- 所有Agent都可以被调用DNS或者HTTP API,并负责检查和维护同步

## Client
- Client 将所有RPC请求转发至Server
- Client 是相对无状态的
- Client 唯一做的就是参与LAN Gossip Pool
- Client 只消耗少量的资源和少量的网络带宽

## Server

- 参与 Raft quorum(一致性判断)
- 响应RPC查询请求
- 维护集群的状态
- 转发查询到Leader 或 远程数据中心

## Datacenter

顾名思义其为数据中心, 如何定义数据中心呢?  需要以下三点
- 私有的
- 低延迟
- 高带宽
  故: 可以简单的理解为同属一个内网环境, 如北京机房和香港机房就不一定满足以上三个条件

## Consensus (一致性)
Consul 使用consensus protocol 来提供CAP(一致性,高可用,分区容错性)

## Gossip
一种协议: 用来保证 最终一致性 , 即: 无法保证在某个时刻, 所有节点状态一致, 但可以保证”最终”一致

## LAN Gossip
Local Area Network Gossip,  位于同一个局域网或者数据中心的节点

## WAN Gossip
Wide Area Network Gossip, 只用于Server之间, 分部在不同的数据中心或广域网



# 构架图

![](https://www.consul.io/assets/images/consul-arch-420ce04a.png)


# 参考文章

[Introduction to Consul](https://www.consul.io/intro/index.html)

[现有系统如何集成Consul服务发现](https://www.jianshu.com/p/28c6bd590ca0)
