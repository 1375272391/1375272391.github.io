1、Auxiliary（辅助模块）

为渗透测试信息搜集提供了大量的辅助模块支持

2、Exploits（攻击模块）

利用发现的安全漏洞或配置弱点对远程目标系统 进行攻击，从而获得对远程目标系统访问权的代码组件。

3、Payload（攻击载荷模块）

利用漏洞之前要先建立一个Payload,其作用是确定漏洞攻击成功之后要执行什么操作，Payload基本上是用于访问远程计算机的反向shell和通过shel植入后门等到被入侵的电脑

4、Post （后渗透攻击模块）

收集更多信息或进一步访问被利用的目标系统

5、Encoders（编码模块）

不能确保所有Metasploit中的exp都可以正常工作，有时候 会遇到防火墙、IPS、IDc等，所有的试图攻击等可能会被 防火墙过滤掉，这时候就需要使用Encoders来对exp进行编码等，用来逃避防火墙、IPS、IDs的检测。

6、options-选项

所有的Exploit和Payload都有一些内置的参数，诸如远程IP、 本地IP、LPORT、RPORT、服务路径、用户名等。这些参数 在利用exp之前需要进行配置，可以使用Show Options命令 来显示具体的选项。


`msfconsole` 启动
search 搜索
use 使用
`set rhost` 远程主机
`set rport` 远程端口
`run` 开始

`set payload`  指定载荷
`show options` 当前选项

`sysinfo` 系统信息

shell 反弹shell

`hashdump`

`load kiwi`
`creds_all`

net user xxx /add
net localgroup administrators xxx /add

screenshot
ps 
migrate PID
getuid
`run post/windows/capture/keylog_recorder` 键盘记录
`execute -f c:\\aaa.exe`
`run vnc` 远程