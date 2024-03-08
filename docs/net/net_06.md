# 部分 IV. H3C

## 第 20 章 H3C Command

## config

### current-configuration

current-configuration

```

[WA2220E-AG]display current-configuration
#
 version 5.20, Release 1110P01
#
 sysname WA2220E-AG
#
 domain default enable system
#
 telnet server enable
#
 port-security enable
#
vlan 1
#
radius scheme system
#
domain system
 access-limit disable
 state active
 idle-cut disable
 self-service-url disable
#
user-group system
#
local-user admin
 password simple h3capadmin
 authorization-attribute level 3
 service-type telnet
#
wlan rrm
 dot11a mandatory-rate 6 12 24
 dot11a supported-rate 9 18 36 48 54
 dot11b mandatory-rate 1 2
 dot11b supported-rate 5.5 11
 dot11g mandatory-rate 1 2 5.5 11
 dot11g supported-rate 6 9 12 18 24 36 48 54
#
wlan service-template 1 clear
 ssid H3C
 service-template enable
#
wlan service-template 2 crypto
 ssid www.davosfair.com
#
interface NULL0
#
interface Vlan-interface1
 ip address 172.16.1.1 255.255.255.0
#
interface Ethernet1/0/1
#
interface WLAN-BSS1
#
interface WLAN-BSS2
#
interface WLAN-Radio1/0/1
 channel auto
 max-power 19
#
interface WLAN-Radio1/0/2
 channel 1
 max-power 20
 service-template 1 interface wlan-bss 1
 service-template 2 interface wlan-bss 2
#
 ip route-static 0.0.0.0 0.0.0.0 172.16.0.254
#
 load xml-configuration
#
user-interface con 0
user-interface vty 0 4
 authentication-mode scheme
#
return
[WA2220E-AG]

```

### saved-configuration

saved-configuration

```

[WA2220E-AG]display saved-configuration
#
 version 5.20, Release 1110P01
#
 sysname WA2220E-AG
#
 domain default enable system
#
 telnet server enable
#
 port-security enable
#
vlan 1
#
domain system
 access-limit disable
 state active
 idle-cut disable
 self-service-url disable
#
user-group system
#
local-user admin
 password simple h3capadmin
 authorization-attribute level 3
 service-type telnet
#
wlan rrm
 dot11a mandatory-rate 6 12 24
 dot11a supported-rate 9 18 36 48 54
 dot11b mandatory-rate 1 2
 dot11b supported-rate 5.5 11
 dot11g mandatory-rate 1 2 5.5 11
 dot11g supported-rate 6 9 12 18 24 36 48 54
#
wlan service-template 1 clear
 ssid H3C
#
wlan service-template 2 clear
 ssid www.example.com
 service-template enable
#
interface NULL0
#
interface Vlan-interface1
 ip address 192.168.0.50 255.255.255.0
#
interface Ethernet1/0/1
#
interface WLAN-BSS1
#
interface WLAN-BSS2
#
interface WLAN-Radio1/0/1
 channel auto
 max-power 19
#
interface WLAN-Radio1/0/2
 channel 1
 max-power 20
 service-template 1 interface wlan-bss 1
 service-template 2 interface wlan-bss 2
#
 load xml-configuration
#
user-interface con 0
user-interface vty 0 4
 authentication-mode scheme
#
return

```

### 保存配置

```

[WA2220E-AG]save
The current configuration will be written to the device. Are you sure? [Y/N]:y
Please input the file name(*.cfg)[flash:/startup.cfg]
(To leave the existing filename unchanged, press the enter key):
flash:/startup.cfg exists, overwrite? [Y/N]:y
 Validating file. Please wait.............................
 Configuration is saved to device successfully.

```

## boot-loader

```

<H3C>dis   boot-loader
 Unit 1:
   The current boot app is: s36si_e-cmw310-r1702p13-s168.bin
   The main boot app is:    s36si_e-cmw310-r1702p13-s168.bin
   The backup boot app is:  s3900si-vrp310-t1540-bad.bin

```

## display

### 显示设备工作状态

```
display lock
display users
display cpu
display memory
display fan
display device
display power

```

### 接口相关信息

```
display arp | include 77
display arp _count 计算 arp 表的记录数
display ndp 显示交换机端口的详细配置信息。
display ntdp device-list verbose 收集设备详细信息

```

## 第 21 章 H3C Switch

### *S3600 S5500 Series*

## 配置文件

```

<H3C>reset saved-configuration
The saved configuration will be erased.
Are you sure?[Y/N]y
Configuration in flash memory is being cleared.
Please wait ...

```

## DHCP

### DHCP Server

S3600 SI 没有 DHCP Server，只有 EI 版本提供，狗日的 H3C(fuck h3c)

```

dhcp enable
dhcp server ip-pool 0
static-bind ip-address 172.16.0.2
static-bind mac-address 000f-e200-0002
dns-list 172.16.0.254
gateway-list 172.16.0.254
quit

```

```

dhcp server ip-pool 0
network 10.1.1.0 mask 255.255.255.0
domain-name aabbcc.com
dns-list 10.1.1.2
nbns-list 10.1.1.4
expired day 10 hour 12
quit

```

```

3\. 配置步骤

(1)        配置端口属于 VLAN 及对应 VLAN 接口的 IP 地址（略）

(2)        配置 DHCP 服务

# 使能 DHCP 服务。

<H3C> system-view

[H3C] dhcp enable

# 配置不参与自动分配的 IP 地址（DNS 服务器、WINS 服务器和出口网关地址）。

[H3C] dhcp server forbidden-ip 10.1.1.2

[H3C] dhcp server forbidden-ip 10.1.1.4

[H3C] dhcp server forbidden-ip 10.1.1.126

[H3C] dhcp server forbidden-ip 10.1.1.254

# 配置 DHCP 地址池 0 的共有属性（地址池范围、DNS 服务器地址）。

[H3C] dhcp server ip-pool 0

[H3C-dhcp-pool-0] network 10.1.1.0 mask 255.255.255.0

[H3C-dhcp-pool-0] domain-name aabbcc.com

[H3C-dhcp-pool-0] dns-list 10.1.1.2

[H3C-dhcp-pool-0] quit

# 配置 DHCP 地址池 1 的属性（地址池范围、出口网关、地址租用期限）。

[H3C] dhcp server ip-pool 1

[H3C-dhcp-pool-1] network 10.1.1.0 mask 255.255.255.128

[H3C-dhcp-pool-1] gateway-list 10.1.1.126

[H3C-dhcp-pool-1] expired day 10 hour 12

[H3C-dhcp-pool-1] quit

# 配置 DHCP 地址池 2 的属性（地址池范围、出口网关、WINS 服务器地址、地址租用期限）。

[H3C] dhcp server ip-pool 2

[H3C-dhcp-pool-2] network 10.1.1.128 mask 255.255.255.128

[H3C-dhcp-pool-2] expired day 5

[H3C-dhcp-pool-2] nbns-list 10.1.1.4

[H3C-dhcp-pool-2] gateway-list 10.1.1.254

```

#### 排除 IP 地址

排除单个 IP 地址

```
dhcp server forbidden-ip 10.1.1.2
dhcp server forbidden-ip 10.1.1.4
dhcp server forbidden-ip 10.1.1.126
dhcp server forbidden-ip 10.1.1.254				

```

排除一段 IP 地址

```
#
 dhcp server forbidden-ip 192.168.2.200 192.168.2.254
 dhcp server forbidden-ip 192.168.3.200 192.168.3.254
 dhcp server forbidden-ip 192.168.4.1 192.168.4.10
 dhcp server forbidden-ip 192.168.4.200 192.168.4.254
 dhcp server forbidden-ip 192.168.3.1 192.168.3.10
 dhcp server forbidden-ip 192.168.5.1 192.168.5.10
 dhcp server forbidden-ip 192.168.5.200 192.168.5.254
 dhcp server forbidden-ip 192.168.6.200 192.168.6.254
 dhcp server forbidden-ip 192.168.7.1 192.168.7.10
 dhcp server forbidden-ip 192.168.7.200 192.168.7.254
 dhcp server forbidden-ip 192.168.8.1 192.168.8.10
 dhcp server forbidden-ip 192.168.8.200 192.168.8.254
 dhcp server forbidden-ip 192.168.9.1 192.168.9.10
 dhcp server forbidden-ip 192.168.9.200 192.168.9.254
 dhcp server forbidden-ip 192.168.2.1 192.168.2.30
 dhcp server forbidden-ip 192.168.6.1 192.168.6.30
#

```

### DHCP 中继配置

```

# 进入系统视图。

<H3C> system-view

# 使能 DHCP 服务。

[H3C] dhcp enable

# 配置 DHCP Server 的组号为 1，IP 地址为 202.38.1.2。

[H3C] dhcp-server 1 ip 202.38.1.2

# 配置 VLAN 接口 2 对应 DHCP Server 组 1。

[H3C] interface Vlan-interface 2

[H3C-Vlan-interface2] dhcp-server 1

# 配置 VLAN 接口 2 的 IP 地址，需与 DHCP Client 属于同一网段。

[H3C-Vlan-interface2] ip address 10.110.1.1 255.255.0.0

```

### DHCP Snooping

```

# 进入系统视图。

<H3C> system-view

# 开启交换机 DHCP-Snooping 功能。

[H3C] dhcp-snooping

# 进入 Ethernet1/0/1 端口视图。

[H3C] interface Ethernet1/0/1

# 设置端口为信任端口。

[H3C-Ethernet1/0/1] dhcp-snooping trust

```

### 查看地址池配置

#### 查看地址池配置

```

<H3C>display dhcp server tree all
Global pool:

Pool name: vlan2
 network 192.168.2.0 mask 255.255.255.0
 Sibling node:vlan3
 gateway-list 192.168.2.254 
 dns-list 211.162.78.2 8.8.8.8 
 expired 7 0 0

Pool name: vlan3
 network 192.168.3.0 mask 255.255.255.0
 PrevSibling node:vlan2
 Sibling node:vlan4
 gateway-list 192.168.3.254 
 dns-list 211.162.78.2 8.8.8.8 
 expired 1 0 0

Pool name: vlan4
 network 192.168.4.0 mask 255.255.255.0
 PrevSibling node:vlan3
 Sibling node:vlan5
 gateway-list 192.168.4.254 
 dns-list 202.96.134.133 202.96.128.68 208.67.222.222 208.67.220.220 
 expired 1 0 0

Pool name: vlan5
 network 192.168.5.0 mask 255.255.255.0
 PrevSibling node:vlan4
 Sibling node:vlan6
 gateway-list 192.168.5.254 
 dns-list 211.162.78.2 8.8.8.8 
 expired 1 0 0

Pool name: vlan6
 network 192.168.6.0 mask 255.255.255.0
 PrevSibling node:vlan5
 Sibling node:vlan7
 gateway-list 192.168.6.254 
 dns-list 202.45.84.58 203.80.96.10 8.8.8.8 
 expired 1 0 0

Pool name: vlan7
 network 192.168.7.0 mask 255.255.255.0
 PrevSibling node:vlan6
 Sibling node:vlan8
 gateway-list 192.168.7.254 
 dns-list 208.67.222.222 208.67.220.220 8.8.8.8 4.4.4.4 
 expired 1 0 0  

Pool name: vlan8
 network 192.168.8.0 mask 255.255.255.0
 PrevSibling node:vlan7
 Sibling node:vlan9
 gateway-list 192.168.8.254 
 dns-list 208.67.222.222 208.67.220.220 8.8.8.8 4.4.4.4 
 expired 1 0 0

Pool name: vlan9
 network 192.168.9.0 mask 255.255.255.0
 PrevSibling node:vlan8
 gateway-list 192.168.9.254 
 dns-list 208.67.222.222 208.67.220.220 8.8.8.8 4.4.4.4 
 expired 1 0 0
<H3C>			

```

#### 查看地址租约

```

<H3C>display dhcp server ip-in-use all
Global pool: 
 IP address       Client-identifier/    Lease expiration          Type
                  Hardware address                                
 192.168.2.39     201a-065b-c488        Jun  3 2000 15:42:50      Auto:COMMITTED
 192.168.2.51     78e3-b590-bed0        Jun  3 2000 07:37:22      Auto:COMMITTED
 192.168.2.71     001b-3899-e518        Jun  4 2000 11:32:43      Auto:COMMITTED
 192.168.3.35     6c62-6da6-8aa3        May 30 2000 06:44:22      Auto:COMMITTED
 192.168.3.33     902b-340b-5b38        May 30 2000 07:45:48      Auto:COMMITTED
 192.168.2.31     001e-ec64-9b46        Jun  3 2000 11:17:01      Auto:COMMITTED
 192.168.2.72     78e3-b5ab-2891        Jun  5 2000 06:57:37      Auto:COMMITTED
 192.168.2.63     50e5-4919-15a9        Jun  3 2000 07:45:42      Auto:COMMITTED
 192.168.6.42     80c1-6edf-b997        May 30 2000 10:49:35      Auto:COMMITTED
 192.168.3.41     5254-0054-dd26        May 29 2000 14:41:22      Auto:COMMITTED
 192.168.2.46     6c62-6dbf-1ea8        Jun  5 2000 11:30:58      Auto:COMMITTED
 192.168.2.53     902b-340b-669f        Jun  3 2000 07:39:01      Auto:COMMITTED
 192.168.2.32     78e3-b590-cc1a        Jun  5 2000 06:44:34      Auto:COMMITTED
 192.168.3.23     80c1-6edf-b844        May 30 2000 10:48:46      Auto:COMMITTED
 192.168.3.30     6c62-6dd2-f8ea        May 30 2000 06:45:07      Auto:COMMITTED
 192.168.3.42     902b-340b-66ab        May 30 2000 08:45:11      Auto:COMMITTED
 192.168.3.40     5254-0032-6e05        May 29 2000 12:03:14      Auto:COMMITTED
 192.168.2.77     78e3-b5ab-5cdf        Jun  5 2000 07:48:11      Auto:COMMITTED
 192.168.2.50     78e3-b596-c42c        Jun  5 2000 06:42:09      Auto:COMMITTED
 192.168.2.73     74d4-3585-ae8a        Jun  5 2000 06:50:13      Auto:COMMITTED
 192.168.2.34     78e3-b5ab-2ab9        Jun  5 2000 06:54:54      Auto:COMMITTED
 192.168.2.35     78e3-b5ab-59ed        Jun  3 2000 06:45:34      Auto:COMMITTED
 192.168.3.16     78e3-b59b-99c5        May 30 2000 09:13:29      Auto:COMMITTED
 192.168.2.49     6c62-6db1-aab5        Jun  5 2000 06:44:27      Auto:COMMITTED
 192.168.3.25     78e3-b5ab-2ac7        May 30 2000 07:07:10      Auto:COMMITTED
 192.168.2.62     78e3-b590-beb0        Jun  5 2000 06:48:40      Auto:COMMITTED
 192.168.3.36     74d4-3585-a743        May 30 2000 10:50:28      Auto:COMMITTED
 192.168.3.39     5254-001a-2f7d        May 29 2000 12:02:49      Auto:COMMITTED
 192.168.2.40     001d-92f0-3200        Jun  5 2000 06:40:34      Auto:COMMITTED
 192.168.2.70     78e3-b59b-818c        Jun  5 2000 06:53:36      Auto:COMMITTED
 192.168.3.29     60a4-4cad-b643        May 30 2000 10:35:06      Auto:COMMITTED
 192.168.6.41     78e3-b590-d14c        May 30 2000 06:56:14      Auto:COMMITTED
 192.168.2.58     78e3-b596-ddfc        Jun  5 2000 06:48:25      Auto:COMMITTED
 192.168.3.38     0025-90af-9e3c        May 30 2000 00:00:50      Auto:COMMITTED
 192.168.2.76     0025-90af-9e3c        Jun  4 2000 11:32:34      Auto:COMMITTED
 192.168.6.39     5254-006a-616e        May 30 2000 06:42:31      Auto:COMMITTED
 192.168.3.32     902b-3440-c54e        May 30 2000 07:46:29      Auto:COMMITTED

 --- total 37 entry ---		

```

#### 查看可分配的地址

```

<H3C> display dhcp server free-ip
IP Range from 192.168.2.78         to  192.168.2.199       
IP Range from 192.168.3.43         to  192.168.3.199       
IP Range from 192.168.4.11         to  192.168.4.199       
IP Range from 192.168.5.12         to  192.168.5.199       
IP Range from 192.168.6.43         to  192.168.6.199       
IP Range from 192.168.7.11         to  192.168.7.199       
IP Range from 192.168.8.12         to  192.168.8.199       
IP Range from 192.168.9.11         to  192.168.9.199  				

```

#### 查看租约过期地址

```

<H3C>display dhcp server expired all
Global pool: 
 IP address       Client-identifier/    Lease expiration          Type
                  Hardware address                                
 192.168.2.67     f4ec-383f-7bf9        May 17 2000 14:08:22      Release
 192.168.2.36     78e3-b590-c26a        May 29 2000 10:52:22      Release
 192.168.3.12     201a-065b-c488        May  1 2000 12:12:56      Release
 192.168.3.37     001b-3899-e518        May 29 2000 11:40:08      Release
 192.168.8.11     001e-ec64-9b46        May  2 2000 14:07:08      Release
 192.168.3.11     001e-ec64-9b46        May 18 2000 13:50:54      Release
 192.168.2.65     3c97-0ea7-e1a9        May  8 2000 12:05:43      Release
 192.168.3.24     6c62-6dbf-1f38        May  7 2000 06:51:49      Release
 192.168.6.31     5254-00f0-fb87        May 14 2000 13:24:45      Release
 192.168.2.38     78e3-b590-c7d5        May 16 2000 14:43:26      Release
 192.168.2.54     1cfa-68ee-9a15        Apr 30 2000 08:08:54      Release
 192.168.3.14     80c1-6edf-b997        Apr 28 2000 16:06:51      Release
 192.168.2.68     000c-29a4-38eb        May 16 2000 12:18:02      Release
 192.168.2.47     78e3-b59b-989f        May 13 2000 12:56:44      Release
 192.168.6.35     5254-009f-afd1        May 14 2000 14:21:53      Release
 192.168.2.45     78e3-b5ab-3ef4        May  2 2000 13:04:38      Release
 192.168.3.18     78e3-b598-f057        May 17 2000 06:45:23      Release
 192.168.6.37     5254-00e5-c31e        May 14 2000 14:25:33      Release
 192.168.3.20     78e3-b590-c82b        May 16 2000 11:22:44      Release
 192.168.3.17     d067-e527-be93        May  9 2000 10:38:25      Release
 192.168.2.56     000c-29c9-b264        Apr 30 2000 13:09:15      Release
 192.168.2.44     78e3-b596-dddd        May 24 2000 13:37:01      Release
 192.168.2.69     78e3-b596-de80        May 17 2000 12:22:29      Release
 192.168.3.34     984b-e1a9-7167        May  3 2000 07:14:17      Release

 --- total 54 entry ---				

```

#### 查看冲突 IP 地址

```

<H3C>display dhcp server conflict all
    Address             Discover time                 
    192.168.2.57        Apr 30 2000 15:12:40          
    192.168.2.75        May 20 2000 14:39:46          
 --- total 2 entry ---				

```

## VLAN

### GVRP

```

l              配置 Switch A：

# 开启全局 GVRP。

<H3C> system-view

[H3C] gvrp

GVRP is enabled globally.

# 将以太网端口 Ethernet1/0/1 配置为 Trunk 端口，并允许所有 VLAN 通过。

[H3C] interface Ethernet 1/0/1

[H3C-Ethernet1/0/1] port link-type trunk

[H3C-Ethernet1/0/1] port trunk permit vlan all

# 在 Trunk 端口上开启 GVRP。

[H3C-Ethernet1/0/1] gvrp

GVRP is enabled on port Ethernet1/0/1.

l              配置 Switch B：

# 开启全局 GVRP。

<H3C> system-view

[H3C] gvrp

GVRP is enabled globally.

# 将以太网端口 Ethernet1/0/2 配置为 Trunk 端口，并允许所有 VLAN 通过。

[H3C] interface Ethernet 1/0/2

[H3C-Ethernet1/0/2] port link-type trunk

[H3C-Ethernet1/0/2] port trunk permit vlan all

# 在 Trunk 端口上开启 GVRP。

[H3C-Ethernet1/0/2] gvrp

GVRP is enabled on port Ethernet1/0/2.

```

## ARP

```

[H3C]display arp 192.168.3.14
            Type: S-Static   D-Dynamic
IP Address       MAC Address     VLAN ID  Port Name / AL ID      Aging     Type
192.168.3.14     0022-1969-c981  1        Ethernet1/0/1          20        D

```

## 流量控制

### 基于接口

cir 它必须是 64 的倍数

qos lr outbound cir 64 64Kbps

qos lr outbound cir 1024 1Mbps

```
[H3C-GigabitEthernet1/0/18]di this
#
interface GigabitEthernet1/0/18
 port access vlan 9
 qos lr outbound cir 64 cbs 4000
 qos apply policy 20 inbound
 dhcp-snooping trust
#
return

```

察看状态

```
display qos lr interface GigabitEthernet 1/0/18

```

### 基于 ACL

```
acl number 3009
rule 0 permit ip source 192.168.9.0 0.0.0.255

traffic classifier traffic-limit
if-match acl 3009

traffic behavior traffic-limit
car cir 1024

qos policy traffic-limit
classifier traffic-limit behavior traffic-limit

interface GigabitEthernet1/0/18
qos apply policy traffic-limit inbound

```

```
[H3C]acl number 3009
[H3C-acl-adv-3009]rule 0 permit ip source 192.168.9.0 0.0.0.255
[H3C-acl-adv-3009]
[H3C-acl-adv-3009]traffic classifier traffic-limit
[H3C-classifier-traffic-limit]if-match acl 3009
[H3C-classifier-traffic-limit]
[H3C-classifier-traffic-limit]traffic behavior traffic-limit
[H3C-behavior-traffic-limit]car cir 1024
Warning: CBS is smaller than (100/16)CIR, and this maybe effect network traffic burst.
[H3C-behavior-traffic-limit]
[H3C-behavior-traffic-limit]qos policy traffic-limit
[H3C-qospolicy-traffic-limit]classifier traffic-limit behavior traffic-limit
[H3C-qospolicy-traffic-limit]
[H3C-qospolicy-traffic-limit]interface GigabitEthernet1/0/18
[H3C-GigabitEthernet1/0/18]qos apply policy traffic-limit inbound

```

```
display traffic behavior user-defined traffic-limit

[H3C-GigabitEthernet1/0/18]display traffic behavior user-defined traffic-limit
  User Defined Behavior Information:
    Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass

[H3C-GigabitEthernet1/0/18]display traffic behavior user-defined
  User Defined Behavior Information:
    Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass
    Behavior: 3
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.30.254
    Behavior: 2
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.20.1
    Behavior: 1
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.40.254

```

```
[H3C-GigabitEthernet1/0/18]display qos policy user-defined

  User Defined QoS Policy Information:

  Policy: 20
   Classifier: 20
     Behavior: 2
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.20.1

  Policy: traffic-limit
   Classifier: traffic-limit
     Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass

  Policy: 2
   Classifier: 2
     Behavior: 1
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.40.254

  Policy: 1
   Classifier: 1
     Behavior: 1
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.40.254

[H3C-GigabitEthernet1/0/18]display qos policy user-defined traffic-limit

  User Defined QoS Policy Information:

  Policy: traffic-limit
   Classifier: traffic-limit
     Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass

```

```
[H3C-GigabitEthernet1/0/18]display qos policy interface

  Interface: GigabitEthernet1/0/3

  Direction: Inbound

  Policy: 2
   Classifier: 2
     Operator: OR
     Rule(s) : If-match acl 3000
     Behavior: 1
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.40.254

  Interface: GigabitEthernet1/0/12

  Direction: Inbound

  Policy: 1
   Classifier: 1
     Operator: AND
     Rule(s) : If-match acl 3001
     Behavior: 1
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.40.254

  Interface: GigabitEthernet1/0/17

  Direction: Inbound

  Policy: 20
   Classifier: 20
     Operator: OR
     Rule(s) : If-match acl 3020
     Behavior: 2
      Redirect enable:
        Redirect type: next-hop
        Redirect destination:
          192.168.20.1

  Interface: GigabitEthernet1/0/18

  Direction: Inbound

  Policy: traffic-limit
   Classifier: traffic-limit
     Operator: AND
     Rule(s) : If-match acl 3009
     Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass
        Green : 230(Packets)
[H3C-GigabitEthernet1/0/18]display qos policy interface GigabitEthernet1/0/18

  Interface: GigabitEthernet1/0/18

  Direction: Inbound

  Policy: traffic-limit
   Classifier: traffic-limit
     Operator: AND
     Rule(s) : If-match acl 3009
     Behavior: traffic-limit
      Committed Access Rate:
        CIR 1024 (kbps), CBS 4000 (byte), EBS 4000 (byte)
        Green Action: pass
        Red Action: discard
        Yellow Action: pass
        Green : 236(Packets)

```

## Routing

### RIP

```
rip 1
 version 2
 network 192.168.20.0
 network 192.168.2.0

```

### OSPF

```
#
ospf 1
 area 0.0.0.1
  network 192.168.40.240 0.0.0.15
  network 192.168.2.0 0.0.0.255
  network 192.168.3.0 0.0.0.255
  network 192.168.4.0 0.0.0.255
  network 192.168.5.0 0.0.0.255
  network 192.168.6.0 0.0.0.255
  network 192.168.7.0 0.0.0.255
  network 192.168.8.0 0.0.0.255
  network 192.168.9.0 0.0.0.255
  network 172.16.0.0 0.0.0.255
  network 192.168.30.0 0.0.0.255
  network 172.16.100.0 0.0.0.255
#

```

### Static

```
#
 ip route-static 0.0.0.0 0.0.0.0 192.168.1.1 preference 10 description TC
 ip route-static 0.0.0.0 0.0.0.0 192.168.20.1 preference 20 description UC
 ip route-static 4.4.4.4 255.255.255.255 192.168.40.254 description google dns
 ip route-static 8.8.8.8 255.255.255.255 192.168.40.254 description google dns
 ip route-static 64.4.61.215 255.255.255.255 192.168.40.254
 ip route-static 74.125.71.94 255.255.255.255 192.168.40.254
 ip route-static 192.168.20.0 255.255.255.0 192.168.20.1 description UC
 ip route-static 192.168.30.112 255.255.255.255 192.168.30.254
 ip route-static 203.208.46.146 255.255.255.255 192.168.40.254
#

```

浮动路由

```
 ip route-static 0.0.0.0 0.0.0.0 192.168.1.1 preference 10 description TC
 ip route-static 0.0.0.0 0.0.0.0 192.168.20.1 preference 20 description UC

```

### 策略路由

案例一

```
acl number 3000
 rule 0 permit ip source 192.168.2.1 0
acl number 3001
 rule 0 permit ip source 192.168.6.0 0.0.0.255
#
traffic classifier 2 operator or
 if-match acl 3000
traffic classifier 1 operator and
 if-match acl 3001
#
traffic behavior 1
 redirect next-hop 192.168.40.254
#
qos policy 2
 classifier 2 behavior 1
qos policy 1
 classifier 1 behavior 1
#
interface GigabitEthernet1/0/11
 qos apply policy 1 inbound
#
interface GigabitEthernet1/0/12
 qos apply policy 2 inbound

```

取消策略路由

```
interface GigabitEthernet1/0/11
un qos apply policy inbound
#
interface GigabitEthernet1/0/12
un qos apply policy inbound

quit
un qos policy 1
un traffic behavior 1
un traffic classifier 1

un acl number 3001

```

这个案例是基于源的策略路由

案例二

```
acl number 3010
 rule 0 permit ip source any destination 192.168.0.0 0.0.255.255

acl number 3020
 rule 0 permit ip source 192.168.6.0 0.0.0.255

traffic classifier classifier1 operator or
 if-match acl 3010
traffic classifier classifier2 operator or
 if-match acl 3020

traffic behavior behavior1
 redirect next-hop 192.168.1.1

traffic behavior behavior2
 redirect next-hop 192.168.40.254

qos policy policy1
 classifier classifier1 behavior behavior1
 classifier classifier2 behavior behavior2

interface GigabitEthernet1/0/11
 qos apply policy policy1 inbound

```

案例二是一个基于目的的测略路由

### Debug

#### routing-table

```

[H3C]display ip routing-table
 Routing Table: public net
Destination/Mask   Protocol Pre  Cost        Nexthop         Interface
0.0.0.0/0          STATIC   60   0           192.168.3.1     Vlan-interface1
127.0.0.0/8        DIRECT   0    0           127.0.0.1       InLoopBack0
127.0.0.1/32       DIRECT   0    0           127.0.0.1       InLoopBack0
192.168.3.0/24     DIRECT   0    0           192.168.3.12    Vlan-interface1
192.168.3.12/32    DIRECT   0    0           127.0.0.1       InLoopBack0
192.168.5.0/24     STATIC   60   0           192.168.3.252   Vlan-interface1
192.168.6.0/24     DIRECT   0    0           192.168.6.254   Vlan-interface6
192.168.6.254/32   DIRECT   0    0           127.0.0.1       InLoopBack0
192.168.7.0/24     DIRECT   0    0           192.168.7.254   Vlan-interface7
192.168.7.254/32   DIRECT   0    0           127.0.0.1       InLoopBack0
192.168.8.0/24     DIRECT   0    0           192.168.8.254   Vlan-interface8
192.168.8.254/32   DIRECT   0    0           127.0.0.1       InLoopBack0
192.168.9.0/24     DIRECT   0    0           192.168.9.254   Vlan-interface9
192.168.9.254/32   DIRECT   0    0           127.0.0.1       InLoopBack0

```

```
[H3C-rip-1]display ip routing-table
Routing Tables: Public
	Destinations : 36	Routes : 36

Destination/Mask    Proto  Pre  Cost         NextHop         Interface

0.0.0.0/0           Static 60   0            192.168.1.1     Vlan1
1.1.1.1/32          Direct 0    0            127.0.0.1       InLoop0
4.4.4.4/32          Static 60   0            192.168.40.254  Vlan40
8.8.8.8/32          Static 60   0            192.168.40.254  Vlan40
64.4.61.215/32      Static 60   0            192.168.40.254  Vlan40
74.125.71.94/32     Static 60   0            192.168.40.254  Vlan40
127.0.0.0/8         Direct 0    0            127.0.0.1       InLoop0
127.0.0.1/32        Direct 0    0            127.0.0.1       InLoop0
172.16.0.0/24       OSPF   10   2            192.168.40.254  Vlan40
192.168.1.0/24      Direct 0    0            192.168.1.254   Vlan1
192.168.1.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.2.0/24      Direct 0    0            192.168.2.254   Vlan2
192.168.2.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.3.0/24      Direct 0    0            192.168.3.254   Vlan3
192.168.3.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.4.0/24      Direct 0    0            192.168.4.254   Vlan4
192.168.4.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.5.0/24      Direct 0    0            192.168.5.254   Vlan5
192.168.5.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.6.0/24      Direct 0    0            192.168.6.254   Vlan6
192.168.6.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.7.0/24      Direct 0    0            192.168.7.254   Vlan7
192.168.7.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.8.0/24      Direct 0    0            192.168.8.254   Vlan8
192.168.8.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.9.0/24      Direct 0    0            192.168.9.254   Vlan9
192.168.9.254/32    Direct 0    0            127.0.0.1       InLoop0
192.168.20.0/24     Direct 0    0            192.168.20.254  Vlan20
192.168.20.254/32   Direct 0    0            127.0.0.1       InLoop0
192.168.30.0/24     Direct 0    0            192.168.30.1    Vlan30
192.168.30.1/32     Direct 0    0            127.0.0.1       InLoop0
192.168.30.112/32   Static 60   0            192.168.30.254  Vlan30
192.168.40.240/28   Direct 0    0            192.168.40.253  Vlan40
192.168.40.253/32   Direct 0    0            127.0.0.1       InLoop0
192.168.50.128/25   OSPF   10   2            192.168.40.254  Vlan40
203.208.46.146/32   Static 60   0            192.168.40.254  Vlan40

```

## SNMP

```

<H3C>system-view
System View: return to User View with Ctrl+Z.
[H3C]snmp-agent
[H3C]snmp-agent sys-info version all
[H3C]snmp-agent community write public
[H3C]snmp-agent mib-view include internet 1.3.6.1

```

```
[root@nis ~]# snmpwalk -c public -v 1 172.16.0.253|more
SNMPv2-MIB::sysDescr.0 = STRING: H3C Comware Platform Software
Comware software, Version 3.10, Release 1702P13
H3C S3600-28P-SI Product Version S3600-SI-1702P13

Copyright(c) 2004-2010 Hangzhou H3C Tech. Co.,Ltd. All rights reserved.

SNMPv2-MIB::sysObjectID.0 = OID: SNMPv2-SMI::enterprises.25506.1.32
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (18165571) 2 days, 2:27:35.71
SNMPv2-MIB::sysContact.0 = STRING: Hangzhou H3C Technologies Co., Ltd.
SNMPv2-MIB::sysName.0 = STRING: H3C
SNMPv2-MIB::sysLocation.0 = STRING: Hangzhou China
SNMPv2-MIB::sysServices.0 = INTEGER: 78
IF-MIB::ifNumber.0 = INTEGER: 36
IF-MIB::ifIndex.14 = INTEGER: 14
IF-MIB::ifIndex.16 = INTEGER: 16
IF-MIB::ifIndex.31 = INTEGER: 31
IF-MIB::ifIndex.71 = INTEGER: 71
IF-MIB::ifIndex.79 = INTEGER: 79
IF-MIB::ifIndex.87 = INTEGER: 87
IF-MIB::ifIndex.95 = INTEGER: 95
IF-MIB::ifIndex.4227614 = INTEGER: 4227614
IF-MIB::ifIndex.4227626 = INTEGER: 4227626
IF-MIB::ifIndex.4227634 = INTEGER: 4227634

```

## Login

### Telnet

Telnet

```

local-user h3c
 password cipher OUM!K%F<+$[Q=^Q`MAF4<1!!
 service-type telnet

user-interface vty 0 4
 authentication-mode scheme
 user privilege level 3
 protocol inbound telnet

```

### SSH

SSH

```
public-key local create rsa
ssh server enable

local-user admin
password cipher your_password
service-type ssh
authorization-attribute level 3

user-interface vty 0 4
authentication-mode scheme
protocol inbound ssh

ssh user admin service-type stelnet authentication-type password
user privilege level 3
ssh client source interface loopback0

```

## Example

例 21.1. dhcp vlan rip

```

[H3C]display current-configuration
#
 sysname H3C
#
 super password level 3 cipher D8Za-\BIMY/Q=^Q`MAF4<1!!
#
radius scheme system
#
domain system
#
local-user lfc
 password cipher D8Za-\BIMY/Q=^Q`MAF4<1!!
 service-type telnet terminal
local-user neo
 password cipher LM,[26IJZUGQ=^Q`MAF4<1!!
 service-type telnet terminal
#
dhcp server ip-pool vlan6
 network 192.168.6.0 mask 255.255.255.0
 gateway-list 192.168.6.254
 dns-list 208.67.222.222 208.67.220.220 202.96.134.133 202.96.128.68
 expired day 7
#
dhcp server ip-pool vlan7
 network 192.168.7.0 mask 255.255.255.0
 gateway-list 192.168.7.254
 dns-list 208.67.222.222 208.67.220.220 202.96.134.133 202.96.128.68
 expired day 7 hour 1
#
vlan 1 to 8
#
interface Vlan-interface1
 ip address 192.168.3.17 255.255.255.0
#
interface Vlan-interface6
 ip address 192.168.6.254 255.255.255.0
#
interface Vlan-interface7
 ip address 192.168.7.254 255.255.255.0
#
interface Aux1/0/0
#
interface Ethernet1/0/1
 port link-type trunk
 port trunk permit vlan 1 to 8
#
interface Ethernet1/0/2
#
interface Ethernet1/0/3
#
interface Ethernet1/0/4
#
interface Ethernet1/0/5
#
interface Ethernet1/0/6
#
interface Ethernet1/0/7
#
interface Ethernet1/0/8
#
interface Ethernet1/0/9
#
interface Ethernet1/0/10
#
interface Ethernet1/0/11
#
interface Ethernet1/0/12
#
interface Ethernet1/0/13
#
interface Ethernet1/0/14
#
interface Ethernet1/0/15
#
interface Ethernet1/0/16
#
interface Ethernet1/0/17
#
interface Ethernet1/0/18
#
interface Ethernet1/0/19
#
interface Ethernet1/0/20
#
interface Ethernet1/0/21
#
interface Ethernet1/0/22
#
interface Ethernet1/0/23
 port access vlan 6
 dhcp-snooping trust
#
interface Ethernet1/0/24
 port access vlan 7
 dhcp-snooping trust
#
interface GigabitEthernet1/1/1
#
interface GigabitEthernet1/1/2
#
interface GigabitEthernet1/1/3
#
interface GigabitEthernet1/1/4
#
 undo irf-fabric authentication-mode
#
interface NULL0
#
rip
 network 192.168.3.0
 network 192.168.6.0
 network 192.168.7.0
#
 dhcp-snooping
#
 voice vlan mac-address 0001-e300-0000 mask ffff-ff00-0000
#
 dhcp server forbidden-ip 192.168.6.1 192.168.6.10
 dhcp server forbidden-ip 192.168.7.1 192.168.7.10
#
 ip route-static 0.0.0.0 0.0.0.0 192.168.3.1 preference 60
#
user-interface aux 0 7
user-interface vty 0 4
 authentication-mode scheme
#
return
[H3C]

```

## 第 22 章 H3C WA2220E-AG

默认参数

```
IP：192.168.0.50
User: admin
Password: h3capadmin
SSID: H3C
HTTP：enable

```

## 用户界面

```

[WA2220E-AG]display user-interface
  Idx  Type     Tx/Rx      Modem Privi Auth  Int
+ 0    CON 0    9600       -     3     N     -
  1    VTY 0               -     0     A     -
  2    VTY 1               -     0     A     -
  3    VTY 2               -     0     A     -
  4    VTY 3               -     0     A     -
  5    VTY 4               -     0     A     -
  +    : Current UI is active.
  F    : Current UI is active and work in async mode.
  Idx  : Absolute index of UIs.
  Type : Type and relative index of UIs.
  Privi: The privilege of UIs.
  Auth : The authentication mode of UIs.
  Int  : The physical location of UIs.
  A    : Authentication use AAA.
  L    : Authentication use local database.
  N    : Current UI need not authentication.
  P    : Authentication use current UI's password.

```

### Console

```

system-view
user-interface aux 0
authentication-mode password
set authentication password simple chen

```

### 启用 HTTP

status

```

[WA2220E-AG]display ip http
HTTP port: 80
WLAN ACL: 0
Basic ACL: 0
Current connection: 0
Operation status: Running

```

enable

```

<WA2220E-AG>system-view
[WA2220E-AG]ip http enable
Info: HTTP server has been started!

```

disable

```

[WA2220E-AG]undo ip http enable
[WA2220E-AG]
%Apr 26 12:28:31:522 2000 WA2220E-AG HTTPD/4/Log:Stop HTTP server.

```

### Telnet

启动

```

<WA2220E-AG>system-view
System View: return to User View with Ctrl+Z.
[WA2220E-AG]telnet server enable
% Start Telnet server

[WA2220E-AG]telnet server enable
% Telnet server has been started

```

关闭

```

[WA2220E-AG]telnet server enable
% Start Telnet server

```

设置密码

```

[WA2220E-AG]user-interface vty 0 4
[WA2220E-AG-ui-vty0-4]authentication-mode password
[WA2220E-AG-ui-vty0-4]set authentication password cipher your_password
[WA2220E-AG-ui-vty0-4]user privilege level 2
[WA2220E-AG-ui-vty0-4]quit

```

## 用户认证

```
[H3C]local-user neo
New local user added.
[H3C-luser-neo]password cipher chen
[H3C-luser-neo]service-type telnet terminal
[H3C-luser-neo]

```

## FAT/FIT AP

fat

```

<WA2220E-AG>ap-mode fat
 Current working mode is already FAT

```

fit

```

<WA2220E-AG>ap-mode fit
 Change working mode will reboot system, do you want to continue? [Y/N]:y
 Change working mode to FIT now? [Y/N]:y

```

### 异常处理

```

system start booting......Version  1.31

      *******************************************************
      *                                                     *
      *          H3C WA2200 BootWare , Ver 1.31             *
      *                                                     *
      *******************************************************
 Copyright(c) 2004-2009 Hangzhou H3C Tech. Co., Ltd. and its licensors.
 Compiled date: Mar  4 2009, 19:30:07
 CPU type        : PPC405EP
 CPU L1 Cache    : 16KB
 CPU Clock Speed : 266MHz
 Memory Type     : SDRAM
 Memory Size     : 64MB
 Memory Speed    : 133MHz
 BootWare Size   : 512KB
 Flash  Size     : 8MB

 HardWare Version is Ver.B

  Application program does not exist.

Press Ctrl+B to enter extended boot menu...
  Please input BootWare password:
===================<EXTEND-BOOTWARE MENU>====================
| <1> Boot System                                           |
| <2> Enter Serial SubMenu                                  |
| <3> Enter Ethernet SubMenu                                |
| <4> File Control                                          |
| <5> Modify BootWare Password                              |
| <6> Ignore System Configuration                           |
| <7> BootWare Operation Menu                               |
| <8> Clear Super Password                                  |
| <9> Device Operation                                      |
| <0> Reboot                                                |
=============================================================
Enter your choice(0-9):4

========================<File CONTROL>=======================
|Note:the operating device is Flash                         |
| <1> Display All File(s)                                   |
| <2> Delete File                                           |
| <0> Exit To Main Menu                                     |
=============================================================
Enter your choice(0-3):1

Display All File In flash:
****************************************************************************
 NO.    Size(B)                Time       Name
  0     6285856   Oct-10-2002 10:10    flash:/wa2200_fat.bin
  1         388   Apr-26-2000 13:52    flash:/private-data.txt
  2       12149   Apr-26-2000 13:52    flash:/config.cwmp
  3         470   Apr-26-2000 13:52    flash:/system.xml
****************************************************************************

========================<File CONTROL>=======================
|Note:the operating device is Flash                         |
| <1> Display All File(s)                                   |
| <2> Delete File                                           |
| <0> Exit To Main Menu                                     |
=============================================================
Enter your choice(0-3):0

===================<EXTEND-BOOTWARE MENU>====================
| <1> Boot System                                           |
| <2> Enter Serial SubMenu                                  |
| <3> Enter Ethernet SubMenu                                |
| <4> File Control                                          |
| <5> Modify BootWare Password                              |
| <6> Ignore System Configuration                           |
| <7> BootWare Operation Menu                               |
| <8> Clear Super Password                                  |
| <9> Device Operation                                      |
| <0> Reboot                                                |
=============================================================
Enter your choice(0-9):9

========================<Device CONTROL>=====================
| <1> Display Current Device Model                          |
| <2> Set The Device Into FIT AP Model                      |
| <3> Set The Device Into FAT AP Model                      |
| <0> Exit To Main Menu                                     |
=============================================================
Enter your choice(0-3):3
........
FAT AP Mode setting successfully.

========================<Device CONTROL>=====================
| <1> Display Current Device Model                          |
| <2> Set The Device Into FIT AP Model                      |
| <3> Set The Device Into FAT AP Model                      |
| <0> Exit To Main Menu                                     |
=============================================================
Enter your choice(0-3):0

===================<EXTEND-BOOTWARE MENU>====================
| <1> Boot System                                           |
| <2> Enter Serial SubMenu                                  |
| <3> Enter Ethernet SubMenu                                |
| <4> File Control                                          |
| <5> Modify BootWare Password                              |
| <6> Ignore System Configuration                           |
| <7> BootWare Operation Menu                               |
| <8> Clear Super Password                                  |
| <9> Device Operation                                      |
| <0> Reboot                                                |
=============================================================
Enter your choice(0-9):0

```

## IP Address

```

<WA2220E-AG>system-view
System View: return to User View with Ctrl+Z.
[WA2220E-AG]interface vlan-interface 1
[WA2220E-AG-Vlan-interface1]ip address 172.16.1.1 24
[WA2220E-AG-Vlan-interface1]quit

```

查看 IP 地址

```

[WA2220E-AG]display ip interface
Vlan-interface1 current state :UP
Line protocol current state :UP
Internet Address is 172.16.1.1/24 Primary
Broadcast address : 172.16.1.255
The Maximum Transmit Unit : 1500 bytes
input packets : 476, bytes : 56024, multicasts : 0
output packets : 439, bytes : 204306, multicasts : 0
ARP packet input number:          12
  Request packet:                 11
  Reply packet:                    1
  Unknown packet:                  0
TTL invalid packet number:         0
ICMP packet input number:          3
  Echo reply:                      0
  Unreachable:                     0
  Source quench:                   0
  Routing redirect:                0
  Echo request:                    3
  Router advert:                   0
  Router solicit:                  0
  Time exceed:                     0
  IP header bad:                   0
  Timestamp request:               0
  Timestamp reply:                 0
  Information request:             0
  Information reply:               0
  Netmask request:                 0
  Netmask reply:                   0
  Unknown type:                    0

[WA2220E-AG]

```

## SSID

禁用 WLAN-BSS1

```

[WA2220E-AG]interface WLAN-BSS1
[WA2220E-AG-WLAN-BSS1]wlan service-template 1 clear
[WA2220E-AG-wlan-st-1]service-template disable

interface WLAN-BSS1
wlan service-template 1 clear
service-template disable

```

新建 WLAN-BSS2

```

[WA2220E-AG]interface WLAN-BSS2
[WA2220E-AG-WLAN-BSS2]wlan service-template 2 clear
[WA2220E-AG-wlan-st-2]ssid www.example.com
[WA2220E-AG-wlan-st-2]authentication-method open-system
[WA2220E-AG-wlan-st-2]service-template enable

interface WLAN-BSS2
wlan service-template 2 clear
ssid www.example.com
authentication-method open-system
service-template enable

```

应用模板

```
[WA2220E-AG]interface WLAN-Radio 1/0/2
[WA2220E-AG-WLAN-Radio1/0/2]service-template 2 interface WLAN-BSS 2
[WA2220E-AG-WLAN-Radio1/0/2]quit

interface WLAN-Radio 1/0/2
service-template 2 interface WLAN-BSS 2
quit

```

## 用户验证

### Telnet

```
local-user neo
 password simple chen
 service-type telnet

```

### WEP

WEP 加密

```

<WA2220E-AG>system-view
System View: return to User View with Ctrl+Z.
[WA2220E-AG]interface WLAN-BSS2
[WA2220E-AG-WLAN-BSS2]wlan service-template 2 crypto
[WA2220E-AG-wlan-st-2]ssid www.example.com
[WA2220E-AG-wlan-st-2]authentication-method open-system
[WA2220E-AG-wlan-st-2]cipher-suite wep40
[WA2220E-AG-wlan-st-2]wep default-key 1 wep40 pass-phrase 123456
[WA2220E-AG-wlan-st-2]service-template enable

[WA2220E-AG]interface WLAN-Radio 1/0/2
[WA2220E-AG-WLAN-Radio1/0/2]service-template 2 interface WLAN-BSS 2
[WA2220E-AG-WLAN-Radio1/0/2]quit

```

interface WLAN-BSS2
wlan service-template 2 crypto
ssid www.example.com
authentication-method open-system
cipher-suite wep40
wep default-key 1 wep40 pass-phrase 12345
service-template enable

interface WLAN-Radio 1/0/2
service-template 2 interface WLAN-BSS 2

### WAP2

```
wlan service-template 2 crypto
 ssid www.example.com
 cipher-suite ccmp
 security-ie rsn
 service-template enable

interface WLAN-BSS2
 port-security port-mode psk
 port-security tx-key-type 11key
 port-security preshared-key pass-phrase simple your_password

 interface WLAN-Radio1/0/2
 service-template 2 interface wlan-bss 2
#

```

### WAP2 cipher

```
 wlan service-template 3 crypto
 ssid www.example.com
 cipher-suite ccmp
 security-ie rsn
 service-template enable

interface WLAN-BSS3
 port link-type hybrid
 port hybrid vlan 1 untagged
 port-security port-mode psk
 port-security tx-key-type 11key
 port-security preshared-key pass-phrase cipher wrWR2LZofLw4qvbcs+daVw==
#
interface WLAN-Radio1/0/2
 service-template 3 interface wlan-bss 3
#

```

### Mac

```
配置 radius scheme：
radius scheme mac-radius
primary authentication 192.168.30.3
primary accounting 192.168.30.3
secondary authentication 192.168.30.4
secondary accounting 192.168.30.4
key authentication 123456
key accounting 123456
user-name-format without-domain

2、配置 MAC 认证的域：
domain mac-dom
authentication default radius-scheme mac-radius
authorization default radius-scheme mac-radius
accounting default radius-scheme mac-radius
access-limit disable
state active
idle-cut disable
self-service-url disable

3、配置全局 MAC 认证：
port-security enable
mac-authentication domain mac-dom

4、开启无线端口的 MAC 认证
[AP01]  int WLAN-BSS 1
[AP01-WLAN-BSS1]   port-security port-mode mac-authentication
[AP01]  int WLAN-BSS 2
[AP01-WLAN-BSS2]   port-security port-mode mac-authentication

```

## WLAN

```
[WA2220E-AG-wlan-st-2]display wlan client
 Total Number of Clients           : 2
 Total Number of Clients Connected : 2
                              Client Information
-------------------------------------------------------------------------------
 MAC Address        BSSID              AID    State           PS Mode  QoS Mode
-------------------------------------------------------------------------------
 0012-7b42-8bd2     0023-898b-10d1     127    Running         Active   None
 d85d-4c7d-7cbb     0023-898b-10d1     126    Running         Active   WMM
-------------------------------------------------------------------------------

```

### 用户互通与隔离

```

<WA2220E-AG>system-view
System View: return to User View with Ctrl+Z.
[WA2220E-AG]l2fw wlan-client-isolation enable
% Info: wlan-client-isolation is enabled.

```

```

<WA2220E-AG>system-view
System View: return to User View with Ctrl+Z.
[WA2220E-AG]undo l2fw wlan-client-isolation enable
% Info: wlan-client-isolation is disabled.

```

## DHCP

H3C WA2220E-AG 这款不支持 DHCP

## 第 23 章 H3C ICG（Information Communication Gateway）

## version

```
[Netkiller]display version
H3C Comware Platform Software
Comware Software, Version 5.20, ESS 1710
Copyright (c) 2004-2008 Hangzhou H3C Tech. Co., Ltd. All rights reserved.
H3C ICG2000 uptime is 0 week, 0 day, 22 hours, 27 minutes
Last reboot 2007/01/01 00:00:09
System returned to ROM By Power-up.

CPU type: FREESCALE MPC8323E 333MHz
256M bytes DDR SDRAM Memory
32M bytes Flash Memory
Pcb               Version:  3.0
Logic             Version:  1.0
Basic    BootROM  Version:  2.03
Extended BootROM  Version:  2.03
[SLOT  0]AUX            (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  0]ETH0/0         (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  0]ETH0/1         (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  0]ETH0/2         (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  0]ETH0/3         (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  0]ETH0/4         (Hardware)3.0,	(Driver)1.0,	(Cpld)1.0
[SLOT  2]WLAN           (Hardware)2.0,	(Driver)1.0,	(Cpld)0.0

```

## 基础操作

### 登陆

```

[root@localhost ~]# telnet 192.168.4.4
Trying 192.168.4.4...
Connected to 192.168.4.4.
Escape character is '^]'.

******************************************************************************
* Copyright (c) 2004-2008 Hangzhou H3C Tech. Co., Ltd. All rights reserved.  *
* Without the owner's prior written consent,                                 *
* no decompiling or reverse-engineering shall be allowed.                    *
******************************************************************************

Login authentication

Username:neo
Password:
<Netkiller>

```

### 创建用户

创建用户 neo 密码 chen，并允许 telnet 登陆

```

local-user neo
 password simple chen
 authorization-attribute level 3
 service-type ssh telnet terminal
 service-type ftp

```

### 开启 SSH 服务

```
[Netkiller]ssh server enable
Info: Enable SSH server.

```

测试登陆

```

[devops@master ~]$ ssh neo@192.168.4.4
neo@192.168.4.4's password:

******************************************************************************
* Copyright (c) 2004-2008 Hangzhou H3C Tech. Co., Ltd. All rights reserved.  *
* Without the owner's prior written consent,                                 *
* no decompiling or reverse-engineering shall be allowed.                    *
******************************************************************************

<Netkiller>

```

### 开启 FTP 服务

```
[Netkiller]ftp server enable

```

### 保存配置

```
[Netkiller]save
The current configuration will be written to the device. Are you sure? [Y/N]:y
Please input the file name(*.cfg)[flash:/startup.cfg]
(To leave the existing filename unchanged, press the enter key):
flash:/startup.cfg exists, overwrite? [Y/N]:y
 Validating file. Please wait........................
 Configuration is saved to device successfully.
[Netkiller]

```

使用 curl 命令链接 ftp 列出目录

```
[root@localhost ~]# curl -l ftp://neo:password@192.168.4.4
main.bin
2003_ca.cer
2003_server.pfx
navigator_ca.cer
navigator_local.cer
config.cwmp
system.xml
startup.cfg

```

使用 curl 查看配置文件

```
[root@localhost ~]# curl ftp://neo:chen@192.168.4.4/startup.cfg

#
 version 5.20, ESS 1710
#
 sysname Netkiller
#
 tcp syn-cookie enable
 tcp anti-naptha enable
 tcp state closing connection-number 500
 tcp state established connection-number 500
 tcp state fin-wait-1 connection-number 500
 tcp state fin-wait-2 connection-number 500
 tcp state last-ack connection-number 500
 tcp state syn-received connection-number 500
#
 ipsec cpu-backup enable
#
 domain default enable system
#
 dns resolve
 dns proxy enable
 dns server 202.96.134.133
#
 telnet server enable
#
 port-security enable
#
 mac-authentication domain system
#
vlan 1
#
domain system
 authentication lan-access local
 access-limit disable
 state active
 idle-cut disable
 self-service-url disable
#
pki domain navigator
  crl check disable
#
ike proposal 1
 encryption-algorithm 3des-cbc
 dh group5
#
ike peer navigator
 pre-shared-key cipher uqWUC2uW1wbDFIn0NObiHg==
#
ipsec proposal navigator
 encapsulation-mode transport
 esp authentication-algorithm sha1
 esp encryption-algorithm 3des
#
ipsec proposal navigator1
 esp authentication-algorithm sha1
 esp encryption-algorithm 3des
#
ipsec policy-template gateway 1
 ike-peer navigator
 proposal navigator navigator1
#
ipsec policy navigator 1 isakmp template gateway
#
dhcp server ip-pool vlan1 extended
 network ip range 172.16.1.30 192.168.1.200
 network mask 255.255.255.0
 gateway-list 172.16.1.254
 dns-list 114.114.114.114 114.114.115.115
 domain-name netkiller.github.io
#
user-group system
#
local-user neo
 password simple chen
 authorization-attribute level 3
 service-type ssh telnet terminal
 service-type ftp
#
wlan rrm
 dot11b mandatory-rate 1 2
 dot11b supported-rate 5.5 11
 dot11g mandatory-rate 1 2 5.5 11
 dot11g supported-rate 6 9 12 18 24 36 48 54
#
wlan service-template 1 crypto
 ssid http://netkiller.github.io
 cipher-suite tkip
 security-ie rsn
 service-template enable
#
wlan service-template 2 crypto
 ssid http://netkiller.sf.net
 cipher-suite ccmp
 security-ie rsn
 service-template enable
#
ssl server-policy chinanet
 pki-domain navigator
#
cwmp
 cwmp acs username netkiller password netkiller
 cwmp cpe inform interval enable
 cwmp cpe inform interval 43200
 cwmp cpe username neo password neo
#
interface Aux0
 async mode flow
 link-protocol ppp
#
interface Ethernet0/0
 port link-mode route
 nat outbound
 ip address 192.168.4.4 255.255.255.0
#
interface NULL0
#
interface Vlan-interface1
 ip address 172.16.1.254 255.255.255.0
 dhcp server apply ip-pool vlan1
#
interface Ethernet0/1
 port link-mode bridge
#
interface Ethernet0/2
 port link-mode bridge
#
interface Ethernet0/3
 port link-mode bridge
#
interface Ethernet0/4
 port link-mode bridge
#
interface WLAN-BSS0
 port-security port-mode psk
 port-security tx-key-type 11key
 port-security preshared-key pass-phrase 13113668890
#
interface WLAN-BSS1
#
interface WLAN-Radio2/0
 service-template 1 interface wlan-bss 0
#
policy-based-route Ethernet0/0 permit node 0
   if-match acl 2000
   apply output-interface Ethernet0/0
#
 ip route-static 0.0.0.0 0.0.0.0 Ethernet0/0 192.168.4.254
#
 dhcp enable
#
 ip https ssl-server-policy chinanet
 ip https enable
#
 nms primary monitor-interface Ethernet0/0
#
 load xml-configuration
#
user-interface aux 0
user-interface vty 0 4
 authentication-mode scheme
#

```

保存配置文件到 startup.cfg 文件

```
# curl ftp://neo:chen@192.168.4.4/startup.cfg > startup.cfg
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3450    0  3450    0     0   2852      0 --:--:--  0:00:01 --:--:--  2853

```

## Ethernet0/0 - Wan 口配置

### DHCP 动态 IP 地址

```

<Netkiller>system-view
System View: return to User View with Ctrl+Z.

interface Ethernet0/0
 port link-mode route
 nat outbound
 ip address dhcp-alloc
 ipsec policy navigator
#

[Netkiller]display dhcp client
Ethernet0/0 DHCP client information:
 Current machine state: BOUND
 Allocated IP: 172.16.0.17 255.255.255.0
 Allocated lease: 172800 seconds, T1: 86400 seconds, T2: 151200 seconds
 DHCP server: 172.16.0.254

[Netkiller]display dhcp client verbose
Ethernet0/0 DHCP client information:
 Current machine state: BOUND
 Allocated IP: 172.16.0.17 255.255.255.0
 Allocated lease: 172800 seconds, T1: 86400 seconds, T2: 151200 seconds
 Lease from 2007.01.01 00:36:08   to   2007.01.03 00:36:08
 DHCP server: 172.16.0.254
 Transaction ID: 0x5d84030b
 Default router: 172.16.0.254
 DNS server: 114.114.114.114
 DNS server: 114.114.115.115
 Client ID: 3030-3066-2e65-3263-
            642e-3733-6430-2d45-
            7468-6572-6e65-7430-
            2f30
 T1 will timeout in 0 day 23 hours 55 minutes 11 seconds.

```

### 固定 IP 地址

```
interface Ethernet0/0
 port link-mode route
 nat outbound
 ip address 192.168.4.4 255.255.255.0

```

## WLAN - 无线局域网配置

```
wlan service-template 1 crypto
 ssid http://netkiller.github.io
 cipher-suite ccmp
 security-ie rsn
 service-template enable
#
interface WLAN-BSS0
 port-security port-mode psk
 port-security tx-key-type 11key
 port-security preshared-key pass-phrase 13113668890
#
interface WLAN-Radio2/0
 service-template 1 interface wlan-bss 0
#

```

最主要的两项：

ssid http://netkiller.github.io 配置 SSID

port-security preshared-key pass-phrase 13113668890 配置密码

### 修改配置

```
[Netkiller]wlan service-template 1
[Netkiller-wlan-st-1]di this
#
wlan service-template 1 crypto
 ssid ChinaNet-73C0
 cipher-suite tkip
 cipher-suite ccmp
 security-ie rsn
 service-template enable
#
return

[Netkiller-wlan-st-1]service-template disable
[Netkiller-wlan-st-1]ssid http://netkiller.github.io
[Netkiller-wlan-st-1]service-template enable

```

注意：首先要禁用模板

### WLAN 状态查看

```
[Netkiller]display wlan client
 Total Number of Clients           : 1
 Total Number of Clients Connected : 1
		              Client Information
-------------------------------------------------------------------------------
 MAC Address        BSSID              AID    State           PS Mode  QoS Mode
-------------------------------------------------------------------------------
 b809-8a69-27ab     000f-e2cd-73c0     1      Running         Sleep    WMM
-------------------------------------------------------------------------------

[Netkiller]display wlan client
 Total Number of Clients           : 2
 Total Number of Clients Connected : 2
		              Client Information
-------------------------------------------------------------------------------
 MAC Address        BSSID              AID    State           PS Mode  QoS Mode
-------------------------------------------------------------------------------
 b809-8a69-27ab     000f-e2cd-73c0     1      Running         Sleep    WMM
 e0c9-7ab1-3d69     000f-e2cd-73c0     2      Running         Sleep    WMM
-------------------------------------------------------------------------------

```

### IDS（Intrusion detection system） 状态

```
[Netkiller]display wlan ids statistics
 Current attack tracking since: 2007-01-01/22:54:41
-------------------------------------------------------------------------------
 Type                                            Current       Total
-------------------------------------------------------------------------------
 Probe Request Frame Flood Attack                0             0
 Authentication Request Frame Flood Attack       0             0
 Deauthentication Frame Flood Attack             0             0
 Association Request Frame Flood Attack          0             0
 Disassociation Request Frame Flood Attack       0             0
 Reassociation Request Frame Flood Attack        0             0
 Action Frame Flood Attack                       0             0
 Null Data Frame Flood Attack                    0             0
 Weak IVs Detected                               0             0
 Spoofed Deauthentication Frame Attack           0             0
 Spoofed Disassociation Frame Attack             0             0
-------------------------------------------------------------------------------

```

### Radio resource management

```
[Netkiller]display wlan rrm
                                 RRM Configuration
-------------------------------------------------------------------------------
 11b Configured Rates (Mbps)
   Mandatory                   : 1, 2
   Supported                   : 5.5, 11
   Disabled                    : -NA-
 11g Configured Rates (Mbps)
   Mandatory                   : 1, 2, 5.5, 11
   Supported                   : 6, 9, 12, 18, 24, 36, 48, 54
   Disabled                    : -NA-
   11g Protection              : Disabled
 11h Configuration
   Spectrum Management         : Disabled
   Power Constraint (dBm)      : 0
   Channel Set                 : All
-------------------------------------------------------------------------------

```

### Service Template Parameters

```
[Netkiller]display wlan service-template
			  Service Template Parameters
-------------------------------------------------------------------------------
 Service Template Number      : 1
 SSID                         : http://netkiller.github.io
 Service Template Type        : Crypto
 Security IE                  : RSN
 Authentication Method        : Open System
 SSID-hide                    : Disabled
 Cipher Suite                 : TKIP
 TKIP Countermeasure Time(s)  : 60
 PTK Life Time(s)             : 43200
 GTK Rekey                    : Enabled
 GTK Rekey Method             : Time-based
 GTK Rekey Time(s)            : 86400
 Service Template Status      : Enabled
 Maximum clients per BSS      : 32
-------------------------------------------------------------------------------

			  Service Template Parameters
-------------------------------------------------------------------------------
 Service Template Number      : 2
 SSID                         : http://netkiller.sf.net
 Service Template Type        : Crypto
 Security IE                  : RSN
 Authentication Method        : Open System
 SSID-hide                    : Disabled
 Cipher Suite                 : CCMP
 TKIP Countermeasure Time(s)  : 60
 PTK Life Time(s)             : 43200
 GTK Rekey                    : Enabled
 GTK Rekey Method             : Time-based
 GTK Rekey Time(s)            : 86400
 Service Template Status      : Enabled
 Maximum clients per BSS      : 32
-------------------------------------------------------------------------------

```

### Client Statistics

```
[Netkiller]display wlan statistics client all
		               Client Statistics
-------------------------------------------------------------------------------
 AP Name                           : Netkiller
 Radio Id                          : 1
 SSID                              : http://netkiller.github.io
 BSSID                             : 000f-e2cd-73c0
 MAC Address                       : b809-8a69-27ab
 RSSI                              : 50
-------------------------------------------------------------------------------
 Transmitted Frames:
 Back Ground (Frames/Bytes)        : 0/0
 Best Effort (Frames/Bytes)        : 7156/2257549
 Video (Frames/Bytes)              : 4/700
 Voice (Frames/Bytes)              : 4/204
 Received Frames:
 Back Ground (Frames/Bytes)        : 396/77443
 Best Effort (Frames/Bytes)        : 15714/1103448
 Video (Frames/Bytes)              : 21/3937
 Voice (Frames/Bytes)              : 737/76615
 Discarded Frames:
 Back Ground (Frames/Bytes)        : 0/0
 Best Effort (Frames/Bytes)        : 0/0
 Video (Frames/Bytes)              : 0/0
 Voice (Frames/Bytes)              : 0/0
-------------------------------------------------------------------------------

		               Client Statistics
-------------------------------------------------------------------------------
 AP Name                           : Netkiller
 Radio Id                          : 1
 SSID                              : http://netkiller.github.io
 BSSID                             : 000f-e2cd-73c0
 MAC Address                       : e0c9-7ab1-3d69
 RSSI                              : 50
-------------------------------------------------------------------------------
 Transmitted Frames:
 Back Ground (Frames/Bytes)        : 0/0
 Best Effort (Frames/Bytes)        : 113/47039
 Video (Frames/Bytes)              : 2/350
 Voice (Frames/Bytes)              : 2/102
 Received Frames:
 Back Ground (Frames/Bytes)        : 0/0
 Best Effort (Frames/Bytes)        : 355/41601
 Video (Frames/Bytes)              : 0/0
 Voice (Frames/Bytes)              : 27/2733
 Discarded Frames:
 Back Ground (Frames/Bytes)        : 0/0
 Best Effort (Frames/Bytes)        : 0/0
 Video (Frames/Bytes)              : 0/0
 Voice (Frames/Bytes)              : 0/0
-------------------------------------------------------------------------------

```

### Wi-Fi multimedia

Wireless client

```
[Netkiller]display wlan wmm client all
--------------------------------------------------------------------------------
 MAC address : b809-8a69-27ab       SSID : http://netkiller.github.io
 QoS Mode        : WMM
 APSD information :
  Max SP Length : all
  L: Legacy	T: Trigger	D: Delivery
  AC		AC-BK	AC-BE	AC-VI	AC-VO
  State		L	L	L	L
  Assoc State	L	L	L	L
 CAC information :
  Uplink CAC packets   : 0           Downlink CAC packets   : 7171
  Uplink CAC bytes     : 0           Downlink CAC bytes     : 2203734
  Downgrade packets    : 0           Discard packets        : 0
  Downgrade bytes      : 0           Discard bytes          : 0
--------------------------------------------------------------------------------

--------------------------------------------------------------------------------
 MAC address : e0c9-7ab1-3d69       SSID : http://netkiller.github.io
 QoS Mode        : WMM
 APSD information :
  Max SP Length : all
  L: Legacy	T: Trigger	D: Delivery
  AC		AC-BK	AC-BE	AC-VI	AC-VO
  State         L	L	L	L
  Assoc State	L	L	L	L
 CAC information :
  Uplink CAC packets   : 0           Downlink CAC packets   : 138
  Uplink CAC bytes     : 0           Downlink CAC bytes     : 51663
  Downgrade packets    : 0           Discard packets        : 0
  Downgrade bytes      : 0           Discard bytes          : 0
--------------------------------------------------------------------------------

```

WLAN radio

```
[Netkiller]display wlan wmm radio
--------------------------------------------------------------------------------
 Radio interface : WLAN-Radio1/0/1
 ------------------------------------------------------------------------------
 Client EDCA update count : 65481828
 QoS Mode                 : WMM        Radio chip QoS mode      : WMM
 Radio chip max AIFSN     : 255        Radio chip max ECWmin    : 10
 Radio chip max TXOPLimit : 32767      Radio chip max ECWmax    : 10
 CAC Information
  Client accepted                : 0
   Voice                         : 0
   Video                         : 0
  Total request mediumtime(us)   : 0
   Voice(us)                     : 0
   Video(us)                     : 0
  Calls rejected due to insufficient resource   : 0
  Calls rejected due to invalid parameters      : 0
  Calls rejected due to invalid mediumtime      : 0
  Calls rejected due to invalid delaybound      : 0
 QoS Mode                        : WMM
 Admission Control Policy        : Users
 Threshold users count           : 20
 CAC-Free's AC Request Policy    : Response Success
 CAC Unauthed Frame Policy       : Downgrade
 CAC Medium Time Limitation(us)  : 100000
 CAC AC-VO's Max Delay(us)       : 50000
 CAC AC-VI's Max Delay(us)       : 300000
 SVP packet mapped AC number     : Disabled
 Radio's WMM Parameters:
                          AC-BK    AC-BE    AC-VI    AC-VO
       ECWmin                 4        4        3        2
       ECWmax                10        6        4        3
       AIFSN                  7        3        1        1
       TXOPLimit              0        0       94       47
       AckPolicy         Normal   Normal   Normal   Normal
 Client's WMM Parameters:
                          AC-BK    AC-BE    AC-VI    AC-VO
       ECWmin                 4        4        3        2
       ECWmax                10       10        4        3
       AIFSN                  7        3        2        2
       TXOPLimit              0        0       94       47
       CAC              Disable  Disable  Disable  Disable
-------------------------------------------------------------------------------

```

## LAN 配置

### DHCP Server

```
dhcp server ip-pool vlan1 extended
 network ip range 172.16.1.30 192.168.1.200
 network mask 255.255.255.0
 gateway-list 172.16.1.254
 dns-list 114.114.114.114 114.114.115.115
 domain-name netkiller.github.io
#

```

OpenDNS

```
[Netkiller-dhcp-pool-vlan1]di th
#
dhcp server ip-pool vlan1 extended
 network ip range 172.16.0.20 172.16.0.200
 network mask 255.255.255.0
 gateway-list 172.16.0.254
 dns-list 208.67.222.222 208.67.220.220 208.67.222.220 208.67.220.222
 domain-name netkiller.github.io
#
return

```

### VLAN

```
interface Vlan-interface1
 ip address 172.16.1.254 255.255.255.0
 dhcp server apply ip-pool vlan1
#

```

## 路由配置

### 默认路由

```
ip route-static 0.0.0.0 0.0.0.0 Ethernet0/0 192.168.4.254

```

## VPN 配置

### l2tp vpn

```
[Netkiller]l2tp enable
[Netkiller]domain system
[Netkiller-isp-system]ip pool 1 172.16.1.10 172.16.1.250
[Netkiller-isp-system]quit

[Netkiller]local-user vpn
New local user added.
[Netkiller-luser-vpn]password simple netkiller
[Netkiller-luser-vpn]service-type ppp
[Netkiller-luser-vpn]quit

[Netkiller]interface Virtual-Template 0
[Netkiller-Virtual-Template0]ppp authentication-mode pap
[Netkiller-Virtual-Template0]ip address 172.16.1.254 255.255.255.0
[Netkiller-Virtual-Template0]remote address pool 1
[Netkiller-Virtual-Template0]quit

[Netkiller]l2tp-group 1
[Netkiller-l2tp1]allow l2tp virtual-template 0
[Netkiller-l2tp1]undo tunnel authentication
[Netkiller-l2tp1]mandatory-lcp
[Netkiller-l2tp1]quit

```

```
[Netkiller-l2tp1]display l2tp session
 Total session = 1

 LocalSID  RemoteSID  LocalTID  
  29030     1          1 			

```

```
[Netkiller-l2tp1]display l2tp tunnel
 Total tunnel = 1

 LocalTID RemoteTID RemoteAddress    Port   Sessions RemoteName
 1        19        192.168.4.69     1701   1        DESKTOP-KLBD3DS 			

```

### ipsec

```
[Netkiller]dis ike proposal    
 priority authentication authentication encryption Diffie-Hellman duration
              method       algorithm    algorithm     group       (seconds)
---------------------------------------------------------------------------
  1        PRE_SHARED     SHA         AES_CBC_256     MODP_1536      86400    
  default  PRE_SHARED     SHA         DES_CBC         MODP_768       86400  			

```

```
[Netkiller]dis ipsec proposal 1

  IPsec proposal name: 1
    encapsulation mode: transport
    transform: esp-new
    ESP protocol: authentication sha1-hmac-96, encryption 128-bits aes

```