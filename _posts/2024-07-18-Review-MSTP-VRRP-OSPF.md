---
layout: post
title:  "复习4：mstp+Vrrp"
author: XM137
date:   2024-07-18 17:00:00 +0800
categories: Daily
tags: HCL H3C MSTP VRRP OSPF Review
---
<h3>在假期</h3>
<h4>要求：使用MSTP多生成树从逻辑上消除环路 <br>
配置VRRP虚拟路由器协议，实现备份 <br>
采用OSPF路由协议实现互通
</h4>

## 1、PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_10    |     192.168.40.5   |   255.255.255.0    |    192.168.40.1    |
|    PC_6     |     192.168.20.5   |   255.255.255.0    |    192.168.20.2    |
|    PC_7     |     192.168.30.5   |   255.255.255.0    |    192.168.30.2    |
|    PC_8     |     192.168.20.6   |   255.255.255.0    |    192.168.20.3    |
|    PC_9     |     192.168.30.6   |   255.255.255.0    |    192.168.30.3    |

<h4>PC6 PC7的网关均为SWA<br>
根据拓扑PC8 PC9的网关均为SWB<br>
在此先这样配置，以便于验证基础网络配置是否正常</h4>

## 2、RT配置-基础IP地址配置
{% highlight cli %}
<H3C>sy
[H3C]sysn RT-ZXB
[RT-ZXB]i g5/0
[RT-ZXB-GigabitEthernet5/0]ip a 192.168.40.1 24
[RT-ZXB-GigabitEthernet5/0]in g0/1
[RT-ZXB-GigabitEthernet0/1]ip a 3.3.1.2 24
[RT-ZXB-GigabitEthernet0/1]in g0/0
[RT-ZXB-GigabitEthernet0/0]ip a 3.3.2.2 24
{% endhighlight %}

### 2.1 SWA 此处先配置互联地址和VLAN
{% highlight cli %}
<H3C>sy
[H3C]sysn SWA-ZXB
[SWA-ZXB]v 100
[SWA-ZXB-vlan100]port g1/0/10
[SWA-ZXB-vlan100]int v 100
[SWA-ZXB-Vlan-interface100]ip a 3.3.1.1 24
[SWA-ZXB-Vlan-interface100]v 2
[SWA-ZXB-vlan2]int v 2
[SWA-ZXB-Vlan-interface2]ip a 192.168.20.2 24
[SWA-ZXB-Vlan-interface2]v 3
[SWA-ZXB-vlan3]int v 3
[SWA-ZXB-Vlan-interface3]ip a 192.168.30.2 24
{% endhighlight %}

### 2.2 SWB
{% highlight cli %}
<H3C>sy
[H3C]sysn SWB-ZXB
[SWB-ZXB]v 100
[SWB-ZXB-vlan100]port g1/0/10
[SWB-ZXB-vlan100]int v 100
[SWB-ZXB-Vlan-interface100]ip a 3.3.2.1 24
[SWB-ZXB-Vlan-interface100]v 2
[SWB-ZXB-vlan2]int v 2
[SWB-ZXB-Vlan-interface2]ip a 192.168.20.3 24
[SWB-ZXB-Vlan-interface2]v 3
[SWB-ZXB-vlan3]int v 3
[SWB-ZXB-Vlan-interface3]ip a 192.168.30.3 24
{% endhighlight %}

### SW1、SW2仅需声明VLAN
### 2.3 SW1
{% highlight cli %}
<H3C>sy
[H3C]sysn SW1-ZXB
[SW1-ZXB]v 2
[SW1-ZXB-vlan2]port g1/0/10
[SW1-ZXB-vlan2]v 3
[SW1-ZXB-vlan3]port g1/0/20
{% endhighlight %}

### 2.4 SW2
{% highlight cli %}
<H3C>sy
[H3C]sysn SW2-ZXB
[SW2-ZXB]v 2
[SW2-ZXB-vlan2]port g1/0/10
[SW2-ZXB-vlan2]v 3
[SW2-ZXB-vlan3]port g1/0/20
{% endhighlight %}

## 3、配置 Trunk
### 3.1 SWA
{% highlight cli %}
[SWA-ZXB]int range g1/0/1 to g1/0/3
[SWA-ZXB-if-range]port link-type t
[SWA-ZXB-if-range]port t p v a
{% endhighlight %}

### 3.2 SWB
{% highlight cli %}
[SWB-ZXB]int range g1/0/1 to g1/0/3
[SWB-ZXB-if-range]port link-t t
[SWB-ZXB-if-range]port t p v a
{% endhighlight %}

### 3.3 SW1
{% highlight cli %}
[SW1-ZXB]int range g1/0/2 to g1/0/3
[SW1-ZXB-if-range]port link-t t
[SW1-ZXB-if-range]port t p v a
{% endhighlight %}

### 3.4 SW2
{% highlight cli %}
[SW2-ZXB]int range g1/0/2 to g1/0/3
[SW2-ZXB-if-range]port link-t t
[SW2-ZXB-if-range]port t p v a
{% endhighlight %}

## 4、配置 OSPF
### 4.1 RT
{% highlight cli %}
[RT-ZXB]ospf
[RT-ZXB-ospf-1]a 0
[RT-ZXB-ospf-1-area-0.0.0.0]n 3.3.1.2 0.0.0.0
[RT-ZXB-ospf-1-area-0.0.0.0]n 3.3.2.2 0.0.0.0
[RT-ZXB-ospf-1-area-0.0.0.0]q
[RT-ZXB-ospf-1]i d
{% endhighlight %}

### 4.2 SWA
{% highlight cli %}
[SWA-ZXB]ospf
[SWA-ZXB-ospf-1]a 0
[SWA-ZXB-ospf-1-area-0.0.0.0]n 3.3.1.1 0.0.0.0
[SWA-ZXB-ospf-1-area-0.0.0.0]q
[SWA-ZXB-ospf-1]i d
{% endhighlight %}

### 4.3 SWB
{% highlight cli %}
[SWB-ZXB]ospf
[SWB-ZXB-ospf-1]a 0
[SWB-ZXB-ospf-1-area-0.0.0.0]n 3.3.2.1 0.0.0.0
[SWB-ZXB-ospf-1-area-0.0.0.0]q
[SWB-ZXB-ospf-1]i d
{% endhighlight %}

## 5、配置 MSTP
### 5.1 SWA
{% highlight cli %}
[SWA-ZXB]stp g e # 全局启用STP
[SWA-ZXB]s m m # STP模式MSTP
[SWA-ZXB]stp re # STP域配置
[SWA-ZXB-mst-region]reg 17 # STP域名
[SWA-ZXB-mst-region]rev 1 # 修订级别
[SWA-ZXB-mst-region]ins 1 v 2 # 实例1配置
[SWA-ZXB-mst-region]ins 2 v 3 # 实例2配置
[SWA-ZXB-mst-region]ac re # 激活域配置
[SWA-ZXB-mst-region]stp ins 1 r p # 实例1配置为主根桥
[SWA-ZXB]stp ins 2 r s # 实例2配置为备根桥
{% endhighlight %}

### 5.2 SWB
{% highlight cli %}
[SWB-ZXB]stp g e
[SWB-ZXB]s m m
[SWB-ZXB]stp re
[SWB-ZXB-mst-region]reg 17
[SWB-ZXB-mst-region]rev 1
[SWB-ZXB-mst-region]ins 1 v 2
[SWB-ZXB-mst-region]ins 2 v 3
[SWB-ZXB-mst-region]ac re 
[SWB-ZXB-mst-region]stp ins 2 r p # 实例2配置为主根桥
[SWB-ZXB]stp ins 1 r s # 实例1配置为备根桥
{% endhighlight %}

## 6、配置 VRRP
<h4>VRRP有一个默认的优先级100<br>
所以配置监视的时候，优先级直接减至100以下<br>
监视前明确上联口</h4>

### 6.1 SWA
{% highlight cli %}
[SWA-ZXB]int v 2
[SWA-ZXB-Vlan-interface2]vrrp vrid 10 vi 192.168.20.1 # 设置虚拟IP
[SWA-ZXB-Vlan-interface2]vr vr 10 pri 170 # 优先级
[SWA-ZXB-Vlan-interface2]vr vr 10 pre # 抢占模式
[SWA-ZXB-Vlan-interface2]q
[SWA-ZXB]track 1 interface g1/0/10 # 设置监视g1/0/10
[SWA-ZXB-track-1]int v 2
[SWA-ZXB-Vlan-interface2]vr vr 10 track 1 p r 80 # 在监视端口发生故障时将优先级减去80，使得，比默认优先级还低，
[SWA-ZXB-Vlan-interface2]int v 3 # 配置下一组
[SWA-ZXB-Vlan-interface3]vr vr 20 vi 192.168.30.1
[SWA-ZXB-Vlan-interface3]vr vr 20 pri 170
[SWA-ZXB-Vlan-interface3]vr vr 20 pre
[SWA-ZXB-Vlan-interface3]vr vr 20 t 1 p r 80
{% endhighlight %}

### 6.2 SWB
<h4>另一边将优先级给低一些</h4>
{% highlight cli %}
[SWB-ZXB]int v 2
[SWB-ZXB-Vlan-interface2]vr vr 10 vi 192.168.20.1
[SWB-ZXB-Vlan-interface2]vr vr 10 pri 150
[SWB-ZXB-Vlan-interface2]vr vr 10 pre
[SWB-ZXB-Vlan-interface2]q
[SWB-ZXB]track 1 int g1/0/10
[SWB-ZXB-track-1]int v 2
[SWB-ZXB-Vlan-interface2]vr vr 10 t 1 p r 80
[SWB-ZXB-Vlan-interface2]int v 3
[SWB-ZXB-Vlan-interface3]vr vr 20 vi 192.168.30.1
[SWB-ZXB-Vlan-interface3]vr vr 20 pri 150
[SWB-ZXB-Vlan-interface3]vr vr 20 pre
[SWB-ZXB-Vlan-interface3]vr vr 20 t 1 p r 80
{% endhighlight %}

## 7、更改PC网关地址

|     PC      |     IP地址    |   子网掩码   |      网关       |
|   :----:    |     :----:    |    :----:   |     :----:     |
|    PC_6     |      不变     |     不变     |  192.168.20.1  |
|    PC_7     |      不变     |     不变     |  192.168.30.1  |
|    PC_8     |      不变     |     不变     |  192.168.20.1  |
|    PC_9     |      不变     |     不变     |  192.168.30.1  |

#### 7.1 查看 VRRP 状态
```CLI
dis vr v
```
#### 7.2 VRRP 中断测试
```CLI
vr vrid xx shut # xx 为备份组vrid
```
<h4>可关闭任一备份组进行ping测试</h4>
