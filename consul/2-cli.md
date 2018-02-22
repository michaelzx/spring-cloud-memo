# Consul-CLI

> Consul可以通过以命令行界面（CLI）来进行控制

# 查看可以用的command和说明
```
$ consul -h
Usage: consul [--version] [--help] <command> [<args>]

Available commands are:
    agent          Runs a Consul agent
    catalog        Interact with the catalog
    event          Fire a new event
    exec           Executes a command on Consul nodes
    force-leave    Forces a member of the cluster to enter the "left" state
    info           Provides debugging information for operators.
    join           Tell Consul agent to join cluster
    keygen         Generates a new encryption key
    keyring        Manages gossip layer encryption keys
    kv             Interact with the key-value store
    leave          Gracefully leaves the Consul cluster and shuts down
    lock           Execute a command holding a lock
    maint          Controls node or service maintenance mode
    members        Lists the members of a Consul cluster
    monitor        Stream logs from a Consul agent
    operator       Provides cluster-level tools for Consul operators
    reload         Triggers the agent to reload configuration files
    rtt            Estimates network round trip time between nodes
    snapshot       Saves, restores and inspects snapshots of Consul server state
    validate       Validate config files/directories
    version        Prints the Consul version
    watch          Watch for changes in Consul
```

> 注意：版本不同，这些命令可能是不用的，
> 我现在装的是1.0.3，这里的命令和[官网文档](https://www.consul.io/docs/commands/index.html)略有不同

# Consul常用命令
| 命令      | 解释                          | 示例                      |
| ------- | --------------------------- | ----------------------- |
| agent   | 运行一个consul agent            | consul agent -dev       |
| join    | 将agent加入到consul集群           | consul join IP          |
| members | 列出consul cluster集群中的members | members  consul members |
| leave   | 将节点移除所在集群                   | consul leave            |

# 核心命令：consul agent

## 参数

我们先来看看这个命令的参数

```
$ consul agent -h
Usage: consul agent [options]

  Starts the Consul agent and runs until an interrupt is received. The
  agent represents a single node in a cluster.

HTTP API Options

  -datacenter=<value>
     Datacenter of the agent.

Command Options

  -advertise=<value>
     Sets the advertise address to use.

  -advertise-wan=<value>
     Sets address to advertise on WAN instead of -advertise address.

  -bind=<value>
     Sets the bind address for cluster communication.

  -bootstrap
     Sets server to bootstrap mode.

  -bootstrap-expect=<value>
     Sets server to expect bootstrap mode.

  -client=<value>
     Sets the address to bind for client access. This includes RPC, DNS,
     HTTP and HTTPS (if configured).

  -config-dir=<value>
     Path to a directory to read configuration files from. This
     will read every file ending in '.json' as configuration in this
     directory in alphabetical order. Can be specified multiple times.

  -config-file=<value>
     Path to a JSON file to read configuration from. Can be specified
     multiple times.

  -config-format=<value>
     Config files are in this format irrespective of their extension.
     Must be 'hcl' or 'json'

  -data-dir=<value>
     Path to a data directory to store agent state.

  -dev
     Starts the agent in development mode.

  -disable-host-node-id
     Setting this to true will prevent Consul from using information
     from the host to generate a node ID, and will cause Consul to
     generate a random node ID instead.

  -disable-keyring-file
     Disables the backing up of the keyring to a file.

  -dns-port=<value>
     DNS port to use.

  -domain=<value>
     Domain to use for DNS interface.

  -enable-script-checks
     Enables health check scripts.

  -encrypt=<value>
     Provides the gossip encryption key.

  -hcl=<value>
     hcl config fragment. Can be specified multiple times.

  -http-port=<value>
     Sets the HTTP API port to listen on.

  -join=<value>
     Address of an agent to join at start time. Can be specified
     multiple times.

  -join-wan=<value>
     Address of an agent to join -wan at start time. Can be specified
     multiple times.

  -log-level=<value>
     Log level of the agent.

  -node=<value>
     Name of this node. Must be unique in the cluster.

  -node-id=<value>
     A unique ID for this node across space and time. Defaults to a
     randomly-generated ID that persists in the data-dir.

  -node-meta=<key:value>
     An arbitrary metadata key/value pair for this node, of the format
     `key:value`. Can be specified multiple times.

  -non-voting-server
     (Enterprise-only) This flag is used to make the server not
     participate in the Raft quorum, and have it only receive the data
     replication stream. This can be used to add read scalability to
     a cluster in cases where a high volume of reads to servers are
     needed.

  -pid-file=<value>
     Path to file to store agent PID.

  -protocol=<value>
     Sets the protocol version. Defaults to latest.

  -raft-protocol=<value>
     Sets the Raft protocol version. Defaults to latest.

  -recursor=<value>
     Address of an upstream DNS server. Can be specified multiple times.

  -rejoin
     Ignores a previous leave and attempts to rejoin the cluster.

  -retry-interval=<value>
     Time to wait between join attempts.

  -retry-interval-wan=<value>
     Time to wait between join -wan attempts.

  -retry-join=<value>
     Address of an agent to join at start time with retries enabled. Can
     be specified multiple times.

  -retry-join-wan=<value>
     Address of an agent to join -wan at start time with retries
     enabled. Can be specified multiple times.

  -retry-max=<value>
     Maximum number of join attempts. Defaults to 0, which will retry
     indefinitely.

  -retry-max-wan=<value>
     Maximum number of join -wan attempts. Defaults to 0, which will
     retry indefinitely.

  -segment=<value>
     (Enterprise-only) Sets the network segment to join.

  -serf-lan-bind=<value>
     Address to bind Serf LAN listeners to.

  -serf-wan-bind=<value>
     Address to bind Serf WAN listeners to.

  -server
     Switches agent to server mode.

  -syslog
     Enables logging to syslog.

  -ui
     Enables the built-in static web UI server.

  -ui-dir=<value>
     Path to directory containing the web UI resources.
```
> 很多很暴力...

## 常用参数

> -data-dir

- 作用：指定agent储存状态的数据目录
- 这是所有agent都必须的
- 对于server尤其重要，因为他们必须持久化集群的状态

> -config-dir

- 作用：指定service的配置文件和检查定义所在的位置
- 通常会指定为”某一个路径/consul.d”（通常情况下，.d表示一系列配置文件存放的目录）

> -config-file

- 作用：指定一个要装载的配置文件
- 该选项可以配置多次，进而配置多个配置文件（后边的会合并前边的，相同的值覆盖）

> -dev

- 作用：创建一个开发环境下的server节点
- 该参数配置下，不会有任何持久化操作，即不会有任何数据写入到磁盘
- 这种模式不能用于生产环境（因为第二条）

> -bootstrap-expect

- 作用：该命令通知consul server我们现在准备加入的server节点个数，该参数是为了延迟日志复制的启动直到我们指定数量的server节点成功的加入后启动。

> -node

- 作用：指定节点在集群中的名称
- 该名称在集群中必须是唯一的（默认采用机器的host）
- 推荐：直接采用机器的IP

> -bind

- 作用：指明节点的IP地址
- 有时候不指定绑定IP，会报`Failed to get advertise address: Multiple private IPs found. Please configure one.`的异常

> -server

- 作用：指定节点为server
- 每个数据中心（DC）的server数推荐至少为1，至多为5
- 所有的server都采用raft一致性算法来确保事务的一致性和线性化，事务修改了集群的状态，且集群的状态保存在每一台server上保证可用性
- server也是与其他DC交互的门面（gateway）

> -client

- 作用：指定节点为client，指定客户端接口的绑定地址，包括：HTTP、DNS、RPC
- 默认是127.0.0.1，只允许回环接口访问
- 若不指定为-server，其实就是-client

> -join

- 作用：将节点加入到集群

> -datacenter（老版本叫-dc，-dc已经失效）

- 作用：指定机器加入到哪一个数据中心中



## 跑一跑

### 单机

上一篇中我以开发模式启动了，现在我要来试试，正常启动

```
$ consul agent
==> data_dir cannot be empty
```

oops,果然没那么简单……那我给他加上

```shell
$ consul agent -data-dir=/Users/michael/tmp_consul
==> Starting Consul agent...
==> Consul agent running!
           Version: 'v1.0.3'
           Node ID: '13042cd6-06ea-7f0a-4997-a2d2aecfe698'
         Node name: 'zhangxiaodeMacBook-Pro.local'
        Datacenter: 'dc1' (Segment: '')
            Server: false (Bootstrap: false)
       Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, DNS: 8600)
      Cluster Addr: 192.168.31.222 (LAN: 8301, WAN: 8302)
           Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false

==> Log data will now stream in as it occurs:

    2018/02/03 16:34:56 [INFO] serf: EventMemberJoin: zhangxiaodeMacBook-Pro.local 192.168.31.222
    2018/02/03 16:34:56 [WARN] serf: Failed to re-join any previously known node
    2018/02/03 16:34:56 [INFO] agent: Started DNS server 127.0.0.1:8600 (tcp)
    2018/02/03 16:34:56 [INFO] agent: Started DNS server 127.0.0.1:8600 (udp)
    2018/02/03 16:34:56 [INFO] agent: Started HTTP server on 127.0.0.1:8500 (tcp)
    2018/02/03 16:34:56 [INFO] agent: started state syncer
    2018/02/03 16:34:56 [WARN] manager: No servers available
    2018/02/03 16:34:56 [ERR] agent: failed to sync remote state: No known Consul servers
    2018/02/03 16:34:57 [WARN] manager: No servers available
    2018/02/03 16:34:57 [ERR] http: Request GET /v1/kv/config/account-service/?recurse&wait=55s&index=1, error: No known Consul servers from=127.0.0.1:54630
```

> 启动是启动了，但是尴尬的是报Err了。
> 对了，第一篇文章中有说道：Agent有Client 和 Server 两种模式
> 看来默认启动的方式是Client，所有一直报错没有可用服务器

这个时候，就得好好看看上面，重新试下：

```
$ consul agent -server -ui -data-dir=/Users/michael/tmp_consul
==> Starting Consul agent...
==> Consul agent running!
           Version: 'v1.0.3'
           Node ID: '13042cd6-06ea-7f0a-4997-a2d2aecfe698'
         Node name: 'zhangxiaodeMacBook-Pro.local'
        Datacenter: 'dc1' (Segment: '<all>')
            Server: true (Bootstrap: false)
       Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, DNS: 8600)
      Cluster Addr: 192.168.31.222 (LAN: 8301, WAN: 8302)
           Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false

==> Log data will now stream in as it occurs:

    2018/02/03 16:45:43 [INFO] raft: Initial configuration (index=0): []
    2018/02/03 16:45:43 [INFO] raft: Node at 192.168.31.222:8300 [Follower] entering Follower state (Leader: "")
    2018/02/03 16:45:43 [INFO] serf: EventMemberJoin: zhangxiaodeMacBook-Pro.local.dc1 192.168.31.222
    2018/02/03 16:45:43 [WARN] serf: Failed to re-join any previously known node
    2018/02/03 16:45:43 [INFO] serf: EventMemberJoin: zhangxiaodeMacBook-Pro.local 192.168.31.222
    2018/02/03 16:45:43 [INFO] consul: Adding LAN server zhangxiaodeMacBook-Pro.local (Addr: tcp/192.168.31.222:8300) (DC: dc1)
    2018/02/03 16:45:43 [INFO] consul: Handled member-join event for server "zhangxiaodeMacBook-Pro.local.dc1" in area "wan"
    2018/02/03 16:45:43 [INFO] agent: Started DNS server 127.0.0.1:8600 (udp)
    2018/02/03 16:45:43 [INFO] agent: Started DNS server 127.0.0.1:8600 (tcp)
    2018/02/03 16:45:43 [INFO] agent: Started HTTP server on 127.0.0.1:8500 (tcp)
    2018/02/03 16:45:43 [INFO] agent: started state syncer
    2018/02/03 16:45:49 [WARN] raft: no known peers, aborting election
    2018/02/03 16:45:50 [ERR] agent: failed to sync remote state: No cluster leader
```
我去，有报错：failed to sync remote state: No cluster leader

回想一下，第一篇文章中的构架图，在一个DC(中)需要有一个leader-server

我现在的场景，就是在本地玩玩，那么相当于是：一个数据中只有一个server,需要把自己作为leader

这里有一个参数可用

><a name="锚点名称">-bootstrap</a> - This flag is used to control if a server is in "bootstrap" mode. It is important that no more than one server per datacenter be running in this mode. Technically, a server in bootstrap mode is allowed to self-elect as the Raft leader. It is important that only a single node is in this mode; otherwise, consistency cannot be guaranteed as multiple nodes are able to self-elect. It is not recommended to use this flag after a cluster has been bootstrapped.

稍作修改

```
$ consul agent -data-dir=/Users/michael/tmp_consul -server -bootstrap -ui -node=server1
bootstrap = true: do not enable unless necessary
==> Starting Consul agent...
==> Consul agent running!
           Version: 'v1.0.3'
           Node ID: 'b53bc378-a8f4-7242-2f92-2f8bdecbc931'
         Node name: 'server1'
        Datacenter: 'dc1' (Segment: '<all>')
            Server: true (Bootstrap: true)
       Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, DNS: 8600)
      Cluster Addr: 192.168.31.222 (LAN: 8301, WAN: 8302)
           Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false

==> Log data will now stream in as it occurs:

    2018/02/03 17:16:03 [INFO] raft: Initial configuration (index=1): [{Suffrage:Voter ID:b53bc378-a8f4-7242-2f92-2f8bdecbc931 Address:192.168.31.222:8300}]
    2018/02/03 17:16:03 [INFO] raft: Node at 192.168.31.222:8300 [Follower] entering Follower state (Leader: "")
    2018/02/03 17:16:03 [INFO] serf: EventMemberJoin: server1.dc1 192.168.31.222
    2018/02/03 17:16:03 [INFO] serf: EventMemberJoin: server1 192.168.31.222
    2018/02/03 17:16:03 [INFO] consul: Handled member-join event for server "server1.dc1" in area "wan"
    2018/02/03 17:16:03 [INFO] agent: Started DNS server 127.0.0.1:8600 (udp)
    2018/02/03 17:16:03 [INFO] consul: Adding LAN server server1 (Addr: tcp/192.168.31.222:8300) (DC: dc1)
    2018/02/03 17:16:03 [INFO] agent: Started DNS server 127.0.0.1:8600 (tcp)
    2018/02/03 17:16:03 [INFO] agent: Started HTTP server on 127.0.0.1:8500 (tcp)
    2018/02/03 17:16:03 [INFO] agent: started state syncer
    2018/02/03 17:16:08 [WARN] raft: Heartbeat timeout from "" reached, starting election
    2018/02/03 17:16:08 [INFO] raft: Node at 192.168.31.222:8300 [Candidate] entering Candidate state in term 2
    2018/02/03 17:16:08 [INFO] raft: Election won. Tally: 1
    2018/02/03 17:16:08 [INFO] raft: Node at 192.168.31.222:8300 [Leader] entering Leader state
    2018/02/03 17:16:08 [INFO] consul: cluster leadership acquired
    2018/02/03 17:16:08 [INFO] consul: New leader elected: server1
    2018/02/03 17:16:08 [INFO] consul: member 'server1' joined, marking health alive
    2018/02/03 17:16:09 [INFO] agent: Synced node info
```

终于是跑起来了，http://localhost:8500/也能访问了

### 集群

> 日后再研究，接下来研究下如何把SpringBoot服务注册到consul上









# 参考文章
[Consul Commands (CLI)](https://www.consul.io/docs/commands/index.html)

[Spring Cloud（二）Consul 服务治理实现](http://www.ymq.io/2017/11/26/spring-cloud-consul/)

[Consul的安装方法](http://xiaoquqi.github.io/blog/2015/12/07/consul-installation/)

[使用consul实现分布式服务注册和发现](http://tonybai.com/2015/07/06/implement-distributed-services-registery-and-discovery-by-consul/)