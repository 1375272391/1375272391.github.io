---
layout: post
title:  "作业17：1+X操作（任务6-9）"
author: XM137
date:   2024-11-17 00:15:00 +0800
categories: ENSP
tags: ENSP VRRP MSTP OSPF DHCP
---


## 拓扑 **[11.7z](/assets/ENSP/20241114/11.7z)** <br> 试题 **[L11](/assets/ENSP/20241114/L11.7z)**

---
## 设备地址配置

|    设备     |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |         DHCP       |       DHCP         |        DHCP        |
|    PC_2     |         DHCP       |       DHCP         |        DHCP        |
|SZ-Edu-Server|     192.168.3.10   |   255.255.255.0    |    192.168.3.254   |


---


## 1-2 设备命名、VLAN
### SZ-Acc01
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Acc01
[SZ-Acc01]v b 10 20
[SZ-Acc01]int e0/0/1
[SZ-Acc01-Ethernet0/0/1]port l a
[SZ-Acc01-Ethernet0/0/1]port d  v 10
[SZ-Acc01-Ethernet0/0/1]int e0/0/2
[SZ-Acc01-Ethernet0/0/2]port l a
[SZ-Acc01-Ethernet0/0/2]port d v 20
[SZ-Acc01-Ethernet0/0/2]port-g g e 0/0/3 t e 0/0/4
[SZ-Acc01-port-group]port l t
[SZ-Acc01-port-group]port t a v 10 20
[SZ-Acc01-port-group]port t p v 1
{% endhighlight %}

### SZ-Acc02
{% highlight cli %}
<Huawei>sy 
[Huawei]u in e
[Huawei]sysn SZ-Acc02
[SZ-Acc02]v b 10 20 100
[SZ-Acc02]int g0/0/1
[SZ-Acc02-GigabitEthernet0/0/1]port l t
[SZ-Acc02-GigabitEthernet0/0/1]port t a v 10 20
[SZ-Acc02-GigabitEthernet0/0/1]port t p v 1
[SZ-Acc02-GigabitEthernet0/0/1]int g0/0/2
[SZ-Acc02-GigabitEthernet0/0/2]port l a
[SZ-Acc02-GigabitEthernet0/0/2]port d v 100
[SZ-Acc02-GigabitEthernet0/0/2]
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Acc03
[SZ-Acc03]v b 10 20 101
[SZ-Acc03]int g0/0/1
[SZ-Acc03-GigabitEthernet0/0/1]port l t
[SZ-Acc03-GigabitEthernet0/0/1]port t a v 10 20
[SZ-Acc03-GigabitEthernet0/0/1]port t p v 1
[SZ-Acc03-GigabitEthernet0/0/1]int g0/0/2
[SZ-Acc03-GigabitEthernet0/0/2]port l a
[SZ-Acc03-GigabitEthernet0/0/2]port d v 101
[SZ-Acc03-GigabitEthernet0/0/2]
{% endhighlight %}

### SZ-Edu-SW1
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Edu-SW1
[SZ-Edu-SW1]v b 30
[SZ-Edu-SW1]port-g g g 0/0/1 t g 0/0/3
[SZ-Edu-SW1-port-group]port l h
[SZ-Edu-SW1-port-group]port h p v 30
[SZ-Edu-SW1-port-group]port h u v 30
{% endhighlight %}


## 3、IP编址
### SZ-Acc02
{% highlight cli %}
[SZ-Acc02]int v10
[SZ-Acc02-Vlanif10]ip a 192.168.1.1 24
[SZ-Acc02-Vlanif10]int v20
[SZ-Acc02-Vlanif20]ip a 192.168.2.2 24
[SZ-Acc02-Vlanif20]int v100
[SZ-Acc02-Vlanif100]ip a 10.1.1.1 24
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
[SZ-Acc03]int v10
[SZ-Acc03-Vlanif10]ip a 192.168.1.2 24
[SZ-Acc03-Vlanif10]int v20
[SZ-Acc03-Vlanif20]ip a 192.168.2.1 24
[SZ-Acc03-Vlanif20]int v101
[SZ-Acc03-Vlanif101]ip a 10.1.2.1 24
{% endhighlight %}

### SZ-Agg01
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Agg01
[SZ-Agg01]int g0/0/0
[SZ-Agg01-GigabitEthernet0/0/0]ip a 10.1.1.2 24
[SZ-Agg01-GigabitEthernet0/0/0]int g0/0/1
[SZ-Agg01-GigabitEthernet0/0/1]ip a 10.1.13.1 24
[SZ-Agg01-GigabitEthernet0/0/1]int lo 0
[SZ-Agg01-LoopBack0]ip a 1.1.1.1 32
{% endhighlight %}

### SZ-Agg02
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Agg02
[SZ-Agg02]int g0/0/0
[SZ-Agg02-GigabitEthernet0/0/0]ip a 10.1.2.2 24
[SZ-Agg02-GigabitEthernet0/0/0]int g0/0/1
[SZ-Agg02-GigabitEthernet0/0/1]ip a 10.1.23.2 24
[SZ-Agg02-GigabitEthernet0/0/1]int lo 0
[SZ-Agg02-LoopBack0]ip a 2.2.2.2 32
{% endhighlight %}

### SZ-Core01
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Core01
[SZ-Core01]int g0/0/0
[SZ-Core01-GigabitEthernet0/0/0]ip a 10.1.13.3 24
[SZ-Core01-GigabitEthernet0/0/0]int g0/0/1
[SZ-Core01-GigabitEthernet0/0/1]ip a 10.1.23.3 24
[SZ-Core01-GigabitEthernet0/0/1]int g0/0/2
[SZ-Core01-GigabitEthernet0/0/2]ip a 10.1.34.3 24
[SZ-Core01-GigabitEthernet0/0/2]int g4/0/0
[SZ-Core01-GigabitEthernet4/0/0]ip a 10.1.35.3 24
[SZ-Core01-GigabitEthernet4/0/0]int lo 0
[SZ-Core01-LoopBack0]ip a 3.3.3.3 32
{% endhighlight %}

### Sz-Edu-R1
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn Sz-Edu-R1
[Sz-Edu-R1]int g0/0/0
[Sz-Edu-R1-GigabitEthernet0/0/0]ip a 10.1.34.4 24
[Sz-Edu-R1-GigabitEthernet0/0/0]int g0/0/1
[Sz-Edu-R1-GigabitEthernet0/0/1]ip a 192.168.3.1 24
[Sz-Edu-R1-GigabitEthernet0/0/1]int lo 0
[Sz-Edu-R1-LoopBack0]ip a 4.4.4.4 32
{% endhighlight %}

### SZ-Edu-R2
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SZ-Edu-R2
[SZ-Edu-R2]int g0/0/0
[SZ-Edu-R2-GigabitEthernet0/0/0]ip a 10.1.35.5 24
[SZ-Edu-R2-GigabitEthernet0/0/0]int g0/0/1
[SZ-Edu-R2-GigabitEthernet0/0/1]ip a 192.168.3.2 24
[SZ-Edu-R2-GigabitEthernet0/0/1]int lo 0
[SZ-Edu-R2-LoopBack0]ip a 5.5.5.5 32
{% endhighlight %}


## 4、MSTP
### SZ-Acc01
{% highlight cli %}
[SZ-Acc01]stp en
[SZ-Acc01]stp m m
[SZ-Acc01]port-g g e 0/0/1 t e 0/0/2
[SZ-Acc01-port-group]stp e e
[SZ-Acc01-Ethernet0/0/1]stp e e
[SZ-Acc01-Ethernet0/0/2]stp e e
[SZ-Acc01-port-group]stp e e
[SZ-Acc01-Ethernet0/0/1]stp e e
[SZ-Acc01-Ethernet0/0/2]stp e e
[SZ-Acc01-port-group]stp e e
[SZ-Acc01-Ethernet0/0/1]stp e e
[SZ-Acc01-Ethernet0/0/2]stp e e
{% endhighlight %}


### SZ-Acc02
{% highlight cli %}
[SZ-Acc02]stp en
[SZ-Acc02]stp m m
[SZ-Acc02]stp re
[SZ-Acc02-mst-region]r huawei
[SZ-Acc02-mst-region]i 1 v 10
[SZ-Acc02-mst-region]i 2 v 20
[SZ-Acc02-mst-region]ac re
[SZ-Acc02-mst-region]stp i 1 r p
[SZ-Acc02]stp i 2 r s
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
[SZ-Acc03]stp en
[SZ-Acc03]stp m m
[SZ-Acc03]stp re
[SZ-Acc03-mst-region]r huawei
[SZ-Acc03-mst-region]i 1 v 10
[SZ-Acc03-mst-region]i 2 v 20
[SZ-Acc03-mst-region]ac re
[SZ-Acc03-mst-region]stp i 1 r s
[SZ-Acc03]stp i 2 r p
{% endhighlight %}


## 5、VRRP
### Sz-Edu-R1
{% highlight cli %}
[Sz-Edu-R1]int g0/0/1
[Sz-Edu-R1-GigabitEthernet0/0/1]vrrp vrid 03 v 192.168.3.254
[Sz-Edu-R1-GigabitEthernet0/0/1]vrrp vrid 03 pri 120
[Sz-Edu-R1-GigabitEthernet0/0/1]vrrp vrid 03 t i g 0/0/0 r 30
{% endhighlight %}

### SZ-Edu-R2
{% highlight cli %}
[SZ-Edu-R2]int g0/0/1
[SZ-Edu-R2-GigabitEthernet0/0/1]vrrp vrid 03 v 192.168.3.254
{% endhighlight %}

### SZ-Acc02
{% highlight cli %}
[SZ-Acc02]int v10
[SZ-Acc02-Vlanif10]vrrp vrid 1 v 192.168.1.254
[SZ-Acc02-Vlanif10]vrrp vrid 1 pri 120
[SZ-Acc02-Vlanif10]vrrp vrid 1 t i g 0/0/2 r 30
[SZ-Acc02-Vlanif10]int v20
[SZ-Acc02-Vlanif20]vrrp vrid 2 v 192.168.2.254
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
[SZ-Acc03]int v10
[SZ-Acc03-Vlanif10]vrrp vrid 1 v 192.168.1.254
[SZ-Acc03-Vlanif10]int v20
[SZ-Acc03-Vlanif20]vrrp vrid 2 v 192.168.2.254
[SZ-Acc03-Vlanif20]vrrp vrid 2 pri 120
[SZ-Acc03-Vlanif20]vrrp vrid 2 t i g 0/0/2 r 30
{% endhighlight %}


## 6、OSPF
### SZ-Acc02
{% highlight cli %}
[SZ-Acc02]int v100
[SZ-Acc02-Vlanif100]ospf authentication-mode md5 1 cipher huawei@123
[SZ-Acc02-Vlanif100]ospf
[SZ-Acc02-ospf-1]a 1
[SZ-Acc02-ospf-1-area-0.0.0.1]n 10.1.1.1 0.0.0.0
[SZ-Acc02-ospf-1-area-0.0.0.1]n 192.168.1.1 0.0.0.0
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
[SZ-Acc03]int v101
[SZ-Acc03-Vlanif101]ospf authentication-mode md5 1 cipher huawei@123
[SZ-Acc03-Vlanif101]ospf
[SZ-Acc03-ospf-1]a 1
[SZ-Acc03-ospf-1-area-0.0.0.1]n 10.1.2.1 0.0.0.0
[SZ-Acc03-ospf-1-area-0.0.0.1]n 192.168.2.1 0.0.0.0
{% endhighlight %}

### SZ-Agg01
{% highlight cli %}
[SZ-Agg01]int g0/0/0
[SZ-Agg01-GigabitEthernet0/0/0]ospf authentication-mode md5 1 cipher huawei@123
[SZ-Agg01-GigabitEthernet0/0/0]ospf router-id 1.1.1.1
[SZ-Agg01-ospf-1]a 0
[SZ-Agg01-ospf-1-area-0.0.0.0]authentication-mode md5 1 cipher huawei@123
[SZ-Agg01-ospf-1-area-0.0.0.0]n 1.1.1.1 0.0.0.0
[SZ-Agg01-ospf-1-area-0.0.0.0]n 10.1.13.1 0.0.0.0
[SZ-Agg01-ospf-1-area-0.0.0.0]a 1
[SZ-Agg01-ospf-1-area-0.0.0.1]n 10.1.1.2 0.0.0.0
{% endhighlight %}

### SZ-Agg02
{% highlight cli %}
[SZ-Agg02]int g0/0/0
[SZ-Agg02-GigabitEthernet0/0/0]ospf authentication-mode md5 1 cipher huawei@123
[SZ-Agg02-GigabitEthernet0/0/0]ospf router-id 2.2.2.2
[SZ-Agg02-ospf-1]a 0 
[SZ-Agg02-ospf-1-area-0.0.0.0]authentication-mode md5 1 cipher huawei@123
[SZ-Agg02-ospf-1-area-0.0.0.0]n 2.2.2.2 0.0.0.0
[SZ-Agg02-ospf-1-area-0.0.0.0]n 10.1.23.2 0.0.0.0
[SZ-Agg02-ospf-1-area-0.0.0.0]a 1
[SZ-Agg02-ospf-1-area-0.0.0.1]n 10.1.2.2 0.0.0.0
{% endhighlight %}

### SZ-Core01
{% highlight cli %}
[SZ-Core01]ospf router-id 3.3.3.3
[SZ-Core01-ospf-1]a 0
[SZ-Core01-ospf-1-area-0.0.0.0]authentication-mode md5 1 cipher huawei@123
[SZ-Core01-ospf-1-area-0.0.0.0]n 3.3.3.3 0.0.0.0
[SZ-Core01-ospf-1-area-0.0.0.0]n 10.1.13.3 0.0.0.0
[SZ-Core01-ospf-1-area-0.0.0.0]n 10.1.23.3 0.0.0.0
{% endhighlight %}


## 7、DHCP
### SZ-Acc02
{% highlight cli %}
[SZ-Acc02]dhcp e
[SZ-Acc02]ip p ACC02-DHCP
[SZ-Acc02-ip-pool-acc02-dhcp]n 192.168.1.0 m 24
[SZ-Acc02-ip-pool-acc02-dhcp]g 192.168.1.254
[SZ-Acc02-ip-pool-acc02-dhcp]e 192.168.1.1 192.168.1.100
[SZ-Acc02-ip-pool-acc02-dhcp]int v10
[SZ-Acc02-Vlanif10]dhcp se g
{% endhighlight %}

### SZ-Acc03
{% highlight cli %}
[SZ-Acc03]dhcp e
[SZ-Acc03]ip p ACC03-DHCP
[SZ-Acc03-ip-pool-acc03-dhcp]n 192.168.2.0 m 24
[SZ-Acc03-ip-pool-acc03-dhcp]g 192.168.2.254
[SZ-Acc03-ip-pool-acc03-dhcp]e 192.168.2.1 192.168.2.100
[SZ-Acc03-ip-pool-acc03-dhcp]int v20
[SZ-Acc03-Vlanif20]dhcp se g
{% endhighlight %}


## 8.1、出口设计
### SZ-Core01
{% highlight cli %}
[SZ-Core01]ip route-static 192.168.3.0 255.255.255.0 10.1.34.4 pre 50
[SZ-Core01]ip route-static 192.168.3.0 255.255.255.0 10.1.35.5
{% endhighlight %}

## 8.2
### Sz-Edu-R1
{% highlight cli %}
[Sz-Edu-R1]ip route-static 192.168.1.0 24 10.1.34.3
[Sz-Edu-R1]ip route-static 192.168.2.0 24 10.1.34.3
{% endhighlight %}

### SZ-Edu-R2
{% highlight cli %}
[SZ-Edu-R2]ip route-static 192.168.1.0 24 10.1.35.3
[SZ-Edu-R2]ip route-static 192.168.2.0 24 10.1.35.3
{% endhighlight %}


## 8.3
### SZ-Core01
{% highlight cli %}
[SZ-Core01]int g0/0/2
[SZ-Core01-GigabitEthernet0/0/2]ba 100
[SZ-Core01-GigabitEthernet0/0/2]int g4/0/0
[SZ-Core01-GigabitEthernet4/0/0]ba 50
{% endhighlight %}

### Sz-Edu-R1
{% highlight cli %}
[Sz-Edu-R1]int g0/0/0
[Sz-Edu-R1-GigabitEthernet0/0/0]ba 100
{% endhighlight %}

### SZ-Edu-R2
{% highlight cli %}
[SZ-Edu-R2]int g0/0/0
[SZ-Edu-R2-GigabitEthernet0/0/0]ba 50
{% endhighlight %}


## 9、路由互通
### SZ-Core01
{% highlight cli %}
[SZ-Core01-GigabitEthernet4/0/0]ospf 
[SZ-Core01-ospf-1]im s
{% endhighlight %}