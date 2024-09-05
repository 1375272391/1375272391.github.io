---
layout: post
title:  "作业2：远程登陆+静态路由"
author: XM137
date:   2024-09-05 23:14:00 +0800
categories: ENSP
tags: ENSP Static Telnet
---

> 按要求完成作业：截图上传全部配置以及测试联通的结果。
> 
> 交换机名HZ-SW1-XXX中的XXX是姓名首字母
> 
> 使用静态路由
> 
> 核心交换机配置te1net远程登陆:
> 
> 用户名认证方式为AAA 用户名为姓名首字母，密码为姓名首字母@234 
> 
> 加密形式为cipher 用户权限等级为15 最多支持5个用户同时访问


### SW1
{% highlight cli %}
<Huawei>sy
[Huawei]sysname HZ-SW1-ZXB
[HZ-SW1-ZXB]undo info enable 
[HZ-SW1-ZXB]v 10
[HZ-SW1-ZXB-vlan10]int v 10
[HZ-SW1-ZXB-Vlanif10]ip a 192.168.10.1 24
[HZ-SW1-ZXB-Vlanif10]vl 20
[HZ-SW1-ZXB-vlan20]int v 20
[HZ-SW1-ZXB-Vlanif20]ip a 192.168.20.1 24
[HZ-SW1-ZXB-Vlanif20]vl 100
[HZ-SW1-ZXB-vlan100]int v 100
[HZ-SW1-ZXB-Vlanif100]ip a 10.10.1.1 30
[HZ-SW1-ZXB-Vlanif100]int g0/0/1
[HZ-SW1-ZXB-GigabitEthernet0/0/1]port link-t a
[HZ-SW1-ZXB-GigabitEthernet0/0/1]port default vlan 100
[HZ-SW1-ZXB-GigabitEthernet0/0/1]int g0/0/2
[HZ-SW1-ZXB-GigabitEthernet0/0/2]port link-t t
[HZ-SW1-ZXB-GigabitEthernet0/0/2]port trunk allow-pass v 10 20
[HZ-SW1-ZXB-GigabitEthernet0/0/2]qu
[HZ-SW1-ZXB]ip route-static 16.12.30.0 24 10.10.1.2
[HZ-SW1-ZXB]telnet server enable 
[HZ-SW1-ZXB]user-interface vty 0 4 
[HZ-SW1-ZXB-ui-vty0-4]authentication-mode aaa
[HZ-SW1-ZXB-ui-vty0-4]user privilege level 15
[HZ-SW1-ZXB-ui-vty0-4]protocol inbound telnet 
[HZ-SW1-ZXB-ui-vty0-4]qu
[HZ-SW1-ZXB]user-interface console 0
[HZ-SW1-ZXB-ui-console0]user privilege level 15
[HZ-SW1-ZXB-ui-console0]return 
<HZ-SW1-ZXB>sy
[HZ-SW1-ZXB]aaa
[HZ-SW1-ZXB-aaa]local-user zxb password cipher zxb@234 ## user后跟用户名 cipher 跟密码 请根据实际情况修改密码
[HZ-SW1-ZXB-aaa]local-user zxb privilege level 15 ## 设置权限等级
[HZ-SW1-ZXB-aaa]local-user zxb service-type telnet 
{% endhighlight %}

## SW2
{% highlight cli %}
<Huawei>sy
[Huawei]sysn HX-SW2-ZXB
[HX-SW2-ZXB]undo info e
[HX-SW2-ZXB]v 100
[HX-SW2-ZXB-vlan100]int v 100
[HX-SW2-ZXB-Vlanif100]ip a 10.10.1.2 30
[HX-SW2-ZXB-Vlanif100]vl 30
[HX-SW2-ZXB-vlan30]int v 30
[HX-SW2-ZXB-Vlanif30]ip a 16.12.30.1 24
[HX-SW2-ZXB-Vlanif30]int g0/0/1
[HX-SW2-ZXB-GigabitEthernet0/0/1]port link-t a
[HX-SW2-ZXB-GigabitEthernet0/0/1]port d v 100
[HX-SW2-ZXB-GigabitEthernet0/0/1]int g0/0/2
[HX-SW2-ZXB-GigabitEthernet0/0/2]port link-t a
[HX-SW2-ZXB-GigabitEthernet0/0/2]port d v 30
[HX-SW2-ZXB-GigabitEthernet0/0/2]qu
[HX-SW2-ZXB]ip route-static 192.168.10.0 24 10.10.1.1
[HX-SW2-ZXB]ip route-static 192.168.20.0 24 10.10.1.1
{% endhighlight %}

### LSW3
{% highlight cli %}
<Huawei>sy
[Huawei]sysn LSW3-ZXB
[LSW3-ZXB]undo inf e
[LSW3-ZXB]v b 10 20
[LSW3-ZXB]in g0/0/1
[LSW3-ZXB-GigabitEthernet0/0/1]port link-t t
[LSW3-ZXB-GigabitEthernet0/0/1]port trunk allow-pass v 10 20
[LSW3-ZXB-GigabitEthernet0/0/1]int e0/0/1
[LSW3-ZXB-Ethernet0/0/1]port link-t a
[LSW3-ZXB-Ethernet0/0/1]port d v 10
[LSW3-ZXB-Ethernet0/0/1]int e0/0/2
[LSW3-ZXB-Ethernet0/0/2]port link-t a
[LSW3-ZXB-Ethernet0/0/2]port d v 20
{% endhighlight %}

### LSW4
{% highlight cli %}
<Huawei>sy
[Huawei]undo inf e
[Huawei]sysn LSW4-ZXB
[LSW4-ZXB]v 30
[LSW4-ZXB-vlan30]int e0/0/1
[LSW4-ZXB-Ethernet0/0/1]port link-t a
[LSW4-ZXB-Ethernet0/0/1]port d v 30
[LSW4-ZXB-Ethernet0/0/1]int g0/0/2
[LSW4-ZXB-GigabitEthernet0/0/2]port link-t a
[LSW4-ZXB-GigabitEthernet0/0/2]port d v 30
{% endhighlight %}

## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_4     |    192.168.10.10   |   255.255.255.0    |    192.168.10.1    |
|    PC_5     |    192.168.20.10   |   255.255.255.0    |    192.168.20.1    |
|    PC_6     |     16.12.30.10    |   255.255.255.0    |     16.12.30.1     |


### PC ping --> PC6
#### PC4
```CLI
ping 16.12.30.10
```
#### PC5
```CLI
ping 16.12.30.10
```

### Telnet SW2 
### 在 SW2 上测试
{% highlight cli %}
[HX-SW2-ZXB]return ## 返回用户视图
<HX-SW2-ZXB>telnet 10.10.1.1 ## 输入用户名密码
<HZ-SW1-ZXB>dis cu ## 成功后查看远端配置
{% endhighlight %}