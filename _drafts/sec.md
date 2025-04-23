2025安徽省网络分布式大赛——漏洞挖掘与防范（高职组）初赛2

1、F5

叫F5隐写

一张图，Stegsolve用没看出来有什么
丢进Hex 发现有个 123456

下载工具

C:\Users\XM137>git clone https://github.com/matthewgao/F5-steganography
Cloning into 'F5-steganography'...
remote: Enumerating objects: 69, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 69 (delta 0), reused 2 (delta 0), pack-reused 64 (from 1)
Receiving objects: 100% (69/69), 257.72 KiB | 1.03 MiB/s, done.
Resolving deltas: 100% (9/9), done.

C:\Users\XM137>cd F5-steganography

先看看参数

C:\Users\XM137\F5-steganography>java Extract
java Extract [Options] "image.jpg"
Options:
        -p password (default: abc123)
        -e extractedFileName (default: output.txt)

Author: Andreas Westfeld, westfeld@inf.tu-dresden.de

用上一步给出的密码

C:\Users\XM137\F5-steganography>java Extract C:\Users\XM137\Downloads\20250420省赛初赛\F5\2.jpg -p 123456
Huffman decoding starts
Permutation starts
138624 indices shuffled
Extraction starts
Length of embedded file: 42 bytes
(1, 127, 7) code used

C:\Users\XM137\F5-steganography>type output.txt
flag{a8b2cad0-0786-4a96-ae1a-1d60e3d2cf5e}

2、
https://www.cnblogs.com/b1ank/p/13554853.html



