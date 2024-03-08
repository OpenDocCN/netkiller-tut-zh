# 部分 II. File System

## 第 11 章 gpart -- control utility for the disk partitioning GEOM class

显示 gpt 分区

```

root@netkiller:~ # gpart show
=>       34  312581741  ada0  GPT  (149G)
         34    8388608     1  freebsd-swap  (4.0G)
    8388642        128     2  freebsd-boot  (64k)
    8388770   94371712     3  freebsd-ufs  (45G)
  102760482  209821293        - free -  (100G)

```

建立 gpt 分区

```

root@netkiller:~ # gpart add -t freebsd-ufs ada0
ada0p4 added
root@netkiller:~ # gpart show
=>       34  312581741  ada0  GPT  (149G)
         34    8388608     1  freebsd-swap  (4.0G)
    8388642        128     2  freebsd-boot  (64k)
    8388770   94371712     3  freebsd-ufs  (45G)
  102760482  209821293     4  freebsd-ufs  (100G)

# newfs /dev/ada0p4 
/dev/ada0p4: 102451.8MB (209821288 sectors) block size 32768, fragment size 4096
	using 164 cylinder groups of 626.09MB, 20035 blks, 80256 inodes.
super-block backups (for fsck_ffs -b #) at:
 192, 1282432, 2564672, 3846912, 5129152, 6411392, 7693632, 8975872, 10258112,
 11540352, 12822592, 14104832, 15387072, 16669312, 17951552, 19233792,
 20516032, 21798272, 23080512, 24362752, 25644992, 26927232, 28209472,
 29491712, 30773952, 32056192, 33338432, 34620672, 35902912, 37185152,
 38467392, 39749632, 41031872, 42314112, 43596352, 44878592, 46160832,
 47443072, 48725312, 50007552, 51289792, 52572032, 53854272, 55136512,
 56418752, 57700992, 58983232, 60265472, 61547712, 62829952, 64112192,
 65394432, 66676672, 67958912, 69241152, 70523392, 71805632, 73087872,
 74370112, 75652352, 76934592, 78216832, 79499072, 80781312, 82063552,
 83345792, 84628032, 85910272, 87192512, 88474752, 89756992, 91039232,
 92321472, 93603712, 94885952, 96168192, 97450432, 98732672, 100014912,
 101297152, 102579392, 103861632, 105143872, 106426112, 107708352, 108990592,
 110272832, 111555072, 112837312, 114119552, 115401792, 116684032, 117966272,
 119248512, 120530752, 121812992, 123095232, 124377472, 125659712, 126941952,
 128224192, 129506432, 130788672, 132070912, 133353152, 134635392, 135917632,
 137199872, 138482112, 139764352, 141046592, 142328832, 143611072, 144893312,
 146175552, 147457792, 148740032, 150022272, 151304512, 152586752, 153868992,
 155151232, 156433472, 157715712, 158997952, 160280192, 161562432, 162844672,
 164126912, 165409152, 166691392, 167973632, 169255872, 170538112, 171820352,
 173102592, 174384832, 175667072, 176949312, 178231552, 179513792, 180796032,
 182078272, 183360512, 184642752, 185924992, 187207232, 188489472, 189771712,
 191053952, 192336192, 193618432, 194900672, 196182912, 197465152, 198747392,
 200029632, 201311872, 202594112, 203876352, 205158592, 206440832, 207723072,
 209005312

```

## 第 12 章 Windows 文件系统

## msdos/fat16 文件系统

### 分区格式化

进入 sysinstall 给硬盘分区

```
sysinstall - confgure - fdisk

```

格式化分区

```
newfs_msdos /dev/da0s1

```

### 挂载 msdoc 分区

```
mount -t msdos /dev/da0s1 /mnt

```

```
mount_msdosfs /dev/da0s1 /mnt

```

## 第 13 章 ZFS

## 初始化

```
echo "zfs_enable=YES" >> /etc/rc.conf
echo 'daily_status_zfs_enable="YES"' >> /etc/periodic.conf

freebsd# vi /etc/rc.conf
zfs_enable="YES"

freebsd# /etc/rc.d/zfs faststart

```

另一种临时启动方法

```
# /etc/rc.d/zfs onestart

```

## Creating a Basic Filesystem

```
# zpool create tank c0t0d0
# zfs create tank/fs

# mkfile 100m /tank/fs/foo
# df -h /tank/fs
Filesystem             size   used  avail capacity  Mounted on
tank/fs                 80G   100M    80G     1%    /tank/fs

```

```
# zpool create tank c0t0d0 c1t0d0

```

## Creating a Storage Pool

### Mirrored Pool

**# zpool create tank mirror c0t0d0 c0t0d1**

```
freebsd# zpool create tank mirror ad1 ad3
freebsd# zpool status tank
  pool: tank
 state: ONLINE
 scrub: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        tank        ONLINE       0     0     0
          mirror    ONLINE       0     0     0
            ad1     ONLINE       0     0     0
            ad3     ONLINE       0     0     0

errors: No known data errors

```

### RAID-Z Pool

```
freebsd# zpool create zfs raidz ad1 ad3
freebsd# zpool status zfs
  pool: zfs
 state: ONLINE
 scrub: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        zfs         ONLINE       0     0     0
          raidz1    ONLINE       0     0     0
            ad1     ONLINE       0     0     0
            ad3     ONLINE       0     0     0

errors: No known data errors

```

### Querying Pool Status

You can see that your pool was successfully created by using the zpool list command:

```
freebsd# zpool list
NAME   SIZE   USED  AVAIL    CAP  HEALTH  ALTROOT
zfs   3.97G   234K  3.97G     0%  ONLINE  -

```

### Destroying Pools

Pools are destroyed by using the zpool destroy command. This command destroys the pool even if it contains mounted datasets.

```
# zpool destroy tank

```

Destroying a Pool With Faulted Devices

```
# zpool destroy tank
cannot destroy 'tank': pool is faulted
use '-f' to force destruction anyway
# zpool destroy -f tank

```

## Creating a Filesystem Hierarchy

### Creating a Filesystem

```
freebsd# zfs create zfs/www
freebsd# mount
/dev/ad0s1a on / (ufs, local)
devfs on /dev (devfs, local, multilabel)
/dev/ad0s1e on /tmp (ufs, local, soft-updates)
/dev/ad0s1f on /usr (ufs, local, soft-updates)
/dev/ad0s1d on /var (ufs, local, soft-updates)
zfs on /zfs (zfs, local)
zfs/www on /zfs/www (zfs, local)

```

```
freebsd# zfs set compression=gzip zfs/www

```

### Setting Quotas

we want to give bonwick a quota of 10 Gbytes

```
# zfs set quota=10G tank/home/bonwick

```

### Setting Reservations

```
# zfs set reservation=5G tank/home/moore
# zfs get reservation tank/home/moore
NAME             PROPERTY      VALUE                      SOURCE
tank/home/moore  reservation   5.00G                      local

```

### Querying Filesystem Information

```
freebsd# zfs list
NAME       USED  AVAIL  REFER  MOUNTPOINT
tank      97.5K  1.95G    18K  /tank
tank/neo    18K  1.95G    18K  /tank/neo

```

### Renaming a Filesystem

```
# zfs rename tank/home/maybee tank/ws/maybee

```

### Destroying a Filesystem

```
# zfs destroy tank/home/tabriz

```

```
# zfs destroy tank/home/ahrens
cannot unmount 'tank/home/ahrens': Device busy

# zfs destroy -f tank/home/ahrens

```

```
# zfs destroy -r tank/home/schrock
cannot destroy 'tank/home/schrock': filesystem has dependant clones
use '-R' to destroy the following datasets:
tank/clones/schrock-clone

# zfs destroy -R tank/home/schrock

```

## zfs mount/umount

Legacy mount points must be managed through legacy tools. An attempt to use ZFS tools result in an error.

```
# zfs mount pool/home/billm
cannot mount 'pool/home/billm': legacy mountpoint
use mount(1M) to mount this filesystem
# mount -F zfs tank/home/billm

```

### Temporary Mount Properties

```
# zfs mount -o ro tank/home/perrin
# zfs mount -o remount,noatime tank/home/perrin
# zfs get atime tank/home/perrin
NAME             PROPERTY      VALUE                      SOURCE
tank/home/perrin atime         off                        temporary

```

### Mounting File Systems

mount

```
freebsd# zfs mount zfs/www
freebsd# mount
/dev/ad0s1a on / (ufs, local)
devfs on /dev (devfs, local, multilabel)
/dev/ad0s1e on /tmp (ufs, local, soft-updates)
/dev/ad0s1f on /usr (ufs, local, soft-updates)
/dev/ad0s1d on /var (ufs, local, soft-updates)
zfs on /zfs (zfs, local)
zfs/www on /zfs/www (zfs, local)

```

The -a option can be used to mount all ZFS managed filesystems. Legacy managed filesystems are not mounted.

```
# zfs mount -a

```

### Unmounting File Systems

umount

```
freebsd# zfs umount /zfs/www
freebsd# mount
/dev/ad0s1a on / (ufs, local)
devfs on /dev (devfs, local, multilabel)
/dev/ad0s1e on /tmp (ufs, local, soft-updates)
/dev/ad0s1f on /usr (ufs, local, soft-updates)
/dev/ad0s1d on /var (ufs, local, soft-updates)
zfs on /zfs (zfs, local)

```

### Legacy Mount Points

```
freebsd# zfs set mountpoint=/tank  tank
freebsd# zfs mount -a
freebsd# mount
/dev/ad0s1a on / (ufs, local)
devfs on /dev (devfs, local, multilabel)
/dev/ad0s1e on /tmp (ufs, local, soft-updates)
/dev/ad0s1f on /usr (ufs, local, soft-updates)
/dev/ad0s1d on /var (ufs, local, soft-updates)
tank on /tank (zfs, local)

```

## Sharing ZFS File Systems

### Controlling Share Semantics

```
freebsd# zfs set sharenfs=on zfs/www

```

### Unsharing Filesystems

```
freebsd# zfs unshare zfs/www

```

This command unshares the zpool filesystem. To unshare all ZFS filesystems on the system, run:

```
freebsd# zfs unshare -a

```

### Sharing Filesystems

```
freebsd# zfs share zfs/www

```

You can also share all ZFS filesystems on the system:

```
# zfs share -a

```

## Device Management

### Adding Devices to a Pool

```
# zpool add scoop mirror c0t1d0 c1t1d0

```

### Onlining and Offlining Devices

#### Taking a Device Offline

```
# zpool offline tank c0t0d0
bringing device 'c0t0d0' offline

```

#### Bringing a Device Online

```
# zpool online tank c0t0d0
bringing device 'c0t0d0' online

```

### Replacing Devices

You can replace a device in a storage pool by using the zpool replace command.

```
# zpool replace tank c0t0d0 c0t0d1

```

In the above example, the previous device, c0t0d0, is replaced by c0t0d1.

## I/O Statistics

```
freebsd# zpool iostat
               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank        73.5K  1.98G      0      2    119  2.10K

```

```
freebsd# zpool iostat
               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank        73.5K  1.98G      0      2    119  2.10K
freebsd# zpool iostat tank 2
               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank        73.5K  1.98G      0      1     84  1.48K
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0
tank        73.5K  1.98G      0      0      0      0

```

### Virtual Device Statistics

```
freebsd# zpool iostat -v
               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank        73.5K  1.98G      0      0     45    824
  mirror    73.5K  1.98G      0      0     45    824
    ad1         -      -      0      0  1.12K  9.04K
    ad3         -      -      0      0    778  9.04K
----------  -----  -----  -----  -----  -----  -----

```

```
freebsd# zpool iostat -v tank 5
               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank         124K  1.98G      0      0     12    367
  mirror     124K  1.98G      0      0     12    367
    ad1         -      -      0      0    297    773
    ad3         -      -      0      0    270    773
----------  -----  -----  -----  -----  -----  -----

               capacity     operations    bandwidth
pool         used  avail   read  write   read  write
----------  -----  -----  -----  -----  -----  -----
tank         124K  1.98G      0      0      0      0
  mirror     124K  1.98G      0      0      0      0
    ad1         -      -      0      0      0      0
    ad3         -      -      0      0      0      0
----------  -----  -----  -----  -----  -----  -----

```

## Health Status

### Basic Health Status

```
freebsd# zpool status -x
all pools are healthy
freebsd#

```

### Detailed Health Status

```
freebsd# zpool status -x
all pools are healthy
freebsd# zpool status -v tank
  pool: tank
 state: ONLINE
 scrub: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        tank        ONLINE       0     0     0
          mirror    ONLINE       0     0     0
            ad1     ONLINE       0     0     0
            ad3     ONLINE       0     0     0

errors: No known data errors

```

## Storage Pool Migration

### Exporting a Pool

```
# zpool export tank

```

```
# zpool export tank
cannot unmount '/export/home/eschrock': Device busy
# zpool export -f tank

```

### Importing Pools

```
# zpool import tank

```

## Querying Properties

```
freebsd# zfs get all tank
NAME  PROPERTY              VALUE                  SOURCE
tank  type                  filesystem             -
tank  creation              Sat Jun 19 17:49 2010  -
tank  used                  98.5K                  -
tank  available             1.95G                  -
tank  referenced            19K                    -
tank  compressratio         1.00x                  -
tank  mounted               yes                    -
tank  quota                 none                   default
tank  reservation           none                   default
tank  recordsize            128K                   default
tank  mountpoint            /tank                  default
tank  sharenfs              off                    default
tank  checksum              on                     default
tank  compression           off                    default
tank  atime                 on                     default
tank  devices               on                     default
tank  exec                  on                     default
tank  setuid                on                     default
tank  readonly              off                    default
tank  jailed                off                    default
tank  snapdir               hidden                 default
tank  aclmode               groupmask              default
tank  aclinherit            restricted             default
tank  canmount              on                     default
tank  shareiscsi            off                    default
tank  xattr                 off                    temporary
tank  copies                1                      default
tank  version               3                      -
tank  utf8only              off                    -
tank  normalization         none                   -
tank  casesensitivity       sensitive              -
tank  vscan                 off                    default
tank  nbmand                off                    default
tank  sharesmb              off                    default
tank  refquota              none                   default
tank  refreservation        none                   default
tank  primarycache          all                    default
tank  secondarycache        all                    default
tank  usedbysnapshots       0                      -
tank  usedbydataset         19K                    -
tank  usedbychildren        79.5K                  -
tank  usedbyrefreservation  0                      -

```

## Backing Up and Restoring ZFS Data

### Backing Up a ZFS Snapshot

```

# zfs backup tank/dana@111505 > /dev/rmt/0

# zfs backup pool/fs@snap | gzip > backupfile.gz

```

### Restoring a ZFS Snapshot

```

# zfs backup tank/gozer@111105 > /dev/rmt/0
.
.
.
# zfs restore tank/gozer2@today < /dev/rmt/0
# zfs rename tank/gozer tank/gozer.old
# zfs rename tank/gozer2 tank/gozer

```

```

# zfs rollback tank/dana@111505
cannot rollback to 'tank/dana@111505': more recent snapshots exist
use '-r' to force deletion of the following snapshots:
tank/dana@now
# zfs rollback -r tank/dana@111505
# zfs restore tank/dana < /dev/rmt/0

```

### Remote Replication of a ZFS File System

```
# zfs backup tank/neo@today | ssh newsys zfs restore sandbox/restfs@today

```

## ZFS Snapshots and Clones

### ZFS Snapshots

filesystem@snapname, volume@snapname

#### Creating ZFS Snapshots

The following example creates a snapshot of tank/neo that is named friday.

```
freebsd# zfs snapshot tank/neo@friday

```

#### Destroying ZFS Snapshots

Snapshots are destroyed by using the zfs destroy command.

```
# zfs destroy tank/home/ahrens@friday

```

#### Renaming ZFS Snapshots

```
# zfs rename tank/home/cindys@111205 pool/home/cindys@today

```

#### Displaying and Accessing ZFS Snapshots

```
freebsd# zfs list -t snapshot
NAME              USED  AVAIL  REFER  MOUNTPOINT
tank/neo@friday      0      -    18K  -

```

#### Rolling Back to a Snapshot

```
# zfs rollback pool/home/ahrens@tuesday
cannot rollback to 'pool/home/ahrens@tuesday': more recent snapshots exist
use '-r' to force deletion of the following snapshots:
pool/home/ahrens@wednesday
pool/home/ahrens@thursday
# zfs rollback -r pool/home/ahrens@tuesday

```

### ZFS Clones

#### Creating a Clone

```
# zfs clone pool/ws/gate@yesterday pool/home/ahrens/bug123

# zfs snapshot projects/newproject@today
# zfs clone projects/newproject@today projects/teamA/tempuser
# zfs set sharenfs=on projects/teamA/tempuser
# zfs set quota=5G projects/teamA/tempuser

```

#### Destroying a Clone

ZFS clones are destroyed with the zfs destroy command.

```
# zfs destroy pool/home/ahrens/bug123

```

Clones must be destroyed before the parent snapshot can be destroyed.

## Emulated Volumes

```
# zfs create -V 5gb tank/vol

```