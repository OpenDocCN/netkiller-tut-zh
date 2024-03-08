# 部分 V. Dell

## 第 24 章 Dell Switch

最垃圾的交换机，DELL 客服不提供技术支持，网上配置资料非常少。你会看到网上很多人提问，但很少人回复问题，因为没有人知道怎么解决。

Dell 全系交换机产品均没有 DHCP Server 功能，买的时候一定要注意，退货流程及其复杂。

```

BOOT Software Version 2.0.0.00 Built  03-Sep-2008  17:31:01

                          ##
 ###########            #####        ######        ######
 ##############       #########      ######        ######
 ###############     #########       ######        ######
 ################  ########## ####   ######        ######
 ################ #########  ######  ######        ######
 ########################  ######### ######        ######
 ######   ##############  ########  #######        ######
 ######    ###################### #########        ######
 ######    ####################  ##########        ######
 ######    ###################  ###########        ######
 ######    ################## #############        ######
 ######   #################################        ######
 ################################################# ############
 ################  ################  ############# ############
 ################   ##############   ############# ############
 ###############      ##########     ############# ############
 ##############        ########      ############# ############
 ###########             ####        ############# ############
                          ##
 PowerConnect 3548 board based on Orion 88F5181 ARM926EJ processor
 64 MByte SDRAM. I-Cache 8 KB. D-Cache 8 KB. Cache Enabled.

MAC Address   :  a4:ba:db:70:6f:63.

Autoboot in 2 seconds - press RETURN or Esc. to abort and enter prom.
Preparing to decompress...
 100%
Decompressing SW from image-1
 100%

OK
Running from RAM...

*********************************************************************
*** Running  SW  Ver. 2.0.0.29  Date  13-Jan-2009  Time  17:42:37 ***
*********************************************************************

HW version is 00.00.02
Base Mac address is: a4:ba:db:70:6f:63
Dram size is  : 128M bytes
Dram first block size is  : 102400K bytes
Dram first PTR is  : 0x1800000
Dram second block size is  : 4096K bytes
Dram second PTR is  : 0x7C00000
Flash size is: 16M
01-Jan-2000 00:00:06 %CDB-I-LOADCONFIG: Loading running configuration.
01-Jan-2000 00:00:06 %CDB-I-LOADCONFIG: Loading startup configuration.
Device configuration:
Slot 1 - PowerConnect 3548
CPLD revision:  4.01

------------------------------------
-- Unit Standalone                --
------------------------------------

Tapi Version: v1.3.3.1
Core Version: v1.3.3.1
01-Jan-2000 00:00:19 %INIT-I-InitCompleted: Initialization task is completed

01-Jan-2000 00:00:19 %Environment-I-FAN-STAT-CHNG: FAN# 1 status changed - operational.
01-Jan-2000 00:00:19 %Environment-I-FAN-STAT-CHNG: FAN# 2 status changed - operational.
01-Jan-2000 00:00:19 %Environment-I-PS-STAT-CHNG: PS# 1 status changed - operational.
01-Jan-2000 00:00:19 %Environment-W-PS-STAT-CHNG: PS# 2 status changed - not present.
01-Jan-2000 00:00:19 %Entity-I-SEND-ENT-CONF-CHANGE-TRAP: entity configuration change trap.
01-Jan-2000 00:00:19 %SNMP-I-CDBITEMSNUM: Number of running configuration items loaded: 0

01-Jan-2000 00:00:19 %SNMP-I-CDBITEMSNUM: Number of startup configuration items loaded: 0

Welcome to Dell Easy Setup Wizard.

The Setup Wizard guides you through the initial switch configuration, and gets
you up and running as quickly as possible.  You can skip the setup wizard, and
enter CLI mode to manually configure the switch.
The system will prompt you with a default answer; by pressing enter, you accept
the default.
You must respond to the next question to run the setup wizard within
60 seconds, otherwise the system will continue with normal operation using
the default system configuration.

Would you like to enter the setup wizard (you must answer this question within
60 seconds)? (Y/N)[Y]
Setup Wizard Timeout.....

```

## show

### bootvar

```

console> show bootvar
Image  Filename   Version     Date                    Status
-----  ---------  ---------   ---------------------   -----------
1      image-1    2.0.0.29    13-Jan-2009  17:42:37   Active*
2      image-2    2.0.0.29    13-Jan-2009  17:42:37   Not active

"*" designates that the image was selected for the next boot

```

### qos

```

console> show qos
Qos: basic
Basic trust: vpt

```

### line

```

console> show line
Console configuration:
Interactive timeout:  10 minute(s)
History:  10
Baudrate:  9600
Databits: 8
Parity: none
Stopbits: 1

Telnet configuration:
Telnet is enabled.
Interactive timeout:  10 minute(s)
History:  10

SSH configuration:
Interactive timeout:  10 minute(s)
History:  10

```

### privilege

```

console> show privilege
Current privilege level is 1

```

### radius-servers

console> show radius-servers IP address Port port Time- Ret- Dead- source IP Prio. Usage Auth Acct Out rans Time --------------- ----- ----- ------ ------ ------ --------------- ----- ----- Global values -------------- TimeOut : 3 Retransmit : 3 Deadtime : 0 Source IP : 0.0.0.0 Source IPv6 : ::

### sessions

console> show sessions Connection Host Address Port Byte ----------- ---------------------------- ------------------- ------ ------

### system

```

console> show  system
System Description:                       Ethernet Switch
System Up Time (days,hour:min:sec):       00,00:09:52
System Contact:
System Name:
System Location:
System MAC Address:                       a4:ba:db:70:6f:63
System Object ID:                         1.3.6.1.4.1.674.10895.3017
Type:                                     PowerConnect 3548
Asset tag:

Main Power Supply Status:                 OK
Fan 1 Status:                             OK
Fan 2 Status:                             OK

          Unit            Temperature (Celsius)            Status
------------------------ ------------------------ ------------------------
           1                        35                       OK

console>

```

### users

console> show users Username Protocol Location --------------- ------------ ----------------------- Serial 0.0.0.0

### version

console> show version SW version 2.0.0.29 ( date 13-Jan-2009 time 17:42:37 ) Boot version 2.0.0.00 ( date 03-Sep-2008 time 17:31:01 ) HW version 00.00.02

### vlan

console> show vlan Vlan Name Ports Type Authorization ---- ----------------- --------------------------- ------------ ------------- 1 1 e(1-48),g(1-4),ch(1-15) other Required

### config

#### startup-config

```

console# show startup-config
Empty configuration

Default settings:
Service tag:

SW version

Fast Ethernet Ports
==========================
no shutdown
speed 100
duplex full
negotiation
flow-control off
mdix auto
no back-pressure

Gigabit Ethernet Ports
=============================
no shutdown
speed 1000
duplex full
negotiation
flow-control off
mdix auto
no back-pressure

interface vlan 1
interface port-channel 1 - 15

spanning-tree
spanning-tree mode STP

qos basic
qos trust cos

```

#### running-config

```

console# show running-config
voice vlan oui-table add 0001e3 Siemens_AG_phone________
voice vlan oui-table add 00036b Cisco_phone_____________
voice vlan oui-table add 00096e Avaya___________________
voice vlan oui-table add 000fe2 H3C_Aolynk______________
voice vlan oui-table add 0060b9 Philips_and_NEC_AG_phone
voice vlan oui-table add 00d01e Pingtel_phone___________
voice vlan oui-table add 00e075 Polycom/Veritel_phone___
voice vlan oui-table add 00e0bb 3Com_phone______________
interface vlan 1
ip address 172.16.0.252 255.255.255.0
exit

Default settings:
Service tag:

SW version

Fast Ethernet Ports
==========================
no shutdown
speed 100
duplex full
negotiation
flow-control off
mdix auto
no back-pressure

Gigabit Ethernet Ports
=============================
no shutdown
speed 1000
duplex full
negotiation
flow-control off
mdix auto
no back-pressure

interface vlan 1
interface port-channel 1 - 15

spanning-tree
spanning-tree mode STP

qos basic
qos trust cos
console#

```

#### copy

```
console# copy running-config startup-config
Overwrite file [startup-config] ?[Yes/press any key for no]....01-Jan-2000 00:13:41 %COPY-I-FILECPY: Files Copy - source URL running-config destination URL flash://startup-config
01-Jan-2000 00:13:49 %COPY-N-TRAP: The copy operation was completed successfully
Copy succeeded

```

#### delete

```

console>

console>enable

console#delete startup-config

Startup file was deleted（启动文件已删除）

console#reload

Management switch has unsaved changes.（管理服务器仍未保存更改。）

Are you sure you want to continue?（确实要继续吗？）(y/n)（是/否）y（是）

Configuration Not Saved!（配置未保存！）

Are you sure you want to reload the stack?（是否确定要重新载入堆栈？）(y/n)（是/否）y（是）

Reloading all switches..（正在重新载入所有交换机..）

```

## login/authentication

### console

```
console(config)# aaa authentication login default line

console(config)# aaa authentication enable default line

console(config)# line console

console(config-line)# login authentication default

console(config-line)# enable authentication default

console(config-line)# password chen

```

### telnet

```
console# configure
console(config)# aaa authentication login default line
console(config)# aaa authentication enable default line
console(config)# line telnet
console(config-line)# login authentication default
console(config-line)# enable authentication default
console(config-line)# password chen
console(config-line)# end

```

### SSH

```
console(config)# aaa authentication login default line

console(config)# aaa authentication enable default line

console(config)# line ssh

console(config-line)# login authentication default

console(config-line)# enable authentication default

console(config-line)# password jones.

```

### HTTP/HTTPS

HTTP

```
console(config)# ip http authentication local

console(config)# username admin password user1 level 15

```

HTTPS

```
console(config)# ip https authentication local

console(config)# username admin password user1 level 15

```

SSL 2.0

```
console(config)# crypto certificate generate key_generate

console(config)# ip https server

```

## Interface

### status

3548

```
console# show interfaces status
                                             Flow Link          Back   Mdix
Port     Type         Duplex  Speed Neg      ctrl State       Pressure Mode
-------- ------------ ------  ----- -------- ---- ----------- -------- -------
e1       100M-Copper    --      --     --     --  Down           --     --
e2       100M-Copper    --      --     --     --  Down           --     --
e3       100M-Copper    --      --     --     --  Down           --     --
e4       100M-Copper    --      --     --     --  Down           --     --
e5       100M-Copper    --      --     --     --  Down           --     --
e6       100M-Copper    --      --     --     --  Down           --     --
e7       100M-Copper    --      --     --     --  Down           --     --
e8       100M-Copper    --      --     --     --  Down           --     --
e9       100M-Copper    --      --     --     --  Down           --     --
e10      100M-Copper    --      --     --     --  Down           --     --
e11      100M-Copper    --      --     --     --  Down           --     --
e12      100M-Copper    --      --     --     --  Down           --     --
e13      100M-Copper    --      --     --     --  Down           --     --
e14      100M-Copper    --      --     --     --  Down           --     --
e15      100M-Copper    --      --     --     --  Down           --     --
e16      100M-Copper    --      --     --     --  Down           --     --
e17      100M-Copper    --      --     --     --  Down           --     --
e18      100M-Copper    --      --     --     --  Down           --     --
e19      100M-Copper    --      --     --     --  Down           --     --
e20      100M-Copper    --      --     --     --  Down           --     --
e21      100M-Copper    --      --     --     --  Down           --     --
e22      100M-Copper    --      --     --     --  Down           --     --
e23      100M-Copper    --      --     --     --  Down           --     --
e24      100M-Copper    --      --     --     --  Down           --     --
e25      100M-Copper    --      --     --     --  Down           --     --
e26      100M-Copper    --      --     --     --  Down           --     --
e27      100M-Copper    --      --     --     --  Down           --     --
e28      100M-Copper    --      --     --     --  Down           --     --
e29      100M-Copper    --      --     --     --  Down           --     --
e30      100M-Copper    --      --     --     --  Down           --     --
e31      100M-Copper    --      --     --     --  Down           --     --
e32      100M-Copper    --      --     --     --  Down           --     --
e33      100M-Copper    --      --     --     --  Down           --     --
e34      100M-Copper    --      --     --     --  Down           --     --
e35      100M-Copper    --      --     --     --  Down           --     --
e36      100M-Copper    --      --     --     --  Down           --     --
e37      100M-Copper  Full    100   Enabled  Off  Up          Disabled Off
e38      100M-Copper    --      --     --     --  Down           --     --
e39      100M-Copper    --      --     --     --  Down           --     --
e40      100M-Copper    --      --     --     --  Down           --     --
e41      100M-Copper    --      --     --     --  Down           --     --
e42      100M-Copper    --      --     --     --  Down           --     --
e43      100M-Copper    --      --     --     --  Down           --     --
e44      100M-Copper    --      --     --     --  Down           --     --
e45      100M-Copper    --      --     --     --  Down           --     --
e46      100M-Copper    --      --     --     --  Down           --     --
e47      100M-Copper    --      --     --     --  Down           --     --
e48      100M-Copper    --      --     --     --  Down           --     --
g1       1G-Fiber       --      --     --     --  Down           --     --
g2       1G-Fiber       --      --     --     --  Down           --     --
g3       1G-Copper      --      --     --     --  Down           --     --
g4       1G-Copper      --      --     --     --  Down           --     --

                                          Flow    Link
Ch       Type    Duplex  Speed  Neg      control  State
-------- ------- ------  -----  -------- -------  -----------
ch1         --     --      --      --       --    Not Present
ch2         --     --      --      --       --    Not Present
ch3         --     --      --      --       --    Not Present
ch4         --     --      --      --       --    Not Present
ch5         --     --      --      --       --    Not Present
ch6         --     --      --      --       --    Not Present
ch7         --     --      --      --       --    Not Present
ch8         --     --      --      --       --    Not Present
ch9         --     --      --      --       --    Not Present
ch10        --     --      --      --       --    Not Present
ch11        --     --      --      --       --    Not Present
ch12        --     --      --      --       --    Not Present
ch13        --     --      --      --       --    Not Present
ch14        --     --      --      --       --    Not Present
ch15        --     --      --      --       --    Not Present

```

6224

```
console#show interfaces status

Port   Type                            Duplex  Speed    Neg  Link  Flow Control
                                                             State Status
-----  ------------------------------  ------  -------  ---- ----- -----------
1/g1   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g2   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g3   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g4   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g5   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g6   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g7   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g8   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g9   Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g10  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g11  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g12  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g13  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g14  Gigabit - Level                 Full    100      Auto  Up    Inactive
1/g15  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g16  Gigabit - Level                 Full    1000     Auto  Up    Inactive
1/g17  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g18  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g19  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
--More-- or (q)uit
1/g20  Gigabit - Level                 Full    100      Auto  Up    Inactive
1/g21  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g22  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g23  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/g24  Gigabit - Level                 N/A     Unknown  Auto  Down  Inactive
1/xg1  10G - Level                     N/A     Unknown  Auto  Down  Inactive
1/xg2  10G - Level                     N/A     Unknown  Auto  Down  Inactive
1/xg3  10G - Level                     N/A     Unknown  Auto  Down  Inactive
1/xg4  10G - Level                     N/A     Unknown  Auto  Down  Inactive

Ch   Type                            Link
                                     State
---  ------------------------------  -----
ch1  Link Aggregate                  Down
ch2  Link Aggregate                  Down
ch3  Link Aggregate                  Down
ch4  Link Aggregate                  Down
ch5  Link Aggregate                  Down
ch6  Link Aggregate                  Down
ch7  Link Aggregate                  Down
ch8  Link Aggregate                  Down
ch9  Link Aggregate                  Down
--More-- or (q)uit
ch10 Link Aggregate                  Down
ch11 Link Aggregate                  Down
ch12 Link Aggregate                  Down
ch13 Link Aggregate                  Down
ch14 Link Aggregate                  Down
ch15 Link Aggregate                  Down
ch16 Link Aggregate                  Down
ch17 Link Aggregate                  Down
ch18 Link Aggregate                  Down
ch19 Link Aggregate                  Down
ch20 Link Aggregate                  Down
ch21 Link Aggregate                  Down
ch22 Link Aggregate                  Down
ch23 Link Aggregate                  Down
ch24 Link Aggregate                  Down

Flow Control:Disabled

```

### ip address

分配动态 IP 地址：

```
console# configure

console(config)# interface ethernet 1/e1

console(config-if)# ip address dhcp hostname powerconnect

console(config-if)# exit

console(config)#

```

分配动态 IP 地址（在 VLAN 上）：

```
console# configure

console(config)# interface ethernet vlan 1

console(config-if)# ip address dhcp hostname device

console(config-if)# exit

console(config)#

接口将自动获取 IP 地址。

```

要验证 IP 地址，请在系统提示符后输入 show ip interface 命令，如以下示例所示。

```
console# show ip interface

IP Address I/F Type

------------- ------ -------

100.1.1.1/24 vlan 1 dynamic

```

### speed

```
console#configure

console(config)#line console

console(config-line)#speed 38400

```

### QOS

1000Kpbs

```
console# configure
console(config)# interface ethernet e36
console(config-if)# rate-limit 1000
console(config-if)#

```

## VLAN

### define vlan id

```
console(config)#vlan database

console(config-vlan)#vlan 2-10
Warning: The use of large numbers of VLANs or interfaces may cause significant
delays in applying the configuration.

console(config-vlan)#

```

### show vlan

```
console#show vlan

VLAN       Name                         Ports          Type      Authorization
-----  ---------------                  -------------  -----     -------------
1      Default                          ch1-24,        Default   Required
                                        1/g1-1/xg4
2                                                      Static    Required
3                                                      Static    Required
4                                                      Static    Required
5                                                      Static    Required
6                                                      Static    Required
7                                                      Static    Required
8                                                      Static    Required
9                                                      Static    Required
10                                                     Static    Required

```

### interface vlan

```

console#configure

console(config)#interface vlan 2

console(config-if-vlan2)#ip address 192.168.0.1 255.255.255.0

```

### interface ethernet

```
console(config)#interface ethernet 1/g5
console(config-if-1/g5)#switchport mode access
console(config-if-1/g5)#switchport access vlan 5
Warning: The use of large numbers of VLANs or interfaces may cause significant
delays in applying the configuration.

console(config-if-1/g5)#

```

### Virtual LAN Routing Commands

```
console(config)#interface vlan 11

console(config-if-vlan11)#routing

```

```
console#vlan database

console(config-vlan)#vlan routing 10

```

## Routing Information Protocol (RIP) Commands

### show line

```
console(config)#router rip

console(config-router)#

```

### show interfaces

FastEthernet0/0 is f0/0

```

console#show ip interface management

IP Address..................................... 172.16.0.251
Subnet Mask.................................... 255.255.255.0
Default Gateway................................ 0.0.0.0
Burned In MAC Address.......................... A4BA.DB6E.522E
Network Configuration Protocol Current......... None
Management VLAN ID............................. 1

```

## 第 25 章 OpenManage

http://linux.dell.com/

## 安装 OpenManage

http://www.dell.com.cn/

```
tar zxvf OM_6.1.0_ManNode_A00.tar.gz

```

```
vi /etc/RedHat-release

CentOS release 5.5 (Final)  Tikanga

或者

vim linux/supportscripts/srvadmin-install.sh
GetOSType()
{
    # Set default values for return variables.
    GBL_OS_TYPE=${GBL_OS_TYPE_UKNOWN}
    GBL_OS_TYPE_STRING="UKNOWN"

改为

GetOSType()
{
    # Set default values for return variables.
#    GBL_OS_TYPE=${GBL_OS_TYPE_UKNOWN}
#    GBL_OS_TYPE_STRING="UKNOWN"
    GBL_OS_TYPE=${GBL_OS_TYPE_RHEL5}
    GBL_OS_TYPE_STRING="RHEL5"

```

```
# yum install libstdc++-devel libidn-devel curl-devel

```

```
# ./setup.sh
Do you agree to the above license terms? ('y' for yes | 'Enter' to exit): y

##############################################

  Server Administrator Custom Install Utility

##############################################

  Components for Server Administrator Managed Node Software:

    [ ] 1\. Server Administrator Web Server
    [ ] 2\. Server Instrumentation
    [ ] 3\. Storage Management
    [ ] 4\. Remote Access Core Component
    [ ] 5\. Remote Access SA Plugin Component
    [ ] 6\. All

  Enter the number to select a component from the above list.
  Enter q to quit.

  Enter : 6

##############################################

  Server Administrator Custom Install Utility

##############################################

  Selected Options
   - All

  Dependencies
   - Server Administrator Web Server
   - Server Instrumentation
   - Storage Management
   - Remote Access Core Component
   - Remote Access SA Plugin Component

  Components for Server Administrator Managed Node Software:

    [x] 1\. Server Administrator Web Server
    [x] 2\. Server Instrumentation
    [x] 3\. Storage Management
    [x] 4\. Remote Access Core Component
    [x] 5\. Remote Access SA Plugin Component
    [x] 6\. All

  Enter the number to select a component from the above list.
  Enter c to copy selected components to destination folder.
  Enter i to install the selected components.
  Enter r to reset selection and start over.
  Enter q to quit.

  Enter : i

  Default install location is: /opt/dell/srvadmin
  Do you want to change it?
  Press ('y' for yes | 'Enter' for default): y
  Enter relocation path or press 'Enter' for default here:

```

```
Installing the selected packages.

warning: srvadmin-cm-6.1.0-648.i386.rpm: Header V3 DSA signature: NOKEY, key ID 23b66a9d
Preparing...                ########################################### [100%]
   1:srvadmin-omilcore      ########################################### [  6%]
     To start all installed services without a reboot,
     enter the following command:  srvadmin-services.sh  start
   2:srvadmin-omcommon      ########################################### [ 12%]
   3:srvadmin-hapi          ########################################### [ 18%]
   4:srvadmin-syscheck      ########################################### [ 24%]
   5:srvadmin-deng          ########################################### [ 29%]
   6:srvadmin-omacore       ########################################### [ 35%]
   7:srvadmin-isvc          ########################################### [ 41%]
   8:srvadmin-omauth        ########################################### [ 47%]
   9:srvadmin-wsmanclient   ########################################### [ 53%]
  10:srvadmin-idrac-componen########################################### [ 59%]
  11:srvadmin-jre           ########################################### [ 65%]
  12:srvadmin-cm            ########################################### [ 71%]
  13:srvadmin-idracadm      ########################################### [ 76%]
  14:srvadmin-idracdrsc     ########################################### [ 82%]
  15:srvadmin-iws           ########################################### [ 88%]
  16:srvadmin-omhip         ########################################### [ 94%]
  17:srvadmin-storage       ########################################### [100%]

   Do you want the Server Administrator services started?
   Press ('y' for yes | 'Enter' to exit): y
Starting Systems Management Device Drivers:
Starting dell_rbu:                                         [  OK  ]
Starting ipmi driver:                                      [  OK  ]
Starting Systems Management Data Engine:
Starting dsm_sa_datamgr32d:                                [  OK  ]
Starting dsm_sa_eventmgr32d:                               [  OK  ]
Starting DSM SA Shared Services:                           [  OK  ]

Starting DSM SA Connection Service:                        [  OK  ]

```

```
# srvadmin-services.sh
Usage: srvadmin-services.sh {start|stop|status|restart|enable|disable|help}
  start  : starts Server Administrator services
  stop   : stops Server Administrator services
  status : display status of Server Administrator services
  restart: restart Server Administrator services
  enable : Enable Server Administrator services in runlevels 2, 3, 4, and 5
  disable: Disable Server Administrator services in runlevels 2, 3, 4, and 5
  help   : Displays this help text

```

## Yum

[`linux.dell.com/repo/hardware/`](http://linux.dell.com/repo/hardware/)

```
wget -q -O - http://linux.dell.com/repo/hardware/OMSA_6.3/bootstrap.cgi | bash

```

```
[root@Search-Web ~]# yum search srvadmin
Loaded plugins: dellsysid, fastestmirror
Determining fastest mirrors
 * addons: mirrors.163.com
 * base: mirrors.163.com
 * epel: mirrors.sohu.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
addons                                                                                                                                                    |  951 B     00:00
addons/primary                                                                                                                                            |  204 B     00:00
base                                                                                                                                                      | 2.1 kB     00:00
base/primary_db                                                                                                                                           | 2.1 MB     00:00
dell-omsa-indep                                                                                                                                           | 1.9 kB     00:00
dell-omsa-indep/primary                                                                                                                                   |  82 kB     00:01
dell-omsa-indep                                                                                                                                                          596/596
dell-omsa-specific                                                                                                                                        | 1.9 kB     00:00
dell-omsa-specific/primary                                                                                                                                | 1.1 kB     00:00
dell-omsa-specific                                                                                                                                                           2/2
epel                                                                                                                                                      | 3.7 kB     00:00
epel/primary_db                                                                                                                                           | 3.4 MB     00:01
extras                                                                                                                                                    | 2.1 kB     00:00
extras/primary_db                                                                                                                                         | 246 kB     00:00
updates                                                                                                                                                   | 1.9 kB     00:00
updates/primary_db                                                                                                                                        | 1.0 MB     00:00
=============================================================================== Matched: srvadmin ===============================================================================
srvadmin-all.x86_64 : Meta package for installing all Server Administrator features, 6.3.0
srvadmin-argtable2.x86_64 : A library for parsing GNU style command line arguments, 6.3.0
srvadmin-base.x86_64 : Meta package for installing the Server Agent, 6.3.0
srvadmin-cm.i386 : OpenManage Inventory Collector, 6.3.0
srvadmin-deng.x86_64 : Data Engine, 5.9.3
srvadmin-hapi.i386 : Hardware Abstraction Programming Interface, 5.9.3
srvadmin-hapi.x86_64 : Hardware Abstraction Programming Interface, 5.9.3
srvadmin-idrac.x86_64 : Meta rpm for iDRAC components, 6.3.0
srvadmin-idrac-ivmcli.x86_64 : Modular Server Virtual Media CLI Utils, 1.0.0
srvadmin-idrac-vmcli.x86_64 : Monolithic Server Virtual Media CLI Utils, 1.0.0
srvadmin-idracadm.x86_64 : iDRAC Command Interface, 6.3.0
srvadmin-isvc.x86_64 : Instrumentation Services, 5.9.3
srvadmin-itunnelprovider.x86_64 : Integrated Tunnel Provider, 1.3.0
srvadmin-iws.x86_64 : Secure Port Server and Server Administrator GUI, 6.3.0
srvadmin-jre.x86_64 : SUN Java Runtime Environment, 1.6.17
srvadmin-omacore.x86_64 : Server Administrator Core and CLI, 6.3.0
srvadmin-omcommon.x86_64 : Server Administrator Common Framework, 6.3.0
srvadmin-omilcore.noarch : Server Administrator Install Core, 6.3.0
srvadmin-rac-components.x86_64 : Remote Access Card Data Populator, 6.3.0
srvadmin-rac4.x86_64 : Meta rpm for RAC4 components, 4.6.5
srvadmin-rac4-populator.x86_64 : Remote Access Card Data Populator, 4.6.5
srvadmin-rac5.x86_64 : Meta rpm for RAC5 components, 6.3.0
srvadmin-racadm4.x86_64 : RAC Command Interface, 4.6.5
srvadmin-racadm5.x86_64 : RAC5 Command Interface, 6.3.0
srvadmin-racdrsc.x86_64 : RAC Integration Layer, 6.3.0
srvadmin-racsvc.x86_64 : Remote Access Card Managed Node, 4.6.5
srvadmin-smcommon.x86_64 : Storage Management common files for GUI and CLI, 3.3.0
srvadmin-smweb.x86_64 : Storage Management package for GUI component, 3.3.0
srvadmin-standardAgent.x86_64 : Meta package for installing the Standard Server Agent, 6.3.0
srvadmin-storage.x86_64 : Storage Management accessors package, 3.3.0
srvadmin-storage-populator.x86_64 : Storage Management populator package, 3.3.0
srvadmin-storage-populator-swraid.x86_64 : Storage Management populator files for DotHill software RAID library, 3.3.0
srvadmin-storageservices.x86_64 : Meta package for installing the Server Administrator Storage Services feature, 6.3.0
srvadmin-storelib.x86_64 : StoreLib package for storage management, 6.3.0
srvadmin-storelib-sysfs.x86_64 : System Utilities Package/Libsysfs for LSI storage libraries, 2.1.0
srvadmin-sysfsutils.x86_64 : Storage Management System Utilities Package / Libsysfs, 1.3.0
srvadmin-webserver.x86_64 : Meta package for installing the Server Administrator Web Server feature, 6.3.0
srvadmin-xmlsup.x86_64 : Server Administrator XML Support SDK, 6.3.0

```

```
yum install srvadmin-all

```

## Dell IT Assistant

## DMC

## 第 26 章 Dell Server

## iDRAC - Integrated Dell Remote Access Controller 6 - Enterprise

### default password

root: calvin

### iDRAC6 Configuration Utility

| ![](img/BIOS.jpg) |

| ![](img/iDRAC6-1.png) |

| ![](img/iDRAC6-2.png) |

| ![](img/iDRAC6-3.png) |

我一般不再 iDRAC 中配置密码，因为我的密码比较复杂，输入起来比较麻烦。

比习惯使用 ipmitool set password 2 password "0KXcHhqPHXg7PrQ9" 设置比较复杂的密码。

### 通过 ipmitool 查看 iDRAC IP 地址

```
# ipmitool -I open lan print 1
Set in Progress         : Set Complete
Auth Type Support       : NONE MD2 MD5 PASSWORD
Auth Type Enable        : Callback : MD2 MD5
                        : User     : MD2 MD5
                        : Operator : MD2 MD5
                        : Admin    : MD2 MD5
                        : OEM      :
IP Address Source       : Static Address
IP Address              : 172.16.5.46
Subnet Mask             : 255.255.255.0
MAC Address             : 18:03:73:f5:ef:87
SNMP Community String   : public
IP Header               : TTL=0x40 Flags=0x40 Precedence=0x00 TOS=0x10
Default Gateway IP      : 172.16.5.254
Default Gateway MAC     : 00:00:00:00:00:00
Backup Gateway IP       : 0.0.0.0
Backup Gateway MAC      : 00:00:00:00:00:00
802.1q VLAN ID          : 1
802.1q VLAN Priority    : 0
RMCP+ Cipher Suites     : 0,1,2,3,4,5,6,7,8,9,10,11,12,13,14
Cipher Suite Priv Max   : aaaaaaaaaaaaaaa
                        :     X=Cipher Suite Unused
                        :     c=CALLBACK
                        :     u=USER
                        :     o=OPERATOR
                        :     a=ADMIN
                        :     O=OEM

```

### 修改 iDRAC 密码

登陆 Linux 服务器使用 ipmitool 命令行修改 drac 密码

```
ipmitool user list [channel number] 			# 列举用户及用户 ID
ipmitool user set password [user id] [password] # 更改密码

```

```
/sbin/service ipmi start

# ipmitool user list 2
ID  Name             Enabled Callin  Link Auth  IPMI Msg   Channel Priv Limit
2   root             true    true    true       true       ADMINISTRATOR

ipmitool set password 2 password "chen" 	#dell 1950 用法
ipmitool set password 2 "chen" 				#dell 2950 用法,去掉后一个 password 关键字

```

R160 用法

```
# ipmitool user list 2
ID  Name	     Callin  Link Auth	IPMI Msg   Channel Priv Limit
2   root             true    true       true       ADMINISTRATOR

# ipmitool user set password 2 your_password

```

FreeBSD

```
cd /usr/ports/sysutils/ipmitool
make install
kldload ipmi
ipmitool user set password 2 "chen"

```

## PERC H700 Integrated - Raid Card

| ![](img/idrac-bios.png) |

| ![](img/idrac-raid.png) |

### Clear Config

| ![](img/idrac-raid-clear-1.png) |

| ![](img/idrac-raid-clear-2.png) |

### Raid 0

F2

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-vdm.png) |

| ![](img/idrac-raid-0-1.png) |

| ![](img/idrac-raid-0-2.png) |

| ![](img/idrac-raid-0-3.png) |

### Raid 1

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-vdm.png) |

| ![](img/idrac-raid-1-1.png) |

| ![](img/idrac-raid-1-2.png) |

| ![](img/idrac-raid-1-3.png) |

### Raid 5

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-5-1.png) |

| ![](img/idrac-raid-5-2.png) |

| ![](img/idrac-raid-5-3.png) |

| ![](img/idrac-raid-5-4.png) |

### Raid 6

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-6.png) |

### Raid 10

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-10-1.png) |

| ![](img/idrac-raid-10-2.png) |

### Raid 50

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-50-1.png) |

| ![](img/idrac-raid-50-2.png) |

### HS

| ![](img/idrac-raid-hs-1.png) |

| ![](img/idrac-raid-hs-2.png) |

| ![](img/idrac-raid-hs-3.png) |

### Virtual Disk

| ![](img/idrac-raid-vd.png) |

| ![](img/idrac-raid-vd-create-1.png) |

| ![](img/idrac-raid-vd-add.png) |

| ![](img/idrac-raid-vd-size.png) |

| ![](img/idrac-raid-vd-list.png) |

Delete VD

| ![](img/idrac-raid-vd-delete-1.png) |

| ![](img/idrac-raid-vd-delete-2.png) |

### Save

| ![](img/idrac-raid-save.png) |

## 第 27 章 MD Storage

## MD1200

H800 配置

## MD3200

## MD3620i