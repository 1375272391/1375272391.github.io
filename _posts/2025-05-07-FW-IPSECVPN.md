---
layout: post
title:  "实验 13 IPSECVPN 配置"
author: XM137
date:   2025-05-07 23:32:20 +0800
categories: FW
tags: USG6000V FW IPSEC VPN
---

## 需要为`IPSEC`添加策略放行协商报文<br> 预共享密钥、IP地址自行替换

### 拓扑 **[IPSECVPN.zip](/assets/ENSP/20250507/IPSECVPN.zip)**


## 地址配置

|    设备     |        IP地址      |       子网掩码       |        网关        |
|   :----:    |        :----:      |       :----:        |       :----:       |
|     PC1     |    192.168.17.10   |    255.255.255.0    |    192.168.17.1    |
|     PC2     |    192.168.18.10   |    255.255.255.0    |    192.168.18.1    |


## AR
{% highlight cli %}
<Huawei>sy
[Huawei]int g0/0/1
[Huawei-GigabitEthernet0/0/1]ip a 20.0.0.2 24
[Huawei-GigabitEthernet0/0/1]int g0/0/2
[Huawei-GigabitEthernet0/0/2]ip a 30.0.0.1 24
{% endhighlight %}
<details>
<summary>文本配置</summary>
{% highlight cli %}
sy
int g0/0/1
ip a 20.0.0.2 24
int g0/0/2
ip a 30.0.0.1 24
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

 Info: Your password has been changed. Save the change to survive a reboot. 
*************************************************************************
*         Copyright (C) 2014-2018 Huawei Technologies Co., Ltd.         *
*                           All rights reserved.                        *
*               Without the owner's prior written consent,              *
*        no decompiling or reverse-engineering shall be allowed.        *
*************************************************************************


<USG6000V1>sy
Enter system view, return user view with Ctrl+Z.
[USG6000V1]firewall zone trust 
[USG6000V1-zone-trust]a i g 1/0/1
[USG6000V1-zone-trust]q
[USG6000V1]firewall zone untrust 
[USG6000V1-zone-untrust]a i g 1/0/0
[USG6000V1-zone-untrust]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip a 20.0.0.1 24
[USG6000V1-GigabitEthernet1/0/0]int g1/0/1
[USG6000V1-GigabitEthernet1/0/1]ip a 192.168.17.1 24
[USG6000V1-GigabitEthernet1/0/1]
[USG6000V1-GigabitEthernet1/0/1]acl 3000
[USG6000V1-acl-adv-3000]rule permit ip source 192.168.17.0 0.0.0.255 destination 192.168.18.0 0.0.0.255
[USG6000V1-acl-adv-3000]q   
[USG6000V1]ike proposal 1
[USG6000V1-ike-proposal-1]encryption-algorithm aes-256 
[USG6000V1-ike-proposal-1]authentication-method pre-share 
[USG6000V1-ike-proposal-1]authentication-algorithm sha2-256 
[USG6000V1-ike-proposal-1]integrity-algorithm hmac-sha2-256
[USG6000V1-ike-proposal-1]dh group14
[USG6000V1-ike-proposal-1]q
[USG6000V1]ike peer FW2
[USG6000V1-ike-peer-FW2]pre-shared-key zxb.2317
[USG6000V1-ike-peer-FW2]ike-proposal 1
[USG6000V1-ike-peer-FW2]remote-address 30.0.0.2
[USG6000V1-ike-peer-FW2]q
[USG6000V1]ipsec proposal ipsec
[USG6000V1-ipsec-proposal-ipsec]encapsulation-mode tunnel 
[USG6000V1-ipsec-proposal-ipsec]transform esp 
[USG6000V1-ipsec-proposal-ipsec]esp authentication-algorithm sha2-256 
[USG6000V1-ipsec-proposal-ipsec]esp encryption-algorithm aes-256 
[USG6000V1-ipsec-proposal-ipsec]q
[USG6000V1]ipsec policy xm137 1 isakmp 
[USG6000V1-ipsec-policy-isakmp-xm137-1]security acl 3000
[USG6000V1-ipsec-policy-isakmp-xm137-1]ike-peer FW2
[USG6000V1-ipsec-policy-isakmp-xm137-1]proposal ipsec 
[USG6000V1-ipsec-policy-isakmp-xm137-1]sec
[USG6000V1-policy-security]rule n ping
[USG6000V1-policy-security-rule-ping]source-zone trust 
[USG6000V1-policy-security-rule-ping]source-zone untrust
[USG6000V1-policy-security-rule-ping]destination-zone untrust 
[USG6000V1-policy-security-rule-ping]destination-zone trust 
[USG6000V1-policy-security-rule-ping]ac p
[USG6000V1-policy-security-rule-ping]q
[USG6000V1-policy-security]rule n ipsec
[USG6000V1-policy-security-rule-ipsec]source-zone local 
[USG6000V1-policy-security-rule-ipsec]source-zone untrust 
[USG6000V1-policy-security-rule-ipsec]destination-zone untrust 
[USG6000V1-policy-security-rule-ipsec]destination-zone local 
[USG6000V1-policy-security-rule-ipsec]ac p
[USG6000V1-policy-security-rule-ipsec]dis th
[USG6000V1-policy-security-rule-ipsec]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ipsec policy xm137 
[USG6000V1-GigabitEthernet1/0/0]q
[USG6000V1]ip route-static 0.0.0.0 0 20.0.0.2
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
firewall zone trust 
a i g 1/0/1
q
firewall zone untrust 
a i g 1/0/0
int g1/0/0
ip a 20.0.0.1 24
int g1/0/1
ip a 192.168.17.1 24
acl 3000
rule permit ip source 192.168.17.0 0.0.0.255 destination 192.168.18.0 0.0.0.255
q   
ike proposal 1
encryption-algorithm aes-256 
authentication-method pre-share 
authentication-algorithm sha2-256 
integrity-algorithm hmac-sha2-256
dh group14
q
ike peer FW2
pre-shared-key zxb.2317
ike-proposal 1
remote-address 30.0.0.2
q
ipsec proposal ipsec
encapsulation-mode tunnel 
transform esp 
esp authentication-algorithm sha2-256 
esp encryption-algorithm aes-256 
q
ipsec policy xm137 1 isakmp 
security acl 3000
ike-peer FW2
proposal ipsec 
sec
rule n ping
source-zone trust 
source-zone untrust
destination-zone untrust 
destination-zone trust 
ac p
q
rule n ipsec
source-zone local 
source-zone untrust 
destination-zone untrust 
destination-zone local 
ac p
int g1/0/0
ipsec policy xm137 
q
ip route-static 0.0.0.0 0 20.0.0.2

{% endhighlight %}
</details>

## FW2
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
[USG6000V1]firewall zone trust 
[USG6000V1-zone-trust]a i g 1/0/1
[USG6000V1-zone-trust]q
[USG6000V1]firewall zone untrust 
[USG6000V1-zone-untrust]a i g 1/0/0
[USG6000V1-zone-untrust]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ip a 30.0.0.2 24
[USG6000V1-GigabitEthernet1/0/0]int g1/0/1
[USG6000V1-GigabitEthernet1/0/1]ip a 192.168.18.1 24
[USG6000V1-GigabitEthernet1/0/1]acl 3000
[USG6000V1-acl-adv-3000]rule permit ip source 192.168.18.0 0.0.0.255 destination 192.168.17.0 0.0.0.255
[USG6000V1-acl-adv-3000]q   
[USG6000V1]ike proposal 1
[USG6000V1-ike-proposal-1]encryption-algorithm aes-256 
[USG6000V1-ike-proposal-1]authentication-method pre-share 
[USG6000V1-ike-proposal-1]authentication-algorithm sha2-256 
[USG6000V1-ike-proposal-1]integrity-algorithm hmac-sha2-256
[USG6000V1-ike-proposal-1]dh group14
[USG6000V1-ike-proposal-1]q
[USG6000V1]ike peer FW1
[USG6000V1-ike-peer-FW1]pre-shared-key zxb.2317
[USG6000V1-ike-peer-FW1]ike-proposal 1
[USG6000V1-ike-peer-FW1]remote-address 20.0.0.1
[USG6000V1-ike-peer-FW1]q
[USG6000V1]ipsec proposal ipsec
[USG6000V1-ipsec-proposal-ipsec]encapsulation-mode tunnel 
[USG6000V1-ipsec-proposal-ipsec]transform esp 
[USG6000V1-ipsec-proposal-ipsec]esp authentication-algorithm sha2-256 
[USG6000V1-ipsec-proposal-ipsec]esp encryption-algorithm aes-256 
[USG6000V1-ipsec-proposal-ipsec]q
[USG6000V1]ipsec policy xm137 1 isakmp 
[USG6000V1-ipsec-policy-isakmp-xm137-1]security acl 3000
[USG6000V1-ipsec-policy-isakmp-xm137-1]ike-peer FW1
[USG6000V1-ipsec-policy-isakmp-xm137-1]proposal ipsec 
[USG6000V1-ipsec-policy-isakmp-xm137-1]sec
[USG6000V1-policy-security]rule n ping
[USG6000V1-policy-security-rule-ping]source-zone trust 
[USG6000V1-policy-security-rule-ping]source-zone untrust
[USG6000V1-policy-security-rule-ping]destination-zone untrust 
[USG6000V1-policy-security-rule-ping]destination-zone trust 
[USG6000V1-policy-security-rule-ping]ac p
[USG6000V1-policy-security-rule-ping]q
[USG6000V1-policy-security]rule n ipsec
[USG6000V1-policy-security-rule-ipsec]source-zone local 
[USG6000V1-policy-security-rule-ipsec]source-zone untrust 
[USG6000V1-policy-security-rule-ipsec]destination-zone untrust 
[USG6000V1-policy-security-rule-ipsec]destination-zone local 
[USG6000V1-policy-security-rule-ipsec]ac p
[USG6000V1-policy-security-rule-ipsec]int g1/0/0
[USG6000V1-GigabitEthernet1/0/0]ipsec policy xm137 
[USG6000V1-GigabitEthernet1/0/0]q
[USG6000V1]ip route-static 0.0.0.0 0 30.0.0.1
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
firewall zone trust 
a i g 1/0/1
q
firewall zone untrust 
a i g 1/0/0
int g1/0/0
ip a 30.0.0.2 24
int g1/0/1
ip a 192.168.18.1 24
acl 3000
rule permit ip source 192.168.18.0 0.0.0.255 destination 192.168.17.0 0.0.0.255
q   
ike proposal 1
encryption-algorithm aes-256 
authentication-method pre-share 
authentication-algorithm sha2-256 
integrity-algorithm hmac-sha2-256
dh group14
q
ike peer FW1
pre-shared-key zxb.2317
ike-proposal 1
remote-address 20.0.0.1
q
ipsec proposal ipsec
encapsulation-mode tunnel 
transform esp 
esp authentication-algorithm sha2-256 
esp encryption-algorithm aes-256 
q
ipsec policy xm137 1 isakmp 
security acl 3000
ike-peer FW1
proposal ipsec 
sec
rule n ping
source-zone trust 
source-zone untrust
destination-zone untrust 
destination-zone trust 
ac p
q
rule n ipsec
source-zone local 
source-zone untrust 
destination-zone untrust 
destination-zone local 
ac p
int g1/0/0
ipsec policy xm137 
q
ip route-static 0.0.0.0 0 30.0.0.1
{% endhighlight %}
</details>

## 查看隧道建立状态
### 在任一`FW`上<br>执行: 
```CLI
display ike sa
display ipsec sa
```
#### 正常来说两个命令都应该有数据 <br>若仅只有时间，请稍作等待<br> 若等待较长时间仍无显示，请检查配置

#### 命令示例如下：
{% highlight cli %}
[USG6000V1]display ike sa
2025-05-07 15:10:12.750 

IKE SA information :
 Conn-ID    Peer                                          VPN              Flag(s)               Phase  RemoteType  RemoteID        
------------------------------------------------------------------------------------------------------------------------------------
 3          30.0.0.2:500                                                   RD|ST|A               v2:2   IP          30.0.0.2        
 2          30.0.0.2:500                                                   RD|ST|A               v2:1   IP          30.0.0.2        

  Number of IKE SA : 2
------------------------------------------------------------------------------------------------------------------------------------

 Flag Description:
 RD--READY   ST--STAYALIVE   RL--REPLACED   FD--FADING   TO--TIMEOUT
 HRT--HEARTBEAT   LKG--LAST KNOWN GOOD SEQ NO.   BCK--BACKED UP
 M--ACTIVE   S--STANDBY   A--ALONE  NEG--NEGOTIATING


[USG6000V1]display ipsec sa
2025-05-07 15:10:28.130 

ipsec sa information:

===============================
Interface: GigabitEthernet1/0/0
===============================

  -----------------------------
  IPSec policy name: "xm137"
  Sequence number  : 1
  Acl group        : 3000
  Acl rule         : 5
  Mode             : ISAKMP
  -----------------------------
    Connection ID     : 3
    Encapsulation mode: Tunnel
    Holding time      : 0d 0h 14m 44s
    Tunnel local      : 20.0.0.1:500
    Tunnel remote     : 30.0.0.2:500
    Flow source       : 192.168.17.0/255.255.255.0 0/0-65535
    Flow destination  : 192.168.18.0/255.255.255.0 0/0-65535

    [Outbound ESP SAs] 
      SPI: 194525348 (0xb9838a4)
      Proposal: ESP-ENCRYPT-AES-256 ESP-AUTH-SHA2-256-128
      SA remaining key duration (kilobytes/sec): 10485760/2717
      Max sent sequence-number: 5         
      UDP encapsulation used for NAT traversal: N
      SA encrypted packets (number/bytes): 4/240

    [Inbound ESP SAs] 
      SPI: 197637217 (0xbc7b461)
      Proposal: ESP-ENCRYPT-AES-256 ESP-AUTH-SHA2-256-128
      SA remaining key duration (kilobytes/sec): 10485760/2717
      Max received sequence-number: 1
      UDP encapsulation used for NAT traversal: N
      SA decrypted packets (number/bytes): 3/180
      Anti-replay : Enable
      Anti-replay window size: 1024
{% endhighlight %}
## 测试
### 隧道成功建立后，可以开始`ping`测试
{% highlight cli %}
PC>ping 192.168.18.10

Ping 192.168.18.10: 32 data bytes, Press Ctrl_C to break
Request timeout!
From 192.168.18.10: bytes=32 seq=2 ttl=126 time=15 ms
From 192.168.18.10: bytes=32 seq=3 ttl=126 time=16 ms
From 192.168.18.10: bytes=32 seq=4 ttl=126 time=31 ms
From 192.168.18.10: bytes=32 seq=5 ttl=126 time=16 ms

--- 192.168.18.10 ping statistics ---
  5 packet(s) transmitted
  4 packet(s) received
  20.00% packet loss
  round-trip min/avg/max = 0/19/31 ms

PC>
{% endhighlight %}
## 参考文档:<br>[查看VPN状态][LTE-IPSEC] <br> [防火墙在LTE IPSec解决方案中的应用][State-IPSEC]

[LTE-IPSEC]: https://support.huawei.com/enterprise/zh/doc/EDOC1100062976/a416fee6?idPath=24030814|9856724|21430823|253423179|22690082
[State-IPSEC]: https://support.huawei.com/enterprise/zh/doc/EDOC1000160160/2d53d255