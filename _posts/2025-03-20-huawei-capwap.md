---
layout: post
title:  "huawei capwap上线"
author: XM137
date:   2025-03-20 01:20:26 +0800
categories: ENSP
tags: ENSP AC AP
---



首先，最基础的，我们需要让AP与AC建立基本的连接

设备上默认有一个vlan1，
我们用它来实现dhcp和capwap


为AP分配地址
{% highlight cli %}
<AC6605>sy
Enter system view, return user view with Ctrl+Z.
[AC6605]dhcp enable 
Info: The operation may take a few seconds. Please wait for a moment.done.
[AC6605]ip pool 1
Info: It is successful to create an IP address pool.
[AC6605-ip-pool-1]gateway-list 192.168.10.1
[AC6605-ip-pool-1]network 192.168.10.0 mask 24
[AC6605-ip-pool-1]dns-list 192.168.10.1
[AC6605-ip-pool-1]l d 21
[AC6605-ip-pool-1]int vl 1
[AC6605-Vlanif1]ip address 192.168.10.1 24
[AC6605-Vlanif1]dhcp sel g
{% endhighlight %}

下一步我们需要指定CAPWAP连接，也就是指定管理VLAN
我们使用VLAN1接口进行capwap通信
```CLI
[AC6605]capwap source interface Vlanif 1
```

在AC下挂几个AP，随后等待地址分配即可

在此之后，如果打开AP CLI应该能在稍后看到
```CLI
===== CAPWAP LINK IS UP!!! =====
```
当然看不到也没关系

下一步，我们需要配置SSID模板，这个模板指定SSID名称及是否隐藏等其他功能
```CLI
[AC6605-wlan-view]ssid-profile name SSID
[AC6605-wlan-ssid-prof-SSID]ssid XM137
Info: This operation may take a few seconds, please wait.done.
```

安全模板，顾名思义配置加密方式、密码等等
```CLI
[AC6605-wlan-view]security-profile name sec
```
加密我们选择开放，懒得输密码
```CLI
[AC6605-wlan-sec-prof-sec]security open 
```

国家码，每个国家都有着自己的无线频段限制。

例如我国将2.4GHz分为13个信道，其他国家对2.4GHz的分配则有着或多或少的信道数量<br>
再例如我国将6GHz分配给了5G/6G使用，而并未分配给wifi使用<br>
相应的，wifi6E网卡在我国就会关闭6GHz

选择相应的国家码以便匹配对应的设备
```CLI
[AC6605-wlan-view]regulatory-domain-profile name CN ## 域名模板
[AC6605-wlan-regulate-domain-cn]country-code CN ## 国家码
Info: The current country code is same with the input country code.
## 默认就是CN呢
```

VAP，可以理解为多个SSID信号<br>
```CLI
在这里我们将引用SSID、加密配置、转发模式、业务VLAN
[AC6605-wlan-view]vap-profile name vap
[AC6605-wlan-vap-prof-vap]ssid-profile SSID
Info: This operation may take a few seconds, please wait.done.
[AC6605-wlan-vap-prof-vap]security-profile sec
Info: This operation may take a few seconds, please wait.done.
```

我们使用直接转发模式
```CLI
[AC6605-wlan-vap-prof-vap]forward-mode direct-forward
```
我们仍然使用VLAN1，将其作为业务VLAN
```CLI
[AC6605-wlan-vap-prof-vap]service-vlan vlan-id 1
Info: This operation may take a few seconds, please wait.done.
```

AP组，多个AP处于一组的时候，可以使用AP组进行快速配置<br>
此处我们使用默认组
```CLI
[AC6605-wlan-view]ap-group name default
```

我们指定默认组的国家码
```CLI
[AC6605-wlan-ap-group-default]regulatory-domain-profile CN
Warning: Modifying the country code will clear channel, power and antenna gain configurations of the radio and reset the AP. Continue?[Y/N]:y
```

在默认组中应用vap的设置<br>
wlan 1，前面说过vap可以是多个SSID信号，那么你就可以配置多组SSID信号，
例如配置出多个2.4GHz SSID和多个5GHz SSID<br>
radio all 是绑定所有射频。
例如radio 0 2.4GHz，radio 1 5Ghz，radio 2 5.8GHz，
一些新款设备还支持6GHz.
```CLI
[AC6605-wlan-ap-group-default]vap-profile vap wlan 1 radio all 
Info: This operation may take a few seconds, please wait...done.
```


在默认情况下，AC对AP的认证模式是MAC认证

PPT给的是手工添加的AP，需要手工查看MAC地址，名字也要取一个，

既然已经通过CAPWAP建立连接，为何不直接使用呢

使用以下命令查看已经建立CAPWAP连接但又没有添加到AC的AP<br>
这种情况下的AP都是未认证AP，没有手工配置且又是新MAC地址的AP会显示在这里

让我们看看未认证AP<br>
在刚刚连接上且指定capwap后，可能并不会立即显示，请稍后再查看
```CLI
[AC6605-wlan-view]display ap unauthorized record
Unauthorized AP record:
Total number: 2
--------------------------------------------------------------------------------
AP type: AP3030DN
AP SN: 210235448310AE3FCF63
AP MAC address: 00e0-fc2b-2da0
AP IP address: 192.168.10.136
Record time: 2025-03-19 21:33:30
--------------------------------------------------------------------------------
AP type: AP2050DN
AP SN: 2102354483109B3F5C3A
AP MAC address: 00e0-fcff-4170
AP IP address: 192.168.10.129
Record time: 2025-03-19 21:33:21
--------------------------------------------------------------------------------
```


现在我们将所有AP手工认证
```CLI
[AC6605-wlan-view]ap-confirm all
Info: Confirm AP completely. Success count: 2. Failure count: 0.
```
OK，让我们看看所有AP
```CLI
[AC6605-wlan-view]dis ap all
dis ap all
Info: This operation may take a few seconds. Please wait for a moment.done.
Total AP information:
nor  : normal          [2]
-----------------------------------------------------------------------------------------------------
ID   MAC            Name           Group   IP             Type            State STA Uptime
-----------------------------------------------------------------------------------------------------
0    00e0-fcff-4170 00e0-fcff-4170 default 192.168.10.226 AP2050DN        nor   0   1M:14S
1    00e0-fc2b-2da0 00e0-fc2b-2da0 default 192.168.10.168 AP3030DN        nor   0   1M:14S
-----------------------------------------------------------------------------------------------------
Total: 2
```

OKOK，AP 已经是　normal (正常状态)，此时已经可以使用虚拟设备连接AP的无线了
