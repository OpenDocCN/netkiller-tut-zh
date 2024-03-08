# 部分 XVII. X Window

http://www.freedesktop.org

## 第 198 章 install x window

```
# yum groupinstall "X Window System" Desktop "Desktop Platform" Font

```

修改/etc/inittab 文件 id:5:initdefault:

## 1. xinput - utility to configure and test X input devices

```

$ xinput list
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ Dell Dell USB Optical Mouse             	id=9	[slave  pointer  (2)]
⎜   ↳ SynPS/2 Synaptics TouchPad              	id=13	[slave  pointer  (2)]
⎜   ↳ TPPS/2 IBM TrackPoint                   	id=14	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Power Button                            	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Sleep Button                            	id=8	[slave  keyboard (3)]
    ↳ CNF7237&CNF7238                         	id=10	[slave  keyboard (3)]
    ↳ Asus Laptop extra buttons               	id=11	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=12	[slave  keyboard (3)]

```

## 第 199 章 X Setup

## 1. 取消开机启动画面

splash 改为 nosplash

```
sudo vim /boot/grub/menu.lst

title           Ubuntu 8.10, kernel 2.6.24-22-generic
root            (hd0,0)
kernel          /boot/vmlinuz-2.6.24-22-generic root=UUID=66320533-a53d-4740-b7f0-ed0c294802ea ro quiet splash
initrd          /boot/initrd.img-2.6.24-22-generic
quiet

```

## 2. Automatic login

```
$ sudo vim gdm.conf-custom

[daemon]
AutomaticLoginEnable=true
AutomaticLogin=neo
TimedLogin=neo

```

## 3. fonts 字体

```

# mkdir -p /usr/share/fonts/zh_CN/TrueType/
# cp -r Fonts/* /usr/share/fonts/zh_CN/TrueType/
# chmod 644 /usr/share/fonts/zh_CN/TrueType/*
# cd /usr/share/fonts/zh_CN/TrueType/
# mkfontscale
# mkfontdir
# fc-cache /usr/share/fonts/zh_CN/TrueType/

```

## 4. disable x window

```
$ sudo chmod 600 /etc/init.d/gdm

```

## 第 200 章 X Terminal

## 1. tsclient - Terminal Server Client supporting XDMCP, VNC and RDP

### 1.1. VNC

让 tsclient 支持 vnc 协议

```
sudo apt-get install xtightvncviewer

```

### 1.2. xdmcp

让 tsclient 支持 xdmcp 协议

```
sudo apt-get install xnest

```

## 2. vinagre - a remote desktop viewer for the GNOME Desktop

```
$ vinagre

```

## 3. rdesktop - A Remote Desktop Protocol client

[`www.rdesktop.org/`](http://www.rdesktop.org/)

### 3.1. -g: desktop geometry (WxH)

**$ rdesktop -g 800x600 -d 16 yourdomain.com/ip address**

```
$ rdesktop -u administrator -p zk1qFwLQWeaPfk -g 1024x768 -k en-us -z 172.16.1.3

```

常用分辨率

```
 SIF/QVGA （ 320*240 ）
     QCIF （ 176*144 ）
QSIF/QQVGA（ 160*120 ）
CIF:	352x288		10 万像素
VGA:	640x480		30 万像素（ 35 万像素是指 648*488 ）
SVGA:	800x600		50 万像素
XGA:	1024x768	80 万像素
SXGA:	1280x1024	130 万像素
		1440x900
HD:		1920x1080

```

### 3.2. -f: full-screen mode

```
rdesktop -u administrator -p password -f 172.16.0.1

```

全屏与恢复使用快捷键 Ctrl+Alt+Enter 切换

### 3.3. -A: enable SeamlessRDP mode

http://www.cendio.com/seamlessrdp/

下载 seamlessrdp.zip，并解压到 C 盘根目录下，C:\seamlessrdp，然后就登出

```
rdesktop -A -s “c:\seamlessrdp\seamlessrdpshell.exe C:\Program Files\Internet Explorer\iexplore.exe” 192.168.0.10:3389 -u administrator -p 123456
即可打开 IE

```

```
rdesktop -A -s "c:\seamlessrdp\seamlessrdpshell.exe notepad" -u administrator -p zLQWPNCc9fk -k en-us -z 172.16.0.4

```

将 QQ 的 TM 安装到 C:\TM2008 目录下，然后运行下面命令启动 QQ

```
$ rdesktop -A -s "c:\seamlessrdp\seamlessrdpshell.exe C:\TM2008\Bin\TM.exe" -u administrator -p PNCcM9 -k en-us -z 172.16.1.3

```

### 3.4. -z: enable rdp compression

```
$ rdesktop -u administrator -p zk1qFwLQ9qfk -k en-us -z 172.16.0.30

```

### 3.5. -r: enable specified device redirection (this flag can be repeated)

```
rdesktop -u administrator -p password -f -r clipboard:PRIMARYCLIPBOARD -r disk:sunray=/home/neo 172.16.0.1

```

## 4. tigervnc

http://tigervnc.org/

```
yum -y install tigervnc-server

```

设置 vnc 登录密码

```
vncpasswd

```

启动 vnc 服务

```
vncserver :1

```

启动 vnc 服务同时指定分辨率

```
vncserver :1 -geometry 800x600 -depth 24

```

停止 vnc 服务

```
vncserver -kill :1

```

## 5. TightVNC

http://www.tightvnc.com/

## 第 201 章 Unity

## 1. Enable/Disable Auto Hide For Unity 2-D Launcher In Ubuntu 11.10

```
sudo apt-get install dconf-tools
dconf list /com/canonical/unity-2d/launcher/
dconf write /com/canonical/unity-2d/launcher/use-strut true

```

## 第 202 章 X Window System

## 1. Fluxbox

http://www.fluxbox.org/

## 2. LXDE

http://www.lxde.org

## 3. Xfce

http://www.xfce.org/

## 4. Xming X Server for Windows

http://sourceforge.net/projects/xming/

## 第 203 章 X Application Software

## 1. ubuntu-restricted-extras

mp3,flash,等支持

```
sudo apt-get install ubuntu-restricted-extras

```

## 2. Keyboard Input Methods(输入法)

```
ibus-daemon -r -d -x

```

## 3. 浏览器

### Browser

### 3.1. Firefox

配置 firefox 选项

在 Firefox 的地址栏中输入 about:config

#### 3.1.1. Error code: NS_ERROR_NET_INADEQUATE_SECURITY

原因：你的 nginx 配置了 http2 (listen 443 ssl http2 default_server;) 目前 firefox 对 http2 支持还不够好。

```
(1) 在 firefox 浏览器地址栏中输入 about:config 进入配置模式

(2) 搜索 http2 选项

(3) 双击 network.http.spdy.enabled.http2 改变 true 为 false

```

### 3.2. Chromium Web Browser

内部参数调整

```
chrome://net-internals/#spdy

```

## 4. Download Software

```
- Downloader for X
- MultiGet

```

## 5. PAC Manager

https://sourceforge.net/projects/pacmanager/files/

## 6. LibreOffice

## 7. VYM (View Your Mind)

```
yum install vym

```

## 8. greenshot

http://sourceforge.net/projects/greenshot/

```
greenshot

```

## 9. Window Switch

http://winswitch.org/

## 10. gparted

```
yum install gparted

```

## 第 204 章 Office

## 1. Calc

### 1.1. 函数

字符串拼接

```
=CONCATENATE("text1";A1;"text2";D2)

```

```

="text1"&A1

```