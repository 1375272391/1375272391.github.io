---
layout: post
title:  "H3C网络技术（233,234,235）-阶段测试卷1"
author: XM137
date:   2024-04-16 13:19:00 +0800
categories: Daily
tags: H3C HCL Eth-Trunk DHCP RouterOnAStick VLAN
---
## 题目拓扑
![](https://p.ananas.chaoxing.com/star3/origin/f21c9852688a64de6fdd4807053f709b.png)

## DHCP部分 在SW3配置
{% highlight cli %}
<H3C>system-view ## 系统视图
[H3C]sysname SW3-ZXB ## 修改设备名
[SW3-ZXB]dhcp enable ##启用DHCP服务
[SW3-ZXB]dhcp server ip-pool 0 ##配置DHCP服务地址池
[SW3-ZXB-dhcp-pool-0]network 192.168.20.0 24 ##设置DHCP 网段
[SW3-ZXB-dhcp-pool-0]gateway-list 192.168.20.2 ##设置DHCP网关
[SW3-ZXB-dhcp-pool-0]dns-list 192.168.20.1 ##设置DHCP　DNS
[SW3-ZXB-dhcp-pool-0]qu
[SW3-ZXB]dhcp server forbidden-ip 192.168.20.1 ##设置不允许分配的IP地址
[SW3-ZXB]dhcp server forbidden-ip 192.168.20.2
[SW3-ZXB]vlan 200　##创建VLAN
[SW3-ZXB-vlan200]port g1/0/3 ##端口加入到VLAN
[SW3-ZXB-vlan200]port g1/0/4
[SW3-ZXB-vlan200]int v 200 ##VLAN接口
[SW3-ZXB-Vlan-interface200]ip address 192.168.20.2 24 ##设置VLAN地址
[SW3-ZXB-Vlan-interface200]vlan 100
[SW3-ZXB-vlan100]port g1/0/1
[SW3-ZXB-vlan100]int v 100
[SW3-ZXB-Vlan-interface100]ip address 10.10.1.10 30
[SW3-ZXB-Vlan-interface100]qu
[SW3-ZXB]ip route-static 0.0.0.0 0 10.10.1.9 ##默认路由
[SW3-ZXB]ip route-static 172.16.20.0 25 10.10.1.9 ##静态路由
{% endhighlight %}

## 单臂路由 RT2 和 SW4
## 路由RT2配置
{% highlight cli %}
<H3C>system-view 
[H3C]sysname RT2-ZXB
[RT2-ZXB]int GigabitEthernet 0/2.1 ##进入子接口
[RT2-ZXB-GigabitEthernet0/2.1]ip address 16.50.10.1 26 ##配置IP地址
[RT2-ZXB-GigabitEthernet0/2.1]vlan-type dot1q vid 110 ##指定VLAN
[RT2-ZXB-GigabitEthernet0/2.1]int g0/2.2
[RT2-ZXB-GigabitEthernet0/2.2]ip address 16.50.10.65 26
[RT2-ZXB-GigabitEthernet0/2.2]vlan-type dot1q vid 120
[RT2-ZXB-GigabitEthernet0/2.2]qu
[RT2-ZXB]int g0/1
[RT2-ZXB-GigabitEthernet0/1]ip add 10.10.1.6 30 ##配置IP地址
[RT2-ZXB-GigabitEthernet0/1]int g0/0
[RT2-ZXB-GigabitEthernet0/0]ip add 10.10.1.9 30
[RT2-ZXB-GigabitEthernet0/0]qu
[RT2-ZXB]ip route-static 0.0.0.0 0 10.10.1.5
[RT2-ZXB]ip route-static 192.168.20.0 24 10.10.1.10
{% endhighlight %}
## 交换机 SW4 配置
{% highlight cli %}
<H3C>system-view 
[H3C]sysname SW4-ZXB
[SW4-ZXB]vlan 110
[SW4-ZXB-vlan110]port g1/0/3
[SW4-ZXB-vlan110]vlan 120
[SW4-ZXB-vlan120]port g1/0/4
[SW4-ZXB-vlan120]int g1/0/1
[SW4-ZXB-GigabitEthernet1/0/1]port link-type trunk ##设置Trunk
[SW4-ZXB-GigabitEthernet1/0/1]port trunk permit vlan 110 120 ##批准VLAN
{% endhighlight %}

## 核心路由 RT1 配置
{% highlight cli %}
<H3C>system-view 
[H3C]sysname RT1-ZXB
interface GigabitEthernet0/0
ip address 10.10.1.1 30 ##配置地址
qu
interface GigabitEthernet0/1
ip address 10.10.1.5 30
qu
interface GigabitEthernet0/2
ip address 16.50.10.129 26
qu
ip route-static 0.0.0.0 0 10.10.1.6 ##默认路由&静态路由
ip route-static 172.16.10.0 24 10.10.1.2
ip route-static 172.16.20.0 25 10.10.1.2
{% endhighlight %}

## 链路聚合 SW1 + SW2
## SW1
{% highlight cli %}
system-view 
sysname SW1-ZXB
vlan 10
port GigabitEthernet 1/0/10
vlan 20
qu
interface Bridge-Aggregation 1 ##创建二层聚合组
link-aggregation mode dynamic ##动态聚合模式
qu
interface range GigabitEthernet 1/0/1 to GigabitEthernet 1/0/3 ##接口范围
port link-aggregation group 1 ##加入聚合组
qu
interface Bridge-Aggregation1
port link-type trunk ##聚合组修改为Trunk模式
port trunk permit vlan all ##批准所有VLAN
{% endhighlight %}

## SW2
{% highlight cli %}
system-view 
sysname SW2-ZXB
vlan 10
port GigabitEthernet 1/0/20 ##加入VLAN
vlan 20
port GigabitEthernet 1/0/21
vlan 100
port GigabitEthernet 1/0/10
qu
interface Vlan-interface 10 ##设置地址
ip address 172.16.10.1 24
qu
interface Vlan-interface 20
ip address 172.16.20.1 25
qu
interface Vlan-interface 100
ip address 10.10.1.2 30
qu
interface Bridge-Aggregation1 ##二层聚合组
link-aggregation mode dynamic ##动态聚合
qu
interface range GigabitEthernet 1/0/1 to GigabitEthernet 1/0/3
port link-aggregation group 1 ##加入聚合
qu
interface Bridge-Aggregation 1
port link-type trunk ##聚合Trunk模式
port trunk permit vlan all ##批准VLAN
qu
ip route-static 0.0.0.0 0 10.10.1.1
{% endhighlight %}


## PC 配置

|     PC      |    IP地址    |       子网掩码     |      网关     |     备注     |
|    :----:   |    :----:    |   :----:          |  :----:       |
|    PC_7     | 172.16.10.7  |   255.255.255.0   |  172.16.10.1  |  链路聚合PC  |
|    PC_8     | 172.16.10.8  |   255.255.255.0   |  172.16.10.1  |
|    PC_9     | 172.16.20.9  |  255.255.255.128  |  172.16.20.1  |    测试PC    |
|    PC_10    | 16.50.10.150 |  255.255.255.192  |  16.50.10.129 |   管理主机    |
|    PC_11    | 16.50.10.30  |  255.255.255.192  |   16.50.10.1  |  单臂路由PC   |
|    PC_14    | 16.50.10.80  |  255.255.255.192  |  16.50.10.65  |
|    PC_12    | 192.168.20.x |  255.255.255.0    |  192.168.20.2 |   DHCP PC    |
|    PC_13    | 192.168.20.x |  255.255.255.0    |  192.168.20.2 |   DHCP 自动配置