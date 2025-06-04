---
layout: post
title:  "CS Payload 一点探究"
author: XM137
date:   2025-06-04 23:49:06 +0800
categories: Cobalt-Strike
tags: Cobalt-Strike
---

学长给了一个shellcode加载器[MentalityXt/bypassAV][bypassAV] <br>
用于加载CS payload 实现一定程度的免杀<br>
这个shellcode 加载器加载一个经由AES加密的payload,实现免杀<br>
虽然最后还是被卡巴检测到了...

原作者给了一个exe，但缺少库文件，我们重新编译了一份，使其与所需DLL一同编译输出

首先我们需要先要生成一份payload<br>
完成后，它的内容如似:
```C
unsigned char buf[] = "\x4d\x5a\x41\x52\x55............";
```
其实有好长一段，在这里并未放全<br>
这个shellcode跟乱码似的，它到底是啥呢<br>
使用Python将其写入到文件
```Python
b = b"\x4d\x5a\x41\x52\x55............"
f = open('payload.bin', "wb")
f.write(b)
f.close()
```
![](/assets/NetSec/20250604/ida.webp)
![](/assets/NetSec/20250604/die.webp)

其实已经试过了，IDA可以查看伪码<br>
die也告知这是一个DLL...

[bypassAV]: https://github.com/MentalityXt/bypassAV