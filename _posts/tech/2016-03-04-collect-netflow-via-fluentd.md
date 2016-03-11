---
layout: post
title: Collect Netflow Data via FluentD
category: 技术
tags: [FluentD, Netflow]
keywords: FluentD, Netflow
---

### Install td-agent (Fluentd)

```
For Trusty,
curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-trusty-td-agent2.sh | sh
```
[[Installation Instruction Here]](http://docs.fluentd.org/v0.12/categories/installation)


### Before Install Plugins

```
Before install geoip plugin, you need to do 2 things :

1. GeoIP Legacy City Database Installation :
   [Instruction]( https://dev.maxmind.com/geoip/legacy/install/city/)
      $ gunzip GeoLiteCity.dat.gz
      $ mv GeoLiteCity.dat /Your/Path/To/GeoIP/, usually /usr/share/GeoIP
2. Install dependent c library as below :
   For RHEL/CentOS
      $ sudo yum group install "Development Tools"
      $ sudo yum install geoip-devel --enablerepo=epel
   For Ubuntu/Debian
      $ sudo apt-get install build-essential
      $ sudo apt-get install libgeoip-dev
   For OS X
      $ brew install geoip
```

### Plugin
```
1. fluent-plugin-record-reformer
  $ sudo td-agent-gem install fluent-plugin-record-reformer
2. fluent-plugin-secure-forward
  $ sudo td-agent-gem install fluent-plugin-secure-forward -v 0.2.6
3. fluent-plugin-netflow
  $ sudo td-agent-gem install fluent-plugin-netflow
4. fluent-plugin-geoip
  $ sudo td-agent-gem install fluent-plugin-geoip
```

### Config File

**Path:/etc/td-agent/td-agent.config**

```
## match tag=debug.** and dump to console
<match debug.**>
  type stdout
</match>

####
## Source descriptions:
##

## built-in TCP input
## @see http://docs.fluentd.org/articles/in_forward
<source>
  type forward
</source>

## built-in UNIX socket input
#<source>
#  type unix
#</source>

# HTTP input
# POST http://localhost:8888/<tag>?json=<json>
# POST http://localhost:8888/td.myapp.login?json={"user"%3A"me"}
# @see http://docs.fluentd.org/articles/in_http
<source>
  type http
  port 8888
</source>

## live debugging agent
<source>
  type debug_agent
  bind 127.0.0.1
  port 24230
</source>

<source>
  type syslog
  port 42185
  tag  syslog
</source>

<match syslog.**>
  type record_reformer
  tag logs.${tag}.username-token
  <record>
    timestamp ${time}
  </record>
</match>

<source>
  type netflow
  tag netflow.event
  # optional parameters
  #bind 192.133.156.7
  bind 0.0.0.0
  port 5140
  # optional parser parameters
  cache_ttl 6000
  versions [5, 9]
</source>

#<match togeoip.netflows>
<match netflow.event.**>
  type                   geoip
  geoip_lookup_key       ipv4_src_addr
  geoip_database         /usr/share/GeoIP/GeoLiteCity.dat
  <record>
    geoip.location      ${latitude["ipv4_src_addr"]},${longitude["ipv4_src_addr"]}
  </record>
  tag                    netflows.geoip.attached
  skip_adding_null_record  true
</match>

<match netflows.geoip.attached.** >
  type record_reformer
  #tag logs.${tag}.<YOUR USERNAME HERE>-<YOUR TOKEN HERE>
   tag logs.${tag}.username-token
  <record>
    @timestamp ${time}
  </record>
</match>

<match logs.**>
#  type copy
#  <store>
#        type file
#        path /var/log/td-agent/netflow-data/netflow.data
#  </store>
#  <store>
  	type secure_forward
  	shared_key cisco_zeus_log_metric_pipline
  	self_hostname fluentd-client1.ciscozeus.io
  	secure false
  	keepalive 10
  	<server>
	     	host data01.ciscozeus.io
  	</server>
#  </store>
#  <store>
#        type stdout
#  </store>
</match>

####
## Examples:
##

## File input
## read apache logs continuously and tags td.apache.access
#<source>
#  type tail
#  format apache
#  path /var/log/httpd-access.log
#  tag td.apache.access
#</source>

## File output
## match tag=local.** and write to file
#<match local.**>
#  type file
#  path /var/log/td-agent/access
#</match>

## Forwarding
## match tag=system.** and forward to another td-agent server
#<match system.**>
#  type forward
#  host 192.168.0.11
#  # secondary host is optional
#  <secondary>
#    host 192.168.0.12
#  </secondary>
#</match>

## Multiple output
## match tag=td.*.* and output to Treasure Data AND file
#<match td.*.*>
#  type copy
#  <store>
#    type tdlog
#    apikey API_KEY
#    auto_create_table
#    buffer_type file
#    buffer_path /var/log/td-agent/buffer/td
#  </store>
#  <store>
#    type file
#    path /var/log/td-agent/td-%Y-%m-%d/%H.log
#  </store>
#</match>
```

### Start Td-agent

```
  $ sudo /etc/init.d/td-agent start
```

### Start Send Your NetFlow Data

```
Tail your td-agent log:
  $ sudo tail -f /var/log/td-agent/td-agent.log

You will see the following info:

[info]: listening fluent socket on 0.0.0.0:24224
[info]: listening dRuby uri="druby://127.0.0.1:24230" object="Engine"
[info]: listening syslog socket on 0.0.0.0:42185 with udp
[info]: listening netflow socket on 0.0.0.0:5140 with udp
[info]: connection established to data.awesome.io.

So the agent listening fluent socket on 0.0.0.0:24224.
Now you can configure your network devices and send the data.
```


### Netflow Json Data Sample

```json
{
"version":5,
"flow_seq_num":65469633,
"engine_type":0,
"engine_id":0,
"sampling_algorithm":0,
"sampling_interval":0,
"flow_records":30,
"ipv4_src_addr":"192.133.151.21”,
"ipv4_dst_addr":"172.22.250.141”,
"ipv4_next_hop":"64.71.176.49”,
"input_snmp":5,
"output_snmp":8,
"in_pkts":1,
"in_bytes":60,
"first_switched":"2016-03-02T15:15:36.999Z”,
"last_switched":"2016-03-02T15:15:36.999Z”,
"l4_src_port":63682,
"l4_dst_port":6001,
"tcp_flags":2,"protocol":6,
"src_tos":0,"src_as":0,"dst_as":0,
"src_mask":0,
"dst_mask":0,
"host":"192.133.159.1”,
"geoip.location":"33.918800354003906,-84.06780242919922”,
"timestamp":"2016-03-02 07:15:51 -0800"
}
```

### Dashboard Demo
