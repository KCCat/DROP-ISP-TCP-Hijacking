# DROP-ISP-TCP-Hijacking
使用 iptables 丢弃运营商的TCP/IP连接劫持包.  
__注意:目前仍在实验阶段,无法保证不会对正常的连接产生破坏.__  
## 使用方式  
* 保存`iptables-save-rule.conf`到本地, 修改`{you WAN}`为你的wan介面.**例**`ppp0`  
* 运行`iptables-restore`来恢复`iptables`规则.  
__注意__ :此操作会重置`iptables`所有表. 

##需求
* Module `bpf`
* Module `u32`
* Module `connmark`
* Module `mark`
* `connmark`的标志位需要`0x7FF`长度来记录MSS值,还有`0xFF`来记录TTL值.默认记录位置为`0x07FF00FF`.
* `mark`的标志位需要`0x1`长度.默认记录位置为`0x00000001`.

##工作原理
* 对每条`TCP`连接在握手时的`SYN+ACK`包的`TTL`值和`MSS`协商值.
* 并使用`TCP flags`标志为`ACK|ACK&&PSH`,且`TCP Data`长度为0或长度为`MSS`协商值的包对保存的`TTL`值更新.
* 其余包在不符合`TTL`值时将被`DROP`.
