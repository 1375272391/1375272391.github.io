---
layout: post
title:  "复习2：chap验证"
author: XM137
date:   2024-07-01 19:42:00 +0800
categories: Daily
tags: HCL H3C ppp chap Review
---
### 注意： 按照学习通拓扑连线
### 仅供复习参考

## RTA
{% highlight cli %}
<H3C>sy
[H3C]sysn RTA-ZXB
[RTA-ZXB]i s1/0
[RTA-ZXB-Serial1/0]ip a 10.10.1.2 24
[RTA-ZXB-Serial1/0]in g0/0
[RTA-ZXB-GigabitEthernet0/0]ip a 192.168.1.1 24
[RTA-ZXB-GigabitEthernet0/0]qu
[RTA-ZXB]ip route-static 0.0.0.0 0 10.10.1.1
[RTA-ZXB]local-user zxb class network 
[RTA-ZXB-luser-network-zxb]password s 0
[RTA-ZXB-luser-network-zxb]service-type ppp
[RTA-ZXB-luser-network-zxb]int s1/0
[RTA-ZXB-Serial1/0]ppp authentication-mode chap
{% endhighlight %}

## RTB
{% highlight cli %}
<H3C>sy
[H3C]sysn RTB-ZXB
[RTB-ZXB]i s1/0
[RTB-ZXB-Serial1/0]ip a 10.10.1.1 24
[RTB-ZXB-Serial1/0]in g0/0
[RTB-ZXB-GigabitEthernet0/0]ip a 192.168.2.1 24
[RTB-ZXB-GigabitEthernet0/0]qu
[RTB-ZXB]ip route-static 0.0.0.0 0 10.10.1.2
[RTB-ZXB]i s1/0
[RTB-ZXB-Serial1/0]ppp chap user zxb
[RTB-ZXB-Serial1/0]ppp chap password s 0
{% endhighlight %}

## PC地址表

|     PC      |     IP地址       |       子网掩码      |        网关        |     
|   :----:    |     :----:       |       :----:       |       :----:       |
|    PC_1     |   192.168.1.100  |   255.255.255.0    |    192.168.1.1     |
|    PC_2     |   192.168.2.100  |   255.255.255.0    |    192.168.2.1     |

## PC1 --> PC2 ping
```CLI
ping 192.168.2.100
```