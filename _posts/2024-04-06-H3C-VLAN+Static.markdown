---
layout: post
title:  "作业12：VLAN+静态路由实验"
author: XM137
date:   2024-04-06 23:26:00 +0800
categories: Daily
tags: H3C HCL VLAN StaticRouters
---
## 题目
>1. (其它)
>
>按照拓扑配置VLAN+静态路由实验。
>
![](https://p.ananas.chaoxing.com/star3/origin/b44546978231fd64a85289a2cd16123b.png)
>
> 要求：重要
> 
> 1.按照拓扑连接设备。（５分）
> 
> 2.PC端IP地址设置，网关为网段第一个有效地址。（10分）
> 
> 3.交换机SWA-XXX配置（20分）
> 
> （1）交换机名称改为SWA-XXX，XXX代表姓名首字母
> 
> （2）配置相应VLAN及IP地址
> 
> 4.交换机SWB-XXX配置（20分）
> 
> （1）交换机名称改为SWB-XXX，XXX代表姓名首字母
> 
> （2）配置相应VLAN及IP地址
> 
> 5.路由器RTA-XXX配置（25分）
> 
> （1）路由器名称改为RTA-XXX，XXX为姓名首字母
> 
> （2）配置相应端口及IP地址
> 
> （3）配置静态路由（不要默认路由）
> 
> 6.SWA-XXX路由配置，配置默认路由（6分）
> 
> 7.SWB-XXX路由配置，配置默认路由（6分）
> 
> 8.测试PC_4与PC_5联通性，测试PC_4与PC_6联通性。（8分）
> 
> 将配置截图上传为作业，包含PC端配置，交换机配置，路由器配置，和测试结果配置。

## 具体配置如下

```WARNING
说了多少遍了，谁TM再直接在路由器配VLAN，直接叉出去
```
## 拓扑如下
![](/assets/Daily-image/20240406/2024-04-09%20211911.png)

```WARNING
若要照抄，请一定按照拓扑连线
```
## 路由配置
{% highlight cli %}
system-view ## 进入系统试图
sysname RTA-XXX  ## 修改设备名
interface GigabitEthernet 0/1 ## 接口视图配置IP地址
ip address 192.168.1.1 30
quit
interface GigabitEthernet 0/2
ip address 192.168.1.5 30
quit
ip route-static 22.12.10.0 24 192.168.1.2 ## 设置静态路由
ip route-static 22.12.20.0 24 192.168.1.2
ip route-static 16.50.10.0 24 192.168.1.6
save force ## 保存
{% endhighlight %}
![](https://p.ananas.chaoxing.com/star3/origin/56a8dd419a983a596e2acc7349a44ecd.png)

## 交换机A配置
{% highlight cli %}
system-view ## 进入系统试图
sysname SWA-XXX  ## 修改设备名
vlan 20 ## 创建VLAN
port GigabitEthernet 1/0/4 ## 将端口添加到VLAN
vlan 30 
port GigabitEthernet 1/0/5
vlan 100
port GigabitEthernet 1/0/1
quit
interface Vlan-interface 20 ## 进入VLAN接口视图
ip address 22.12.10.1 24  ## 配置IP地址
quit
interface Vlan-interface 30
ip address 22.12.20.1 24
quit
interface Vlan-interface 100
ip address 192.168.1.2 30
quit
ip route-static 0.0.0.0 0.0.0.0 192.168.1.1 ## 配置默认路由
save force
{% endhighlight %}

## 交换机B配置
{% highlight cli %}
system-view ## 进入系统试图
sysname SWB-XXX  ## 修改设备名
vlan 40
port GigabitEthernet 1/0/6 ## 将端口添加到VLAN
vlan 100
port GigabitEthernet 1/0/2
quit
interface Vlan-interface 40 ## 设置VLAN 40 IP地址
ip address 16.50.10.1 24
quit
interface Vlan-interface 100
ip address 192.168.1.6 30
quit
ip route-static 0.0.0.0 0.0.0.0 192.168.1.5 ## 配置默认路由
save force ## 保存
{% endhighlight %}

## PC配置
![](https://p.ananas.chaoxing.com/star3/origin/199d9cf3854bfd9693ae5ecdb65f0ec0.png)
![](https://p.ananas.chaoxing.com/star3/origin/b7c00d220bf402c1e9b63dcd57943cb4.png)
![](https://p.ananas.chaoxing.com/star3/origin/78ba3a300aead3ed75361d9e4f92d1f8.png)
## 连接测试
{% highlight cli %}
ping 22.12.20.5
ping 16.50.10.6
{% endhighlight %}
![](https://p.ananas.chaoxing.com/star3/origin/46a3638068876488cb8b517a483e6189.png)
### 若最后测试不通，请检查你的接口连接是否与拓扑一致
### 你的配置是否与本文有出入