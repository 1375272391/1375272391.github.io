---
layout: post
title:  "作业5：链路聚合"
author: XM137
date:   2024-09-19 18:57:00 +0800
categories: ENSP
tags: ENSP LACP link-aggregation
---

>
> 要求：使用LACP链路聚合方法，全过程视频上传。ping通结果
>

### SW1
{% highlight cli %}
<Huawei>sy
[Huawei]undo in e
[Huawei]sysn SW1
[SW1]v b 10 20
[SW1]int v 10
[SW1-Vlanif10]ip a 10.0.1.1 24
[SW1-Vlanif10]int v 20
[SW1-Vlanif20]ip a 10.0.2.1 24
[SW1-Vlanif20]qu
[SW1]int e 1
[SW1-Eth-Trunk1]mode l
[SW1-Eth-Trunk1]trunkport g 0/0/1 to 0/0/3
[SW1-Eth-Trunk1]lacp preempt e
[SW1-Eth-Trunk1]la p d 10
[SW1-Eth-Trunk1]port link-type t 
[SW1-Eth-Trunk1]port t a v 10 20
[SW1-Eth-Trunk1]qu
[SW1]int g0/0/10
[SW1-GigabitEthernet0/0/10]port link-t a
[SW1-GigabitEthernet0/0/10]port d v 10
[SW1-GigabitEthernet0/0/10]int g0/0/11
[SW1-GigabitEthernet0/0/11]port link-t a
[SW1-GigabitEthernet0/0/11]port d v 20
{% endhighlight %}

### SW2
{% highlight cli %}
<Huawei>sy
[Huawei]undo in e
[Huawei]sysn SW2
[SW2]v b 10 20 
[SW2]int e 1
[SW2-Eth-Trunk1]mo l
[SW2-Eth-Trunk1]trunkport g 0/0/1 to 0/0/3
[SW2-Eth-Trunk1]lacp p e
[SW2-Eth-Trunk1]la p d 10
[SW2-Eth-Trunk1]port link-type t 
[SW2-Eth-Trunk1]port t a v 10 20
[SW2-Eth-Trunk1]qu
[SW2]int g0/0/10
[SW2-GigabitEthernet0/0/10]port link-t a
[SW2-GigabitEthernet0/0/10]port d v 10
[SW2-GigabitEthernet0/0/10]int g0/0/11
[SW2-GigabitEthernet0/0/11]port link-t a
[SW2-GigabitEthernet0/0/11]port d v 20
{% endhighlight %}




## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |       10.0.1.10    |   255.255.255.0    |      10.0.1.1      |
|    PC_2     |       10.0.1.20    |   255.255.255.0    |      10.0.1.1      |
|    PC_3     |       10.0.2.11    |   255.255.255.0    |      10.0.2.1      |
|    PC_4     |       10.0.2.10    |   255.255.255.0    |      10.0.2.1      |


## PC ping
## PC 1
{% highlight cli %}
ping 10.0.1.20
ping 10.0.2.11
ping 10.0.2.10
{% endhighlight %}