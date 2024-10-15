---
layout: post
title:  "作业9：DHCP综合实验"
author: XM137
date:   2024-10-15 22:43:00 +0800
categories: ENSP
tags: ENSP DHCP-Relay OSPF
---


> DHCP 中继实验
> 
> PC机地址均由DHCP分配
> 
> 不保证都会分配相同的地址，
> 
> 请在PC机执行 `ipconfig` 查看IP地址
> 

### select01
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn select01
[select01]v b 10 100
[select01]dhcp ena
[select01]int v 10
[select01-Vlanif10]ip a 192.168.10.254 24
[select01-Vlanif10]dhcp select interface 
[select01-Vlanif10]dhcp server dns-list 192.168.10.253
[select01-Vlanif10]dhcp server ex 192.168.10.253
[select01-Vlanif10]dhcp server ex 192.168.10.252
[select01-Vlanif10]int v100
[select01-Vlanif100]ip a 17.16.5.5 24
[select01-Vlanif100]int g0/0/5
[select01-GigabitEthernet0/0/5]port l a
[select01-GigabitEthernet0/0/5]port d v 10
[select01-GigabitEthernet0/0/5]int g0/0/1
[select01-GigabitEthernet0/0/1]port l a
[select01-GigabitEthernet0/0/1]port d v 100
[select01-GigabitEthernet0/0/1]ospf
[select01-ospf-1]a 0
[select01-ospf-1-area-0.0.0.0]n 17.16.5.5 0.0.0.0
[select01-ospf-1-area-0.0.0.0]n 192.168.10.0 0.0.0.255
{% endhighlight %}


### edge01
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn edge01
[edge01]int g0/0/1
[edge01-GigabitEthernet0/0/1]ip a 17.16.5.2 24
[edge01-GigabitEthernet0/0/1]int g0/0/0
[edge01-GigabitEthernet0/0/0]ip a 17.16.1.2 30
[edge01-GigabitEthernet0/0/0]dhcp ena
[edge01]ip pool V40
[edge01-ip-pool-V40]net 192.168.40.0 m 24
[edge01-ip-pool-V40]g 192.168.40.1 
[edge01-ip-pool-V40]d 192.168.40.254
[edge01-ip-pool-V40]e 192.168.40.254
[edge01-ip-pool-V40]int g0/0/0
[edge01-GigabitEthernet0/0/0]dhcp se gl
[edge01-GigabitEthernet0/0/0]ospf 
[edge01-ospf-1]a 0
[edge01-ospf-1-area-0.0.0.0]n 17.16.5.2 0.0.0.0
[edge01-ospf-1-area-0.0.0.0]n 17.16.1.2 0.0.0.0
{% endhighlight %}


### Core1
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn Core1
[Core1]int g0/0/0
[Core1-GigabitEthernet0/0/0]ip a 17.16.1.1 30
[Core1-GigabitEthernet0/0/0]int g0/0/1
[Core1-GigabitEthernet0/0/1]ip a 17.16.3.1 30
[Core1-GigabitEthernet0/0/1]int g0/0/2
[Core1-GigabitEthernet0/0/2]ip a 17.16.2.1 30
[Core1-GigabitEthernet0/0/2]ospf
[Core1-ospf-1]a 0
[Core1-ospf-1-area-0.0.0.0]n 17.16.1.1 0.0.0.0
[Core1-ospf-1-area-0.0.0.0]n 17.16.2.1 0.0.0.0
[Core1-ospf-1-area-0.0.0.0]n 17.16.3.1 0.0.0.0
{% endhighlight %}


### global
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn global
[global]v b 20 30 100
[global]dhcp ena
[global]int v 100
[global-Vlanif100]ip a 17.16.3.2 30
[global-Vlanif100]int v 20
[global-Vlanif20]ip a 10.10.20.1 24
[global-Vlanif20]int v 30
[global-Vlanif30]ip a 10.10.30.1 24
[global-Vlanif30]int v20
[global-Vlanif20]dhcp se in
[global-Vlanif20]dhcp server d 114.114.114.114
[global-Vlanif20]dhcp ser l d 1
[global-Vlanif20]int v30
[global-Vlanif30]dhcp se in
[global-Vlanif30]dhcp ser d 114.114.114.114
[global-Vlanif30]dhcp ser l d 1
[global-Vlanif30]int g0/0/5
[global-GigabitEthernet0/0/5]port l a
[global-GigabitEthernet0/0/5]port d v 20
[global-GigabitEthernet0/0/5]int g0/0/6
[global-GigabitEthernet0/0/6]port l a
[global-GigabitEthernet0/0/6]port d v 30
[global-GigabitEthernet0/0/6]int g0/0/01
[global-GigabitEthernet0/0/1]port l a
[global-GigabitEthernet0/0/1]port d v 100
[global-GigabitEthernet0/0/1]ospf 
[global-ospf-1]a 0
[global-ospf-1-area-0.0.0.0]n 17.16.3.2 0.0.0.0
[global-ospf-1-area-0.0.0.0]n 10.10.20.0 0.0.0.255
[global-ospf-1-area-0.0.0.0]n 10.10.30.0 0.0.0.255
{% endhighlight %}


### edge02
{% highlight cli %}
<Huawei>sy
[Huawei]un in e
[Huawei]sysn edge02
[edge02]v b 40 100
[edge02]dhcp ena
[edge02]int v 100
[edge02-Vlanif100]ip a 17.16.2.2 30
[edge02-Vlanif100]int v 40
[edge02-Vlanif40]ip a 192.168.40.1 24
[edge02-Vlanif40]dhcp se re
[edge02-Vlanif40]dhcp relay server-i 17.16.1.2
[edge02-Vlanif40]ospf
[edge02-ospf-1]a 0
[edge02-ospf-1-area-0.0.0.0]n 17.16.2.2 0.0.0.0
[edge02-ospf-1-area-0.0.0.0]n 192.168.40.0 0.0.0.255
[edge02-ospf-1-area-0.0.0.0]q
[edge02-ospf-1]int g0/0/1
[edge02-GigabitEthernet0/0/1]port l a
[edge02-GigabitEthernet0/0/1]port d v 40
[edge02-GigabitEthernet0/0/1]int g0/0/2
[edge02-GigabitEthernet0/0/2]port l a
[edge02-GigabitEthernet0/0/2]port d v 100
{% endhighlight %}

---

## ping 测试
### 在其余 `PC` 使用 `ipconfig` 获取到IP地址后
### 在PC4逐一　`ping` 测试即可

---

## 拓扑 [1015.zip](/assets/ENSP/20241015/1015.zip)
