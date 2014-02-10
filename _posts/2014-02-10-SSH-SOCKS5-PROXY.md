---
layout: default 
title:  "SSH SOCKS5 代理"
date:   2014-02-10 11:33:02
categories: git 
---

###SSH SOCKS5 代理
ssh功能强大，可作为SOCKS5代理，只要你能用ssh登录远端Linux主机，就能将远端Linux主机当作SOCKS5代理，如果远端Linux主机在国外，那么就能翻越GFW，访问Youtube.
参考:[SSH SOCKS5 Proxy](http://people.arsc.edu/~murakami/SSH_SOCKS5_Proxy/)

* 本地端是Linux平台
```
`ssh -f -C2qTnN -D 1080 <username>@<remote SSH server>`
```
```
ssh参数解释：
`-f`:ssh在后台运行
`-C`:启用数据压缩
`-2`:使用SSH v2版本. 
`-q`:Quiet 模式
`-T`:关闭 pseudo-tty allocation
`-n`:禁止从stdin读取数据.当ssh在后台运行时，这项参数很有用
`-N`:不进入远程登录命令模式.当ssh仅做为端口转发时，这项参数很有用
`-D`:`[bind_address : ] port`.设置本地"Dynamic"转发端口
```
结束ssh进程:
找到ssh进程的PID号
```
`ps -ef | grep 'ssh -f' | grep -v grep`
```
结束ssh进程
```
`kill -9 <PID>`
```
* 本地端是Windows
可使用putty作为ssh客户端,使用putty tunnel功能.参见[利用PuTTY的SSH Tunnels实现安全的代理](http://www.huluboke.com/putty-ssh-tunnels/)

* 浏览器设置
浏览器设置为SOCKS5代理，地址为localhost,端口为上面设置的端口:1080.Chrome可使用SwitchySharp插件设置SOCKS5代理.

> Written with [StackEdit](https://stackedit.io/).
