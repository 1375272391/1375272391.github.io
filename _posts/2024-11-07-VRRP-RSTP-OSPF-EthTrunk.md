---
layout: post
title:  "作业12:1+X操作3"
author: XM137
date:   2024-11-02 00:30:00 +0800
categories: ENSP
tags: ENSP VRRP RSTP OSPF Eth-Trunk
---

## 拓扑 **[2215.zip](/assets/ENSP/20241107/2215.zip)**

---
## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |     192.168.10.1   |   255.255.255.0    |   192.168.10.254   |
|    PC_2     |     192.168.10.2   |   255.255.255.0    |   192.168.10.254   |
|    PC_3     |     192.168.20.1   |   255.255.255.0    |   192.168.20.254   |
|    PC_4     |     192.168.4.1    |   255.255.255.0    |   192.168.4.254    |
|    PC_5     |     192.168.5.1    |   255.255.255.0    |   192.168.5.254    |


---

## 1-3.设备命名 链路聚合 VLAN

### HZ-HZXiaoYuan-Agg01-S5731
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Agg01-S5731
[HZ-HZXiaoYuan-Agg01-S5731]v b 10 20 100
[HZ-HZXiaoYuan-Agg01-S5731]int g0/0/1
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/1]port l t
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/1]port t a v 10 20
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/1]port t p v 1
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/1]int g0/0/3
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/3]port l t
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/3]port t a v 10 20
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/3]int g0/0/24
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/24]port l a
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/24]port d v 100
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/24]int e1
[HZ-HZXiaoYuan-Agg01-S5731-Eth-Trunk1]tr g 0/0/21 t 0/0/23
[HZ-HZXiaoYuan-Agg01-S5731-Eth-Trunk1]port l t
[HZ-HZXiaoYuan-Agg01-S5731-Eth-Trunk1]port t a v 10 20
[HZ-HZXiaoYuan-Agg01-S5731-Eth-Trunk1]port t p v 1
{% endhighlight %}

### HZ-HZXiaoYuan-Agg02-S5731
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Agg02-S5731
[HZ-HZXiaoYuan-Agg02-S5731]v b 10 20 101
[HZ-HZXiaoYuan-Agg02-S5731]int g0/0/2
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/2]port l t
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/2]port t a v 10 20
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/2]port t p v 1
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/2]int g0/0/4
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/4]port l t
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/4]port t a v 10 20
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/4]port t p v 1
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/4]int g0/0/24
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/24]port l a
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/24]port d v 101
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/24]int e1
[HZ-HZXiaoYuan-Agg02-S5731-Eth-Trunk1]tr g 0/0/21 t 0/0/23
[HZ-HZXiaoYuan-Agg02-S5731-Eth-Trunk1]port l t
[HZ-HZXiaoYuan-Agg02-S5731-Eth-Trunk1]port t a v 10 20
[HZ-HZXiaoYuan-Agg02-S5731-Eth-Trunk1]port t p v 1
{% endhighlight %}

### HZ-HZXiaoYuan-Acc01-S5731
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Acc01-S5731
[HZ-HZXiaoYuan-Acc01-S5731]v b 10 20 
[HZ-HZXiaoYuan-Acc01-S5731]int g0/0/3
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]port l t
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]port t a v 10 20
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]port t p v 1
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]int g0/0/4
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/4]port l t
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/4]port t a v 10 20
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/4]port t p v 1
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/4]int g0/0/24
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/24]port l h
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/24]port h p v 20
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/24]port h u v 20
{% endhighlight %}

### HZ-HZXiaoYuan-Acc02-S5731
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Acc02-S5731
[HZ-HZXiaoYuan-Acc02-S5731]v b 10 20
[HZ-HZXiaoYuan-Acc02-S5731]int g0/0/1
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/1]port l t
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/1]port t a v 10 20
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/1]port t p v 1
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/1]int g0/0/2
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]port l t
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]port t a v 10 20
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]port t p v 1
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]int g0/0/23
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/23]port l a
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/23]port d v 10
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/23]int g0/0/24
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/24]port l a
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/24]port d v 10
{% endhighlight %}

---

## 3.IP编址
### HZ-HZXiaoYuan-Edge01-AR6140
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Edge01-AR6140
[HZ-HZXiaoYuan-Edge01-AR6140]int g0/0/0
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/0]ip a 10.1.12.1 24
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/0]int g0/0/1
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/1]ip a 10.1.13.1 24
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/1]int g0/0/2
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/2]ip a 10.1.15.1 24
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/2]int s4/0/0
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]ip a 10.2.14.1 24
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]int lo 0
[HZ-HZXiaoYuan-Edge01-AR6140-LoopBack0]ip a 10.1.1.1 32
{% endhighlight %}

#### HZ-HZXiaoYuan-Core01-AR6140
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZXiaoYuan-Core01-AR6140
[HZ-HZXiaoYuan-Core01-AR6140]int g0/0/0
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/0]ip a 10.1.12.2 24
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/0]int g0/0/1
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/1]ip a 10.1.26.2 24
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/1]int g0/0/2
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/2]ip a 10.1.23.2 24
[HZ-HZXiaoYuan-Core01-AR6140-GigabitEthernet0/0/2]int lo 0
[HZ-HZXiaoYuan-Core01-AR6140-LoopBack0]ip a 10.1.2.2 32
{% endhighlight %}

### HZ-HZXiaoYuan-Core02-AR6140
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn HZ-HZXiaoYuan-Core02-AR6140
[HZ-HZXiaoYuan-Core02-AR6140]int g0/0/0
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/0]ip a 10.1.37.3 24
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/0]int g0/0/1
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/1]ip a 10.1.13.3 24
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/1]int g0/0/2
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/2]ip a 10.1.23.3 24
[HZ-HZXiaoYuan-Core02-AR6140-GigabitEthernet0/0/2]int lo 0
[HZ-HZXiaoYuan-Core02-AR6140-LoopBack0]ip a 10.1.3.3 32
{% endhighlight %}

### HZ-HZEDU-Edge01-AR6140
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn HZ-HZEDU-Edge01-AR6140
[HZ-HZEDU-Edge01-AR6140]int g0/0/0
[HZ-HZEDU-Edge01-AR6140-GigabitEthernet0/0/0]ip a 192.168.4.254 24
[HZ-HZEDU-Edge01-AR6140-GigabitEthernet0/0/0]int s4/0/0
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]ip a 10.2.14.4 24
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]int lo 0
[HZ-HZEDU-Edge01-AR6140-LoopBack0]ip a 10.1.4.4 32
{% endhighlight %}

### SH-SHXiaoYuan-Edge01-AR6140
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SH-SHXiaoYuan-Edge01-AR6140
[SH-SHXiaoYuan-Edge01-AR6140]int g0/0/0
[SH-SHXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/0]ip a 10.1.15.5 24
[SH-SHXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/0]int g0/0/1
[SH-SHXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/1]ip a 192.168.5.254 24
[SH-SHXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/1]int lo 0
[SH-SHXiaoYuan-Edge01-AR6140-LoopBack0]ip a 10.1.5.5 32
{% endhighlight %}


### HZ-HZXiaoYuan-Agg01
{% highlight cli %}
[HZ-HZXiaoYuan-Agg01-S5731]int v10
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]ip a 192.168.10.100 24
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]int v20
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif20]ip a 192.168.20.101 24
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif20]int v100
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif100]ip a 10.1.26.6 24
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif100]int lo 0
[HZ-HZXiaoYuan-Agg01-S5731-LoopBack0]ip a 10.1.6.6 32
{% endhighlight %}

### HZ-HZXiaoYuan-Agg02
{% highlight cli %}
[HZ-HZXiaoYuan-Agg02-S5731]int v10
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]ip a 192.168.10.101 24
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]int v20
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]ip a 192.168.20.100 24
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]int v101
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif101]ip a 10.1.37.7 24
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif101]int lo 0
[HZ-HZXiaoYuan-Agg02-S5731-LoopBack0]ip a 10.1.7.7 32
{% endhighlight %}

---

## 5.RSTP
### HZ-HZXiaoYuan-Agg01
{% highlight cli %}
[HZ-HZXiaoYuan-Agg01-S5731]stp m r
[HZ-HZXiaoYuan-Agg01-S5731]stp r p
[HZ-HZXiaoYuan-Agg01-S5731]int g0/0/3
[HZ-HZXiaoYuan-Agg01-S5731-GigabitEthernet0/0/3]stp cost 200000
{% endhighlight %}

### HZ-HZXiaoYuan-Agg02
{% highlight cli %}
[HZ-HZXiaoYuan-Agg02-S5731]stp ena
[HZ-HZXiaoYuan-Agg02-S5731]stp m r
[HZ-HZXiaoYuan-Agg02-S5731]stp r s
[HZ-HZXiaoYuan-Agg02-S5731]int g0/0/2
[HZ-HZXiaoYuan-Agg02-S5731-GigabitEthernet0/0/2]stp cos 200000
{% endhighlight %}


### HZ-HZXiaoYuan-Acc02
{% highlight cli %}
[HZ-HZXiaoYuan-Acc02-S5731]stp ena
[HZ-HZXiaoYuan-Acc02-S5731]stp m r
[HZ-HZXiaoYuan-Acc02-S5731]int g0/0/2
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]stp cos 200000
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/2]int g0/0/23
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/23]stp e e
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/23]int g0/0/24
[HZ-HZXiaoYuan-Acc02-S5731-GigabitEthernet0/0/24]stp e e
{% endhighlight %}

### HZ-HZXiaoYuan-Acc01-S5731
{% highlight cli %}
[HZ-HZXiaoYuan-Acc01-S5731]stp ena
[HZ-HZXiaoYuan-Acc01-S5731]stp m r
[HZ-HZXiaoYuan-Acc01-S5731]int g0/0/3
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]stp cos 200000
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/3]int g0/0/24
[HZ-HZXiaoYuan-Acc01-S5731-GigabitEthernet0/0/24]stp e e
{% endhighlight %}

---

## 6.VRRP 
### HZ-HZXiaoYuan-Agg01-S5731
{% highlight cli %}
[HZ-HZXiaoYuan-Agg01-S5731]int v10
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]vrrp vrid 1 v 192.168.10.254
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]vrrp vrid 1 pri 120
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]vrrp vrid 1 p t d 3
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]vrrp vrid 1 t i g 0/0/24 r 30
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif10]int v20
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif20]vrrp vrid 2 v 192.168.20.254
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif20]vrrp vrid 2 p t d 3
[HZ-HZXiaoYuan-Agg01-S5731-Vlanif20]vrrp vrid 2 t i g 0/0/24 r 30
{% endhighlight %}

### HZ-HZXiaoYuan-Agg02-S5731
{% highlight cli %}
[HZ-HZXiaoYuan-Agg02-S5731]int v10
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]vrrp vrid 1 v 192.168.10.254
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]vrrp vrid 1 p t d 3
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]vrrp vrid 1 t i g 0/0/24 r 30
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif10]int v20
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]vrrp vrid 2 v 192.168.20.254
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]vrrp vrid 2 pri 120
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]vrrp vrid 2 p t d 3
[HZ-HZXiaoYuan-Agg02-S5731-Vlanif20]vrrp vrid 2 t i g 0/0/24 r 30
{% endhighlight %}

----

## 7.OSPF
### HZ-HZXiaoYuan-Agg01-S5731
{% highlight cli %}
[HZ-HZXiaoYuan-Agg01-S5731]ospf router-id 10.1.6.6
[HZ-HZXiaoYuan-Agg01-S5731-ospf-1]a 0
[HZ-HZXiaoYuan-Agg01-S5731-ospf-1-area-0.0.0.0]n 192.168.10.100 0.0.0.0
[HZ-HZXiaoYuan-Agg01-S5731-ospf-1-area-0.0.0.0]n 192.168.20.101 0.0.0.0
[HZ-HZXiaoYuan-Agg01-S5731-ospf-1-area-0.0.0.0]n 10.1.26.6 0.0.0.0
[HZ-HZXiaoYuan-Agg01-S5731-ospf-1-area-0.0.0.0]n 10.1.6.6 0.0.0.0
{% endhighlight %}

### HZ-HZXiaoYuan-Agg02-S5731
{% highlight cli %}
[HZ-HZXiaoYuan-Agg02-S5731]ospf router-id 10.1.7.7
[HZ-HZXiaoYuan-Agg02-S5731-ospf-1]a 0
[HZ-HZXiaoYuan-Agg02-S5731-ospf-1-area-0.0.0.0]n 192.168.10.101 0.0.0.0
[HZ-HZXiaoYuan-Agg02-S5731-ospf-1-area-0.0.0.0]n 192.168.20.100 0.0.0.0
[HZ-HZXiaoYuan-Agg02-S5731-ospf-1-area-0.0.0.0]n 10.1.37.7 0.0.0.0
[HZ-HZXiaoYuan-Agg02-S5731-ospf-1-area-0.0.0.0]n 10.1.7.7 0.0.0.0
{% endhighlight %}


### HZ-HZXiaoYuan-Core01-AR6140
{% highlight cli %}
[HZ-HZXiaoYuan-Core01-AR6140]ospf router-id 10.1.2.2
[HZ-HZXiaoYuan-Core01-AR6140-ospf-1]a 0
[HZ-HZXiaoYuan-Core01-AR6140-ospf-1-area-0.0.0.0]n 10.1.12.2 0.0.0.0
[HZ-HZXiaoYuan-Core01-AR6140-ospf-1-area-0.0.0.0]n 10.1.26.2 0.0.0.0
[HZ-HZXiaoYuan-Core01-AR6140-ospf-1-area-0.0.0.0]n 10.1.23.2 0.0.0.0
[HZ-HZXiaoYuan-Core01-AR6140-ospf-1-area-0.0.0.0]n 10.1.2.2 0.0.0.0
{% endhighlight %}

### HZ-HZXiaoYuan-Core02-AR6140
{% highlight cli %}
[HZ-HZXiaoYuan-Core02-AR6140]ospf router-id 10.1.3.3
[HZ-HZXiaoYuan-Core02-AR6140-ospf-1]a 0
[HZ-HZXiaoYuan-Core02-AR6140-ospf-1-area-0.0.0.0]n 10.1.37.3 0.0.0.0
[HZ-HZXiaoYuan-Core02-AR6140-ospf-1-area-0.0.0.0]n 10.1.13.3 0.0.0.0
[HZ-HZXiaoYuan-Core02-AR6140-ospf-1-area-0.0.0.0]n 10.1.23.3 0.0.0.0
[HZ-HZXiaoYuan-Core02-AR6140-ospf-1-area-0.0.0.0]n 10.1.3.3 0.0.0.0
{% endhighlight %}

### HZ-HZXiaoYuan-Edge01-AR6140
{% highlight cli %}
[HZ-HZXiaoYuan-Edge01-AR6140]ospf router-id 10.1.1.1
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1]a 0
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.0]n 10.1.12.1 0.0.0.0
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.0]n 10.1.13.1 0.0.0.0
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.0]n 10.1.1.1 0.0.0.0
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.0]q
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1]a 1
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.1]n 10.1.15.1 0.0.0.0
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.1]int g0/0/0
[HZ-HZXiaoYuan-Edge01-AR6140-GigabitEthernet0/0/0]ospf d 255
{% endhighlight %}

### SH-SHXiaoYuan-Edge01-AR6140
{% highlight cli %}
[SH-SHXiaoYuan-Edge01-AR6140]ospf router-id 10.1.5.5 
[SH-SHXiaoYuan-Edge01-AR6140-ospf-1]a 1
[SH-SHXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.1]n 10.1.15.5 0.0.0.0
[SH-SHXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.1]n 192.168.5.254 0.0.0.0
[SH-SHXiaoYuan-Edge01-AR6140-ospf-1-area-0.0.0.1]n 10.1.5.5 0.0.0.0
{% endhighlight %}

---

## 8.1 出口设计

### HZ-HZEDU-Edge01-AR6140
{% highlight cli %}
[HZ-HZEDU-Edge01-AR6140]aaa
[HZ-HZEDU-Edge01-AR6140-aaa]local-user huawei password cipher Huawei123
[HZ-HZEDU-Edge01-AR6140-aaa]local-user huawei service-type ppp
[HZ-HZEDU-Edge01-AR6140-aaa]int s4/0/0
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]link-protocol ppp
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]ppp authentication-mode chap 
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]shu
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]un shu
{% endhighlight %}

### HZ-HZXiaoYuan-Edge01-AR6140
{% highlight cli %}
[HZ-HZXiaoYuan-Edge01-AR6140]int s4/0/0
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]ppp chap user huawei
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]ppp chap p c Huawei123
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]shu
[HZ-HZXiaoYuan-Edge01-AR6140-Serial4/0/0]un shu
{% endhighlight %}

## 8.2
### HZ-HZXiaoYuan-Edge01-AR6140
```CLI
[HZ-HZXiaoYuan-Edge01-AR6140]ip route-static 192.168.4.0 24 s4/0/0
```
#### HZ-HZEDU-Edge01-AR6140
```CLI
[HZ-HZEDU-Edge01-AR6140-Serial4/0/0]ip route-static 0.0.0.0 0 s4/0/0
```

---

## 9. 路由引入
### HZ-HZXiaoYuan-Edge01-AR6140
{% highlight cli %}
[HZ-HZXiaoYuan-Edge01-AR6140]ospf
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1]
[HZ-HZXiaoYuan-Edge01-AR6140-ospf-1]import-route static ty 1
{% endhighlight %}



