# 部分 III. Juniper

## 第 16 章 Firewall

```

[root@dev1 ~]# ssh neo@192.168.1.1
neo@192.168.3.1's password:
Remote Management Console
firewall->

```

## config

```

firewall-> get config
Total Config size 17376:
set clock ntp
set clock timezone 7
set vrouter trust-vr sharable
set vrouter "untrust-vr"
exit
set vrouter "trust-vr"
unset auto-route-export
exit
...
...
...
exit
set vrouter "untrust-vr"
exit
set vrouter "trust-vr"
exit
firewall->

```

## interface

```

firewall-> get interface all
box is not in pure_l2_mode

A - Active, I - Inactive, U - Up, D - Down, R - Ready

Total interface: 12
Name           IP Address         Zone        MAC            VLAN State VSD
trust          192.168.3.1/24     Trust       001f.1255.a902    -   U   -
untrust        61.144.230.41/29   Untrust     001f.1255.a901    -   U   -
serial         0.0.0.0/0          Null        001f.1255.a906    -   D   -
tun.1          unnumbered         Untrust     untrust           -   D   -
vlan1          0.0.0.0/0          VLAN        001f.1255.a90f    1   D   -
null           0.0.0.0/0          Null        N/A               -   U   0
firewall->

```

### PPPoE

```
set pppoe name "PPPoE"
set pppoe name "PPPoE" username "cjf0000@163.gd" password "yVizHVPmNgsYRvCpTP7RsQnxg2VpbQ=="
set pppoe name "PPPoE" idle 0
set pppoe name "PPPoE" interface untrust
set pppoe name "PPPoE" auto-connect 30

```

### 接口模式

```
set interface eth4 nat    //将接口 4 设置为 nat 模式
set interface eth4 route  //将接口 4 设置为路由模式

```

Route between multiple subnets without a router

```
set interface trust ip (ip address) (subnet mask) secondary [Enter]
save [Enter]

```

### vlan

```
set zone name office //建立一个 3 层的 zone，名为 Office
set zone name L2-office  L2 1   //建立一个 2 层的 zone，名为 L2-Office（二层接口必须以 L2-开始命名），vlan id 为 1。
set interface eth4 zone office   //将接口 4 设置为 office  zone 的接口。
set interface vlan1 ip 10.10.10.10/24  //将 vlan1 的 ip 设置为 10.10.10.10
set interface vlan1 manage web  //开通 vlan1 接口的 web 管理功能
set interface vlan1 manage ping  //开通 vlan1 接口的 ping 功能

```

### MIP

```
set interface eth3 mip 1.1.1.1 host 2.2.2.2 vrouter trust-vr   //设置 mip，外网 ip1.1.1.1 绑定到内网 ip 2.2.2.2 上
unset interface eth3 mip 1.1.1.1   //取消 1.1.1.1 的 mip 设置

```

```
unset interface "untrust" mip 61.144.230.44
set interface "untrust" mip 61.144.230.44 host 192.168.3.46 netmask 255.255.255.255 vr "trust-vr"

set policy from "Untrust" to "Trust"  "Any" "MIP(61.144.230.44)" "HTTP" permit log

policy id = 79

set policy id 79
set service "HTTPS"
exit

```

### VIP

```
set interface eth3 vip untrust-ip + 21 ftp 192.168.0.10       //设置 vip
set interface eth3 vip untrust-ip + 8000 ftp 192.168.0.10

```

```
set service "OpenSSH" protocol tcp src-port 0-65535 dst-port 22-22

set interface untrust vip 61.144.230.45 + 22 OpenSSH 192.168.3.10

set policy from untrust to trust any vip(61.144.230.45) OpenSSH permit

save

```

## arp

```
host1(config)#arp 192.56.20.1 0090.1a00.0170
host1(config-if)#arp timeout 8000
host1#clear arp

```

## ntp-server

```
set interface ethernet0/3 ntp-server

```

## DHCP

```

set interface ethernet1 dhcp server service  //在接口 1 开启 dhcp 服务
set interface ethernet1 dhcp server enable  //在接口 1 开启 dhcp 服务
set interface ethernet1 dhcp server option lease 1440000  //设置 dhcp 服务租期
set interface ethernet1 dhcp server option gateway 192.168.0.2
set interface ethernet1 dhcp server option netmask 255.255.255.0
set interface ethernet1 dhcp server option dns1 202.96.209.5
set interface ethernet1 dhcp server option dns2 202.96.209.133
set interface ethernet1 dhcp server ip 192.168.0.10 to 192.168.0.100   //dhcp 地址池

```

DMZ 口 DHCP

```
set interface ethernet0/3 dhcp server service
set interface ethernet0/3 dhcp server enable
set interface ethernet0/3 dhcp server option lease 1440
set interface ethernet0/3 dhcp server option gateway 10.10.0.254
set interface ethernet0/3 dhcp server option netmask 255.255.255.0
set interface ethernet0/3 dhcp server option dns1 208.67.222.222
set interface ethernet0/3 dhcp server option dns2 208.67.220.220
set interface ethernet0/3 dhcp server option dns3 8.8.8.8
set interface ethernet0/3 dhcp server ip 10.10.0.200 to 10.10.0.250
set interface ethernet0/3 dhcp server config next-server-ip

```

## SNMP

v1 / v2

```
set snmp community "public" Read-Only Trap-on version v2
set snmp host "public" 172.16.0.0 255.255.255.0 src-interface ethernet0/0
set snmp location "XXX Build 4F"
set snmp contact "neo.chen@example.com"
set snmp port listen 161

```

v2c

```
set snmp community "public" Read-Only Trap-on version v2c
set snmp host "public" 192.168.3.5 255.255.255.255 src-interface ethernet0/0 trap v2
set snmp location "XXX Build 4F"
set snmp contact "neo.chen@example.com"
set snmp port listen 161
set snmp port trap 162

```

登录 Linux 测试 SNMP

```

$ snmpwalk -v 2c -c public <juniper address>

$ snmpwalk -v 2c -Ob -c public 172.16.0.254 |head
iso.3.6.1.2.1.1.1.0 = STRING: "SSG-520 version 6.2.0r5.0 (SN: JN119AD15ADA, Firewall+VPN)"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.3224.1.50
iso.3.6.1.2.1.1.3.0 = Timeticks: (508773200) 58 days, 21:15:32.00
iso.3.6.1.2.1.1.4.0 = STRING: "neo.chen@example.com"
iso.3.6.1.2.1.1.5.0 = STRING: "SSG520"
iso.3.6.1.2.1.1.6.0 = STRING: "TianXiang Build 4F"
iso.3.6.1.2.1.1.7.0 = INTEGER: 72
iso.3.6.1.2.1.2.1.0 = INTEGER: 5
iso.3.6.1.2.1.2.2.1.1.1 = INTEGER: 1
iso.3.6.1.2.1.2.2.1.1.2 = INTEGER: 2

```

http://www.juniper.net/techpubs/software/junos/junos74/swconfig74-net-mgmt/html/enterprise-traps3.html#1097425

```
unset snmp community "public"

```

## Service

```
set service "OpenSSH" protocol tcp src-port 0-65535 dst-port 22-22
set service "POP3" protocol tcp src-port 0-65535 dst-port 110-110
set service "SMTP" protocol tcp src-port 0-65535 dst-port 25-25
set service "RemoteDesktop" protocol tcp src-port 0-65535 dst-port 3389-3389

```

```
unset service "OpenSSH"

```

## Address

```
set address "Trust" "webserver" 192.168.0.1 255.255.255.255
set address "Trust" "ftpserver" 192.168.0.2 255.255.255.255
set address "Untrust" "DNS1" 202.96.134.133 255.255.255.0
set address "Untrust" "DNS2" 202.96.128.68 255.255.255.0

```

```
unset address "Trust" "webserver"

```

## syslog

syslog host

```

	set syslog config "10.0.0.1"
	set syslog config "10.0.0.1"" facilities local1 local1
	set syslog config "10.0.0.1"" log traffic
	set syslog src-interface eth0/0

```

## PPTP

```
set vip multi-port [Enter]
save [Enter]
reset [Enter]

The multi-port command will match the first port it sees in the custom service.

Next, define a custom service for PPTP and apply this service in the VIP.  From the CLI:

set service CustomPPTP group "other" 47 src 2048-2048 dst 2048-2048 [Enter]
set service CustomPPTP + tcp src 0-65535 dst 1723-1723 [Enter]
set interface ethernet0/0 vip 2048 CustomPPTP 10.1.1.10 [Enter]

Finally, create an incoming policy with destination address as the VIP using the custom service object.  From the CLI:

set policy from untrust to trust "any" "VIP::1" "CustomPPTP" permit [Enter]
save [Enter]

```

## 第 17 章 Router

```
set interface trust nat
set interface untrust route

set route 0.0.0.0/0 interface untrust gateway 61.144.230.40 preference 20
set route 192.168.6.0/24 interface trust gateway 192.168.3.12
set route 192.168.7.0/24 interface trust gateway 192.168.3.12
set route 192.168.8.0/24 interface trust gateway 192.168.3.12
set route 192.168.9.0/24 interface trust gateway 192.168.3.12

```

```
set route 172.16.3.0/24 interface ethernet0/3 gateway 10.10.0.5

```

## 第 18 章 Policy

## 策略管理

show policy

```
firewall-> get policy
Total regular policies 24, Default deny.
    ID From     To       Src-address  Dst-address  Service              Action State   ASTLCB
    76 Untrust  Trust    Any          VIP(61.144.~ OpenSSH              Permit enabled -----X
    77 Untrust  Trust    Any          VIP(61.144.~ CTBS                 Permit enabled -----X
    78 Untrust  Trust    Any          VIP(61.144.~ RemoteDesktop        Permit enabled -----X

firewall-> get policy
Total regular policies 23, Default deny.
    76 Untrust  Trust    Any          VIP(61.144.~ OpenSSH              Permit enabled -----X
    78 Untrust  Trust    Any          VIP(61.144.~ RemoteDesktop        Permit enabled -----X

```

Removing policy

```

firewall-> get policy
Total regular policies 24, Default deny.
    ID From     To       Src-address  Dst-address  Service              Action State   ASTLCB
    76 Untrust  Trust    Any          VIP(61.144.~ OpenSSH              Permit enabled -----X
    77 Untrust  Trust    Any          VIP(61.144.~ CTBS                 Permit enabled -----X
    78 Untrust  Trust    Any          VIP(61.144.~ RemoteDesktop        Permit enabled -----X

firewall-> unset policy 77

firewall-> get policy
Total regular policies 23, Default deny.
    76 Untrust  Trust    Any          VIP(61.144.~ OpenSSH              Permit enabled -----X
    78 Untrust  Trust    Any          VIP(61.144.~ RemoteDesktop        Permit enabled -----X

```

policy id = 79

```
set policy id 79
set service "HTTPS"

```

```
unset service "SSH"
exit

```

## OpenSSH

OpenSSH 22 Port

```
set service "OpenSSH" protocol tcp src-port 0-65535 dst-port 22-22

set interface untrust vip 61.144.230.45 + 22 OpenSSH 192.168.3.10

set policy from untrust to trust any vip(61.144.230.45) OpenSSH permit

save

```

## HTTP

```
set service "HTTP" protocol tcp src-port 0-65535 dst-port 80-80

set interface untrust vip 61.144.230.45 + 80 HTTP 192.168.3.114

set policy from untrust to trust any vip(61.144.230.45) HTTP permit

save

```

## RemoteDesktop

```
set service "RemoteDesktop" protocol tcp src-port 0-65535 dst-port 3389-3389

set interface untrust vip 61.144.230.45 + 3389 RemoteDesktop 192.168.3.114

set policy from untrust to trust any vip(61.144.230.45) RemoteDesktop permit

save

```

## PPTP

user -> internet -> juniper -> vip 1723 -> 192.168.3.9:1723

```
set vip multi-port
set alg pptp enable

set service "LinuxPPTP" protocol tcp src-port 0-65535 dst-port 1723-1723
set service "LinuxPPTP" + udp src-port 0-65535 dst-port 1723-1723

set interface untrust vip 61.144.230.44 + 1723 "LinuxPPTP" 192.168.3.9

set policy id 82 from "Trust" to "Untrust"  "Any" "Any" "PPTP" permit
set policy id 82 application "PPTP"
set policy id 82

```

## DMZ to Untrust (nat src)

```
set policy id 94 from "Untrust" to "DMZ"  "Any" "Any" "ANY" permit log
set policy id 94 disable
set policy id 94
exit
set policy id 95 from "DMZ" to "Untrust"  "Any" "Any" "ANY" nat src permit log
set policy id 95
exit
set policy id 96 from "Trust" to "DMZ"  "Any" "Any" "ANY" permit
set policy id 96
exit
set policy id 97 from "DMZ" to "Trust"  "Any" "Any" "ANY" permit log
set policy id 97
exit

```

未设置 nat src,DMZ 将不能访问外网

## 第 19 章 Juniper Flow

```
interfaces {
    ge-0/3/0 {
        unit 0 {
            family inet {
                filter {
                    input all;
                    output all;
                }
                address 10.0.0.1/24;
            }
        }
    }

firewall {
    filter all {
        term all {
            then {
                sample;
                accept;
            }
        }
    }
}

forwarding-options {
    sampling {
        input {
            family inet {
                rate 100;
            }
        }
        output {
            cflowd 10.0.0.100 {
                port 9800;
                version 5;
            }
        }
    }
}

```

调试命令

```
debug flow basic
debug flow drop
undebug all

get dbuf stream

```