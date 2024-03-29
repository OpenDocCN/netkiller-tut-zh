# 部分 VIII. A10 Networks

## 第 36 章 WebUI

## MGMT IP

```
Mgr: 172.31.31.31
User: admin
Passwd: a10

```

## 第 37 章 ACOS - CLI

```
Last login: Wed Aug  3 10:17:23 2011 from 192.168.80.69

AX system is ready now.

[type ? for help]

AX1000>en
Password:
AX1000#

```

## show

### version

```

AX1000#show version
AX Series Advanced Traffic Manager AX1000
  Copyright 2007-2011 by A10 Networks, Inc.  All A10 Networks products are
  protected by one or more of the following US patents and patents pending:
  7716378, 7675854, 7647635, 7552126, 20090049537, 20080229418, 20080040789,
  20070283429, 20070271598, 20070180101

      Advanced Core OS (ACOS) version 2.6.1, build 479 (May-03-2011,20:57)
        Booted from Hard Disk primary image
      Serial Number: AX10A92211190054
      aFleX version: 2.0.0
      Hard Disk primary image (default) version 2.6.1, build 479
      Hard Disk secondary image version 2.4.3-p6, build 27
      Compact Flash primary image (default) version 2.4.3-p6, build 27
      Compact Flash secondary image version 2.4.3-p6, build 27
      Last configuration saved at Aug-3-2011, 10:07
      Hardware: 4 CPUs(Stepping 10), Single 233G Hard disk
      Memory 2063 Mbyte, Free Memory 799 Mbyte
      Current time is Aug-3-2011, 10:42
      The system has been up 0 day, 1 hour, 12 minutes

```

### config

```
# show running-config
# show startup-config

```

### clock

```
#show clock
*18:02:16 CST Wed Aug 3 2011

```

### show ip nat pool statistics

```
AX1000#show ip nat pool statistics
Pool         Address          Port Usage   Total Used   Total Freed  Failed
-------------------------------------------------------------------------------
snat_pool    172.16.0.21      0            1796770      1796770      0
Pool         Address          Port Usage   Total Used   Total Freed  Failed
-------------------------------------------------------------------------------
snat_pool_user 172.16.0.22      0            1            1            0

```

## clock

```

AX1000#config terminal
AX1000(config)#clock timezone ?
  Pacific/Midway               (GMT-11:00)Midway Island, Samoa
  Pacific/Honolulu             (GMT-10:00)Hawaii
  America/Anchorage            (GMT-09:00)Alaska
  America/Tijuana              (GMT-08:00)Pacific Time - Tijuana
  America/Los_Angeles          (GMT-08:00)Pacific Time(US & Canada)
  America/Vancouver            (GMT-08:00)Pacific Time - west British Columbia
  America/Phoenix              (GMT-07:00)Arizona
  America/Shiprock             (GMT-07:00)Mountain Time(US & Canada)
  America/Chicago              (GMT-06:00)Central Time(US & Canada)
  America/Mexico_City          (GMT-06:00)Mexico City
  America/Regina               (GMT-06:00)Saskatchewan
  America/Swift_Current        (GMT-06:00)Central America
  America/Kentucky/Monticello  (GMT-05:00)Eastern Time(US & Canada)
  America/Indiana/Marengo      (GMT-05:00)Indiana(East)
  America/Montreal             (GMT-05:00)Eastern Time - Ontario & Quebec -
                               most locations
  America/New_York             (GMT-05:00)Eastern Time
  America/Toronto              (GMT-05:00)Eastern Time - Toronto, Ontario
  America/Caracas              (GMT-04:00)Caracas, La Paz
  America/Halifax              (GMT-04:00)Atlantic Time(Canada)
  America/Santiago             (GMT-04:00)Santiago
  America/St_Johns             (GMT-03:30)Newfoundland
  America/Buenos_Aires         (GMT-03:00)Buenos Aires, Georgetown
  America/Godthab              (GMT-03:00)Greenland
  Atlantic/South_Georgia       (GMT-02:00)Mid-Atlantic
  Atlantic/Azores              (GMT-01:00)Azores
  Atlantic/Cape_Verde          (GMT-01:00)Cape Verde Is.
  Europe/Dublin                (GMT)Greenwich Mean Time: Dublin, Edinburgh,
                               Lisbon, London
  Africa/Algiers               (GMT+01:00)West Central Africa
  Europe/Amsterdam             (GMT+01:00)Amsterdam, Berlin, Bern, Rome,
                               Stockholm, Vienna
  Europe/Belgrade              (GMT+01:00)Belgrade, Bratislava, Budapest,
                               Ljubljana, Prague
  Europe/Brussels              (GMT+01:00)Brussels, Copenhagen, Madrid, Paris
  Europe/Sarajevo              (GMT+01:00)Sarajevo, Skopje, Sofija, Vilnius,
                               Warsaw, Zagreb
  Europe/Bucharest             (GMT+02:00)Bucharest
  Africa/Cairo                 (GMT+02:00)Cairo
  Europe/Athens                (GMT+02:00)Athens, Istanbul, Minsk
  Africa/Harare                (GMT+02:00)Harare, Pretoria
  Asia/Jerusalem               (GMT+02:00)Jerusalem
  Europe/Helsinki              (GMT+02:00)Helsinki, Riga, Tallinn
  Africa/Nairobi               (GMT+03:00)Nairobi
  Asia/Baghdad                 (GMT+03:00)Baghdad
  Asia/Kuwait                  (GMT+03:00)Kuwait, Riyadh
  Europe/Moscow                (GMT+03:00)Moscow, St.Petersburg, Volgogard
  Asia/Tehran                  (GMT+03:30)Tehran
  Asia/Baku                    (GMT+04:00)Baku, Tbilisi, Yerevan
  Asia/Muscat                  (GMT+04:00)Abu Dhabi, Muscat
  Asia/Kabul                   (GMT+04:30)Kabul
  Asia/Karachi                 (GMT+05:00)Islamabad, Karachi, Tashkent
  Asia/Yekaterinburg           (GMT+05:00)Ekaterinburg
  Asia/Calcutta                (GMT+05:30)Calcutta, Chennai, Mumbai, New Delhi
  Asia/Katmandu                (GMT+05:45)Kathmandu
  Asia/Almaty                  (GMT+06:00)Almaty, Novosibirsk
  Asia/Dhaka                   (GMT+06:00)Astana, Dhaka
  Indian/Chagos                (GMT+06:00)Sri Jayawardenepura
  Asia/Rangoon                 (GMT+06:30)Rangoon
  Asia/Bangkok                 (GMT+07:00)Bangkok, Hanoi, Jakarta
  Asia/Krasnoyarsk             (GMT+07:00)Krasnoyarsk
  Asia/Irkutsk                 (GMT+08:00)Irkutsk, Ulaan Bataar
  Asia/Kuala_Lumpur            (GMT+08:00)Kuala Lumpur, Singapore
  Asia/Shanghai                (GMT+08:00)Beijing, Chongqing, Hong Kong, Urumqi
  Asia/Taipei                  (GMT+08:00)Taipei
  Australia/Perth              (GMT+08:00)Perth
  Asia/Seoul                   (GMT+09:00)Seoul
  Asia/Tokyo                   (GMT+09:00)Osaka, Sapporo, Tokyo
  Asia/Yakutsk                 (GMT+09:00)Yakutsk
  Australia/Adelaide           (GMT+09:30)Adelaide
  Australia/Darwin             (GMT+09:30)Darwin
  Australia/Hobart             (GMT+10:00)Hobart
  Australia/Brisbane           (GMT+10:00)Brisbane
  Asia/Vladivostok             (GMT+10:00)Vladivostok
  Australia/Sydney             (GMT+10:00)Canberra, Melbourne, Sydney
  Pacific/Guam                 (GMT+10:00)Guam, Port Moresby
  Asia/Magadan                 (GMT+11:00)Magadan, Solomon., New Caledonia
  Pacific/Auckland             (GMT+12:00)Auckland, Wellington
  Pacific/Fiji                 (GMT+12:00)Fiji, Kamchatka, Marshall Is.
  Pacific/Kwajalein            (GMT+12:00)Eniwetok, Kwajalein
  Pacific/Enderbury            (GMT+13:00)Nuku'alofa
AX1000(config)#clock timezone Asia/Shanghai

```

## interface

```
AX1000#conf t
AX1000(config)#interface ethernet 5
AX1000(config-if:ethernet5)#enable
AX1000(config-if:ethernet5)#ip address 172.16.120.10 /24

```

```
interface ethernet 5
enable
 ip address 172.16.120.10 255.255.255.0

```

## 聚合端口

```
trunk 1
 ethernet 1 to 2

vlan 2
 untagged ethernet 1 to 2
 router-interface ve 2

interface ve 2
 ip address 172.16.0.20 255.255.255.0

```

交换机端简单配置

```
interface Port-channel4

interface GigabitEthernet1/0/3
 channel-group 4 mode on
 spanning-tree portfast
!
interface GigabitEthernet1/0/4
 channel-group 4 mode on
 spanning-tree portfast
!

```

华为交换机

```
undo port-group A10
undo port-group A102
45 47;46 48 聚合成一个端口
interface eth-trunk 1
port link-type trunk
port trunk allow-pass vlan all
bpdu enable
ntdp enable
ndp enable
quit

interface gi 0/0/45
undo port link-type
undo port default vlan
bpdu disable
undo ntdp enable
undo ndp enable
undo port trunk allow-pass vlan all
eth-trunk 1

interface gi 0/0/47
undo port link-type
undo port default vlan
bpdu disable
undo ntdp enable
undo ndp enable
undo port trunk allow-pass vlan all
eth-trunk 1

```

思科交换机

```
gz-c3560-1-s61-11f-11#show run inter port-channel 37 Building configuration...

Current configuration : 132 bytes
!
interface Port-channel37
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 2,45,75
 switchport mode trunk
end

gz-c3560-1-s61-11f-11#show run inter g0/37
Building configuration...

Current configuration : 180 bytes
!
interface GigabitEthernet0/37
 description A10
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 2,45,75
 switchport mode trunk
 channel-group 37 mode on
end

gz-c3560-1-s61-11f-11#

ct-c4506-1-s96-2f-i8-n397#show run inter port-channel 4
Building configuration...

Current configuration : 159 bytes
!
interface Port-channel4
 description AX2100-1
 switchport
 switchport trunk allowed vlan 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
end

ct-c4506-1-s96-2f-i8-n397#show run inter g6/3
Building configuration...

Current configuration : 181 bytes
!
interface GigabitEthernet6/3
 description AX2100-1-e10
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1
 switchport mode trunk
 channel-group 4 mode on
end

ct-c4506-1-s96-2f-i8-n397#show run inter g6/4
Building configuration...

Current configuration : 181 bytes
!
interface GigabitEthernet6/4
 description AX2100-1-e11
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1
 switchport mode trunk
 channel-group 4 mode on
end

ct-c4506-1-s96-2f-i8-n397#show run inter g6/5
Building configuration...

Current configuration : 155 bytes
!
interface GigabitEthernet6/5
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 1
 switchport mode trunk
 channel-group 4 mode on
end

```

## route

```
ip route 0.0.0.0 /0 172.16.0.254

```

## slb

```
slb server rs_172.16.0.5 172.16.0.5
   port 80  tcp
slb server rs_172.16.0.6 172.16.0.6
   port 80  tcp
!
slb service-group test tcp
    member rs_172.16.0.5:80
    member rs_172.16.0.6:80
!
!
slb virtual-server vip_172.16.0.21 172.16.0.21
   port 80  tcp
      source-nat pool snat_pool
      service-group test

```

## example

```
!Current configuration: 2054 bytes
!Configuration last updated at 17:27:46 CST Thu Aug 4 2011
!Configuration last saved at 17:28:07 CST Thu Aug 4 2011
!version 2.6.1, build 479 (May-03-2011,20:57)
!
hostname AX1000
!
clock timezone Asia/Shanghai
!
!
!
trunk 1
 ethernet 1 to 2
!
trunk 2
 ethernet 3 to 4
!
vlan 2
 untagged ethernet 1 to 2
 router-interface ve 2
!

!

interface management
 ip address 172.31.31.31 255.255.255.0
!
interface ethernet 5
 ip address 172.16.120.10 255.255.255.0
!
interface ethernet 6
 disable
!
interface ethernet 7
 disable
!
interface ethernet 8
 disable
!
interface ve 2
 ip address 172.16.0.20 255.255.255.0
!
ip route 0.0.0.0 /0 172.16.0.254
!

!
!
!
!
!
!
!
!
!
!
!
ip nat pool snat_pool 172.16.0.21 172.16.0.21 netmask /24
ip nat pool snat_pool_user 172.16.0.22 172.16.0.22 netmask /24
!
!
!
!
!
!
!
!
!
!
slb server rs_172.16.0.5 172.16.0.5
   port 80  tcp
slb server rs_172.16.0.6 172.16.0.6
   port 80  tcp
slb server rs_user_1 10.0.0.24
   port 80  tcp
slb server rs_user_2 10.0.0.25
   port 80  tcp
slb server rs_user_3 10.0.0.26
   port 80  tcp
slb server rs_68 10.0.0.68
   port 80  tcp
slb server rs_69 10.0.0.69
   port 80  tcp
!
slb service-group test tcp
    member rs_172.16.0.5:80
    member rs_172.16.0.6:80
!
slb service-group sg_user tcp
    member rs_user_1:80
    member rs_user_2:80
    member rs_user_3:80
!
slb service-group nginx-group tcp
    member rs_68:80
    member rs_69:80
!
!
slb virtual-server vip_172.16.0.21 172.16.0.21
   port 80  tcp
      source-nat pool snat_pool
      service-group nginx-group
!
slb virtual-server vip_172.16.0.22 172.16.0.22
   port 80  http
      source-nat pool snat_pool_user
      service-group sg_user
!
!
!
!
!
!
!
!
!
!
!
!
enable-management service ssh ethernet 1 to 8 ve 2
enable-management service telnet management ethernet 1 to 8 ve 2
enable-management service http ethernet 1 to 8 ve 2
enable-management service https ethernet 1 to 8 ve 2
enable-management service snmp ethernet 1 to 8 ve 2
!
!
!
!
!
!
!
!
!
!
no terminal auto-size
terminal width 80
terminal length 0
!
end

```