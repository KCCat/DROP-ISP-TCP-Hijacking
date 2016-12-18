# DROP-ISP-TCP-Hijacking

使用 iptables 来阻止运营商的TCP/IP连接劫持.

需要模块 U32,BPF 等.

原理:通过记录每条连接的TTL和MSS值来排除劫持包
