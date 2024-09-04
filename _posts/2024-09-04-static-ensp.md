---
layout: post
title:  "作业1：静态路由"
author: XM137
date:   2024-09-04 13:10:00 +0800
categories: ENSP
tags: ENSP Static
---
# 如下
> 按照拓扑完成实验，使用静态路由实现网络互通。截图所有配置
> 
> 路由器R1和R2分别命名为姓名首字母+学号后三位+A

## R1
{% highlight cli %}
<Huawei>sy
[Huawei]undo info-center enable ## 关闭信息提示，请根据实际要求决定是否需要关闭
[Huawei]sysname ZXB-317-A
[ZXB-317-A]i g0/0/1
[ZXB-317-A-GigabitEthernet0/0/1]ip a 2.2.2.1 24
[ZXB-317-A-GigabitEthernet0/0/1]int g0/0/0
[ZXB-317-A-GigabitEthernet0/0/0]ip a 1.1.1.1 30
[ZXB-317-A-GigabitEthernet0/0/0]qu
[ZXB-317-A]ip route-static 3.3.3.0 24 1.1.1.2
{% endhighlight %}

## R2
{% highlight cli %}
<Huawei>sy
[Huawei]undo in e
[Huawei]sysn ZXB-317-B
[ZXB-317-B]i g0/0/0
[ZXB-317-B-GigabitEthernet0/0/0]ip a 1.1.1.2 30
[ZXB-317-B-GigabitEthernet0/0/0]int g0/0/1
[ZXB-317-B-GigabitEthernet0/0/1]ip a 3.3.3.1 24
[ZXB-317-B-GigabitEthernet0/0/1]qu
[ZXB-317-B]ip route-static 2.2.2.0 24 1.1.1.1
{% endhighlight %}


## PC 地址配置

|     PC      |        IP地址      |      子网掩码       |        网关        |
|   :----:    |        :----:      |      :----:        |       :----:       |
|    PC_1     |       2.2.2.2      |   255.255.255.0    |       2.2.2.1      |
|    PC_2     |       3.3.3.2      |   255.255.255.0    |       3.3.3.1      |
