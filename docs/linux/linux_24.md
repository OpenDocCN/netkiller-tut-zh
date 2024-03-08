# 部分 XVIII. SBC - Single-board computers

## 第 206 章 Raspberry Pi

[`www.raspberrypi.org/`](https://www.raspberrypi.org/)

## 1. 配置工具

### 1.1. rpi-update

```

sudo rpi-update			

```

## 2. WiFi 配置

### 2.1. 网络状态

```

pi@raspberrypi:~ $ networkctl 
IDX LINK             TYPE               OPERATIONAL SETUP     
  1 lo               loopback           carrier     unmanaged 
  2 eth0             ether              routable    unmanaged 
  3 wlan0            wlan               no-carrier  unmanaged 

3 links listed.

pi@raspberrypi:~ $ networkctl status
●        State: routable
       Address: 192.168.0.147 on eth0
                fe80::108e:eede:2340:564e on eth0
       Gateway: 192.168.0.1 (Tenda Technology Co.,Ltd.Dongguan branch) on eth0		

```

### 2.2. WIFI 配置

```

自动连接区域内 WIFI

sudo vi /etc/wpa_supplicant/wpa_supplicant.conf
在文件的末尾添加 WIFI 网络的名称以及密码，将要连接的 wifi 名称和密码替换即可。

network={
    ssid="SSID"
    psk="wifi_password"
}

使用 sudo wpa_cli reconfigure 命令启动连接

pi@raspberrypi:~ $ sudo wpa_cli reconfigure
Selected interface 'wlan0'
OK		

```

### 2.3. WiFi 热点配置

准备树莓派 Raspberry Pi 3 B+

```

eth0 接入本地网络
wlan0 做 WiFi 热点		

```

首先安装必须的软件，dnsmasq 是 DNS 域名解析服务。udhcpd 是 DHCP 服务，主要功能是为热点动态分配 IP 地址。hostapd 是热点服务

```

sudo apt upgrade		
sudo apt install dnsmasq hostapd udhcpd

```

#### 2.3.1. 配置网络接口

在 /etc/network/interfaces.d/ 目录中创建 wlan0 文件

```

sudo vim /etc/network/interfaces.d/wlan0

auto wlan0
iface wlan0 inet static
address 172.16.0.254
netmask 255.255.255.0

```

#### 2.3.2. 配置 DHCP

启用 DHCP

```

sudo vim /etc/default/udhcpd

DHCPD_ENABLED="no"

改为

DHCPD_ENABLED="yes"		

```

配置 DHCP 分配 IP 地址范围

```

sudo cp /etc/udhcpd.conf{,.original}		
sudo vim /etc/udhcpd.conf

start 172.16.0.20
end 172.16.0.200
interface wlan0
opt	dns	172.16.0.254
#opt	dns	8.8.8.8 4.4.4.4
option	subnet	255.255.255.0
opt	router	172.16.0.254
opt	wins	172.16.0.254
option	dns	114.114.114.114
option	domain	local
option	lease	864000		# 10 days of seconds

```

start 和 end 是分配 IP 的起始与结束范围，interface wlan0 是指定 wlan0 接口广播 DHCP，这样不会影响 eth0, 注意分配地址必须与 wlan0 在同一个网段。

opt dns 8.8.8.8 4.4.4.4 使用 Google 的 DNS，如果希望使用 DNSMASQ 需要设置为 172.16.0.254

#### 2.3.3. 配置 dnsmasq

如果使用 dnsmasq 解析域名，上面的 DHCP 需要配置 opt dns 172.16.0.254，这样 DHCP 分配地址的时候 DNS 被设置为 172.16.0.254。

```

sudo cp /etc/dnsmasq.conf{,.original}					
sudo vim /etc/dnsmasq.conf

interface=wlan0
bind-interfaces
server=8.8.8.8
server=4.4.4.4
server=114.114.114.114
domain-needed
bogus-priv
dhcp-range=172.16.0.20,172.16.0.200,12h

```

#### 2.3.4. 配置 hostapd

```

sudo vim /etc/default/hostapd

找到 

#DAEMON_CONF= ""

修改为：

DAEMON_CONF="/etc/hostapd/hostapd.conf"			

```

创建 /etc/hostapd/hostapd.conf 配置文件

```

sudo vim /etc/hostapd/hostapd.conf

# Basic configuration  
interface=wlan0
ssid=netkiller
channel=1
#bridge=br0  

# WPA and WPA2 configuration  
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=3
wpa_passphrase=13113668890
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP

# Hardware configuration  
wmm_enabled=1		

```

#### 2.3.5. 路由与转发

开启 ipv4 转发

```

sudo vim /etc/sysctl.conf

# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1			

```

转发规则

```

sudo iptables -F
sudo iptables -X
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT

```

```

sudo bash -c iptables-save > /etc/iptables.up.rules		
sudo vim /etc/network/if-pre-up.d/iptables

#!/bin/bash
/sbin/iptables-restore < /etc/iptables.up.rules

sudo chmod 755 /etc/network/if-pre-up.d/iptables			

```

#### 2.3.6. 启动热点

```

sudo systemctl restart networking
sudo systemctl restart udhcpd
sudo systemctl restart dnsmasq
sudo systemctl restart hostapd			

```

#### 2.3.7. 故障排除

如果 hostapd 启动失败，可以运行下面命令调试

```

sudo hostapd -d /etc/hostapd/hostapd.conf			

```

日志

```

$ cat /var/log/syslog | egrep "hostapd|dhcpcd"			

```

执行 iptable 提示如下

```

pi@raspberrypi:~ $ sudo iptables -L
iptables v1.6.0: can't initialize iptables table `filter': Table does not exist (do you need to insmod?)
Perhaps iptables or your kernel needs to be upgraded.			

```

解决方案

```

pi@raspberrypi:~ $ sudo rpi-update			

```

## 3. Android 9 Pie

首先下载固件 https://www.brobwind.com/wp-content/uploads/2019/03/2019_03_02_rpi3_13fa200.bin.gz

```

neo@MacBook-Pro-Neo ~/Downloads % sudo dd if=2019_03_02_rpi3_13fa200.bin of=/dev/disk2 bs=4m

```