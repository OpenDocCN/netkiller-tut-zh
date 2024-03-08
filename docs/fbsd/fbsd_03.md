# 部分 I. System Administration

## 第 2 章 Packages System

## Using the Packages System

### PACKAGESROOT / PKG_PATH 环境变量

```
setenv PACKAGESROOT ftp://ftp.freebsdchina.org
pkg_add -r libxml2 gmake autoconf262

export PKG_PATH=ftp://ftp.freebsd.org/pub/FreeBSD/8.2/packages/i386/
pkg_add mysql-server php5-fastcgi php5-gd-5.2.6-no_x11 lighttpd-1.4.19p3 nginx pecl-APC

```

### Installing a Package

```
# pkg_add -r lsof

```

### Managing Packages

```
# pkg_info
libiconv-1.13.1_1   A character set conversion library
libxml2-2.7.7       XML parser library for GNOME
lighttpd-1.4.26_3   A secure, fast, compliant, and very flexible Web Server
pcre-8.02           Perl Compatible Regular Expressions library
php5-5.3.2_1        PHP Scripting Language
pkg-config-0.23_1   A utility to retrieve information about installed libraries
spawn-fcgi-1.6.3    spawn-fcgi is used to spawn fastcgi applications

```

```
freebsd0# pkg_version
libiconv                            =
libxml2                             =
lighttpd                            =
pcre                                =
php5                                =
pkg-config                          =
spawn-fcgi                          =

#  pkg_version -v
libiconv-1.13.1_1                   =   up-to-date with port
libxml2-2.7.7                       =   up-to-date with port
lighttpd-1.4.26_3                   =   up-to-date with port
pcre-8.02                           =   up-to-date with port
php5-5.3.2_1                        =   up-to-date with port
php5-ctype-5.3.2_1                  =   up-to-date with port
php5-dom-5.3.2_1                    =   up-to-date with port
php5-extensions-1.4                 =   up-to-date with port
php5-filter-5.3.2_1                 =   up-to-date with port
php5-hash-5.3.2_1                   =   up-to-date with port
php5-iconv-5.3.2_1                  =   up-to-date with port
php5-json-5.3.2_1                   =   up-to-date with port
php5-pdo-5.3.2_1                    =   up-to-date with port
php5-pdo_sqlite-5.3.2_1             =   up-to-date with port
php5-posix-5.3.2_1                  =   up-to-date with port
php5-session-5.3.2_1                =   up-to-date with port
php5-simplexml-5.3.2_1              =   up-to-date with port
php5-sqlite-5.3.2_1                 =   up-to-date with port
php5-tokenizer-5.3.2_1              =   up-to-date with port
php5-xml-5.3.2_1                    =   up-to-date with port
php5-xmlreader-5.3.2_1              =   up-to-date with port
php5-xmlwriter-5.3.2_1              =   up-to-date with port
pkg-config-0.23_1                   =   up-to-date with port
spawn-fcgi-1.6.3                    =   up-to-date with port
sqlite3-3.6.23.1_1                  =   up-to-date with port

```

### Deleting a Package

```
# pkg_delete php5-5.3.2_1

```

## Updating and Upgrading FreeBSD

### update

安全补丁

```
# freebsd-update fetch
# freebsd-update install

```

如果出现错误回滚

```
# freebsd-update rollback

```

```
freebsd# rehash
freebsd# freebsd-update fetch
Looking up update.FreeBSD.org mirrors... 3 mirrors found.
Fetching public key from update5.FreeBSD.org... done.
Fetching metadata signature for 8.0-RELEASE from update5.FreeBSD.org... done.
Fetching metadata index... done.
Fetching 2 metadata files... done.
Inspecting system... done.
Preparing to download files... done.
Fetching 35 patches.....10....20....30.. done.
Applying patches... done.

The following files will be updated as part of updating to 8.0-RELEASE-p3:
/boot/kernel/ip_mroute.ko
/boot/kernel/ip_mroute.ko.symbols
/boot/kernel/kernel
/boot/kernel/kernel.symbols
/boot/kernel/krpc.ko
/boot/kernel/krpc.ko.symbols
/boot/kernel/nfsclient.ko
/boot/kernel/nfsclient.ko.symbols
/boot/kernel/zfs.ko
/boot/kernel/zfs.ko.symbols
/etc/mtree/BSD.var.dist
/lib/libzpool.so.2
/libexec/ld-elf.so.1
/usr/bin/dig
/usr/bin/host
/usr/bin/nslookup
/usr/bin/nsupdate
/usr/lib/libopie.a
/usr/lib/libopie.so.6
/usr/lib/libssl.a
/usr/lib/libssl.so.6
/usr/lib/libzpool.a
/usr/sbin/dnssec-dsfromkey
/usr/sbin/dnssec-keyfromlabel
/usr/sbin/dnssec-keygen
/usr/sbin/dnssec-signzone
/usr/sbin/freebsd-update
/usr/sbin/jail
/usr/sbin/lwresd
/usr/sbin/named
/usr/sbin/named-checkconf
/usr/sbin/named-checkzone
/usr/sbin/named-compilezone
/usr/sbin/ntpd
/usr/sbin/rndc-confgen
/usr/share/man/man2/mount.2.gz
/usr/share/man/man2/nmount.2.gz
/usr/share/man/man2/unmount.2.gz
/var/db/freebsd-update
/var/db/mergemaster.mtree

freebsd# freebsd-update install
Installing updates... done.

```

### upgrade

寻找版本

http://update5.freebsd.org/

升级至 9.1-RELEASE

```
freebsd# freebsd-update upgrade -r 9.1-RELEASE

```

```
Looking up update.FreeBSD.org mirrors... 3 mirrors found.
Fetching metadata signature for 9.1-RC3 from update5.freebsd.org... done.
Fetching metadata index... done.
Fetching 1 metadata files... done.
Inspecting system... done.

The following components of FreeBSD seem to be installed:
kernel/generic world/base world/games world/lib32

The following components of FreeBSD do not seem to be installed:
src/src world/doc

Does this look reasonable (y/n)? y

Fetching metadata signature for 9.1-RELEASE from update5.freebsd.org... done.
Fetching metadata index... done.
Fetching 1 metadata patches. done.
Applying metadata patches... done.
Fetching 1 metadata files... done.
Inspecting system... done.
Fetching files from 9.1-RC3 for merging... done.
Preparing to download files...

```

```
freebsd# freebsd-update install

```

## ports

### update ports

update your port tree by typing following commands:

```
# portsnap fetch
# portsnap extract
# portsnap update

```

首次运行 portsnap 需要执行 portsnap extract 这个步骤，之后不再需要,直接运行 portsnap update 即可

### install

```
# cd /usr/ports/lang/php5
# make install

```

reinstall

```
# make reinstall

```

### remove

```
# make deinstall

```

如果用 make install clean 安装，请用 pkg_delete 卸载

### config menu

```
# cd /usr/ports/lang/php5
# make config
# make install

```

## PKGNG - Pkgng is the Next Generation package management tool for FreeBSD.

https://mebsd.com/make-build-your-freebsd-word/pkgng-first-look-at-freebsds-new-package-manager.html

http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/pkgng-intro.html

https://wiki.freebsd.org/pkgng

### 安装 pkgng

方法一：

```

# /usr/sbin/pkg
The package management tool is not yet installed on your system.
Do you want to fetch and install it now? [y/N]: y
Bootstrapping pkg please wait
Installing pkg-1.0.11... done
If you are upgrading from the old package format, first run:

  # pkg2ng
usage: pkg [-v] [-d] [-j <jail name or id>|-c <chroot path>] <command> [<args>]

Global options supported:
	-d             Increment debug level
	-j             Execute pkg(1) inside a jail(8)
	-c             Execute pkg(1) inside a chroot(8)
	-v             Display pkg(1) version

Commands supported:
	add            Registers a package and installs it on the system
	audit          Reports vulnerable packages
	autoremove     Removes orphan packages
	backup         Backs-up and restores the local package database
	check          Checks for missing dependencies and database consistency
	clean          Cleans old packages from the cache
	create         Creates software package distributions
	delete         Deletes packages from the database and the system
	fetch          Fetches packages from a remote repository
	help           Displays help information
	info           Displays information about installed packages
	install        Installs packages from remote package repositories
	query          Queries information about installed packages
	register       Registers a package into the local database
	remove         Deletes packages from the database and the system
	repo           Creates a package repository catalogue
	rquery         Queries information in repository catalogues
	search         Performs a search of package repository catalogues
	set            Modifies information about packages in the local database
	shell          Opens a debug shell
	shlib          Displays which packages link against a specific shared library
	stats          Displays package database statistics
	update         Updates package repository catalogues
	updating       Displays UPDATING information for a package
	upgrade        Performs upgrades of packaged software distributions
	version        Displays the versions of installed packages
	which          Displays which package installed a specific file

For more information on the different commands see 'pkg help <command>'.

```

方法二：

```
# portsnap fetch update
# cd /usr/ports/ports-mgmt/pkg
# make
# make install clean

```

方法三

```
# pkg_add -r pkg

```

### 转换数据库

运行 pkg2ng 命令

```
# pkg2ng

```

设置 make.conf 文件

```

cat >> /etc/make.conf <<EOF

WITH_PKGNG=yes
EOF

```

### install

安装包

```
# pkg install curl

```

### delete

删除包

```
# pkg delete curl

```

### info

显示所有包信息

```
pkg info

```

显示包之间的依赖关系

```
# pkg info -d subversion
subversion-1.7.7 depends on:
expat-2.0.1_2
pkgconf-0.8.9
sqlite3-3.7.14.1
gdbm-1.9.1
db42-4.2.52_5
libiconv-1.14
gettext-0.18.1.1

```

显示包中的文件

```
# pkg info -l subversion
subversion-1.7.7 owns the following files:
/usr/local/bin/svn
/usr/local/bin/svnadmin
/usr/local/bin/svndumpfilter
/usr/local/bin/svnlook
/usr/local/bin/svnrdump
/usr/local/bin/svnserve
/usr/local/bin/svnsync
/usr/local/bin/svnversion
/usr/local/etc/rc.d/svnserve

```

### query

```
# pkg query "Package name = %n, Version = %v, Size = %sh" libxml2
Package name = libxml2, Version = 2.7.8_5, Size = 4 MB

# pkg query -a "Package name = %n, Version = %v, Size = %sh"

```

```

# pkg query "\
	package[%n]\n\
	version[%v]\n\
	origin[%o]\n\
	prefix[%p]\n\
	maintainer[%m]\n\
	comment[%c]\n\
	www[%w]\n\
	licenselogic[%l]\n\
	flatsize[%sh]\n\
	flatsizebytes[%sb]\n\
	orphan[%a]\n\
	message[%M]\n\
" libxml2

typescript package[libxml2]

typescript version[2.7.8_5]

typescript origin[textproc/libxml2]

typescript prefix[/usr/local]

typescript maintainer[gnome@FreeBSD.org]

typescript comment[XML parser library for GNOME]

typescript www[http://xmlsoft.org/]

typescript licenselogic[single]

typescript flatsize[4 MB]

typescript flatsizebytes[4258960]

typescript orphan[0]

typescript message[]

```

### upgrade

```
# pkg upgrade

```

## FAQ

### Shared object "libarchive.so.5" not found, required by "pkg"

问题出现在从 FreeBSD 9.2 升级到 FreeBSD 10.0，升级 pkg 可以解决

```
# pkg-static upgrade

```

### pkg: PACKAGESITE in pkg.conf is deprecated. Please create a repository configuration file

编辑下面的文件

```
# cat /usr/local/etc/pkg.conf                                                                                                                                                                                                                           [0]
#packagesite: http://pkg.FreeBSD.org/freebsd:9:x86:64/latest
PACKAGESITE	    : http://pkg.freebsd.org/${ABI}/latest

```

到 http://pkg.freebsd.org/ 查找适合你版本的 URL

```
# cat /usr/local/etc/pkg.conf                                                                                                                                                                                                                           [0]
#packagesite: http://pkg.FreeBSD.org/freebsd:9:x86:64/latest
#PACKAGESITE	    : http://pkg.freebsd.org/${ABI}/latest
PACKAGESITE	    : http://pkg.freebsd.org/freebsd:10:x86:64/latest			

```

更新 pkg

```
pkg update			

```

## 第 3 章 编辑器 (editors)

## vim

```
# pkg_add -r vim

```

```
# cd /usr/ports/editors/vim
# make install

```

## 第 4 章 terminal

## 分辨率

使用下面的命令获得本机支持的显示模式

```

vidcontrol -i mode < /dev/ttyv0

```

```

root@netkiller:/root # vidcontrol -i mode < /dev/ttyv0
    mode#     flags   type    size       font      window      linear buffer
------------------------------------------------------------------------------
  0 (0x000) 0x00000001 T 40x25           8x8   0xb8000 32k 32k 0x00000000 32k
  1 (0x001) 0x00000001 T 40x25           8x8   0xb8000 32k 32k 0x00000000 32k
  2 (0x002) 0x00000001 T 80x25           8x8   0xb8000 32k 32k 0x00000000 32k
  3 (0x003) 0x00000001 T 80x25           8x8   0xb8000 32k 32k 0x00000000 32k
  4 (0x004) 0x00000003 G 320x200x2 C     8x8   0xb8000 32k 32k 0x00000000 32k
  5 (0x005) 0x00000003 G 320x200x2 C     8x8   0xb8000 32k 32k 0x00000000 32k
  6 (0x006) 0x00000003 G 640x200x1 C     8x8   0xb8000 32k 32k 0x00000000 32k
 13 (0x00d) 0x00000003 G 320x200x4 4     8x8   0xa0000 64k 64k 0x00000000 256k
 14 (0x00e) 0x00000003 G 640x200x4 4     8x8   0xa0000 64k 64k 0x00000000 256k
 16 (0x010) 0x00000003 G 640x350x2 2     8x14  0xa0000 64k 64k 0x00000000 128k
 18 (0x012) 0x00000003 G 640x350x4 4     8x14  0xa0000 64k 64k 0x00000000 256k
 19 (0x013) 0x00000001 T 40x25           8x14  0xb8000 32k 32k 0x00000000 32k
 20 (0x014) 0x00000001 T 40x25           8x14  0xb8000 32k 32k 0x00000000 32k
 21 (0x015) 0x00000001 T 80x25           8x14  0xb8000 32k 32k 0x00000000 32k
 22 (0x016) 0x00000001 T 80x25           8x14  0xb8000 32k 32k 0x00000000 32k
 23 (0x017) 0x00000001 T 40x25           8x16  0xb8000 32k 32k 0x00000000 32k
 24 (0x018) 0x00000001 T 80x25           8x16  0xb8000 32k 32k 0x00000000 32k
 26 (0x01a) 0x00000003 G 640x480x4 4     8x16  0xa0000 64k 64k 0x00000000 256k
 27 (0x01b) 0x00000003 G 640x480x4 4     8x16  0xa0000 64k 64k 0x00000000 256k
 28 (0x01c) 0x00000003 G 320x200x8 P     8x8   0xa0000 64k 64k 0x00000000 64k
 30 (0x01e) 0x00000001 T 80x50           8x8   0xb8000 32k 32k 0x00000000 32k
 32 (0x020) 0x00000001 T 80x30           8x16  0xb8000 32k 32k 0x00000000 32k
 34 (0x022) 0x00000001 T 80x60           8x8   0xb8000 32k 32k 0x00000000 32k
 37 (0x025) 0x00000003 G 320x240x8 V     8x8   0xa0000 64k 64k 0x00000000 256k
112 (0x070) 0x00000000 T 80x43           8x8   0xb8000 32k 32k 0x00000000 32k
113 (0x071) 0x00000001 T 80x43           8x8   0xb8000 32k 32k 0x00000000 32k
257 (0x101) 0x0000000f G 640x480x8 P     8x16  0xa0000 64k 64k 0x80000000 300k
259 (0x103) 0x0000000f G 800x600x8 P     8x16  0xa0000 64k 64k 0x80000000 487k
261 (0x105) 0x0000000f G 1024x768x8 P    8x16  0xa0000 64k 64k 0x80000000 768k
273 (0x111) 0x0000000f G 640x480x16 D    8x16  0xa0000 64k 64k 0x80000000 600k
274 (0x112) 0x0000000f G 640x480x32 D    8x16  0xa0000 64k 64k 0x80000000 1200k
276 (0x114) 0x0000000f G 800x600x16 D    8x16  0xa0000 64k 64k 0x80000000 937k
277 (0x115) 0x0000000f G 800x600x32 D    8x16  0xa0000 64k 64k 0x80000000 1875k
279 (0x117) 0x0000000f G 1024x768x16 D   8x16  0xa0000 64k 64k 0x80000000 1536k
280 (0x118) 0x0000000f G 1024x768x32 D   8x16  0xa0000 64k 64k 0x80000000 3072k

```

使用下面命令改变当前屏幕的分辨率 280 为 1024x768x32

```
# vidcontrol MODE_280

```

启动时生效，就在 /etc/rc.conf 加入

```
allscreens_flags="MODE_280"

```

## 屏幕保护

方法一，编辑 /etc/rc.conf 文件加入

```
blanktime=”60″
saver=”daemon”

```

方法二，sysinstall

```

选择
Configure——Console——Saver—–Timeout（设置屏保时间 60 秒）
在选则 Daemon

```

屏保文件

```
# ls /boot/kernel/*saver* | grep -v symbols
/boot/kernel/beastie_saver.ko
/boot/kernel/blank_saver.ko
/boot/kernel/daemon_saver.ko
/boot/kernel/dragon_saver.ko
/boot/kernel/fade_saver.ko
/boot/kernel/fire_saver.ko
/boot/kernel/green_saver.ko
/boot/kernel/logo_saver.ko
/boot/kernel/rain_saver.ko
/boot/kernel/snake_saver.ko
/boot/kernel/star_saver.ko
/boot/kernel/warp_saver.ko

```

屏保预览

```
# kldload logo_saver
# kldload fire_saver
# kldload rain_saver

```

kldload 不能重复运行，已经载入的屏保不能再重新载入，使用下面命令查看详情。

```
# kldstat
Id Refs Address            Size     Name
 1   22 0xffffffff80200000 1323388  kernel
 5    1 0xffffffff81612000 861      snake_saver.ko
 6    1 0xffffffff81613000 a2d      fire_saver.ko
 7    1 0xffffffff81614000 e89      dragon_saver.ko
 8    1 0xffffffff81615000 11ad     daemon_saver.ko
 9    1 0xffffffff81617000 4dd      star_saver.ko
10    1 0xffffffff81618000 9cd      rain_saver.ko
11    1 0xffffffff81619000 c4d      warp_saver.ko

```

## 键盘设置

使用 linux 的用户转到 BSD 很不适应终端键盘设置

```

bindkey "^[[1~" beginning-of-line
bindkey "^[[4~" end-of-line
bindkey "^[[3~" delete-char

```

## Unicode(UTF-8)

显示当前设置

```
% locale                                                                                                                                                                                                                                                 [0]
LANG=
LC_CTYPE="C"
LC_COLLATE="C"
LC_TIME="C"
LC_NUMERIC="C"
LC_MONETARY="C"
LC_MESSAGES="C"
LC_ALL=

```

列出所有可用的公共语言环境的名称，通常我们只关心 zh_CN.UTF-8、zh_HK.UTF-8、zh_TW.UTF-8

```
% locale -a | grep '\.UTF-8$'                                                                                                                                                                                                                            [1]
af_ZA.UTF-8
am_ET.UTF-8
be_BY.UTF-8
bg_BG.UTF-8
ca_AD.UTF-8
ca_ES.UTF-8
ca_FR.UTF-8
ca_IT.UTF-8
cs_CZ.UTF-8
da_DK.UTF-8
de_AT.UTF-8
de_CH.UTF-8
de_DE.UTF-8
el_GR.UTF-8
en_AU.UTF-8
en_CA.UTF-8
en_GB.UTF-8
en_IE.UTF-8
en_NZ.UTF-8
en_US.UTF-8
es_ES.UTF-8
et_EE.UTF-8
eu_ES.UTF-8
fi_FI.UTF-8
fr_BE.UTF-8
fr_CA.UTF-8
fr_CH.UTF-8
fr_FR.UTF-8
he_IL.UTF-8
hr_HR.UTF-8
hu_HU.UTF-8
hy_AM.UTF-8
is_IS.UTF-8
it_CH.UTF-8
it_IT.UTF-8
ja_JP.UTF-8
kk_KZ.UTF-8
ko_KR.UTF-8
lt_LT.UTF-8
lv_LV.UTF-8
mn_MN.UTF-8
nb_NO.UTF-8
nl_BE.UTF-8
nl_NL.UTF-8
nn_NO.UTF-8
no_NO.UTF-8
pl_PL.UTF-8
pt_BR.UTF-8
pt_PT.UTF-8
ro_RO.UTF-8
ru_RU.UTF-8
sk_SK.UTF-8
sl_SI.UTF-8
sr_YU.UTF-8
sv_SE.UTF-8
tr_TR.UTF-8
uk_UA.UTF-8
zh_CN.UTF-8
zh_HK.UTF-8
zh_TW.UTF-8

```

~/.login_conf 文件的作用是，当你登陆系统后会改变你的终端设置

```

% cat ~/.login_conf                                                                                                                                                                                                                                      [0]
# $FreeBSD: release/9.2.0/share/skel/dot.login_conf 77995 2001-06-10 17:08:53Z ache $
#
# see login.conf(5)
#
#me:\
#	:charset=iso-8859-1:\
#	:lang=de_DE.ISO8859-1:

```

设置 UTF-8 支持

```

% cat >> ~/.login_conf <<'EOF'

me:\
	:charset=UTF-8:\
	:lang=en_US.UTF-8:
EOF

```

## 第 5 章 FreeBSD 开机启动

将开机运行脚本写入

```
/etc/rc.local

```

将关机前运行脚本写入

```
/etc/rc.shutdown.local

```

测试

```
/etc/rc.d/local start
/etc/rc.d/local stop

```

## /etc/rc.conf

修改 /etc/rc.conf 生效

```
sh /etc/rc

```

## 第 6 章 Date

```
# date -v -1d +%Y%m%d
20060326
# date -v -1m +%Y%m%d
20060227
# date -v -1y +%Y%m%d
20050327

```

## 第 7 章 shell

## Bash

install bash

```
freebsd# cd /usr/ports/shells/bash
freebsd# make install clean

```

change shell

```
$ chsh -s /usr/local/bin/bash
Password:
chsh: user information updated
$ exit

```

## zsh

```
freebsd# pkg_add -r zsh
freebsd# chsh -s /usr/local/bin/zsh

```

### 初始化 zsh

```
# compinstall -Uz compinit
# compinit
# compinstall

```

### prompt

zsh prompt colors:

```
autoload colors; colors
export PS1="%B[%{$fg[red]%}%n%{$reset_color%}%b@%B%{$fg[cyan]%}%m%b%{$reset_color%}:%~%B]%b "

```

## 第 8 章 Users and Basic Account Management

## 添加用户

```
# pw useradd www
# passwd www

```

添加新用户 jerry 并允许他 su 到 root 用户

```
# pw useradd jerry -G wheel
# passwd jerry

```

```
# pw useradd -n tom -s /bin/csh -m
# passwd tom

```

## 删除用户

rmuser 命令

```
root@freebsd:~ # pw adduser test
root@freebsd:~ # rmuser test
Matching password entry:

test:*:1002:1002::0:0:User :/home/test:/bin/sh

Is this the entry you wish to remove? y
Remove user's home directory (/home/test)? y
Removing user (test): mailspool home passwd.

```

pw userdel 命令

```
root@freebsd:~ # pw adduser test
root@freebsd:~ # pw userdel test

```

```
# pw userdel -n tom -r

```

## 修改用户信息

```
root@freebsd:~ # chpass neo
#Changing user information for neo.
Login: neo
Password: $6$d9faZkqxd3xQ5AqB$hqAXYv4xbZKz11AHqrva8WoCDstruFmqW2d3Lhzu.9s36tuAiqVfOaNSy9mDFP5I.VaKWhQUjUEgiXF183uVm0
Uid [#]: 1001
Gid [# or name]: 1001
Change [month day year]:
Expire [month day year]:
Class:
Home directory: /home/neo
Shell: /bin/sh
Full Name: neo chen
Office Location:
Office Phone:
Home Phone:
Other information:

```

## su - root

add a user to wheel group

```

root@freebsd:~ # pw usermod neo -G wheel
root@freebsd:~ # id neo
uid=1001(neo) gid=1001(neo) groups=1001(neo),0(wheel)

freebsd# grep 'wheel' /etc/group
wheel:*:0:root,neo

```

## profile

### shell

```
Finally, change your account to use zsh as your default shell
FreeBSD users:
# chpass -s /usr/local/bin/zsh

```

### Changing user information

```
#Changing user information for neo.
Shell: /usr/local/bin/zsh
Full Name: Neo Chen
Office Location:
Office Phone:
Home Phone:
Other information:

```

## passwd file

```
freebsd 不能直接改/etc/passwd 需要使用 vipw，否则不生效
```

```
# vipw

```

## 第 9 章 Network

```
vi /etc/rc.conf

```

## DHCP

```
hostname="freebse.example.com"
ifconfig_em0="DHCP"

```

## IP Address

```
hostname="freebse.example.com"
ifconfig_em0="inet 192.168.3.71  netmask 255.255.255.0"
defaultrouter="192.168.3.1"

```

## alias

```

[root@freebsd0:~] ifconfig bce0 alias 172.16.1.81 netmask 255.255.255.0
[root@freebsd0:~] ifconfig bce0
bce0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=c01bb<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,JUMBO_MTU,VLAN_HWCSUM,TSO4,VLAN_HWTSO,LINKSTATE>
        ether 00:22:19:5f:52:0b
        inet 172.16.1.27 netmask 0xffffff00 broadcast 172.16.1.255
        inet 172.16.1.81 netmask 0xffffff00 broadcast 172.16.1.255
        media: Ethernet autoselect (100baseTX <full-duplex>)
        status: active

vim /etc/rc.conf
ifconfig_bce0_alias0="inet 172.16.1.81 netmask 255.255.255.0"

```

## Default Gateway

```
defaultrouter="192.168.3.1"

```

## DNS

```
freebse# cat /etc/resolv.conf
domain  example.com
nameserver      202.96.134.133

```

## netstart

restart network

```
/etc/netstart

```

## Route

route table

```
netstat -nr

```

Default Routes

```
freebsd# route add  default 172.16.0.254
add net default: gateway 172.16.0.254

```

添加静态路由

```
route add 172.16.3.0/24 172.16.1.240

```

编辑/etc/rc.conf 使上面命令开机时执行

```
static_routes="static1"
route_static1="-net 172.16.3.0/24 172.16.1.240"

```

## 第 10 章 Debug

## lsof - list open files

```
[root@freebsd:~] pkg_add -r lsof
Fetching ftp://ftp.freebsd.org/pub/FreeBSD/ports/amd64/packages-8.1-release/Latest/lsof.tbz... Done.

```