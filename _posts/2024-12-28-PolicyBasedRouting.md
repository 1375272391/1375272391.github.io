---
layout: post
title:  "作业28：策略路由-复习"
author: XM137
date:   2024-12-28 20:10:57 +0800
categories: ENSP
tags: ENSP Policy-Based_Routing
---

策略路由 traffic classifier、traffic behavior、traffic policy

## 拓扑 **[PBR.zip](/assets/ENSP/20241226/PBR.zip)**


## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |    192.168.10.1    |   255.255.255.0    |   192.168.10.254   |
|    PC_2     |    192.168.10.2    |   255.255.255.0    |   192.168.10.254   |
|    PC_3     |    172.16.100.2    |   255.255.255.0    |    172.16.100.1    |

## 策略路由需要明确 <br> 给谁做策略(traffic classifier) <br> 要干什么 (traffic behavior)
## 最后将其做成策略(traffic policy) <br> 并应用到相应位置

## SW1
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn SW1
[SW1]v b 317 100
[SW1]int v317
[SW1-Vlanif317]ip a 192.168.10.254 24
[SW1-Vlanif317]int v100
[SW1-Vlanif100]ip a 101.101.1.1 24
[SW1-Vlanif100]int g0/0/6
[SW1-GigabitEthernet0/0/6]p l a
[SW1-GigabitEthernet0/0/6]p d v 317
[SW1-GigabitEthernet0/0/6]int g0/0/8
[SW1-GigabitEthernet0/0/8]p l a
[SW1-GigabitEthernet0/0/8]p d v 317
[SW1-GigabitEthernet0/0/8]int g0/0/20
[SW1-GigabitEthernet0/0/20]p l a
[SW1-GigabitEthernet0/0/20]p d v 100
[SW1-GigabitEthernet0/0/20]ospf
[SW1-ospf-1]a 0
[SW1-ospf-1-area-0.0.0.0]n 192.168.10.0 0.0.0.255
[SW1-ospf-1-area-0.0.0.0]n 101.101.1.0 0.0.0.255
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn SW1
v b 317 100
int v317
ip a 192.168.10.254 24
int v100
ip  a 101.101.1.1 24
int g0/0/6
p l a
p d v 317
int g0/0/8
p l a
p d v 317
int g0/0/20
p l a
p d v 100
ospf
a 0
n 192.168.10.0 0.0.0.255
n 101.101.1.0 0.0.0.255
{% endhighlight %}
</details>


## AR3
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn AR3
[AR3]int g0/0/0
[AR3-GigabitEthernet0/0/0]ip a 101.101.1.2 24
[AR3-GigabitEthernet0/0/0]int g0/0/1
[AR3-GigabitEthernet0/0/1]ip a 100.1.1.1 24
[AR3-GigabitEthernet0/0/1]int g0/0/2
[AR3-GigabitEthernet0/0/2]ip a 200.1.1.1 24
[AR3-GigabitEthernet0/0/2]ospf
[AR3-ospf-1]a 0
[AR3-ospf-1-area-0.0.0.0]n 100.1.1.0 0.0.0.255
[AR3-ospf-1-area-0.0.0.0]n 200.1.1.0 0.0.0.255
[AR3-ospf-1-area-0.0.0.0]n 101.101.1.0 0.0.0.255
[AR3-ospf-1-area-0.0.0.0]q
[AR3-ospf-1]q
[AR3]acl 2000 ## 使用两个ACL分别指定
[AR3-acl-basic-2000]rule p s 192.168.10.1 0
[AR3-acl-basic-2000]acl 2001
[AR3-acl-basic-2001]rule p s 192.168.10.2 0
[AR3-acl-basic-2001]q
[AR3]traffic c 101 ## 流 类 确定给谁做策略
[AR3-classifier-101]if a 2000
[AR3-classifier-101]traff c 102
[AR3-classifier-102]if a 2001
[AR3-classifier-102]traff b 1002 ## 流行为 确定要做什么
[AR3-behavior-1002]red ip-nexthop 100.1.1.2 ## 重定向
[AR3-behavior-1002]traff b 2002
[AR3-behavior-2002]redirect ip-nexthop 200.1.1.2
[AR3-behavior-2002]q
[AR3]traffic policy PBR ## 流策略 绑定类与行为
[AR3-trafficpolicy-PBR]c 101 b 1002
[AR3-trafficpolicy-PBR]c 102 b 2002
[AR3-trafficpolicy-PBR]int g0/0/0
[AR3-GigabitEthernet0/0/0]traffic-policy PBR i ## 应用流策略
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn AR3
int g0/0/0
ip a 101.101.1.2 24
int g0/0/1
ip a 100.1.1.1 24
int g0/0/2
ip a 200.1.1.1 24
ospf
a 0
n 100.1.1.0 0.0.0.255
n 200.1.1.0 0.0.0.255
n 101.101.1.0 0.0.0.255
q
q
acl 2000
rule p s 192.168.10.1 0
acl 2001
rule p s 192.168.10.2 0
q
traffic c 101
if a 2000
traff c 102
if a 2001
traff b 1002
red ip-nexthop 100.1.1.2
traff b 2002
redirect ip-nexthop 200.1.1.2
q
traffic policy PBR
c 101 b 1002
c 102 b 2002
int g0/0/0
traffic-policy PBR i
{% endhighlight %}
</details>


## AR4
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn AR4
[AR4]int g0/0/1
[AR4-GigabitEthernet0/0/1]ip a 100.1.1.2 24
[AR4-GigabitEthernet0/0/1]int g0/0/2
[AR4-GigabitEthernet0/0/2]ip a 200.1.1.2 24
[AR4-GigabitEthernet0/0/2]int g0/0/0
[AR4-GigabitEthernet0/0/0]ip a 172.16.100.1 24
[AR4-GigabitEthernet0/0/0]ospf 
[AR4-ospf-1]a 0
[AR4-ospf-1-area-0.0.0.0]n 100.1.1.0 0.0.0.255
[AR4-ospf-1-area-0.0.0.0]n 200.1.1.0 0.0.0.255
[AR4-ospf-1-area-0.0.0.0]n 172.16.100.0 0.0.0.255
[AR4-ospf-1-area-0.0.0.0]q
[AR4-ospf-1]q
[AR4]acl 3000 ## 为满足题目要求，使用高级ACL 确定目标地址
[AR4-acl-adv-3000]rule p ip dest 192.168.10.1 0
[AR4-acl-adv-3000]acl 3001
[AR4-acl-adv-3001]rule per ip dest 192.168.10.2 0
[AR4-acl-adv-3001]traff c 101
[AR4-classifier-101]if a 3000
[AR4-classifier-101]traff c 102
[AR4-classifier-102]if a 3001
[AR4-classifier-102]traff b 1001
[AR4-behavior-1001]redirect ip-nexthop 100.1.1.1
[AR4-behavior-1001]traff b 2001
[AR4-behavior-2001]redirect ip-nexthop 200.1.1.1
[AR4-behavior-2001]traff po PBR
[AR4-trafficpolicy-PBR]c 101 b 1001
[AR4-trafficpolicy-PBR]c 102 b 2001
[AR4-trafficpolicy-PBR]int g0/0/0
[AR4-GigabitEthernet0/0/0]traffic-policy PBR i
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn AR4
int g0/0/1
ip a 100.1.1.2 24
int g0/0/2
ip a 200.1.1.2 24
int g0/0/0
ip a 172.16.100.1 24
ospf 
a 0
n 100.1.1.0 0.0.0.255
n 200.1.1.0 0.0.0.255
n 172.16.100.0 0.0.0.255
q
q
acl 3000
rule p ip dest 192.168.10.1 0
acl 3001
rule per ip dest 192.168.10.2 0
traff c 101
if a 3000
traff c 102
if a 3001
traff b 1001
redirect ip-nexthop 100.1.1.1
traff b 2001
redirect ip-nexthop 200.1.1.1
traff po PBR
c 101 b 1001
c 102 b 2001
int g0/0/0
traffic-policy PBR i
{% endhighlight %}
</details>

## 测试
## PC3
```CLI
tracert 192.168.10.1 ## 应途径 100.1.1.1
tracert 192.168.10.2 ## 应途径 200.1.1.1
```

## 或者
## PC1-PC2
```CLI
tracert 172.16.100.2
## PC1 应途经 100.1.1.2
## PC2 应途经 200.1.1.2
```