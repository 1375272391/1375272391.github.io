---
layout: post
title:  "实验 11 DNS透明代理"
author: XM137
date:   2025-04-23 15:20:40 +0800
categories: FW
tags: USG6000V FW NAT DNS-Proxy
---

实验 11 DNS透明代理
## 名称、地址自行替换

拓扑 **[0423dnsproxy.zip](/assets/ENSP/20250423/0423dnsproxy.zip)**

## 地址配置

|    设备     |       IP地址       |      子网掩码       |        网关        |     DNS        |
|   :----:    |       :----:       |      :----:        |       :----:       |   :----:       |
|   Client    |   192.168.17.254   |    255.255.255.0   |    192.168.17.1    |  10.10.10.10   |
| DNS Server  |  110.110.110.254   |    255.255.255.0   |    110.110.110.1   |                 |
| www.name.com|     17.0.0.254     |    255.255.255.0   |      17.0.0.1      |                 |


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
[USG6000V1]u in e
Info: Saving log files...
Info: Information center is disabled.
[USG6000V1]firewall zone trust 
[USG6000V1-zone-trust]a i g 1/0/1
[USG6000V1-zone-trust]q
[USG6000V1]firewall zone untrust 
[USG6000V1-zone-untrust]a i g 1/0/0
[USG6000V1-zone-untrust]int g1/0/1
[USG6000V1-GigabitEthernet1/0/1]ip a 192.168.17.1 24
[USG6000V1-GigabitEthernet1/0/1]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip a 100.100.100.1 24
[USG6000V1-GigabitEthernet1/0/0]q
[USG6000V1]security-policy
[USG6000V1-policy-security]rule n sec
[USG6000V1-policy-security-rule-sec]source-zone trust 
[USG6000V1-policy-security-rule-sec]destination-zone untrust 
[USG6000V1-policy-security-rule-sec]source-address 192.168.17.0 24
[USG6000V1-policy-security-rule-sec]ac p
[USG6000V1-policy-security-rule-sec]q
[USG6000V1-policy-security]q
[USG6000V1]nat-policy
[USG6000V1-policy-nat]rule n nat
[USG6000V1-policy-nat-rule-nat]source-zone trust 
[USG6000V1-policy-nat-rule-nat]destination-zone untrust 
[USG6000V1-policy-nat-rule-nat]source-address 192.168.17.0 24
[USG6000V1-policy-nat-rule-nat]act source-nat easy-ip 
[USG6000V1-policy-nat-rule-nat]q
[USG6000V1-policy-nat]q
[USG6000V1]slb
[USG6000V1-slb]group telcome
[USG6000V1-slb-group-0]rserver rip 110.110.110.254 p 53
[USG6000V1-slb-group-0]action optimize 
[USG6000V1-slb-group-0]q
[USG6000V1-slb]vserver PDNS
[USG6000V1-slb-vserver-0]vip 10.10.10.10
[USG6000V1-slb-vserver-0]protocol any 
[USG6000V1-slb-vserver-0]group telcome
[USG6000V1-slb-vserver-0]slb enable 
[USG6000V1]dns-transparent-policy
[USG6000V1-policy-dns]dns transparent-proxy enable
[USG6000V1-policy-dns]dns server bind interface GigabitEthernet 1/0/0 preferred 110.110.110.254
[USG6000V1-policy-dns]q
[USG6000V1]ip route-static 0.0.0.0 0 100.100.100.2
{% endhighlight %}
<details>
<summary>文本配置</summary>
{% highlight cli %}
admin
Admin@123
y
Admin@123
Aa123456
Aa123456

sy
u in e
firewall zone trust 
a i g 1/0/1
q
firewall zone untrust 
a i g 1/0/0
int g1/0/1
ip a 192.168.17.1 24
int g1/0/0
ip a 100.100.100.1 24
q
security-policy
rule n sec
source-zone trust 
destination-zone untrust 
source-address 192.168.17.0 24
ac p
q
q
nat-policy
rule n nat
source-zone trust 
destination-zone untrust 
source-address 192.168.17.0 24
act source-nat easy-ip 
q
q
slb
group telcome
rserver rip 110.110.110.254 p 53
action optimize 
q
vserver PDNS
vip 10.10.10.10
protocol any 
group telcome
slb enable 
dns-transparent-policy
dns transparent-proxy enable
dns server bind interface GigabitEthernet 1/0/0 preferred 110.110.110.254
q
ip route-static 0.0.0.0 0 100.100.100.2
{% endhighlight %}
</details>


## RT
{% highlight cli %}
<Huawei>sy
Enter system view, return user view with Ctrl+Z.
[Huawei]u in e
Info: Information center is disabled.
[Huawei]int g 0/0/0
[Huawei-GigabitEthernet0/0/0]ip a 100.100.100.2 24
[Huawei-GigabitEthernet0/0/0]int g0/0/1
[Huawei-GigabitEthernet0/0/1]ip a 110.110.110.1 24
[Huawei-GigabitEthernet0/0/1]int g0/0/2
[Huawei-GigabitEthernet0/0/2]ip a 17.0.0.1 24
{% endhighlight %}
<details>
<summary>文本配置</summary>
{% highlight cli %}
sy
Enter system view, return user view with Ctrl+Z.
u in e
Info: Information center is disabled.
int g 0/0/0
ip a 100.100.100.2 24
int g0/0/1
ip a 110.110.110.1 24
int g0/0/2
ip a 17.0.0.1 24

{% endhighlight %}
</details>

## 设备配置
### DNS Server
![](/assets/ENSP/20250423/image1.webp)
### HTTP Server 
![](/assets/ENSP/20250423/image2.webp)

## 测试
![](/assets/ENSP/20250423/image3.webp)