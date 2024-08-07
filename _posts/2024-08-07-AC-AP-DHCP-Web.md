---
layout: post
title:  "AC+AP Web配置"
author: XM137
date:   2024-08-07 15:43:00 +0800
categories: Daily
tags: HCL H3C AC AP DHCP default-group Web
---
![](/assets/H3C/2024-08-07/image12.png)
#### 简要说明
<h4>HCL 依赖于 VMBOX <br>
我们直接使用VMBOX的DHCP服务</h4>
![](/assets/H3C/2024-08-07/image1.png)
<h4>在默认情况下：<br>
仅主机网卡地址为 192.168.56.1<br>
DHCP 自动分配从 192.168.56.101 到 192.168.56.254<br>
设置静态IP的时候避免使用这些地址<br>
我们为 AC 设置同网段 静态IP 以便PC宿主机可以访问虚拟机<br>
模拟器使用 Host本地主机 VMBOX仅主机网卡 连接AC</h4>

#### 1、AC Console 配置
{% highlight cli %}
<H3C>sy
[H3C]int vl 1
[H3C-Vlan-interface1]ip a 192.168.56.10 24 ## 
[H3C-Vlan-interface1]ip http enable ## 启用 http 
{% endhighlight %}

#### 创建Web管理账户
{% highlight cli %}
[H3C]local-user admin class manage ## 配置用户分类
[H3C-luser-manage-admin]authorization-attribute user-role network-admin ## 配置用户角色
[H3C-luser-manage-admin]password simple a123456789 ## 最少10位
[H3C-luser-manage-admin]service-type http ## 协议绑定
{% endhighlight %}
<h4>至此，可在PC宿主机使用浏览器进行登录<br>
浏览器地址输入 192.168.56.10 即可</h4>
![](/assets/H3C/2024-08-07/image2.png)

#### 2、启用自动AP && 自动固化 <br >无线配置-->AP管理-->AP全局配置<br> 打开自动AP可以自动添加AP<br> 打开自动固化可以方便后续手工单独配置AP

![](/assets/H3C/2024-08-07/image3.png)

<h4>可以看到AP已经成功添加</h4>
![](/assets/H3C/2024-08-07/image4.png)

#### 3、创建 WLAN服务模板 <br>无线配置 --> 无线网络<br> 点击添加WLAN配置模板
![](/assets/H3C/2024-08-07/image5.png)

<h4>需要添加 WLAN模板名称 和 无线名称，开启 无线服务，点击确定</h4>
![](/assets/H3C/2024-08-07/image6.png)

#### 4、配置 WLAN <br> 无线配置 --> AP管理--> AP组<br> 点击修改默认组
![](/assets/H3C/2024-08-07/image7.png)

<h4>需要指定AP的型号，以及需要开启的频段<br>
这里我们仅开启5Ghz radio 1<br>
点击确定即可</h4>
![](/assets/H3C/2024-08-07/image8.png)

<h4>点击无线服务配置<br>
在 5Ghz 处点击添加</h4>
![](/assets/H3C/2024-08-07/image9.png)

<h4>选择创建的 WLAN无线服务模板<br>
VLAN 不填的话，它默认属于 VLAN1<br>
点击确定即可</h4>
![](/assets/H3C/2024-08-07/image10.png)

<h4>最后点击确定</h4>
![](/assets/H3C/2024-08-07/image11.png)

![](/assets/H3C/2024-08-07/image13.png)


#### 参考: <br> [AC控制器开启Web登录][Web]
[Web]: https://zhiliao.h3c.com/questions/dispcont/115857
