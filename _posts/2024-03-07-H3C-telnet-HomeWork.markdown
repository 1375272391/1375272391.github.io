---
layout: post
title:  "作业3：Telnet远程登录实验"
author: XM137
date:   2024-03-07 23:00:00 +0800
categories: Daily
tags: H3C HCL
---
## 题目如下
> 1.根据如下拓扑完成实验
>
> 2.要求：
>
> 按照拓扑使用HCL模拟器实验
>
> 配置要求：
>
> 1.按照拓扑连线，并更改设备用户名为自己姓名首字母 （10分）
>
> 2.按要求正确配置IP地址，路由器配置（10分），终端配置（10分）
>
> 3.配置Telnet远程登录，选择password验证方式或scheme验证方式，选择scheme验证方式用户名设置为自己姓名首字母，密码自定义（40分）
>
> 4.PC端远程登录测试，进入路由器并显示当前配置信息。（20分）
>
> 5.所有配置以截图方式上传。包含PC终端IP配置，路由器配置和验证成功截图（10分）


![作业图片](https://p.ananas.chaoxing.com/star3/origin/29b7925834b7c73f2a9502275b5e72d0.PNG)
-------------

## 解答如下
使用HCL创建并启动设备以后，输入命令

```CLI
system-view  ## 使用该命令进入系统视图
sysname xxx  ## 根据需要将xxx修改后执行
interface GigabitEthernet 0/1 ## 进入G 0/1 口
ip address 10.1.23.1 28 ## 根据题目要求设置IP地址及子网掩码
quit
telnet server enable ## 启用telnet服务
line vty 0 4 ## 进入vty接口 并设置0-4 共计5个用户
authentication-mode scheme ## 修改认证模式为用户名密码认证
quit
local-user xxx ## 创建账户 xxx
password simple xxxxxxxxxx ## 设置密码，至少10位，至少包含一个字母
service-type telnet # #设置服务模式为telnet
authorization-attribute user-role network-admin ##设置用户角色
quit
save ##保存
Y ##确认

```

## PC配置如下
![PC配置](https://p.ananas.chaoxing.com/star3/origin/257b206f021d0de55b876486da7ef4bd.png)
<br>
右键PC打开命令行，键盘敲几行回车<br>
```WARNING
谁TM再说输入密码没有显示，直接叉出去
```
<br>
```CLI
telnet 10.1.23.1
```
输入上一步设置的用户名密码<br>

登录完成后
```CLI
system-view ## 进入系统试图
display current-configuration ## 显示当前配置信息
```
![作业](https://p.ananas.chaoxing.com/star3/origin/0637cb2115f87bea1fe177296b15e3e2.png)

再次输入几行空格，打印其余输出<br>

![其余输出](https://p.ananas.chaoxing.com/star3/origin/c4862f4872e9c69ae4bd1b531dc43ecf.png)
-------------