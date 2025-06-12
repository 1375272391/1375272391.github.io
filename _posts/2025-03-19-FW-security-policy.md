---
layout: post
title:  "实验 3 防火墙安全域及策略"
author: XM137
date:   2025-03-19 00:10:59 +0800
categories: Firewall
tags: [ENSP, FireWall, USG6000V]
---

ENSP USG6000V security-policy firewall zone Firewall 防火墙 华为 

## 拓扑 **[security-policy](/assets/ENSP/20250319/security-policy.zip)**

>
> 任务要求：
> 
> 按照拓扑完成接口基本配置及路由器的静态路由配置；
> 
> 将防火墙的G1/0/0口加入到trust安全域，G1/0/1口加入到untrust安全域；
> 
> 添加AR1到AR2允许PING的安全策略（策略名称为“AR1-AR2-姓名简拼”）
> 
> 测试AR1到AR2能否PING通，并截图提交。
>


# `IP地址`、`姓名简拼` 自行更改

## AR1
{% highlight cli %}
<Huawei>sy
Enter system view, return user view with Ctrl+Z.
[Huawei]int g0/0/0
[Huawei-GigabitEthernet0/0/0]ip a 192.168.17.2 24
[Huawei-GigabitEthernet0/0/0]q
[Huawei]ip route-static 0.0.0.0 0 192.168.17.1
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
int g0/0/0
ip a 192.168.17.2 24
q
ip route-static 0.0.0.0 0 192.168.17.1
{% endhighlight %}
</details>


## AR2
{% highlight cli %}
<Huawei>sy
Enter system view, return user view with Ctrl+Z.
[Huawei]int g0/0/0
[Huawei-GigabitEthernet0/0/0]ip a 17.1.1.2 24
[Huawei-GigabitEthernet0/0/0]q
[Huawei]ip route-static 0.0.0.0 0 17.1.1.1
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
int g0/0/0
ip a 17.1.1.2 24
q
ip route-static 0.0.0.0 0 17.1.1.1
{% endhighlight %}
</details>


## FW1
{% highlight cli %}
Username:admin
Password:Admin@123
The password needs to be changed. Change now? [Y/N]: y
Please enter old password: Admin@123
Please enter new password: Aa123456
Please confirm new password: Aa123456
<USG6000V1>sy
Enter system view, return user view with Ctrl+Z.
[USG6000V1]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip a 192.168.17.1 24
[USG6000V1-GigabitEthernet1/0/0]service-m p p
[USG6000V1-GigabitEthernet1/0/0]int g1/0/1
[USG6000V1-GigabitEthernet1/0/1]ip a 17.1.1.1 24
[USG6000V1-GigabitEthernet1/0/1]service-m p p
[USG6000V1-GigabitEthernet1/0/1]q
[USG6000V1]firewall zone trust 
[USG6000V1-zone-trust]a i g1/0/0
[USG6000V1-zone-trust]q
[USG6000V1]firewall zone untrust 
[USG6000V1-zone-untrust]a i g1/0/1
[USG6000V1-zone-untrust]q
[USG6000V1]security-policy
[USG6000V1-policy-security]r n AR1-AR2-ZXB
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]source-z trust 
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]destination-zone untrust 
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]source-address 192.168.17.2 0.0.0.0
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]destination-address 17.1.1.2 0.0.0.0
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]service icmp
[USG6000V1-policy-security-rule-AR1-AR2-ZXB]act p
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
admin
Admin@123
y
Admin@123
Aa123456
Aa123456

sy
int g1/0/0
ip a 192.168.17.1 24
service-m p p
int g1/0/1
ip a 17.1.1.1 24
service-m p p
q
firewall zone trust 
a i g1/0/0
q
firewall zone untrust 
a i g1/0/1
q
security-policy
r n AR1-AR2-ZXB
source-z trust 
destination-zone untrust 
source-address 192.168.17.2 0.0.0.0
destination-address 17.1.1.2 0.0.0.0
service icmp
act p
{% endhighlight %}
</details>