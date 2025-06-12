---
layout: post
title:  "实验 10 DHCP服务及中继配置"
subtitle: "eNSP USG6000V"
author: XM137
date:   2025-04-15 19:28:31 +0800
categories: Firewall
tags: [USG6000V, DHCP-relay]
---

实验 10 DHCP服务及中继配置

### 请将其中的17、18等替换为你自己的

拓扑 **[dhcp-relay.zip](/assets/ENSP/20250415/dhcp-relay.zip)**

## PC地址 DHCP

## FW
{% highlight cli %}
Username:admin
Password:Admin@123
The password needs to be changed. Change now? [Y/N]: y
Please enter old password: Admin@123
Please enter new password: Aa123456
Please confirm new password: Aa123456

 Info: Your password has been changed. Save the change to survive a reboot. 
*************************************************************************
*         Copyright (C) 2014-2018 Huawei Technologies Co., Ltd.         *
*                           All rights reserved.                        *
*               Without the owner's prior written consent,              *
*        no decompiling or reverse-engineering shall be allowed.        *
*************************************************************************


<USG6000V1>sy
Enter system view, return user view with Ctrl+Z.
[USG6000V1]
[USG6000V1]u in e
Info: Saving log files...
Info: Information center is disabled.
[USG6000V1]firewall zone trust 
[USG6000V1-zone-trust]a i g 1/0/0
[USG6000V1-zone-trust]dhcp e
Info: The operation may take a few seconds. Please wait for a moment.done.
[USG6000V1]ip pool zxb
Info: It is successful to create an IP address pool.
[USG6000V1-ip-pool-zxb]net 192.168.17.0 m 24
[USG6000V1-ip-pool-zxb]g 192.168.17.1 
[USG6000V1-ip-pool-zxb]d 8.8.8.8
[USG6000V1-ip-pool-zxb]l d 0 h 2
[USG6000V1-ip-pool-zxb]q
[USG6000V1]ip pool zxb2
Info: It is successful to create an IP address pool.
[USG6000V1-ip-pool-zxb2]net 192.168.18.0 m 24
[USG6000V1-ip-pool-zxb2]g 192.168.18.1
[USG6000V1-ip-pool-zxb2]d 8.8.8.8
[USG6000V1-ip-pool-zxb2]l d 1 ## 默认配置，配置后不显示
[USG6000V1-ip-pool-zxb2]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip a 172.16.0.1 16
[USG6000V1-GigabitEthernet1/0/0]dhcp se g
[USG6000V1-GigabitEthernet1/0/0]q
[USG6000V1]ip route-static 192.168.17.0 24 172.16.0.2
[USG6000V1]ip route-static 192.168.18.0 24 172.16.0.2
{% endhighlight %}


## SW
{% highlight cli %}
<Huawei>sy
Enter system view, return user view with Ctrl+Z.
[Huawei]u in e
Info: Information center is disabled.
[Huawei]v b 10 20 100
Info: This operation may take a few seconds. Please wait for a moment...done.
[Huawei]dhcp e
Info: The operation may take a few seconds. Please wait for a moment.done.
[Huawei]int vl 10
[Huawei-Vlanif10]ip a 192.168.17.1 24
[Huawei-Vlanif10]dhcp se re
[Huawei-Vlanif10]dhcp relay server-ip 172.16.0.1 
[Huawei-Vlanif10]int vl 20
[Huawei-Vlanif20]ip a 192.168.18.1 24
[Huawei-Vlanif20]dhcp se re
[Huawei-Vlanif20]dhcp re server-ip 172.16.0.1
[Huawei-Vlanif20]q
[Huawei]int vl 100
[Huawei-Vlanif100]ip a 172.16.0.2 16
[Huawei-Vlanif100]int g0/0/1
[Huawei-GigabitEthernet0/0/1]p l a
[Huawei-GigabitEthernet0/0/1]p d v 100
[Huawei-GigabitEthernet0/0/1]int g0/0/2
[Huawei-GigabitEthernet0/0/2]p l a
[Huawei-GigabitEthernet0/0/2]p d v 10
[Huawei-GigabitEthernet0/0/2]int g0/0/3
[Huawei-GigabitEthernet0/0/3]p l a
[Huawei-GigabitEthernet0/0/3]p d v 20
{% endhighlight %}

## PC 拿到地址即可
```CLI
PC>ipconfig
```