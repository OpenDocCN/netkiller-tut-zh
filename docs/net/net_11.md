# 部分 VII. Array

http://www.arraynetworks.net/

Array APV 3520

Array APV 4600

## 第 33 章 Array CLI

Array 的命令行与 Cisco IOS 极其相似。

## 登录

默认用户名 array, 密码 admin, enable 密码为空

SSH

```

ssh array@array.example.com

ArrayOS Rel.TM.6.5.2.5 build on Fri Apr 30 15:34:51 2010
Copyright (c) 2000-2010 Array NetWorks Inc. All rights reserved.

Type "?" for available commands

Array>

```

### Config 模式

```

Array>enable
Enable password:

Array#configure terminal

Array(config)#

```

## 系统信息

show tech 运行这个命令，用户可以抓取并查看必要的系统信息及网络运行状态。

```

Array(config)#show tech

<<<< Version >>>>

ArrayOS Rel.TM.8.1.1.27 build on Mon Apr 11 15:17:52 2011

        Host name : AN
       System CPU : Intel(R) Xeon(R) CPU           E5620  @ 2.40GHz
    System Module : X8DTH-i
       System RAM : 16451788 kbytes.
 System boot time : Tue Aug 02 14:34:48 GMT (+0000) 2011
     Current time : Thu Aug 04 11:09:04 GMT (+0000) 2011
   System up time :  1 day, 20:34
Platform Bld Date : Wed Oct 27 18:02:57 UTC 2010
     SSL Hardware : No HW Available
   Compression HW : No HW Available
     Power supply : 2U, AC, 2-cords, Redundancy
Network Interface : 6 x Gigabit Ethernet copper
            Model : Array APV 4600
    Serial Number : 1117S0151904600002262015541070
Licensed Features : WebWall  Clustering  L4SLB  L7SLB  Caching
                    SwCompression  LLB  GSLB  QoS  MultiLang  DynRoute
                    FFO
      License Key : cbfc3d11-81e3413c-825b24d0-01b7c2c2-90932980-00000000-00d5d80b-20110902

Array Networks Customer Support
Telephone         : 877-992-7729 (877-MY-ARRAY)
Email             : support@arraynetworks.net
Update            : please contact support for instructions
Website           : http://www.arraynetworks.net

Other Root Version
Rel.TM.8.1.1.27 build on Mon Apr 11 15:17:52 2011

<<<< Running Configuration >>>>

```

```
AN(config)#show version

ArrayOS Rel.TM.8.1.1.27 build on Mon Apr 11 15:17:52 2011

        Host name : AN
       System CPU : Intel(R) Xeon(R) CPU           E5620  @ 2.40GHz
    System Module : X8DTH-i
       System RAM : 16451788 kbytes.
 System boot time : Tue Aug 02 14:34:48 GMT (+0000) 2011
     Current time : Thu Aug 04 11:08:12 GMT (+0000) 2011
   System up time :  1 day, 20:33
Platform Bld Date : Wed Oct 27 18:02:57 UTC 2010
     SSL Hardware : No HW Available
   Compression HW : No HW Available
     Power supply : 2U, AC, 2-cords, Redundancy
Network Interface : 6 x Gigabit Ethernet copper
            Model : Array APV 4600
    Serial Number : 1117S0151904600002262015541070
Licensed Features : WebWall  Clustering  L4SLB  L7SLB  Caching
                    SwCompression  LLB  GSLB  QoS  MultiLang  DynRoute
                    FFO
      License Key : cbfc3d11-81e3413c-825b24d0-01b7c2c2-90932980-00000000-00d5d80b-20110902

Array Networks Customer Support
Telephone         : 877-992-7729 (877-MY-ARRAY)
Email             : support@arraynetworks.net
Update            : please contact support for instructions
Website           : http://www.arraynetworks.net

Other Root Version
Rel.TM.8.1.1.27 build on Mon Apr 11 15:17:52 2011

```

### Configuration

```

Array(config)#show running
Array(config)#show startup

```

### Save configuration

```

Array(config)#write memory

```

### date/time

```
Array(config)#show date
Mon Jan 11 16:44:57 GMT (+0000) 2010

```

```

system date <year> <month> <date>
system time <hour> <minute> <second>
system timezone [timezone_string]

```

ntp

```

ntp on
ntp server <ip> [version]
show ntp

```

### show statistics tcp

```
Array(config)#show statistics tcp
         LISTEN:  1
       SYN_SENT:  0
       SYN_RCVD:  0
    ESTABLISHED:  0
     CLOSE_WAIT:  0
     FIN_WAIT_1:  0
        CLOSING:  0
       LAST_ACK:  0
     FIN_WAIT_2:  0
      TIME_WAIT:  75

```

### show memory

```
Array(config)#show memory
vm.arrayzone:
ITEM                       SIZE     LIMIT    USED    FREE  REQUESTS
Compression data:            96,     8192,      0,   8192,     8192
Compression poll items:      32,     8192,      0,   8192,     8192
slb_vs:                     576,        0,      0,      0,        0
Cache filter host queue:     32,      500,      0,    500,      500
Cache filter host node:      32,      500,      0,    500,      500
Cache filter url regex:      32,      500,      0,    500,      500
cache filter action:         32,      500,      0,    500,      500
ChannelTable:               320,        2,      1,      1,        3
ChannelStat:                160,     1024,      0,   1024,     1024
Cache override urlquery:     32,     1000,      0,   1000,     1000
Cache override regex:        32,     1000,      0,   1000,     1000
Cache override hostname:     32,     1000,      0,   1000,     1000
Cache chunk:                 32,   118746,      0, 118746,   118746
Cache extension:             32,   237492,      0, 237492,   237492
Cache range:                 32,   237492,      0, 237492,   237492
Cache hostname:             128,    65535,      0,  65535,    65535
Cache url:                  256,    65535,      0,  65535,    65535
Cache manager:               32,    65535,      0,  65535,    65535
Cache:                      416,   237492,      0, 237492,   237492
Proxy client:                32,   474984,      0, 474984,   474984
Proxy cookie:               128,   237492,      0, 237492,   237492
Proxy connection:           224,   474984,      0, 474984,   474984
Proxy:                      416,   474984,      0, 474984,   474984
RTS entry:                   32,    40000,      0,  40000,    40000
IP flow entry:               32,    40000,      0,  40000,    40000
Eroute gateway:              96,     2000,      2,   1998,     2002
Eroute rule:                 96,     2000,      4,   1996,     2005
TCP hash node:               32,  1899936,     76, 1899860,  1971188
TCP small pcb:               64,   949968,     75, 949893,  1002343
TCP pcb:                    288,   949968,      1, 949967,  1021221
tcpcb:                      544,     5000,     19,   4981,    57939
socket:                     192,     5000,     45,   4955,    63882

diskspace: available = 1164 Mb, total = 2032 Mb

```

## hostname configuration

```

Array(config)#hostname MyArray

```

## ip configuration

### interface

```

Array(config)#show interface name
Unknown interface name.

Array(config)#show interface
port1(port1): flags=8843<UP,BROADCAST,RUNNING,SIMPLEX> mtu 1500
        inet 192.168.3.20 netmask 0xffffff00 broadcast 192.168.3.255
        ether 00:25:90:05:6d:82
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573l
        Input queue: 311/512 (size/max)
                total: 148722 packets, good: 148722 packets, 14020742 bytes
                broadcasts: 134870, multicasts: 268
                84658 64 bytes, 53370 65-127 bytes,7071 128-255 bytes
                1300 255-511 bytes,2294 512-1023 bytes,29 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 2/512 (size/max)
                total: 41587 packets, good:  41587 packets, 41121234 bytes
                broadcasts: 1, multicasts: 0
                1678 64 bytes, 8223 65-127 bytes,219 128-255 bytes
                3984 255-511 bytes,3020 512-1023 bytes,24463 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec
port2(port2): flags=8842<BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:25:90:05:6d:83
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573l
        Input queue: 248/512 (size/max)
                total: 134862 packets, good: 134862 packets, 11584332 bytes
                broadcasts: 134862, multicasts: 0
                74160 64 bytes, 52527 65-127 bytes,6903 128-255 bytes
                1262 255-511 bytes,10 512-1023 bytes,0 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 0/512 (size/max)
                total: 0 packets, good:  0 packets, 0 bytes
                broadcasts: 0, multicasts: 0
                0 64 bytes, 0 65-127 bytes,0 128-255 bytes
                0 255-511 bytes,0 512-1023 bytes,0 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec
port3(port3): flags=8842<BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:25:90:05:6d:80
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573e_iamt
        Input queue: 248/512 (size/max)
                total: 134859 packets, good: 134859 packets, 11584140 bytes
                broadcasts: 134859, multicasts: 0
                74157 64 bytes, 52527 65-127 bytes,6903 128-255 bytes
                1262 255-511 bytes,10 512-1023 bytes,0 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 0/512 (size/max)
                total: 0 packets, good:  0 packets, 0 bytes
                broadcasts: 0, multicasts: 0
                0 64 bytes, 0 65-127 bytes,0 128-255 bytes
                0 255-511 bytes,0 512-1023 bytes,0 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec
port4(port4): flags=8842<BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:25:90:05:6d:81
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573l
        Input queue: 248/512 (size/max)
                total: 134859 packets, good: 134859 packets, 11584140 bytes
                broadcasts: 134859, multicasts: 0
                74157 64 bytes, 52527 65-127 bytes,6903 128-255 bytes
                1262 255-511 bytes,10 512-1023 bytes,0 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 0/512 (size/max)
                total: 0 packets, good:  0 packets, 0 bytes
                broadcasts: 0, multicasts: 0
                0 64 bytes, 0 65-127 bytes,0 128-255 bytes
                0 255-511 bytes,0 512-1023 bytes,0 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec

bond1(bond1): flags=8843<UP,BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:00:00:00:00:00
        status: no carrier
        webwall status: OFF
        packet drop(output error): 0
        packet drop(not permit): 0
                tcp 0 udp 0 icmp 0
        packet drop(deny): 0
                tcp 0 udp 0 icmp 0

bond2(bond2): flags=8843<UP,BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:00:00:00:00:00
        status: no carrier
        webwall status: OFF
        packet drop(output error): 0
        packet drop(not permit): 0
                tcp 0 udp 0 icmp 0
        packet drop(deny): 0
                tcp 0 udp 0 icmp 0

bond3(bond3): flags=8843<UP,BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:00:00:00:00:00
        status: no carrier
        webwall status: OFF
        packet drop(output error): 0
        packet drop(not permit): 0
                tcp 0 udp 0 icmp 0
        packet drop(deny): 0
                tcp 0 udp 0 icmp 0

```

```

Array(config)#show interface outside
outside(port1): flags=8843<UP,BROADCAST,RUNNING,SIMPLEX> mtu 1500
        inet 192.168.3.20 netmask 0xffffff00 broadcast 192.168.3.255
        ether 00:25:90:05:6d:82
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573l
        Input queue: 238/512 (size/max)
                total: 232641 packets, good: 232641 packets, 21102806 bytes
                broadcasts: 215679, multicasts: 424
                132773 64 bytes, 84622 65-127 bytes,10781 128-255 bytes
                2106 255-511 bytes,2330 512-1023 bytes,29 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 2/512 (size/max)
                total: 51900 packets, good:  51900 packets, 42371369 bytes
                broadcasts: 1, multicasts: 0
                3355 64 bytes, 16623 65-127 bytes,274 128-255 bytes
                4012 255-511 bytes,3056 512-1023 bytes,24580 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec

Array(config)#show interface inside
inside(port2): flags=8842<BROADCAST,RUNNING,SIMPLEX> mtu 1500
        ether 00:25:90:05:6d:83
        media: autoselect (1000baseT <full-duplex>)
        status: active
        webwall status: OFF
        Hardware is i82573l
        Input queue: 289/512 (size/max)
                total: 215833 packets, good: 215833 packets, 18440668 bytes
                broadcasts: 215833, multicasts: 0
                119845 64 bytes, 83292 65-127 bytes,10610 128-255 bytes
                2061 255-511 bytes,25 512-1023 bytes,0 1024-1522 bytes
                0 input errors
                0 runts, 0 giants, 0 Jabbers, 0 CRCs
                0 Flow Control, 0 Fragments, 0 Receive errors
                0 Driver dropped, 0 Frame, 0 Lengths, 0 No Buffers
                0 overruns, Carrier extension errors: 0
        Output queue: 0/512 (size/max)
                total: 0 packets, good:  0 packets, 0 bytes
                broadcasts: 0, multicasts: 0
                0 64 bytes, 0 65-127 bytes,0 128-255 bytes
                0 255-511 bytes,0 512-1023 bytes,0 1024-1522 bytes
                0 output errors
                0 Collisions, 0 Late collisions, 0 Deferred
                0 Single Collisions, 0 Multiple Collisions, 0 Excessive collisions
        0 lost carrier, 0 WDT reset
        packet drop (not permit): 0
                tcp 0          udp 0          icmp 0
        packet drop (deny): 0
                tcp 0          udp 0          icmp 0
        5 minute input rate 0 bits/sec, 0 packets/sec
        5 minute output rate 0 bits/sec, 0 packets/sec

```

### interface name

```

interface name <interface_ID> <interface_name>

Array(config)#interface name port1 outside
The interface's name has been changed from "port1" to "outside".

Array(config)#interface name port2 inside
The interface's name has been changed from "port2" to "inside".

Array(config)#interface name port3 dmz
The interface's name has been changed from "port3" to "dmz".

```

### interface speed

```

interface speed <interface_ID> <speed_option>

interface_ID 默认的序号为（port1，port2，port3，port4，port5 以及 port6）。
speed_option 速度可以分为 10 兆半双工，100 兆半双工，100 兆全双工，1000 兆全双工或自适应

```

### ip address

```

Array(config)# ip address "port1" 192.168.3.20 255.255.255.0

Array(config)#show ip address
ip address "port1" 192.168.3.20 255.255.255.0

```

### bond configuration

```
#bond configuration
bond interface "bond1" "port1" 1
bond interface "bond1" "port2" 1
ip address "bond1" 172.16.0.9 255.255.255.0

```

### ip nameserver

```

Array(config)#ip nameserver 208.67.222.222

Array(config)#ip nameserver 208.67.220.220

Array(config)# show ip nameserver
name server 208.67.222.222
name server 208.67.220.220

```

## route configuration

### gateway ip

```

Array(config)#ip route default 192.168.3.1
Default Route: 192.168.3.1 has existed in system

Array(config)#show ip route
Destination     Netmask         Gateway
default                         192.168.3.1

RIP routes:
Destination     Netmask         Gateway

OSPF routes:
Destination     Netmask         Gateway

```

### RIP

```

ip route static <destination> <destination_netmask> <gateway>

```

```
rip
rip network 172.16.0.0
rip network 172.16.1.0

```

### ospf

```

ospf on
ospf network <ip_address> <netmask> <area_id>

```

## mnet / vlan

### mnet

```

[no/show/clear] mnet <{inside|outside|dmz|eng|eng1|eng2|bond_ifname}> <user_interface_name>

```

mnet

```

#mnet configuration
mnet outside "mnet0"
ip address "mnet0" 172.16.0.10 255.255.255.0

```

### vlan

```

[no/show/clear] vlan <{inside|outside|dmz|eng|eng1|eng2|bond_ifname}> <user_interface_name> <vlan_tag>

```

## Port Forwarding / NAT / Firewall

### Port Forwarding

```

fwd tcp <local_ip> <local_port> <remote_ip> <remote_port> [timeout]
fwd udp <local_ip> <local_port> <remote_ip> <remote_port> [timeout]

```

### NAT

```

nat port <vip> <network_ip> <netmask> [timeout] [gateway]

```

### Firewall

```

accessgroup <accesslist_id> <interface>

```

```

accesslist permit icmp echoreply <source_ip> <source_mask> <destination_ip> <destination_mask> <accesslist_id>
accesslist permit icmp echorequest <source_ip> <source_mask> <destination_ip> <destination_mask> <accesslist_id>
accesslist permit tcp <source_ip> <source_mask> <source_port> <destination_ip> <destination_mask> <destination_port> <accesslist_id>
accesslist permit udp <source_ip> <source_mask> <source_port> <destination_ip> <destination_mask> <destination_port> <accesslist_id>
accesslist deny icmp echoreply <source_ip> <source_mask> <destination_ip> <destination_mask> <accesslist_id>

accesslist deny icmp echorequest <source_ip> <source_mask> <destination_ip> <destination_mask> <accesslist_id>
accesslist deny tcp <source_ip> <source_mask> <source_port> <destination_ip> <destination_mask> <destination_port> <accesslist_id>
accesslist deny udp <source_ip> <source_mask> <source_port> <destination_ip> <destination_mask> <destination_port> <accesslist_id

```

## slb configuration

### slb real

```

slb real http <real_name> <ip> [port] [max_conn][{http|tcp|icmp|script-tcp|script-udp|sip-tcp|sip-udp}] [hc_up] [hc_down]
slb real tcp <real_name> <ip> <port> [max_conn][{http|tcp|icmp|script-tcp|script-udp|sip-tcp|sip-udp}] [hc_up] [hc_down]
slb real ftp <real_name> <ip> [port] [max_conn] [{tcp|icmp|script-tcp|script-udp|sip-tcp|sip-udp}] [hc_up] [hc_down]
slb real udp <real_name> <ip> <port> [max_conn] [hc_up] [hc_down] [timeout] [{icmp|script-tcp|script-udp|radius-auth|radius-acct}]
slb real https <real_name> <ip> [port] [max_conn] [{https|tcp|tcps|icmp|script-tcp|script-udp|script-tcps|sip-tcp|sip-udp}] [hc_up] [hc_down]
slb real tcps <real_name> <ip> <port> [max_conn] [{tcp|tcps|icmp|script-tcp|script-udp|script-tcps|sip-tcp|sip-udp}] [hc_up] [hc_down]
slb real dns <real_name> <ip> <port> [max_conn] [{dns|icmp|script-tcp|script-udp|sip-tcp|sip-udp}] [hc_up] [hc_down] [timeout]
slb real siptcp <real_name> <ip> [port] [max_conn] [{ http|tcp|icmp|script-tcp|script-udp|sip-tcp|sip-udp}] [hc_up][hc_down]
slb real sipudp <real_name> <ip> [port] [max_conn] [{icmp|script-tcp|script-udp|radius-auth|radius-acct|sip-tcp|sip-udp}] [hc_up][hc_down][timeout]
slb real rtsp <real_name> <ip> [port] [max_conn] [{rtsp-tcp|tcp|icmp|script-tcp|script-udp|none}] [hc_up] [hc_down] [timeout]

```

基于三层（IP）的类型为“IP”的后台服务。该类服务能同时支持 TCP 和 UDP 协议

```

slb real ip <real_name> <IP> [max_conn] [{icmp|none}] [hc_up] [hc_down] [udp timeout]

```

例 33.1. slb real http

```

slb real http rs1 172.16.0.9 80
slb real http rs2 172.16.0.5 80

Array(config)#show slb real http
slb real http "rs1" 172.16.0.9 80 1000 tcp 3 3
slb real http "rs2" 172.16.0.5 80 1000 tcp 3 3

Array(config)#clear  slb real http
slb real http rs1 172.16.0.9 80 4096 http
slb real http rs2 172.16.0.5 80	4096 http

Array(config)#show slb real http
slb real http "rs1" 172.16.0.9 80 4096 http 3 3
slb real http "rs2" 172.16.0.5 80 4096 http 3 3

Array(config)#show health server
----------------------------------- Server Status ---------------------------------
real server name      status
rs1                   UP
rs2                   UP
----------------------------------- Health Check ----------------------------------
real server name      ip              :port    status  hct        rqr rpr checklist
-----------------------------------------------------------------------------------
rs1                   172.16.0.9      :80      UP      http         0   0
rs2                   172.16.0.5      :80      UP      http         0   0

```

### slb virtual

4-7 layer slb

```

slb virtual http <virtual_name> <vip> [vport] [{arp|noarp}] [max_conn]
slb virtual https <virtual_name> <vip>[vport] [{arp|noarp}] [max_conn]
slb virtual tcp <virtual_name> <vip> <vport> [{arp|noarp}] [max_conn]
slb virtual tcps <virtual_name> <vip><vport> [{arp|noarp}] [max_conn]
slb virtual ftp <virtual_name> <vip> [vport] [max_conn]
slb virtual udp <virtual_name> <vip> <vport> [{arp|noarp}] [max_conn]
slb virtual dns <virtual_name> <vip> [vport] [{arp|noarp}] [max_conn]
slb virtual sipudp <virtual_name> <vip>[vport] [{arp|noarp}] [max_conn]
slb virtual siptcp <virtual_name> <vip> [vport] [{arp|noarp}] [max_conn]
slb virtual rtsp <virtual_name> <vip> [vport] [mode] [noarp] [max_conn]

```

3 layer slb

```

slb virtual ip <virtual name> <IP>

这个命令是用来创建基于三层协议的负载均衡操作的虚拟服务。这种虚拟服务可以同时支持 TCP 和 UDP 协议。

```

例 33.2. slb virtual http

```

slb virtual http vs1 172.16.0.3 80

Array(config)#show slb virtual http
slb virtual http "vs1" 172.16.0.3 80 arp 0

```

### slb group method

```

slb group method <group_name> [algorithm]

algorithm 在组内的后台服务中进行负载均衡的算法。可选参数，缺省值为轮循（rr）。基于使用的算法，需要不同的扩展参数。下面标有"*"的算法需要扩展参数。
	rr 轮循
	pc 保持 Cookie*
	pi 保持 IP 地址*
	hi Hash IP 地址*
	hc Hash Cookie*
	ph 保持域名*
	pu 保持 URL*
	ic 插入 Cookie*
	rc 改写 Cookie*
	ec 嵌入 Cookie*
	lc 最少连接数*
	sr 最短响应时间
	hh Hash Header*
	sslsid SSL Session ID*
	chi Consistent Hash IP*
	prox 就近性*
	snmp 简单网络管理协议*
	sipcid SIP CallID*
	sipuid SIP UserID*
	chh Consistent Hash Header*
	hq Hash Query*
	hip Hash (IP+Port) *

```

例 33.3. slb group method

```

Array(config)#slb group method gm1 rr

Array(config)#show slb group method
slb group method "gm1" rr

```

### slb group member

```

slb group member <group_name> <real_name>

```

例 33.4. slb group member

```

Array(config)#slb group member gm1 rs1

Array(config)#slb group member gm1 rs2

Array(config)#show slb group member gm1
slb group member "gm1" "rs1" 1
slb group member "gm1" "rs2" 1

```

### slb policy

```

slb policy default "vs1" "gm1"

```

例 33.5. slb policy default

```
Array(config)#slb policy default "vs1" "gm1"

Array(config)#show slb policy all
slb policy default "vs1" "gm1"

```

### slb group flush

这条命令允许系统管理员清空指定服务组的保持性关系表。这条命令会消除所有已经建立的保持性关系，所有使用这条命令时有提醒信息。已经建立保持连接的用户，会被迫重新建立保持性连接。“group_name”参数，必须是采用 hc、hh、ph 或者 pi 算法的服务组。

```
slb group flush

```

### slb configuration example

例 33.6. slb example

```
slb real http "http-nginx-0" 10.0.0.68 80 100000 http 3 3
slb real http "http-nginx-1" 10.0.0.69 80 100000 http 3 3

slb real http "http-user-1" 10.0.0.24 80 100000 http 3 3
slb real http "http-user-2" 10.0.0.25 80 100000 http 3 3
slb real http "http-user-3" 10.0.0.26 80 100000 http 3 3

slb group method "group-nginx-0" rr
slb group member "group-nginx-0" "http-nginx-0" 1 0
slb group member "group-nginx-0" "http-nginx-1" 1 0
slb virtual http "vs-nginx-http" 172.16.0.60 80 arp 0

slb group method "group-user-0" rr
slb group member "group-user-0" "http-user-1" 1 0
slb group member "group-user-0" "http-user-2" 1 0
slb group member "group-user-0" "http-user-3" 1 0
slb virtual http "vs-user-http" 172.16.0.61 80 arp 0

slb policy default "vs-nginx-http" "group-nginx-0"
slb policy default "vs-user-http" "group-user-0"

```

## Logging

```

log on

```

### log http

```

Array(config)#log http squid novip nohost

```

Apache Log combined Format

```

Array(config)#log http combined novip nohost

```

### show log config

```
Array(config)#show log config
Logging is on
Logging Facility: LOCAL0
Logging source port: 514
Logging level: info
Logging option level information is off
Logging option logid is off
Logging timestamp is on
log http squid

```

### show log buff

```

show log buff backward
show log buff backward

```

```

Array(config)#show log buff forward
start of buffer
INFO    Jan 11 22:41:23 CLI: User "array" executed cmd "log on"
INFO    Jan 11 22:41:30 CLI: User "array" executed cmd "show log config"
INFO    Jan 11 22:43:00 CLI: User "array" executed cmd "show log buff backward"
INFO    Jan 11 22:43:15 CLI: User "array" executed cmd "show log buff backward"
INFO    Jan 11 22:43:27 CLI: User "array" executed cmd "show log buff backward"

```

### log host

```

Array(config)#log facility LOCAL1
Array(config)#log host 172.16.0.9

```

linux syslogd

/etc/syslog.conf

```
local1.*          /var/log/array.log

```

/var/log/array.log

```
# cat /var/log/array.log
Jan 12 00:29:29 192.168.3.20 CLI: User "array" executed cmd "show run"

```

防止日志文件本修改，加入某些属性。

```
1：chattr 命令可以使某个文件属性改变
     chattr  +a /var/log/array.log   只允许追加，不允许减少。
     chattr  -a /var/log/array.log   去掉属性。
2：lsattr 命令查看 chattr 属性
# lsattr /var/log/array.log
-----a------- /var/log/array.log

```

## webui

Array Web UI 是 PHP4 写的，去/ca/webui/htdocs/proxy/里面看吧，好原始没有 MVC 框架呵呵

```

AN# cat /ca/webui/conf/httpd.conf
##
## httpd.conf -- Apache HTTP server configuration file
##

#
# Based upon the NCSA server configuration files originally by Rob McCool.
#
# This is the main Apache server configuration file.  It contains the
# configuration directives that give the server its instructions.
# See <URL:http://www.apache.org/docs/> for detailed information about
# the directives.
#
# Do NOT simply read the instructions in here without understanding
# what they do.  They're here only as hints or reminders.  If you are unsure
# consult the online docs. You have been warned.
#
# After this file is processed, the server will look for and process
# /ca/webui/conf/srm.conf and then /ca/webui/conf/access.conf
# unless you have overridden these with ResourceConfig and/or
# AccessConfig directives here.
#
# The configuration directives are grouped into three basic sections:
#  1\. Directives that control the operation of the Apache server process as a
#     whole (the 'global environment').
#  2\. Directives that define the parameters of the 'main' or 'default' server,
#     which responds to requests that aren't handled by a virtual host.
#     These directives also provide default values for the settings
#     of all virtual hosts.
#  3\. Settings for virtual hosts, which allow Web requests to be sent to
#     different IP addresses or hostnames and have them handled by the
#     same Apache server process.
#
# Configuration and logfile names: If the filenames you specify for many
# of the server's control files begin with "/" (or "drive:/" for Win32), the
# server will use that explicit path.  If the filenames do *not* begin
# with "/", the value of ServerRoot is prepended -- so "logs/foo.log"
# with ServerRoot set to "/usr/local/apache" will be interpreted by the
# server as "/usr/local/apache/logs/foo.log".
#

### Section 1: Global Environment
#
# The directives in this section affect the overall operation of Apache,
# such as the number of concurrent requests it can handle or where it
# can find its configuration files.
#

#
# ServerType is either inetd, or standalone.  Inetd mode is only supported on
# Unix platforms.
#
ServerType standalone

#
# ServerRoot: The top of the directory tree under which the server's
# configuration, error, and log files are kept.
#
# NOTE!  If you intend to place this on an NFS (or otherwise network)
# mounted filesystem then please read the LockFile documentation
# (available at <URL:http://www.apache.org/docs/mod/core.html#lockfile>);
# you will save yourself a lot of trouble.
#
# Do NOT add a slash at the end of the directory path.
#
ServerRoot "/ca/an_apache"

#
# The LockFile directive sets the path to the lockfile used when Apache
# is compiled with either USE_FCNTL_SERIALIZED_ACCEPT or
# USE_FLOCK_SERIALIZED_ACCEPT. This directive should normally be left at
# its default value. The main reason for changing it is if the logs
# directory is NFS mounted, since the lockfile MUST BE STORED ON A LOCAL
# DISK. The PID of the main server process is automatically appended to
# the filename.
#
#LockFile /ca/webui/logs/httpd.lock

#
# PidFile: The file in which the server should record its process
# identification number when it starts.
#
PidFile /ca/webui/logs/httpd.pid

#
# ScoreBoardFile: File used to store internal server process information.
# Not all architectures require this.  But if yours does (you'll know because
# this file will be  created when you run Apache) then you *must* ensure that
# no two invocations of Apache share the same scoreboard file.
#
ScoreBoardFile /ca/webui/logs/httpd.scoreboard

#
# In the standard configuration, the server will process httpd.conf (this
# file, specified by the -f command line option), srm.conf, and access.conf
# in that order.  The latter two files are now distributed empty, as it is
# recommended that all directives be kept in a single file for simplicity.
# The commented-out values below are the built-in defaults.  You can have the
# server ignore these files altogether by using "/dev/null" (for Unix) or
# "nul" (for Win32) for the arguments to the directives.
#
#ResourceConfig conf/srm.conf
#AccessConfig conf/access.conf

#
# Timeout: The number of seconds before receives and sends time out.
#
Timeout 300

#
# KeepAlive: Whether or not to allow persistent connections (more than
# one request per connection). Set to "Off" to deactivate.
#
KeepAlive On

#
# MaxKeepAliveRequests: The maximum number of requests to allow
# during a persistent connection. Set to 0 to allow an unlimited amount.
# We recommend you leave this number high, for maximum performance.
#
MaxKeepAliveRequests 256

#
# KeepAliveTimeout: Number of seconds to wait for the next request from the
# same client on the same connection.
#
KeepAliveTimeout 100

#
# Server-pool size regulation.  Rather than making you guess how many
# server processes you need, Apache dynamically adapts to the load it
# sees --- that is, it tries to maintain enough server processes to
# handle the current load, plus a few spare servers to handle transient
# load spikes (e.g., multiple simultaneous requests from a single
# Netscape browser).
#
# It does this by periodically checking how many servers are waiting
# for a request.  If there are fewer than MinSpareServers, it creates
# a new spare.  If there are more than MaxSpareServers, some of the
# spares die off.  The default values are probably OK for most sites.
#
MinSpareServers 2
MaxSpareServers 5

#
# Number of servers to start initially --- should be a reasonable ballpark
# figure.
#
StartServers 1

#
# Limit on total number of servers running, i.e., limit on the number
# of clients who can simultaneously connect --- if this limit is ever
# reached, clients will be LOCKED OUT, so it should NOT BE SET TOO LOW.
# It is intended mainly as a brake to keep a runaway server from taking
# the system with it as it spirals down...
#
MaxClients 100

#
# MaxRequestsPerChild: the number of requests each child process is
# allowed to process before the child dies.  The child will exit so
# as to avoid problems after prolonged use when Apache (and maybe the
# libraries it uses) leak memory or other resources.  On most systems, this
# isn't really needed, but a few (such as Solaris) do have notable leaks
# in the libraries. For these platforms, set to something like 10000
# or so; a setting of 0 means unlimited.
#
# NOTE: This value does not include keepalive requests after the initial
#       request per connection. For example, if a child process handles
#       an initial request and 10 subsequent "keptalive" requests, it
#       would only count as 1 request towards this limit.
#
MaxRequestsPerChild 10000

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, in addition to the default. See also the <VirtualHost>
# directive.
#
#Listen 3000
#Listen 12.34.56.78:80

#
# BindAddress: You can support virtual hosts with this option. This directive
# is used to tell the server which IP address to listen to. It can either
# contain "*", an IP address, or a fully qualified Internet domain name.
# See also the <VirtualHost> and Listen directives.
#
#BindAddress *

#
# Dynamic Shared Object (DSO) Support
#
# To be able to use the functionality of a module which was built as a DSO you
# have to place corresponding `LoadModule' lines at this location so the
# directives contained in it are actually available _before_ they are used.
# Please read the file README.DSO in the Apache 1.3 distribution for more
# details about the DSO mechanism and run `httpd -l' for the list of already
# built-in (statically linked and thus always available) modules in your httpd
# binary.
#
# Note: The order in which modules are loaded is important.  Don't change
# the order below without expert advice.
#
# Example:
# LoadModule foo_module libexec/mod_foo.so

#
# ExtendedStatus controls whether Apache will generate "full" status
# information (ExtendedStatus On) or just basic information (ExtendedStatus
# Off) when the "server-status" handler is called. The default is Off.
#
#ExtendedStatus On

### Section 2: 'Main' server configuration
#
# The directives in this section set up the values used by the 'main'
# server, which responds to any requests that aren't handled by a
# <VirtualHost> definition.  These values also provide defaults for
# any <VirtualHost> containers you may define later in the file.
#
# All of these directives may appear inside <VirtualHost> containers,
# in which case these default settings will be overridden for the
# virtual host being defined.
#

#
# If your ServerType directive (set earlier in the 'Global Environment'
# section) is set to "inetd", the next few directives don't have any
# effect since their settings are defined by the inetd configuration.
# Skip ahead to the ServerAdmin directive.
#

#
# Port: The port to which the standalone server listens. For
# ports < 1023, you will need httpd to be run as root initially.
#
##
##  SSL Support
##
##  When we also provide SSL we have to listen to the
##  standard HTTP port (see above) and to the HTTPS port
##
Include /ca/conf/webui.conf

#
# If you wish httpd to run as a different user or group, you must run
# httpd as root initially and it will switch.
#
# User/Group: The name (or #number) of the user/group to run httpd as.
#  . On SCO (ODT 3) use "User nouser" and "Group nogroup".
#  . On HPUX you may not be able to use shared memory as nobody, and the
#    suggested workaround is to create a user www and use that user.
#  NOTE that some kernels refuse to setgid(Group) or semctl(IPC_SET)
#  when the value of (unsigned)Group is above 60000;
#  don't use Group nobody on these systems!
#
User nobody
Group nobody

#
# ServerAdmin: Your address, where problems with the server should be
# e-mailed.  This address appears on some server-generated pages, such
# as error documents.
#
ServerAdmin support@arraynetworks.net

#
# ServerName allows you to set a host name which is sent back to clients for
# your server if it's different than the one the program would get (i.e., use
# "www" instead of the host's real name).
#
# Note: You cannot just invent host names and hope they work. The name you
# define here must be a valid DNS name for your host. If you don't understand
# this, ask your network administrator.
# If your host doesn't have a registered DNS name, enter its IP address here.
# You will have to access it by its address (e.g., http://123.45.67.89/)
# anyway, and this will make redirections work in a sensible way.
#
# 127.0.0.1 is the TCP/IP local loop-back address, often named localhost. Your
# machine always knows itself by this address. If you use Apache strictly for
# local testing and development, you may use 127.0.0.1 as the server name.
#
ServerName 127.0.0.1

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
# in /ca/conf/webui.conf

#
# Each directory to which Apache has access, can be configured with respect
# to which services and features are allowed and/or disabled in that
# directory (and its subdirectories).
#
# First, we configure the "default" to be a very restrictive set of
# permissions.
#
<Directory />
    Options FollowSymLinks
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>

#
# Note that from this point forward you must specifically allow
# particular features to be enabled - so if something's not working as
# you might expect, make sure that you have specifically enabled it
# below.
#

#
# This should be changed to whatever you set DocumentRoot to.
#

#
# UserDir: The name of the directory which is appended onto a user's home
# directory if a ~user request is received.
#
<IfModule mod_userdir.c>
    UserDir public_html
</IfModule>

#
# Control access to UserDir directories.  The following is an example
# for a site where these directories are restricted to read-only.
#
<Directory /home/*/public_html>
    Options None
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>

#
# DirectoryIndex: Name of the file or files to use as a pre-written HTML
# directory index.  Separate multiple entries with spaces.
#
<IfModule mod_dir.c>
    DirectoryIndex index.html
</IfModule>

#
# AccessFileName: The name of the file to look for in each directory
# for access control information.
#
AccessFileName .htaccess

#
# The following lines prevent .htaccess files from being viewed by
# Web clients.  Since .htaccess files often contain authorization
# information, access is disallowed for security reasons.  Comment
# these lines out if you want Web visitors to see the contents of
# .htaccess files.  If you change the AccessFileName directive above,
# be sure to make the corresponding changes here.
#
# Also, folks tend to use names such as .htpasswd for password
# files, so this will protect those as well.
#

#
# CacheNegotiatedDocs: By default, Apache sends "Pragma: no-cache" with each
# document that was negotiated on the basis of content. This asks proxy
# servers not to cache the document. Uncommenting the following line disables
# this behavior, and proxies will be allowed to cache the documents.
#
#CacheNegotiatedDocs

#
# UseCanonicalName:  (new for 1.3)  With this setting turned on, whenever
# Apache needs to construct a self-referencing URL (a URL that refers back
# to the server the response is coming from) it will use ServerName and
# Port to form a "canonical" name.  With this setting off, Apache will
# use the hostname:port that the client supplied, when possible.  This
# also affects SERVER_NAME and SERVER_PORT in CGI scripts.
#
UseCanonicalName Off

#
# TypesConfig describes where the mime.types file (or equivalent) is
# to be found.
#
<IfModule mod_mime.c>
    TypesConfig /ca/an_apache/conf/mime.types
</IfModule>

#
# DefaultType is the default MIME type the server will use for a document
# if it cannot otherwise determine one, such as from filename extensions.
# If your server contains mostly text or HTML documents, "text/plain" is
# a good value.  If most of your content is binary, such as applications
# or images, you may want to use "application/octet-stream" instead to
# keep browsers from trying to display binary files as though they are
# text.
#
DefaultType text/plain

#
# The mod_mime_magic module allows the server to use various hints from the
# contents of the file itself to determine its type.  The MIMEMagicFile
# directive tells the module where the hint definitions are located.
# mod_mime_magic is not part of the default server (you have to add
# it yourself with a LoadModule [see the DSO paragraph in the 'Global
# Environment' section], or recompile the server and include mod_mime_magic
# as part of the configuration), so it's enclosed in an <IfModule> container.
# This means that the MIMEMagicFile directive will only be processed if the
# module is part of the server.
#
<IfModule mod_mime_magic.c>
    MIMEMagicFile /ca/an_apache/conf/magic
</IfModule>

#
# HostnameLookups: Log the names of clients or just their IP addresses
# e.g., www.apache.org (on) or 204.62.129.132 (off).
# The default is off because it'd be overall better for the net if people
# had to knowingly turn this feature on, since enabling it means that
# each client request will result in AT LEAST one lookup request to the
# nameserver.
#
HostnameLookups Off

#
# ErrorLog: The location of the error log file.
# If you do not specify an ErrorLog directive within a <VirtualHost>
# container, error messages relating to that virtual host will be
# logged here.  If you *do* define an error logfile for a <VirtualHost>
# container, that host's errors will be logged there and not here.
#
#ErrorLog /ca/webui/logs/error_log
ErrorLog syslog:user

#
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel error

#
# The following directives define some format nicknames for use with
# a CustomLog directive (see below).
#
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %b" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent

#
# The location and format of the access logfile (Common Logfile Format).
# If you do not define any access logfiles within a <VirtualHost>
# container, they will be logged here.  Contrariwise, if you *do*
# define per-<VirtualHost> access logfiles, transactions will be
# logged therein and *not* in this file.
#
#CustomLog /ca/webui/logs/access_log common

#
# If you would like to have agent and referer logfiles, uncomment the
# following directives.
#
#CustomLog /ca/webui/logs/referer_log referer
#CustomLog /ca/webui/logs/agent_log agent

#
# If you prefer a single logfile with access, agent, and referer information
# (Combined Logfile Format) you can use the following directive.
#
#CustomLog /ca/webui/logs/access_log combined

#
# Optionally add a line containing the server version and virtual host
# name to server-generated pages (error documents, FTP directory listings,
# mod_status and mod_info output etc., but not CGI generated documents).
# Set to "EMail" to also include a mailto: link to the ServerAdmin.
# Set to one of:  On | Off | EMail
#
ServerSignature Off

#
# Aliases: Add here as many aliases as you need (with no limit). The format is
# Alias fakename realname
#
<IfModule mod_alias.c>

    #
    # Note that if you include a trailing / on fakename then the server will
    # require it to be present in the URL.  So "/icons" isn't aliased in this
    # example, only "/icons/"..
    #

    <Directory "/ca/webui/icons">
        Options Indexes MultiViews
        AllowOverride None
        Order allow,deny
        Allow from all
    </Directory>

    #
    # ScriptAlias: This controls which directories contain server scripts.
    # ScriptAliases are essentially the same as Aliases, except that
    # documents in the realname directory are treated as applications and
    # run by the server when requested rather than as documents sent to the client.
    # The same rules about trailing "/" apply to ScriptAlias directives as to
    # Alias.
    #
    ScriptAlias /cgi-bin/ "/ca/webui/cgi-bin/"

    #"/ca/webui/cgi-bin" should be changed to whatever your ScriptAliased
    #CGI directory exists, if you have that configured.

    <Directory "/ca/webui/cgi-bin">
        Options None
        AllowOverride AuthConfig
        Order allow,deny
        Allow from all
    </Directory>

</IfModule>
# End of aliases.

#
# Redirect allows you to tell clients about documents which used to exist in
# your server's namespace, but do not anymore. This allows you to tell the
# clients where to look for the relocated document.
# Format: Redirect old-URI new-URL
#

#
# Directives controlling the display of server-generated directory listings.
#
<IfModule mod_autoindex.c>

    #
    # FancyIndexing is whether you want fancy directory indexing or standard
    #
    IndexOptions FancyIndexing

    #
    # AddIcon* directives tell the server which icon to show for different
    # files or filename extensions.  These are only displayed for
    # FancyIndexed directories.
    #
    AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip

    AddIconByType (TXT,/icons/text.gif) text/*
    AddIconByType (IMG,/icons/image2.gif) image/*
    AddIconByType (SND,/icons/sound2.gif) audio/*
    AddIconByType (VID,/icons/movie.gif) video/*

    AddIcon /icons/binary.gif .bin .exe
    AddIcon /icons/binhex.gif .hqx
    AddIcon /icons/tar.gif .tar
    AddIcon /icons/world2.gif .wrl .wrl.gz .vrml .vrm .iv
    AddIcon /icons/compressed.gif .Z .z .tgz .gz .zip
    AddIcon /icons/a.gif .ps .ai .eps
    AddIcon /icons/layout.gif .html .shtml .htm .pdf
    AddIcon /icons/text.gif .txt
    AddIcon /icons/c.gif .c
    AddIcon /icons/p.gif .pl .py
    AddIcon /icons/f.gif .for
    AddIcon /icons/dvi.gif .dvi
    AddIcon /icons/uuencoded.gif .uu
    AddIcon /icons/script.gif .conf .sh .shar .csh .ksh .tcl
    AddIcon /icons/tex.gif .tex
    AddIcon /icons/bomb.gif core

    AddIcon /icons/back.gif ..
    AddIcon /icons/hand.right.gif README
    AddIcon /icons/folder.gif ^^DIRECTORY^^
    AddIcon /icons/blank.gif ^^BLANKICON^^

    #
    # DefaultIcon is which icon to show for files which do not have an icon
    # explicitly set.
    #
    DefaultIcon /icons/unknown.gif

    #
    # AddDescription allows you to place a short description after a file in
    # server-generated indexes.  These are only displayed for FancyIndexed
    # directories.
    # Format: AddDescription "description" filename
    #
    #AddDescription "GZIP compressed document" .gz
    #AddDescription "tar archive" .tar
    #AddDescription "GZIP compressed tar archive" .tgz

    #
    # ReadmeName is the name of the README file the server will look for by
    # default, and append to directory listings.
    #
    # HeaderName is the name of a file which should be prepended to
    # directory indexes.
    #
    # If MultiViews are amongst the Options in effect, the server will
    # first look for name.html and include it if found.  If name.html
    # doesn't exist, the server will then look for name.txt and include
    # it as plaintext if found.
    #
    ReadmeName README
    HeaderName HEADER

    #
    # IndexIgnore is a set of filenames which directory indexing should ignore
    # and not include in the listing.  Shell-style wildcarding is permitted.
    #
    IndexIgnore .??* *~ *# HEADER* README* RCS CVS *,v *,t

</IfModule>
# End of indexing directives.

#
# Document types.
#
<IfModule mod_mime.c>

    #
    # AddEncoding allows you to have certain browsers (Mosaic/X 2.1+) uncompress
    # information on the fly. Note: Not all browsers support this.
    # Despite the name similarity, the following Add* directives have nothing
    # to do with the FancyIndexing customization directives above.
    #
    AddEncoding x-compress Z
    AddEncoding x-gzip gz tgz

    #
    # AddLanguage allows you to specify the language of a document. You can
    # then use content negotiation to give a browser a file in a language
    # it can understand.
    #
    # Note 1: The suffix does not have to be the same as the language
    # keyword --- those with documents in Polish (whose net-standard
    # language code is pl) may wish to use "AddLanguage pl .po" to
    # avoid the ambiguity with the common suffix for perl scripts.
    #
    # Note 2: The example entries below illustrate that in quite
    # some cases the two character 'Language' abbriviation is not
    # identical to the two character 'Country' code for its country,
    # E.g. 'Danmark/dk' versus 'Danish/da'.
    #
    # Note 3: In the case of 'ltz' we violate the RFC by using a three char
    # specifier. But there is 'work in progress' to fix this and get
    # the reference data for rfc1766 cleaned up.
    #
    # Danish (da) - Dutch (nl) - English (en) - Estonian (ee)
    # French (fr) - German (de) - Greek-Modern (el)
    # Italian (it) - Korean (kr) - Norwegian (no)
    # Portugese (pt) - Luxembourgeois* (ltz)
    # Spanish (es) - Swedish (sv) - Catalan (ca) - Czech(cz)
    # Polish (pl) - Brazilian Portuguese (pt-br) - Japanese (ja)
    # Russian (ru)
    #
    AddLanguage da .dk
    AddLanguage nl .nl
    AddLanguage en .en
    AddLanguage et .ee
    AddLanguage fr .fr
    AddLanguage de .de
    AddLanguage el .el
    AddLanguage he .he
    AddCharset ISO-8859-8 .iso8859-8
    AddLanguage it .it
    AddLanguage ja .ja
    AddCharset ISO-2022-JP .jis
    AddLanguage kr .kr
    AddCharset ISO-2022-KR .iso-kr
    AddLanguage no .no
    AddLanguage pl .po
    AddCharset ISO-8859-2 .iso-pl
    AddLanguage pt .pt
    AddLanguage pt-br .pt-br
    AddLanguage ltz .lu
    AddLanguage ca .ca
    AddLanguage es .es
    AddLanguage sv .se
    AddLanguage cz .cz
    AddLanguage ru .ru
    AddLanguage tw .tw
    AddCharset Big5         .Big5    .big5
    AddCharset WINDOWS-1251 .cp-1251
    AddCharset CP866        .cp866
    AddCharset ISO-8859-5   .iso-ru
    AddCharset KOI8-R       .koi8-r
    AddCharset UCS-2        .ucs2
    AddCharset UCS-4        .ucs4
    AddCharset UTF-8        .utf8

    # LanguagePriority allows you to give precedence to some languages
    # in case of a tie during content negotiation.
    #
    # Just list the languages in decreasing order of preference. We have
    # more or less alphabetized them here. You probably want to change this.
    #
    <IfModule mod_negotiation.c>
        LanguagePriority en da nl et fr de el it ja kr no pl pt pt-br ru ltz ca es sv tw
    </IfModule>

    #
    # AddType allows you to tweak mime.types without actually editing it, or to
    # make certain files to be certain types.
    #
    # For example, the PHP 3.x module (not part of the Apache distribution - see
    # http://www.php.net) will typically use:
    #
    #AddType application/x-httpd-php3 .php3
    #AddType application/x-httpd-php3-source .phps
    #
    # And for PHP 4.x, use:
    #
    XBitHack on
    AddType application/x-httpd-php .html
    AddType application/x-httpd-php-source .phps

    AddType application/x-tar .tgz

    #
    # AddHandler allows you to map certain file extensions to "handlers",
    # actions unrelated to filetype. These can be either built into the server
    # or added with the Action command (see below)
    #
    # If you want to use server side includes, or CGI outside
    # ScriptAliased directories, uncomment the following lines.
    #
    # To use CGI scripts:
    #
    #AddHandler cgi-script .cgi

    #
    # To use server-parsed HTML files
    #
    #AddType text/html .shtml
    #AddHandler server-parsed .shtml

    #
    # Uncomment the following line to enable Apache's send-asis HTTP file
    # feature
    #
    #AddHandler send-as-is asis

    #
    # If you wish to use server-parsed imagemap files, use
    #
    #AddHandler imap-file map

    #
    # To enable type maps, you might want to use
    #
    #AddHandler type-map var

</IfModule>
# End of document types.

#
# Action lets you define media types that will execute a script whenever
# a matching file is called. This eliminates the need for repeated URL
# pathnames for oft-used CGI file processors.
# Format: Action media/type /cgi-script/location
# Format: Action handler-name /cgi-script/location
#

#
# MetaDir: specifies the name of the directory in which Apache can find
# meta information files. These files contain additional HTTP headers
# to include when sending the document
#
#MetaDir .web

#
# MetaSuffix: specifies the file name suffix for the file containing the
# meta information.
#
#MetaSuffix .meta

#
# Customizable error response (Apache style)
#  these come in three flavors
#
ErrorDocument 406 "Error 406: Not Acceptable
ErrorDocument 405 "Error 405: Method Not Allowed
ErrorDocument 404 "The file you have requested does not exist.
ErrorDocument 403 "Error 403: Forbidden
ErrorDocument 401 "Error 401: Unauthorized
ErrorDocument 400 "This server uses SSL for security. Please use HTTPS to connect.

#
# Customize behaviour based on the browser
#
<IfModule mod_setenvif.c>

    #
    # The following directives modify normal HTTP response behavior.
    # The first directive disables keepalive for Netscape 2.x and browsers that
    # spoof it. There are known problems with these browser implementations.
    # The second directive is for Microsoft Internet Explorer 4.0b2
    # which has a broken HTTP/1.1 implementation and does not properly
    # support keepalive when it is used on 301 or 302 (redirect) responses.
    #
    BrowserMatch "Mozilla/2" nokeepalive
    BrowserMatch "MSIE 4\.0b2;" nokeepalive downgrade-1.0 force-response-1.0

    #
    # The following directive disables HTTP/1.1 responses to browsers which
    # are in violation of the HTTP/1.0 spec by not being able to grok a
    # basic 1.1 response.
    #
    BrowserMatch "RealPlayer 4\.0" force-response-1.0
    BrowserMatch "Java/1\.0" force-response-1.0
    BrowserMatch "JDK/1\.0" force-response-1.0

</IfModule>
# End of browser customization directives

#
# Allow server status reports, with the URL of http://servername/server-status
# Change the ".your_domain.com" to match your domain to enable.
#
#<Location /server-status>
#    SetHandler server-status
#    Order deny,allow
#    Deny from all
#    Allow from .your_domain.com
#</Location>

#
# Allow remote server configuration reports, with the URL of
# http://servername/server-info (requires that mod_info.c be loaded).
# Change the ".your_domain.com" to match your domain to enable.
#
#<Location /server-info>
#    SetHandler server-info
#    Order deny,allow
#    Deny from all
#    Allow from .your_domain.com
#</Location>

#
# There have been reports of people trying to abuse an old bug from pre-1.1
# days.  This bug involved a CGI script distributed as a part of Apache.
# By uncommenting these lines you can redirect these attacks to a logging
# script on phf.apache.org.  Or, you can record them yourself, using the script
# support/phf_abuse_log.cgi.
#
#<Location /cgi-bin/phf*>
#    Deny from all
#    ErrorDocument 403 http://phf.apache.org/phf_abuse_log.cgi
#</Location>

#
# Proxy Server directives. Uncomment the following lines to
# enable the proxy server:
#
#<IfModule mod_proxy.c>
#    ProxyRequests On

#    <Directory proxy:*>
#        Order deny,allow
#        Deny from all
#        Allow from .your_domain.com
#    </Directory>

    #
    # Enable/disable the handling of HTTP/1.1 "Via:" headers.
    # ("Full" adds the server version; "Block" removes all outgoing Via: headers)
    # Set to one of: Off | On | Full | Block
    #
#    ProxyVia On

    #
    # To enable the cache as well, edit and uncomment the following lines:
    # (no cacheing without CacheRoot)
    #
#    CacheRoot "/ca/webui/proxy"
#    CacheSize 5
#    CacheGcInterval 4
#    CacheMaxExpire 24
#    CacheLastModifiedFactor 0.1
#    CacheDefaultExpire 1
#    NoCache a_domain.com another_domain.edu joes.garage_sale.com

#</IfModule>
# End of proxy directives.

### Section 3: Virtual Hosts
#
# VirtualHost: If you want to maintain multiple domains/hostnames on your
# machine you can setup VirtualHost containers for them. Most configurations
# use only name-based virtual hosts so the server doesn't need to worry about
# IP addresses. This is indicated by the asterisks in the directives below.
#
# Please see the documentation at <URL:http://www.apache.org/docs/vhosts/>
# for further details before you try to setup virtual hosts.
#
# You may use the command line option '-S' to verify your virtual host
# configuration.

#
# Use name-based virtual hosting.
#
#NameVirtualHost *

#
# VirtualHost example:
# Almost any Apache directive may go into a VirtualHost container.
# The first VirtualHost section is used for requests without a known
# server name.
#
#<VirtualHost *>
#    ServerAdmin webmaster@dummy-host.example.com
#    DocumentRoot /www/docs/dummy-host.example.com
#    ServerName dummy-host.example.com
#    ErrorLog logs/dummy-host.example.com-error_log
#    CustomLog logs/dummy-host.example.com-access_log common
#</VirtualHost>

#<VirtualHost _default_:*>
#</VirtualHost>

##
##  SSL Global Context
##
##  All SSL configuration in this context applies both to
##  the main server and all SSL-enabled virtual hosts.
##

#
#   Some MIME-types for downloading Certificates and CRLs
#
<IfDefine SSL>
AddType application/x-x509-ca-cert .crt
AddType application/x-pkcs7-crl    .crl
</IfDefine>

<IfModule mod_ssl.c>

#   Pass Phrase Dialog:
#   Configure the pass phrase gathering process.
#   The filtering dialog program (`builtin' is a internal
#   terminal dialog) has to provide the pass phrase on stdout.
SSLPassPhraseDialog  builtin

#   Inter-Process Session Cache:
#   Configure the SSL Session Cache: First either `none'
#   or `dbm:/path/to/file' for the mechanism to use and
#   second the expiring timeout (in seconds).
#SSLSessionCache        none
#SSLSessionCache        shm:/ca/webui/logs/ssl_scache(512000)
SSLSessionCache         dbm:/ca/webui/logs/ssl_scache
SSLSessionCacheTimeout  300

#   Semaphore:
#   Configure the path to the mutual explusion semaphore the
#   SSL engine uses internally for inter-process synchronization.
SSLMutex  file:/ca/webui/logs/ssl_mutex

#   Pseudo Random Number Generator (PRNG):
#   Configure one or more sources to seed the PRNG of the
#   SSL library. The seed data should be of good random quality.
#   WARNING! On some platforms /dev/random blocks if not enough entropy
#   is available. This means you then cannot use the /dev/random device
#   because it would lead to very long connection times (as long as
#   it requires to make more entropy available). But usually those
#   platforms additionally provide a /dev/urandom device which doesn't
#   block. So, if available, use this one instead. Read the mod_ssl User
#   Manual for more details.
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
#SSLRandomSeed startup file:/dev/random  512
#SSLRandomSeed startup file:/dev/urandom 512
#SSLRandomSeed connect file:/dev/random  512
#SSLRandomSeed connect file:/dev/urandom 512

#   Logging:
#   The home of the dedicated SSL protocol logfile. Errors are
#   additionally duplicated in the general error log file.  Put
#   this somewhere where it cannot be used for symlink attacks on
#   a real server (i.e. somewhere where only root can write).
#   Log levels are (ascending order: higher ones include lower ones):
#   none, error, warn, info, trace, debug.
#SSLLog      /ca/webui/logs/ssl_engine_log
SSLLogLevel none

</IfModule>

# Shockwave Flash
AddType application/x-shockwave-flash swf
AddType application/futuresplash spl

# Support new php register_globals as OFF until webui code and be changed
# (probably never)
<IfModule mod_php4.c>
    php_flag register_globals on
        # Bug 17223, lintq, 20070807
        # Since the build size of TMX has bigger than the original one,
        # I increase this size to entitle user to upgrade build from WebUI,
        # using mode of "Local Host File".
    php_admin_value post_max_size 80000000
    php_admin_value upload_max_filesize 80000000
        # Bug 17223, end
    php_admin_value upload_tmp_dir /tmp
</IfModule>

```

```

AN# cat /ca/conf/webui.conf
# Configuration File for WebUI

<Files ~ "^\.ht">
    Order allow,deny
    Deny from all
</Files>

<Files ~ "^\.inc">
    Order allow,deny
    Deny from all
</Files>

<IfDefine ARRAYOS>
<IfDefine SSL>
Listen 8888

Alias /monitor/graphs /var/run/graphs
<Directory /var/run/graphs>
    Options None
    Allow from all
</Directory>

# For ArrayOS, allow all ciphersuites
<VirtualHost _default_:8888>
ErrorLog syslog:user
# Bug 15193, lintq, 20070105
# This log isn't necessary for user, to avoid filling up
# file system, disable it.
# TransferLog none
# Bug 15193, end
SSLEngine on
# disable SSLv2 and low strength encryption to cover security holes
SSLProtocol all -SSLv2
#SSLCipherSuite ALL:!ADH:!EXP56:RC4+RSA:+HIGH:+MEDIUM:-LOW:-SSLv2:+EXP:+eNULL
SSLCipherSuite  kRSA:kDHr:kDHd:kEDH:-EXP
SSLCertificateFile /ca/an_apache/conf/ssl.crt/server.crt
SSLCertificateKeyFile /ca/an_apache/conf/ssl.key/server.key
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/ca/webui/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

#   Followings are not needed any more for new version of IE.
#   SSL Protocol Adjustments:
#   For MSIE's broken keep-alive and HTTP/1.1 implementations.
#SetEnvIf User-Agent ".*MSIE.*" \
#         nokeepalive ssl-unclean-shutdown \
#         downgrade-1.0 force-response-1.0

#   Per-Server Logging:
#CustomLog /ca/webui/logs/ssl_request_log \
#          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
SSLLogLevel none

</VirtualHost>

</IfDefine>

DocumentRoot "/ca/webui/htdocs/proxy/new"
<Directory "/ca/webui/htdocs/proxy/new">
    Options FollowSymLinks
    AllowOverride AuthConfig Options
    Order allow,deny
    Allow from all
    php_admin_value upload_tmp_dir /tmp
    php_value upload_max_filesize 200M
    php_value post_max_size 200M
</Directory>
</IfDefine>

# For SProxy, only allow high grade encryption
<IfDefine SPROXY>
<IfDefine SSL>
Listen 8888

<VirtualHost _default_:8888>
ErrorLog syslog:user
TransferLog none
SSLEngine on
# disable SSLv2 and low strength encryption to cover security holes
SSLProtocol all -SSLv2
SSLCipherSuite RC4-SHA:RC4-MD5:DES-CBC3-SHA:DES-CBC3-MD5
SSLCertificateFile /ca/an_apache/conf/ssl.crt/server.crt
SSLCertificateKeyFile /ca/an_apache/conf/ssl.key/server.key
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/ca/webui/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

#   SSL Protocol Adjustments:
#   Followings are not needed any more for new version of IE.
#   SSL Protocol Adjustments:
#   For MSIE's broken keep-alive and HTTP/1.1 implementations.
#SetEnvIf User-Agent ".*MSIE.*" \
#         nokeepalive ssl-unclean-shutdown \
#         downgrade-1.0 force-response-1.0

#   Per-Server Logging:
#CustomLog /ca/webui/logs/ssl_request_log \
#          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
SSLLogLevel none

</VirtualHost>

</IfDefine>

DocumentRoot "/ca/webui/htdocs/sproxy"
<Directory "/ca/webui/htdocs/sproxy">
    Options FollowSymLinks
    AllowOverride AuthConfig
    Order allow,deny
    Allow from all
        SSLRequireSSL
</Directory>
</IfDefine>

```

## example

```

#command timeout configuration
system command timeout 0

#admin aaa configuration
admin aaa off
admin aaa method RADIUS

#hostname configuration
hostname "AN"
config timeout 180

#switch configuration
switch weblink "http://10.90.90.90:80"

#system timezone configuration
system timezone "GMT"

#rts configuration
ip rts off
ip rts expire 600

#interface mtu
interface mtu port1 1500
interface mtu port2 1500
interface mtu port3 1500
interface mtu port4 1500
interface mtu port5 1500
interface mtu port6 1500

#interface configuration
interface name "port1" "port1"
interface name "port2" "port2"
interface name "port3" "port3"
interface name "port4" "port4"
interface name "port5" "port5"
interface name "port6" "port6"
interface speed "port1" auto
interface speed "port2" auto
interface speed "port3" auto
interface speed "port4" auto
interface speed "port5" auto
interface speed "port6" auto

#bond configuration
bond interface "bond1" "port1" 1
bond interface "bond1" "port2" 1
ip address "bond1" 172.16.0.9 255.255.255.0

#ip configuration

#ip host configuration

#support ip configuration

#ssh configuration
ssh on

#vlan configuration

#mnet configuration

#extended routing policies
ip ipflow off
ip ipflow priority 1000
ip ipflow expire 60

#route configuration
ip route default 172.16.0.254

#ip link configuration

#port redundancy configuration

#ip statistic configuration
ip statistic off

#link load balancing configuration

#link load balancing DNS configuration

#smart DNS configuration
sdns off
sdns interval heartbeat 2
sdns interval report 30
sdns dps interval send 120
sdns dps interval query 1200
sdns dps history 9000
sdns dps expire 1
sdns dps method rtt
sdns dps off
sdns dps master off
#NoCheck IP Address
sdns snmp interval 300
sdns snmp version "v2c"
sdns statistics off all
sdns statistics off localdns
sdns persistent timeout 3600
sdns recursion off

#access list and webwall configuration

#synconfig configuration

#synconfig sdns configuration

#cluster configuration
cluster virtual ffo off
cluster virtual ffo interface carrier loss timeout 1000
cluster virtual arp interval 60
cluster virtual discreet off

#nameserver configuration

#enable password
passwd enable "XXXXXXXXXsaFLGt/QKS6yw"

#user configuration
user "array"       "XXXXXXXXX9WfFLfH5KTFBg"

#webui configuration
webui port 8888
webui on

#webui language configuration
webui language en

#xmlrpc configuration
xmlrpc off
xmlrpc port 9999

#snmp configuration
snmp community "public"
snmp contact ""
snmp location ""
no snmp enable traps
snmp ipcontrol off
snmp off

#fastlog configuration
log facility "local0"
log level "info"
log off
log source port 514
log option levelinfo off
log option logid off
log http squid

#ntp configuration

#system mail configuration
system mail from "%h<alert@log.domain>"
system mail hostname "%l.alert_pseudo_domain"

#system mail relay configuration
system mail relay off

#tune configuration
system tune hwcksum on
system tune tcpidle 300
system tune tcp retransmit timeout 1000
system tune tcp retransmit dupacks 3
system tune tcp delack count 4
system tune tcp delack timeout 1000
system tune tcp slowstart on
system tune tcp retransmit policy newreno
system tune tcp syntimeout 60
system tune tcp zwdefend off
system tune tcp pktdropopt 2
system tune udp pktdropopt 0
system tune defraglimit 0
system tune ip randomid off

#dump flag when panic
system dump on

#system interactive configuration
system interactive off

#port forwarding configuration
fwd mode transparent

#sytem fast forwarding configuration
slb directfwd off
slb directfwd syncache off

#slb configuration
slb mode ircookie plainname
slb mode icookie always
slb real http "http-nginx-0" 10.0.0.68 80 100000 http 3 3
slb real http "http-nginx-1" 10.0.0.69 80 100000 http 3 3
slb real http "http-user-1" 10.0.0.24 80 100000 http 3 3
slb real http "http-user-2" 10.0.0.25 80 100000 http 3 3
slb real http "http-user-3" 10.0.0.26 80 100000 http 3 3
slb real http "http0" 172.16.0.5 80 100000 http 3 3
slb real http "http1" 172.16.0.6 80 100000 http 3 3
slb group method "gn-nginx-0" rr
slb group method "gn-user-0" rr
slb group method "gn0" rr
slb group member "gn-nginx-0" "http-nginx-0" 1 0
slb group member "gn-nginx-0" "http-nginx-1" 1 0
slb group member "gn-user-0" "http-user-1" 1 0
slb group member "gn-user-0" "http-user-2" 1 0
slb group member "gn-user-0" "http-user-3" 1 0
slb group member "gn0" "http0" 1 0
slb group member "gn0" "http1" 1 0
slb virtual http "test-http" 172.16.0.10 80 arp 0
slb virtual http "vs-nginx-http" 172.16.0.63 80 arp 0
slb virtual http "vs-user-http" 172.16.0.61 80 arp 0
slb virtual health off
#default policy order:
#                 qos-clientport 1
#                 qos-network 2
#                 pu 3
#                 rc 4
#                 ic 5
#                 pc 6
#                 qos-cookie 7
#                 qos-hostname 8
#                 qos-url 9
#                 regex 10
#                 header 11
#                 hu 12
slb policy default "test-http" "gn0"
slb policy default "vs-nginx-http" "gn-nginx-0"
slb policy default "vs-user-http" "gn-user-0"

#proxy cache configuration
cache off
cache on test-http
cache on vs-nginx-http
cache on vs-user-http
cache settings expire "82800"
cache settings objectsize 5120

#cache channel statistic configuration

#http modifyheader configuration
http modifyheader http10 off

#filter configuration
filter vip "global"
filter mode active "global"
filter length url 1024 "global"
filter length queryvariable 128 "global"
filter length querydata 512 "global"
filter length query 1024 "global"
filter length request 10000 "global"
filter length header 1024 "global"
filter url keyword default permit "global"
#filter request controlchar configuration
#It works only when filter vip setting is configured.
filter request controlchar on

#http error page configuration

#http compression configuration
http compression on
http compression on "test-http"
http compression on "vs-nginx-http"
http compression on "vs-user-http"

#http configuration
http xforwardedfor off
http xforwardedfor on "test-http" header "X-Forwarded-For"
http xforwardedfor on "vs-nginx-http" header "X-Forwarded-For"
http xforwardedfor on "vs-user-http" header "X-Forwarded-For"
http owa off
http xclientcert header "X-Client-Cert: "
http serverpersist on
http serverconnreuse on
http buffer nomsglen on
http mask server off
http mask via off
http rewrite response cookie secure on
http rewrite response cookie secure icookie on
http shuntreset off
system mode reverse

#SIP configuration

#sip multiregistration configuration
sip multireg off

#nat configuration

#health check configuration
health interval 5 5
health server "http0" 0 0
health server "http1" 0 0
health on
health failover disable
health failover retries 3
health relation http0 and
health relation http1 and
health relation http-nginx-0 and
health relation http-nginx-1 and
health relation http-user-1 and
health relation http-user-2 and
health relation http-user-3 and

#ssl configuration

#ssl configuration

#RIP routing configuration

#Bypass configuration

#graph configuration

#slb dns cache configuration
dns cache off
dns cache expire 60 3600

#cache filter configuration
cache filter off

#Qos configuration

#statmon configuration
statmon off

#rip configuration
rip off

#ospf configuration
ospf off

#debug monitor configuration
debug monitor on

#no default user configuration
#

```

## 第 34 章 Array WebUI

https://your_ip_address:8888

array

admin

## 第 35 章 FreeBSD

## uname

```
AN# uname
FreeBSD
AN# uname -a
FreeBSD AN 7.0-RELEASE FreeBSD 7.0-RELEASE #0: Fri Jul 15 17:57:48 CST 2011     rolex@Build2.arraynetworks.com.cn:/usr/home/rolex/build/rel_tm_8_2/src/FreeBSD/src/sys/compile/CA1000  amd64

```

## passwd

```

AN# cat /etc/passwd
# $FreeBSD: src/etc/master.passwd,v 1.40 2005/06/06 20:19:56 brooks Exp $
#
root:*:0:0:Charlie &:/root:/bin/csh
toor:*:0:0:Bourne-again Superuser:/root:
daemon:*:1:1:Owner of many system processes:/root:/usr/sbin/nologin
operator:*:2:5:System &:/:/usr/sbin/nologin
bin:*:3:7:Binaries Commands and Source:/:/usr/sbin/nologin
tty:*:4:65533:Tty Sandbox:/:/usr/sbin/nologin
kmem:*:5:65533:KMem Sandbox:/:/usr/sbin/nologin
games:*:7:13:Games pseudo-user:/usr/games:/usr/sbin/nologin
news:*:8:8:News Subsystem:/:/usr/sbin/nologin
man:*:9:9:Mister Man Pages:/usr/share/man:/usr/sbin/nologin
sshd:*:22:22:Secure Shell Daemon:/var/empty:/usr/sbin/nologin
smmsp:*:25:25:Sendmail Submission User:/var/spool/clientmqueue:/usr/sbin/nologin
mailnull:*:26:26:Sendmail Default User:/var/spool/mqueue:/usr/sbin/nologin
bind:*:53:53:Bind Sandbox:/:/usr/sbin/nologin
proxy:*:62:62:Packet Filter pseudo-user:/nonexistent:/usr/sbin/nologin
_pflogd:*:64:64:pflogd privsep user:/var/empty:/usr/sbin/nologin
_dhcp:*:65:65:dhcp programs:/var/empty:/usr/sbin/nologin
uucp:*:66:66:UUCP pseudo-user:/var/spool/uucppublic:/usr/local/libexec/uucp/uucico
pop:*:68:6:Post Office Owner:/nonexistent:/usr/sbin/nologin
www:*:80:80:World Wide Web Owner:/nonexistent:/usr/sbin/nologin
nobody:*:65534:65534:Unprivileged user:/nonexistent:/usr/sbin/nologin
sync:*:1005:0:sync:/export/sync:/bin/sh
recovery:*:65533:0:Recovery User:/:/ca/bin/recovery
test:*:1002:0:test:/export/test:/bin/csh
array:*:1006:1001:User &:/:/ca/bin/ca_shell

```

ca_shell 就是 Array 的 CLI 接口程序

## process

```

AN# ps ax
  PID  TT  STAT      TIME COMMAND
    0  ??  WLs    0:00.02 [swapper]
    1  ??  ILs    0:00.01 /sbin/init --
    2  ??  DL     0:00.02 [g_event]
    3  ??  DL     0:00.07 [g_up]
    4  ??  DL     0:00.07 [g_down]
    5  ??  DL     0:02.03 [atcp2: core(L4&TQ)]
    6  ??  DL     0:02.11 [atcp3: core(L4&TQ)]
    7  ??  DL     0:04.39 [atcp4: core(L4&TQ)]
    8  ??  DL     0:05.54 [atcp5: core(L4&TQ)]
    9  ??  DL     0:02.21 [atcp6: core(L4&TQ)]
   10  ??  DL     0:00.00 [audit]
   11  ??  RL     6:52.71 [idle: cpu7]
   12  ??  RL     6:52.21 [idle: cpu6]
   13  ??  RL     6:52.47 [idle: cpu5]
   14  ??  RL     6:51.68 [idle: cpu4]
   15  ??  RL     6:49.23 [idle: cpu3]
   16  ??  RL     6:44.41 [idle: cpu2]
   17  ??  RL     6:50.65 [idle: cpu1]
   18  ??  RL     6:31.31 [idle: cpu0]
   19  ??  WL     0:00.98 [swi1: net]
   20  ??  WL     0:08.05 [swi4: clock sio]
   21  ??  WL     0:00.00 [swi3: vm]
   22  ??  DL     0:02.18 [atcp7: core(L4&TQ)]
   23  ??  DL     0:02.10 [atcp8: core(L4&TQ)]
   24  ??  DL     0:02.14 [atcp9: core(L4/7&TQ]
   25  ??  DL     0:00.11 [yarrow]
   26  ??  WL     0:00.00 [swi6: Giant taskq]
   27  ??  WL     0:00.00 [swi6: task queue]
   28  ??  DL     0:00.54 [kqueue taskq]
   29  ??  DL     0:00.56 [acpi_task_0]
   30  ??  DL     0:00.57 [acpi_task_1]
   31  ??  DL     0:00.57 [acpi_task_2]
   32  ??  WL     0:00.00 [swi5: +]
   33  ??  DL     0:00.53 [thread taskq]
   34  ??  WL     0:00.00 [irq9: acpi0]
   35  ??  WL     0:00.00 [irq16: uhci0]
   36  ??  DL     0:00.00 [usb0]
   37  ??  DL     0:00.00 [usbtask-hc]
   38  ??  DL     0:00.00 [usbtask-dr]
   39  ??  WL     0:00.00 [irq21: uhci1]
   40  ??  DL     0:00.00 [usb1]
   41  ??  WL     0:00.02 [irq18: ehci0 uhci+]
   42  ??  DL     0:00.00 [usb2]
   43  ??  WL     0:00.00 [irq23: uhci2 ehci1]
   44  ??  DL     0:00.00 [usb3]
   45  ??  WL     0:00.14 [irq19: uhci3++]
   46  ??  DL     0:00.00 [usb4]
   47  ??  DL     0:00.00 [usb5]
   48  ??  DL     0:00.00 [usb6]
   49  ??  WL     0:00.00 [swi0: sio]
   50  ??  WL     0:00.00 [irq1: atkbd0]
   51  ??  DL     0:02.70 [atcp0: management]
   52  ??  DL     0:00.68 [atcp1: IP]
   53  ??  DL     0:00.00 [pagedaemon]
   54  ??  DL     0:00.00 [vmdaemon]
   55  ??  DL     0:00.00 [pagezero]
   56  ??  DL     0:00.00 [bufdaemon]
   57  ??  DL     0:00.01 [syncer]
   58  ??  DL     0:00.00 [vnlru]
   59  ??  DL     0:00.00 [softdepflush]
26083  ??  Is     0:00.00 /sbin/devd
26131  ??  Ss     0:00.01 /ca/bin/syslogd -s -C
26262  ??  Is     0:00.00 /ca/bin/sshd
26267  ??  S      0:00.03 /ca/bin/eventlogd
26269  ??  S      0:00.01 /ca/bin/sysmond
26274  ??  Is     0:00.00 /ca/bin/authlogd
26279  ??  S      0:00.01 /ca/bin/lcd
26280  ??  S      0:00.03 /ca/bin/cert
26288  ??  S      0:01.45 /ca/bin/hc_daemon
26290  ??  S      0:00.03 /ca/bin/llb_hc_daemon
26293  ??  S      0:00.02 /ca/bin/va
26295  ??  S      0:00.01 /ca/bin/ulandmond /ca/bin/uproxy http
26298  ??  S      0:00.09 /ca/bin/SdnsApp
26299  ??  S      0:00.01 /ca/bin/webui_agent
26303  ??  Ss     0:00.00 /ca/bin/wwlogd
26304  ??  Ss     0:00.00 /ca/bin/backend
26307  ??  S      0:00.01 /ca/bin/snmpinfod
26309  ??  I      0:00.00 /ca/bin/diskfreed
26317  ??  I      0:00.00 /bin/sh /ca/bin/purgegraph.sh /var/run/graphs 3600
26323  ??  I      0:00.00 sleep 3600
26362  ??  Is     0:00.00 /usr/sbin/cron -s
26437  ??  I      0:00.02 /bin/sh /ca/bin/monitor.sh
26521  ??  Ss     0:00.02 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26522  ??  I      0:00.19 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26523  ??  S      0:00.19 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26582  ??  D      0:09.78 /ca/bin/uproxy http 2 0
26583  ??  Is     0:00.00 sh -c cat > /var/log/login_fail_messages
26584  ??  I      0:00.00 cat
26606  ??  D      0:00.17 /ca/bin/uproxy http 3 1
26607  ??  D      0:00.17 /ca/bin/uproxy http 4 2
26608  ??  D      0:00.15 /ca/bin/uproxy http 5 3
26609  ??  D      0:00.16 /ca/bin/uproxy http 6 4
26610  ??  D      0:00.15 /ca/bin/uproxy http 7 5
26611  ??  D      0:00.17 /ca/bin/uproxy http 8 6
26612  ??  D      0:00.16 /ca/bin/uproxy http 9 7
26629  ??  Is     0:00.03 sshd: array@ttyp0 (sshd)
26640  ??  I      0:00.04 /ca/bin/backend
26645  ??  S      0:00.19 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26652  ??  S      0:00.05 ca_shell
26653  ??  I      0:00.00 webui_monitor 26299 26652
26654  ??  S      0:00.01 /ca/bin/backend
26673  ??  I      0:00.10 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26674  ??  I      0:00.16 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26679  ??  I      0:00.00 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26680  ??  I      0:00.00 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26681  ??  I      0:00.00 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26682  ??  I      0:00.00 /ca/an_apache/bin/httpd -f /ca/webui/conf/httpd.conf -DSSL -DARRAYOS
26949  ??  Ss     0:00.02 sshd: test@ttyp1 (sshd)
27289  ??  I      0:00.00 sleep 60
26410  d0  Is+    0:00.00 /usr/libexec/getty std.9600 ttyd0
26408  v0  Is+    0:00.00 /usr/libexec/getty Pc ttyv0
26409  v1  Is+    0:00.00 /usr/libexec/getty Pc ttyv1
26635  p0  Ss+    0:00.04 -ca_shell (ca_shell)
26953  p1  Ss     0:00.01 -csh (csh)
27343  p1  R+     0:00.00 ps ax

```

## webui

Array Web UI 是 PHP4 写的，去/ca/webui/htdocs/proxy/里面看吧，好原始没有 MVC 框架呵呵

```

AN# cat /ca/webui/conf/httpd.conf
##
## httpd.conf -- Apache HTTP server configuration file
##

#
# Based upon the NCSA server configuration files originally by Rob McCool.
#
# This is the main Apache server configuration file.  It contains the
# configuration directives that give the server its instructions.
# See <URL:http://www.apache.org/docs/> for detailed information about
# the directives.
#
# Do NOT simply read the instructions in here without understanding
# what they do.  They're here only as hints or reminders.  If you are unsure
# consult the online docs. You have been warned.
#
# After this file is processed, the server will look for and process
# /ca/webui/conf/srm.conf and then /ca/webui/conf/access.conf
# unless you have overridden these with ResourceConfig and/or
# AccessConfig directives here.
#
# The configuration directives are grouped into three basic sections:
#  1\. Directives that control the operation of the Apache server process as a
#     whole (the 'global environment').
#  2\. Directives that define the parameters of the 'main' or 'default' server,
#     which responds to requests that aren't handled by a virtual host.
#     These directives also provide default values for the settings
#     of all virtual hosts.
#  3\. Settings for virtual hosts, which allow Web requests to be sent to
#     different IP addresses or hostnames and have them handled by the
#     same Apache server process.
#
# Configuration and logfile names: If the filenames you specify for many
# of the server's control files begin with "/" (or "drive:/" for Win32), the
# server will use that explicit path.  If the filenames do *not* begin
# with "/", the value of ServerRoot is prepended -- so "logs/foo.log"
# with ServerRoot set to "/usr/local/apache" will be interpreted by the
# server as "/usr/local/apache/logs/foo.log".
#

### Section 1: Global Environment
#
# The directives in this section affect the overall operation of Apache,
# such as the number of concurrent requests it can handle or where it
# can find its configuration files.
#

#
# ServerType is either inetd, or standalone.  Inetd mode is only supported on
# Unix platforms.
#
ServerType standalone

#
# ServerRoot: The top of the directory tree under which the server's
# configuration, error, and log files are kept.
#
# NOTE!  If you intend to place this on an NFS (or otherwise network)
# mounted filesystem then please read the LockFile documentation
# (available at <URL:http://www.apache.org/docs/mod/core.html#lockfile>);
# you will save yourself a lot of trouble.
#
# Do NOT add a slash at the end of the directory path.
#
ServerRoot "/ca/an_apache"

#
# The LockFile directive sets the path to the lockfile used when Apache
# is compiled with either USE_FCNTL_SERIALIZED_ACCEPT or
# USE_FLOCK_SERIALIZED_ACCEPT. This directive should normally be left at
# its default value. The main reason for changing it is if the logs
# directory is NFS mounted, since the lockfile MUST BE STORED ON A LOCAL
# DISK. The PID of the main server process is automatically appended to
# the filename.
#
#LockFile /ca/webui/logs/httpd.lock

#
# PidFile: The file in which the server should record its process
# identification number when it starts.
#
PidFile /ca/webui/logs/httpd.pid

#
# ScoreBoardFile: File used to store internal server process information.
# Not all architectures require this.  But if yours does (you'll know because
# this file will be  created when you run Apache) then you *must* ensure that
# no two invocations of Apache share the same scoreboard file.
#
ScoreBoardFile /ca/webui/logs/httpd.scoreboard

#
# In the standard configuration, the server will process httpd.conf (this
# file, specified by the -f command line option), srm.conf, and access.conf
# in that order.  The latter two files are now distributed empty, as it is
# recommended that all directives be kept in a single file for simplicity.
# The commented-out values below are the built-in defaults.  You can have the
# server ignore these files altogether by using "/dev/null" (for Unix) or
# "nul" (for Win32) for the arguments to the directives.
#
#ResourceConfig conf/srm.conf
#AccessConfig conf/access.conf

#
# Timeout: The number of seconds before receives and sends time out.
#
Timeout 300

#
# KeepAlive: Whether or not to allow persistent connections (more than
# one request per connection). Set to "Off" to deactivate.
#
KeepAlive On

#
# MaxKeepAliveRequests: The maximum number of requests to allow
# during a persistent connection. Set to 0 to allow an unlimited amount.
# We recommend you leave this number high, for maximum performance.
#
MaxKeepAliveRequests 256

#
# KeepAliveTimeout: Number of seconds to wait for the next request from the
# same client on the same connection.
#
KeepAliveTimeout 100

#
# Server-pool size regulation.  Rather than making you guess how many
# server processes you need, Apache dynamically adapts to the load it
# sees --- that is, it tries to maintain enough server processes to
# handle the current load, plus a few spare servers to handle transient
# load spikes (e.g., multiple simultaneous requests from a single
# Netscape browser).
#
# It does this by periodically checking how many servers are waiting
# for a request.  If there are fewer than MinSpareServers, it creates
# a new spare.  If there are more than MaxSpareServers, some of the
# spares die off.  The default values are probably OK for most sites.
#
MinSpareServers 2
MaxSpareServers 5

#
# Number of servers to start initially --- should be a reasonable ballpark
# figure.
#
StartServers 1

#
# Limit on total number of servers running, i.e., limit on the number
# of clients who can simultaneously connect --- if this limit is ever
# reached, clients will be LOCKED OUT, so it should NOT BE SET TOO LOW.
# It is intended mainly as a brake to keep a runaway server from taking
# the system with it as it spirals down...
#
MaxClients 100

#
# MaxRequestsPerChild: the number of requests each child process is
# allowed to process before the child dies.  The child will exit so
# as to avoid problems after prolonged use when Apache (and maybe the
# libraries it uses) leak memory or other resources.  On most systems, this
# isn't really needed, but a few (such as Solaris) do have notable leaks
# in the libraries. For these platforms, set to something like 10000
# or so; a setting of 0 means unlimited.
#
# NOTE: This value does not include keepalive requests after the initial
#       request per connection. For example, if a child process handles
#       an initial request and 10 subsequent "keptalive" requests, it
#       would only count as 1 request towards this limit.
#
MaxRequestsPerChild 10000

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, in addition to the default. See also the <VirtualHost>
# directive.
#
#Listen 3000
#Listen 12.34.56.78:80

#
# BindAddress: You can support virtual hosts with this option. This directive
# is used to tell the server which IP address to listen to. It can either
# contain "*", an IP address, or a fully qualified Internet domain name.
# See also the <VirtualHost> and Listen directives.
#
#BindAddress *

#
# Dynamic Shared Object (DSO) Support
#
# To be able to use the functionality of a module which was built as a DSO you
# have to place corresponding `LoadModule' lines at this location so the
# directives contained in it are actually available _before_ they are used.
# Please read the file README.DSO in the Apache 1.3 distribution for more
# details about the DSO mechanism and run `httpd -l' for the list of already
# built-in (statically linked and thus always available) modules in your httpd
# binary.
#
# Note: The order in which modules are loaded is important.  Don't change
# the order below without expert advice.
#
# Example:
# LoadModule foo_module libexec/mod_foo.so

#
# ExtendedStatus controls whether Apache will generate "full" status
# information (ExtendedStatus On) or just basic information (ExtendedStatus
# Off) when the "server-status" handler is called. The default is Off.
#
#ExtendedStatus On

### Section 2: 'Main' server configuration
#
# The directives in this section set up the values used by the 'main'
# server, which responds to any requests that aren't handled by a
# <VirtualHost> definition.  These values also provide defaults for
# any <VirtualHost> containers you may define later in the file.
#
# All of these directives may appear inside <VirtualHost> containers,
# in which case these default settings will be overridden for the
# virtual host being defined.
#

#
# If your ServerType directive (set earlier in the 'Global Environment'
# section) is set to "inetd", the next few directives don't have any
# effect since their settings are defined by the inetd configuration.
# Skip ahead to the ServerAdmin directive.
#

#
# Port: The port to which the standalone server listens. For
# ports < 1023, you will need httpd to be run as root initially.
#
##
##  SSL Support
##
##  When we also provide SSL we have to listen to the
##  standard HTTP port (see above) and to the HTTPS port
##
Include /ca/conf/webui.conf

#
# If you wish httpd to run as a different user or group, you must run
# httpd as root initially and it will switch.
#
# User/Group: The name (or #number) of the user/group to run httpd as.
#  . On SCO (ODT 3) use "User nouser" and "Group nogroup".
#  . On HPUX you may not be able to use shared memory as nobody, and the
#    suggested workaround is to create a user www and use that user.
#  NOTE that some kernels refuse to setgid(Group) or semctl(IPC_SET)
#  when the value of (unsigned)Group is above 60000;
#  don't use Group nobody on these systems!
#
User nobody
Group nobody

#
# ServerAdmin: Your address, where problems with the server should be
# e-mailed.  This address appears on some server-generated pages, such
# as error documents.
#
ServerAdmin support@arraynetworks.net

#
# ServerName allows you to set a host name which is sent back to clients for
# your server if it's different than the one the program would get (i.e., use
# "www" instead of the host's real name).
#
# Note: You cannot just invent host names and hope they work. The name you
# define here must be a valid DNS name for your host. If you don't understand
# this, ask your network administrator.
# If your host doesn't have a registered DNS name, enter its IP address here.
# You will have to access it by its address (e.g., http://123.45.67.89/)
# anyway, and this will make redirections work in a sensible way.
#
# 127.0.0.1 is the TCP/IP local loop-back address, often named localhost. Your
# machine always knows itself by this address. If you use Apache strictly for
# local testing and development, you may use 127.0.0.1 as the server name.
#
ServerName 127.0.0.1

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
# in /ca/conf/webui.conf

#
# Each directory to which Apache has access, can be configured with respect
# to which services and features are allowed and/or disabled in that
# directory (and its subdirectories).
#
# First, we configure the "default" to be a very restrictive set of
# permissions.
#
<Directory />
    Options FollowSymLinks
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>

#
# Note that from this point forward you must specifically allow
# particular features to be enabled - so if something's not working as
# you might expect, make sure that you have specifically enabled it
# below.
#

#
# This should be changed to whatever you set DocumentRoot to.
#

#
# UserDir: The name of the directory which is appended onto a user's home
# directory if a ~user request is received.
#
<IfModule mod_userdir.c>
    UserDir public_html
</IfModule>

#
# Control access to UserDir directories.  The following is an example
# for a site where these directories are restricted to read-only.
#
<Directory /home/*/public_html>
    Options None
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>

#
# DirectoryIndex: Name of the file or files to use as a pre-written HTML
# directory index.  Separate multiple entries with spaces.
#
<IfModule mod_dir.c>
    DirectoryIndex index.html
</IfModule>

#
# AccessFileName: The name of the file to look for in each directory
# for access control information.
#
AccessFileName .htaccess

#
# The following lines prevent .htaccess files from being viewed by
# Web clients.  Since .htaccess files often contain authorization
# information, access is disallowed for security reasons.  Comment
# these lines out if you want Web visitors to see the contents of
# .htaccess files.  If you change the AccessFileName directive above,
# be sure to make the corresponding changes here.
#
# Also, folks tend to use names such as .htpasswd for password
# files, so this will protect those as well.
#

#
# CacheNegotiatedDocs: By default, Apache sends "Pragma: no-cache" with each
# document that was negotiated on the basis of content. This asks proxy
# servers not to cache the document. Uncommenting the following line disables
# this behavior, and proxies will be allowed to cache the documents.
#
#CacheNegotiatedDocs

#
# UseCanonicalName:  (new for 1.3)  With this setting turned on, whenever
# Apache needs to construct a self-referencing URL (a URL that refers back
# to the server the response is coming from) it will use ServerName and
# Port to form a "canonical" name.  With this setting off, Apache will
# use the hostname:port that the client supplied, when possible.  This
# also affects SERVER_NAME and SERVER_PORT in CGI scripts.
#
UseCanonicalName Off

#
# TypesConfig describes where the mime.types file (or equivalent) is
# to be found.
#
<IfModule mod_mime.c>
    TypesConfig /ca/an_apache/conf/mime.types
</IfModule>

#
# DefaultType is the default MIME type the server will use for a document
# if it cannot otherwise determine one, such as from filename extensions.
# If your server contains mostly text or HTML documents, "text/plain" is
# a good value.  If most of your content is binary, such as applications
# or images, you may want to use "application/octet-stream" instead to
# keep browsers from trying to display binary files as though they are
# text.
#
DefaultType text/plain

#
# The mod_mime_magic module allows the server to use various hints from the
# contents of the file itself to determine its type.  The MIMEMagicFile
# directive tells the module where the hint definitions are located.
# mod_mime_magic is not part of the default server (you have to add
# it yourself with a LoadModule [see the DSO paragraph in the 'Global
# Environment' section], or recompile the server and include mod_mime_magic
# as part of the configuration), so it's enclosed in an <IfModule> container.
# This means that the MIMEMagicFile directive will only be processed if the
# module is part of the server.
#
<IfModule mod_mime_magic.c>
    MIMEMagicFile /ca/an_apache/conf/magic
</IfModule>

#
# HostnameLookups: Log the names of clients or just their IP addresses
# e.g., www.apache.org (on) or 204.62.129.132 (off).
# The default is off because it'd be overall better for the net if people
# had to knowingly turn this feature on, since enabling it means that
# each client request will result in AT LEAST one lookup request to the
# nameserver.
#
HostnameLookups Off

#
# ErrorLog: The location of the error log file.
# If you do not specify an ErrorLog directive within a <VirtualHost>
# container, error messages relating to that virtual host will be
# logged here.  If you *do* define an error logfile for a <VirtualHost>
# container, that host's errors will be logged there and not here.
#
#ErrorLog /ca/webui/logs/error_log
ErrorLog syslog:user

#
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel error

#
# The following directives define some format nicknames for use with
# a CustomLog directive (see below).
#
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %b" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent

#
# The location and format of the access logfile (Common Logfile Format).
# If you do not define any access logfiles within a <VirtualHost>
# container, they will be logged here.  Contrariwise, if you *do*
# define per-<VirtualHost> access logfiles, transactions will be
# logged therein and *not* in this file.
#
#CustomLog /ca/webui/logs/access_log common

#
# If you would like to have agent and referer logfiles, uncomment the
# following directives.
#
#CustomLog /ca/webui/logs/referer_log referer
#CustomLog /ca/webui/logs/agent_log agent

#
# If you prefer a single logfile with access, agent, and referer information
# (Combined Logfile Format) you can use the following directive.
#
#CustomLog /ca/webui/logs/access_log combined

#
# Optionally add a line containing the server version and virtual host
# name to server-generated pages (error documents, FTP directory listings,
# mod_status and mod_info output etc., but not CGI generated documents).
# Set to "EMail" to also include a mailto: link to the ServerAdmin.
# Set to one of:  On | Off | EMail
#
ServerSignature Off

#
# Aliases: Add here as many aliases as you need (with no limit). The format is
# Alias fakename realname
#
<IfModule mod_alias.c>

    #
    # Note that if you include a trailing / on fakename then the server will
    # require it to be present in the URL.  So "/icons" isn't aliased in this
    # example, only "/icons/"..
    #

    <Directory "/ca/webui/icons">
        Options Indexes MultiViews
        AllowOverride None
        Order allow,deny
        Allow from all
    </Directory>

    #
    # ScriptAlias: This controls which directories contain server scripts.
    # ScriptAliases are essentially the same as Aliases, except that
    # documents in the realname directory are treated as applications and
    # run by the server when requested rather than as documents sent to the client.
    # The same rules about trailing "/" apply to ScriptAlias directives as to
    # Alias.
    #
    ScriptAlias /cgi-bin/ "/ca/webui/cgi-bin/"

    #"/ca/webui/cgi-bin" should be changed to whatever your ScriptAliased
    #CGI directory exists, if you have that configured.

    <Directory "/ca/webui/cgi-bin">
        Options None
        AllowOverride AuthConfig
        Order allow,deny
        Allow from all
    </Directory>

</IfModule>
# End of aliases.

#
# Redirect allows you to tell clients about documents which used to exist in
# your server's namespace, but do not anymore. This allows you to tell the
# clients where to look for the relocated document.
# Format: Redirect old-URI new-URL
#

#
# Directives controlling the display of server-generated directory listings.
#
<IfModule mod_autoindex.c>

    #
    # FancyIndexing is whether you want fancy directory indexing or standard
    #
    IndexOptions FancyIndexing

    #
    # AddIcon* directives tell the server which icon to show for different
    # files or filename extensions.  These are only displayed for
    # FancyIndexed directories.
    #
    AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip

    AddIconByType (TXT,/icons/text.gif) text/*
    AddIconByType (IMG,/icons/image2.gif) image/*
    AddIconByType (SND,/icons/sound2.gif) audio/*
    AddIconByType (VID,/icons/movie.gif) video/*

    AddIcon /icons/binary.gif .bin .exe
    AddIcon /icons/binhex.gif .hqx
    AddIcon /icons/tar.gif .tar
    AddIcon /icons/world2.gif .wrl .wrl.gz .vrml .vrm .iv
    AddIcon /icons/compressed.gif .Z .z .tgz .gz .zip
    AddIcon /icons/a.gif .ps .ai .eps
    AddIcon /icons/layout.gif .html .shtml .htm .pdf
    AddIcon /icons/text.gif .txt
    AddIcon /icons/c.gif .c
    AddIcon /icons/p.gif .pl .py
    AddIcon /icons/f.gif .for
    AddIcon /icons/dvi.gif .dvi
    AddIcon /icons/uuencoded.gif .uu
    AddIcon /icons/script.gif .conf .sh .shar .csh .ksh .tcl
    AddIcon /icons/tex.gif .tex
    AddIcon /icons/bomb.gif core

    AddIcon /icons/back.gif ..
    AddIcon /icons/hand.right.gif README
    AddIcon /icons/folder.gif ^^DIRECTORY^^
    AddIcon /icons/blank.gif ^^BLANKICON^^

    #
    # DefaultIcon is which icon to show for files which do not have an icon
    # explicitly set.
    #
    DefaultIcon /icons/unknown.gif

    #
    # AddDescription allows you to place a short description after a file in
    # server-generated indexes.  These are only displayed for FancyIndexed
    # directories.
    # Format: AddDescription "description" filename
    #
    #AddDescription "GZIP compressed document" .gz
    #AddDescription "tar archive" .tar
    #AddDescription "GZIP compressed tar archive" .tgz

    #
    # ReadmeName is the name of the README file the server will look for by
    # default, and append to directory listings.
    #
    # HeaderName is the name of a file which should be prepended to
    # directory indexes.
    #
    # If MultiViews are amongst the Options in effect, the server will
    # first look for name.html and include it if found.  If name.html
    # doesn't exist, the server will then look for name.txt and include
    # it as plaintext if found.
    #
    ReadmeName README
    HeaderName HEADER

    #
    # IndexIgnore is a set of filenames which directory indexing should ignore
    # and not include in the listing.  Shell-style wildcarding is permitted.
    #
    IndexIgnore .??* *~ *# HEADER* README* RCS CVS *,v *,t

</IfModule>
# End of indexing directives.

#
# Document types.
#
<IfModule mod_mime.c>

    #
    # AddEncoding allows you to have certain browsers (Mosaic/X 2.1+) uncompress
    # information on the fly. Note: Not all browsers support this.
    # Despite the name similarity, the following Add* directives have nothing
    # to do with the FancyIndexing customization directives above.
    #
    AddEncoding x-compress Z
    AddEncoding x-gzip gz tgz

    #
    # AddLanguage allows you to specify the language of a document. You can
    # then use content negotiation to give a browser a file in a language
    # it can understand.
    #
    # Note 1: The suffix does not have to be the same as the language
    # keyword --- those with documents in Polish (whose net-standard
    # language code is pl) may wish to use "AddLanguage pl .po" to
    # avoid the ambiguity with the common suffix for perl scripts.
    #
    # Note 2: The example entries below illustrate that in quite
    # some cases the two character 'Language' abbriviation is not
    # identical to the two character 'Country' code for its country,
    # E.g. 'Danmark/dk' versus 'Danish/da'.
    #
    # Note 3: In the case of 'ltz' we violate the RFC by using a three char
    # specifier. But there is 'work in progress' to fix this and get
    # the reference data for rfc1766 cleaned up.
    #
    # Danish (da) - Dutch (nl) - English (en) - Estonian (ee)
    # French (fr) - German (de) - Greek-Modern (el)
    # Italian (it) - Korean (kr) - Norwegian (no)
    # Portugese (pt) - Luxembourgeois* (ltz)
    # Spanish (es) - Swedish (sv) - Catalan (ca) - Czech(cz)
    # Polish (pl) - Brazilian Portuguese (pt-br) - Japanese (ja)
    # Russian (ru)
    #
    AddLanguage da .dk
    AddLanguage nl .nl
    AddLanguage en .en
    AddLanguage et .ee
    AddLanguage fr .fr
    AddLanguage de .de
    AddLanguage el .el
    AddLanguage he .he
    AddCharset ISO-8859-8 .iso8859-8
    AddLanguage it .it
    AddLanguage ja .ja
    AddCharset ISO-2022-JP .jis
    AddLanguage kr .kr
    AddCharset ISO-2022-KR .iso-kr
    AddLanguage no .no
    AddLanguage pl .po
    AddCharset ISO-8859-2 .iso-pl
    AddLanguage pt .pt
    AddLanguage pt-br .pt-br
    AddLanguage ltz .lu
    AddLanguage ca .ca
    AddLanguage es .es
    AddLanguage sv .se
    AddLanguage cz .cz
    AddLanguage ru .ru
    AddLanguage tw .tw
    AddCharset Big5         .Big5    .big5
    AddCharset WINDOWS-1251 .cp-1251
    AddCharset CP866        .cp866
    AddCharset ISO-8859-5   .iso-ru
    AddCharset KOI8-R       .koi8-r
    AddCharset UCS-2        .ucs2
    AddCharset UCS-4        .ucs4
    AddCharset UTF-8        .utf8

    # LanguagePriority allows you to give precedence to some languages
    # in case of a tie during content negotiation.
    #
    # Just list the languages in decreasing order of preference. We have
    # more or less alphabetized them here. You probably want to change this.
    #
    <IfModule mod_negotiation.c>
        LanguagePriority en da nl et fr de el it ja kr no pl pt pt-br ru ltz ca es sv tw
    </IfModule>

    #
    # AddType allows you to tweak mime.types without actually editing it, or to
    # make certain files to be certain types.
    #
    # For example, the PHP 3.x module (not part of the Apache distribution - see
    # http://www.php.net) will typically use:
    #
    #AddType application/x-httpd-php3 .php3
    #AddType application/x-httpd-php3-source .phps
    #
    # And for PHP 4.x, use:
    #
    XBitHack on
    AddType application/x-httpd-php .html
    AddType application/x-httpd-php-source .phps

    AddType application/x-tar .tgz

    #
    # AddHandler allows you to map certain file extensions to "handlers",
    # actions unrelated to filetype. These can be either built into the server
    # or added with the Action command (see below)
    #
    # If you want to use server side includes, or CGI outside
    # ScriptAliased directories, uncomment the following lines.
    #
    # To use CGI scripts:
    #
    #AddHandler cgi-script .cgi

    #
    # To use server-parsed HTML files
    #
    #AddType text/html .shtml
    #AddHandler server-parsed .shtml

    #
    # Uncomment the following line to enable Apache's send-asis HTTP file
    # feature
    #
    #AddHandler send-as-is asis

    #
    # If you wish to use server-parsed imagemap files, use
    #
    #AddHandler imap-file map

    #
    # To enable type maps, you might want to use
    #
    #AddHandler type-map var

</IfModule>
# End of document types.

#
# Action lets you define media types that will execute a script whenever
# a matching file is called. This eliminates the need for repeated URL
# pathnames for oft-used CGI file processors.
# Format: Action media/type /cgi-script/location
# Format: Action handler-name /cgi-script/location
#

#
# MetaDir: specifies the name of the directory in which Apache can find
# meta information files. These files contain additional HTTP headers
# to include when sending the document
#
#MetaDir .web

#
# MetaSuffix: specifies the file name suffix for the file containing the
# meta information.
#
#MetaSuffix .meta

#
# Customizable error response (Apache style)
#  these come in three flavors
#
ErrorDocument 406 "Error 406: Not Acceptable
ErrorDocument 405 "Error 405: Method Not Allowed
ErrorDocument 404 "The file you have requested does not exist.
ErrorDocument 403 "Error 403: Forbidden
ErrorDocument 401 "Error 401: Unauthorized
ErrorDocument 400 "This server uses SSL for security. Please use HTTPS to connect.

#
# Customize behaviour based on the browser
#
<IfModule mod_setenvif.c>

    #
    # The following directives modify normal HTTP response behavior.
    # The first directive disables keepalive for Netscape 2.x and browsers that
    # spoof it. There are known problems with these browser implementations.
    # The second directive is for Microsoft Internet Explorer 4.0b2
    # which has a broken HTTP/1.1 implementation and does not properly
    # support keepalive when it is used on 301 or 302 (redirect) responses.
    #
    BrowserMatch "Mozilla/2" nokeepalive
    BrowserMatch "MSIE 4\.0b2;" nokeepalive downgrade-1.0 force-response-1.0

    #
    # The following directive disables HTTP/1.1 responses to browsers which
    # are in violation of the HTTP/1.0 spec by not being able to grok a
    # basic 1.1 response.
    #
    BrowserMatch "RealPlayer 4\.0" force-response-1.0
    BrowserMatch "Java/1\.0" force-response-1.0
    BrowserMatch "JDK/1\.0" force-response-1.0

</IfModule>
# End of browser customization directives

#
# Allow server status reports, with the URL of http://servername/server-status
# Change the ".your_domain.com" to match your domain to enable.
#
#<Location /server-status>
#    SetHandler server-status
#    Order deny,allow
#    Deny from all
#    Allow from .your_domain.com
#</Location>

#
# Allow remote server configuration reports, with the URL of
# http://servername/server-info (requires that mod_info.c be loaded).
# Change the ".your_domain.com" to match your domain to enable.
#
#<Location /server-info>
#    SetHandler server-info
#    Order deny,allow
#    Deny from all
#    Allow from .your_domain.com
#</Location>

#
# There have been reports of people trying to abuse an old bug from pre-1.1
# days.  This bug involved a CGI script distributed as a part of Apache.
# By uncommenting these lines you can redirect these attacks to a logging
# script on phf.apache.org.  Or, you can record them yourself, using the script
# support/phf_abuse_log.cgi.
#
#<Location /cgi-bin/phf*>
#    Deny from all
#    ErrorDocument 403 http://phf.apache.org/phf_abuse_log.cgi
#</Location>

#
# Proxy Server directives. Uncomment the following lines to
# enable the proxy server:
#
#<IfModule mod_proxy.c>
#    ProxyRequests On

#    <Directory proxy:*>
#        Order deny,allow
#        Deny from all
#        Allow from .your_domain.com
#    </Directory>

    #
    # Enable/disable the handling of HTTP/1.1 "Via:" headers.
    # ("Full" adds the server version; "Block" removes all outgoing Via: headers)
    # Set to one of: Off | On | Full | Block
    #
#    ProxyVia On

    #
    # To enable the cache as well, edit and uncomment the following lines:
    # (no cacheing without CacheRoot)
    #
#    CacheRoot "/ca/webui/proxy"
#    CacheSize 5
#    CacheGcInterval 4
#    CacheMaxExpire 24
#    CacheLastModifiedFactor 0.1
#    CacheDefaultExpire 1
#    NoCache a_domain.com another_domain.edu joes.garage_sale.com

#</IfModule>
# End of proxy directives.

### Section 3: Virtual Hosts
#
# VirtualHost: If you want to maintain multiple domains/hostnames on your
# machine you can setup VirtualHost containers for them. Most configurations
# use only name-based virtual hosts so the server doesn't need to worry about
# IP addresses. This is indicated by the asterisks in the directives below.
#
# Please see the documentation at <URL:http://www.apache.org/docs/vhosts/>
# for further details before you try to setup virtual hosts.
#
# You may use the command line option '-S' to verify your virtual host
# configuration.

#
# Use name-based virtual hosting.
#
#NameVirtualHost *

#
# VirtualHost example:
# Almost any Apache directive may go into a VirtualHost container.
# The first VirtualHost section is used for requests without a known
# server name.
#
#<VirtualHost *>
#    ServerAdmin webmaster@dummy-host.example.com
#    DocumentRoot /www/docs/dummy-host.example.com
#    ServerName dummy-host.example.com
#    ErrorLog logs/dummy-host.example.com-error_log
#    CustomLog logs/dummy-host.example.com-access_log common
#</VirtualHost>

#<VirtualHost _default_:*>
#</VirtualHost>

##
##  SSL Global Context
##
##  All SSL configuration in this context applies both to
##  the main server and all SSL-enabled virtual hosts.
##

#
#   Some MIME-types for downloading Certificates and CRLs
#
<IfDefine SSL>
AddType application/x-x509-ca-cert .crt
AddType application/x-pkcs7-crl    .crl
</IfDefine>

<IfModule mod_ssl.c>

#   Pass Phrase Dialog:
#   Configure the pass phrase gathering process.
#   The filtering dialog program (`builtin' is a internal
#   terminal dialog) has to provide the pass phrase on stdout.
SSLPassPhraseDialog  builtin

#   Inter-Process Session Cache:
#   Configure the SSL Session Cache: First either `none'
#   or `dbm:/path/to/file' for the mechanism to use and
#   second the expiring timeout (in seconds).
#SSLSessionCache        none
#SSLSessionCache        shm:/ca/webui/logs/ssl_scache(512000)
SSLSessionCache         dbm:/ca/webui/logs/ssl_scache
SSLSessionCacheTimeout  300

#   Semaphore:
#   Configure the path to the mutual explusion semaphore the
#   SSL engine uses internally for inter-process synchronization.
SSLMutex  file:/ca/webui/logs/ssl_mutex

#   Pseudo Random Number Generator (PRNG):
#   Configure one or more sources to seed the PRNG of the
#   SSL library. The seed data should be of good random quality.
#   WARNING! On some platforms /dev/random blocks if not enough entropy
#   is available. This means you then cannot use the /dev/random device
#   because it would lead to very long connection times (as long as
#   it requires to make more entropy available). But usually those
#   platforms additionally provide a /dev/urandom device which doesn't
#   block. So, if available, use this one instead. Read the mod_ssl User
#   Manual for more details.
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
#SSLRandomSeed startup file:/dev/random  512
#SSLRandomSeed startup file:/dev/urandom 512
#SSLRandomSeed connect file:/dev/random  512
#SSLRandomSeed connect file:/dev/urandom 512

#   Logging:
#   The home of the dedicated SSL protocol logfile. Errors are
#   additionally duplicated in the general error log file.  Put
#   this somewhere where it cannot be used for symlink attacks on
#   a real server (i.e. somewhere where only root can write).
#   Log levels are (ascending order: higher ones include lower ones):
#   none, error, warn, info, trace, debug.
#SSLLog      /ca/webui/logs/ssl_engine_log
SSLLogLevel none

</IfModule>

# Shockwave Flash
AddType application/x-shockwave-flash swf
AddType application/futuresplash spl

# Support new php register_globals as OFF until webui code and be changed
# (probably never)
<IfModule mod_php4.c>
    php_flag register_globals on
        # Bug 17223, lintq, 20070807
        # Since the build size of TMX has bigger than the original one,
        # I increase this size to entitle user to upgrade build from WebUI,
        # using mode of "Local Host File".
    php_admin_value post_max_size 80000000
    php_admin_value upload_max_filesize 80000000
        # Bug 17223, end
    php_admin_value upload_tmp_dir /tmp
</IfModule>

```

```

AN# cat /ca/conf/webui.conf
# Configuration File for WebUI

<Files ~ "^\.ht">
    Order allow,deny
    Deny from all
</Files>

<Files ~ "^\.inc">
    Order allow,deny
    Deny from all
</Files>

<IfDefine ARRAYOS>
<IfDefine SSL>
Listen 8888

Alias /monitor/graphs /var/run/graphs
<Directory /var/run/graphs>
    Options None
    Allow from all
</Directory>

# For ArrayOS, allow all ciphersuites
<VirtualHost _default_:8888>
ErrorLog syslog:user
# Bug 15193, lintq, 20070105
# This log isn't necessary for user, to avoid filling up
# file system, disable it.
# TransferLog none
# Bug 15193, end
SSLEngine on
# disable SSLv2 and low strength encryption to cover security holes
SSLProtocol all -SSLv2
#SSLCipherSuite ALL:!ADH:!EXP56:RC4+RSA:+HIGH:+MEDIUM:-LOW:-SSLv2:+EXP:+eNULL
SSLCipherSuite  kRSA:kDHr:kDHd:kEDH:-EXP
SSLCertificateFile /ca/an_apache/conf/ssl.crt/server.crt
SSLCertificateKeyFile /ca/an_apache/conf/ssl.key/server.key
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/ca/webui/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

#   Followings are not needed any more for new version of IE.
#   SSL Protocol Adjustments:
#   For MSIE's broken keep-alive and HTTP/1.1 implementations.
#SetEnvIf User-Agent ".*MSIE.*" \
#         nokeepalive ssl-unclean-shutdown \
#         downgrade-1.0 force-response-1.0

#   Per-Server Logging:
#CustomLog /ca/webui/logs/ssl_request_log \
#          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
SSLLogLevel none

</VirtualHost>

</IfDefine>

DocumentRoot "/ca/webui/htdocs/proxy/new"
<Directory "/ca/webui/htdocs/proxy/new">
    Options FollowSymLinks
    AllowOverride AuthConfig Options
    Order allow,deny
    Allow from all
    php_admin_value upload_tmp_dir /tmp
    php_value upload_max_filesize 200M
    php_value post_max_size 200M
</Directory>
</IfDefine>

# For SProxy, only allow high grade encryption
<IfDefine SPROXY>
<IfDefine SSL>
Listen 8888

<VirtualHost _default_:8888>
ErrorLog syslog:user
TransferLog none
SSLEngine on
# disable SSLv2 and low strength encryption to cover security holes
SSLProtocol all -SSLv2
SSLCipherSuite RC4-SHA:RC4-MD5:DES-CBC3-SHA:DES-CBC3-MD5
SSLCertificateFile /ca/an_apache/conf/ssl.crt/server.crt
SSLCertificateKeyFile /ca/an_apache/conf/ssl.key/server.key
<Files ~ "\.(cgi|shtml|phtml|php3?)$">
    SSLOptions +StdEnvVars
</Files>
<Directory "/ca/webui/cgi-bin">
    SSLOptions +StdEnvVars
</Directory>

#   SSL Protocol Adjustments:
#   Followings are not needed any more for new version of IE.
#   SSL Protocol Adjustments:
#   For MSIE's broken keep-alive and HTTP/1.1 implementations.
#SetEnvIf User-Agent ".*MSIE.*" \
#         nokeepalive ssl-unclean-shutdown \
#         downgrade-1.0 force-response-1.0

#   Per-Server Logging:
#CustomLog /ca/webui/logs/ssl_request_log \
#          "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
SSLLogLevel none

</VirtualHost>

</IfDefine>

DocumentRoot "/ca/webui/htdocs/sproxy"
<Directory "/ca/webui/htdocs/sproxy">
    Options FollowSymLinks
    AllowOverride AuthConfig
    Order allow,deny
    Allow from all
        SSLRequireSSL
</Directory>
</IfDefine>

```

## array route

Array Route 包括 RIP，OSPF，BGP 都是使用 zebra 实现

```
AN# cat /ca/etc/zebra.conf
!
! Zebra configuration file
!

```

## ssh 证书植入

```
neo@neo-OptiPlex-780:~$ ssh-copy-id test@172.16.0.9
test@172.16.0.9's password:
Now try logging into the machine, with "ssh 'test@172.16.0.9'", and check in:

  ~/.ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting.

neo@neo-OptiPlex-780:~$ ssh test@172.16.0.9
AN#

```

这样方便登录

## pkg_version

看不到太多信息

```
AN# pkg_version -v
perl-5.8.8_1                        =   up-to-date with port
AN# pkg_version
perl                                =

```

```
AN# find /usr/ports/ | more
/usr/ports/

```

## /etc/fstab

```
AN# cat /etc/fstab
# Device                Mountpoint      FStype  Options         Dump    Pass#
/dev/ad4s1b             none            swap    sw              0       0
/dev/ad4s1a             /boot           ufs     rw              2       2
/dev/ad4s1d             /               ufs     rw              1       1
/dev/ad4s1e             /ca_upgrade     ufs     rw              2       2
/dev/ad4s1f             /var            ufs     rw              2       2
/dev/ad4s1g             /tmp            ufs     rw              2       2
/dev/acd0               /cdrom          cd9660  ro,noauto       0       0

AN# df -h
Filesystem     Size    Used   Avail Capacity  Mounted on
/dev/ad4s1d    7.5G    1.1G    5.8G    15%    /
devfs          1.0K    1.0K      0B   100%    /dev
/dev/ad4s1a    186M    354K    171M     0%    /boot
/dev/ad4s1e    7.5G    1.1G    5.8G    16%    /ca_upgrade
/dev/ad4s1f     91G    450M     83G     1%    /var
/dev/ad4s1g    3.7G    7.0K    3.4G     0%    /tmp
AN#

```