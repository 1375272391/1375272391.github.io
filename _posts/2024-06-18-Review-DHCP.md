---
layout: post
title:  "复习1：DHCP"
author: XM137
date:   2024-06-19 23:12:00 +0800
categories: H3C
tags: HCL H3C DHCP Review
---

### 注意： 按照学习通拓扑连线
### 仅供复习参考

{% highlight cli %}
<H3C>sy
[H3C]sysn RT-ZXB
[RT-ZXB]dhcp e
[RT-ZXB]dhcp server ip-pool 0
[RT-ZXB-dhcp-pool-0]n 172.16.30.0 24
[RT-ZXB-dhcp-pool-0]g 172.16.30.2
[RT-ZXB-dhcp-pool-0]d 172.16.30.1
[RT-ZXB-dhcp-pool-0]e d 21
[RT-ZXB-dhcp-pool-0]q
[RT-ZXB]dhcp server forbidden-ip 172.16.30.1
[RT-ZXB]dhcp server forbidden-ip 172.16.30.2
[RT-ZXB]i g0/0
[RT-ZXB-GigabitEthernet0/0]ip a 172.16.30.2 24
[RT-ZXB-GigabitEthernet0/0]dhcp server apply ip-pool 0
[RT-ZXB-GigabitEthernet0/0]dis dh s ip ## 请在PC获取到地址后执行
[RT-ZXB-GigabitEthernet0/0]dis dhcp s s
{% endhighlight %}