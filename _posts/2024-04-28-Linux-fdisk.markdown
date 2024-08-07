---
layout: post
title:  "Linux作业 项目五 作业一 磁盘分区与格式化"
author: XM137
date:   2024-04-28 22:14:00 +0800
categories: CentOS-7
tags: Linux CentOS7 fdisk
---
> 项目五 作业一 磁盘分区与格式化
> 
> 1. 在虚拟机上添加一块10G的SCSI硬盘，分为五个区，其中3个主分区，2个逻辑分区，每个分区的大小是2G，提交fdisk  -l   /dev/sdb  截图
> 
> 2. 将分区均格式化为ext4的文件系统，分别挂载在Linux系统中的/data1, /data2, /data3, /data4，/data5目录下使用。使用df  -h 查看挂载情况。
> 
> 3. 往第二个分区写入1G的数据保存在文件abc中

## VMware 配置
![](/assets/Daily-image/20240428/image1.png)
![](/assets/Daily-image/20240428/image2.png)
![](/assets/Daily-image/20240428/image3.png)
![](/assets/Daily-image/20240428/image4.png)
![](/assets/Daily-image/20240428/image5.png)
![](/assets/Daily-image/20240428/image6.png)
![](/assets/Daily-image/20240428/image7.png)
![](/assets/Daily-image/20240428/image8.png)
![](/assets/Daily-image/20240428/image9.png)

## 具体命令行如下
### 开始前请切换到root
## 1 新建分区
{% highlight cli %}
fdisk -l ##查看当前分区
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image1.png)
可以确定 /dev/sdb 为我们所添加的新硬盘 <br>
下一步 开始分区
{% highlight cli %}
fdisk /dev/sdb
{% endhighlight %}
进入以后，输入 
{% highlight cli %}
n
p
1
回车
+2G
{% endhighlight %}

即完成第一分区的创建
![](/assets/Daily-image/20240428/media/image2.png)

继续输入
{% highlight cli %}
n
p
2
回车
+2G
{% endhighlight %}
第二分区完成
{% highlight cli %}
n
p
3
回车
+2G
{% endhighlight %}
第三分区完成

输入p
查看当前新建的分区
![](/assets/Daily-image/20240428/media/image3.png)

下面开始新建拓展分区
{% highlight cli %}
n
e
后面的直接回车即可
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image4.png)

下面开始新建第五分区
{% highlight cli %}
n
回车
+2G
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image5.png)

第六分区
{% highlight cli %}
n
回车
回车
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image6.png)


按下p查看当前分区
### 确认完成后输入 w 保存分区
![](/assets/Daily-image/20240428/media/image7.png)


使用fdisk 查看分区 
{% highlight cli %}
fdisk -l /dev/sdb
{% endhighlight %}
截图提交此题结束
![](/assets/Daily-image/20240428/media/image8.png)

## 2 格式化并挂载
### 格式化分区
{% highlight cli %}
mkfs -t ext4 /dev/sdb1
mkfs -t ext4 /dev/sdb2
mkfs -t ext4 /dev/sdb3
mkfs -t ext4 /dev/sdb5
mkfs -t ext4 /dev/sdb6
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image9.png)

### 创建挂载点
{% highlight cli %}
mkdir /data1
mkdir /data2
mkdir /data3
mkdir /data4
mkdir /data5
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image10.png)

### 挂载分区
{% highlight cli %}
mount /dev/sdb1 /data1
mount /dev/sdb2 /data2
mount /dev/sdb3 /data3
mount /dev/sdb5 /data4
mount /dev/sdb6 /data5
{% endhighlight %}
### 查看挂载
{% highlight cli %}
df -h
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image11.png)
#### 此题结束

## 3 写入文件
{% highlight cli %}
dd if=/dev/zero of=/data2/abc bs=1G count=1
{% endhighlight %}
![](/assets/Daily-image/20240428/media/image12.png)


