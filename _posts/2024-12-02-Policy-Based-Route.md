---
layout: post
title:  "作业21：策略路由"
author: XM137
date:   2024-12-02 19:49:26 +0800
categories: ENSP
tags: ENSP Policy-Based-Route
---

## 拓扑 **[1202.zip](/assets/ENSP/20241202/1202.zip)**


---
## 设备地址配置

|    设备     |        IP地址       |     子网掩码      |       网关        |
|   :----:    |        :----:      |      :----:       |      :----:      |
|    PC_1     |    172.16.10.100   |  255.255.255.0    |    172.16.10.1   |
|    PC_2     |    172.16.20.100   |  255.255.255.0    |    172.16.20.1   |


---
## 设备命令配置
### S1
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn S1-ZXB
[S1-ZXB]v b 2 10 20
[S1-ZXB]int v2
[S1-ZXB-Vlanif2]ip a 172.16.1.2 24
[S1-ZXB-Vlanif2]int v10
[S1-ZXB-Vlanif10]ip a 172.16.10.1 24
[S1-ZXB-Vlanif10]int v20
[S1-ZXB-Vlanif20]ip a 172.16.20.1 24
[S1-ZXB-Vlanif20]int g0/0/2
[S1-ZXB-GigabitEthernet0/0/2]port l a
[S1-ZXB-GigabitEthernet0/0/2]p d v 2
[S1-ZXB-GigabitEthernet0/0/2]int g0/0/10
[S1-ZXB-GigabitEthernet0/0/10]p l a
[S1-ZXB-GigabitEthernet0/0/10]p d v 10
[S1-ZXB-GigabitEthernet0/0/10]int g0/0/20
[S1-ZXB-GigabitEthernet0/0/20]p l a
[S1-ZXB-GigabitEthernet0/0/20]p d v 20
[S1-ZXB-GigabitEthernet0/0/20]ospf
[S1-ZXB-ospf-1]a 0
[S1-ZXB-ospf-1-area-0.0.0.0]n 172.16.1.2 0.0.0.0
[S1-ZXB-ospf-1-area-0.0.0.0]q
[S1-ZXB-ospf-1]i d
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn S1-ZXB
v b 2 10 20
int v2
ip a 172.16.1.2 24
int v10
ip a 172.16.10.1 24
int v20
ip a 172.16.20.1 24
int g0/0/2
port l a
p d v 2
int g0/0/10
p l a
p d v 10
int g0/0/20
p l a
p d v 20
ospf
a 0
n 172.16.1.2 0.0.0.0
q
i d
{% endhighlight %}
</details>


### R1
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn R1-ZXB
[R1-ZXB]int g0/0/2
[R1-ZXB-GigabitEthernet0/0/2]ip a 172.16.1.1 24
[R1-ZXB-GigabitEthernet0/0/2]int g0/0/0
[R1-ZXB-GigabitEthernet0/0/0]ip a 172.16.12.1 30
[R1-ZXB-GigabitEthernet0/0/0]int g0/0/1
[R1-ZXB-GigabitEthernet0/0/1]ip a 172.16.21.1 30
[R1-ZXB-GigabitEthernet0/0/1]ospf
[R1-ZXB-ospf-1]a 0
[R1-ZXB-ospf-1-area-0.0.0.0]n 172.16.1.1 0.0.0.0
[R1-ZXB-ospf-1-area-0.0.0.0]n 172.16.12.1 0.0.0.0
[R1-ZXB-ospf-1-area-0.0.0.0]n 172.16.21.1 0.0.0.0
[R1-ZXB-ospf-1-area-0.0.0.0]acl n 2000
[R1-ZXB-acl-basic-2000]rule p s 172.16.10.0 0.0.0.255
[R1-ZXB-acl-basic-2000]acl n 2001
[R1-ZXB-acl-basic-2001]ru p s 172.16.20.0 0.0.0.255
[R1-ZXB-acl-basic-2001]q
[R1-ZXB]traffic classifier vlan10
[R1-ZXB-classifier-vlan10]i a 2000
[R1-ZXB-classifier-vlan10]q
[R1-ZXB]traffic classifier vlan20
[R1-ZXB-classifier-vlan20]i a 2001
[R1-ZXB-classifier-vlan20]q
[R1-ZXB]traffic b vlan10
[R1-ZXB-behavior-vlan10]red ip 172.16.12.2
[R1-ZXB-behavior-vlan10]s e
[R1-ZXB-behavior-vlan10]q
[R1-ZXB]traffic b vlan20
[R1-ZXB-behavior-vlan20]red ip 172.16.21.2 
[R1-ZXB-behavior-vlan20]s e
[R1-ZXB-behavior-vlan20]q
[R1-ZXB]traffic policy zxb
[R1-ZXB-trafficpolicy-zxb]cla vlan10 b vlan10
[R1-ZXB-trafficpolicy-zxb]cla vlan20 b vlan20
[R1-ZXB-trafficpolicy-zxb]int g0/0/2
[R1-ZXB-GigabitEthernet0/0/2]traffic-policy zxb in
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn R1-ZXB
int g0/0/2
ip a 172.16.1.1 24
int g0/0/0
ip a 172.16.12.1 30
int g0/0/1
ip a 172.16.21.1 30
ospf
a 0
n 172.16.1.1 0.0.0.0
n 172.16.12.1 0.0.0.0
n 172.16.21.1 0.0.0.0
acl n 2000
rule p s 172.16.10.0 0.0.0.255
acl n 2001
ru p s 172.16.20.0 0.0.0.255
q
traffic classifier vlan10
i a 2000
q
traffic classifier vlan20
i a 2001
q
traffic b vlan10
red ip 172.16.12.2
s e
q
traffic b vlan20
red ip 172.16.21.2 
s e
q
traffic policy zxb
cla vlan10 b vlan10
cla vlan20 b vlan20
int g0/0/2
traffic-policy zxb in
{% endhighlight %}
</details>


### R2
{% highlight cli %}
<Huawei>sy
[Huawei]u in e
[Huawei]sysn R2-ZXB
[R2-ZXB]int l 0
[R2-ZXB-LoopBack0]ip a 172.16.2.2 32
[R2-ZXB-LoopBack0]int g0/0/0
[R2-ZXB-GigabitEthernet0/0/0]ip a 172.16.12.2 30
[R2-ZXB-GigabitEthernet0/0/0]int g0/0/1
[R2-ZXB-GigabitEthernet0/0/1]ip a 172.16.21.2 30
[R2-ZXB-GigabitEthernet0/0/1]ospf
[R2-ZXB-ospf-1]a 0
[R2-ZXB-ospf-1-area-0.0.0.0]n 172.16.2.2 0.0.0.0
[R2-ZXB-ospf-1-area-0.0.0.0]n 172.16.12.2 0.0.0.0
[R2-ZXB-ospf-1-area-0.0.0.0]n 172.16.21.2 0.0.0.0
{% endhighlight %}
<details>
<summary>纯文本配置</summary>
{% highlight cli %}
sy
u in e
sysn R2-ZXB
int l 0
ip a 172.16.2.2 32
int g0/0/0
ip a 172.16.12.2 30
int g0/0/1
ip a 172.16.21.2 30
ospf
a 0
n 172.16.2.2 0.0.0.0
n 172.16.12.2 0.0.0.0
n 172.16.21.2 0.0.0.0
{% endhighlight %}
</details>

## 测试 
### PC1
```CLI
ping 172.16.2.2
```

### R1
```CLI
[R1-ZXB]display traffic policy statistics interface GigabitEthernet 0/0/2 inbound
```