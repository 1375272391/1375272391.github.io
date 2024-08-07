---
layout: post
title:  "Linux作业 项目五 作业二 磁盘限额配置"
author: XM137
date:   2024-04-29 22:21:00 +0800
categories: Daily
tags: Linux CentOS7 fdisk quota
---
1. (简答题)
> 操作题：(提交结果测试截图）
> 
> 系统中添加一块2G的新硬盘/dev/sdb，分一个512M的主分区/dev/sdb1，挂载到/data1，在该分区上做用户磁盘限额。
> 
> 要求：用户user1和user2。
> 
> 1） 各有200MB的磁盘使用量（hard），容量超过160MB（soft）开始警告。
>
> 2） 每个用户最多存放4个文件（hard），超过2个文件（soft）开始警告。
> 
> 3） 超过soft限制值后，14天的宽限时间。
> 
> 注意：只提交结果测试截图！！！


#### 添加磁盘的过程不再赘述，直接开始
### 创建分区&&保存分区表
{% highlight cli %}
fdisk /dev/sdb
n
p
1
回车
+512M
w
{% endhighlight %}

### 格式化&&挂载
{% highlight cli %}
mkdir /data1 ## 创建挂载点
mkfs -t ext4 /dev/sdb1 ## 格式化分区
mount /dev/sdb1 /data1 ##挂载分区
{% endhighlight %}

### 创建账户&&改密
{% highlight cli %}
useradd user1 ##创建账户
useradd user2
echo "123456" | passwd --stdin user1 修改密码
echo "123456" | passwd --stdin user2
{% endhighlight %}

### quota准备
{% highlight cli %}
mount -o remount,usrquota,grpquota /data1 ## 配额重挂
quotacheck -avug -mf ## quota 记录文件
quotaon -auvg ## 启动quota
{% endhighlight %}

### 编辑quota规则
```cli
edquota -u user1 ##编辑用户user1 quota 规则
```

```vim
vim 默认命令模式，输入i进入编辑，完成后按下键盘ESC 输入冒号":wq" 保存退出
```

![](/assets/Daily-image/20240429/image1.png)


```cli
edquota -p user1 -u user2 ## 直接将user1的配置给user2
```

```cli
edquota -t ##修改宽限期
```
![](/assets/Daily-image/20240429/image2.png)


### quota测试
{% highlight cli %}
chmod 777 /data1 ## 修改权限
su user1
cd /data1
dd if=/dev/zero of=file1 bs=85M count=1
dd if=/dev/zero of=file2 bs=85M count=1
dd if=/dev/zero of=file3 bs=85M count=1
{% endhighlight %}
![](/assets/Daily-image/20240429/image3.png)

{% highlight cli %}
su user2
touch a
touch b
touch c
touch d
touch e
{% endhighlight %}
![](/assets/Daily-image/20240429/image4.png)
