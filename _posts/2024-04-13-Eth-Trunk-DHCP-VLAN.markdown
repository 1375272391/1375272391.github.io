---
layout: post
title:  "作业14：链路聚合"
author: XM137
date:   2024-04-13 09:28:00 +0800
categories: Daily
tags: H3C HCL Eth-Trunk DHCP VLAN
---
## 题目
> 按照拓扑图连接设备，并配置命令，使PC5、PC6、PC7、PC8、PC9与PC10之间互通。
> 
> 设备互通使用静态路由或默认路由，所有配置截图。设备名称改为名字首字母。
> 
![](https://p.ananas.chaoxing.com/star3/origin/ac88e088967afba5eab5cfba9935f5c3.png)
> 拓扑
![](https://p.ananas.chaoxing.com/star3/origin/76c811f0f630fbe3124117a801bbc7f9.png)

# 若要照抄一定按照拓扑连线
![](/assets/Daily-image/20240413/2024-04-13%20154055.png)

## SWA 配置如下
{% highlight cli %}
system-view ## 进入系统视图
sysname SWA-XXX ## 修改设备名称
vlan 2 ## 创建并进入VLAN视图
port GigabitEthernet 1/0/5 ## 在VLAN视图下将端口添加到对应VLAN
vlan 3
port GigabitEthernet 1/0/6
vlan 10
quit
interface Vlan-interface 2 ## 进入VLAN接口视图
ip address 17.16.50.1 24 ## 配置IP地址
quit
interface Vlan-interface 3
ip address 17.16.60.1 24
quit
interface Vlan-interface 10
ip address 10.1.1.1 30
quit
interface Bridge-Aggregation 1 ## 创建并进入二层聚合接口视图
link-aggregation mode dynamic ## 配置聚合组工作在动态聚合模式下
quit
interface range GigabitEthernet 1/0/1 to GigabitEthernet 1/0/3 ##选中多个接口
port link-aggregation group 1 ## 将接口加入聚合组 1
quit
interface Bridge-Aggregation 1 ## 进入二层聚合接口视图
port link-type trunk ## 修改模式为Trunk
port trunk permit vlan all ## 设置允许所有VLAN通过
quit
ip route-static 192.168.1.0 24 10.1.1.2 ## 设置静态路由
{% endhighlight %}

## SWB配置如下
{% highlight cli %}
system-view
sysname SWB-XXX 
vlan 3 
port GigabitEthernet 1/0/7
vlan 4
port GigabitEthernet 1/0/8
vlan 10
vlan 100
port GigabitEthernet 1/0/10
quit
interface Vlan-interface 4
ip address 17.16.70.1 24
quit
interface Vlan-interface 10
ip address 10.1.1.2 30
quit
interface Vlan-interface 100
ip address 10.1.0.1 30
quit
interface Bridge-Aggregation 1
link-aggregation mode dynamic
quit
interface range GigabitEthernet 1/0/1 to GigabitEthernet 1/0/3
port link-aggregation group 1
quit
interface Bridge-Aggregation 1
port link-type trunk
port trunk permit vlan all
quit
ip route-static 17.16.50.0 24 10.1.1.1
ip route-static 17.16.60.0 24 10.1.1.1
ip route-static 192.168.1.0 24 10.1.0.2
{% endhighlight %}

## RTA配置
{% highlight cli %}
system-view
sysname RTA-XXX 
interface GigabitEthernet 0/0
ip address 10.1.0.2 30
quit
dhcp enable ## 启用DHCP
dhcp server ip-pool 0 ## 创建DHCP地址池
gateway-list 192.168.1.2 ## 指定DHCP网关地址
network 192.168.1.0 mask 255.255.255.0 ## 指定DHCP网段
dns-list 192.168.1.1 ## 指定DHCP DNS
expired day 21 ## 指定DHCP租约期限
quit
dhcp server forbidden-ip 192.168.1.1 ## 设置DHCP不允许分配的地址
dhcp server forbidden-ip 192.168.1.2 ## 网关地址与DNS地址不允许DHCP再分配
interface GigabitEthernet 0/1 ## 进入接口
ip address 192.168.1.2 24 ## 配置IP
dhcp server apply ip-pool 0 ## 在该接口启用DHCP
quit
ip route-static 17.16.50.0 24 10.1.0.1 ## 静态路由设置
ip route-static 17.16.60.0 24 10.1.0.1
ip route-static 17.16.70.0 24 10.1.0.1
{% endhighlight %}
## PC配置
![](https://p.ananas.chaoxing.com/star3/origin/3178039cd275e3c6ff31fcf4a1f613e1.png)
![](https://p.ananas.chaoxing.com/star3/origin/fce72a7ab687e550a20f007ae2d17917.png)
![](https://p.ananas.chaoxing.com/star3/origin/4d10e34767536457bc1d5ab46356aa03.png)
![](https://p.ananas.chaoxing.com/star3/origin/4fd50a5cf7c39f2e3a1dc2d4b7e96c1b.png)
![](https://p.ananas.chaoxing.com/star3/origin/c459fed78578c7b108ea6d8720f1b681.png)
![](https://p.ananas.chaoxing.com/star3/origin/7856ce52945347b0274869d19277ce77.png)
## ping 测试
### ping测试分别在 PC5-PC9 中 ping PC10的IP地址
### 例如PC10的IP地址为 192.168.1.4，则：
{% highlight cli %}
ping 192.168.1.4
{% endhighlight %}
![](https://p.ananas.chaoxing.com/star3/origin/002f4e8a84d63d0e18d48cabae90f3be.png)
### 本次配置按照题目要求 仅PC5-10 PC6-PC10 PC7-PC10 PC8-PC10 PC9-10互通
### 也就是 PC5-9 与PC 10相通 其他相互之间可能并不通，因为其他的静态路由并未指定
### 例如PC5 ping PC8就不通，因为没有为PC8的网段配置静态路由