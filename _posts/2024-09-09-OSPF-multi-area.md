---
layout: post
title:  "作业3：ospf多区域实验"
author: XM137
date:   2024-09-09 13:42:00 +0800
categories: ENSP
tags: ENSP OSPF Multi-Area
---

>
> 按照拓扑要求完成作业。上传作业完成全过程视频。
> 

### 关闭信息中心后将不会收到OSPF相关信息
### 请根据实际情况决定是否需要关闭信息中心


## HZ-xiaoyuan-Edge01
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Edge01-ZXB
[HZ-xiaoyuan-Edge01-ZXB]i loop 0
[HZ-xiaoyuan-Edge01-ZXB-LoopBack0]ip a 10.1.1.1 32
[HZ-xiaoyuan-Edge01-ZXB-LoopBack0]in g0/0/0
[HZ-xiaoyuan-Edge01-ZXB-GigabitEthernet0/0/0]ip a 10.1.12.1 24
[HZ-xiaoyuan-Edge01-ZXB-GigabitEthernet0/0/0]int g0/0/1
[HZ-xiaoyuan-Edge01-ZXB-GigabitEthernet0/0/1]ip a 10.1.13.1 24
[HZ-xiaoyuan-Edge01-ZXB-GigabitEthernet0/0/1]int e0/0/0
[HZ-xiaoyuan-Edge01-ZXB-Ethernet0/0/0]ip a 10.1.15.1 24
[HZ-xiaoyuan-Edge01-ZXB-Ethernet0/0/0]ospf
[HZ-xiaoyuan-Edge01-ZXB-ospf-1]a 0
[HZ-xiaoyuan-Edge01-ZXB-ospf-1-area-0.0.0.0]n 10.1.12.0 0.0.0.255
[HZ-xiaoyuan-Edge01-ZXB-ospf-1-area-0.0.0.0]n 10.1.13.0 0.0.0.255
[HZ-xiaoyuan-Edge01-ZXB-ospf-1-area-0.0.0.0]n 10.1.1.1 0.0.0.0
[HZ-xiaoyuan-Edge01-ZXB-ospf-1-area-0.0.0.0]q
[HZ-xiaoyuan-Edge01-ZXB-ospf-1]i d
{% endhighlight %}

## HZ-xiaoyuan-Core01
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Core01-ZXB
[HZ-xiaoyuan-Core01-ZXB]i loop 0
[HZ-xiaoyuan-Core01-ZXB-LoopBack0]ip a 10.1.2.2 32
[HZ-xiaoyuan-Core01-ZXB-LoopBack0]in g0/0/0
[HZ-xiaoyuan-Core01-ZXB-GigabitEthernet0/0/0]ip a 10.1.12.2 24
[HZ-xiaoyuan-Core01-ZXB-GigabitEthernet0/0/0]int g0/0/1
[HZ-xiaoyuan-Core01-ZXB-GigabitEthernet0/0/1]ip a 10.1.26.2 24
[HZ-xiaoyuan-Core01-ZXB-GigabitEthernet0/0/1]ospf
[HZ-xiaoyuan-Core01-ZXB-ospf-1]a 0
[HZ-xiaoyuan-Core01-ZXB-ospf-1-area-0.0.0.0]n 10.1.2.2 0.0.0.0
[HZ-xiaoyuan-Core01-ZXB-ospf-1-area-0.0.0.0]n 10.1.12.0 0.0.0.255
[HZ-xiaoyuan-Core01-ZXB-ospf-1-area-0.0.0.0]q
[HZ-xiaoyuan-Core01-ZXB-ospf-1]a 100
[HZ-xiaoyuan-Core01-ZXB-ospf-1-area-0.0.0.100]n 10.1.26.0 0.0.0.255
{% endhighlight %}

## HZ-xiaoyuan-Agg01
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Agg01-ZXB
[HZ-xiaoyuan-Agg01-ZXB]v b 10 20 100
[HZ-xiaoyuan-Agg01-ZXB]i loop 0
[HZ-xiaoyuan-Agg01-ZXB-LoopBack0]ip a 10.1.6.6 32
[HZ-xiaoyuan-Agg01-ZXB-LoopBack0]int v 10
[HZ-xiaoyuan-Agg01-ZXB-Vlanif10]ip a 192.168.10.1 24
[HZ-xiaoyuan-Agg01-ZXB-Vlanif10]int v 20
[HZ-xiaoyuan-Agg01-ZXB-Vlanif20]ip a 192.168.20.1 24
[HZ-xiaoyuan-Agg01-ZXB-Vlanif20]int v 100
[HZ-xiaoyuan-Agg01-ZXB-Vlanif100]ip a 10.1.26.10 24
[HZ-xiaoyuan-Agg01-ZXB-Vlanif100]int g0/0/24
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/24]port link-t a
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/24]port d v 100
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/24]int g0/0/1
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/1]port link-t t
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/1]port tr a v 10 20
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/1]port tr p v 1
[HZ-xiaoyuan-Agg01-ZXB-GigabitEthernet0/0/1]ospf
[HZ-xiaoyuan-Agg01-ZXB-ospf-1]a 100
[HZ-xiaoyuan-Agg01-ZXB-ospf-1-area-0.0.0.100]n 10.1.6.6 0.0.0.0
[HZ-xiaoyuan-Agg01-ZXB-ospf-1-area-0.0.0.100]n 10.1.26.0 0.0.0.255
[HZ-xiaoyuan-Agg01-ZXB-ospf-1-area-0.0.0.100]q
[HZ-xiaoyuan-Agg01-ZXB-ospf-1]i d
{% endhighlight %}

## HZ-xiaoyuan-Acc02
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Acc02-ZXB
[HZ-xiaoyuan-Acc02-ZXB]v b 10 20
[HZ-xiaoyuan-Acc02-ZXB]i g0/0/1
[HZ-xiaoyuan-Acc02-ZXB-GigabitEthernet0/0/1]port link-t t
[HZ-xiaoyuan-Acc02-ZXB-GigabitEthernet0/0/1]port tr a v 10 20
[HZ-xiaoyuan-Acc02-ZXB-GigabitEthernet0/0/1]port tr p v 1
[HZ-xiaoyuan-Acc02-ZXB-GigabitEthernet0/0/1]int e0/0/1
[HZ-xiaoyuan-Acc02-ZXB-Ethernet0/0/1]port link-t a
[HZ-xiaoyuan-Acc02-ZXB-Ethernet0/0/1]port d v 10
[HZ-xiaoyuan-Acc02-ZXB-Ethernet0/0/1]int e0/0/2
[HZ-xiaoyuan-Acc02-ZXB-Ethernet0/0/2]port link-t a
[HZ-xiaoyuan-Acc02-ZXB-Ethernet0/0/2]port d v 20
{% endhighlight %}

## HZ-xiaoyuan-Core02
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Core02-ZXB
[HZ-xiaoyuan-Core02-ZXB]int loop 0
[HZ-xiaoyuan-Core02-ZXB-LoopBack0]ip a 10.1.3.3 32
[HZ-xiaoyuan-Core02-ZXB-LoopBack0]in g0/0/0
[HZ-xiaoyuan-Core02-ZXB-GigabitEthernet0/0/0]ip a 10.1.37.3 24
[HZ-xiaoyuan-Core02-ZXB-GigabitEthernet0/0/0]int g0/0/1
[HZ-xiaoyuan-Core02-ZXB-GigabitEthernet0/0/1]ip a 10.1.13.2 24
[HZ-xiaoyuan-Core02-ZXB-GigabitEthernet0/0/1]ospf
[HZ-xiaoyuan-Core02-ZXB-ospf-1]a 0
[HZ-xiaoyuan-Core02-ZXB-ospf-1-area-0.0.0.0]n 10.1.3.3 0.0.0.0
[HZ-xiaoyuan-Core02-ZXB-ospf-1-area-0.0.0.0]n 10.1.13.2 0.0.0.255
[HZ-xiaoyuan-Core02-ZXB-ospf-1-area-0.0.0.0]q
[HZ-xiaoyuan-Core02-ZXB-ospf-1]a 100
[HZ-xiaoyuan-Core02-ZXB-ospf-1-area-0.0.0.100]n 10.1.37.3 0.0.0.255
{% endhighlight %}

## HZ-xiaoyuan-Agg02
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Agg02-ZXB
[HZ-xiaoyuan-Agg02-ZXB]v b 30 101
[HZ-xiaoyuan-Agg02-ZXB]i loop 0
[HZ-xiaoyuan-Agg02-ZXB-LoopBack0]ip a 10.1.7.7 32
[HZ-xiaoyuan-Agg02-ZXB-LoopBack0]int v 30
[HZ-xiaoyuan-Agg02-ZXB-Vlanif30]ip a 192.168.30.1 24
[HZ-xiaoyuan-Agg02-ZXB-Vlanif30]int v 101
[HZ-xiaoyuan-Agg02-ZXB-Vlanif101]ip a 10.1.37.10 24
[HZ-xiaoyuan-Agg02-ZXB-Vlanif101]int g0/0/1
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/1]port link-t a
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/1]port d v 30
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/1]int g0/0/24
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/24]port link-t a
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/24]port d v 101
[HZ-xiaoyuan-Agg02-ZXB-GigabitEthernet0/0/24]ospf
[HZ-xiaoyuan-Agg02-ZXB-ospf-1]a 100
[HZ-xiaoyuan-Agg02-ZXB-ospf-1-area-0.0.0.100]n 10.1.7.7 0.0.0.0
[HZ-xiaoyuan-Agg02-ZXB-ospf-1-area-0.0.0.100]n 10.1.37.0 0.0.0.255
[HZ-xiaoyuan-Agg02-ZXB-ospf-1-area-0.0.0.100]q
[HZ-xiaoyuan-Agg02-ZXB-ospf-1]i d
{% endhighlight %}

## HZ-xiaoyuan-Acc01
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn HZ-xiaoyuan-Acc01-ZXB
[HZ-xiaoyuan-Acc01-ZXB]v 30
[HZ-xiaoyuan-Acc01-ZXB-vlan30]int g0/0/1
[HZ-xiaoyuan-Acc01-ZXB-GigabitEthernet0/0/1]port link-t a
[HZ-xiaoyuan-Acc01-ZXB-GigabitEthernet0/0/1]port d v 30
[HZ-xiaoyuan-Acc01-ZXB-GigabitEthernet0/0/1]int e0/0/1
[HZ-xiaoyuan-Acc01-ZXB-Ethernet0/0/1]port link-t a
[HZ-xiaoyuan-Acc01-ZXB-Ethernet0/0/1]port d v 30
{% endhighlight %}


## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |    192.168.10.2    |   255.255.255.0    |    192.168.10.1    |
|    PC_2     |    192.168.20.2    |   255.255.255.0    |    192.168.20.1    |
|    PC_3     |    192.168.30.3    |   255.255.255.0    |    192.168.30.1    |
|    PC_4     |     10.1.15.10     |   255.255.255.0    |     10.1.15.1      |

## PC ping
## PC 4
{% highlight cli %}
ping 192.168.10.2
ping 192.168.20.2
ping 192.168.30.3
ping 10.1.15.10
{% endhighlight %}