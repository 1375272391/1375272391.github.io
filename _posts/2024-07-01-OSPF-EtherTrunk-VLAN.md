---
layout: post
title:  "复习3：OSPF+链路聚合+VLAN综合"
author: XM137
date:   2024-07-01 19:51:00 +0800
categories: H3C
tags: HCL H3C OSPF EtherTrunk VLAN Review
---
### 注意： 按照学习通拓扑连线
### 仅供复习参考

## SW3
{% highlight cli %}
<H3C>sy
[H3C]sysn SW3-ZXB
[SW3-ZXB]v 10
[SW3-ZXB-vlan10]port g1/0/3
[SW3-ZXB-vlan10]v 13
[SW3-ZXB-vlan13]port g1/0/1
[SW3-ZXB-vlan13]int v 10
[SW3-ZXB-Vlan-interface10]ip a 192.168.10.1 24
[SW3-ZXB-Vlan-interface10]int v 13
[SW3-ZXB-Vlan-interface13]ip a 22.12.30.5 24
[SW3-ZXB-Vlan-interface13]ospf
[SW3-ZXB-ospf-1]a 0
[SW3-ZXB-ospf-1-area-0.0.0.0]n 22.12.30.5 0.0.0.0
[SW3-ZXB-ospf-1-area-0.0.0.0]q
[SW3-ZXB-ospf-1]i d
{% endhighlight %}

## Core-SW
{% highlight cli %}
<H3C>sy
[H3C]sysn Core-SW-ZXB
[Core-SW-ZXB]v 13
[Core-SW-ZXB-vlan13]port g1/0/7
[Core-SW-ZXB-vlan13]v 12
[Core-SW-ZXB-vlan12]port g1/0/5
[Core-SW-ZXB-vlan12]v 14
[Core-SW-ZXB-vlan14]port g1/0/1
[Core-SW-ZXB-vlan14]int v 13
[Core-SW-ZXB-Vlan-interface13]ip a 22.12.30.1 24
[Core-SW-ZXB-Vlan-interface13]int v 12
[Core-SW-ZXB-Vlan-interface12]ip a 22.12.20.1 30
[Core-SW-ZXB-Vlan-interface12]int v 14
[Core-SW-ZXB-Vlan-interface14]ip a 22.12.10.1 30
[Core-SW-ZXB-Vlan-interface14]ospf
[Core-SW-ZXB-ospf-1]a 0
[Core-SW-ZXB-ospf-1-area-0.0.0.0]n 22.12.30.1 0.0.0.0
[Core-SW-ZXB-ospf-1-area-0.0.0.0]n 22.12.20.1 0.0.0.0
[Core-SW-ZXB-ospf-1-area-0.0.0.0]n 22.12.10.1 0.0.0.0
{% endhighlight %}

## SW1
{% highlight cli %}
<H3C>sy
[H3C]sysn SW1-ZXB
[SW1-ZXB]v 20
[SW1-ZXB-vlan20]v 30
[SW1-ZXB-vlan30]port g1/0/8
[SW1-ZXB-vlan30]v 12
[SW1-ZXB-vlan12]port g1/0/5
[SW1-ZXB-vlan12]int v 12
[SW1-ZXB-Vlan-interface12]ip a 22.12.20.2 30
[SW1-ZXB-Vlan-interface12]int v 20
[SW1-ZXB-Vlan-interface20]ip a 192.168.20.1 24
[SW1-ZXB-Vlan-interface20]int v 30
[SW1-ZXB-Vlan-interface30]ip a 192.168.30.1 24
[SW1-ZXB-Vlan-interface30]int b 1
[SW1-ZXB-Bridge-Aggregation1]link-aggregation m d
[SW1-ZXB-Bridge-Aggregation1]int range g1/0/1 to g1/0/3
[SW1-ZXB-if-range]port link-aggregation group 1
[SW1-ZXB-if-range]int b 1
[SW1-ZXB-Bridge-Aggregation1]port link-type t
[SW1-ZXB-Bridge-Aggregation1]port t p v a
[SW1-ZXB-Bridge-Aggregation1]ospf
[SW1-ZXB-ospf-1]a 0
[SW1-ZXB-ospf-1-area-0.0.0.0]n 22.12.20.2 0.0.0.0
[SW1-ZXB-ospf-1-area-0.0.0.0]q
[SW1-ZXB-ospf-1]i d
{% endhighlight %}

## SW2
{% highlight cli %}
<H3C>sy
[H3C]sysn SW2-ZXB
[SW2-ZXB]v 20
[SW2-ZXB-vlan20]port g1/0/5
[SW2-ZXB-vlan20]v 30
[SW2-ZXB-vlan30]port g1/0/6
[SW2-ZXB-vlan30]int b 1
[SW2-ZXB-Bridge-Aggregation1]link-aggregation m d
[SW2-ZXB-Bridge-Aggregation1]int range g1/0/1 to g1/0/3
[SW2-ZXB-if-range]port link-aggregation group 1
[SW2-ZXB-if-range]int b 1
[SW2-ZXB-Bridge-Aggregation1]port link-type t
[SW2-ZXB-Bridge-Aggregation1]port t p v a
{% endhighlight %}

## RTA
{% highlight cli %}
<H3C>sy
[H3C]sysn RTA-ZXB
[RTA-ZXB]i g0/1
[RTA-ZXB-GigabitEthernet0/1]ip a 22.12.10.2 30
[RTA-ZXB-GigabitEthernet0/1]in g0/0
[RTA-ZXB-GigabitEthernet0/0]ip a 22.12.40.1 24
[RTA-ZXB-GigabitEthernet0/0]ospf
[RTA-ZXB-ospf-1]a 0
[RTA-ZXB-ospf-1-area-0.0.0.0]n 22.12.10.2 0.0.0.0
[RTA-ZXB-ospf-1-area-0.0.0.0]q
[RTA-ZXB-ospf-1]i d
{% endhighlight %}


## PC地址表

|     PC      |     IP地址        |       子网掩码      |        网关         |     
|   :----:    |     :----:        |       :----:       |       :----:        |
|    PC_5     |   192.168.20.5    |   255.255.255.0    |     192.168.20.1    |
|    PC_6     |   192.168.30.6    |   255.255.255.0    |     192.168.30.1    |
|    PC_8     |   192.168.30.8    |   255.255.255.0    |     192.168.30.1    | 
|    PC_11    |    22.12.40.2     |   255.255.255.0    |      22.12.40.1     | 
|    PC_10    |   192.168.10.10   |   255.255.255.0    |     192.168.10.1    | 

## PC 10 ping --> 5 & 6 & 8 & 11
```CLI
ping 192.168.20.5
ping 192.168.30.6
ping 192.168.30.8
ping 22.12.40.2
```
