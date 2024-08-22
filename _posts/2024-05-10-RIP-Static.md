---
layout: post
title:  "20240510-H3C随堂练习"
author: XM137
date:   2024-05-10 13:14:00 +0800
categories: H3C
tags: H3C HCL RIP Static
---

# 题目
> 要求：按照拓扑链接设备，实现全网互通
> 
> 按照要求连接设备，设备名改为姓名首字母。
> 
> 完成基础网络设置,
> 
> 使用RIP协议+静态路由+路由引入实现全网互通。
> 
> 上传全部配置截图+测试结果
> 
> 拓扑图：

![](https://p.ananas.chaoxing.com/star3/origin/543f664375b2868c07855ec0601f83b3.png)

## RT1
{% highlight cli %}
<H3C>sys
[H3C]sysn RT1-ZXB
[RT1-ZXB]ip route-static 192.168.100.0 24 12.10.1.2
[RT1-ZXB]int g0/0
[RT1-ZXB-GigabitEthernet0/0]ip add 10.0.12.1 30
[RT1-ZXB-GigabitEthernet0/0]int g0/1
[RT1-ZXB-GigabitEthernet0/1]ip add 10.0.13.1 30
[RT1-ZXB-GigabitEthernet0/1]int g0/2
[RT1-ZXB-GigabitEthernet0/2]ip add 12.10.1.1 30
[RT1-ZXB-GigabitEthernet0/2]rip
[RT1-ZXB-rip-1]v 2
[RT1-ZXB-rip-1]undo sum
[RT1-ZXB-rip-1]import-route s
[RT1-ZXB-rip-1]net 10.0.12.0
[RT1-ZXB-rip-1]net 10.0.13.0
{% endhighlight %}

## RT2
{% highlight cli %}
<H3C>sys
[H3C]sysn RT2-ZXB
[RT2-ZXB]int g0/0
[RT2-ZXB-GigabitEthernet0/0]ip add 10.0.12.2 30
[RT2-ZXB-GigabitEthernet0/0]int g0/1
[RT2-ZXB-GigabitEthernet0/1]ip add 192.168.120.254 24
[RT2-ZXB-GigabitEthernet0/1]rip
[RT2-ZXB-rip-1]v 2
[RT2-ZXB-rip-1]undo sum
[RT2-ZXB-rip-1]import-route d
[RT2-ZXB-rip-1]net 10.0.12.0
{% endhighlight %}

## RT3
{% highlight cli %}
<H3C>sys
[H3C]sysn RT3-ZXB
[RT3-ZXB]int g0/1
[RT3-ZXB-GigabitEthernet0/1]ip add 10.0.13.2 30
[RT3-ZXB-GigabitEthernet0/1]int g0/2
[RT3-ZXB-GigabitEthernet0/2]ip add 172.16.80.1 24
[RT3-ZXB-GigabitEthernet0/2]rip
[RT3-ZXB-rip-1]v 2
[RT3-ZXB-rip-1]undo sum
[RT3-ZXB-rip-1]net 10.0.13.0
[RT3-ZXB-rip-1]net 172.16.80.0
{% endhighlight %}

## RT4
{% highlight cli %}
<H3C>sys
[H3C]sysn RT4-ZXB
[RT4-ZXB]ip route-static 0.0.0.0 0 12.10.1.1
[RT4-ZXB]int g0/0
[RT4-ZXB-GigabitEthernet0/0]ip add 192.168.100.1 24
[RT4-ZXB-GigabitEthernet0/0]int g0/2
[RT4-ZXB-GigabitEthernet0/2]ip add 12.10.1.2 30
{% endhighlight %}



## PC 配置

|     PC      |     IP地址     |      子网掩码      |        网关       |     
|   :----:    |     :----:     |       :----:      |       :----:      |
|    PC_8     |  192.168.100.2 |   255.255.255.0   |   192.168.100.1   |
|    PC_9     |  192.168.120.2 |   255.255.255.0   |  192.168.120.254  |   
|    PC_10    |   172.16.80.2  |   255.255.255.0   |    172.16.80.1    |   

## ping 测试
### 在PC8 ping 以下地址
```CLI
ping 192.168.120.2
ping 172.16.80.2
```

### 在 PC9 ping 以下地址
```CLI
ping 192.168.120.2
```