---
layout: post
title:  "基于Docker的DVWA"
excerpt_image: /assets/docker-logos/PNG/docker-logo-blue-900.png
author: XM137
date:   2025-04-13 17:10:37 +0800
categories: Docker
tags: Docker DVWA
---

![banner](/assets/docker-logos/SVG/docker-logo-blue.svg)

使用`Docker`搭建`DVWA`相当的简单，因为有做好的镜像，直接拉取就能使用<br>
写这篇文章主要是为了再次记忆其中的一些参数

`docker hub` 链接 [docker_dvwa][docker_dvwa]

文档中给出了命令
```Bash
docker run --rm -it -p 80:80 vulnerables/web-dvwa
```

直接运行会发生什么，在本地没有对应镜像的情况下，它会开始拉取镜像

`--rm`在这里是什么意思<br>
在容器停止运行后，删除容器

`-p`使用端口映射，两个都是`80`怎么理解<br>
前一个代表宿主机端口，后一个代表容器端口

笔者前些时间在`Linux`上写了一个`systemd`服务<br>
用于在系统启动时开启docker里的靶机

那个靶机搭建的过程笔者并不了解，只是为其添加启动服务

所有的命令都是基于容器来写的，仅仅针对现有容器

DVWA靶机，一般情况下并不需要保留数据，每次运行都是一个新的容器，虽然可以继续使用name将其停止<br>
但是笔者想用其他方式将其停止

```Bash
root@xm137-virtual-machine:~# docker ps ## 可以列出当前正在运行的容器
 ##添加参数-q，可以只显示容器id
--filter ## 过滤条件
--filter ancestor ## 以镜像名称过滤
--filter ancestor=vulnerables/web-dvwa ## 过滤镜像名称为`vulnerables/web-dvwa`的容器
```

完整命令如下
```Bash
root@xm137-virtual-machine:~# docker ps -q --filter ancestor=vulnerables/web-dvwa
bcf0d5cf1b2e
```

我们得到了容器ID

下一步，停止容器
```Bash
root@xm137-virtual-machine:~# docker stop $(docker ps -q --filter ancestor=vulnerables/web-dvwa)
bcf0d5cf1b2e
```


参考链接: <br>
[docker container ls][docker_ls] <br>
[docker_dbwa][docker_dvwa]

[docker_ls]: https://docs.docker.com/reference/cli/docker/container/ls/
[docker_dvwa]: https://hub.docker.com/r/vulnerables/web-dvwa