# 安装Consul
下载地址：https://www.consul.io/downloads.html

各种平台的都有下载：`Mac OS X`、`FreeBSD`、`Linux`、`Solaris`、`Windows`

我是在mac上使用，所以下了个mac版本的

下过来的zip，解压以后得到`一个单独二级制文件`，我把它直接放到了`/usr/local/bin`下

这样，在控制台中，我就直接能使用了，可以使用以下命令，来测试是否可运行

```
$ consul -v
Consul v1.0.3
Protocol 2 spoken by default, understands 2 to 3 (agent will automatically use protocol >2 when speaking to compatible agents)
```

# 以调试模式启动Consul


可以用下面的命令启动Consul的开发模式：
```
$ consul agent -dev
==> Starting Consul agent...
==> Consul agent running!
           Version: 'v1.0.3'
           Node ID: 'aef05660-c518-c124-f8bd-e2e8148b0fa2'
         Node name: 'zhangxiaodeMacBook-Pro.local'
        Datacenter: 'dc1' (Segment: '<all>')
            Server: true (Bootstrap: false)
       Client Addr: [127.0.0.1] (HTTP: 8500, HTTPS: -1, DNS: 8600)
      Cluster Addr: 127.0.0.1 (LAN: 8301, WAN: 8302)
           Encrypt: Gossip: false, TLS-Outgoing: false, TLS-Incoming: false

==> Log data will now stream in as it occurs:

    2018/02/03 15:37:32 [DEBUG] Using random ID "aef05660-c518-c124-f8bd-e2e8148b0fa2" as node ID
    2018/02/03 15:37:32 [INFO] raft: Initial configuration (index=1): [{Suffrage:Voter ID:aef05660-c518-c124-f8bd-e2e8148b0fa2 Address:127.0.0.1:8300}]
    2018/02/03 15:37:32 [INFO] raft: Node at 127.0.0.1:8300 [Follower] entering Follower state (Leader: "")
    2018/02/03 15:37:32 [INFO] serf: EventMemberJoin: zhangxiaodeMacBook-Pro.local.dc1 127.0.0.1
    2018/02/03 15:37:32 [INFO] serf: EventMemberJoin: zhangxiaodeMacBook-Pro.local 127.0.0.1
    2018/02/03 15:37:32 [INFO] consul: Handled member-join event for server "zhangxiaodeMacBook-Pro.local.dc1" in area "wan"
    2018/02/03 15:37:32 [INFO] consul: Adding LAN server zhangxiaodeMacBook-Pro.local (Addr: tcp/127.0.0.1:8300) (DC: dc1)
    2018/02/03 15:37:32 [INFO] agent: Started DNS server 127.0.0.1:8600 (udp)
    2018/02/03 15:37:32 [INFO] agent: Started DNS server 127.0.0.1:8600 (tcp)
    2018/02/03 15:37:32 [INFO] agent: Started HTTP server on 127.0.0.1:8500 (tcp)
    2018/02/03 15:37:32 [INFO] agent: started state syncer
    2018/02/03 15:37:32 [WARN] raft: Heartbeat timeout from "" reached, starting election
    2018/02/03 15:37:32 [INFO] raft: Node at 127.0.0.1:8300 [Candidate] entering Candidate state in term 2
    2018/02/03 15:37:32 [DEBUG] raft: Votes needed: 1
    2018/02/03 15:37:32 [DEBUG] raft: Vote granted from aef05660-c518-c124-f8bd-e2e8148b0fa2 in term 2. Tally: 1
    2018/02/03 15:37:32 [INFO] raft: Election won. Tally: 1
    2018/02/03 15:37:32 [INFO] raft: Node at 127.0.0.1:8300 [Leader] entering Leader state
    2018/02/03 15:37:32 [INFO] consul: cluster leadership acquired
    2018/02/03 15:37:32 [INFO] consul: New leader elected: zhangxiaodeMacBook-Pro.local
    2018/02/03 15:37:32 [DEBUG] consul: Skipping self join check for "zhangxiaodeMacBook-Pro.local" since the cluster is too small
    2018/02/03 15:37:32 [INFO] consul: member 'zhangxiaodeMacBook-Pro.local' joined, marking health alive
    2018/02/03 15:37:33 [DEBUG] Skipping remote check "serfHealth" since it is managed automatically
    2018/02/03 15:37:33 [INFO] agent: Synced node info
    2018/02/03 15:37:33 [DEBUG] agent: Node info in sync
    2018/02/03 15:37:33 [DEBUG] Skipping remote check "serfHealth" since it is managed automatically
    2018/02/03 15:37:33 [DEBUG] agent: Node info in sync
    2018/02/03 15:37:38 [DEBUG] http: Request GET /v1/event/list?wait=5s&index=1 (5.001249189s) from=127.0.0.1:54222
```
> 启动一个开发模式的consul非常简单
> 但是在生产环境，我想并没有这么简单，后面再深入了解下

现在访问下：http://localhost:8500 ，可以看到consul的ui界面





# 参考文章
[Install Consul](https://www.consul.io/docs/install/index.html)

[Spring Cloud构建微服务架构：服务注册与发现（Eureka、Consul）【Dalston版】](http://www.spring4all.com/article/291)

[服务注册发现consul之一：spring cloud consul介绍及安装](http://www.cnblogs.com/duanxz/p/7053301.html)
