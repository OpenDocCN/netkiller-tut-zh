# 部分 VI. Backup, Recovery, and Archiving Solutions

## File Transfer, Synchronize, Storage

## 第 67 章 Logical Volume Manager (LVM)

vg,lv 命名规则，建议采用：

1\. /dev/vg00/lvol00

2\. /dev/VolGroup00/LogVol00

lvm 创建流程 pvcreate - vgcreate - lvcreate

```
# pvcreate /dev/sdb4
  Physical volume "/dev/sdb4" successfully created

# vgcreate vg1 /dev/sdb4
  Volume group "vg1" successfully created

# lvcreate -l 10239 -n lv0 vg1
  Logical volume "lv0" created

```

## 1. 物理卷管理（physical volume）

### 1.1. pvcreate

将整个硬盘划为物理卷

```
# pvcreate /dev/hdb

```

将单个分区创建为物理卷的命令为：

```
# pvcreate /dev/hda5

```

实例

```
# pvcreate /dev/sdb4
  Physical volume "/dev/sdb4" successfully created

```

### 1.2. pvdisplay

```
# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sdb4
  VG Name               vg1
  PV Size               1.02 TiB / not usable 4.90 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              267301
  Free PE               257062
  Allocated PE          10239
  PV UUID               g2xLQ8-7tgm-iNZc-8dVq-vo3z-CFJp-LryYAs

```

### 1.3. pvs

```
# pvs
  PV         VG   Fmt  Attr PSize PFree
  /dev/sdb4  vg1  lvm2 a-   1.02t 1004.15g

```

## 2. 卷组管理（Volume Group）

### 2.1. vgcreate

```
# vgcreate vg1 /dev/sdb4
  Volume group "vg1" successfully created

```

### 2.2. vgdisplay

```
# vgdisplay
  --- Volume group ---
  VG Name               vg1
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1.02 TiB
  PE Size               4.00 MiB
  Total PE              267301
  Alloc PE / Size       10239 / 40.00 GiB
  Free  PE / Size       257062 / 1004.15 GiB
  VG UUID               Kxd02t-mFtJ-nThA-Lciy-zI2A-Dwzq-2nJoVh

```

### 2.3. vgs

```
# vgs
  VG   #PV #LV #SN Attr   VSize VFree
  vg1    1   1   0 wz--n- 1.02t 1004.15g

```

### 2.4. vgchange

激活卷组

```
# vgchange -a y vg1

```

### 2.5. vgextend

```
vgextend vg1 /dev/sdb3

```

```
# vgdisplay
  --- Volume group ---
  VG Name               vg1
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1.02 TiB
  PE Size               4.00 MiB
  Total PE              267301
  Alloc PE / Size       10239 / 40.00 GiB
  Free  PE / Size       257062 / 1004.15 GiB
  VG UUID               Kxd02t-mFtJ-nThA-Lciy-zI2A-Dwzq-2nJoVh

# vgs
  VG   #PV #LV #SN Attr   VSize VFree
  vg1    1   1   0 wz--n- 1.02t 1004.15g

# vgextend vg1 /dev/sdb3
  No physical volume label read from /dev/sdb3
  Physical volume "/dev/sdb3" successfully created
  Volume group "vg1" successfully extended

# vgdisplay
  --- Volume group ---
  VG Name               vg1
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               1.51 TiB
  PE Size               4.00 MiB
  Total PE              395303
  Alloc PE / Size       10239 / 40.00 GiB
  Free  PE / Size       385064 / 1.47 TiB
  VG UUID               Kxd02t-mFtJ-nThA-Lciy-zI2A-Dwzq-2nJoVh

# vgs
  VG   #PV #LV #SN Attr   VSize VFree
  vg1    2   1   0 wz--n- 1.51t 1.47t

  # pvdisplay
  --- Physical volume ---
  PV Name               /dev/sdb4
  VG Name               vg1
  PV Size               1.02 TiB / not usable 4.90 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              267301
  Free PE               257062
  Allocated PE          10239
  PV UUID               g2xLQ8-7tgm-iNZc-8dVq-vo3z-CFJp-LryYAs

  --- Physical volume ---
  PV Name               /dev/sdb3
  VG Name               vg1
  PV Size               500.01 GiB / not usable 1.12 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              128002
  Free PE               128002
  Allocated PE          0
  PV UUID               77RRJm-e4iz-Zfos-ZYHT-XEBa-AZ7D-Yd7fdU

```

### 2.6. vgreduce

```
# vgreduce vg1 /dev/sdb3
  Removed "/dev/sdb3" from volume group "vg1"

# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sdb4
  VG Name               vg1
  PV Size               1.02 TiB / not usable 4.90 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              267301
  Free PE               257062
  Allocated PE          10239
  PV UUID               g2xLQ8-7tgm-iNZc-8dVq-vo3z-CFJp-LryYAs

  "/dev/sdb3" is a new physical volume of "500.01 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb3
  VG Name
  PV Size               500.01 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               77RRJm-e4iz-Zfos-ZYHT-XEBa-AZ7D-Yd7fdU

```

## 3. 逻辑卷管理（logical volume）

### 3.1. lvcreate

创建 1000M 逻辑卷

```
# lvcreate -l 1000 -n lv0 vg1
  Logical volume "lv0" created

# ls /dev/vg1/lv0

```

使用-L 参数

```
# lvcreate -L 100G -n lv3 vg1
  Logical volume "lv3" created

```

#### 3.1.1. snapshot

```
# lvcreate --size 16m --snapshot --name snap0 /dev/vg1/lv0
  Logical volume "snap0" created

# find /dev/vg1/
/dev/vg1/
/dev/vg1/snap0
/dev/vg1/lv3
/dev/vg1/lv1
/dev/vg1/lv0

```

### 3.2. lvdisplay

```
# lvdisplay
  --- Logical volume ---
  LV Name                /dev/vg1/lv0
  VG Name                vg1
  LV UUID                DyvPgz-VFjs-gu58-mxNX-ybCm-tcUP-kKk90y
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                40.00 GiB
  Current LE             10239
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Name                /dev/vg1/lv1
  VG Name                vg1
  LV UUID                8Nbuio-w2CH-euVD-9LNB-3Dcd-frS0-Cm3EBD
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                3.91 GiB
  Current LE             1000
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1

```

### 3.3. lvremove

```
# lvcreate -l 1000 -n lv1 vg1
  Logical volume "lv1" created

# lvdisplay
  --- Logical volume ---
  LV Name                /dev/vg1/lv0
  VG Name                vg1
  LV UUID                DyvPgz-VFjs-gu58-mxNX-ybCm-tcUP-kKk90y
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                40.00 GiB
  Current LE             10239
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Name                /dev/vg1/lv1
  VG Name                vg1
  LV UUID                8Nbuio-w2CH-euVD-9LNB-3Dcd-frS0-Cm3EBD
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                3.91 GiB
  Current LE             1000
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1

# lvremove /dev/vg1/lv1
Do you really want to remove active logical volume lv1? [y/n]: y
  Logical volume "lv1" successfully removed

# lvdisplay
  --- Logical volume ---
  LV Name                /dev/vg1/lv0
  VG Name                vg1
  LV UUID                DyvPgz-VFjs-gu58-mxNX-ybCm-tcUP-kKk90y
  LV Write Access        read/write
  LV Status              available
  # open                 0
  LV Size                40.00 GiB
  Current LE             10239
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

```

#### 3.3.1. snapshot

```
# lvremove /dev/vg1/snap0
Do you really want to remove active logical volume snap0? [y/n]: y
  Logical volume "snap0" successfully removed

```

## 4. Format

```
# mkfs.ext4 /dev/vg1/lv0
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
2621440 inodes, 10484736 blocks
524236 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
320 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 24 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.

```

## 5. mount

### 5.1. lv

```
# mkdir /mnt/lv0
# mount /dev/vg1/lv0 /mnt/lv0

```

### 5.2. snapshot

```
# find /dev/vg1/
/dev/vg1/
/dev/vg1/snap0
/dev/vg1/lv3
/dev/vg1/lv1
/dev/vg1/lv0

# mkdir /mnt/snap0
# mount /dev/vg1/snap0 /mnt/snap0

```

## 6. snapshot backup

dump + restore

```
1.挂载备份源 www
mount /dev/vg1/www /www

2.创建快照
lvcreate -L 16m -p r -s -n www-backup /dev/vg1/www

3.挂载快照
mkdir /mnt/www-backup
mount -o ro /dev/vg1/www-backup /mnt/www-backup

4.备份快照
dump -0u -f /tmp/www-backup.dump /mnt/www-backup

5.删除快照
umount /mnt/www-backup
lvremove /dev/vg1/www-backup

6.重做 www
umount /www
mkfs.ext4 /dev/vg1/www
mount /dev/vg1/www /www

7.恢复快照
cd /www
restore -rf /tmp/www-backup.dump

```

dd

```
# mount -o remount,ro /dev/VolGroup00/LogVol01
# lvcreate -L500M -s -n backup /dev/VolGroup00/LogVol01
# dd if=/dev/VolGroup00/backup of=/mnt/VolGroup01/LogVol01/
# mount -o remount,rw /dev/VolGroup00/LogVol01
# umount /mnt/VolGroup01/LogVol01
# lvremove /dev/VolGroup00/backup

```

## 第 68 章 文件传输

## 1. 跨服务器文件传输

### 1.1. scp - secure copy (remote file copy program)

限速 1M

```
# scp -l 1000 /www/index.html root@172.16.0.1:/www

```

指定 identity_file 文件

```
scp -i /path/to/id_dsa user@host:/path/to/ceph.conf $conf

```

### 1.2. nc - TCP/IP swiss army knife

tar 通过 nc 发送到另一端

```
# Server
$ tar cf - win98 | nc -l -p 5555

# Backup Machine
nc server_ip/server_doman_name 5555 | tar xf -

```

## 2. wget - retrieves files from the web

```
setlocal ENABLEDELAYEDEXPANSION
for /l %%i in (1001,1,1162) do for /l %%j in (101,1,112) do @(
                         set s=%%i
                         set t=%%j
                         wget -O !s:~1,3!!t:~1,2!.jpg hxxp://www.sergeaura.net/TGP/!s:~1,3!/images/!t:~1,2!.jpg)
endlocal

```

-np 的作用是不遍历父目录

-nd 不重新创建目录结构。

--accept=iso 仅下载所有扩展名为 iso 的文件

-i filename.txt 此命令常用于批量下载的情形，把所有需要下载文件的地址放到 filename.txt 中，然后 wget 就会自动为你下载所有文件了。

-c 选项的作用为断点续传。

$ wget -m -k (-H) http://www.example.com/ 该命令可用来镜像一个网站，wget 将对链接进行转换。如果网站中的图像是放在另外的站点，那么可以使用 -H 选项。

### 2.1. 下载所有图片

```
wget --reject=htm,html,txt --accept=jpg,gif -p -m -H http://www.example.com
wget --domains=example.com --reject=htm,html,txt --accept=jpg,gif -p -m -H http://www.example.com

```

### 2.2. mirror

```
wget -m -e robots=off http://www.example.com/

wget -m -e robots=off -U "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6″ "http://www.example.com/"

```

### 2.3. reject

```
wget -m --reject=gif http://target.web.site/subdirectory

```

### 2.4. ftp 下载

```
wget -q -c -m -P /backup/logs/cdn -nH  ftp://user:passwd@localhost/

```

## 3. axel - A light download accelerator - Console version

axel

```
sudo apt-get install axel

```

## 第 69 章 FTP (File Transfer Protocol)

参考 http://netkiller.8800.org/article/ftpserver/

## 1. lftp

### 1.1. pget

多线程下载

```
lftp -c 'pget http://url.example.com/file.ext' # 默认 5 个线程
lftp -c 'pget -n 10 http://url.example.com/file.ext'

```

### 1.2. lftp 批处理

```

lftp $HOSTADDR<<FTPCMD
cd $REMOTEPATH
lcd $DESTPATH
nlist > $DYNAFILE
quit
FTPCMD

```

## 2. ncftp

```
sudo apt-get install ncftp
ncftp ftp://neo@127.0.0.1

```

### 2.1. batch command

batch ftp command

```

neo@netkiller:~$ cat upload
#!/bin/bash

ncftp ftp://netkiller:******@netkiller.hikz.com/www/book/linux <<END_SCRIPT
put /home/neo/workspace/Development/public_html/book/linux/*.html

```

### 2.2. ncftpget

```
  ncftpget ftp.freebsd.org . /pub/FreeBSD/README.TXT /pub/FreeBSD/index.html
  ncftpget ftp.gnu.org /tmp '/pub/gnu/README.*'
  ncftpget ftp://ftp.freebsd.org/pub/FreeBSD/README.TXT
  ncftpget -R ftp.ncftp.com /tmp /ncftp  (ncftp is a directory)
  ncftpget -u gleason -p my.password Bozo.probe.net . '/home/mjg/.*rc'
  ncftpget -u gleason Bozo.probe.net . /home/mjg/foo.txt  (prompt for password)
  ncftpget -f Bozo.cfg '/home/mjg/.*rc'
  ncftpget -c ftp.freebsd.org /pub/FreeBSD/README.TXT | /usr/bin/more
  ncftpget -c ftp://ftp.freebsd.org/pub/FreeBSD/README.TXT | /usr/bin/more
  ncftpget -a -d /tmp/debug.log -t 60 ftp.wustl.edu . '/pub/README*'

```

### 2.3. ncftpput

```
$ ncftpput -R -u netkiller -p password netkiller.hikz.com /home/netkiller/www ~/public_html/*

```

## 3. FileZilla

http://filezilla-project.org/

## 4. vsftpd - The Very Secure FTP Daemon

### 4.1. 安装 vsftpd

#### 4.1.1. Ubuntu 环境安装

```
$ sudo apt-get install vsftpd

```

test

```

[08:25:37 jobs:0] $ ncftp ftp://127.0.0.1
NcFTP 3.2.1 (Jul 29, 2007) by Mike Gleason (http://www.NcFTP.com/contact/).
Connecting to 127.0.0.1...
(vsFTPd 2.0.7)
Logging in...
Login successful.
Logged in to 127.0.0.1.
Current remote directory is /.
ncftp / >

```

enable local user

```
$ sudo vim /etc/vsftpd.conf

# Uncomment this to allow local users to log in.
local_enable=YES
chroot_local_user=YES

$ sudo /etc/init.d/vsftpd reload

```

testing for local user

```

$ ncftp ftp://neo@127.0.0.1/
NcFTP 3.2.1 (Jul 29, 2007) by Mike Gleason (http://www.NcFTP.com/contact/).
Connecting to 127.0.0.1...
(vsFTPd 2.0.7)
Logging in...
Password requested by 127.0.0.1 for user "neo".

    Please specify the password.

Password: *******

Login successful.
Logged in to 127.0.0.1.
Current remote directory is /home/neo.
ncftp /home/neo >

```

#### 4.1.2. CentOS 7 环境安装

```

yum install -y vsftpd

systemctl enable vsftpd

cp /etc/vsftpd/vsftpd.conf{,.original}

sed -i 's/anonymous_enable=YES/anonymous_enable=NO/' /etc/vsftpd/vsftpd.conf
sed -i 's/#chroot_local_user=YES/chroot_local_user=YES/' /etc/vsftpd/vsftpd.conf
sed -i 's/listen=NO/listen=YES/' /etc/vsftpd/vsftpd.conf
sed -i 's/listen_ipv6=YES/listen_ipv6=NO/' /etc/vsftpd/vsftpd.conf

echo "allow_writeable_chroot=YES" >> /etc/vsftpd/vsftpd.conf

systemctl start vsftpd

```

firewalld 防火墙

```
# firewall-cmd --permanent --add-port=21/tcp				

```

iptables

```
sed -i 's/IPTABLES_MODULES=""/IPTABLES_MODULES="ip_conntrack_ftp"/' /etc/sysconfig/iptables-config

# vim /etc/sysconfig/iptables
-A INPUT -m state --state NEW -m tcp -p tcp --dport 20 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT

```

### 4.2. ftp 帐号的 shell 权限

我们不想让 FTP 用户通过 shell 登录系统, 可以将用户的 Shell 改为/sbin/nologin

```
neo:x:1000:1000:neo,,,:/home/neo:/sbin/nologin

```

### 4.3. vsftpd 认证模块

#### 4.3.1. pam_shells.so

```
# cat /etc/pam.d/vsftpd
#%PAM-1.0
session    optional     pam_keyinit.so    force revoke
auth       required	pam_listfile.so item=user sense=deny file=/etc/vsftpd/ftpusers onerr=succeed
auth       required	pam_shells.so
auth       include	system-auth
account    include	system-auth
session    include	system-auth
session    required     pam_loginuid.so

```

/etc/vsftpd/ftpusers 列表中的用户将不能登录 ftp 服务器

#### 4.3.2. virtual user

创建明文密码文件，一行用户名后回车跟一行密码

```
# cat virtual-users.txt
user
password
neo
123456
jam
654321

```

转为数据库文件

```
# sudo apt-get install db-util
# db_load -T -t hash -f virtual-users.txt /etc/vsftpd/virtual-users.db

```

创建插件认证配置文件 /etc/pam.d/vsftpd-virtual

```
auth required pam_userdb.so db=/etc/vsftpd/virtual-users
account required pam_userdb.so db=/etc/vsftpd/virtual-users

```

```
/etc/vsftpd/vsftpd.conf：

# virtual users to use local privs, not anon privs
virtual_use_local_privs=YES

# the PAM file used by authentication of virtual uses
pam_service_name=vsftpd-virtual

# in conjunction with 'local_root',
# specifies a home directory for each virtual user
user_sub_token=$USER
local_root=/var/www/virtual/$USER
# the virtual user is restricted to the virtual FTP area

chroot_local_user=YES
# hides the FTP server user IDs and just display "ftp" in directory listings
hide_ids=YES

guest_enable=YES
guest_username=nobody

# the umask for file creation
local_umask=022

```

guest_username=nobody 虚拟用户将使用 nobody 用户作为他的 uid,gid.

```
# mkdir /var/www/virtual/mary
# chown ftp:ftp /var/www/virtual/mary

```

#### 4.3.3. 虚拟用户权限

```
vim /etc/vsftpd.conf

user_config_dir=/etc/vsftpd/conf.d

mkdir /etc/vsftpd/conf.d

```

neo 只能下载不能上传

```

echo "anon_world_readable_only=NO" > /etc/vsftpd/conf.d/neo

```

jam 可以下上传跟下载

```

echo "anon_world_readable_only=NO" > /etc/vsftpd/conf.d/jam
echo "anon_upload_enable=YES" >> /etc/vsftpd/conf.d/jam
echo "write_enable=YES" >> /etc/vsftpd/conf.d/jam

```

### 4.4. chroot

#### 4.4.1. local user

chroot 所有本地用户

```
chroot_local_user=YES

```

#### 4.4.2. /etc/vsftpd/chroot_list

受限用户用户添加到文件 vsftpd.chroot_list

```
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list

```

注意：每行一个用户名

#### 4.4.3. test

```
adduser -o --home /www --shell /sbin/nologin --uid 99 --gid 99 --group nobody www
echo "www:chen" | chpasswd
echo www > /etc/vsftpd/chroot_list
ncftp ftp://www:chen@172.16.0.1

```

### 4.5. FAT

#### 4.5.1. vsftpd: refusing to run with writable root inside chroot()

添加 allow_writeable_chroot=YES 项到 /etc/vsftpd/vsftpd.conf 配置文件

```
echo "allow_writeable_chroot=YES" >> /etc/vsftpd/vsftpd.conf

```

```
重启 vsftpd 
```

## 5. ProFTPD + MySQL / OpenLDAP 用户认证

准备工作

下载 ProFTPD : ftp://ftp.proftpd.org/distrib/source/proftpd-1.2.7.tar.gz

下载 mod_sql : http://www.lastditcheffort.org/~aah/proftpd/mod_sql/

下载 mod_ldap-2.8.10 : http://www.horde.net/~jwm/software/mod_ldap/

### 5.1. Proftpd + MySQL

```
tar xvzf proftpd-version.tar.gz

cd proftpd-version

./configure --prefix=/usr/local/proftpd --with-modules=mod_sql:mod_sql_mysql

make

make install

```

```

安装成功后，测试 ProFTPD，启动 ProFTPD

/usr/local/proftpd/sbin/in.proftpd

如果没有显示任何信息，ProFTPD 启动成功。使用系统用户登录 Ftp Server

[root@linux sbin]# ftp localhost

Connected to localhost (127.0.0.1).

220 ProFTPD 1.2.7 Server (ProFTPD Default Installation) [linux.xuser.net]

Name (localhost:root):usera

331 Password required for usera.

Password:

230 User usera logged in.

Remote system type is UNIX.

Using binary mode to transfer files.

ftp>

ProFTPD 测试成功，关闭 ProFTPD

killall in.proftpd

编辑 proftpd.conf 文件

vi /usr/local/proftpd/etc/proftpd.conf

添加下面几行参数

<Global>

SQLConnectInfo ftpusers@localhost:3306 root chen

SQLAuthTypes Plaintext

SQLUserInfo users userid passwd uid gid homedir NULL

RequireValidShell off

SQLAuthenticate users groups usersetfast groupsetfast

</Global>

格式说明：

SQLConnectInfo 数据库@主机名:端口 用户 密码

SQLAuthTypes 密码类型（Plaintext 明文密码，Crypt DES 密码，Backend MySQL password()函数产生的密码）

SQLUserInfo [用户表] [用户名字段] [密码字段] [用户 ID] [组 ID] [用户目录] NULL

创建 ftpusers.sql 文件

[mysql@linux mysql]$ vi ftpusers.sql

-- MySQL dump 8.22

--

-- Host: localhost    Database: proftpd

---------------------------------------------------------

-- Server version       3.23.52-max

--

-- Table structure for table 'groups'

--

CREATE TABLE groups (

  groupname varchar(255) binary NOT NULL default '',

  gid int(11) NOT NULL default '0',

  members text NOT NULL,

  PRIMARY KEY  (groupname)

) TYPE=MyISAM;

--

-- Dumping data for table 'groups'

--

INSERT INTO groups VALUES ('nogroup',502,'FTP Group');

--

-- Table structure for table 'users'

--

CREATE TABLE users (

  userid varchar(255) binary NOT NULL default '',

  passwd varchar(255) binary NOT NULL default '',

  uid int(11) default NULL,

  gid int(11) default NULL,

  homedir varchar(255) default NULL,

  shell varchar(255) default NULL,

  count int(11) default NULL,

  used double(10,1) default '0.0',

  quota double(10,1) default '10000000.0',

  PRIMARY KEY  (userid)

) TYPE=MyISAM;

--

-- Dumping data for table 'users'

--

INSERT INTO users VALUES ('chen','chen',500,500,'/home/samba','/bin/sh',0,0.0,10000000.0);

INSERT INTO users VALUES ('user2','123456',500,500,'/home/samba','/bin/bash',1,0.0,10000000.0);

INSERT INTO users VALUES ('user1','123456',NULL,NULL,'/u01',NULL,1,0.0,10000000.0);

创建数据库与表

[mysql@linux mysql]$ echo "create database ftpusers" | mysql -uroot -pchen

[mysql@linux mysql]$ mysql -uroot -pchen ftpusers < ftpusers.sql

[mysql@linux mysql]$

再次启动 ProFTPD

/usr/local/proftpd/sbin/in.proftpd

这次使用 MySQL 用户登录 Ftp Server

显示 230 User xxxxx logged in. ＭySQL 认证成功

```

### 5.2. Proftpd + OpenLDAP

```

tar xvzf proftpd-version.tar.gz

cd proftpd-version

./configure --prefix=/usr/local/proftpd --with-modules=mod_ldap

make

make install

# tar zxvf mod_ldap-2.8.10.tar.gz

将 mod_ldap-2.8.10 目录下的 posixAccount-objectclass 和 posixGroup-objectclass

复制到 OpenLDAP 的 schema 目录下：

# cp mod_ldap-2.8.10/posix* /etc/openldap/schema/

# vi /etc/openldap/slapd.conf

修改 OpenLDAP 的配置文件 slapd.conf，将这两个文件包含到该文件中：

include /etc/openldap/schema/posixAccount-objectclass

include /etc/openldap/schema/posixGroup-objectclass

重新启动 OpenLDAP：

# service ldap restart

Stopping slapd:                                            [  OK  ]

Starting slapd:                                            [  OK  ]

编辑 proftpd.conf 文件

vi /usr/local/proftpd/etc/proftpd.conf

添加下面几行参数

<Global>

LDAPServer localhost
LDAPDNInfo cn=your-dn,dc=horde,dc=net dnpass
LDAPDoAuth on "dc=users,dc=horde,dc=net"

</Global>

格式说明：

LDAPServer OpenLDAP 服务器
LDAPDNInfo cn=你的-dn,dc=区域名,dc=区域名 dn 密码
LDAPDoAuth on "dc=区域名,dc=区域名"

例子：

       <Global>

LDAPServer localhost

LDAPDNInfo cn=manager,dc=xuser,dc=net secret

LDAPDoAuth on dc=xuser,dc=net

</Global>

根据自己需要修改 mod_ldap-2.8.10 目录中的 group-ldif 和 user-ldif 文件，并将条目添加到 OpenLDAP 中：

# ldapadd -x -D "cn=manager,dc=xuser,dc=net" -w secret -f group-ldif

# ldapadd -x -D "cn=manager,dc=xuser,dc=net" -w secret -f user-ldif

显示：adding new entry "cn=mygroup, dc=xuser, dc=net" 添加成功

使用 ldapsearch 查看记录

# ldapsearch -x -b "dc=xuser,dc=net"

启动 ProFTPD：

/usr/local/proftpd/sbin/in.proftpd

使用 OpenLDAP 用户登录 Ftp Server

显示 230 User xxxxx logged in. OpenLDAP 认证成功

例：

[root@linux mod_ldap-2.8.10]# cat group-ldif

dn: cn=mygroup, dc=xuser, dc=net

objectclass: posixGroup

cn: mygroup

gidNumber: 100

memberUid: user1

memberUid: user2

memberUid: user3

memberUid: user4

memberUid: ftpusersb

memberUid: usera

memberUid: jwm

memberUid: 100

[root@linux mod_ldap-2.8.10]# cat user-ldif

dn: uid=jwm, dc=xuser, dc=net

objectclass: posixAccount

cn: John Morrissey

uid: jwm

uidNumber: 2000

gidNumber: 100

homeDirectory: /home/chen

userPassword: {crypt}*

loginShell: /bin/bash

dn: uid=chen, dc=xuser, dc=net

objectclass: posixAccount

cn: chen

uid: chen

uidNumber: 2000

gidNumber: 100

homeDirectory: /home/chen

userPassword: {crypt}sa7XjjlytXZZ2

loginShell: /bin/bash

dn: cn=ftpuser1, dc=xuser, dc=net

objectclass: posixAccount

cn: ftpuser1

uid: ftpuser1

uidNumber: 2000

gidNumber: 100

homeDirectory: /home/chen

userPassword: {crypt}sa7XjjlytXZZ2

loginShell: /bin/bash

dn: uid=usera, dc=xuser, dc=net

objectclass: posixAccount

cn: usera

uid: usera

uidNumber: 2000

gidNumber: 100

homeDirectory: /tmp

userPassword:{crypt}sa7XjjlytXZZ2

loginShell: /bin/bash

dn: uid=ftpuserb, dc=xuser, dc=net

objectclass: posixAccount

cn: ftpuserb

uid: ftpuserb

uidNumber: 2000

gidNumber: 100

homeDirectory: /tmp

userPassword:{crypt}O2BooHEK9JI06

loginShell: /bin/bash

上面的用户密码是用 crypt 方式加密的密码，密码产生请看

使用 PHP 产生：

# cat des.php

<html>

<p>DES 密碼產生器</p>

<form method=post action=des.php>

<p>password:<input name=passwd type=text size=20></p>

<input type=submit value=submit>

</form>

<?

$enpw=crypt($passwd);

echo "password is: $enpw";

?>

使用 perl 产生：

perl -e 'print("userPassword: ".crypt("secret","salt")."\n");'

产生的 DES 密码，同样也可以用于 OpenLDAP 的管理员密码

# vi /etc/openldap/slapd.conf

rootpw                {crypt}ijFYNcSNctBYg

四、 标准的配置文件

MySQL 认证配置实例

[root@linux root]# cat /usr/local/proftpd/etc/proftpd.conf

ServerName                      "ProFTPD Default Installation"

ServerType                      standalone

DefaultServer                   on

# Port 21 is the standard FTP port.

Port                            21

# Umask 022 is a good standard umask to prevent new dirs and files

# from being group and world writable.

Umask                           022

# We put our mod_sql directives in a <Global> block so they'll be

# inherited by the <Anonymous> block below, and any other <VirtualHost>

# blocks we may want to add.  For a simple server these don't need to

# be in a <Global> block but it won't hurt anything.

<Global>

SQLConnectInfo ftpusers@localhost:3306 root chen

SQLAuthTypes Plaintext

SQLUserInfo users userid passwd uid gid homedir NULL

RequireValidShell off

SQLAuthenticate users groups usersetfast groupsetfast

</Global>

# To prevent DoS attacks, set the maximum number of child processes

# to 30\.  If you need to allow more than 30 concurrent connections

# at once, simply increase this value.  Note that this ONLY works

# in standalone mode, in inetd mode you should use an inetd server

# that allows you to limit maximum number of processes per service

# (such as xinetd)

MaxInstances                    30

# Set the normal user and group permissions for the server.

User                            nobody

Group                           nogroup

# Normally, we want files to be overwriteable.

<Directory /*>

  AllowOverwrite                on

</Directory>

# A basic anonymous configuration, no upload directories.  If you

# don't want to support anonymous access, simply remove this

# <Anonymous ..> ... </Anonymous> block.

<Anonymous ~ftp>

  User                          ftp

  Group                         ftp

  # We want clients to be able to login with "anonymous" as well as "ftp"

  UserAlias                     anonymous ftp

  # Limit the maximum number of anonymous logins

  MaxClients                    10

  # We want 'welcome.msg' displayed at login, and '.message' displayed

  # in each newly chdired directory.

  DisplayLogin                  welcome.msg

  DisplayFirstChdir             .message

  # Limit WRITE everywhere in the anonymous chroot

  <Limit WRITE>

    DenyAll

  </Limit>

</Anonymous>

OpenLDAP 认证配置实例

[root@linux root]# cat /usr/local/proftpd/etc/proftpd.conf

# This is a basic ProFTPD configuration file (rename it to

# 'proftpd.conf' for actual use.  It establishes a single server

# and a single anonymous login.  It assumes that you have a user/group

# "nobody" and "ftp" for normal operation and anon.

ServerName                      "ProFTPD Default Installation"

ServerType                      standalone

DefaultServer                   on

# Port 21 is the standard FTP port.

Port                            21

# Umask 022 is a good standard umask to prevent new dirs and files

# from being group and world writable.

Umask                           022

<Global>

LDAPDoAuth on dc=xuser,dc=net

LDAPServer localhost

LDAPDNInfo cn=manager,dc=xuser,dc=net secret

</Global>

# To prevent DoS attacks, set the maximum number of child processes

# to 30\.  If you need to allow more than 30 concurrent connections

# at once, simply increase this value.  Note that this ONLY works

# in standalone mode, in inetd mode you should use an inetd server

# that allows you to limit maximum number of processes per service

# (such as xinetd).

MaxInstances                    30

# Set the user and group under which the server will run.

User                            nobody

Group                           nogroup

# Normally, we want files to be overwriteable.

<Directory />

  AllowOverwrite                on

</Directory>

# A basic anonymous configuration, no upload directories.

<Anonymous ~ftp>

  User                          ftp

  Group                         ftp

  # We want clients to be able to login with "anonymous" as well as "ftp"

  UserAlias                     anonymous ftp

  # Limit the maximum number of anonymous logins

  MaxClients                    10

  # We want 'welcome.msg' displayed at login, and '.message' displayed

  # in each newly chdired directory.

  DisplayLogin                  welcome.msg

  DisplayFirstChdir             .message

  # Limit WRITE everywhere in the anonymous chroot

  <Limit WRITE>

    DenyAll

  </Limit>

</Anonymous>

# Include /usr/local/etc/mod_ldap.conf

OpenLDAP 配置文件

[root@linux root]# cat /etc/openldap/slapd.conf

# $OpenLDAP: pkg/ldap/servers/slapd/slapd.conf,v 1.8.8.6 2001/04/20 23:32:43 kurt Exp $

#

# See slapd.conf(5) for details on configuration options.

# This file should NOT be world readable.

#

include         /etc/openldap/schema/core.schema

include         /etc/openldap/schema/cosine.schema

include         /etc/openldap/schema/inetorgperson.schema

include         /etc/openldap/schema/nis.schema

include         /etc/openldap/schema/redhat/rfc822-MailMember.schema

include         /etc/openldap/schema/redhat/autofs.schema

include         /etc/openldap/schema/redhat/kerberosobject.schema

include         /etc/openldap/schema/chen

include         /etc/openldap/schema/posixAccount-objectclass

include         /etc/openldap/schema/posixGroup-objectclass

#include                /etc/openldap/schema/qmail_schema

#include        /etc/openldap/slapd.info.oc.conf

#include        /etc/openldap/slapd.account.oc.conf

# Define global ACLs to disable default read access.

# Do not enable referrals until AFTER you have a working directory

# service AND an understanding of referrals.

#referral       ldap://root.openldap.org

#pidfile        //var/run/slapd.pid

#argsfile       //var/run/slapd.args

# Create a replication log in /var/lib/ldap for use by slurpd.

#replogfile     /var/lib/ldap/master-slapd.replog

# Load dynamic backend modules:

# modulepath    /usr/sbin/openldap

# moduleload    back_ldap.la

# moduleload    back_ldbm.la

# moduleload    back_passwd.la

# moduleload    back_shell.la

# The next two lines allow use of TLS for connections using a dummy test

# certificate, but you should generate a proper certificate by changing to

# /usr/share/ssl/certs, running "make slapd.pem", and fixing permissions on

# slapd.pem so that the ldap user or group can read it.

#TLSCertificateFile /usr/share/ssl/certs/slapd.pem

#TLSCertificateKeyFile /usr/share/ssl/certs/slapd.pem

#######################################################################

# ldbm database definitions

#######################################################################

database        ldbm

suffix          "dc=xuser,dc=net"

rootdn          "cn=Manager,dc=xuser,dc=net"

#rootdn         "cn=Manager,dc=my-domain,dc=com"

#rootdn         "cn=Manager,o=My Organization Name,c=US"

# Cleartext passwords, especially for the rootdn, should

# be avoided.  See slappasswd(8) and slapd.conf(5) for details.

# Use of strong authentication encouraged.

rootpw          secret

# rootpw                secret

# rootpw                {crypt}ijFYNcSNctBYg

# The database directory MUST exist prior to running slapd AND

# should only be accessible by the slapd/tools. Mode 700 recommended.

directory       /var/lib/ldap

# Indices to maintain

index   objectClass,uid,uidNumber,gidNumber,memberUid   eq

index   cn,mail,surname,givenname                       eq,subinitial

# Replicas to which we should propagate changes

#replica ldap-1.example.com:389 tls=yes

#       bindmethod=sasl saslmech=GSSAPI

#       authcId=host/ldap-master.example.com@EXAMPLE.COM

五、 FAQ
Q：在本地 ftp localhost 输入用户名、密码回车后。等很久才进入 FTP Server

A：ftp 127.0.0.1

Q：在远程服务器上 ftp ip 输入用户名、密码回车后。等很久才进入 FTP Server

A：LDAPServer localhost 改为 LDAPServer 127.0.0.1

Q：[root@linux mod_ldap-2.8.10]# ftp 127.0.0.1

Connected to 127.0.0.1 (127.0.0.1).

500 FTP server shut down (going down at Tue Dec 17 19:00:00 2002) -- please try again later.

ftp>

A：rm –rf /etc/shutmsg

Q：登录 Ftp Server 提示

530 Login incorrect.

Login failed.
我确认输入的用户、密码决对正确

A：在登录 ProFTPD 时加参数 proftpd –d5 –n 会输出调试信息。你可以在其中

找到答案。如果在调试信息中找到这一行 no such user 'xxxx'
可能是与 MySQL/OpenLDAP 连接有问题。

Q：我在网上看见很多介绍如何安装 ProFTPD 文章,阅读大量的 How to，按 How to 一步一步做，从来没有安装成功过。

A：网上很多文章，比较老，很多定义现以不在使用如：

SQLConnectInfo laftp@localhost 用户名 口令

SQLAuthTypes Plaintext Backend

SQLAuthoritative ON

SQLDefaultGID 1001

SQLDefaultUID 1001

SQLDoAuth ON

SQLDoGroupAuth ON

SQLGidField gid

SQLGroupGIDField gid

SQLGroupMembersField members

SQLGroupTable ftpgroup

SQLGroupnameField groupname

SQLHomedirField homedir

SQLMinUserUID 400

SQLMinUserGID 400

SQLPasswordField passwd

SQLUidField uid

SQLUserTable ftpuser

SQLUsernameField userid

SQLLoginCountField count

########################################################

LDAPServer                      "localhost"

LDAPPrefix                      "dc=horde,dc=net"

LDAPDN                          "cn=thedn,dc=horde,dc=net"

LDAPDNPass                      "ldap_dnpass"

LDAPNegativeCache       on

```

## 6. Pure-FTPd + LDAP + MySQL + PGSQL + Virtual-Users + Quota

参考 http://netkiller.sourceforge.net/pureftpd/

## 第 70 章 File Synchronize

## 1. rsync - fast remote file copy program (like rcp)

rsync is an open source utility that provides fast incremental file transfer. rsync is freely available under the GNU General Public License version 2 and is currently being maintained by Wayne Davison.

### 1.1. 安装 Rsync 与配置守护进程

#### 1.1.1. install with source

过程 70.1. rsync

1.  安装 rsync

    在 AS3 第二张 CD 上找到 rsync-2.5.6-20.i386.rpm

    ```
    [root@linuxas3 root]# cd /mnt
    [root@linuxas3 mnt]# mount cdrom
    [root@linuxas3 mnt]# cd cdrom/RedHat/RPMS
    [root@linuxas3 RPMS]# rpm -ivh rsync-2.5.6-20.i386.rpm

    ```

2.  配置/etc/rsyncd.conf

    在 rh9,as3 系统上 rsync 安装后,并没有创建 rsyncd.conf 文档，要自己创建 rsyncd.conf 文档

    ```
    [root@linuxas3 root]# vi /etc/rsyncd.conf

    uid=nobody
    gid=nobody
    max connections=5
    use chroot=no
    log file=/var/log/rsyncd.log
    pid file=/var/run/rsyncd.pid
    lock file=/var/run/rsyncd.lock
    #auth users=root
    secrets file=/etc/rsyncd.passwd

    [postfix]
    path=/var/mail
    comment = backup mail
    ignore errors
    read only = yes
    list = no
    auth users = postfix

    [netkiller]
    path=/home/netkiller/web
    comment = backup 9812.net
    ignore errors
    read only = yes
    list = no
    auth users = netkiller

    [pgsqldb]
    path=/var/lib/pgsql
    comment = backup postgresql database
    ignore errors
    read only = yes
    list = no

    ```

    1.  选项说明

        ```
        uid = nobody
        gid = nobody
        use chroot = no         # 不使用 chroot
        max connections = 4     # 最大连接数为 4
        pid file = /var/run/rsyncd.pid           #进程 ID 文件
        lock file = /var/run/rsync.lock
        log file = /var/log/rsyncd.log    # 日志记录文件
        secrets file = /etc/rsyncd.pwd    # 认证文件名,主要保存用户密码，权限建议设为 600，所有者 root

        [module]            # 这里是认证的模块名，在 client 端需要指定
        path = /var/mail    # 需要做镜像的目录
        comment = backup xxxx # 注释
        ignore errors         # 可以忽略一些无关的 IO 错误
        read only = yes       # 只读
        list = no             # 不允许列文件
        auth users = postfix  # 认证的用户名，如果没有这行，则表明是匿名

        [other]
        path = /path/to...
        comment = xxxxx

        ```

    2.  密码文件

        在 server 端生成一个密码文件/etc/rsyncd.pwd

        ```
        [root@linuxas3 root]# echo postfix:xxx >>/etc/rsyncd.pwd
        [root@linuxas3 root]# echo netkiller:xxx >>/etc/rsyncd.pwd
        [root@linuxas3 root]# chmod 600 /etc/rsyncd.pwd

        ```

    3.  启动 rsync daemon

        ```
        [root@linuxas3 root]# rsync --daemon

        ```

3.  添加到启动文件

    ```
    echo　"rsync　--daemon"　>>　/etc/rc.d/rc.local                                    [  OK  ]

    ```

    cat /etc/rc.d/rc.local 确认一下

4.  测试

    ```
    [root@linux docbook]#  rsync rsync://netkiller.8800.org/netkiller
    [root@linux tmp]# rsync rsync://netkiller@netkiller.8800.org/netkiller
    Password:

    [chen@linux temp]$  rsync -vzrtopg --progress --delete postfix@netkiller.8800.org::postfix /tmp
    Password:

    ```

#### 1.1.2. install with aptitude

过程 70.2. installation setp by setp

1.  installation

    ```
    $ sudo apt-get install rsync

    ```

2.  enable

    ```
    $ sudo vim /etc/default/rsync

    RSYNC_ENABLE=true

    ```

3.  config /etc/rsyncd.conf

    ```
    $ sudo vim /etc/rsyncd.conf

    uid=nobody
    gid=nobody
    max connections=5
    use chroot=no
    pid file=/var/run/rsyncd.pid
    lock file=/var/run/rsyncd.lock
    log file=/var/log/rsyncd.log
    #auth users=root
    secrets file=/etc/rsyncd.secrets

    [neo]
    path=/home/neo/www
    comment = backup neo
    ignore errors
    read only = yes
    list = no
    auth users = neo

    [netkiller]
    path=/home/netkiller/public_html
    comment = backup netkiller
    ignore errors
    read only = yes
    list = no
    auth users = netkiller

    [mirror]
    path=/var/www/netkiller.8800.org/html/
    comment = mirror netkiller.8800.org
    exclude = .svn
    ignore errors
    read only = yes
    list = yes

    [music]
    path=/var/music
    comment = backup music database
    ignore errors
    read only = yes
    list = no

    [pgsqldb]
    path=/var/lib/pgsql
    comment = backup postgresql database
    ignore errors
    read only = yes
    list = no
    auth users = neo,netkiller

    ```

4.  /etc/rsyncd.secrets

    ```
    $ sudo vim  /etc/rsyncd.secrets

    neo:123456
    netkiller:123456

    ```

    ```
    $ sudo chmod 600 /etc/rsyncd.secrets

    ```

5.  start

    ```
    $ sudo /etc/init.d/rsync start

    ```

6.  test

    ```
    $ rsync -vzrtopg --progress --delete neo@localhost::neo /tmp/test1/
    $ rsync -vzrtopg --progress --delete localhost::music /tmp/test2/

    ```

7.  firewall

    ```
    $ sudo ufw allow rsync

    ```

#### 1.1.3. xinetd

CentOS 6 之前的版本可以使用 xinetd， CentOS 7 不建议使用

```
yum install xinetd

```

配置 /etc/xinetd.d/rsync

```
vim /etc/xinetd.d/rsync

# default: off
# description: The rsync server is a good addition to an ftp server, as it \
#	allows crc checksumming etc.
service rsync
{
	disable	= yes
	flags		= IPv6
	socket_type     = stream
	wait            = no
	user            = root
	server          = /usr/bin/rsync
	server_args     = --daemon
	log_on_failure  += USERID
}

```

disable = yes 改为 disable = no

# vim /etc/rsyncd.conf

```
chkconfig xinetd on
/etc/init.d/xinetd restart

```

#### 1.1.4. CentOS 7 - systemctl

```
systemctl enable rsyncd
systemctl start rsyncd
systemctl restart rsyncd
systemctl stop rsyncd

```

启动配置项 /etc/sysconfig/rsyncd

```
# cat /etc/sysconfig/rsyncd
OPTIONS="" 			

```

启动脚本

```
# cat /usr/lib/systemd/system/rsyncd.service
[Unit]
Description=fast remote file copy program daemon
ConditionPathExists=/etc/rsyncd.conf

[Service]
EnvironmentFile=/etc/sysconfig/rsyncd
ExecStart=/usr/bin/rsync --daemon --no-detach "$OPTIONS"

[Install]
WantedBy=multi-user.target			

```

### 1.2. rsyncd.conf

```
# Minimal configuration file for rsync daemon
# See rsync(1) and rsyncd.conf(5) man pages for help

# This line is required by the /etc/init.d/rsyncd script
pid file = /var/run/rsyncd.pid
port = 873
address = 192.168.1.171
#uid = nobody
#gid = nobody
uid = root
gid = root

use chroot = yes
read only = yes

#limit access to private LANs
hosts allow=192.168.1.0/255.255.255.0 10.0.1.0/255.255.255.0
hosts deny=*

max connections = 5
motd file = /etc/rsyncd/rsyncd.motd

#This will give you a separate log file
#log file = /var/log/rsync.log

#This will log every file transferred - up to 85,000+ per user, per sync
#transfer logging = yes

log format = %t %a %m %f %b
syslog facility = local3
timeout = 300

[home]
path = /home
list=yes
ignore errors
auth users = linux
secrets file = /etc/rsyncd/rsyncd.secrets
comment = linuxsir home
exclude =   beinan/  samba/

[beinan]
path = /opt
list=no
ignore errors
comment = optdir
auth users = beinan
secrets file = /etc/rsyncd/rsyncd.secrets

[www]
path = /www/
ignore errors
read only = true
list = false
hosts allow = 172.16.1.1
hosts deny = 0.0.0.0/32
auth users = backup
secrets file = /etc/backserver.pas

[web_user1]
path = /home/web_user1/
ignore errors
read only = true
list = false
hosts allow = 202.99.11.121
hosts deny = 0.0.0.0/32
uid = web_user1
gid = web_user1
auth users = backup
secrets file = /etc/backserver.pas

[pub]
  	comment = Random things available for download
  	path = /path/to/my/public/share
  	read only = yes
  	list = yes
  	uid = nobody
  	gid = nobody
  	auth users = pub
  	secrets file = /etc/rsyncd.secrets

```

### 1.3. rsync 参数说明

```

命令行选项
-v, --verbose 详细模式输出
-q, --quiet 精简输出模式
-c, --checksum 打开校验开关，强制对文件传输进行校验
-a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
-r, --recursive 对子目录以递归模式处理
-R, --relative 使用相对路径信息
-b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix 选项来指定不同的备份文件前缀。
--backup-dir 将备份文件(如~filename)存放在在目录下。
-suffix=SUFFIX 定义备份文件前缀
-u, --update 仅仅进行更新，也就是跳过所有已经存在于 DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)
-l, --links 保留软链结
-L, --copy-links 想对待常规文件一样处理软链结
--copy-unsafe-links 仅仅拷贝指向 SRC 路径目录树以外的链结
--safe-links 忽略指向 SRC 路径目录树以外的链结
-H, --hard-links 保留硬链结
-p, --perms 保持文件权限
-o, --owner 保持文件属主信息
-g, --group 保持文件属组信息
-D, --devices 保持设备文件信息
-t, --times 保持文件时间信息
-S, --sparse 对稀疏文件进行特殊处理以节省 DST 的空间
-n, --dry-run 现实哪些文件将被传输
-W, --whole-file 拷贝文件，不进行增量检测
-x, --one-file-system 不要跨越文件系统边界
-B, --block-size=SIZE 检验算法使用的块尺寸，默认是 700 字节
-e, --rsh=COMMAND 指定使用 rsh、ssh 方式进行数据同步
--rsync-path=PATH 指定远程服务器上的 rsync 命令所在路径信息
-C, --cvs-exclude 使用和 CVS 一样的方法自动忽略文件，用来排除那些不希望传输的文件
--existing 仅仅更新那些已经存在于 DST 的文件，而不备份那些新创建的文件
--delete 删除那些 DST 中 SRC 没有的文件
--delete-excluded 同样删除接收端那些被该选项指定排除的文件
--delete-after 传输结束以后再删除
--ignore-errors 及时出现 IO 错误也进行删除
--max-delete=NUM 最多删除 NUM 个文件
--partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
--force 强制删除目录，即使不为空
--numeric-ids 不将数字的用户和组 ID 匹配为用户名和组名
--timeout=TIME IP 超时时间，单位为秒
-I, --ignore-times 不跳过那些有同样的时间和长度的文件
--size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
--modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为 0
-T --temp-dir=DIR 在 DIR 中创建临时文件
--compare-dest=DIR 同样比较 DIR 中的文件来决定是否需要备份
-P 等同于 --partial
--progress 显示备份过程
-z, --compress 对备份的文件在传输时进行压缩处理
--exclude=PATTERN 指定排除不需要传输的文件模式
--include=PATTERN 指定不排除而需要传输的文件模式
--exclude-from=FILE 排除 FILE 中指定模式的文件
--include-from=FILE 不排除 FILE 指定模式匹配的文件
--version 打印版本信息
--address 绑定到特定的地址
--config=FILE 指定其他的配置文件，不使用默认的 rsyncd.conf 文件
--port=PORT 指定其他的 rsync 服务端口
--blocking-io 对远程 shell 使用阻塞 IO
-stats 给出某些文件的传输状态
--progress 在传输时现实传输过程
--log-format=formAT 指定日志文件格式
--password-file=FILE 从 FILE 中得到密码
--bwlimit=KBPS 限制 I/O 带宽，KBytes per second
-h, --help 显示帮助信息

```

#### 1.3.1. -n, --dry-run perform a trial run with no changes made

模拟运行，显示日志，但不做复制操作。

```
rsync -anvzP /www/* root@172.16.0.1/www

```

#### 1.3.2. --bwlimit=KBPS limit I/O bandwidth; KBytes per second

速度限制，限制为 100k Bytes/s

```
rsync -auvzP--bwlimit=100 /www/* root@172.16.0.1/www

```

#### 1.3.3. -e, --rsh=COMMAND specify the remote shell to use

```
rsync -auzv --rsh=ssh root@202.130.101.33:/www/example.com/* /backup/example.com/ 
# --rsh=ssh 可以省略
rsync -auzv root@202.130.101.33:/www/example.com/* /backup/example.com/				

```

如果需要特别参数，可以这样写，这里指定连接 SSH 的端口为 20

```
rsync -auzv --rsh='ssh -p20' root@202.130.101.34:/www/example.com/* /backup/example.com/  			

```

### 1.4. step by step to learn rsync

1.  transfer file from src to dest directory

    ```
    neo@netkiller:/tmp$ mkdir rsync
    neo@netkiller:/tmp$ cd rsync/
    neo@netkiller:/tmp/rsync$ ls
    neo@netkiller:/tmp/rsync$ mkdir src dest
    neo@netkiller:/tmp/rsync$ echo file1 > src/file1
    neo@netkiller:/tmp/rsync$ echo file2 > src/file2
    neo@netkiller:/tmp/rsync$ echo file3 > src/file3

    ```

2.  skipping directory

    ```
    neo@netkiller:/tmp/rsync$ mkdir src/dir1
    neo@netkiller:/tmp/rsync$ mkdir src/dir2
    neo@netkiller:/tmp/rsync$ rsync src/* dest/
    skipping directory src/dir1
    skipping directory src/dir2

    ```

3.  recurse into directories

    ```
    neo@netkiller:/tmp/rsync$ rsync -r src/* dest/
    neo@netkiller:/tmp/rsync$ ls dest/
    dir1  dir2  file1  file2  file3

    ```

4.  backup

    ```
    neo@netkiller:/tmp/rsync$ rsync -r --backup --suffix=.2008-11-21 src/* dest/
    neo@netkiller:/tmp/rsync$ ls dest/
    dir1  dir2  file1  file1.2008-11-21  file2  file2.2008-11-21  file3  file3.2008-11-21
    neo@netkiller:/tmp/rsync$

    ```

    backup-dir

    ```
    neo@netkiller:/tmp/rsync$ rsync -r --backup --suffix=.2008-11-21 --backup-dir mybackup src/* dest/
    neo@netkiller:/tmp/rsync$ ls dest/
    dir1  dir2  file1  file1.2008-11-21  file2  file2.2008-11-21  file3  file3.2008-11-21  mybackup
    neo@netkiller:/tmp/rsync$ ls dest/mybackup/
    file1.2008-11-21  file2.2008-11-21  file3.2008-11-21

    ```

    ```
    rsync -r --backup --suffix=.2008-11-21 --backup-dir ../mybackup src/* dest/
    neo@netkiller:/tmp/rsync$ ls
    dest  mybackup  src
    neo@netkiller:/tmp/rsync$ ls src/
    dir1  dir2  file1  file2  file3

    ```

5.  update

    ```
    neo@netkiller:/tmp/rsync$ rm -rf dest/*
    neo@netkiller:/tmp/rsync$ rsync -r -u src/* dest/
    neo@netkiller:/tmp/rsync$ echo netkiller>>src/file2
    neo@netkiller:/tmp/rsync$ rsync -v -r -u src/* dest/
    building file list ... done
    file2

    sent 166 bytes  received 42 bytes  416.00 bytes/sec
    total size is 38  speedup is 0.18

    ```

    update by time and size

    ```
    neo@netkiller:/tmp/rsync$ echo Hi>src/dir1/file1.1
    neo@netkiller:/tmp/rsync$ rsync -v -r -u src/* dest/
    building file list ... done
    dir1/file1.1

    sent 166 bytes  received 42 bytes  416.00 bytes/sec
    total size is 41  speedup is 0.20

    ```

6.  --archive

    ```
    rsync -a src/ dest/

    ```

7.  --compress

    ```
    rsync -a -z src/ dest/

    ```

8.  --delete

    src

    ```
    svn@netkiller:~$ ls src/
    dir1  dir2  file1  file2  file3

    ```

    dest

    ```
    neo@netkiller:~$ rsync -v -u -a --delete -e ssh svnroot@127.0.0.1:/home/svnroot/src /tmp/dest
    svnroot@127.0.0.1's password:
    receiving file list ... done
    created directory /tmp/dest
    src/
    src/file1
    src/file2
    src/file3
    src/dir1/
    src/dir2/

    sent 104 bytes  received 309 bytes  118.00 bytes/sec
    total size is 0  speedup is 0.00

    ```

    src

    ```
    svn@netkiller:~$ rm -rf src/file2
    svn@netkiller:~$ rm -rf src/dir2

    ```

    dest

    ```
    neo@netkiller:~$ rsync -v -u -a --delete -e ssh svnroot@127.0.0.1:/home/svnroot/src /tmp/dest
    svnroot@127.0.0.1's password:
    receiving file list ... done
    deleting src/dir2/
    deleting src/file2
    src/

    sent 26 bytes  received 144 bytes  68.00 bytes/sec
    total size is 0  speedup is 0.00

    ```

### 1.5. rsync examples

[`samba.anu.edu.au/rsync/examples.html`](http://samba.anu.edu.au/rsync/examples.html)

#### 1.5.1. upload

```
$ rsync -v -u -a --delete --rsh=ssh --stats localfile username@hostname:/home/username/

```

for example:

I want to copy local workspace of eclipse directory to another computer.

```
$ rsync -v -u -a --delete --rsh=ssh --stats workspace neo@192.168.245.131:/home/neo/

```

#### 1.5.2. download

```
$ rsync -v -u -a --delete --rsh=ssh --stats neo@192.168.245.131:/home/neo/* /tmp/

```

#### 1.5.3. mirror

rsync 使用方法

rsync rsync://认证用户@主机/模块

```
rsync -vzrtopg --progress --delete 认证用户@主机::模块 /mirror 目录

```

#### 1.5.4. rsync delete

例 70.1. examples

用 rsync 删除目标目录

mkdir /root/blank
rsync --delete-before -a -H -v --progress --stats /root/blank/ ./cache/

#### 1.5.5. backup to a central backup server with 7 day incremental

例 70.2. backup to a central backup server with 7 day incremental

```

#!/bin/sh

# This script does personal backups to a rsync backup server. You will end up
# with a 7 day rotating incremental backup. The incrementals will go
# into subdirectories named after the day of the week, and the current
# full backup goes into a directory called "current"
# tridge@linuxcare.com

# directory to backup
BDIR=/home/$USER

# excludes file - this contains a wildcard pattern per line of files to exclude
EXCLUDES=$HOME/cron/excludes

# the name of the backup machine
BSERVER=owl

# your password on the backup server
export RSYNC_PASSWORD=XXXXXX

########################################################################

BACKUPDIR=`date +%A`
OPTS="--force --ignore-errors --delete-excluded --exclude-from=$EXCLUDES
      --delete --backup --backup-dir=/$BACKUPDIR -a"

export PATH=$PATH:/bin:/usr/bin:/usr/local/bin

# the following line clears the last weeks incremental directory
[ -d $HOME/emptydir ] || mkdir $HOME/emptydir
rsync --delete -a $HOME/emptydir/ $BSERVER::$USER/$BACKUPDIR/
rmdir $HOME/emptydir

# now the actual transfer
rsync $OPTS $BDIR $BSERVER::$USER/current

```

#### 1.5.6. backup to a spare disk

例 70.3. backup to a spare disk

```

I do local backups on several of my machines using rsync. I have an
extra disk installed that can hold all the contents of the main
disk. I then have a nightly cron job that backs up the main disk to
the backup. This is the script I use on one of those machines.

    #!/bin/sh

    export PATH=/usr/local/bin:/usr/bin:/bin

    LIST="rootfs usr data data2"

    for d in $LIST; do
	mount /backup/$d
	rsync -ax --exclude fstab --delete /$d/ /backup/$d/
	umount /backup/$d
    done

    DAY=`date "+%A"`

    rsync -a --delete /usr/local/apache /data2/backups/$DAY
    rsync -a --delete /data/solid /data2/backups/$DAY

The first part does the backup on the spare disk. The second part
backs up the critical parts to daily directories.  I also backup the
critical parts using a rsync over ssh to a remote machine.

```

#### 1.5.7. mirroring vger CVS tree

例 70.4. mirroring vger CVS tree

```

The vger.rutgers.edu cvs tree is mirrored onto cvs.samba.org via
anonymous rsync using the following script.

    #!/bin/bash

    cd /var/www/cvs/vger/
    PATH=/usr/local/bin:/usr/freeware/bin:/usr/bin:/bin

    RUN=`lps x | grep rsync | grep -v grep | wc -l`
    if [ "$RUN" -gt 0 ]; then
	    echo already running
	    exit 1
    fi

    rsync -az vger.rutgers.edu::cvs/CVSROOT/ChangeLog $HOME/ChangeLog

    sum1=`sum $HOME/ChangeLog`
    sum2=`sum /var/www/cvs/vger/CVSROOT/ChangeLog`

    if [ "$sum1" = "$sum2" ]; then
	    echo nothing to do
	    exit 0
    fi

    rsync -az --delete --force vger.rutgers.edu::cvs/ /var/www/cvs/vger/
    exit 0

Note in particular the initial rsync of the ChangeLog to determine if
anything has changed. This could be omitted but it would mean that the
rsyncd on vger would have to build a complete listing of the cvs area
at each run. As most of the time nothing will have changed I wanted to
save the time on vger by only doing a full rsync if the ChangeLog has
changed. This helped quite a lot because vger is low on memory and
generally quite heavily loaded, so doing a listing on such a large
tree every hour would have been excessive.

```

#### 1.5.8. automated backup at home

例 70.5. automated backup at home

```

I use rsync to backup my wifes home directory across a modem link each
night. The cron job looks like this

    #!/bin/sh
    cd ~susan
    {
    echo
    date
    dest=~/backup/`date +%A`
    mkdir $dest.new
    find . -xdev -type f \( -mtime 0 -or -mtime 1 \) -exec cp -aPv "{}"
    $dest.new \;
    cnt=`find $dest.new -type f | wc -l`
    if [ $cnt -gt 0 ]; then
      rm -rf $dest
      mv $dest.new $dest
    fi
    rm -rf $dest.new
    rsync -Cavze ssh . samba:backup
    } >> ~/backup/backup.log 2>&1

note that most of this script isn't anything to do with rsync, it just
creates a daily backup of Susans work in a ~susan/backup/ directory so
she can retrieve any version from the last week. The last line does
the rsync of her directory across the modem link to the host
samba. Note that I am using the -C option which allows me to add
entries to .cvsignore for stuff that doesn't need to be backed up.

```

#### 1.5.9. Fancy footwork with remote file lists

例 70.6. Fancy footwork with remote file lists

```

One little known feature of rsync is the fact that when run over a
remote shell (such as rsh or ssh) you can give any shell command as
the remote file list. The shell command is expanded by your remote
shell before rsync is called. For example, see if you can work out
what this does:

	rsync -avR remote:'`find /home -name "*.[ch]"`' /tmp/

note that that is backquotes enclosed by quotes (some browsers don't
show that correctly).

```

### 1.6. rsync for windows

http://www.rsync.net/resources/howto/windows_rsync.html

### 1.7. 多进程 rsync 脚本

```

#!/usr/bin/perl

my $path = "/data";          #本地目录
my $ip="172.16.xxx.xxx";     #远程目录
my $maxchild=5;              #同时并发的个数

open FILE,"ls $path|";
while()
{

        chomp;
        my $filename = $_;
        my $i = 1;
        while($i<=1){
                my $un = `ps -ef |grep rsync|grep -v grep |grep avl|wc -l`;
                $i =$i+1;
                if( $un < $maxchild){
                        system("rsync -avl --size-only $path/$_   $ip:$path &") ;
                }else{
                        sleep 5;
                        $i = 1;
                }
        }
}

```

## 2. tsync

homepage: http://tsyncd.sourceforge.net/

## 3. lsyncd

### 3.1. 安装

Ubuntu

```

apt install lsyncd		

```

CentOS

```

yum install lsyncd		

```

### 3.2. 配置 lsyncd.conf

```

vi etc/lsyncd.conf
settings {
    logfile      ="/var/log/lsyncd/lsyncd.log",
    statusFile   ="/var/log/lsyncd/lsyncd.status",
    inotifyMode  = "CloseWrite",
    maxProcesses = 7,
    -- nodaemon =true,
    }

sync {
    default.rsync,
    source    = "/tmp/src",
    target    = "/tmp/dest",
    -- excludeFrom = "/etc/rsyncd.d/rsync_exclude.lst",
    rsync     = {
        binary    = "/usr/bin/rsync",
        archive   = true,
        compress  = true,
        verbose   = true
        }
    }		

```

#### 3.2.1. lsyncd.conf 配置项说明

##### 3.2.1.1. settings 全局设置

```

logfile 定义日志文件
stausFile 定义状态文件
nodaemon=true 表示不启用守护模式，默认
statusInterval 将 lsyncd 的状态写入上面的 statusFile 的间隔，默认 10 秒
inotifyMode 指定 inotify 监控的事件，默认是 CloseWrite，还可以是 Modify 或 CloseWrite or Modify
maxProcesses 同步进程的最大个数。假如同时有 20 个文件需要同步，而 maxProcesses = 8，则最大能看到有 8 个 rysnc 进程
maxDelays 累计到多少所监控的事件激活一次同步，即使后面的 delay 延迟时间还未到

```

##### 3.2.1.2. sync 定义同步参数

可以继续使用 maxDelays 来重写 settings 的全局变量。一般第一个参数指定 lsyncd 以什么模式运行：rsync、rsyncssh、direct 三种模式：

```

default.rsync ：本地目录间同步，使用 rsync，也可以达到使用 ssh 形式的远程 rsync 效果，或 daemon 方式连接远程 rsyncd 进程；
default.direct ：本地目录间同步，使用 cp、rm 等命令完成差异文件备份；
default.rsyncssh ：同步到远程主机目录，rsync 的 ssh 模式，需要使用 key 来认证

source 同步的源目录，使用绝对路径。
target 定义目的地址，三种模式写法:
	/tmp/dest ：本地目录同步，可用于 direct 和 rsync 模式
	172.16.0.1:/tmp/dest ：同步到远程服务器目录，可用于 rsync 和 rsyncssh 模式
	172.16.0.1::module ：同步到远程服务器目录，用于 rsync 模式

init 这是一个优化选项，当 init = false，只同步进程启动以后发生改动事件的文件，原有的目录即使有差异也不会同步。默认是 true

delay 累计事件，等待 rsync 同步延时时间，默认 15 秒（最大累计到 1000 个不可合并的事件）。也就是 15s 内监控目录下发生的改动，会累积到一次 rsync 同步，避免过于频繁的同步。（可合并的意思是，15s 内两次修改了同一文件，最后只同步最新的文件）

excludeFrom 排除选项，后面指定排除的列表文件，如 excludeFrom = "/etc/lsyncd.exclude"，如果是简单的排除，可以使用 exclude = LIST。
这里的排除规则写法与原生 rsync 有点不同，更为简单：
监控路径里的任何部分匹配到一个文本，都会被排除，例如/bin/foo/bar 可以匹配规则 foo
如果规则以斜线/开头，则从头开始要匹配全部
如果规则以/结尾，则要匹配监控路径的末尾
?匹配任何字符，但不包括/
*匹配 0 或多个字符，但不包括/
**匹配 0 或多个字符，可以是/
delete 为了保持 target 与 souce 完全同步，Lsyncd 默认会 delete = true 来允许同步删除。它除了 false，还有 startup、running 值

```

##### 3.2.1.3. rsync

```

bwlimit 限速，单位 kb/s，与 rsync 相同（这么重要的选项在文档里竟然没有标出）
compress 压缩传输默认为 true。在带宽与 cpu 负载之间权衡，本地目录同步可以考虑把它设为 false
perms 默认保留文件权限。

```

### 3.3. 配置演示

```

settings {
    logfile ="/var/log/lsyncd.log",
    statusFile ="/var/log/lsyncd.status",
    inotifyMode = "CloseWrite",
    maxProcesses = 8,
    }

-- 本地目录同步，direct：cp/rm/mv。 适用：500+万文件，变动不大
sync {
    default.direct,
    source    = "/tmp/src",
    target    = "/tmp/dest",
    delay = 1
    maxProcesses = 1
    }

-- 本地目录同步，rsync 模式：rsync
sync {
    default.rsync,
    source    = "/tmp/src",
    target    = "/tmp/dest1",
    excludeFrom = "/etc/rsyncd.d/rsync_exclude.lst",
    rsync     = {
        binary = "/usr/bin/rsync",
        archive = true,
        compress = true,
        bwlimit   = 2000
        } 
    }

-- 远程目录同步，rsync 模式 + rsyncd daemon
sync {
    default.rsync,
    source    = "/tmp/src",
    target    = "www@192.168.0.1::module",
    delete="running",
    exclude = { ".*", ".tmp" },
    delay = 30,
    init = false,
    rsync     = {
        binary = "/usr/bin/rsync",
        archive = true,
        compress = true,
        verbose   = true,
        password_file = "/etc/rsyncd.d/rsync.pwd",
        _extra    = {"--bwlimit=200"}
        }
    }

-- 远程目录同步，rsync 模式 + ssh shell
sync {
    default.rsync,
    source    = "/tmp/src",
    target    = "www.netkiller.cn:/tmp/dest",
    -- target    = "root@www.netkiller.cn:/www/netkiller.cn/www.netkiller.cn",
    maxDelays = 5,
    delay = 30,
    -- init = true,
    rsync     = {
        binary = "/usr/bin/rsync",
        archive = true,
        compress = true,
        bwlimit   = 2000
        -- rsh = "/usr/bin/ssh -p 22 -o StrictHostKeyChecking=no"
        -- 如果要指定其它端口，请用上面的 rsh
        }
    }

-- 远程目录同步，rsync 模式 + rsyncssh，效果与上面相同
sync {
    default.rsyncssh,
    source    = "/tmp/src",
    host      = "www.netkiller.cn",
    targetdir = "/remote/dir",
    excludeFrom = "/etc/rsyncd.d/rsync_exclude.lst",
    -- maxDelays = 5,
    delay = 0,
    -- init = false,
    rsync    = {
        binary = "/usr/bin/rsync",
        archive = true,
        compress = true,
        verbose   = true,
        _extra = {"--bwlimit=2000"},
        },
    ssh      = {
        port  =  1234
        }
    }		

```

## 4. Unison File Synchronizer

If you are looking for a tool to sync your laptop with your workstation, you better have a look at Unison.

homepage: http://www.cis.upenn.edu/~bcpierce/unison/

installation

```
$ sudo apt-get install unison	

```

### 4.1. local

dir to dir

```
unison dir1 dir2		

```

### 4.2. remote

ssh

```
unison dir1 ssh://username@remotehostname(ip)//absolute/path/to/dir2		

```

socket

target host

```
# unison -socket NNNN	

```

source host

```
# unison dir1 socket://remotehost(ip):port//absolute/path/to/dir2			

```

### 4.3. config

create a config file under '.unison' directory.

```
vim ~/.unison/config.prf

root = /var/www
root = ssh://netkiller@netkiller.8800.org//var/www
force = /var/www
ignore = Path templates_compiled
ignore = Name tmp/*.pdf
auto = true
log = true
logfile = /home/netkiller/.unison/netkiller.8800.org.log

```

## 5. csync2 - cluster synchronization tool

homepage: [`oss.linbit.com/`](http://oss.linbit.com/)

### 5.1. server

过程 70.3. Install and setup csync2 on Ubuntu

1.  installation

    ```
    $ sudo apt-get install csync2 sqlite3 openssl xinetd

    ```

    The following line will be added to your /etc/inetd.conf file:

    ```
    $ cat /etc/inetd.conf
    csync2          stream  tcp     nowait  root    /usr/sbin/csync2        csync2 -i			

    ```

    If you are indeed using xinetd, you will have to convert the above into /etc/xinetd.conf format, and add it manually.

    ```
    service csync2
    {
    	disable = no
    	protocol = tcp
    	socket_type = stream
    	wait = no
    	user = root
    	server = /usr/sbin/csync2
    	server_args = -i
    }			

    ```

    /etc/services

    ```
    $ cat /etc/services |grep csync2
    csync2          30865/tcp                       # cluster synchronization tool

    ```

2.  create a self-signed SSL certificate for csync2

    ```
    sudo openssl genrsa -out /etc/csync2_ssl_key.pem 1024
    sudo openssl req -new -key /etc/csync2_ssl_key.pem -out /etc/csync2_ssl_cert.csr
    sudo openssl x509 -req -days 600 -in /etc/csync2_ssl_cert.csr -signkey /etc/csync2_ssl_key.pem -out /etc/csync2_ssl_cert.pem			

    ```

    ```
    $ sudo csync2 -k /etc/csync2_ssl_cert.key			

    ```

3.  After having done everything, we are now going to configure Csync2 so that we can determine which files are going to be synchronized.

    For this example, we are going to synchronize /etc/apache2 and /etc/mysql. For that we open /etc/csync2.cfg and we configure it like this:

    ```
    $ sudo vim /etc/csync2.cfg
    # please see the REAMDE file how to configure csync2

    group testing #group name, we can have multiple groups
    {
        host master; #master server
        host (slave); #slave server
        #host (node1);

        key /etc/csync2_ssl_cert.key;

        include /etc/apache2/;
        include /home/neo;

        backup-directory /var/backups/csync2;
        backup-generations 3;
        auto none; #no automatic sync
    }

    ```

4.  hosts

    ```
    $ sudo vim /etc/hosts			
    192.168.245.131 slave			

    ```

5.  restart

    ```
    $ sudo /etc/init.d/xinetd restart			

    ```

### 5.2. node

过程 70.4. node

1.  login to slave node

    ```
    neo@slave:~$ sudo vim /etc/hosts			
    192.168.245.129 master			

    ```

2.  install

    ```
    $ sudo apt-get install csync2 xinetd			

    ```

3.  copy config file from master

    ```
    neo@slave:~$  sudo scp root@master:/etc/csync2* /etc/			

    ```

4.  restart

    ```
    neo@slave:~$ sudo /etc/init.d/xinetd restart

    ```

### 5.3. test

过程 70.5. testing

1.  master

    ```
    neo@master:/etc/apache2$ sudo touch test.master
    neo@master:/etc/apache2$ sudo csync2 -x

    ```

2.  node

    ```
    neo@slave:/etc/apache2$ ls test.master -l
    -rw-r--r-- 1 root root 0 2008-10-31 06:37 test.master				

    ```

### 5.4. Advanced Configuration

例 70.7. /etc/csync2.cfg

```
$ sudo cat /etc/csync2.cfg

# please see the REAMDE file how to configure csync2
# group name, we can have multiple groups

group www {
    host master;
    host (slave);

    key /etc/csync2_ssl_cert.key;

    include /etc/apache2/;
    include /etc/csync2.cfg;
    include /var/www;
    include %homedir%/neo;
    exclude %homedir%/neo/temp;
    exclude *~ .*;

action
{
        pattern /etc/apache2/httpd.conf;
        pattern /etc/apache2/sites-available/*;
        exec "/usr/sbin/apache2ctl graceful";
        logfile "/var/log/csync2_action.log";
        do-local;
}

    backup-directory /var/backups/csync2;
    backup-generations 3;
    auto none;
}

prefix homedir
{
        on *: /home;
}			

```

### 5.5. 编译安装

过程 70.6. 

*   ```
    # yum install byacc -y

    ```

    ```

    # tar zxvf librsync-0.9.7.tar.gz
    # cd librsync-0.9.7
    ./configure --prefix=/usr/local/librsync-0.9.7
    # make && make install

    ```

    ```
    # www.sqlite.org
    # wget http://www.sqlite.org/sqlite-3.7.2.tar.gz
    # tar zxvf sqlite-3.7.2.tar.gz

    ```

    ```
    # www.gnu.org/software/gnutls/
    # wget http://ftp.gnu.org/pub/gnu/gnutls/gnutls-2.10.1.tar.bz2
    # tar jxvf gnutls-2.10.1.tar.bz2

    ```

    ```
    # wget http://oss.linbit.com/csync2/csync2-1.34.tar.gz
    # tar csync2-1.34.tar.gz
    # ./configure --prefix=/usr/local/csync2-1.34 --with-librsync-source=/usr/local/src/librsync-0.9.7.tar.gz --with-libsqlite-source=/usr/local/src/sqlite-3.7.2.tar.gz --disable-gnutls

    ```

## 6. synctool

synctool 是一个集群管理工具，用来在集群中的所有节点间进行保证配置文件的同步。节点可以是一个逻辑组和类的一部分，它们可能需要部分的配置文件。synctool 守护进程可以根据配置更改而对应用进行重启，还包括执行一些其他的管理任务。 新版本增加了一个新的工具 synctool-scp ，你可以使用这个工具来将文件复制到集群中的所有节点。

## 第 71 章 File Share

## 1. NFSv4

### 1.1. Ubuntu

#### 1.1.1. NFSv4 server

```
sudo apt-get install nfs-kernel-server

```

Configuration

```
vim /etc/exports
/www	 *(ro,sync,no_root_squash)
/home    *(rw,sync,no_root_squash)
/export       192.168.1.0/24(rw,fsid=0,insecure,no_subtree_check,async)
/export/users 192.168.1.0/24(rw,nohide,insecure,no_subtree_check,async)

```

To start the NFS server

```
sudo /etc/init.d/nfs-kernel-server start

```

#### 1.1.2. NFSv4 client

```
sudo apt-get install nfs-common

```

NFSv3

```
sudo mount example.hostname.com:/www /www

```

NFSv4

```
# mount -t nfs4 -o proto=tcp,port=2049 nfs-server:/ /mnt
# mount -t nfs4 -o proto=tcp,port=2049 nfs-server:/users /home/users

```

NFS Client Configuration

```
vim /etc/fstab
example.hostname.com:/ubuntu /local/ubuntu nfs rsize=8192,wsize=8192,timeo=14,intr

```

### 1.2. CentOS

#### 1.2.1. NFS Server Configuration

```
yum install -y nfs-utils

```

过程 71.1. On the *SERVER* side

1.  stop & disable services

    ```
    service nfs stop
    service nfslock stop
    service rpcbind stop
    service rpcidmapd stop

    ```

2.  /etc/fstab

    ```
    as root edit /etc/fstab and add nfs4 exports

    /www  /exports    none    bind    0 0

    ```

3.  as root edit /etc/exports

    NFSv3

    ```
    /exports 		172.16.1.0/24 (rw,sync)

    ```

    NFSv4

    ```
    /exports 		172.16.1.0/24(rw,sync,fsid=0,anonuid=99,anongid=99)
    /exports/neo	*(rs,sync)

    ```

4.  reload exported filesystems

    ```
    # exportfs -rv

    ```

5.  start required services

    ```
    chkconfig rpcbind on
    chkconfig nfs on
    chkconfig nfslock on
    chkconfig rpcidmapd on

    service rpcbind start
    service rpcidmapd start
    service nfs start
    service nfslock start

    ```

6.  nfs status

    ```
    #  nfsstat
    Server rpc stats:
    calls      badcalls   badauth    badclnt    xdrcall
    171        0          0          0          0

    Server nfs v3:
    null         getattr      setattr      lookup       access       readlink
    3         1% 150      88% 0         0% 3         1% 2         1% 0         0%
    read         write        create       mkdir        symlink      mknod
    0         0% 0         0% 0         0% 0         0% 0         0% 0         0%
    remove       rmdir        rename       link         readdir      readdirplus
    0         0% 0         0% 0         0% 0         0% 0         0% 9         5%
    fsstat       fsinfo       pathconf     commit
    0         0% 3         1% 0         0% 0         0%

    ```

    ```
    # watch nfsstat -c

    Every 2.0s: nfsstat -c                                                                                                                          Mon Sep 20 16:53:55 2010

    Client rpc stats:
    calls      retrans    authrefrsh
    286818929   1160       0

    Client nfs v4:
    null         read         write        commit       open         open_conf
    0         0% 37286763 13% 6         0% 1         0% 38990106 13% 17986485  6%
    open_noat    open_dgrd    close        setattr      fsinfo       renew
    6         0% 0         0% 38774539 13% 2172019   0% 16        0% 147       0%
    setclntid    confirm      lock         lockt        locku        access
    321       0% 321       0% 0         0% 0         0% 0         0% 62157123 21%
    getattr      lookup       lookup_root  remove       rename       link
    80553542 28% 8828991   3% 8         0% 5         0% 5         0% 0         0%
    symlink      create       pathconf     statfs       readlink     readdir
    0         0% 1         0% 0         0% 5         0% 0         0% 13933     0%
    server_caps  delegreturn
    24        0% 54556     0%

    ```

7.  security

    ```
    # vi /etc/hosts.deny
    rpcbind:ALL

    # vi /etc/hosts.allow
    rpcbind:172.16.1.0/255.255.254.0

    ```

NFS 的队列大小下面将设置为较合理的值 256K

```
# echo 262144 > /proc/sys/net/core/rmem_default
# echo 262144 > /proc/sys/net/core/rmem_max
# echo 262144 > /proc/sys/net/core/wmmen_default
# echo 262144 > /proc/sys/net/core/wmmen_max

```

过程 71.2. NFSv4

1.  /etc/exports

    ```
    # cat /etc/exports
    /www		172.16.1.2/32(ro,sync,fsid=0,anonuid=99,anongid=99)
    /www/logs	*(rw,sync)

    ```

    注意，要通过 NFS4 共享一个目录,必须使用 fsid=0 的参数,使用 fsid=0 选项的时候只能共享一个目录，这个目录将成为 NFS 服务器的根目录。

2.  启动 NFS,v4 不需要 rpcbind

    ```
    service rpcbind stop
    service rpcidmapd stop
    service nfs restart
    service nfslock stop

    ```

3.  查看 export 设置

    ```
    # exportfs
    /www          	172.16.1.2/32
    /www/logs     	172.16.1.0/24

    ```

4.  mount NFSv4

    ```
    mount -t nfs4 172.16.1.15:/logs /mnt

    ```

##### 1.2.1.1. NFS 防火墙配置

查看 NFS 正在使用的端口

```
rpcinfo -p localhost			

```

vi /etc/sysconfig/nfs

```
LOCKD_TCPPORT=32803
LOCKD_UDPPORT=32769
MOUNTD_PORT=892
RQUOTAD_PORT=875
STATD_PORT=662
STATD_OUTGOING_PORT=2020

```

```
service nfs restart

```

```
iptables -I INPUT -m state --state NEW -p tcp \
    -m multiport --dport 111,892,2049,32803 -s 192.168.0.0/24 -j ACCEPT

iptables -I INPUT -m state --state NEW -p udp \
    -m multiport --dport 111,892,2049,32769 -s 192.168.0.0/24 -j ACCEPT		

```

#### 1.2.2. NFS Client Configuration

CentOS 6 NFSv3 portmap 已经不存,已经被 rpcbind 替代

```
chkconfig rpcbind on
service rpcbind start

```

test nfs

```
mount 172.16.1.10:/exports /mnt

```

NFSv4

```
mount -t nfs4 -o ro,intr 172.16.1.10:/ /mnt

```

```
umount /mnt

```

过程 71.3. On the *CLIENT* side

1.  Mounting NFS File Systems using /etc/fstab

    The general syntax for the line in /etc/fstab is as follows:

    ```
    server:/usr/local/pub    /pub   nfs    rsize=8192,wsize=8192,timeo=14,intr

    ```

    NFSv4

    ```
    server:/ /mount/point nfs4 rw,hard,intr,proto=tcp,port=2049,auto 0 0

    ```

2.  mount all stuff from /etc/fstab

    ```
    # mount -a

    ```

3.  rpcinfo

    ```
    rpcinfo -p
       program vers proto   port
        100000    2   tcp    111  portmapper
        100000    2   udp    111  portmapper
        100024    1   udp    707  status
        100024    1   tcp    710  status
        100021    1   udp  48233  nlockmgr
        100021    3   udp  48233  nlockmgr
        100021    4   udp  48233  nlockmgr
        100021    1   tcp  58065  nlockmgr
        100021    3   tcp  58065  nlockmgr
        100021    4   tcp  58065  nlockmgr

    ```

4.  start required services

    centos 5.x

    ```
    chkconfig portmap on
    service portmap start

    ```

    centos 6

    ```
    chkconfig rpcbind on
    service rpcbind start

    ```

##### 1.2.2.1. Using NFS over UDP

For example, on demand via the command line (client side):

```
mount -o udp shadowman.example.com:/misc/export /misc/local

```

When the NFS mount is specified in /etc/fstab (client side):

```
server:/usr/local/pub    /pub   nfs    rsize=8192,wsize=8192,timeo=14,intr,udp

```

### 1.3. exports

#### 1.3.1. Permission

```
/etc/exports 为：

/tmp　　　　　*(rw,no_root_squash)

/home/public　192.168.0.*(rw)　　 *(ro)

/home/test　　192.168.0.100(rw)

/home/linux　 *.example.com(rw,all_squash,anonuid=40,anongid=40)

```

#### 1.3.2. Parameters

General Options

```
ro                      只读访问
rw                      读写访问
rsize					同时传输(读 )的数据块大小
wsize					同时传输(写)的数据块大小

sync                    所有数据在请求时写入共享
async                   NFS 在写入数据前可以相应请求

secure                  NFS 通过 1024 以下的安全 TCP/IP 端口发送
insecure                NFS 通过 1024 以上的端口发送
wdelay                  如果多个用户要写入 NFS 目录，则归组写入（默认）
no_wdelay               如果多个用户要写入 NFS 目录，则立即写入，当使用 async 时，无需此设置。
hide                    在 NFS 共享目录中不共享其子目录
no_hide                 共享 NFS 目录的子目录
subtree_check           如果共享/usr/bin 之类的子目录时，强制 NFS 检查父目录的权限（默认）
no_subtree_check        和上面相对，不检查父目录权限

```

User ID Mapping

```
all_squash              共享文件的 UID 和 GID 映射匿名用户 anonymous，适合公用目录。
no_all_squash           保留共享文件的 UID 和 GID（默认）
root_squash             root 用户的所有请求映射成如 anonymous 用户一样的权限（默认）
no_root_squas           root 用户具有根目录的完全管理访问权限
anonuid=xxx             指定 NFS 服务器/etc/passwd 文件中匿名用户的 UID
anongid=xxx             指定 NFS 服务器/etc/passwd 文件中匿名用户的 GID

```

#### 1.3.3. 实例参考

只读挂载

```
172.16.2.5:/   /www/images   nfs4       ro,rsize=8192,wsize=8192,timeo=15,intr,noac

```

### 1.4. NFS For Windows

安装 NFS 服务，进入“控制面板”，点击“打开或关闭 Windows 功能”，再勾选“NFS 服务”，最后确定

启动 NFS 服务，控制面板\管理工具\Network File System 服务(NFS)

或者通过命令启动 NFS 服务

```
nfsadmin client [ComputerName] start

```

指定挂在用户 ID，开始“运行”输入“regedit”回车，然后找到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default，右键“新建”选择“DWORD(32 为)值” 添加 AnonymousUid，AnonymousGid，然后双击 AnonymousUid，AnonymousGid 编辑，选择十进制并输入用户 ID。

重新启动 NFS 服务，不需要重新启动计算机。

挂载文件系统

```

C:\Users\neo>mount \\192.168.2.15\www x:\

```

卸载文件系统

```

C:\Users\neo>umount x:

正在断开                x:      \\192.168.2.15\www
连接上存在打开的文件和/或未完成的目录搜索。

要继续此操作吗? (Y/N) [N]:Y

命令已成功完成。

```

### 提示

很不幸 Microsoft Windows 目前尚不支持 UTF-8 字符集。

### 1.5. exportfs - maintain table of exported NFS file systems

```
# exportfs -o rw,all_squash,sync,anonuid=500,anongid=500 172.16.0.0/24:/www
# exportfs
/www          	172.16.0.0/24

# cat /var/lib/nfs/etab
/www	172.16.0.0/24(rw,sync,wdelay,hide,nocrossmnt,secure,root_squash,all_squash,no_subtree_check,secure_locks,acl,anonuid=500,anongid=500)

```

reload /etc/exports

```
/usr/sbin/exportfs -r

```

To unexport the /usr/tmp directory:

```
# exportfs -u netkiller.github.com:/usr/tmp

```

To unexport all exports listed in /etc/exports:

```
# exportfs -au

```

```
#!/bin/bash
RETVAL=0

start()
{
	/usr/sbin/exportfs -o rw,all_squash,sync,anonuid=500,anongid=500 172.16.0.0/24:/backup
	mount /dev/sdb1 /backup
	RETVAL=$?
	echo
}

stop()
{
	exportfs -u 172.16.0.0/24:/backup
	umount /backup
	RETVAL=$?
}

```

## 2. Parallel NFS(pNFS)

## 3. Samba

### 3.1. install

#### 3.1.1. Ubuntu

环境 ubuntu 17.10

```
$ sudo apt install samba

```

查看 Samba 服务器的端口

```
neo@shenzhen:~$ sudo netstat -tlnp |grep smb
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN     4480/smbd
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN     4480/smbd
neo@shenzhen:~$

```

#### 3.1.2. Samba

```
# yum -y install samba
# service smbd start

```

smbpasswd

```
[root@development ~]# sudo smbpasswd -L -a neo		

```

smb.conf

```
#============================ Share Definitions ==============================

[homes]
        comment = Home Directories
        browseable = no
        writable = yes
        valid users = %S

[developer]
        comment = Developer Stuff
        path = /var/www/html
        public = yes
        writable = yes
        printable = no
        write list = +apache

```

#### 3.1.3. CentOS 7

```
yum install -y samba

cp /etc/samba/smb.conf{,.original}

systemctl enable smb
systemctl start smb

```

#### 3.1.4. firewall

防火墙

```
firewall-cmd --permanent --add-port=137/tcp
firewall-cmd --permanent --add-port=138/tcp
firewall-cmd --permanent --add-port=139/tcp
firewall-cmd --permanent --add-port=445/tcp
firewall-cmd --permanent --add-port=901/tcp

firewall-cmd --reload

```

iptables -L

#### 3.1.5. SELinux Configuration

```
setsebool -P samba_enable_home_dirs on
chcon -t samba_share_t /home/samba

```

/home/samba 改为你共享的目录

### 3.2. smb.conf

security = share|user 共享|用户模式

```
comment = 描述
valid users = '%S'登录用户，'neo'允许 neo 访问
read only = 'No'读写模式，'Yes'只读模式
browseable = 'No'不显示, 'Yes'显示

```

#### 3.2.1. Security consideration

```
[global]
interfaces = lo, eth0
bind interfaces only = true

```

### 3.3. testparm - check an smb.conf configuration file for internal correctness

```
# testparm
Load smb config files from /etc/samba/smb.conf
rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
Processing section "[homes]"
Processing section "[printers]"
Loaded services file OK.
Server role: ROLE_STANDALONE
Press enter to see a dump of your service definitions

[global]
	workgroup = MYGROUP
	server string = Samba Server Version %v
	log file = /var/log/samba/log.%m
	max log size = 50
	idmap config * : backend = tdb
	cups options = raw

[homes]
	comment = Home Directories
	read only = No
	browseable = No

[printers]
	comment = All Printers
	path = /var/spool/samba
	printable = Yes
	print ok = Yes
	browseable = No

```

### 3.4. smbstatus - report on current Samba connections

```
# smbstatus 

Samba version 4.1.12
PID     Username      Group         Machine                        
-------------------------------------------------------------------

Service      pid     machine       Connected at
-------------------------------------------------------

No locked files

```

链接共享目录后再次查看

```
# smbstatus 

Samba version 4.1.12
PID     Username      Group         Machine                        
-------------------------------------------------------------------
12507     www           www           192.168.4.69 (ipv4:192.168.4.69:65102)

Service      pid     machine       Connected at
-------------------------------------------------------
www          12507   192.168.4.69  Wed Sep 23 01:34:44 2015
IPC$         12507   192.168.4.69  Wed Sep 23 01:34:43 2015

Locked files:
Pid          Uid        DenyMode   Access      R/W        Oplock           SharePath   Name   Time
--------------------------------------------------------------------------------------------------
12507        80         DENY_NONE  0x100081    RDONLY     NONE             /www   SOA   Wed Sep 23 02:01:22 2015
12507        80         DENY_NONE  0x100081    RDONLY     NONE             /www   SOA/queue   Wed Sep 23 02:01:22 2015
12507        80         DENY_NONE  0x100081    RDONLY     NONE             /www   .   Wed Sep 23 01:37:53 2015
12507        80         DENY_NONE  0x100081    RDONLY     NONE             /www   .   Wed Sep 23 01:58:22 2015

```

### 3.5. smbpasswd - change a user's SMB password

```
# smbpasswd -a www
New SMB password:
Retype new SMB password:
Added user www.

```

### 3.6. nmblookup - NetBIOS over TCP/IP client used to lookup NetBIOS names

```

$ nmblookup -A 172.16.0.5
Looking up status of 172.16.0.5
        USER            <00> -         B <ACTIVE>
        WORKGROUP       <00> - <GROUP> B <ACTIVE>
        USER            <20> -         B <ACTIVE>
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>
        WORKGROUP       <1d> -         B <ACTIVE>
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>

        MAC Address = 00-25-64-A7-18-97

```

### 3.7. smbfs/smbmount/smbumount

```
sudo apt-get install smbfs

```

smbmount

```
$ sudo mkdir /mnt/winfs
$ sudo smbmount //172.16.0.92/tmp /mnt/winfs
$ ls /mnt/winfs/

```

使用 neo 帐号登录

```
$ sudo smbmount //172.16.0.92/tmp /mnt/winfs -o username=neo

```

mount

```
$ mount -t smbfs -o username=jwhittal \\\\172.16.1.3\\c$ /mnt/thumb

```

linux 不再使用 smbfs, 替换为 cifs

```
$ mount -t cifs //192.168.0.2/ /mnt/

```

### 3.8. smbclient - ftp-like client to access SMB/CIFS resources on servers

```
$ sudo apt-get install smbclient

```

#### 3.8.1. 显示共享目录

**$ smbclient -L 172.16.1.3**

```
neo@netkiller:~$ smbclient -L 172.16.0.1
Enter neo's password:
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.4.0]

        Sharename       Type      Comment
        ---------       ----      -------
        IPC$            IPC       IPC Service (netkiller server (Samba, Ubuntu))
        www             Disk      www diretcory
        print$          Disk      Printer Drivers
        neo             Disk      Home Directories
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.4.0]

        Server               Comment
        ---------            -------
        DEBIAN               debian server
        NETKILLER            netkiller server (Samba, Ubuntu)

        Workgroup            Master
        ---------            -------
        WORKGROUP            DEBIAN

```

#### 3.8.2. 访问共享资源

访问 developer 共享目录

```
$ smbclient //localhost/developer

Enter neo's password:
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.3.2]
Server not using user level security and no password supplied.
smb: \> ls
  .                                   D        0  Thu Oct 29 02:05:37 2009
  ..                                  D        0  Thu Oct 22 05:27:16 2009
  ofcard.php                                1104  Tue Oct 27 02:00:49 2009
  index.html                                 580  Thu Oct 29 02:05:37 2009
  webapps                             D        0  Wed Oct 28 06:04:08 2009
  ecmall                              D        0  Thu Oct 22 00:00:12 2009
  doc                                 D        0  Wed Oct 28 06:04:09 2009
  supersite                           D        0  Thu Oct 22 03:35:08 2009
  empire                              D        0  Thu Oct 22 02:56:12 2009
  discuz                              D        0  Wed Oct 21 22:04:29 2009
  resin-data                          D        0  Wed Oct 28 06:21:02 2009
  phpMyAdmin                          D        0  Sat Oct 24 09:02:29 2009
  empirecms6                          D        0  Thu Oct 22 04:12:44 2009
  ecshop                              D        0  Wed Oct 21 21:56:40 2009
  watchdog-data                       D        0  Wed Oct 28 06:07:19 2009
  ucenter                             D        0  Wed Oct 21 22:41:58 2009
  ecshop.old                          D        0  Fri Oct 23 11:35:39 2009
  magento                             D        0  Tue Oct  6 19:19:54 2009
  weberp                              D        0  Fri Oct 23 05:21:33 2009

                61335 blocks of size 131072\. 41655 blocks available
smb: \>

```

#### 3.8.3. 用户登录

使用用户 Neo 登录

```
$ smbclient //localhost/developer -U neo

Enter neo's password:
Domain=[UBUNTU] OS=[Unix] Server=[Samba 3.3.2]
smb: \> ls
  .                                   D        0  Thu Oct 29 03:13:31 2009
  ..                                  D        0  Thu Oct 22 05:27:16 2009
  ofcard.php                                1104  Tue Oct 27 02:00:49 2009
  index.html                                 676  Thu Oct 29 03:13:31 2009
  webapps                             D        0  Wed Oct 28 06:04:08 2009
  ecmall                              D        0  Thu Oct 22 00:00:12 2009
  doc                                 D        0  Wed Oct 28 06:04:09 2009
  supersite                           D        0  Thu Oct 22 03:35:08 2009
  empire                              D        0  Thu Oct 22 02:56:12 2009
  discuz                              D        0  Wed Oct 21 22:04:29 2009
  resin-data                          D        0  Wed Oct 28 06:21:02 2009
  phpMyAdmin                          D        0  Sat Oct 24 09:02:29 2009
  empirecms6                          D        0  Thu Oct 22 04:12:44 2009
  ecshop                              D        0  Wed Oct 21 21:56:40 2009
  watchdog-data                       D        0  Wed Oct 28 06:07:19 2009
  ucenter                             D        0  Wed Oct 21 22:41:58 2009
  ecshop.old                          D        0  Fri Oct 23 11:35:39 2009
  magento                             D        0  Tue Oct  6 19:19:54 2009
  weberp                              D        0  Fri Oct 23 05:21:33 2009

                61335 blocks of size 131072\. 41654 blocks available
smb: \> quit

```

### 3.9. smbtar - shell script for backing up SMB/CIFS shares directly to UNIX tape drives

### 3.10. by Example

Backup the /etc/samba/smb.conf file:

```
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original

```

#### 3.10.1. share

security = share

```

[tmp]
   comment = test
   writable = yes
   locking = yes
   path = /tmp
   public = yes

[neo]
   comment = neo
   writable = yes
   locking = yes
   path = /home/neo/
   public = yes

[htdocs]
   comment = neo
   writable = yes
   locking = yes
   path = /opt/lampp/htdocs
   public = yes

```

#### 3.10.2. user

```
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original

```

```
security = user

```

add user

```
sudo useradd -s /bin/true neo
sudo smbpasswd -L -a neo

```

enable

```
sudo smbpasswd -L -e neo

```

del user

```
sudo smbpasswd -L -x neo

```

#### 3.10.3. test

测试配置文件是否正确

```
$ testparm

```

查看共享目录

```
$ smbclient -L localhost -N

Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.3.2]

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        developer       Disk      Development
        IPC$            IPC       IPC Service (ubuntu server (Samba, Ubuntu))
Domain=[WORKGROUP] OS=[Unix] Server=[Samba 3.3.2]

        Server               Comment
        ---------            -------
        PRINTSERVER
        UBUNTU               ubuntu server (Samba, Ubuntu)

        Workgroup            Master
        ---------            -------
        WORKGROUP            PRINTSERVER

```

Windows 访问测试

```
C:\>net view \\192.168.3.40
在 \\192.168.3.40 的共享资源

ubuntu server (Samba, Ubuntu)

共享名     类型  使用为  注释

----------------------------------------------------------
developer  Disk  Development
命令运行完毕，但发生一个或多个错误。

```

### 3.11. FAQ

#### 3.11.1. smbd/service.c:make_connection_snum(1013)

```
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:08, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:08, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:11, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:13, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:13, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:13, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied
[2010/05/17 17:26:13, 0] smbd/service.c:make_connection_snum(1013)
  '/www' does not exist or permission denied when connecting to [www] Error was Permission denied

```

关闭 SELinux

## 第 72 章 Distributed File Systems

## 1. DRBD (Distributed Replicated Block Device)

Homepage: http://www.drbd.org/

![](img/drbd.gif)![](img/drbd_ha.gif)

实验环境需要两台电脑，如果你没有，建议你使用 VMware，并且为每一个虚拟机添加两块硬盘。

实验环境

1.  master: 192.168.0.1 DRBD:/dev/sdb

2.  slave: 192.168.0.2 DRBD:/dev/sdb

### 1.1. disk and partition

Each of the following steps must be completed on both nodes

show all of disk and partition

```
neo@master:~$ sudo sfdisk -s
/dev/sda:   8388608
/dev/sdb:   2097152
total: 10485760 blocks

```

create a new partition on the disk /dev/sdb

```
$ sudo cfdisk /dev/sdb			

```

you must have extended partition

check partition

```
neo@master:~$ sudo fdisk -l

Disk /dev/sda: 8589 MB, 8589934592 bytes
255 heads, 63 sectors/track, 1044 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x000301bd

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1         993     7976241   83  Linux
/dev/sda2             994        1044      409657+   5  Extended
/dev/sda5             994        1044      409626   82  Linux swap / Solaris

Disk /dev/sdb: 2147 MB, 2147483648 bytes
255 heads, 63 sectors/track, 261 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x00000000

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1               1         261     2096451    5  Extended
/dev/sdb5               1         261     2096419+  83  Linux

```

format /dev/sdb1

```
neo@master:~$ sudo mkfs.ext3 /dev/sdb1

```

you also can using other file system

reiserfs

```
neo@master:~$ sudo mkfs.reiserfs /dev/sdb1		

```

I suggest you using reiserfs.

### 1.2. Installation

Each of the following steps must be completed on both nodes

search drbd8-utils package

```
neo@master:~$ apt-cache search drbd
drbd8-utils - RAID 1 over tcp/ip for Linux utilities
drbd0.7-module-source - RAID 1 over tcp/ip for Linux module source
drbd0.7-utils - RAID 1 over tcp/ip for Linux utilities
drbdlinks - Manages symlinks into a shared DRBD partition

```

installation

```
neo@master:~$ sudo apt-get install drbd8-utils			

```

to add modules from the Linux Kernel

```
neo@master:~$ sudo modprobe drbd
neo@master:~$ lsmod |grep drbd
drbd                  213000  0
cn                      9632  1 drbd

```

### 1.3. configure

Each of the following steps must be completed on both nodes

backup configure file

```

neo@master:~$ sudo cp /etc/drbd.conf /etc/drbd.conf.old

```

edit /etc/drbd.conf

```
global { 
  usage-count yes; 
}
common {
  protocol C;
}
resource r0 {
  on master {
    device    /dev/drbd0;
    disk      /dev/sdb5;
    address   192.168.0.1:7789;
    meta-disk internal;
  }
  on slave {
    device    /dev/drbd0;
    disk      /dev/sdb5;
    address   10.1.1.32:7789;
    meta-disk internal;
  }
}

```

### 1.4. Starting

Each of the following steps must be completed on both nodes.

```
neo@master:~$ sudo drbdadm create-md r0	
neo@master:~$ sudo drbdadm attach r0
neo@master:~$ sudo drbdadm connect r0
neo@master:~$ sudo drbdadm -- --overwrite-data-of-peer primary r0

neo@slave:~$ sudo drbdadm create-md r0		
neo@slave:~$ sudo drbdadm attach r0
neo@slave:~$ sudo drbdadm connect r0

```

master

```

neo@master:~$ sudo drbdadm create-md r0
v08 Magic number not found
md_offset 2146725888
al_offset 2146693120
bm_offset 2146627584

Found some data
 ==> This might destroy existing data! <==

Do you want to proceed?
[need to type 'yes' to confirm] yes

v07 Magic number not found
v07 Magic number not found
v08 Magic number not found
Writing meta data...
initialising activity log
NOT initialized bitmap
New drbd meta data block sucessfully created.
success

```

slave

```

neo@slave:~# sudo drbdadm create-md r0
v08 Magic number not found
md_offset 2146725888
al_offset 2146693120
bm_offset 2146627584

Found some data
 ==> This might destroy existing data! <==

Do you want to proceed?
[need to type 'yes' to confirm] yes

v07 Magic number not found
v07 Magic number not found
v08 Magic number not found
Writing meta data...
initialising activity log
NOT initialized bitmap
New drbd meta data block sucessfully created.
success

```

status

```
neo@master:~$ cat /proc/drbd
version: 8.0.11 (api:86/proto:86)
GIT-hash: b3fe2bdfd3b9f7c2f923186883eb9e2a0d3a5b1b build by phil@mescal, 2008-02-12 11:56:43
 0: cs:StandAlone st:Primary/Unknown ds:UpToDate/DUnknown   r---
    ns:0 nr:0 dw:0 dr:0 al:0 bm:0 lo:0 pe:0 ua:0 ap:0
        resync: used:0/31 hits:0 misses:0 starving:0 dirty:0 changed:0
        act_log: used:0/127 hits:0 misses:0 starving:0 dirty:0 changed:0
 1: cs:Connected st:Secondary/Secondary ds:Diskless/Inconsistent C r---
    ns:0 nr:0 dw:0 dr:0 al:0 bm:0 lo:0 pe:0 ua:0 ap:0

```

### 1.5. Using

master

```
neo@master:~$ sudo drbdadm primary all		
neo@master:~$ sudo mkfs.reiserfs /dev/drbd0
neo@master:~$ sudo mkdir /mnt/drbd0
neo@master:~$ sudo mount /dev/drbd0 /mnt/drbd0/
neo@master:~$ sudo touch /mnt/drbd0/helloworld.tmp
neo@master:~$ df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1             7.6G  1.3G  6.0G  18% /
varrun                125M  216K  125M   1% /var/run
varlock               125M  8.0K  125M   1% /var/lock
udev                  125M   60K  125M   1% /dev
devshm                125M     0  125M   0% /dev/shm
/dev/drbd0            2.0G   33M  2.0G   2% /mnt/drbd0
neo@master:~$ sudo dd if=/dev/zero of=/mnt/drbd0/tempfile1.tmp bs=104857600 count=1
1+0 records in
1+0 records out
104857600 bytes (105 MB) copied, 0.564911 s, 186 MB/s
neo@master:~$ sudo umount /mnt/drbd0/
neo@master:~$ sudo drbdadm secondary all

```

slave

```
neo@slave:~$ sudo drbdadm primary all			
neo@slave:~$ sudo mkdir /mnt/drbd0
neo@slave:~$ sudo mount /dev/drbd0 /mnt/drbd0/
neo@slave:~$ ls /mnt/drbd0/
helloworld.tmp  tempfile1.tmp			

```

## 2. Network Block Device protocol

### 2.1. nbd-server - Network Block Device protocol - server

```
apt-get install nbd-server		

# modprobe nbd
# mkdir -p /home/exported
# dd if=/dev/zero of=/home/exported/trial.img count=256 bs=1024k
# mkfs.ext3 /home/exported/trial.img

# nbd-server 1234 /home/exported/trial.img

# touch /root/empty
# nbd-server 1234 /home/exported/trial.img -C /root/empty

```

### 2.2. nbd-client - Network Block Device protocol - client

```
# apt-get install nbd-client

# nbd-client mine.my.flat 1234 /dev/nbd0
Negotiation: ..size = 262144KB
bs=1024, sz=262144

# mkdir /mnt/remote
# mount /dev/nbd0 /mnt/remote
# for i in $(seq 1 100) ; do echo $i > /mnt/remote/$i; done

# umount /mnt/remote 

root@vain:~#  nbd-client 127.0.0.1 1234 /dev/nbd0
root@vain:~# mkdir /tmp/foo
root@vain:~#  mount /dev/nbd0 /tmp/foo
root@vain:~#  ls /tmp/foo/
1    14  2   25  30  36  41  47  52  58  63  69  74  8   85  90  96
10   15  20  26  31  37  42  48  53  59  64  7   75  80  86  91  97
100  16  21  27  32  38  43  49  54  6   65  70  76  81  87  92  98
11   17  22  28  33  39  44  5   55  60  66  71  77  82  88  93  99
12   18  23  29  34  4   45  50  56  61  67  72  78  83  89  94  
13   19  24  3   35  40  46  51  57  62  68  73  79  84  9   95  lost+found

```

## 3. GridFS

http://www.mongodb.org/display/DOCS/GridFS

GridFS 类似 MogileFS

### 3.1. nginx-gridfs

http://github.com/mdirolf/nginx-gridfs

```

yum -y install pcre-devel

wget http://nginx.org/download/nginx-1.2.3.tar.gz
tar zxvf nginx-1.2.3.tar.gz

./configure --prefix=/srv/nginx-1.2.3 \
--sbin-path=/srv/nginx-1.2.3/sbin/nginx \
--conf-path=/srv/nginx-1.2.3/conf/nginx.conf \
--user=www --group=www \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--pid-path=/var/run/nginx.pid \
--lock-path=/var/run/nginx.lock \
--http-client-body-temp-path=/var/cache/nginx/client_temp \
--http-proxy-temp-path=/var/cache/nginx/proxy_temp \
--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
--http-scgi-temp-path=/var/cache/nginx/scgi_temp \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_stub_status_module \
--with-mail --with-mail_ssl_module \
--with-file-aio \
--with-cc-opt='-O2 -g' \
--add-module=/usr/local/src/nginx-gridfs

make && make install

```

配置语法说明：

```
gridfs DB_NAME [root_collection=ROOT] [field=QUERY_FIELD] [type=QUERY_TYPE] [user=USERNAME] [pass=PASSWORD]

gridfs 表示告诉 nginx 服务器要调用 gridfs 模块
root_collection= 指定 Gridfs collection 的前缀. 默认: fs
field= 指定用于查询的字段 可以是 _id 和 filename. 默认: _id
type= 指定查询的类型，这里支持 objectid, string 和 int. 默认: objectid
user= 指定数据库的用户名. 默认: NULL, 可省略
pass= 指定数据库的密码. 默认: NULL, 可省略

```

Nginx 配置文件中的具体写法：

```
location /images/ {
     gridfs images
     field=_id
     type=objectid;
     mongo 127.0.0.1:27017;
}

```

上传图片

```
sudo /srv/mongodb/bin/mongofiles put --host localhost --port 27017 --db images --local ~/photo.jpg --type jpg

```

在浏览器里输入 http://localhost/images/photo.jpg 能显示图片就说明成功了

例 72.1. nginx-gridfs

```
#指定 db 为 static，其它均为默认，默认服务器为本地
location /static/ {

	gridfs static;

}

location /static/ {

        gridfs static
        field=filename
        type=string;
        mongo 127.0.0.1:27017;

}

location /static/ {
	gridfs static;
	    field=filename
	    type=string;
	mongo "foo"
	    172.16.1.1:27017
	    172.16.1.2:27017;

}

location /static/ {

    gridfs static
    root_collection=images
    field=_id
    type=int
    user=admin
    pass=pass;
    mongo 127.0.0.1:27017;

}

```

### 3.2. lighttpd-gridfs

[`bitbucket.org/bwmcadams/lighttpd-gridfs/src/`](https://bitbucket.org/bwmcadams/lighttpd-gridfs/src/)

## 4. Moose File System

http://www.moosefs.org/

### 4.1. Master server installation

```
groupadd mfs
useradd -g mfs mfs
cd /usr/local/src
wget http://pro.hit.gemius.pl/hitredir/id=nXCV9nrckU2Et.zoR5kxdXZJLQq1fqbG4AIiq5K95Gz.07/url=moosefs.org/tl_files/mfscode/mfs-1.6.19.tar.gz
tar zxvf mfs-1.6.19.tar.gz
cd mfs-1.6.19
./configure --prefix=/srv/mfs \
--with-default-user=mfs \
--with-default-group=mfs \
--disable-mfschunkserver \
--disable-mfsmount

make
make install

```

```
cd /srv/mfs/etc/
cp /srv/mfs/var/mfs/metadata.mfs.empty /srv/mfs/var/mfs/metadata.mfs

cp mfsexports.cfg.dist mfsexports.cfg
cp mfsmaster.cfg.dist mfsmaster.cfg
cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
vim mfsmaster.cfg	

```

```
WORKING_USER = mfs
WORKING_GROUP = mfs
SYSLOG_IDENT = mfsmaster
LOCK_MEMORY = 0
NICE_LEVEL = -19

EXPORTS_FILENAME = /srv/mfs/etc/mfsexports.cfg

DATA_PATH = /srv/mfs/var/mfs

BACK_LOGS = 50

REPLICATIONS_DELAY_INIT = 300
REPLICATIONS_DELAY_DISCONNECT = 3600

MATOML_LISTEN_HOST = *
MATOML_LISTEN_PORT = 9419

MATOCS_LISTEN_HOST = *
MATOCS_LISTEN_PORT = 9420

MATOCU_LISTEN_HOST = *
MATOCU_LISTEN_PORT = 9421

CHUNKS_LOOP_TIME = 300
CHUNKS_DEL_LIMIT = 100
CHUNKS_WRITE_REP_LIMIT = 1
CHUNKS_READ_REP_LIMIT = 5

REJECT_OLD_CLIENTS = 0

# deprecated, to be removed in MooseFS 1.7
# LOCK_FILE = /srv/mfs/var/run/mfs/mfsmaster.lock

```

```

echo "192.168.3.10        mfsmaster" >> /etc/hosts

```

```

# /srv/mfs/sbin/mfsmaster start
working directory: /srv/mfs/var/mfs
lockfile created and locked
initializing mfsmaster modules ...
loading sessions ... ok
sessions file has been loaded
exports file has been loaded
loading metadata ...
create new empty filesystemmetadata file has been loaded
no charts data file - initializing empty charts
master <-> metaloggers module: listen on *:9419
master <-> chunkservers module: listen on *:9420
main master server module: listen on *:9421
mfsmaster daemon initialized properly

```

```
# /srv/mfs/sbin/mfscgiserv
starting simple cgi server (host: any , port: 9425 , rootpath: /srv/mfs/share/mfscgi)		

```

[`192.168.3.10:9425/`](http://192.168.3.10:9425/)

### 4.2. Backup server (metalogger) installation

```
groupadd mfs
useradd -g mfs mfs
cd /usr/local/src
wget http://pro.hit.gemius.pl/hitredir/id=nXCV9nrckU2Et.zoR5kxdXZJLQq1fqbG4AIiq5K95Gz.07/url=moosefs.org/tl_files/mfscode/mfs-1.6.19.tar.gz
tar zxvf mfs-1.6.19.tar.gz
cd mfs-1.6.19
./configure --prefix=/srv/mfs \
--with-default-user=mfs \
--with-default-group=mfs \
--disable-mfschunkserver \
--disable-mfsmount

make
make install

cd /srv/mfs/etc/
cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
vim mfsmetalogger.cfg

```

```
WORKING_USER = mfs
WORKING_GROUP = mfs
SYSLOG_IDENT = mfsmetalogger
LOCK_MEMORY = 0
NICE_LEVEL = -19

DATA_PATH = /srv/mfs/var/mfs

BACK_LOGS = 50
META_DOWNLOAD_FREQ = 24

MASTER_RECONNECTION_DELAY = 5

MASTER_HOST = mfsmaster
MASTER_PORT = 9419

MASTER_TIMEOUT = 60

# deprecated, to be removed in MooseFS 1.7
# LOCK_FILE = /srv/mfs/var/run/mfs/mfsmetalogger.lock

```

```

echo "192.168.3.10        mfsmaster" >> /etc/hosts

```

```
# /srv/mfs/sbin/mfsmetalogger start
working directory: /srv/mfs/var/mfs
lockfile created and locked
initializing mfsmetalogger modules ...
mfsmetalogger daemon initialized properly

```

### 4.3. Chunk servers installation

```
groupadd mfs
useradd -g mfs mfs
cd /usr/local/src
wget http://pro.hit.gemius.pl/hitredir/id=nXCV9nrckU2Et.zoR5kxdXZJLQq1fqbG4AIiq5K95Gz.07/url=moosefs.org/tl_files/mfscode/mfs-1.6.19.tar.gz
tar zxvf mfs-1.6.19.tar.gz
cd mfs-1.6.19

./configure --prefix=/srv/mfs \
--with-default-user=mfs \
--with-default-group=mfs \
--disable-mfsmaster \
--disable-mfsmount

make
make install

cd /srv/mfs/etc/
cp mfschunkserver.cfg.dist mfschunkserver.cfg
cp mfshdd.cfg.dist mfshdd.cfg
vim mfschunkserver.cfg		

```

```
WORKING_USER = mfs
WORKING_GROUP = mfs
SYSLOG_IDENT = mfschunkserver
LOCK_MEMORY = 0
NICE_LEVEL = -19

DATA_PATH = /srv/mfs/var/mfs

MASTER_RECONNECTION_DELAY = 5

BIND_HOST = *
MASTER_HOST = mfsmaster
MASTER_PORT = 9420

MASTER_TIMEOUT = 60

CSSERV_LISTEN_HOST = *
CSSERV_LISTEN_PORT = 9422
CSSERV_TIMEOUT = 5

HDD_CONF_FILENAME = /srv/mfs/etc/mfshdd.cfg
HDD_TEST_FREQ = 10

# deprecated, to be removed in MooseFS 1.7
# LOCK_FILE = /srv/mfs/var/run/mfs/mfschunkserver.lock
# BACK_LOGS = 50

```

```

cat >> /srv/mfs/etc/mfshdd.cfg <<EOF
/mnt/mfschunks
EOF

chown -R mfs:mfs /mnt/mfschunks

```

```

echo "192.168.3.10        mfsmaster" >> /etc/hosts

```

```
# /srv/mfs/sbin/mfschunkserver start
working directory: /srv/mfs/var/mfs
lockfile created and locked
initializing mfschunkserver modules ...
hdd space manager: scanning folder /mnt/mfschunks/ ...
hdd space manager: scanning complete
hdd space manager: /mnt/mfschunks/: 0 chunks found
hdd space manager: scanning complete
main server module: listen on *:9422
no charts data file - initializing empty charts
mfschunkserver daemon initialized properly		

```

http://192.168.3.10:9425/mfs.cgi?sections=CS

http://192.168.3.10:9425/mfs.cgi?sections=HD

### 4.4. Users’ computers installation

```
yum install fuse-devel

cd /usr/local/src
wget http://pro.hit.gemius.pl/hitredir/id=nXCV9nrckU2Et.zoR5kxdXZJLQq1fqbG4AIiq5K95Gz.07/url=moosefs.org/tl_files/mfscode/mfs-1.6.19.tar.gz
tar zxvf mfs-1.6.19.tar.gz
cd mfs-1.6.19
./configure --prefix=/srv/mfs \
 --with-default-user=mfs \
--with-default-group=mfs \
--disable-mfsmaster \
--disable-mfschunkserver

make
make install

```

mount

```
mkdir -p /mnt/mfs
modprobe fuse
/srv/mfs/bin/mfsmount /mnt/mfs -H 192.168.3.10

```

```
# df /mnt/mfs
Filesystem           1K-blocks      Used Available Use% Mounted on
mfs#192.168.3.10:9421
                       6085120         0   6085120   0% /mnt/mfs		

```

umount

```
umount /mnt/mfs

```

### 4.5. Testing MFS

mfs client

```
[root@dev4 ~]# mkdir -p /mnt/mfs/neo
[root@dev4 ~]# touch test /mnt/mfs/
[root@dev4 ~]# touch /mnt/mfs/neo/test
[root@dev4 ~]# touch /mnt/mfs/helloworld		

```

write testing

```
# time dd if=/dev/zero of=sometestfile bs=1024 count=100000

```

mfs chunk server

```
# ls /mnt/mfschunks/
00  07  0E  15  1C  23  2A  31  38  3F  46  4D  54  5B  62  69  70  77  7E  85  8C  93  9A  A1  A8  AF  B6  BD  C4  CB  D2  D9  E0  E7  EE  F5  FC
01  08  0F  16  1D  24  2B  32  39  40  47  4E  55  5C  63  6A  71  78  7F  86  8D  94  9B  A2  A9  B0  B7  BE  C5  CC  D3  DA  E1  E8  EF  F6  FD
02  09  10  17  1E  25  2C  33  3A  41  48  4F  56  5D  64  6B  72  79  80  87  8E  95  9C  A3  AA  B1  B8  BF  C6  CD  D4  DB  E2  E9  F0  F7  FE
03  0A  11  18  1F  26  2D  34  3B  42  49  50  57  5E  65  6C  73  7A  81  88  8F  96  9D  A4  AB  B2  B9  C0  C7  CE  D5  DC  E3  EA  F1  F8  FF
04  0B  12  19  20  27  2E  35  3C  43  4A  51  58  5F  66  6D  74  7B  82  89  90  97  9E  A5  AC  B3  BA  C1  C8  CF  D6  DD  E4  EB  F2  F9
05  0C  13  1A  21  28  2F  36  3D  44  4B  52  59  60  67  6E  75  7C  83  8A  91  98  9F  A6  AD  B4  BB  C2  C9  D0  D7  DE  E5  EC  F3  FA
06  0D  14  1B  22  29  30  37  3E  45  4C  53  5A  61  68  6F  76  7D  84  8B  92  99  A0  A7  AE  B5  BC  C3  CA  D1  D8  DF  E6  ED  F4  FB		

```

## 5. LizardFS

LizardFS 是 MooseFS 的一个衍生版本 [`lizardfs.com/`](https://lizardfs.com/)

## 6. Ceph

[`ceph.com/`](http://ceph.com/)

### 6.1. Installation on Ubuntu

```
$ apt-cache search ceph
ceph - distributed storage
ceph-common - common utilities to mount and interact with a ceph filesystem
ceph-common-dbg - debugging symbols for ceph-common
ceph-dbg - debugging symbols for ceph
ceph-fs-common - common utilities to mount and interact with a ceph filesystem
ceph-fs-common-dbg - debugging symbols for ceph-fs-common
ceph-mds-dbg - debugging symbols for ceph
gceph - Graphical ceph cluster status utility
gceph-dbg - debugging symbols for gceph
libcephfs-dev - Ceph distributed file system client library (development files)
libcephfs1 - Ceph distributed file system client library
libcephfs1-dbg - debugging symbols for libcephfs1
librados-dev - RADOS distributed object store client library (development files)
librados2 - RADOS distributed object store client library
librados2-dbg - debugging symbols for librados2
librbd-dev - RADOS block device client library (development files)
librbd1 - RADOS block device client library
librbd1-dbg - debugging symbols for librbd1
ceph-mds - distributed filesystem service
ceph-resource-agents - OCF-compliant resource agents for Ceph
obsync - synchronize data between cloud object storage providers or a local directory
python-ceph - Python libraries for the Ceph distributed filesystem

$ sudo apt-get install ceph
$ sudo apt-get install ceph-mds

```

创建配置文件 /etc/ceph/ceph.conf

```

$ vim /etc/ceph/ceph.conf
[global]

	# For version 0.55 and beyond, you must explicitly enable
	# or disable authentication with "auth" entries in [global].

	auth cluster required = cephx
	auth service required = cephx
	auth client required = cephx

[osd]
	osd journal size = 1000

	#The following assumes ext4 filesystem.
	filestore xattr use omap = true

	# For Bobtail (v 0.56) and subsequent versions, you may
	# add settings for mkcephfs so that it will create and mount
	# the file system on a particular OSD for you. Remove the comment `#`
	# character for the following settings and replace the values
	# in braces with appropriate values, or leave the following settings
	# commented out to accept the default values. You must specify the
	# --mkfs option with mkcephfs in order for the deployment script to
	# utilize the following settings, and you must define the 'devs'
	# option for each osd instance; see below.

	#osd mkfs type = {fs-type}
	#osd mkfs options {fs-type} = {mkfs options}   # default for xfs is "-f"
	#osd mount options {fs-type} = {mount options} # default mount option is "rw,noatime"

	# For example, for ext4, the mount option might look like this:

	#osd mkfs options ext4 = user_xattr,rw,noatime

	# Execute $ hostname to retrieve the name of your host,
	# and replace ubuntu with the name of your host.
	# For the monitor, replace 192.168.6.2 with the IP
	# address of your host.

[mon.a]

	host = ubuntu
	mon addr = 192.168.6.2:6789

[osd.0]
	host = ubuntu

	# For Bobtail (v 0.56) and subsequent versions, you may
	# add settings for mkcephfs so that it will create and mount
	# the file system on a particular OSD for you. Remove the comment `#`
	# character for the following setting for each OSD and specify
	# a path to the device if you use mkcephfs with the --mkfs option.

	#devs = {path-to-device}

[osd.1]
	host = ubuntu
	#devs = {path-to-device}

[mds.a]
	host = ubuntu

```

创建目录

```
sudo mkdir -p /var/lib/ceph/osd/ceph-0
sudo mkdir -p /var/lib/ceph/osd/ceph-1
sudo mkdir -p /var/lib/ceph/mon/ceph-a
sudo mkdir -p /var/lib/ceph/mds/ceph-a

```

创建 key 文件

```
$ cd /etc/ceph
$ sudo mkcephfs -a -c /etc/ceph/ceph.conf -k ceph.keyring

```

创建 key 文件过程如下

```
$ sudo mkcephfs -a -c /etc/ceph/ceph.conf -k ceph.keyring
temp dir is /tmp/mkcephfs.4rUAn1MJYV
preparing monmap in /tmp/mkcephfs.4rUAn1MJYV/monmap
/usr/bin/monmaptool --create --clobber --add a 192.168.6.2:6789 --print /tmp/mkcephfs.4rUAn1MJYV/monmap
/usr/bin/monmaptool: monmap file /tmp/mkcephfs.4rUAn1MJYV/monmap
/usr/bin/monmaptool: generated fsid a5afe011-bfde-4784-8d3d-e488418897d6
epoch 0
fsid a5afe011-bfde-4784-8d3d-e488418897d6
last_changed 2013-04-10 18:05:46.409761
created 2013-04-10 18:05:46.409761
0: 192.168.6.2:6789/0 mon.a
/usr/bin/monmaptool: writing epoch 0 to /tmp/mkcephfs.4rUAn1MJYV/monmap (1 monitors)
=== osd.0 ===
2013-04-10 18:05:46.899898 7f8b26ec8780 -1 filestore(/var/lib/ceph/osd/ceph-0) limited size xattrs -- filestore_xattr_use_omap enabled
2013-04-10 18:05:47.303918 7f8b26ec8780 -1 filestore(/var/lib/ceph/osd/ceph-0) could not find 23c2fcde/osd_superblock/0//-1 in index: (2) No such file or directory
2013-04-10 18:05:47.658550 7f8b26ec8780 -1 created object store /var/lib/ceph/osd/ceph-0 journal /var/lib/ceph/osd/ceph-0/journal for osd.0 fsid a5afe011-bfde-4784-8d3d-e488418897d6
2013-04-10 18:05:47.659360 7f8b26ec8780 -1 auth: error reading file: /var/lib/ceph/osd/ceph-0/keyring: can't open /var/lib/ceph/osd/ceph-0/keyring: (2) No such file or directory
2013-04-10 18:05:47.659489 7f8b26ec8780 -1 created new key in keyring /var/lib/ceph/osd/ceph-0/keyring
=== osd.1 ===
2013-04-10 18:05:48.039253 7f27289be780 -1 filestore(/var/lib/ceph/osd/ceph-1) limited size xattrs -- filestore_xattr_use_omap enabled
2013-04-10 18:05:48.338222 7f27289be780 -1 filestore(/var/lib/ceph/osd/ceph-1) could not find 23c2fcde/osd_superblock/0//-1 in index: (2) No such file or directory
2013-04-10 18:05:48.734861 7f27289be780 -1 created object store /var/lib/ceph/osd/ceph-1 journal /var/lib/ceph/osd/ceph-1/journal for osd.1 fsid a5afe011-bfde-4784-8d3d-e488418897d6
2013-04-10 18:05:48.734992 7f27289be780 -1 auth: error reading file: /var/lib/ceph/osd/ceph-1/keyring: can't open /var/lib/ceph/osd/ceph-1/keyring: (2) No such file or directory
2013-04-10 18:05:48.735294 7f27289be780 -1 created new key in keyring /var/lib/ceph/osd/ceph-1/keyring
=== mds.a ===
creating private key for mds.a keyring /var/lib/ceph/mds/ceph-a/keyring
creating /var/lib/ceph/mds/ceph-a/keyring
Building generic osdmap from /tmp/mkcephfs.4rUAn1MJYV/conf
/usr/bin/osdmaptool: osdmap file '/tmp/mkcephfs.4rUAn1MJYV/osdmap'
/usr/bin/osdmaptool: writing epoch 1 to /tmp/mkcephfs.4rUAn1MJYV/osdmap
Generating admin key at /tmp/mkcephfs.4rUAn1MJYV/keyring.admin
creating /tmp/mkcephfs.4rUAn1MJYV/keyring.admin
Building initial monitor keyring
added entity mds.a auth auth(auid = 18446744073709551615 key=AQB8OWVR0JMKMhAAZNnl4D2JkWIppS7gkdYkhw== with 0 caps)
added entity osd.0 auth auth(auid = 18446744073709551615 key=AQB7OWVRIFdNJxAAHjgfc+J1uVTMj4uVLtTSaQ== with 0 caps)
added entity osd.1 auth auth(auid = 18446744073709551615 key=AQB8OWVROCLPKxAAJ/Jim86K7Ip1PGnCw3Fb/g== with 0 caps)
=== mon.a ===
/usr/bin/ceph-mon: created monfs at /var/lib/ceph/mon/ceph-a for mon.a
placing client.admin keyring in ceph.keyring

$ ls
ceph.conf  ceph.keyring

```

启动 ceph

```
$ sudo service ceph -a start
$ sudo ceph health

```

启动过程如下

```
$ sudo service ceph -a start
=== mon.a ===
Starting Ceph mon.a on ubuntu...
starting mon.a rank 0 at 192.168.6.2:6789/0 mon_data /var/lib/ceph/mon/ceph-a fsid a5afe011-bfde-4784-8d3d-e488418897d6
=== mds.a ===
Starting Ceph mds.a on ubuntu...
starting mds.a at :/0
=== osd.0 ===
Starting Ceph osd.0 on ubuntu...
starting osd.0 at :/0 osd_data /var/lib/ceph/osd/ceph-0 /var/lib/ceph/osd/ceph-0/journal
=== osd.1 ===
Starting Ceph osd.1 on ubuntu...
starting osd.1 at :/0 osd_data /var/lib/ceph/osd/ceph-1 /var/lib/ceph/osd/ceph-1/journal

$ sudo ceph health
HEALTH_OK

```

```
$ sudo mkdir /mnt/ceph
$ sudo mount -t ceph 192.168.6.2:6789:/ /mnt/ceph

```

查看文件系统的挂在情况

```
$ df -T
Filesystem              Type     1K-blocks     Used Available Use% Mounted on
/dev/mapper/ubuntu-root ext4      49263424  8860876  37900100  19% /
udev                    devtmpfs   2014956        4   2014952   1% /dev
tmpfs                   tmpfs       809808     1612    808196   1% /run
none                    tmpfs         5120        0      5120   0% /run/lock
none                    tmpfs      2024516        0   2024516   0% /run/shm
none                    tmpfs       102400        0    102400   0% /run/user
/dev/vda1               ext2        233191    80600    140150  37% /boot
192.168.6.2:6789:/      ceph      98526208 22726656  75799552  24% /mnt/ceph

```

尝试创建一个文件

```
$ sudo touch /mnt/ceph/hello

```

### 6.2. Installation on CentOS

CentOS 6.4

#### 6.2.1. mon

```
rpm --import 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc'
rpm -Uvh http://ceph.com/rpm-bobtail/el6/x86_64/ceph-release-1-0.el6.noarch.rpm
yum install ceph

```

配置文件，可以参考/usr/share/doc/ceph/sample.ceph.conf，或者复制后修改

```

[global]

	# For version 0.55 and beyond, you must explicitly enable
	# or disable authentication with "auth" entries in [global].

	auth cluster required = cephx
	auth service required = cephx
	auth client required = cephx

[osd]
	osd journal size = 1000

	#The following assumes ext4 filesystem.
	filestore xattr use omap = true

	# For Bobtail (v 0.56) and subsequent versions, you may
	# add settings for mkcephfs so that it will create and mount
	# the file system on a particular OSD for you. Remove the comment `#`
	# character for the following settings and replace the values
	# in braces with appropriate values, or leave the following settings
	# commented out to accept the default values. You must specify the
	# --mkfs option with mkcephfs in order for the deployment script to
	# utilize the following settings, and you must define the 'devs'
	# option for each osd instance; see below.

	#osd mkfs type = {fs-type}
	#osd mkfs options {fs-type} = {mkfs options}   # default for xfs is "-f"
	#osd mount options {fs-type} = {mount options} # default mount option is "rw,noatime"

	# For example, for ext4, the mount option might look like this:

	#osd mkfs options ext4 = user_xattr,rw,noatime

	# Execute $ hostname to retrieve the name of your host,
	# and replace {hostname} with the name of your host.
	# For the monitor, replace {ip-address} with the IP
	# address of your host.

[mon.a]

	host = {hostname}
	mon addr = {ip-address}:6789

[osd.0]
	host = {hostname}

	# For Bobtail (v 0.56) and subsequent versions, you may
	# add settings for mkcephfs so that it will create and mount
	# the file system on a particular OSD for you. Remove the comment `#`
	# character for the following setting for each OSD and specify
	# a path to the device if you use mkcephfs with the --mkfs option.

	#devs = {path-to-device}

[osd.1]
	host = {hostname}
	#devs = {path-to-device}

[mds.a]
	host = {hostname}

```

```
# mkcephfs -a -c /etc/ceph/ceph.conf -k ceph.keyring

```

#### 6.2.2. mds

```
rpm --import 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc'
rpm -Uvh http://ceph.com/rpm-bobtail/el6/x86_64/ceph-release-1-0.el6.noarch.rpm
yum install ceph

```

#### 6.2.3. osd

```
rpm --import 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc'
rpm -Uvh http://ceph.com/rpm-bobtail/el6/x86_64/ceph-release-1-0.el6.noarch.rpm
yum install ceph

```

#### 6.2.4. client

```
rpm --import 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc'
rpm -Uvh http://ceph.com/rpm-bobtail/el6/x86_64/ceph-release-1-0.el6.noarch.rpm
yum install ceph-fuse

```

从服务器复制 ceph.keyring 到客户端

```
scp -a root@ceph-server:/etc/ceph/ceph.keyring /etc/ceph/

```

```
mkdir /mnt/cephfs/
ceph-fuse -m 192.168.6.2:6789 /mnt/cephfs/

```

```
mount -t ceph 192.168.6.2:6789:/ /mnt/cephfs

```

#### 6.2.5. RADOS Gateway

```
yum install ceph-radosgw

```

### 6.3. Block Devices

## 7. GlusterFS

http://www.gluster.org/

```
$ apt-cache search glusterfs
glusterfs-client - clustered file-system (client package)
glusterfs-dbg - GlusterFS debugging symbols
glusterfs-examples - example files for the glusterfs server and client
glusterfs-server - clustered file-system (server package)
libglusterfs-dev - GlusterFS development libraries and headers (development files)
libglusterfs0 - GlusterFS libraries and translator modules

```

### 7.1. glusterfs-server

```
$ sudo apt-get install glusterfs-server
$ sudo cp /etc/glusterfs/glusterfsd.vol /etc/glusterfs/glusterfsd.vol.orig

```

```
$ cat /etc/glusterfs/glusterfsd.vol
### file: server-volume.vol.sample

#####################################
###  GlusterFS Server Volume File  ##
#####################################

#### CONFIG FILE RULES:
### "#" is comment character.
### - Config file is case sensitive
### - Options within a volume block can be in any order.
### - Spaces or tabs are used as delimitter within a line.
### - Multiple values to options will be : delimitted.
### - Each option should end within a line.
### - Missing or commented fields will assume default values.
### - Blank/commented lines are allowed.
### - Sub-volumes should already be defined above before referring.

### Export volume "brick" with the contents of "/home/export" directory.
volume brick
  type storage/posix                   # POSIX FS translator
  option directory /home/export        # Export this directory
end-volume

### Add network serving capability to above brick.
volume server
  type protocol/server
  option transport-type tcp
# option transport-type unix
# option transport-type ib-sdp
# option transport.socket.bind-address 192.168.1.10     # Default is to listen on all interfaces
# option transport.socket.listen-port 6996              # Default is 6996

# option transport-type ib-verbs
# option transport.ib-verbs.bind-address 192.168.1.10     # Default is to listen on all interfaces
# option transport.ib-verbs.listen-port 6996              # Default is 6996
# option transport.ib-verbs.work-request-send-size  131072
# option transport.ib-verbs.work-request-send-count 64
# option transport.ib-verbs.work-request-recv-size  131072
# option transport.ib-verbs.work-request-recv-count 64

# option client-volume-filename /etc/glusterfs/glusterfs-client.vol
  subvolumes brick
# NOTE: Access to any volume through protocol/server is denied by
# default. You need to explicitly grant access through # "auth"
# option.
  option auth.addr.brick.allow * # Allow access to "brick" volume
end-volume

```

```
$ sudo mkdir /home/export
$ sudo /etc/init.d/glusterfs-server start
$ sudo /etc/init.d/glusterfs-server status
 * GlusterFS server is running.

```

### 7.2. glusterfs-client

```
$ sudo apt-get install glusterfs-client
$ sudo cp /etc/glusterfs/glusterfs.vol /etc/glusterfs/glusterfs.vol.orig

```

```
# cat /etc/glusterfs/glusterfs.vol
### file: client-volume.vol.sample

#####################################
###  GlusterFS Client Volume File  ##
#####################################

#### CONFIG FILE RULES:
### "#" is comment character.
### - Config file is case sensitive
### - Options within a volume block can be in any order.
### - Spaces or tabs are used as delimitter within a line.
### - Each option should end within a line.
### - Missing or commented fields will assume default values.
### - Blank/commented lines are allowed.
### - Sub-volumes should already be defined above before referring.

### Add client feature and attach to remote subvolume
volume client
  type protocol/client
  option transport-type tcp
# option transport-type unix
# option transport-type ib-sdp
  option remote-host 192.168.80.1         # IP address of the remote brick
# option transport.socket.remote-port 6996              # default server port is 6996

# option transport-type ib-verbs
# option transport.ib-verbs.remote-port 6996              # default server port is 6996
# option transport.ib-verbs.work-request-send-size  1048576
# option transport.ib-verbs.work-request-send-count 16
# option transport.ib-verbs.work-request-recv-size  1048576
# option transport.ib-verbs.work-request-recv-count 16

# option transport-timeout 30          # seconds to wait for a reply
                                       # from server for each request
  option remote-subvolume brick        # name of the remote volume
end-volume

### Add readahead feature
#volume readahead
#  type performance/read-ahead
#  option page-size 1MB     # unit in bytes
#  option page-count 2       # cache per file  = (page-count x page-size)
#  subvolumes client
#end-volume

### Add IO-Cache feature
#volume iocache
#  type performance/io-cache
#  option page-size 256KB
#  option page-count 2
#  subvolumes readahead
#end-volume

### Add writeback feature
#volume writeback
#  type performance/write-behind
#  option aggregate-size 1MB
#  option window-size 2MB
#  option flush-behind off
#  subvolumes iocache
#end-volume

```

```
mkdir /mnt/glusterfs

glusterfs -f /etc/glusterfs/glusterfs.vol /mnt/glusterfs
or
mount -t glusterfs /etc/glusterfs/glusterfs.vol /mnt/glusterfs

```

fstab

```
/etc/glusterfs/glusterfs.vol  /mnt/glusterfs  glusterfs  defaults  0  0

```

### 7.3. Testing

client

```
touch /mnt/glusterfs/test1
touch /mnt/glusterfs/test2

```

server

```
# ll /mnt/glusterfs
total 0
-rw-r--r-- 1 root root 0 Jun 16 11:57 test1
-rw-r--r-- 1 root root 0 Jun 16 11:57 test2

```

### 7.4. RAID

http://www.gluster.com/community/documentation/index.php/GlusterFS_User_Guide

http://www.gluster.com/community/documentation/index.php/Storage_Server_Installation_and_Configuration

ref:http://www.howtoforge.com/high-availability-storage-cluster-with-glusterfs-on-ubuntu-p2

#### 7.4.1. Mirror

例 72.2. Mirror

```
glusterfs-volgen --name store1 --raid 1 gluster1:/home/export gluster2:/home/export

```

#### 7.4.2. Strip

例 72.3. Strip

```
glusterfs-volgen --name store1 --raid 0 gluster1:/home/export gluster2:/home/export

```

### 7.5. Filesystem Administration

```

# /etc/init.d/glusterd start

gluster peer probe gluster1
gluster peer probe gluster2

# gluster peer status
Number of Peers: 3

Hostname: gluster1
Uuid: 195c5908-750f-4051-accc-697ab72fa3f2
State: Probe Sent to Peer (Connected)

Hostname: gluster2
Uuid: 5f9887a9-da15-443f-aab1-5d9952247507
State: Probe Sent to Peer (Connected)

# gluster peer detach gluster3
Detach successful

```

To create a new volume

```
gluster volume create test-volume gluster1:/exp3 gluster2:/exp4

```

### 7.6. CentOS 6.3

```
一，准备两台服务器
serverA(Client+Server)  202.231.13.6（内网 172.16.0.5）cpu:4 核 Intel(R) Xeon(R) CPU E31220 @ 3.10GHz 内存：4G 硬盘：500G
serverB(Server)  211.14.14.14（内网 172.16.0.3）cpu:4 核 Intel(R) Xeon(R) CPU E31220 @ 3.10GHz 内存：4G 硬盘：500G

二，安装步骤
1，yum search gluster
2，yum install glusterfs-server
3，yum install fuse fuse-libs
4，cp /etc/glusterfs/glusterfsd.vol /etc/glusterfs/glusterfsd.vol.orig
5，mkdir /www/export

三，启动
modprobe fuse
/etc/init.d/glusterd start

四，创建盘

服务器有两台,要先绑定在一起(假设使用 ServerA 做主服务器)
ServerA# gluster peer probe 172.16.0.3
创建 Volume,名为 gluster-volume
分布式：ServerA# gluster volume create gluster-volume 172.16.0.5:/www/export 172.16.0.3:/www/export
镜像式：ServerA# gluster volume create gluster-volume replica 2 172.16.0.5:/www/export 172.16.0.3:/www/export
条带式：ServerA# gluster volume create gluster-volume stripe 2 172.16.0.5:/www/export 172.16.0.3:/www/export
启动 volume
ServerA# gluster volume start gluster-volume
查看当前所有 volume 状态
ServerA# gluster volume info
若要使用 Cache,则使用
ServerA# gluster volume set gluster-volume performance.cache-size 1GB

Gluster 自动生成配置文件，在/etc/glusterd/vols/gluster-volume/文件夹中

在客户端挂载 gluster 镜像,客户端直接使用 Server 端的配置文件，不必创建自己的配置文件了
Client# modprobe fuse
Client# /etc/init/glusterd start
Client# mkdir /mnt/local-volume
Client# mount.glusterfs 172.16.0.5:/gluster-volume /mnt/local-volume
Client# umount.glusterfs /mnt/local-volume

命令扩展：
gluster volume stop gluster-volume
gluster volume delete gluster-volume

```

## 8. Lustre

## 8. Lustre

## 9. MogileFS

[`www.danga.com/mogilefs/`](http://www.danga.com/mogilefs/)

## 10. Kosmos distributed file system (KFS)

http://kosmosfs.sourceforge.net/

## 11. Hadoop - HDFS

改章节已从此处经移出到《Netkiller Linux 手札》中

## 12. BeeGFS - The Parallel Cluster File System

http://www.beegfs.com/

## 13. Coda

## 14. OpenAFS

http://www.openafs.org/

## 第 73 章 Shared Storage

### *cluster file system*

## 1. Oracle OCFS2

### 1.1. 安装

## 2. GFS2

## 3. fam & imon

## 第 74 章 Network Attached Storage(NAS 网络附加存储)

## 1. Network Storage - Openfiler

Openfiler is a powerful, intuitive browser-based network storage software distribution. Openfiler delivers file-based Network Attached Storage and block-based Storage Area Networking in a single framework.

[openfiler 的官方网站](http://www.openfiler.com/)过程 74.1. Openfiler Storage Control Center

1.  登录管理界面

    ```

    https://<ip address>:446/

    ```

    初始帐号和密码是: openfiler/password

2.  首先要修改默认密码

    Accounts->Admin Password

    ```
    Current Password:  `password`
    New Password:  `新密码`
    Confirm New Password:  `确认密码`

    ```

    Submit 提交

### 1.1. Accounts

*   用户认证

    openfiler.ldif

    ```
    dn: ou=people,dc=bg7nyt,dc=cn
    ou: people
    objectClass: organizationalUnit

    dn: ou=Idmap,dc=bg7nyt,dc=cn
    ou: Idmap
    objectClass: organizationalUnit

    ```

    添加 people 组织单元

    ```
    [chenjingfeng@backup ldap]$ ldapadd -x -D "cn=root,dc=bg7nyt,dc=cn" -W -f openfiler.ldif
    Enter LDAP Password:
    adding new entry "ou=people,dc=bg7nyt,dc=cn"

    adding new entry "ou=Idmap,dc=bg7nyt,dc=cn"

    ```

    1.  Accounts->Authentication

        Use LDAP: 打勾

        ```
        Server:  `ldap.bg7nyt.cn`
        Base DN:  `dc=bg7nyt,dc=cn`
        Root bind DN:  `cn=root,dc=bg7nyt,dc=cn`
        Root bind Password:  `你的密码`

        ```

    2.  Services->LDAP Settings

        ![](img/openfiler_services_ldap.png)

        ```
        Base DN:  `dc=bg7nyt,dc=cn`
        Root bind DN:  `cn=root,dc=bg7nyt,dc=cn`
        Root Password:  `你的密码`

        ```

    3.  Services->Enable/Disable

        ![](img/openfiler_services_en_dis.png)
    4.  Accounts->Account Administration

        1.  Group Administration

            ```
            Group Name: `nfs`

            ```

        2.  User Administration

            ```
            Username: `用户名`
            Password: `密码`
            Retype password: `确认密码`
            Primary Group: `用户组`

            ```

            查看组织单元：ou=people,dc=bg7nyt,dc=cn

            ```

            [chenjingfeng@backup ldap]$ ldapsearch -x -b 'ou=people,dc=bg7nyt,dc=cn'
            # extended LDIF
            #
            # LDAPv3
            # base <ou=people,dc=bg7nyt,dc=cn> with scope sub
            # filter: (objectclass=*)
            # requesting: ALL
            #

            # people, bg7nyt.cn
            dn: ou=people,dc=bg7nyt,dc=cn
            ou: people
            objectClass: organizationalUnit

            # neo, People, bg7nyt.cn
            dn: uid=neo,ou=People,dc=bg7nyt,dc=cn
            objectClass: inetOrgPerson
            objectClass: posixAccount
            homeDirectory: /dev/null
            loginShell: /bin/false
            cn: neo
            givenName: neo
            sn: neo
            uid: neo
            uidNumber: 500
            gidNumber: 500

            # search result
            search: 2
            result: 0 Success

            # numResponses: 3
            # numEntries: 2

            ```

### 1.2. Volumes

*   卷管理 [Volumes]

    我这里是使用 VMware 做的试验,在 VMware 中增加一些硬盘即可.

    1.  Volumes -> Physical Storage Mgmt.

        ![](img/openfiler_volumes_physical.png)

        ```
        Edit Disk Type Description Size Label type Partitions
        /dev/sda SCSI VMware, VMware Virtual S 8.00 GB msdos 3 (view)
        /dev/sdb SCSI VMware, VMware Virtual S 8.00 GB gpt 0 (view)
        /dev/sdc SCSI VMware, VMware Virtual S 8.00 GB gpt 0 (view)
        /dev/sdd SCSI VMware, VMware Virtual S 8.00 GB gpt 0 (view)
        ...

        ```

        openfiler 安装在/dev/sda,/dev/sda 硬盘空间不用太大,单独给 openfiler 使用.建议做 RAID 1(硬件 RAID 卡或服务器主版提供的 RAID)

        其它硬盘是用于存储的硬盘,如果有条件这些硬盘组也最好做成硬 RAID,没有条件我们可以在 openfiler 中做软件 RAID.

        点击"Edit Disk"列表内硬盘标签,之后可以看到"Create a partition in /dev/sdb"

        ![](img/openfiler-volumes-1.png)

        ```
        Mode: `Primary`
        Partition Type: `[Physical volume]` / [RAID array member]
        Starting cylinder: `1`
        Ending cylinder Size: `1044`
        Size: `自动产生`

        ```

        单击"Create"创建分区

        ![](img/openfiler-volumes-2.png)

        Back to the list of physical storage devices

        如果没有特别需求,不需要创建多个分区.

        ```
        Edit partitions in /dev/sdb (1044 cylinders with "gpt" label)

        Device Type Number Start cyl End cyl Blocks Size Type Delete
        /dev/sdb1 Linux Physical Volume (0x8e) 1 1 10 78831 76.98 MB Primary Delete
        /dev/sdb2 Linux Physical Volume (0x8e) 2 10 100 721920 705.00 MB Primary Delete
        /dev/sdb3 Linux Physical Volume (0x8e) 3 100 200 801792 783.00 MB Primary Delete
        /dev/sdb4 Linux Physical Volume (0x8e) 4 200 300 802816 784.00 MB Primary Delete
        /dev/sdb5 Linux Physical Volume (0x8e) 5 300 400 801792 783.00 MB Primary Delete

        ```

    2.  Volumes->Volume Group Mgmt.

        Volume Group 可以实现动态扩展空间,注意如果在使用中有一个成员盘损坏,你将无法恢复数据.

        应急使用可以,不建议长期使用.

        ![](img/openfiler-vg-0.png)

        ```
        Volume group name: `vg0`
        Select physical volumes to add: `在列表前面打勾`
        /dev/sdb1 8.00 GB
        /dev/sdc1 8.00 GB

        ```

        单击"Add volume group"创建 vg0

        ![](img/openfiler-vg-1.png)表 74.1. Volume Group Management

        | Volume Group Name | Size | Allocated | Free | Members | Add physical storage | Delete VG |
        | --- | --- | --- | --- | --- | --- | --- |
        | vg0 | 15.94 GB | 0 bytes | 15.94 GB | View member | PVs Add | PVs Delete |

        扩展 Volume Group 单击[PVs Add]按钮

        ![](img/openfiler-vg-2.png)

        分区列表前面打勾

        [Submit]提交

    3.  Volumes -> Create New Volume

        选择 VG

        ![](img/openfiler_cnv_svg.png)

        创建卷

        ![](img/openfiler_cnv_cv.png)

        ```
        Volume Name: `卷名`
        Volume Description: `描述`
        Required Space (MB): `配额`
        Filesystem type: `文件系统`

        ```

        单击[Create]按钮

        ![](img/openfiler_cnv_loev.png)

#### 1.2.1. RAID

Openfiler 提供软 RAID.

1.  Volumes -> Physical Storage Mgmt.

    ![](img/openfiler_volumes_physical.png)

    点击"Edit Disk"列表内硬盘标签,之后可以看到"Create a partition in /dev/sdb"

    ![](img/openfiler_raid_array_member.png)

    单击[Create]按钮创建 RAID 组成员

    ![](img/openfiler_volumes_physical_edit_partition.png)

    单击[Back to the list of physical storage devices]返回到 "Physical Storage Management"

2.  Volumes -> Software RAID Mgmt.

    ![](img/openfiler_volumes_raidmanage.png)

    ```
    Select RAID array type: `RAID(0,1,5,6,10)`
    Select chunk size: `这可以针对你的需求做优化`
    Select RAID devices to add: `打勾选择`

    ```

    单击[Add array]创建 RAID

    ![](img/openfiler_volumes_raidmanage1.png)

    RAID 创建完成后,就可以卷组和卷

    Volumes -> Volume Group Mgmt. -> Create New Volume

    RAID 6 采用双校验盘最少 4 块硬盘

#### 1.2.2. iSCSI

Volumes -> Create New Volume

![](img/openfiler_volumes_create_iscsi.png)

单击[Create]按钮

![](img/openfiler_volumes_loev_iscsi.png)

单击[Update]按钮

Services -> Enable/Disable -> iSCSI target 确认已经 Enable

General -> Local Networks

![](img/openfiler_general_local_networks.png)

单击[Update]按钮

Volumes -> List of Existing Volumes -> Select Volume Group

单击 iScsi 卷列表 Properties 下的 [Edit] 连接

![](img/openfiler_volumes_iscsi_access.png)

默认是:Deny, 修为 Allow

##### 1.2.2.1. Microsoft iSCSI Software Initiator

开始菜单 找到 Microsoft iSCSI Initiator 并运行

单击 Discovery 选项卡

单击 [ Add ] 按钮

![](img/ms_iscsi_add_target_portal.png)

单击 [ OK ] 按钮

![](img/ms_iscsi_discovery.png)

单击 Targets 选项卡

![](img/ms_iscsi_targets.png)

单击 [Refresh] 按钮 -> [Log On...]

![](img/ms_iscsi_logon.png)

单击 [ OK ] 按钮

完成 Initiator 设置

我的电脑 -> 单击鼠标右键 -> 管理

![](img/ms_iscsi_disk_manager.png)

初始化硬盘

![](img/ms_iscsi_disk_initial.png)

选择硬盘

![](img/ms_iscsi_disk_select.png)

初始化完成，红色图标消失后你就可以对磁盘分区，挂载卷，格式化。

使用 iSCSI 与使用本地磁盘完全一样。

Status -> iSCSI

![](img/openfiler_status_iscsi.png)

### 1.3. Quota

*   ### 注意

    有些文件系统不支持 Quota

    1.  Quota -> Guest Quota

        ![](img/openfiler_quota_sv.png)

        单击[Change]按钮

        ![](img/openfiler_quota_guest.png)

        单击[Apply]按钮

### 1.4. Shares

*   Shares

    ![](img/openfiler_share.png)

    单击列表内的连接.

    ![](img/openfiler_share_subfolder.png)

    ```
    Folder name: `输入文件夹名`

    ```

    单击 [Create Sub-folder] 按钮 创建文件夹

    ![](img/openfiler_share_make_share.png)

    ```
    Share name: `输入共享名`
    Share description: `描述`
    Override SMB share name: 

    ```

    ![](img/openfiler_share_edit.png)

    单击[Change]按钮 修改

    组的权限制

    ![](img/openfiler_share_group_access.png)

    单击[Update]按钮

    主机访问权限配置

    ![](img/openfiler_share_host.png)

    单击[Update]按钮

## 2. OpenMediaVault

[`www.openmediavault.org/`](http://www.openmediavault.org/)

## 3. FreeNAS

http://www.freenas.org/

## 第 75 章 Backup / Restore

## 1. 备份策略

### 1.1. Incremental backup

### 1.2. Differential backup

## 2. btrbk.noarch : Tool for creating snapshots and remote backups of btrfs sub-volumes

## 3. dump / restore

过程 75.1. dump 步骤

1.  确认设备

    准备用 dump 备份/boot 目录下的文件. 使用 df /boot 查看/boot 所在的设备(以下假设为/dev/hda1)

    ```
    neo@netkiller:~$ df
    Filesystem           1K-blocks      Used Available Use% Mounted on
    /dev/sda1             19710288   3054956  15654084  17% /
    none                   1016608       208   1016400   1% /dev
    none                   1023328         0   1023328   0% /dev/shm
    none                   1023328       736   1022592   1% /var/run
    none                   1023328         0   1023328   0% /var/lock
    /dev/sda6             19228276  16456940   1794588  91% /home
    /dev/sda10              569204    171728    368564  32% /boot
    /dev/sda7             48062440   3170748  42450216   7% /var
    /dev/sda8            384497840  64897804 300068616  18% /opt
    /dev/sda9              6728280    146336   6240164   3% /tmp

    ```

2.  首先确认备份需要的空间. 查看一个 0 级备份需要的字节数,使用 -S

    ```
    # dump -oS /dev/hda1

    ```

3.  备份到文件而非磁带. 确认在/var/tmp 目录是否有足够的空间,执行.

    ```
    # dump -0u –f /var/tmp/dumpfile /dev/hda1

    ```

    检查/etc/dumpdates,查看完全备份的时间戳.

过程 75.2. restore 步骤

1.  使用 restore 检查备份文件的内容

    ```
    # restore –tf /var/tmp/dumpfile

    ```

2.  我们可以使用 restore 的互动模式恢复特定文件到一个临时目录.

    ```
    # mkdir /tmp/restored; cd /tmp/restored
    # restore –if /var/tmp/dumpfile

    ```

    这时会看到一个 restore > 提示符. 键入 help 查看可用命令的列表. 使用 ls 和 cd 命令查看备份文件的列表.

    使用 add,选中/grub.menu.1st 和/grub/grub.conf 文件.列出所在目录,恢复的文件应该带有星号.

3.  展开备份文件

    键入 extract 命令恢复选中的文件.设置下个卷名为 1, 不为解压目录设置所有者模式. quit 退出 restore 模式.

    在 restore 运行的目录中应该有一个 grub 目录,包含恢复的 grub.conf 和 menu.1st 文件.

## 4. Bacula, the Open Source, Enterprise ready, Network Backup Tool for Linux, Unix, Mac and Windows.

http://www.bacula.org/

ubuntu 10.10

```
neo@backup:~$ apt-cache search bacula
bacula - network backup, recovery and verification - meta-package
bacula-client - network backup, recovery and verification - client meta-package
bacula-common - network backup, recovery and verification - common support files
bacula-common-mysql - network backup, recovery and verification - MySQL common files
bacula-common-pgsql - network backup, recovery and verification - PostgreSQL common files
bacula-common-sqlite3 - network backup, recovery and verification - SQLite v3 common files
bacula-console - network backup, recovery and verification - text console
bacula-director-common - network backup, recovery and verification - Director common files
bacula-director-mysql - network backup, recovery and verification - MySQL storage for Director
bacula-director-pgsql - network backup, recovery and verification - PostgreSQL storage for Director
bacula-director-sqlite3 - network backup, recovery and verification - SQLite 3 storage for Director
bacula-fd - network backup, recovery and verification - file daemon
bacula-sd - network backup, recovery and verification - storage daemon
bacula-sd-mysql - network backup, recovery and verification - MySQL SD tools
bacula-sd-pgsql - network backup, recovery and verification - PostgreSQL SD tools
bacula-sd-sqlite3 - network backup, recovery and verification - SQLite 3 SD tools
bacula-server - network backup, recovery and verification - server meta-package
bacula-console-qt - Bacula Administration Tool Console
bacula-director-sqlite - network backup, recovery and verification - SQLite 2 director transition
bacula-doc - Documentation for Bacula
bacula-sd-sqlite - network backup, recovery and verification - SQLite SD tools
bacula-traymonitor - network backup, recovery and verification - tray monitor

```

### 4.1. Install Backup Server

过程 75.3. 

1.  安装 bacula 服务器

    ```
    $ sudo apt-get install bacula

    ```

    启动脚本.

    ```
    neo@backup:/etc/bacula$ ls -1 /etc/init.d/bacula-*
    /etc/init.d/bacula-director
    /etc/init.d/bacula-fd
    /etc/init.d/bacula-sd

    ```

    Bacula Config files

    ```
    neo@backup:~$ ls -1 /etc/bacula/
    bacula-dir.conf
    bacula-fd.conf
    bacula-sd.conf
    bconsole.conf
    common_default_passwords
    scripts

    ```

    Checking Bacula Daemons Status

    ```
    neo@backup:~$ ps auwx | grep bacula
    bacula   25044  0.0  0.1  72624  2092 ?        Ssl  14:55   0:00 /usr/sbin/bacula-sd -c /etc/bacula/bacula-sd.conf -u bacula -g tape
    root     25659  0.0  0.0  60068  1376 ?        Ssl  14:56   0:00 /usr/sbin/bacula-fd -c /etc/bacula/bacula-fd.conf
    bacula   29551  0.0  0.1  87672  3096 ?        Ssl  15:48   0:00 /usr/sbin/bacula-dir -c /etc/bacula/bacula-dir.conf -u bacula -g bacula
    neo      30344  0.0  0.0   7748   876 pts/0    S+   15:57   0:00 grep --color=auto bacula

    ```

2.  bconsole

    ```
    neo@backup:/etc/bacula$ sudo bconsole
    Connecting to Director localhost:9101
    1000 OK: backup.example.com-dir Version: 5.0.2 (28 April 2010)
    Enter a period to cancel a command.
    *help
      Command       Description
      =======       ===========
      add           Add media to a pool
      autodisplay   Autodisplay console messages
      automount     Automount after label
      cancel        Cancel a job
      create        Create DB Pool from resource
      delete        Delete volume, pool or job
      disable       Disable a job
      enable        Enable a job
      estimate      Performs FileSet estimate, listing gives full listing
      exit          Terminate Bconsole session
      gui           Non-interactive gui mode
      help          Print help on specific command
      label         Label a tape
      list          List objects from catalog
      llist         Full or long list like list command
      messages      Display pending messages
      memory        Print current memory usage
      mount         Mount storage
      prune         Prune expired records from catalog
      purge         Purge records from catalog
      python        Python control commands
      quit          Terminate Bconsole session
      query         Query catalog
      restore       Restore files
      relabel       Relabel a tape
      release       Release storage
      reload        Reload conf file
      run           Run a job
      status        Report status
      setdebug      Sets debug level
      setip         Sets new client address -- if authorized
      show          Show resource records
      sqlquery      Use SQL to query catalog
      time          Print current time
      trace         Turn on/off trace to file
      unmount       Unmount storage
      umount        Umount - for old-time Unix guys, see unmount
      update        Update volume, pool or stats
      use           Use catalog xxx
      var           Does variable expansion
      version       Print Director version
      wait          Wait until no jobs are running

    When at a prompt, entering a period cancels the command.

    *

    ```

3.  修改配置文件，增加备份策略.

    备份配置文件，以免把文件改坏。

    ```
    root@backup:~# cd /etc/bacula/
    root@backup:/etc/bacula# mkdir original
    root@backup:/etc/bacula# cp *.conf original/
    root@backup:/etc/bacula#

    ```

    bacula-dir.conf

    ```
    root@backup:/etc/bacula# vim bacula-dir.conf
    Job {
      Name = "BackupClient2"
      Client = web-fd
      JobDefs = "DefaultJob"
    }

    ```

### 4.2. Install Backup Client

*   ```
    neo@web:~$ sudo apt-get install bacula-client

    ```

## 5. Amanda: Open Source Backup

http://www.amanda.org/

Amanda is the most popular open source backup and recovery software in the world. Amanda protects more than half a million of servers and desktops running various versions of Linux, UNIX, BSD, Mac OS-X and Microsoft Windows operating systems worldwide.

## 6. Attic - 拥有重复数据删除技术的备份软件

Attic 是一个拥有重复数据删除技术的备份软件

### 6.1. 安装 Attic

```

$ pip install attic

```

### 6.2. 快速开始

过程 75.4. Attic 快速开始

1.  初始化仓库

    ```

    $ attic init /somewhere/my-repository.attic

    ```

2.  备份目录~/src 和 ~/Documents 归档名称 Monday

    ```

    $ attic create /somwhere/my-repository.attic::Monday ~/src ~/Documents

    ```

3.  一次类推下一份叫做 Tuesday

    ```

    $ attic create --stats /somwhere/my-repository.attic::Tuesday ~/src ~/Documents

    ```

    --stats 参数将显示备份过程的状态

4.  列出库中的所有档案

    ```

    $ attic list /somewhere/my-repository.attic

    ```

5.  列出周一归档文件的内容

    ```

    $ attic list /somewhere/my-repository.attic::Monday

    ```

6.  通过手动删除周一存档恢复磁盘空间

    ```

    $ attic delete /somwhere/my-backup.attic::Monday

    ```

    Attic is quiet by default. Add the -v or --verbose option to get progress reporting during command execution.

## 7. SafeKeep

[`safekeep.sourceforge.net/index.shtml`](http://safekeep.sourceforge.net/index.shtml)

## 8. Opendedup

http://www.opendedup.org/

## 第 76 章 inotify

**$ ls -ld /proc/sys/fs/inotify/***

## 1. inotify-tools

Installation

ubuntu

```
sudo apt-get install inotify-tools

```

centos

```
yum install inotify-tools

```

inotifywait -r -m $HOME

监控登录过程

```
neo@master:~$ inotifywait -r  -m $HOME
Setting up watches.  Beware: since -r was given, this may take a while!
Watches established.
/home/neo/ OPEN .profile
/home/neo/ ACCESS .profile
/home/neo/ CLOSE_NOWRITE,CLOSE .profile
/home/neo/ OPEN .bashrc
/home/neo/ ACCESS .bashrc
/home/neo/ CLOSE_NOWRITE,CLOSE .bashrc
/home/neo/ OPEN .bash_history
/home/neo/ ACCESS .bash_history
/home/neo/ CLOSE_NOWRITE,CLOSE .bash_history
/home/neo/ OPEN .bash_history
/home/neo/ ACCESS .bash_history
/home/neo/ CLOSE_NOWRITE,CLOSE .bash_history

```

create a new file helloworld.txt

```
/home/neo/ CREATE helloworld.txt
/home/neo/ OPEN helloworld.txt
/home/neo/ MODIFY helloworld.txt
/home/neo/ CLOSE_WRITE,CLOSE helloworld.txt

```

cat a file using cat helloworld.txt

```
/home/neo/ OPEN,ISDIR
/home/neo/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/neo/ OPEN,ISDIR
/home/neo/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/neo/ OPEN helloworld.txt
/home/neo/ ACCESS helloworld.txt
/home/neo/ CLOSE_NOWRITE,CLOSE helloworld.txt

```

delete a file helloworld.txt

```
/home/neo/ OPEN,ISDIR
/home/neo/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/neo/ OPEN,ISDIR
/home/neo/ CLOSE_NOWRITE,CLOSE,ISDIR
/home/neo/ DELETE helloworld.txt

```

## 2. Incron - cron-like daemon which handles filesystem events

Incron 在指定文件系统发生某些指定变化后运行指定程序，工作类似 cron

```
# yum install -y incron
# systemctl enable incrond
# systemctl start incrond

```

相关文件

```
# rpm -ql incron-0.5.10-8.el7
/etc/incron.conf
/etc/incron.d
/usr/bin/incrontab
/usr/lib/systemd/system/incrond.service
/usr/sbin/incrond
/usr/share/doc/incron-0.5.10
/usr/share/doc/incron-0.5.10/CHANGELOG
/usr/share/doc/incron-0.5.10/COPYING
/usr/share/doc/incron-0.5.10/LICENSE-GPL
/usr/share/doc/incron-0.5.10/README
/usr/share/doc/incron-0.5.10/TODO
/usr/share/man/man1/incrontab.1.gz
/usr/share/man/man5/incron.conf.5.gz
/usr/share/man/man5/incrontab.5.gz
/usr/share/man/man8/incrond.8.gz
/var/spool/incron

```

### 2.1. incrontab - inotify cron table manipulator

incrontab 用法语 crontab 极其类似。

配置触发事件,格式如下：

```

<path> <mask> <command>

```

```
[root@localhost ~]# incrontab -e
/etc IN_MODIFY /tmp/test.sh $@/$#			

```

查看触发事件

```
# incrontab -l
/etc IN_MODIFY /tmp/test.sh $@/$#

```

测试脚本

```

# cat /tmp/test.sh
#!/bin/bash
echo $@ >> /tmp/test.log

```

现在你可以修改/etc 下面的文件，然后查看/tmp/test.log 日志的变化。

### 2.2. 使用说明

#### 2.2.1. mask 参数

```
IN_ACCESS           File was accessed (read) (*)
IN_ATTRIB           Metadata changed (permissions, timestamps, extended attributes, etc.) (*)
IN_CLOSE_WRITE      File opened for writing was closed (*)
IN_CLOSE_NOWRITE    File not opened for writing was closed (*)
IN_CREATE           File/directory created in watched directory (*)
IN_DELETE           File/directory deleted from watched directory (*)
IN_DELETE_SELF           Watched file/directory was itself deleted
IN_MODIFY           File was modified (*)
IN_MOVE_SELF        Watched file/directory was itself moved
IN_MOVED_FROM       File moved out of watched directory (*)
IN_MOVED_TO         File moved into watched directory (*)
IN_OPEN             File was opened (*)

IN_ALL_EVENTS       all of the above events
IN_MOVE             a combination of IN_MOVED_FROM and IN_MOVED_TO
IN_CLOSE            combines IN_CLOSE_WRITE and IN_CLOSE_NOWRITE.

IN_DONT_FOLLOW      Don't dereference pathname if it is a symbolic link
IN_ONESHOT          Monitor pathname for only one event
IN_ONLYDIR          Only watch pathname if it is a directory				

```

#### 2.2.2. command 参数

```

$$   dollar sign
$@   watched filesystem path (see above)
$#   event-related file name
$%   event flags (textually)
$&   event flags (numerically)				

```

## 3. inotify-tools + rsync

1.  -m 是保持一直监听

2.  -r 是递归查看目录

3.  -q 是打印出事件～

4.  -e create,move,delete,modify 监听 创建 移动 删除 写入 事件

```

inotifywait -mrq --event create,delete,modify,move --format '%w %e' /your_path | while read w e; do
    if [ "$e" = "IGNORED" ]; then
        continue
    fi
    rsync -az --delete $w username@your_ip:$w
done

```

```

#!/bin/sh
# A slightly complex but actually useful example
inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %f' \
-e close_write /home/billy | while read date time file; do
    rsync /home/billy/${file} rsync://billy@example.com/backup/${file} && \
    echo "At ${time} on ${date}, file ${file} was backed up via rsync"
done

```

```

[root@development ~]# cat inotify-rsync
#!/bin/bash
# $Id: chapter.storage.inotify.xml 334 2012-02-01 05:59:34Z netkiller $ #
# Author neo<openunix@163.com> #

# monitor path
monitor_path=cms
#inotifywait path
INOTIFYWAIT=inotifywait

# rsync image file
function images {
        local file=$1
        rsync -az --delete $file /tmp/images/$file
#       rsync ${file} ${rsync_url}/${file}
}

# rsync html file
function html {
        local file=$1
        rsync -az --delete $file /tmp/$file
}

$INOTIFYWAIT -mrq --event close_write --format '%w%f %e' $monitor_path | while read file event; do
    if [ "$event" = "CLOSE_WRITE,CLOSE" ]; then
        ext=$(echo $file | awk -F'.' '{print $2}')
        if [ $ext = 'jpg' ]; then
                images $file
        fi
        if [ $ext = 'html' ]; then
                html $file
        fi
    fi
done &

```

## 4. pyinotify

```
[root@development ~]# easy_install pyinotify
[root@development ~]# yum install gcc
[root@development ctypes-1.0.2]# python setup.py install

```