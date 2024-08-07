---
layout: post
title:  "项目十一 DHCP服务器的配置"
author: XM137
date:   2024-06-18 00:12:00 +0800
categories: Daily
tags: Linux DHCP
---

## 题目

> 一. 简答题（1 questions，100 points in total）
> 1. (简答题)
> 按照以下要求配置一台DHCP服务器：（以下XX均为学号后两位）
> 
> （1）服务器IP地址是192.168.XX.XX，子网掩码是255.255.255.0，网关为192.168.XX.2，DNS地址是10.10.10.10
> 
> （2）客户端可以使用的地址段是192.168.XX.100~192.168.XX.200，其中192.168.XX.166和192.168.XX.188是保留地址
> 
> （3）测试windows客户端自动获取方式配置IP
> 
> （4）测试第一台Linux客户端使用保留地址192.168.XX.166，主机名是名字的拼音。
> 
> （5）测试第二台Linux客户端使用保留地址192.168.XX.188，主机名是学号。
> 
> 提交截图：
> 
> （1）windows客户端网络参数截图
> 
> （2）第一台Linux客户端网络参数截图
> 
> （3）第二台Linux客户端网络参数截图
> 
> （4）完成以上任务后的服务端dhcpd.conf文件内容截图

## DHCP
#### 在开始之前，请确保你的虚拟机网卡连接为NAT，我们需要使用互联网安装DHCP
![](/assets/Daily-image/20240617/image2.png)
#### 使用以下命令安装dhcp

```CLI
yum install dhcp -y
```
#### 若出现其他进程占用，可尝试重启
#### 开启DHCP开机自启
```CLI
systemctl enable dhcpd
```

## 配置虚拟网卡网络
#### 打开虚拟网络编辑器,选中仅主机,修改IP地址，关闭使用本地DHCP，
#### 图中的17替换为你自己的数字即可，点击确定
![](/assets/Daily-image/20240617/image8.png)

## 虚拟机内网络配置
#### 请改为手动，IP地址如图所示，图中的17替换为你自己的数字即可，点击Apply
#### 重新开关一次网络开关
![](/assets/Daily-image/20240617/image4.png)

## 配置DHCP服务
#### 请一定先配置好主机网络以后，再启动DHCP服务
```CLI
vim /etc/dhcp/dhcpd.conf
```
#### 文本内容如下
{% highlight text %}
ddns-update-style none;
log-facility local7;
subnet 192.168.17.0 netmask 255.255.255.0{
        range 192.168.17.100 192.168.17.165;
        range 192.168.17.167 192.168.17.187;
        range 192.168.17.189 192.168.17.200;
        option domain-name-servers 10.10.10.10;
        option routers 192.168.17.2;
        option broadcast-address 192.168.17.255;
        default-lease-time 600;
        max-lease-time 7200;
}
{% endhighlight %}
![](/assets/Daily-image/20240617/image3.png)
#### 请将配置中的17替换为你自己的数字即可

## 启动DHCP
#### 如果你的配置文件正确，那么不会有任何回显，代表你的配置文件正确
#### 若报错，请检查你的虚拟机网络是否配置正确，以及DHCP配置是否正确
```CLI
systemctl start dhcpd
```
![](/assets/Daily-image/20240617/image6.png)
#### 关闭防火墙自启
```CLI
systemctl disable firewalld.service
```
#### 将虚拟机关机,修改网卡模式为仅主机模式
![](/assets/Daily-image/20240617/image9.png)
#### 克隆出两个完整的虚拟机，分别开机

#### 在第一个克隆机机修改主机名为你的姓名拼音全拼
```CLI
hostnamectl set-hostname xxxxxxxxxx
```
#### 第二个克隆机修改主机名为你的学号
```CLI
hostnamectl set-hostname 00000000
```

## 静态DHCP
#### 获取克隆机MAC地址
```CLI
ifconfig
```
#### ens33后面的ether,后面即为MAC地址

#### 再次编辑虚拟机DHCP配置文件
```CLI
vim /etc/dhcp/dhcpd.conf 
```
#### host 后跟主机名，hardware ethernet 后跟ifconfig获取到的克隆机MAC地址，fixed-address 后跟静态分配地址，记得把17替换为你自己的
#### 保存文件后，重启DHCP服务
```CLI
systemctl restart dhcpd
```
![](/assets/Daily-image/20240617/image14.png)

{% highlight conf %}
ddns-update-style none;
log-facility local7;
subnet 192.168.17.0 netmask 255.255.255.0{
        range 192.168.17.100 192.168.17.165;
        range 192.168.17.167 192.168.17.187;
        range 192.168.17.189 192.168.17.200;
        option domain-name-servers 10.10.10.10;
        option routers 192.168.17.2;
        option broadcast-address 192.168.17.255;
        default-lease-time 600;
        max-lease-time 7200;
}

host yourFullName{
        hardware ethernet 00:0c:29:81:db:f6;
        fixed-address 192.168.17.166;
}

host yourStuNum{
        hardware ethernet 00:0c:29:5f:cb:4f;
        fixed-address 192.168.17.188;
}
{% endhighlight %}


### 在两台克隆机分别修改连接模式为DHCP，并重新开关网络，点击查看地址
![](/assets/Daily-image/20240617/image10.png)
### 若地址正确，则成功，否则可以再次尝试重新开关网络
![](/assets/Daily-image/20240617/image15.png)
![](/assets/Daily-image/20240617/image16.png)

## Windows下测试，
打开设置--> 网络和Internet-->高级网络设置
#### 展开VMnet1,点击编辑

#### 选中IPV4，点属性，点击自动获得，确定
![](/assets/Daily-image/20240617/image19.png)
![](/assets/Daily-image/20240617/image20.png)
### Win+R 
### cmd
```CLI
ipconfig /release ## 释放地址，该操作会使你断网
ipconfig /renew ## 重新获取地址，过程较慢
```

#### 实际上在上一步修改完成后，就已经获取到了由虚拟机分配到的IP地址
#### 可以直接使用
```CLI
ipconfig
```
### 即可看到地址，用哪种自己选
![](/assets/Daily-image/20240617/image21.png)
