---
layout: post
title: Mac Wireshark 监听远程主机端口的网络包
category: 技术
tags: [技术,网络,Wireshark]
keywords: 技术,网络,Wireshark
---


### 面对的问题

```
在服务器XXX上，FluentD正在监听自己的5140端口。
客户正往服务器XXX发送的Netflow,Syslog数据。
```

### Use tshark (Terminal-based Wireshark)

```
FluentD is listening on port 5140 for 'netflow'
$ tshark -f 'udp port 5140' -i any
$ tshark -f "udp port 5140" -i any -V -d "udp.port==5140,cflow"

The output would be in binary without decode by '-d' option

Result:
"IP 1->IP 2 UDP 312 Source port: 36663  Destination port: 5140"

Problem is that I can not view the content of the netflow data.


FluentD is listening on port 42185 for 'syslog'
$ tshark -f 'udp port 42185' -i any

Result:
"Syslog 96 LOCAL4.CRIT: %ASA-2: System Memory usage reached 90%\n"
```

### Wireshark抓取远程服务器上端口的数据包

#### No “add remote interface” on Mac OS Wireshark
```
Wireshark has the "add remote interface" function.
It can capture packets from a remote interface on another server.

But, can't find "add remote interface" on
Wireshark installed on mac os x.

So use terminal and commands
To capture packets from a remote interface on another server.
```

#### Commands to remote capturing
```
Terminal 1:
wireshark -k -i /tmp/pipes/netflow.cap

Terminal 2:
ssh ubuntu@10.157.8.243 "tcpdump -s 10000 -U -n  -w - -i any 'port 5140'" > /tmp/pipes/netflow.cap
```

#### Tcpdump permissions problem
```
Here I use tcmdump with the para -i any.
And you might encouter "tcpdump: XXXXX: Permission denied" problem.
So we need to make tcpdump have the permission.

1. Set permissions
sudo chmod 777 /usr/sbin/tcpdump
2. Give tcpdump the necessary permissions
sudo setcap cap_net_raw,cap_net_admin=eip /usr/sbin/tcpdump
```

[Reference 1: No "add remote interface" and use remote capturing](https://ask.wireshark.org/questions/12644/wireshark-18-installed-on-mac-os-x-107-dont-find-add-remote-interface)

[Reference 2: Using wireshark with remote capturing](http://blog.nielshorn.net/2010/02/using-wireshark-with-remote-capturing/)

[Reference 3: Tcpdump permissions problem](https://askubuntu.com/questions/530920/tcpdump-permissions-problem)
