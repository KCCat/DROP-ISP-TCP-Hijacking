# DROP-ISP-TCP-Hijacking
使用 iptables 丢弃运营商的TCP/IP连接劫持包.  
## 使用方式  
* 保存`iptables-save-rule.conf`到本地, 修改`{you WAN}`为你的出口介面.**例**`eth0`  
* 运行`iptables-restore`来恢复`iptables`规则.  
__注意__ :此操作会重置`iptables`所有表. 

##需求
* Module bpf
* Module u32
* Module connmark
* Module mark

##工作原理
* 对每条TCP连接在握手时的SYN+ACK包的TTL值和MSS协商值.
* 并使用 TCP flags 标志为 ACK|ACK&&PSH ,且 TCP Data 长度为0或长度为MSS协商值的包对保存的TTL值更新.
* 其余包在不符合TTL值时将被DROP.
