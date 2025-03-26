---
layout: post
title:  "作业：AC与AP认证"
author: XM137
date:   2025-03-25 22:55:40 +0800
categories: ENSP
tags: ENSP AC AP
---

拓扑 **[20250326.zip](/assets/ENSP/20250326/20250326.zip)**

如果懒得画，介拓扑还是建议自己换俩AP，避免MAC雷同

这次的作业仅是AC和AP建立基本连接<br>
其余并无特殊


先说Cloud<br>
![](/assets/ENSP/20250326/image1.webp)
我们需要添加一个UDP接口，该接口是Cloud与虚拟设备连接的接口<br>
然后我们需要再选择一个网卡，与之绑定，这样虚拟设备就能通过Cloud与宿主机网卡通信
![](/assets/ENSP/20250326/image2.webp)

笔者选出接口为添加的网卡，入接口选UDP接口，勾选双向通道，此时你会发现，究竟选哪个作为出入好像又没那么重要了

对于ENSP则不建议选择 `VirtualBox Host-Only Ethernet Adapter` 网卡<br>
因为ENSP命令行使用该网卡与虚拟设备相连，介网卡要是发生了什么变化，那.....<br>
但是我们可以使用其他网卡，例如 `VMware Virtual Ethernet Adapter for VMnet8`<br>
这个网卡是`VMware`虚拟机给的`NAT`网卡，它同样开启了`DHCP`，那么我们就可以....

在虚拟设备的设置里我们可以看到串口号，我们可以使用 `Putty`、`SecureCRT`、`MobaXterm` 等软件<br>
![](/assets/ENSP/20250326/image3.webp)
使用`VirtualBox Host-Only Ethernet Adapter` 网卡的IP+虚拟设备端口号来访问虚拟设备<br>
一般情况下 `VirtualBox Host-Only Ethernet Adapter` 网卡的IP地址为 192.168.56.1<br>
那么，
![](/assets/ENSP/20250326/image4.webp)

看起来这很好对吧，但是它会出现一些奇怪的东西
![](/assets/ENSP/20250326/image5.webp)
有时间的同窗可以研究一下<br>
著名机房313给的CRT倒是不会出现那种问题


下面我们开始<br>
管理VLAN 100、业务VLAN 101<br>
建一个VLAN 100 送给 `GigabitEthernet0/0/1` `GigabitEthernet0/0/4` <br>
再建一个 VLAN 101 送给 `GigabitEthernet0/0/2`
{% highlight cli %}
<AC6605>sy
Enter system view, return user view with Ctrl+Z.
[AC6605]v b 100 101
Info: This operation may take a few seconds. Please wait for a moment...done.
[AC6605]int g0/0/2
[AC6605-GigabitEthernet0/0/2]p l a
[AC6605-GigabitEthernet0/0/2]p d v 101
{% endhighlight %}

AP与AC相连端口还要有隔离，那么，如下
{% highlight cli %}
[AC6605-GigabitEthernet0/0/2]int g0/0/1
[AC6605-GigabitEthernet0/0/1]p l t
[AC6605-GigabitEthernet0/0/1]p t a v 100 101
[AC6605-GigabitEthernet0/0/1]p t p v 100
[AC6605-GigabitEthernet0/0/1]port-isolate e

[AC6605-GigabitEthernet0/0/1]int g0/0/4
[AC6605-GigabitEthernet0/0/4]p l t
[AC6605-GigabitEthernet0/0/4]p t a v 100 101
[AC6605-GigabitEthernet0/0/4]p t p v 100
[AC6605-GigabitEthernet0/0/4]port-isolate e
{% endhighlight %}

使用管理VLAN为AP分配地址
{% highlight cli %}
[AC6605]dhcp e
Info: The operation may take a few seconds. Please wait for a moment.done.
[AC6605]int vl 100
[AC6605-Vlanif100]ip a 192.168.100.1 24
[AC6605-Vlanif100]dhcp se in
[AC6605-Vlanif100]dhcp server excluded-ip-address 192.168.100.1
[AC6605-Vlanif100]dhcp ser l d 21 ## 根据需要加吧，不加也行
[AC6605-Vlanif100]dhcp server dns-list 182.168.100.1 ## 同上
{% endhighlight %}

指定CAPWAP源
{% highlight cli %}
[AC6605]capwap s in v 100
{% endhighlight %}

AP组，我寻思默认组就不错，为撒子不用呢
{% highlight cli %}
[AC6605]wlan
[AC6605-wlan-view]regulatory-domain-profile n CN ## 指定下国家码，默认就是CN么,意思一下
[AC6605-wlan-regulate-domain-CN]country-code CN
Info: The current country code is same with the input country code.
[AC6605-wlan-regulate-domain-CN]q
[AC6605-wlan-view]ap auth-mode mac-auth ## 默认就是MAC认证，想打可以打一打，笔者不打
[AC6605-wlan-view]ap-group n ap ## 让我们创建一个名为AP的组
[AC6605-wlan-ap-group-ap]regulatory-domain-profile CN ## 应用一下国家码
Warning: Modifying the country code will clear channel, power and antenna gain c
onfigurations of the radio and reset the AP. Continue?[Y/N]:y ## 记得确认
[AC6605-wlan-ap-group-ap]q
{% endhighlight %}

OK 下面添加AP <br>
MAC认证就找AP MAC么，AP虚拟设备配置里面有，但是咱比较懒，就用上学期学到的方法偷懒

在AP下
{% highlight cli %}
<Huawei>dis int vl 1
Vlanif1 current state : UP
Line protocol current state : UP
Last line protocol up time : 2025-03-25 20:34:21 UTC-05:13
Description:HUAWEI, AP Series, Vlanif1 Interface
Route Port,The Maximum Transmit Unit is 1500
Internet Address is allocated by DHCP, 192.168.217.139/24
IP Sending Frames' Format is PKTFMT_ETHNT_2, Hardware address is 00e0-fc26-0260
Current system time: 2025-03-25 21:13:59-05:13
    Input bandwidth utilization  : --
    Output bandwidth utilization : --
	
## 哇哦获取到了Net8的地址呢，但是在这里不重要
{% endhighlight %}
就可以看到MAC地址，让我们复制一下

下面手工添加AP
{% highlight cli %}
[AC6605-wlan-view]ap-id 1 ap-mac 00e0-fc26-0260 ## MAC地址看清楚，不一样的
[AC6605-wlan-ap-1]ap-group ap ## 给它加进组里
Warning: This operation may cause AP reset. If the country code changes, it will
 clear channel, power and antenna gain configurations of the radio, Whether to c
ontinue? [Y/N]:y ## 记得确认
Info: This operation may take a few seconds. Please wait for a moment.. done.
[AC6605-wlan-ap-1]q
{% endhighlight %}

同样的方法添加第二个
{% highlight cli %}
[AC6605-wlan-view]ap-id 2 ap-mac 00e0-fc17-4fc0
[AC6605-wlan-ap-2]ap-group ap
Warning: This operation may cause AP reset. If the country code changes, it will
 clear channel, power and antenna gain configurations of the radio, Whether to c
ontinue? [Y/N]:y
Info: This operation may take a few seconds. Please wait for a moment.. done.
{% endhighlight %}

下面让我们看看状态
{% highlight cli %}
[AC6605-wlan-ap-2]dis ap all
{% endhighlight %}
![](/assets/ENSP/20250326/image6.webp)
Very Good 分配到了管理地址且还是正常状态，那么就此结束


其他的要求按照学习通截图就得了
