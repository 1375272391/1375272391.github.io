---
layout: post
title:  "AC+AP批量配置"
author: XM137
date:   2024-08-06 21:43:00 +0800
categories: H3C
tags: HCL H3C AC AP DHCP default-group
---

![](/assets/H3C/2024-08-06/DHCP/Group/image1.png)

<h4>我们这次仅使用 AC+AP 配置，DHCP、VLAN 全部配置到 AC 上<br>
所以需要配置的只有 AC<br>
<br>
AP零配置启动后，会自动创建 VLAN-interface 1，并在该接口上默认开启 DHCP 客户端<br>
还是那个熟悉的默认 VLAN1</h4>

### 1、基础网络配置
{% highlight cli %}
<H3C>sy
[H3C]i vl 1
[H3C-Vlan-interface1]ip a 192.168.1.1 24
[H3C-Vlan-interface1]v 10
[H3C-vlan10]int vl 10 ## 为 AP 下的客户端准备
[H3C-Vlan-interface10]ip a 192.168.10.1 24
{% endhighlight %}

### 2、为 AP 配置 DHCP
{% highlight cli %}
[H3C-Vlan-interface10]dhcp en ## 启用 DHCP
[H3C]dhcp server ip-pool ap
[H3C-dhcp-pool-ap]n 192.168.1.0 24
[H3C-dhcp-pool-ap]g 192.168.1.1
[H3C-dhcp-pool-ap]d 192.168.1.1
[H3C-dhcp-pool-ap]e d 21
{% endhighlight %}

### 3、为 AP 客户端配置 DHCP
{% highlight cli %}
[H3C-dhcp-pool-ap]dhcp ser ip wlan
[H3C-dhcp-pool-wlan]n 192.168.10.0 24
[H3C-dhcp-pool-wlan]g 192.168.10.1
[H3C-dhcp-pool-wlan]d 192.168.10.1
[H3C-dhcp-pool-wlan]e d 21
[H3C-dhcp-pool-wlan]dhcp ser f 192.168.1.1
[H3C]dhcp ser f 192.168.10.1
{% endhighlight %}

### 4、创建 WLAN 服务模板
{% highlight cli %}
[H3C]wlan service-template wlan
[H3C-wlan-st-wlan]ssid xm137
[H3C-wlan-st-wlan]service-template e
[H3C-wlan-st-wlan]exit
{% endhighlight %}

### 5、使用 默认组 配置 AP
<h4>AP必须属于一个AP组，且只能属于一个AP组<br>
它有一个默认组 default-group。<br>
且默认都在 default-group 这个组里面</h4>

{% highlight cli %}
[H3C]wlan auto-ap e ## 启用自动 AP
[H3C]wlan ap-group default-group ## 配置默认组
[H3C-wlan-ap-group-default-group]ap-model WA6320-HCL ## 指定并进入型号视图
[H3C-wlan-ap-group-default-group-ap-model-WA6320-HCL]radio 1 ## 射频 1
[H3C-wlan-ap-group-default-group-ap-model-WA6320-HCL-radio-1]channel auto unlock　## 自动选择信道
[H3C-wlan-ap-group-default-group-ap-model-WA6320-HCL-radio-1]service-template wlan vlan 10 ## 绑定 WLAN模板 和 VLAN 到 射频 1
[H3C-wlan-ap-group-default-group-ap-model-WA6320-HCL-radio-1]radio enable ## 开启射频
{% endhighlight %}

#### 参考：<br> [03-H3C 融合AC 配置指导][AC] <br> [07-AP管理命令参考][AP] 
[AC]: https://www.h3c.com/cn/d_202001/1266047_30005_0.htm#_Toc524618802  
[AP]: https://www.h3c.com/cn/d_202206/1627409_30005_0.htm 
