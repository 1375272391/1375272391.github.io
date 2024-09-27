---
layout: post
title:  "作业6：VRRP+RSTP"
author: XM137
date:   2024-09-28 00:40:00 +0800
categories: ENSP
tags: ENSP RSTP VRRP OSPF
---

>
> 具体要求拓扑以学习通为准
>


## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |    192.168.10.10   |   255.255.255.0    |   192.168.10.254   |
|    PC_2     |    192.168.20.20   |   255.255.255.0    |   192.168.20.254   |
|    PC_3     |     17.16.50.10    |   255.255.255.0    |     17.16.50.1     |


### R1
{% highlight cli %}
<Huawei>sy
[Huawei]sysn R1
[R1]undo in e
[R1]int loo 0
[R1-LoopBack0]ip a 10.1.1.1 32
[R1-LoopBack0]int g0/0/2
[R1-GigabitEthernet0/0/2]ip a 17.16.50.1 24
[R1-GigabitEthernet0/0/2]int g0/0/0
[R1-GigabitEthernet0/0/0]ip a 10.1.20.1 24
[R1-GigabitEthernet0/0/0]int g0/0/1
[R1-GigabitEthernet0/0/1]ip a 10.1.26.1 24
[R1-GigabitEthernet0/0/1]ospf
[R1-ospf-1]a 0
[R1-ospf-1-area-0.0.0.0]n 10.1.1.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]n 10.1.20.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]n 10.1.26.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]q
[R1-ospf-1]i d
{% endhighlight %}


### Agg01
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn Agg01
[Agg01]int loop 0
[Agg01-LoopBack0]ip a 10.2.2.2 32
[Agg01-LoopBack0]stp ena
[Agg01]stp m r
[Agg01]stp root primary 
[Agg01]v b 10 20 30
[Agg01]int v 10
[Agg01-Vlanif10]ip a 192.168.10.252 24
[Agg01-Vlanif10]vrrp vrid 1 vi 192.168.10.254 
[Agg01-Vlanif10]vrrp vrid 1 pri 120
[Agg01-Vlanif10]vrrp vrid 1 pre t d 5
[Agg01-Vlanif10]vrrp vrid 1 track interface g 0/0/8 r 30
[Agg01-Vlanif10]int v 20
[Agg01-Vlanif20]ip a 192.168.20.252 24
[Agg01-Vlanif20]vrrp vrid 2 vi 192.168.20.254 
[Agg01-Vlanif20]vrrp vrid 2 pre t d 5
[Agg01-Vlanif20]vrrp vrid 2 track interface g 0/0/8 r 30
[Agg01-Vlanif20]int v 30
[Agg01-Vlanif30]ip a 10.1.20.2 24
[Agg01-Vlanif30]port-g 1
[Agg01-port-group-1]gr g 0/0/1 t g 0/0/7
[Agg01-port-group-1]port l t
[Agg01-port-group-1]port t a v 10 20 
[Agg01-port-group-1]qu
[Agg01]int g0/0/8
[Agg01-GigabitEthernet0/0/8]port link-t a
[Agg01-GigabitEthernet0/0/8]port d v 30
[Agg01-GigabitEthernet0/0/8]ospf
[Agg01-ospf-1]a 0
[Agg01-ospf-1-area-0.0.0.0]n 10.1.20.2 0.0.0.0
[Agg01-ospf-1-area-0.0.0.0]n 10.2.2.2 0.0.0.0
[Agg01-ospf-1-area-0.0.0.0]q
[Agg01-ospf-1]i d
{% endhighlight %}


### Agg02
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn Agg02
[Agg02]int loo 0
[Agg02-LoopBack0]ip a 10.3.3.3 32
[Agg02-LoopBack0]stp ena
[Agg02]stp m r
[Agg02]stp root s
[Agg02]v b 10 20 30
[Agg02]int v 10
[Agg02-Vlanif10]ip a 192.168.10.253 24
[Agg02-Vlanif10]vrrp vrid 1 virtual-ip 192.168.10.254
[Agg02-Vlanif10]vrrp vrid 1 preempt-mode t d 5
[Agg02-Vlanif10]vrrp vrid 1 track i g 0/0/8 r 30
[Agg02-Vlanif10]int v 20
[Agg02-Vlanif20]ip a 192.168.20.253 24
[Agg02-Vlanif20]vrrp vrid 2 virtual-ip 192.168.20.254
[Agg02-Vlanif20]vrrp vrid 2 pri 120
[Agg02-Vlanif20]vrrp vrid 2 pree t d 5
[Agg02-Vlanif20]vrrp vrid 2 track i g 0/0/8 r 30 
[Agg02-Vlanif20]int v 30
[Agg02-Vlanif30]ip a 10.1.26.2 24
[Agg02-Vlanif30]port-g 1
[Agg02-port-group-1]gr g 0/0/1 t g 0/0/7
[Agg02-port-group-1]port l t
[Agg02-port-group-1]port t a v 10 20
[Agg02-port-group-1]qu
[Agg02]int g0/0/8
[Agg02-GigabitEthernet0/0/8]port link-t a
[Agg02-GigabitEthernet0/0/8]port d v 30
[Agg02-GigabitEthernet0/0/8]ospf
[Agg02-ospf-1]a 0
[Agg02-ospf-1-area-0.0.0.0]n 10.3.3.3 0.0.0.0
[Agg02-ospf-1-area-0.0.0.0]n 10.1.26.2 0.0.0.0
[Agg02-ospf-1-area-0.0.0.0]q
[Agg02-ospf-1]i d
{% endhighlight %}


### Acc01
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn Acc01
[Acc01]stp ena
[Acc01]stp m r
[Acc01]stp roo p
[Acc01]stp bpdu-protection 
[Acc01]v b 10 20
[Acc01]port-g 1
[Acc01-port-group-1]gr g0/0/6 t g 0/0/7
[Acc01-port-group-1]port l t
[Acc01-port-group-1]port t a v 10 20
[Acc01-port-group-1]qu
[Acc01]int g0/0/1
[Acc01-GigabitEthernet0/0/1]port link-t a
[Acc01-GigabitEthernet0/0/1]port d v 10
[Acc01-GigabitEthernet0/0/1]stp e e
{% endhighlight %}


### Acc02
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn Acc02
[Acc02]stp ena
[Acc02]stp m r
[Acc02]stp roo s
[Acc02]stp bpdu-protection 
[Acc02]v b 10 20
[Acc02]port-g 1
[Acc02-port-group-1]gr g 0/0/6 t g 0/0/7
[Acc02-port-group-1]port l t
[Acc02-port-group-1]port t a v 10 20
[Acc02-port-group-1]qu
[Acc02]int g0/0/1
[Acc02-GigabitEthernet0/0/1]port link-t a
[Acc02-GigabitEthernet0/0/1]port d v 20
[Acc02-GigabitEthernet0/0/1]stp e e
{% endhighlight %}


### ping 测试

### PC 3 --> PC 1 关闭 AGG01 监视口
{% highlight cli %}
ping 192.168.10.10 -t
## ping测试连通后，关闭Agg01 监视口 G0/0/8 
[Agg01]interface GigabitEthernet 0/0/8
[Agg01-GigabitEthernet0/0/8]shut
等待重新连通，可能耗时较长
重新连通后，重新打开端口，进行下一轮测试
[Agg01-GigabitEthernet0/0/8]undo shut
{% endhighlight %}



### PC3 --> PC 2 关闭 AGG02 监视口
{% highlight cli %}
ping 192.168.20.20 -t
## ping测试连通后，关闭Agg02 监视口 G0/0/8 
[Agg02]interface GigabitEthernet 0/0/8
[Agg02-GigabitEthernet0/0/8]shut
## 等待重新连通，可能耗时较长
{% endhighlight %}
