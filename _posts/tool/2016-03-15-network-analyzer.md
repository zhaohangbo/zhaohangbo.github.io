---
layout: post
title: 网络数据包抓取和分析工具
category: 工具
tags: [工具,Network]
keywords: 工具,Network
---


### wireshark

- powerfull sniffer which can decode lots of protocols, lots of filters.

### tshark

- command line version of wireshark [[Examples]](http://linuxsimba.com/tshark-examples/)

### dumpcap (part of wireshark)

- can only capture traffic and can be used by wireshark / tshark

### tcpdump

- limited protocol decoding but available on most *NIX platforms

### ettercap

- used for injecting traffic not sniffing

```
All tools use libpcap (on windows winpcap) for sniffing.

Wireshark / tshark / dumpcap can use tcpdump filter syntax as capture filter.



As tcpdump is available on most *NIX system I usually use tcpdump.

Depending on the problem I sometimes use tcpdump to capture traffic

and write it to a file and the later use wireshark to analyze it.

If available I use tshark but if the problem gets more complicated

I still like to write data to a file, then use Wireshark for analysis.

```
