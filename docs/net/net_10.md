# 部分 VI. F5 BIG-IP 3900

## 第 30 章 Linux

## default password

ssh: root default

web: admin admin

## uname

```
[root@F5:Active] config # uname -a
Linux F5.3900.Test 2.6.18-164.11.1.el5.1.0.f5app #1 SMP Thu Apr 8 19:23:04 PDT 2010 x86_64 x86_64 x86_64 GNU/Linux

[root@test:Active] config # uname -a
Linux test.f5.com 2.6.18-164.11.1.el5.1.0.f5app #1 SMP Fri Apr 29 14:30:15 PDT 2011 x86_64 x86_64 x86_64 GNU/Linux

```

### /etc/issue

```
[root@F5:Active] config # cat /etc/issue
BIG-IP 10.2.0 Build 1707.0
Kernel \r on an \m

[root@test:Active] config # cat /etc/issue
BIG-IP 10.2.2 Build 852.0
Kernel \r on an \m

```

## cpu

```
[root@F5:Active] config # cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 15
model name      : Intel(R) Xeon(R) CPU           X3220  @ 2.40GHz
stepping        : 11
cpu MHz         : 2400.134
cache size      : 4096 KB
physical id     : 0
siblings        : 4
core id         : 0
cpu cores       : 4
apicid          : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 10
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm syscall nx lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
bogomips        : 4800.26
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 15
model name      : Intel(R) Xeon(R) CPU           X3220  @ 2.40GHz
stepping        : 11
cpu MHz         : 2400.134
cache size      : 4096 KB
physical id     : 0
siblings        : 4
core id         : 1
cpu cores       : 4
apicid          : 1
fpu             : yes
fpu_exception   : yes
cpuid level     : 10
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm syscall nx lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
bogomips        : 4799.45
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:

processor       : 2
vendor_id       : GenuineIntel
cpu family      : 6
model           : 15
model name      : Intel(R) Xeon(R) CPU           X3220  @ 2.40GHz
stepping        : 11
cpu MHz         : 2400.134
cache size      : 4096 KB
physical id     : 0
siblings        : 4
core id         : 2
cpu cores       : 4
apicid          : 2
fpu             : yes
fpu_exception   : yes
cpuid level     : 10
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm syscall nx lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
bogomips        : 4799.54
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:

processor       : 3
vendor_id       : GenuineIntel
cpu family      : 6
model           : 15
model name      : Intel(R) Xeon(R) CPU           X3220  @ 2.40GHz
stepping        : 11
cpu MHz         : 2400.134
cache size      : 4096 KB
physical id     : 0
siblings        : 4
core id         : 3
cpu cores       : 4
apicid          : 3
fpu             : yes
fpu_exception   : yes
cpuid level     : 10
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm syscall nx lm constant_tsc pni monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr lahf_lm
bogomips        : 4799.47
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:

```

## memory

```
[root@F5:Active] config # free -m
             total       used       free     shared    buffers     cached
Mem:          7995       7661        333          0         42        299
-/+ buffers/cache:       7318        676
Swap:         1023          0       1023

```

```
[root@F5:Active] config # cat /proc/meminfo
MemTotal:      8187068 kB
MemFree:        341868 kB
Buffers:         43956 kB
Cached:         307236 kB
SwapCached:          0 kB
Active:         482300 kB
Inactive:       205392 kB
HighTotal:           0 kB
HighFree:            0 kB
LowTotal:      8187068 kB
LowFree:        341868 kB
SwapTotal:     1048312 kB
SwapFree:      1048312 kB
Dirty:             208 kB
Writeback:           0 kB
AnonPages:      350304 kB
Mapped:         108304 kB
Slab:            45012 kB
PageTables:       4420 kB
NFS_Unstable:        0 kB
Bounce:              0 kB
CommitLimit:   1603924 kB
Committed_AS:   729416 kB
VmallocTotal: 34359738367 kB
VmallocUsed:      5544 kB
VmallocChunk: 34359732627 kB
HugePages_Total:  3455
HugePages_Free:     14
HugePages_Rsvd:      0
Hugepagesize:     2048 kB

```

## disk

```
[root@F5:Active] config # fdisk -l

Disk /dev/hda: 8455 MB, 8455200768 bytes
16 heads, 63 sectors/track, 16383 cylinders
Units = cylinders of 1008 * 512 = 516096 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/hda1   *           1         219      110375+   b  W95 FAT32
/dev/hda2             220        2299     1048320   82  Linux swap / Solaris
/dev/hda3            2300       16382     7097832   8e  Linux LVM

Disk /dev/sda: 300.0 GB, 300069052416 bytes
255 heads, 63 sectors/track, 36481 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1               1          57      457852    b  W95 FAT32
/dev/sda2              58         187     1044225   82  Linux swap / Solaris
/dev/sda3             188       36480   291523522+  8e  Linux LVM

```

```
[root@F5:Active] config # df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/mapper/vg--db--sda-set.1.root
                        253871    129260    111504  54% /
/dev/mapper/vg--db--sda-set.1._config
                        507748     20668    460866   5% /config
/dev/mapper/vg--db--sda-set.1._usr
                       1378784    984928    323816  76% /usr
/dev/mapper/vg--db--sda-set.1._var
                       3096336    168280   2770772   6% /var
/dev/mapper/vg--db--sda-dat.share.1
                      30963708    944348  28446496   4% /shared
/dev/mapper/vg--db--sda-dat.log.1
                       7224824    133564   6724260   2% /var/log
none                   4093532       408   4093124   1% /dev/shm
none                   4093532     13240   4080292   1% /var/tmstat
none                   4093532      1088   4092444   1% /var/run
prompt                    4096        12      4084   1% /var/prompt

```

## process

```
top - 15:42:57 up  1:35,  1 user,  load average: 0.41, 0.37, 0.42
Tasks: 179 total,   1 running, 178 sleeping,   0 stopped,   0 zombie
Cpu0  :  7.0%us,  3.0%sy,  0.1%ni, 89.7%id,  0.2%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu1  :  6.6%us,  2.7%sy,  0.2%ni, 90.5%id,  0.1%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu2  :  6.6%us,  2.7%sy,  0.2%ni, 90.2%id,  0.3%wa,  0.1%hi,  0.0%si,  0.0%st
Cpu3  :  6.6%us,  2.7%sy,  0.1%ni, 90.6%id,  0.1%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   8187068k total,  7845792k used,   341276k free,    43832k buffers
Swap:  1048312k total,        0k used,  1048312k free,   307448k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
 4904 root      RT   0 1750m  26m  21m S  7.3  0.3   7:50.43 tmm
 4905 root      RT   0 1750m  26m  21m S  7.3  0.3   7:52.84 tmm
 4906 root      RT   0 1750m  26m  21m S  7.3  0.3   7:51.41 tmm
 4903 root      RT   0 1750m  26m  21m S  5.5  0.3   8:23.62 tmm
 3634 root      15   0 17696 9.9m 2348 S  1.8  0.1   0:43.75 statsd

```

```

[root@F5:Active] ~ # cd /config/
[root@F5:Active] config # ps ax
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:00 init [3]
    2 ?        S<     0:00 [migration/0]
    3 ?        SN     0:00 [ksoftirqd/0]
    4 ?        S<     0:00 [watchdog/0]
    5 ?        S<     0:00 [migration/1]
    6 ?        SN     0:00 [ksoftirqd/1]
    7 ?        S<     0:00 [watchdog/1]
    8 ?        S<     0:00 [migration/2]
    9 ?        SN     0:00 [ksoftirqd/2]
   10 ?        S<     0:00 [watchdog/2]
   11 ?        S<     0:00 [migration/3]
   12 ?        SN     0:00 [ksoftirqd/3]
   13 ?        S<     0:00 [watchdog/3]
   14 ?        S<     0:00 [events/0]
   15 ?        S<     0:00 [events/1]
   16 ?        S<     0:00 [events/2]
   17 ?        S<     0:00 [events/3]
   18 ?        S<     0:00 [khelper]
   19 ?        S<     0:00 [kthread]
   26 ?        S<     0:00 [kblockd/0]
   27 ?        S<     0:00 [kblockd/1]
   28 ?        S<     0:00 [kblockd/2]
   29 ?        S<     0:00 [kblockd/3]
   30 ?        S<     0:00 [kacpid]
  117 ?        S<     0:00 [cqueue/0]
  118 ?        S<     0:00 [cqueue/1]
  119 ?        S<     0:00 [cqueue/2]
  120 ?        S<     0:00 [cqueue/3]
  123 ?        S<     0:00 [khubd]
  125 ?        S<     0:00 [kseriod]
  190 ?        S      0:00 [pdflush]
  191 ?        S      0:00 [pdflush]
  192 ?        S<     0:00 [kswapd0]
  193 ?        S<     0:00 [aio/0]
  194 ?        S<     0:00 [aio/1]
  195 ?        S<     0:00 [aio/2]
  196 ?        S<     0:00 [aio/3]
  305 ?        S<     0:00 [kpsmoused]
  331 ?        S<     0:00 [kstriped]
  347 ?        S<     0:00 [ksnapd]
  378 ?        S<     0:00 [ata/0]
  379 ?        S<     0:00 [ata/1]
  380 ?        S<     0:00 [ata/2]
  381 ?        S<     0:00 [ata/3]
  382 ?        S<     0:00 [ata_aux]
  403 ?        S<     0:00 [scsi_eh_0]
  404 ?        S<     0:00 [scsi_eh_1]
  405 ?        S<     0:00 [scsi_eh_2]
  406 ?        S<     0:00 [scsi_eh_3]
  457 ?        S<     0:00 [kjournald]
  508 ?        S<     0:00 [kauditd]
  537 ?        S<s    0:00 /sbin/udevd -d
  654 pts/0    R+     0:00 /bin/ps ax ax
 1637 ?        S<     0:00 [kjournald]
 1645 ?        S<     0:00 [kjournald]
 1652 ?        S<     0:00 [kjournald]
 1656 ?        S<     0:00 [kjournald]
 1663 ?        S<     0:00 [kjournald]
 1929 ?        Ss     0:01 /usr/sbin/syslog-ng -p /var/run/syslog-ng.pid
 2684 ?        Ss     0:00 mcstransd
 2990 ?        S<sl   0:00 auditd
 2992 ?        S<sl   0:00 /sbin/audispd
 3010 ?        Ss     0:00 /usr/sbin/restorecond
 3081 ?        Ss     0:00 irqbalance
 3149 ?        Ss     0:00 /usr/sbin/sshd
 3373 ?        SLs    0:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
 3401 ?        Ss     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3402 ?        S      0:00 /usr/bin/logger -p local6.info
 3403 ?        S      0:00 /usr/bin/logger -p local6.info
 3425 ?        Ss     0:00 crond
 3430 ?        S      0:00 /usr/sbin/fcgi- -DTrafficShield -DWebAccelerator -DSAM
 3433 ?        S      0:00 /usr/local/www/mcpq/mcpq
 3434 ?        S      0:00 /usr/local/www/rrdstats/rrdstats
 3435 ?        Sl     0:02 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3436 ?        S      0:00 /usr/local/www/rtstats/rtstats
 3437 ?        S      0:00 /usr/local/www/iControl/iControlPortal.cgi
 3463 ?        S      0:00 /usr/sbin/smartd -q never
 3467 ?        Ss     0:00 runsvdir /var/service log: ..................................................................................................................
 3468 ?        Ssl    0:00 /usr/bin/promptstatusd
 3469 ?        S<Ls   0:00 /usr/bin/overdog
 3470 ttyS0    Ss+    0:00 /sbin/agetty -L ttyS0 0 vt100
 3548 ?        S      0:00 runsv bigd
 3549 ?        S      0:00 runsv big3d
 3550 ?        S      0:00 runsv pvac
 3551 ?        S      0:00 runsv eam
 3552 ?        S      0:00 runsv ntlmconnpool
 3553 ?        S      0:00 runsv tamd
 3554 ?        S      0:00 runsv nokiasnmpd
 3555 ?        S      0:00 runsv named
 3556 ?        S      0:00 runsv sod
 3557 ?        S      0:00 runsv cssd
 3558 ?        S      0:00 runsv wocplugin
 3559 ?        S      0:00 runsv httpd_apm
 3560 ?        S      0:00 runsv rewrite
 3561 ?        S      0:00 runsv tmrouted
 3562 ?        S      0:00 runsv dedup_admin
 3563 ?        S      0:00 runsv clusterd
 3564 ?        S      0:00 runsv datastor
 3565 ?        S      0:00 runsv logstatd
 3566 ?        S      0:00 runsv stpd
 3567 ?        S      0:00 runsv fpdd
 3568 ?        S      0:00 runsv asm
 3569 ?        S      0:00 runsv csyncd
 3570 ?        S      0:00 runsv hds_prune
 3571 ?        S      0:00 runsv waicd
 3572 ?        S      0:00 runsv aced
 3573 ?        S      0:00 runsv mcpd
 3574 ?        S      0:00 runsv zrd
 3575 ?        S      0:00 runsv mysql
 3576 ?        S      0:00 runsv acctd
 3577 ?        S      0:00 runsv wocd
 3578 ?        S      0:00 runsv lacpd
 3579 ?        S      0:00 runsv eventd
 3580 ?        S      0:00 runsv wccpd
 3581 ?        S      0:00 runsv rmonsnmpd
 3582 ?        S      0:00 runsv websso
 3583 ?        S      0:00 runsv gtmd
 3584 ?        S      0:00 runsv tomcat
 3585 ?        S      0:00 runsv apd
 3586 ?        S      0:00 runsv dnscached
 3587 ?        S      0:00 runsv diskevent
 3588 ?        S      0:00 runsv syscalld
 3589 ?        S      0:00 runsv lind
 3590 ?        S      0:00 runsv snmpd
 3591 ?        S      0:00 runsv subsnmpd
 3592 ?        S      0:00 runsv bcm56xxd
 3593 ?        S      0:00 runsv cbrd
 3594 ?        S      0:00 runsv radvd
 3595 ?        S      0:00 runsv statsd
 3596 ?        S      0:00 runsv alertd
 3597 ?        S      0:00 runsv comm_srv
 3598 ?        S      0:00 runsv chmand
 3599 ?        S      0:00 runsv tmm
 3600 ?        S      0:00 /usr/bin/perl /etc/bigstart/scripts/tmm.start /var/run 4 --platform C106 -m -s 1727
 3601 ?        S      0:00 /shared/bin/big3d
 3603 ?        S      0:00 /usr/bin/cssd -f
 3604 ?        SL     0:00 /usr/bin/sod
 3606 ?        S      0:00 /usr/bin/tmrouted -d 0 -o /var/tmp/tmrouted.out
 3607 ?        S      0:33 /usr/bin/mcpd -dbmem 208 -listen 127.0.0.1 -f
 3612 ?        S      0:00 /usr/bin/csyncd
 3613 ?        SL     0:00 /usr/bin/lacpd
 3614 ?        S      0:00 /usr/sbin/stpd
 3620 ?        Sl     0:00 /usr/bin/eventd -f
 3624 ?        S      0:00 /usr/sbin/ntlmconnpool -c mem://ntlm0
 3626 ?        Sl     0:00 /usr/bin/fpdd
 3628 ?        SLl    0:20 /usr/bin/bcm56xxd -f
 3629 ?        Sl     0:00 /usr/bin/tamd -f
 3630 ?        S      0:00 /usr/sbin/snmpd -f -c /config/snmp/snmpd.conf -s -l /dev/null -P /var/run/snmpd.pid
 3631 ?        Sl     0:00 /usr/bin/chmand -f
 3632 ?        S      0:00 /usr/sbin/logstatd
 3634 ?        S      0:45 /usr/bin/statsd -f
 3636 ?        Sl     0:00 /usr/share/cbr/bin/cbrd --threads=4 --host-memory=402653184 --umu_threshold=90 --pending_trans=5000 --request_size=10240 --big_request_size 1
 3638 ?        S      0:00 /usr/bin/bigd
 3640 ?        S      0:00 /usr/sbin/rmonsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 3641 ?        S      0:00 /usr/bin/syscalld
 3642 ?        S      0:00 su - tomcat -c export JAVA_OPTS='-Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx120m'; exec /usr/sbin/dtomcat run >> /var/run/to
 3644 ?        S      0:00 /usr/sbin/alertd -f
 3665 ?        S      0:00 /usr/bin/lind
 3673 ?        S      0:00 mdadm --monitor --scan --program /usr/lib/install/mdchange
 3688 ?        S      0:00 /sbin/runsm1_named /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 3708 ?        S      0:00 /usr/sbin/subsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 4104 ?        S      0:00 /usr/sbin/LCDd -c /etc/sysconfig/lcdproc/LCDd_mercury.conf
 4903 ?        SL     8:42 tmm.0 --tmid 0 --npus 4 --platform C106 -m -s 1727
 4904 ?        SL     8:07 tmm.1 --tmid 1 --npus 4 --platform C106 -m -s 1727
 4905 ?        SL     8:10 tmm.2 --tmid 2 --npus 4 --platform C106 -m -s 1727
 4906 ?        SL     8:08 tmm.3 --tmid 3 --npus 4 --platform C106 -m -s 1727
 4916 ?        S      0:00 [audit_forwarder]
 5398 ?        S      0:00 /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 5629 ?        Ssl    0:51 /usr/lib/jvm/jre/bin/java -Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx120m -classpath :/usr/share/tomcat/bin/bootstrap.jar:/u
 6087 ?        S      0:01 /usr/sbin/rrdshim
 6130 ?        Sl     0:02 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6179 ?        Sl     0:02 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6371 ?        Sl     0:02 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6373 ?        Sl     0:01 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6374 ?        Sl     0:01 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6377 ?        Sl     0:01 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 6448 ?        Sl     0:02 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
32758 ?        Ss     0:00 sshd: root@pts/0
32764 pts/0    Ss     0:00 -bash

```

```

[root@test:Active] config # ps axf
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:00 init [3]
    2 ?        S<     0:00 [migration/0]
    3 ?        SN     0:00 [ksoftirqd/0]
    4 ?        S<     0:00 [watchdog/0]
    5 ?        S<     0:00 [migration/1]
    6 ?        SN     0:00 [ksoftirqd/1]
    7 ?        S<     0:00 [watchdog/1]
    8 ?        S<     0:00 [migration/2]
    9 ?        SN     0:00 [ksoftirqd/2]
   10 ?        S<     0:00 [watchdog/2]
   11 ?        S<     0:00 [migration/3]
   12 ?        SN     0:00 [ksoftirqd/3]
   13 ?        S<     0:00 [watchdog/3]
   14 ?        S<     0:00 [events/0]
   15 ?        S<     0:00 [events/1]
   16 ?        S<     0:00 [events/2]
   17 ?        S<     0:00 [events/3]
   18 ?        S<     0:00 [khelper]
   19 ?        S<     0:00 [kthread]
   26 ?        S<     0:00  \_ [kblockd/0]
   27 ?        S<     0:00  \_ [kblockd/1]
   28 ?        S<     0:00  \_ [kblockd/2]
   29 ?        S<     0:00  \_ [kblockd/3]
   30 ?        S<     0:00  \_ [kacpid]
  117 ?        S<     0:00  \_ [cqueue/0]
  118 ?        S<     0:00  \_ [cqueue/1]
  119 ?        S<     0:00  \_ [cqueue/2]
  120 ?        S<     0:00  \_ [cqueue/3]
  123 ?        S<     0:00  \_ [khubd]
  125 ?        S<     0:00  \_ [kseriod]
  187 ?        S      0:00  \_ [pdflush]
  188 ?        S      0:00  \_ [pdflush]
  189 ?        S<     0:00  \_ [kswapd0]
  190 ?        S<     0:00  \_ [aio/0]
  191 ?        S<     0:00  \_ [aio/1]
  192 ?        S<     0:00  \_ [aio/2]
  193 ?        S<     0:00  \_ [aio/3]
  302 ?        S<     0:00  \_ [kpsmoused]
  328 ?        S<     0:00  \_ [kstriped]
  344 ?        S<     0:00  \_ [ksnapd]
  375 ?        S<     0:00  \_ [ata/0]
  376 ?        S<     0:00  \_ [ata/1]
  377 ?        S<     0:00  \_ [ata/2]
  378 ?        S<     0:00  \_ [ata/3]
  379 ?        S<     0:00  \_ [ata_aux]
  400 ?        S<     0:00  \_ [scsi_eh_0]
  401 ?        S<     0:00  \_ [scsi_eh_1]
  402 ?        S<     0:00  \_ [scsi_eh_2]
  403 ?        S<     0:00  \_ [scsi_eh_3]
  454 ?        S<     0:00  \_ [kjournald]
  503 ?        S<     0:00  \_ [kauditd]
 1654 ?        S<     0:00  \_ [kjournald]
 1662 ?        S<     0:00  \_ [kjournald]
 1670 ?        S<     0:00  \_ [kjournald]
 1675 ?        S<     0:00  \_ [kjournald]
 1686 ?        S<     0:00  \_ [kjournald]
 5337 ?        S<     0:00  \_ [kjournald]
  532 ?        S<s    0:00 /sbin/udevd -d
 1964 ?        Ss     0:00 /usr/sbin/syslog-ng -p /var/run/syslog-ng.pid
 5011 ?        S      0:00  \_ /usr/bin/audit_forwarder
 2836 ?        Ss     0:00 mcstransd
 3138 ?        S<sl   0:00 auditd
 3140 ?        S<sl   0:00  \_ /sbin/audispd
 3158 ?        Ss     0:00 /usr/sbin/restorecond
 3231 ?        Ss     0:00 irqbalance
 3303 ?        Ss     0:00 /usr/sbin/sshd
 7424 ?        Ss     0:00  \_ sshd: root@pts/0
 7427 pts/0    Ss     0:00      \_ -bash
 7506 pts/0    R+     0:00          \_ /bin/ps ax axf
 3525 ?        SLs    0:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
 3553 ?        Ss     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3554 ?        S      0:00  \_ /usr/bin/logger -p local6.info
 3555 ?        S      0:00  \_ /usr/bin/logger -p local6.info
 3591 ?        S      0:00  \_ /usr/sbin/fcgi- -DTrafficShield -DWebAccelerator -DSAM
 3592 ?        S      0:00  |   \_ /usr/local/www/mcpq/mcpq
 3593 ?        S      0:00  |   \_ /usr/local/www/rrdstats/rrdstats
 3595 ?        S      0:00  |   \_ /usr/local/www/rtstats/rtstats
 3597 ?        S      0:00  |   \_ /usr/local/www/iControl/iControlPortal.cgi
 3594 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7061 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7063 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7064 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7067 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7070 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7072 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7284 ?        Sl     0:00  \_ /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3577 ?        Ss     0:00 crond
 3611 ?        S      0:00 runsvdir /var/service log: ..................................................................................................................
 3705 ?        S      0:00  \_ runsv mcpd
 3757 ?        S      0:31  |   \_ /usr/bin/mcpd -dbmem 208 -listen 127.0.0.1 -f
 3706 ?        S      0:00  \_ runsv waicd
 3823 ?        S      0:00  |   \_ /usr/bin/perl /usr/bin/waicd
 3707 ?        S      0:00  \_ runsv eventd
 3761 ?        Sl     0:00  |   \_ /usr/bin/eventd -f
 3708 ?        S      0:00  \_ runsv cbrd
 3822 ?        Sl     0:00  |   \_ /usr/share/cbr/bin/cbrd --threads=4 --host-memory=402653184 --umu_threshold=90 --pending_trans=5000 --request_size=10240 --big_reques
 3709 ?        S      0:00  \_ runsv cssd
 3796 ?        S      0:00  |   \_ /usr/bin/cssd -f
 3710 ?        S      0:00  \_ runsv tamd
 3775 ?        Sl     0:00  |   \_ /usr/bin/tamd -f
 3711 ?        S      0:00  \_ runsv tomcat
 3764 ?        S      0:00  |   \_ su tomcat -s /bin/bash -c export JAVA_OPTS='-Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx122m '; exec /usr/sbin/dtomca
 6179 ?        Ssl    0:47  |       \_ /usr/lib/jvm/jre/bin/java -Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx122m -classpath :/usr/share/tomcat/bin/boot
 3712 ?        S      0:00  \_ runsv statsd
 3790 ?        S      0:51  |   \_ /usr/bin/statsd -f
 6633 ?        S      0:00  |       \_ /usr/sbin/rrdshim
 3713 ?        S      0:00  \_ runsv clusterd
 3714 ?        S      0:00  \_ runsv chmand
 3769 ?        Sl     0:00  |   \_ /usr/bin/chmand -f
 3715 ?        S      0:00  \_ runsv pvac
 3779 ?        Sl     0:01  |   \_ /usr/bin/pvac -f /config/wa/pvsystem.conf -t 4
 3716 ?        S      0:00  \_ runsv wocd
 3717 ?        S      0:00  \_ runsv websso
 3718 ?        S      0:00  \_ runsv httpd_apm
 3719 ?        S      0:00  \_ runsv logstatd
 3818 ?        S      0:00  |   \_ /usr/sbin/logstatd
 3720 ?        S      0:00  \_ runsv syscalld
 3784 ?        S      0:00  |   \_ /usr/bin/syscalld
 3721 ?        S      0:00  \_ runsv rmonsnmpd
 3760 ?        S      0:00  |   \_ /usr/sbin/rmonsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 3722 ?        S      0:00  \_ runsv diskevent
 3766 ?        S      0:00  |   \_ mdadm --monitor --scan --program /usr/lib/install/mdchange
 3723 ?        S      0:00  \_ runsv eam
 3724 ?        S      0:00  \_ runsv mysql
 3802 ?        S      0:00  |   \_ /usr/share/mysql/mysqlhad
 3725 ?        S      0:00  \_ runsv big3d
 3824 ?        S      0:00  |   \_ /shared/bin/big3d
 3726 ?        S      0:00  \_ runsv rewrite
 3727 ?        S      0:00  \_ runsv nokiasnmpd
 3728 ?        S      0:00  \_ runsv aced
 3729 ?        S      0:00  \_ runsv lacpd
 3767 ?        SL     0:00  |   \_ /usr/bin/lacpd
 3730 ?        S      0:00  \_ runsv acctd
 3731 ?        S      0:00  \_ runsv hds_prune
 3789 ?        S      0:00  |   \_ /usr/bin/perl -w /usr/bin/hds_prune
 3732 ?        S      0:00  \_ runsv fpdd
 3771 ?        Sl     0:01  |   \_ /usr/bin/fpdd
 3958 ?        S      0:00  |       \_ /usr/sbin/LCDd -c /etc/sysconfig/lcdproc/LCDd_mercury.conf
 3733 ?        S      0:00  \_ runsv dnscached
 3734 ?        S      0:00  \_ runsv subsnmpd
 3786 ?        S      0:00  |   \_ /usr/sbin/subsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 3735 ?        S      0:00  \_ runsv stpd
 3814 ?        S      0:00  |   \_ /usr/sbin/stpd
 3736 ?        S      0:00  \_ runsv alertd
 3821 ?        S      0:00  |   \_ /usr/sbin/alertd -f
 3737 ?        S      0:00  \_ runsv apd
 3738 ?        S      0:00  \_ runsv wccpd
 3739 ?        S      0:00  \_ runsv wocplugin
 3740 ?        S      0:00  \_ runsv tmm
 3758 ?        S      0:00  |   \_ /usr/bin/perl /etc/bigstart/scripts/tmm.start /var/run 4 --platform C106 -m -s 1411
 5012 ?        SL     8:40  |       \_ tmm.0 --tmid 0 --npus 4 --platform C106 -m -s 1411
 5013 ?        SL     8:31  |       \_ tmm.1 --tmid 1 --npus 4 --platform C106 -m -s 1411
 5014 ?        RL     8:31  |       \_ tmm.2 --tmid 2 --npus 4 --platform C106 -m -s 1411
 5015 ?        SL     8:21  |       \_ tmm.3 --tmid 3 --npus 4 --platform C106 -m -s 1411
 3741 ?        S      0:00  \_ runsv bigd
 3803 ?        S      0:00  |   \_ /usr/bin/bigd
 3742 ?        S      0:00  \_ runsv zrd
 3743 ?        S      0:00  \_ runsv asm
 3744 ?        S      0:00  \_ runsv bcm56xxd
 3765 ?        SLl    0:18  |   \_ /usr/bin/bcm56xxd -f
 3745 ?        S      0:00  \_ runsv sod
 3774 ?        SL     0:00  |   \_ /usr/bin/sod
 3746 ?        S      0:00  \_ runsv radvd
 3747 ?        S      0:00  \_ runsv datastor
 3748 ?        S      0:00  \_ runsv csyncd
 3776 ?        S      0:00  |   \_ /usr/bin/csyncd
 3749 ?        S      0:00  \_ runsv tmrouted
 3770 ?        S      0:00  |   \_ /usr/bin/tmrouted -d 0 -o /var/tmp/tmrouted.out
 3750 ?        S      0:00  \_ runsv comm_srv
 3829 ?        Sl     0:00  |   \_ /usr/bin/comm_srv -f /config/wa/pvsystem.conf
 3751 ?        S      0:00  \_ runsv snmpd
 3820 ?        S      0:00  |   \_ /usr/sbin/snmpd -f -c /config/snmp/snmpd.conf -s -l /dev/null -P /var/run/snmpd.pid
 3752 ?        S      0:00  \_ runsv lind
 3826 ?        S      0:00  |   \_ /usr/bin/lind
 3753 ?        S      0:00  \_ runsv gtmd
 3754 ?        S      0:00  \_ runsv ntlmconnpool
 3778 ?        S      0:00  |   \_ /usr/sbin/ntlmconnpool -c mem://ntlm0
 3755 ?        S      0:00  \_ runsv dedup_admin
 3756 ?        S      0:00  \_ runsv named
 3782 ?        S      0:00      \_ /sbin/runsm1_named /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 5393 ?        S      0:00          \_ /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 3621 ?        S      0:00 /usr/sbin/smartd -q never
 3625 ?        Ssl    0:00 /usr/bin/promptstatusd
 3626 ?        S<Ls   0:00 /usr/bin/overdog
 3627 ttyS0    Ss+    0:00 /sbin/agetty -L ttyS0 0 vt100
 5517 ?        S      0:00 /bin/sh /usr/bin/mysqld_safe --defaults-extra-file=/var/lib/mysql/my_ssl.cnf --log-error=/var/lib/mysql/mysqld.err --datadir=/var/lib/mysql -
 5666 ?        Sl     0:00  \_ /usr/sbin/mysqld --defaults-extra-file=/var/lib/mysql/my_ssl.cnf --basedir=/ --datadir=/var/lib/mysql --user=mysql --pid-file=/var/lib/my
[root@test:Active] config #

[root@test:Active] config # ps ax
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:00 init [3]
    2 ?        S<     0:00 [migration/0]
    3 ?        SN     0:00 [ksoftirqd/0]
    4 ?        S<     0:00 [watchdog/0]
    5 ?        S<     0:00 [migration/1]
    6 ?        SN     0:00 [ksoftirqd/1]
    7 ?        S<     0:00 [watchdog/1]
    8 ?        S<     0:00 [migration/2]
    9 ?        SN     0:00 [ksoftirqd/2]
   10 ?        S<     0:00 [watchdog/2]
   11 ?        S<     0:00 [migration/3]
   12 ?        SN     0:00 [ksoftirqd/3]
   13 ?        S<     0:00 [watchdog/3]
   14 ?        S<     0:00 [events/0]
   15 ?        S<     0:00 [events/1]
   16 ?        S<     0:00 [events/2]
   17 ?        S<     0:00 [events/3]
   18 ?        S<     0:00 [khelper]
   19 ?        S<     0:00 [kthread]
   26 ?        S<     0:00 [kblockd/0]
   27 ?        S<     0:00 [kblockd/1]
   28 ?        S<     0:00 [kblockd/2]
   29 ?        S<     0:00 [kblockd/3]
   30 ?        S<     0:00 [kacpid]
  117 ?        S<     0:00 [cqueue/0]
  118 ?        S<     0:00 [cqueue/1]
  119 ?        S<     0:00 [cqueue/2]
  120 ?        S<     0:00 [cqueue/3]
  123 ?        S<     0:00 [khubd]
  125 ?        S<     0:00 [kseriod]
  187 ?        S      0:00 [pdflush]
  188 ?        S      0:00 [pdflush]
  189 ?        S<     0:00 [kswapd0]
  190 ?        S<     0:00 [aio/0]
  191 ?        S<     0:00 [aio/1]
  192 ?        S<     0:00 [aio/2]
  193 ?        S<     0:00 [aio/3]
  302 ?        S<     0:00 [kpsmoused]
  328 ?        S<     0:00 [kstriped]
  344 ?        S<     0:00 [ksnapd]
  375 ?        S<     0:00 [ata/0]
  376 ?        S<     0:00 [ata/1]
  377 ?        S<     0:00 [ata/2]
  378 ?        S<     0:00 [ata/3]
  379 ?        S<     0:00 [ata_aux]
  400 ?        S<     0:00 [scsi_eh_0]
  401 ?        S<     0:00 [scsi_eh_1]
  402 ?        S<     0:00 [scsi_eh_2]
  403 ?        S<     0:00 [scsi_eh_3]
  454 ?        S<     0:00 [kjournald]
  503 ?        S<     0:00 [kauditd]
  532 ?        S<s    0:00 /sbin/udevd -d
 1654 ?        S<     0:00 [kjournald]
 1662 ?        S<     0:00 [kjournald]
 1670 ?        S<     0:00 [kjournald]
 1675 ?        S<     0:00 [kjournald]
 1686 ?        S<     0:00 [kjournald]
 1964 ?        Ss     0:00 /usr/sbin/syslog-ng -p /var/run/syslog-ng.pid
 2836 ?        Ss     0:00 mcstransd
 3138 ?        S<sl   0:00 auditd
 3140 ?        S<sl   0:00 /sbin/audispd
 3158 ?        Ss     0:00 /usr/sbin/restorecond
 3231 ?        Ss     0:00 irqbalance
 3303 ?        Ss     0:00 /usr/sbin/sshd
 3525 ?        SLs    0:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
 3553 ?        Ss     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3554 ?        S      0:00 /usr/bin/logger -p local6.info
 3555 ?        S      0:00 /usr/bin/logger -p local6.info
 3577 ?        Ss     0:00 crond
 3591 ?        S      0:00 /usr/sbin/fcgi- -DTrafficShield -DWebAccelerator -DSAM
 3592 ?        S      0:00 /usr/local/www/mcpq/mcpq
 3593 ?        S      0:00 /usr/local/www/rrdstats/rrdstats
 3594 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 3595 ?        S      0:00 /usr/local/www/rtstats/rtstats
 3597 ?        S      0:00 /usr/local/www/iControl/iControlPortal.cgi
 3611 ?        S      0:00 runsvdir /var/service log: ..................................................................................................................
 3621 ?        S      0:00 /usr/sbin/smartd -q never
 3625 ?        Ssl    0:00 /usr/bin/promptstatusd
 3626 ?        S<Ls   0:00 /usr/bin/overdog
 3627 ttyS0    Ss+    0:00 /sbin/agetty -L ttyS0 0 vt100
 3705 ?        S      0:00 runsv mcpd
 3706 ?        S      0:00 runsv waicd
 3707 ?        S      0:00 runsv eventd
 3708 ?        S      0:00 runsv cbrd
 3709 ?        S      0:00 runsv cssd
 3710 ?        S      0:00 runsv tamd
 3711 ?        S      0:00 runsv tomcat
 3712 ?        S      0:00 runsv statsd
 3713 ?        S      0:00 runsv clusterd
 3714 ?        S      0:00 runsv chmand
 3715 ?        S      0:00 runsv pvac
 3716 ?        S      0:00 runsv wocd
 3717 ?        S      0:00 runsv websso
 3718 ?        S      0:00 runsv httpd_apm
 3719 ?        S      0:00 runsv logstatd
 3720 ?        S      0:00 runsv syscalld
 3721 ?        S      0:00 runsv rmonsnmpd
 3722 ?        S      0:00 runsv diskevent
 3723 ?        S      0:00 runsv eam
 3724 ?        S      0:00 runsv mysql
 3725 ?        S      0:00 runsv big3d
 3726 ?        S      0:00 runsv rewrite
 3727 ?        S      0:00 runsv nokiasnmpd
 3728 ?        S      0:00 runsv aced
 3729 ?        S      0:00 runsv lacpd
 3730 ?        S      0:00 runsv acctd
 3731 ?        S      0:00 runsv hds_prune
 3732 ?        S      0:00 runsv fpdd
 3733 ?        S      0:00 runsv dnscached
 3734 ?        S      0:00 runsv subsnmpd
 3735 ?        S      0:00 runsv stpd
 3736 ?        S      0:00 runsv alertd
 3737 ?        S      0:00 runsv apd
 3738 ?        S      0:00 runsv wccpd
 3739 ?        S      0:00 runsv wocplugin
 3740 ?        S      0:00 runsv tmm
 3741 ?        S      0:00 runsv bigd
 3742 ?        S      0:00 runsv zrd
 3743 ?        S      0:00 runsv asm
 3744 ?        S      0:00 runsv bcm56xxd
 3745 ?        S      0:00 runsv sod
 3746 ?        S      0:00 runsv radvd
 3747 ?        S      0:00 runsv datastor
 3748 ?        S      0:00 runsv csyncd
 3749 ?        S      0:00 runsv tmrouted
 3750 ?        S      0:00 runsv comm_srv
 3751 ?        S      0:00 runsv snmpd
 3752 ?        S      0:00 runsv lind
 3753 ?        S      0:00 runsv gtmd
 3754 ?        S      0:00 runsv ntlmconnpool
 3755 ?        S      0:00 runsv dedup_admin
 3756 ?        S      0:00 runsv named
 3757 ?        S      0:31 /usr/bin/mcpd -dbmem 208 -listen 127.0.0.1 -f
 3758 ?        S      0:00 /usr/bin/perl /etc/bigstart/scripts/tmm.start /var/run 4 --platform C106 -m -s 1411
 3760 ?        S      0:00 /usr/sbin/rmonsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 3761 ?        Sl     0:00 /usr/bin/eventd -f
 3764 ?        S      0:00 su tomcat -s /bin/bash -c export JAVA_OPTS='-Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx122m '; exec /usr/sbin/dtomcat run >>
 3765 ?        SLl    0:18 /usr/bin/bcm56xxd -f
 3766 ?        S      0:00 mdadm --monitor --scan --program /usr/lib/install/mdchange
 3767 ?        SL     0:00 /usr/bin/lacpd
 3769 ?        Sl     0:00 /usr/bin/chmand -f
 3770 ?        S      0:00 /usr/bin/tmrouted -d 0 -o /var/tmp/tmrouted.out
 3771 ?        Sl     0:01 /usr/bin/fpdd
 3774 ?        SL     0:00 /usr/bin/sod
 3775 ?        Sl     0:00 /usr/bin/tamd -f
 3776 ?        S      0:00 /usr/bin/csyncd
 3778 ?        S      0:00 /usr/sbin/ntlmconnpool -c mem://ntlm0
 3779 ?        Sl     0:01 /usr/bin/pvac -f /config/wa/pvsystem.conf -t 4
 3782 ?        S      0:00 /sbin/runsm1_named /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 3784 ?        S      0:00 /usr/bin/syscalld
 3786 ?        S      0:00 /usr/sbin/subsnmpd -f -c /config/snmp/subagents.conf -s -l /dev/null
 3789 ?        S      0:00 /usr/bin/perl -w /usr/bin/hds_prune
 3790 ?        S      0:51 /usr/bin/statsd -f
 3796 ?        S      0:00 /usr/bin/cssd -f
 3802 ?        S      0:00 /usr/share/mysql/mysqlhad
 3803 ?        S      0:00 /usr/bin/bigd
 3814 ?        S      0:00 /usr/sbin/stpd
 3818 ?        S      0:00 /usr/sbin/logstatd
 3820 ?        S      0:00 /usr/sbin/snmpd -f -c /config/snmp/snmpd.conf -s -l /dev/null -P /var/run/snmpd.pid
 3821 ?        S      0:00 /usr/sbin/alertd -f
 3822 ?        Sl     0:00 /usr/share/cbr/bin/cbrd --threads=4 --host-memory=402653184 --umu_threshold=90 --pending_trans=5000 --request_size=10240 --big_request_size 1
 3823 ?        S      0:00 /usr/bin/perl /usr/bin/waicd
 3824 ?        S      0:00 /shared/bin/big3d
 3826 ?        S      0:00 /usr/bin/lind
 3829 ?        Sl     0:00 /usr/bin/comm_srv -f /config/wa/pvsystem.conf
 3958 ?        S      0:00 /usr/sbin/LCDd -c /etc/sysconfig/lcdproc/LCDd_mercury.conf
 5011 ?        S      0:00 /usr/bin/audit_forwarder
 5012 ?        SL     8:42 tmm.0 --tmid 0 --npus 4 --platform C106 -m -s 1411
 5013 ?        SL     8:33 tmm.1 --tmid 1 --npus 4 --platform C106 -m -s 1411
 5014 ?        SL     8:33 tmm.2 --tmid 2 --npus 4 --platform C106 -m -s 1411
 5015 ?        SL     8:23 tmm.3 --tmid 3 --npus 4 --platform C106 -m -s 1411
 5337 ?        S<     0:00 [kjournald]
 5393 ?        S      0:00 /usr/sbin/named -f -t /var/named -u named -c /config/named.conf
 5517 ?        S      0:00 /bin/sh /usr/bin/mysqld_safe --defaults-extra-file=/var/lib/mysql/my_ssl.cnf --log-error=/var/lib/mysql/mysqld.err --datadir=/var/lib/mysql -
 5666 ?        Sl     0:00 /usr/sbin/mysqld --defaults-extra-file=/var/lib/mysql/my_ssl.cnf --basedir=/ --datadir=/var/lib/mysql --user=mysql --pid-file=/var/lib/mysql/
 6179 ?        Ssl    0:47 /usr/lib/jvm/jre/bin/java -Dpython.cachedir=/var/tmp -Djava.library.path=/usr/lib -Xmx122m -classpath :/usr/share/tomcat/bin/bootstrap.jar:/u
 6633 ?        S      0:00 /usr/sbin/rrdshim
 7061 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7063 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7064 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7067 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7070 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7072 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7284 ?        Sl     0:00 /usr/sbin/httpd -DTrafficShield -DWebAccelerator -DSAM
 7424 ?        Ss     0:00 sshd: root@pts/0
 7427 pts/0    Ss     0:00 -bash
 7512 pts/0    R+     0:00 /bin/ps ax ax

```

## sysctl

```
[root@test:Active] config # sysctl -a
crypto.fips_enabled = 0
abi.vsyscall32 = 1
dev.parport.default.spintime = 500
dev.parport.default.timeslice = 200
dev.raid.speed_limit_max = 200000
dev.raid.speed_limit_min = 1000
dev.hpet.max-user-freq = 64
dev.rtc.max-user-freq = 64
dev.scsi.logging_level = 0
debug.exception-trace = 1
net.lasthop.idle_timeout.other = 5
net.lasthop.idle_timeout.udp = 5
net.lasthop.idle_timeout.tcp = 5
net.lasthop.sweep_entries = 64
net.lasthop.max_entries = 65536
net.ipv6.conf.Internal.rp_filter = 0
net.ipv6.conf.Internal.router_probe_interval = 60
net.ipv6.conf.Internal.accept_ra_rtr_pref = 1
net.ipv6.conf.Internal.accept_ra_pinfo = 1
net.ipv6.conf.Internal.accept_ra_defrtr = 1
net.ipv6.conf.Internal.max_addresses = 16
net.ipv6.conf.Internal.max_desync_factor = 600
net.ipv6.conf.Internal.regen_max_retry = 5
net.ipv6.conf.Internal.temp_prefered_lft = 86400
net.ipv6.conf.Internal.temp_valid_lft = 604800
net.ipv6.conf.Internal.use_tempaddr = 0
net.ipv6.conf.Internal.accept_dad = 1
net.ipv6.conf.Internal.disable_ipv6 = 0
net.ipv6.conf.Internal.force_mld_version = 0
net.ipv6.conf.Internal.router_solicitation_delay = 1
net.ipv6.conf.Internal.router_solicitation_interval = 4
net.ipv6.conf.Internal.router_solicitations = 3
net.ipv6.conf.Internal.dad_transmits = 1
net.ipv6.conf.Internal.autoconf = 0
net.ipv6.conf.Internal.accept_redirects = 1
net.ipv6.conf.Internal.accept_ra = 0
net.ipv6.conf.Internal.mtu = 1500
net.ipv6.conf.Internal.hop_limit = 64
net.ipv6.conf.Internal.forwarding = 0
net.ipv6.conf.tmm0.rp_filter = 0
net.ipv6.conf.tmm0.router_probe_interval = 60
net.ipv6.conf.tmm0.accept_ra_rtr_pref = 1
net.ipv6.conf.tmm0.accept_ra_pinfo = 1
net.ipv6.conf.tmm0.accept_ra_defrtr = 1
net.ipv6.conf.tmm0.max_addresses = 16
net.ipv6.conf.tmm0.max_desync_factor = 600
net.ipv6.conf.tmm0.regen_max_retry = 5
net.ipv6.conf.tmm0.temp_prefered_lft = 86400
net.ipv6.conf.tmm0.temp_valid_lft = 604800
net.ipv6.conf.tmm0.use_tempaddr = 0
net.ipv6.conf.tmm0.accept_dad = 1
net.ipv6.conf.tmm0.disable_ipv6 = 0
net.ipv6.conf.tmm0.force_mld_version = 0
net.ipv6.conf.tmm0.router_solicitation_delay = 1
net.ipv6.conf.tmm0.router_solicitation_interval = 4
net.ipv6.conf.tmm0.router_solicitations = 3
net.ipv6.conf.tmm0.dad_transmits = 1
net.ipv6.conf.tmm0.autoconf = 0
net.ipv6.conf.tmm0.accept_redirects = 1
net.ipv6.conf.tmm0.accept_ra = 0
net.ipv6.conf.tmm0.mtu = 1500
net.ipv6.conf.tmm0.hop_limit = 64
net.ipv6.conf.tmm0.forwarding = 0
net.ipv6.conf.eth0/1.rp_filter = 0
net.ipv6.conf.eth0/1.router_probe_interval = 60
net.ipv6.conf.eth0/1.accept_ra_rtr_pref = 1
net.ipv6.conf.eth0/1.accept_ra_pinfo = 1
net.ipv6.conf.eth0/1.accept_ra_defrtr = 1
net.ipv6.conf.eth0/1.max_addresses = 16
net.ipv6.conf.eth0/1.max_desync_factor = 600
net.ipv6.conf.eth0/1.regen_max_retry = 5
net.ipv6.conf.eth0/1.temp_prefered_lft = 86400
net.ipv6.conf.eth0/1.temp_valid_lft = 604800
net.ipv6.conf.eth0/1.use_tempaddr = 0
net.ipv6.conf.eth0/1.accept_dad = 1
net.ipv6.conf.eth0/1.disable_ipv6 = 0
net.ipv6.conf.eth0/1.force_mld_version = 0
net.ipv6.conf.eth0/1.router_solicitation_delay = 1
net.ipv6.conf.eth0/1.router_solicitation_interval = 4
net.ipv6.conf.eth0/1.router_solicitations = 3
net.ipv6.conf.eth0/1.dad_transmits = 1
net.ipv6.conf.eth0/1.autoconf = 0
net.ipv6.conf.eth0/1.accept_redirects = 1
net.ipv6.conf.eth0/1.accept_ra = 0
net.ipv6.conf.eth0/1.mtu = 1500
net.ipv6.conf.eth0/1.hop_limit = 64
net.ipv6.conf.eth0/1.forwarding = 0
net.ipv6.conf.eth0.rp_filter = 0
net.ipv6.conf.eth0.router_probe_interval = 60
net.ipv6.conf.eth0.accept_ra_rtr_pref = 1
net.ipv6.conf.eth0.accept_ra_pinfo = 1
net.ipv6.conf.eth0.accept_ra_defrtr = 1
net.ipv6.conf.eth0.max_addresses = 16
net.ipv6.conf.eth0.max_desync_factor = 600
net.ipv6.conf.eth0.regen_max_retry = 5
net.ipv6.conf.eth0.temp_prefered_lft = 86400
net.ipv6.conf.eth0.temp_valid_lft = 604800
net.ipv6.conf.eth0.use_tempaddr = 0
net.ipv6.conf.eth0.accept_dad = 1
net.ipv6.conf.eth0.disable_ipv6 = 0
net.ipv6.conf.eth0.force_mld_version = 0
net.ipv6.conf.eth0.router_solicitation_delay = 1
net.ipv6.conf.eth0.router_solicitation_interval = 4
net.ipv6.conf.eth0.router_solicitations = 3
net.ipv6.conf.eth0.dad_transmits = 1
net.ipv6.conf.eth0.autoconf = 0
net.ipv6.conf.eth0.accept_redirects = 1
net.ipv6.conf.eth0.accept_ra = 0
net.ipv6.conf.eth0.mtu = 1500
net.ipv6.conf.eth0.hop_limit = 64
net.ipv6.conf.eth0.forwarding = 0
net.ipv6.conf.default.rp_filter = 0
net.ipv6.conf.default.router_probe_interval = 60
net.ipv6.conf.default.accept_ra_rtr_pref = 1
net.ipv6.conf.default.accept_ra_pinfo = 1
net.ipv6.conf.default.accept_ra_defrtr = 1
net.ipv6.conf.default.max_addresses = 16
net.ipv6.conf.default.max_desync_factor = 600
net.ipv6.conf.default.regen_max_retry = 5
net.ipv6.conf.default.temp_prefered_lft = 86400
net.ipv6.conf.default.temp_valid_lft = 604800
net.ipv6.conf.default.use_tempaddr = 0
net.ipv6.conf.default.accept_dad = 1
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.default.force_mld_version = 0
net.ipv6.conf.default.router_solicitation_delay = 1
net.ipv6.conf.default.router_solicitation_interval = 4
net.ipv6.conf.default.router_solicitations = 3
net.ipv6.conf.default.dad_transmits = 1
net.ipv6.conf.default.autoconf = 0
net.ipv6.conf.default.accept_redirects = 1
net.ipv6.conf.default.accept_ra = 0
net.ipv6.conf.default.mtu = 1280
net.ipv6.conf.default.hop_limit = 64
net.ipv6.conf.default.forwarding = 0
net.ipv6.conf.all.rp_filter = 1
net.ipv6.conf.all.router_probe_interval = 60
net.ipv6.conf.all.accept_ra_rtr_pref = 1
net.ipv6.conf.all.accept_ra_pinfo = 1
net.ipv6.conf.all.accept_ra_defrtr = 1
net.ipv6.conf.all.max_addresses = 16
net.ipv6.conf.all.max_desync_factor = 600
net.ipv6.conf.all.regen_max_retry = 5
net.ipv6.conf.all.temp_prefered_lft = 86400
net.ipv6.conf.all.temp_valid_lft = 604800
net.ipv6.conf.all.use_tempaddr = 0
net.ipv6.conf.all.accept_dad = 1
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.all.force_mld_version = 0
net.ipv6.conf.all.router_solicitation_delay = 1
net.ipv6.conf.all.router_solicitation_interval = 4
net.ipv6.conf.all.router_solicitations = 3
net.ipv6.conf.all.dad_transmits = 1
net.ipv6.conf.all.autoconf = 0
net.ipv6.conf.all.accept_redirects = 1
net.ipv6.conf.all.accept_ra = 0
net.ipv6.conf.all.mtu = 1280
net.ipv6.conf.all.hop_limit = 64
net.ipv6.conf.all.forwarding = 0
net.ipv6.conf.lo.rp_filter = 0
net.ipv6.conf.lo.router_probe_interval = 60
net.ipv6.conf.lo.accept_ra_rtr_pref = 1
net.ipv6.conf.lo.accept_ra_pinfo = 1
net.ipv6.conf.lo.accept_ra_defrtr = 1
net.ipv6.conf.lo.max_addresses = 16
net.ipv6.conf.lo.max_desync_factor = 600
net.ipv6.conf.lo.regen_max_retry = 5
net.ipv6.conf.lo.temp_prefered_lft = 86400
net.ipv6.conf.lo.temp_valid_lft = 604800
net.ipv6.conf.lo.use_tempaddr = -1
net.ipv6.conf.lo.accept_dad = 1
net.ipv6.conf.lo.disable_ipv6 = 0
net.ipv6.conf.lo.force_mld_version = 0
net.ipv6.conf.lo.router_solicitation_delay = 1
net.ipv6.conf.lo.router_solicitation_interval = 4
net.ipv6.conf.lo.router_solicitations = 3
net.ipv6.conf.lo.dad_transmits = 1
net.ipv6.conf.lo.autoconf = 1
net.ipv6.conf.lo.accept_redirects = 1
net.ipv6.conf.lo.accept_ra = 1
net.ipv6.conf.lo.mtu = 16436
net.ipv6.conf.lo.hop_limit = 64
net.ipv6.conf.lo.forwarding = 0
net.ipv6.neigh.Internal.base_reachable_time_ms = 30000
net.ipv6.neigh.Internal.retrans_time_ms = 1000
net.ipv6.neigh.Internal.locktime = 0
net.ipv6.neigh.Internal.proxy_delay = 79
net.ipv6.neigh.Internal.anycast_delay = 99
net.ipv6.neigh.Internal.proxy_qlen = 64
net.ipv6.neigh.Internal.unres_qlen = 3
net.ipv6.neigh.Internal.gc_stale_time = 60
net.ipv6.neigh.Internal.delay_first_probe_time = 5
net.ipv6.neigh.Internal.base_reachable_time = 30
net.ipv6.neigh.Internal.retrans_time = 1000
net.ipv6.neigh.Internal.app_solicit = 0
net.ipv6.neigh.Internal.ucast_solicit = 3
net.ipv6.neigh.Internal.mcast_solicit = 3
net.ipv6.neigh.tmm0.base_reachable_time_ms = 30000
net.ipv6.neigh.tmm0.retrans_time_ms = 1000
net.ipv6.neigh.tmm0.locktime = 0
net.ipv6.neigh.tmm0.proxy_delay = 79
net.ipv6.neigh.tmm0.anycast_delay = 99
net.ipv6.neigh.tmm0.proxy_qlen = 64
net.ipv6.neigh.tmm0.unres_qlen = 3
net.ipv6.neigh.tmm0.gc_stale_time = 60
net.ipv6.neigh.tmm0.delay_first_probe_time = 5
net.ipv6.neigh.tmm0.base_reachable_time = 30
net.ipv6.neigh.tmm0.retrans_time = 1000
net.ipv6.neigh.tmm0.app_solicit = 0
net.ipv6.neigh.tmm0.ucast_solicit = 3
net.ipv6.neigh.tmm0.mcast_solicit = 3
net.ipv6.neigh.eth0/1.base_reachable_time_ms = 30000
net.ipv6.neigh.eth0/1.retrans_time_ms = 1000
net.ipv6.neigh.eth0/1.locktime = 0
net.ipv6.neigh.eth0/1.proxy_delay = 79
net.ipv6.neigh.eth0/1.anycast_delay = 99
net.ipv6.neigh.eth0/1.proxy_qlen = 64
net.ipv6.neigh.eth0/1.unres_qlen = 3
net.ipv6.neigh.eth0/1.gc_stale_time = 60
net.ipv6.neigh.eth0/1.delay_first_probe_time = 5
net.ipv6.neigh.eth0/1.base_reachable_time = 30
net.ipv6.neigh.eth0/1.retrans_time = 1000
net.ipv6.neigh.eth0/1.app_solicit = 0
net.ipv6.neigh.eth0/1.ucast_solicit = 3
net.ipv6.neigh.eth0/1.mcast_solicit = 3
net.ipv6.neigh.eth0.base_reachable_time_ms = 30000
net.ipv6.neigh.eth0.retrans_time_ms = 1000
net.ipv6.neigh.eth0.locktime = 0
net.ipv6.neigh.eth0.proxy_delay = 79
net.ipv6.neigh.eth0.anycast_delay = 99
net.ipv6.neigh.eth0.proxy_qlen = 64
net.ipv6.neigh.eth0.unres_qlen = 3
net.ipv6.neigh.eth0.gc_stale_time = 60
net.ipv6.neigh.eth0.delay_first_probe_time = 5
net.ipv6.neigh.eth0.base_reachable_time = 30
net.ipv6.neigh.eth0.retrans_time = 1000
net.ipv6.neigh.eth0.app_solicit = 0
net.ipv6.neigh.eth0.ucast_solicit = 3
net.ipv6.neigh.eth0.mcast_solicit = 3
net.ipv6.neigh.lo.base_reachable_time_ms = 30000
net.ipv6.neigh.lo.retrans_time_ms = 1000
net.ipv6.neigh.lo.locktime = 0
net.ipv6.neigh.lo.proxy_delay = 79
net.ipv6.neigh.lo.anycast_delay = 99
net.ipv6.neigh.lo.proxy_qlen = 64
net.ipv6.neigh.lo.unres_qlen = 3
net.ipv6.neigh.lo.gc_stale_time = 60
net.ipv6.neigh.lo.delay_first_probe_time = 5
net.ipv6.neigh.lo.base_reachable_time = 30
net.ipv6.neigh.lo.retrans_time = 1000
net.ipv6.neigh.lo.app_solicit = 0
net.ipv6.neigh.lo.ucast_solicit = 3
net.ipv6.neigh.lo.mcast_solicit = 3
net.ipv6.neigh.default.base_reachable_time_ms = 30000
net.ipv6.neigh.default.retrans_time_ms = 1000
net.ipv6.neigh.default.gc_thresh3 = 2048
net.ipv6.neigh.default.gc_thresh2 = 1024
net.ipv6.neigh.default.gc_thresh1 = 128
net.ipv6.neigh.default.gc_interval = 30
net.ipv6.neigh.default.locktime = 0
net.ipv6.neigh.default.proxy_delay = 79
net.ipv6.neigh.default.anycast_delay = 99
net.ipv6.neigh.default.proxy_qlen = 64
net.ipv6.neigh.default.unres_qlen = 3
net.ipv6.neigh.default.gc_stale_time = 60
net.ipv6.neigh.default.delay_first_probe_time = 5
net.ipv6.neigh.default.base_reachable_time = 30
net.ipv6.neigh.default.retrans_time = 1000
net.ipv6.neigh.default.app_solicit = 0
net.ipv6.neigh.default.ucast_solicit = 3
net.ipv6.neigh.default.mcast_solicit = 3
net.ipv6.optimistic_dad = 0
net.ipv6.mld_max_msf = 64
net.ipv6.ip6frag_secret_interval = 600
net.ipv6.ip6frag_time = 60
net.ipv6.ip6frag_low_thresh = 196608
net.ipv6.ip6frag_high_thresh = 262144
net.ipv6.bindv6only = 0
net.ipv6.icmp.ratelimit = 1000
net.ipv6.route.gc_min_interval_ms = 500
net.ipv6.route.min_adv_mss = 1
net.ipv6.route.mtu_expires = 600
net.ipv6.route.gc_elasticity = 0
net.ipv6.route.gc_interval = 30
net.ipv6.route.gc_timeout = 60
net.ipv6.route.gc_min_interval = 0
net.ipv6.route.max_size = 8192
net.ipv6.route.gc_thresh = 1024
net.unix.max_dgram_qlen = 10
net.ipv4.conf.Internal.promote_secondaries = 1
net.ipv4.conf.Internal.force_igmp_version = 0
net.ipv4.conf.Internal.disable_policy = 0
net.ipv4.conf.Internal.disable_xfrm = 0
net.ipv4.conf.Internal.arp_accept = 0
net.ipv4.conf.Internal.arp_ignore = 0
net.ipv4.conf.Internal.arp_announce = 0
net.ipv4.conf.Internal.arp_filter = 0
net.ipv4.conf.Internal.tag = 0
net.ipv4.conf.Internal.log_martians = 0
net.ipv4.conf.Internal.bootp_relay = 0
net.ipv4.conf.Internal.medium_id = 0
net.ipv4.conf.Internal.proxy_arp = 0
net.ipv4.conf.Internal.accept_source_route = 0
net.ipv4.conf.Internal.send_redirects = 1
net.ipv4.conf.Internal.rp_filter = 0
net.ipv4.conf.Internal.shared_media = 1
net.ipv4.conf.Internal.secure_redirects = 1
net.ipv4.conf.Internal.accept_redirects = 1
net.ipv4.conf.Internal.mc_forwarding = 0
net.ipv4.conf.Internal.forwarding = 0
net.ipv4.conf.eth0.promote_secondaries = 1
net.ipv4.conf.eth0.force_igmp_version = 0
net.ipv4.conf.eth0.disable_policy = 0
net.ipv4.conf.eth0.disable_xfrm = 0
net.ipv4.conf.eth0.arp_accept = 0
net.ipv4.conf.eth0.arp_ignore = 0
net.ipv4.conf.eth0.arp_announce = 0
net.ipv4.conf.eth0.arp_filter = 0
net.ipv4.conf.eth0.tag = 0
net.ipv4.conf.eth0.log_martians = 0
net.ipv4.conf.eth0.bootp_relay = 0
net.ipv4.conf.eth0.medium_id = 0
net.ipv4.conf.eth0.proxy_arp = 0
net.ipv4.conf.eth0.accept_source_route = 0
net.ipv4.conf.eth0.send_redirects = 1
net.ipv4.conf.eth0.rp_filter = 0
net.ipv4.conf.eth0.shared_media = 1
net.ipv4.conf.eth0.secure_redirects = 1
net.ipv4.conf.eth0.accept_redirects = 1
net.ipv4.conf.eth0.mc_forwarding = 0
net.ipv4.conf.eth0.forwarding = 0
net.ipv4.conf.tmm0.promote_secondaries = 1
net.ipv4.conf.tmm0.force_igmp_version = 0
net.ipv4.conf.tmm0.disable_policy = 0
net.ipv4.conf.tmm0.disable_xfrm = 0
net.ipv4.conf.tmm0.arp_accept = 0
net.ipv4.conf.tmm0.arp_ignore = 0
net.ipv4.conf.tmm0.arp_announce = 0
net.ipv4.conf.tmm0.arp_filter = 0
net.ipv4.conf.tmm0.tag = 0
net.ipv4.conf.tmm0.log_martians = 0
net.ipv4.conf.tmm0.bootp_relay = 0
net.ipv4.conf.tmm0.medium_id = 0
net.ipv4.conf.tmm0.proxy_arp = 0
net.ipv4.conf.tmm0.accept_source_route = 0
net.ipv4.conf.tmm0.send_redirects = 1
net.ipv4.conf.tmm0.rp_filter = 0
net.ipv4.conf.tmm0.shared_media = 1
net.ipv4.conf.tmm0.secure_redirects = 1
net.ipv4.conf.tmm0.accept_redirects = 1
net.ipv4.conf.tmm0.mc_forwarding = 0
net.ipv4.conf.tmm0.forwarding = 0
net.ipv4.conf.eth0/1.promote_secondaries = 1
net.ipv4.conf.eth0/1.force_igmp_version = 0
net.ipv4.conf.eth0/1.disable_policy = 0
net.ipv4.conf.eth0/1.disable_xfrm = 0
net.ipv4.conf.eth0/1.arp_accept = 0
net.ipv4.conf.eth0/1.arp_ignore = 0
net.ipv4.conf.eth0/1.arp_announce = 0
net.ipv4.conf.eth0/1.arp_filter = 0
net.ipv4.conf.eth0/1.tag = 0
net.ipv4.conf.eth0/1.log_martians = 0
net.ipv4.conf.eth0/1.bootp_relay = 0
net.ipv4.conf.eth0/1.medium_id = 0
net.ipv4.conf.eth0/1.proxy_arp = 0
net.ipv4.conf.eth0/1.accept_source_route = 0
net.ipv4.conf.eth0/1.send_redirects = 1
net.ipv4.conf.eth0/1.rp_filter = 0
net.ipv4.conf.eth0/1.shared_media = 1
net.ipv4.conf.eth0/1.secure_redirects = 1
net.ipv4.conf.eth0/1.accept_redirects = 1
net.ipv4.conf.eth0/1.mc_forwarding = 0
net.ipv4.conf.eth0/1.forwarding = 0
net.ipv4.conf.lo.promote_secondaries = 0
net.ipv4.conf.lo.force_igmp_version = 0
net.ipv4.conf.lo.disable_policy = 1
net.ipv4.conf.lo.disable_xfrm = 1
net.ipv4.conf.lo.arp_accept = 0
net.ipv4.conf.lo.arp_ignore = 0
net.ipv4.conf.lo.arp_announce = 0
net.ipv4.conf.lo.arp_filter = 0
net.ipv4.conf.lo.tag = 0
net.ipv4.conf.lo.log_martians = 0
net.ipv4.conf.lo.bootp_relay = 0
net.ipv4.conf.lo.medium_id = 0
net.ipv4.conf.lo.proxy_arp = 0
net.ipv4.conf.lo.accept_source_route = 1
net.ipv4.conf.lo.send_redirects = 1
net.ipv4.conf.lo.rp_filter = 0
net.ipv4.conf.lo.shared_media = 1
net.ipv4.conf.lo.secure_redirects = 1
net.ipv4.conf.lo.accept_redirects = 1
net.ipv4.conf.lo.mc_forwarding = 0
net.ipv4.conf.lo.forwarding = 0
net.ipv4.conf.default.promote_secondaries = 1
net.ipv4.conf.default.force_igmp_version = 0
net.ipv4.conf.default.disable_policy = 0
net.ipv4.conf.default.disable_xfrm = 0
net.ipv4.conf.default.arp_accept = 0
net.ipv4.conf.default.arp_ignore = 0
net.ipv4.conf.default.arp_announce = 0
net.ipv4.conf.default.arp_filter = 0
net.ipv4.conf.default.tag = 0
net.ipv4.conf.default.log_martians = 0
net.ipv4.conf.default.bootp_relay = 0
net.ipv4.conf.default.medium_id = 0
net.ipv4.conf.default.proxy_arp = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.default.send_redirects = 1
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.default.shared_media = 1
net.ipv4.conf.default.secure_redirects = 1
net.ipv4.conf.default.accept_redirects = 1
net.ipv4.conf.default.mc_forwarding = 0
net.ipv4.conf.default.forwarding = 0
net.ipv4.conf.all.promote_secondaries = 1
net.ipv4.conf.all.force_igmp_version = 0
net.ipv4.conf.all.disable_policy = 0
net.ipv4.conf.all.disable_xfrm = 0
net.ipv4.conf.all.arp_accept = 0
net.ipv4.conf.all.arp_ignore = 0
net.ipv4.conf.all.arp_announce = 1
net.ipv4.conf.all.arp_filter = 1
net.ipv4.conf.all.tag = 0
net.ipv4.conf.all.log_martians = 0
net.ipv4.conf.all.bootp_relay = 0
net.ipv4.conf.all.medium_id = 0
net.ipv4.conf.all.proxy_arp = 0
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.send_redirects = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.shared_media = 1
net.ipv4.conf.all.secure_redirects = 1
net.ipv4.conf.all.accept_redirects = 1
net.ipv4.conf.all.mc_forwarding = 0
net.ipv4.conf.all.forwarding = 0
net.ipv4.neigh.Internal.base_reachable_time_ms = 30000
net.ipv4.neigh.Internal.retrans_time_ms = 1000
net.ipv4.neigh.Internal.locktime = 99
net.ipv4.neigh.Internal.proxy_delay = 79
net.ipv4.neigh.Internal.anycast_delay = 99
net.ipv4.neigh.Internal.proxy_qlen = 64
net.ipv4.neigh.Internal.unres_qlen = 3
net.ipv4.neigh.Internal.gc_stale_time = 60
net.ipv4.neigh.Internal.delay_first_probe_time = 5
net.ipv4.neigh.Internal.base_reachable_time = 30
net.ipv4.neigh.Internal.retrans_time = 99
net.ipv4.neigh.Internal.app_solicit = 0
net.ipv4.neigh.Internal.ucast_solicit = 3
net.ipv4.neigh.Internal.mcast_solicit = 3
net.ipv4.neigh.eth0.base_reachable_time_ms = 30000
net.ipv4.neigh.eth0.retrans_time_ms = 1000
net.ipv4.neigh.eth0.locktime = 99
net.ipv4.neigh.eth0.proxy_delay = 79
net.ipv4.neigh.eth0.anycast_delay = 99
net.ipv4.neigh.eth0.proxy_qlen = 64
net.ipv4.neigh.eth0.unres_qlen = 3
net.ipv4.neigh.eth0.gc_stale_time = 60
net.ipv4.neigh.eth0.delay_first_probe_time = 5
net.ipv4.neigh.eth0.base_reachable_time = 30
net.ipv4.neigh.eth0.retrans_time = 99
net.ipv4.neigh.eth0.app_solicit = 0
net.ipv4.neigh.eth0.ucast_solicit = 3
net.ipv4.neigh.eth0.mcast_solicit = 3
net.ipv4.neigh.tmm0.base_reachable_time_ms = 30000
net.ipv4.neigh.tmm0.retrans_time_ms = 1000
net.ipv4.neigh.tmm0.locktime = 99
net.ipv4.neigh.tmm0.proxy_delay = 79
net.ipv4.neigh.tmm0.anycast_delay = 99
net.ipv4.neigh.tmm0.proxy_qlen = 64
net.ipv4.neigh.tmm0.unres_qlen = 3
net.ipv4.neigh.tmm0.gc_stale_time = 60
net.ipv4.neigh.tmm0.delay_first_probe_time = 5
net.ipv4.neigh.tmm0.base_reachable_time = 30
net.ipv4.neigh.tmm0.retrans_time = 99
net.ipv4.neigh.tmm0.app_solicit = 0
net.ipv4.neigh.tmm0.ucast_solicit = 3
net.ipv4.neigh.tmm0.mcast_solicit = 3
net.ipv4.neigh.eth0/1.base_reachable_time_ms = 30000
net.ipv4.neigh.eth0/1.retrans_time_ms = 1000
net.ipv4.neigh.eth0/1.locktime = 99
net.ipv4.neigh.eth0/1.proxy_delay = 79
net.ipv4.neigh.eth0/1.anycast_delay = 99
net.ipv4.neigh.eth0/1.proxy_qlen = 64
net.ipv4.neigh.eth0/1.unres_qlen = 3
net.ipv4.neigh.eth0/1.gc_stale_time = 60
net.ipv4.neigh.eth0/1.delay_first_probe_time = 5
net.ipv4.neigh.eth0/1.base_reachable_time = 30
net.ipv4.neigh.eth0/1.retrans_time = 99
net.ipv4.neigh.eth0/1.app_solicit = 0
net.ipv4.neigh.eth0/1.ucast_solicit = 3
net.ipv4.neigh.eth0/1.mcast_solicit = 3
net.ipv4.neigh.lo.base_reachable_time_ms = 30000
net.ipv4.neigh.lo.retrans_time_ms = 1000
net.ipv4.neigh.lo.locktime = 99
net.ipv4.neigh.lo.proxy_delay = 79
net.ipv4.neigh.lo.anycast_delay = 99
net.ipv4.neigh.lo.proxy_qlen = 64
net.ipv4.neigh.lo.unres_qlen = 3
net.ipv4.neigh.lo.gc_stale_time = 60
net.ipv4.neigh.lo.delay_first_probe_time = 5
net.ipv4.neigh.lo.base_reachable_time = 30
net.ipv4.neigh.lo.retrans_time = 99
net.ipv4.neigh.lo.app_solicit = 0
net.ipv4.neigh.lo.ucast_solicit = 3
net.ipv4.neigh.lo.mcast_solicit = 3
net.ipv4.neigh.default.base_reachable_time_ms = 30000
net.ipv4.neigh.default.retrans_time_ms = 1000
net.ipv4.neigh.default.gc_thresh3 = 8192
net.ipv4.neigh.default.gc_thresh2 = 4096
net.ipv4.neigh.default.gc_thresh1 = 128
net.ipv4.neigh.default.gc_interval = 30
net.ipv4.neigh.default.locktime = 99
net.ipv4.neigh.default.proxy_delay = 79
net.ipv4.neigh.default.anycast_delay = 99
net.ipv4.neigh.default.proxy_qlen = 64
net.ipv4.neigh.default.unres_qlen = 3
net.ipv4.neigh.default.gc_stale_time = 60
net.ipv4.neigh.default.delay_first_probe_time = 5
net.ipv4.neigh.default.base_reachable_time = 30
net.ipv4.neigh.default.retrans_time = 99
net.ipv4.neigh.default.app_solicit = 0
net.ipv4.neigh.default.ucast_solicit = 3
net.ipv4.neigh.default.mcast_solicit = 3
net.ipv4.udp_wmem_min = 4096
net.ipv4.udp_rmem_min = 4096
net.ipv4.udp_mem = 774816       1033088 1549632
net.ipv4.tcp_slow_start_after_idle = 1
net.ipv4.tcp_dma_copybreak = 4096
net.ipv4.tcp_workaround_signed_windows = 0
net.ipv4.tcp_base_mss = 512
net.ipv4.tcp_mtu_probing = 0
net.ipv4.tcp_abc = 0
net.ipv4.tcp_congestion_control = bic
net.ipv4.tcp_tso_win_divisor = 3
net.ipv4.tcp_moderate_rcvbuf = 1
net.ipv4.tcp_no_metrics_save = 0
net.ipv4.ipfrag_max_dist = 64
net.ipv4.ipfrag_secret_interval = 600
net.ipv4.tcp_low_latency = 0
net.ipv4.tcp_frto = 0
net.ipv4.tcp_tw_reuse = 0
net.ipv4.icmp_ratemask = 6168
net.ipv4.icmp_ratelimit = 1000
net.ipv4.tcp_adv_win_scale = 2
net.ipv4.tcp_app_win = 31
net.ipv4.tcp_rmem = 4096        87380   4194304
net.ipv4.tcp_wmem = 4096        16384   4194304
net.ipv4.tcp_mem = 196608       262144  393216
net.ipv4.tcp_dsack = 1
net.ipv4.tcp_ecn = 0
net.ipv4.tcp_reordering = 3
net.ipv4.tcp_fack = 1
net.ipv4.tcp_orphan_retries = 0
net.ipv4.inet_peer_gc_maxtime = 120
net.ipv4.inet_peer_gc_mintime = 10
net.ipv4.inet_peer_maxttl = 600
net.ipv4.inet_peer_minttl = 120
net.ipv4.inet_peer_threshold = 65664
net.ipv4.igmp_max_msf = 10
net.ipv4.igmp_max_memberships = 20
net.ipv4.route.rt_cache_rebuild_count = 4
net.ipv4.route.secret_interval = 600
net.ipv4.route.min_adv_mss = 256
net.ipv4.route.min_pmtu = 552
net.ipv4.route.mtu_expires = 600
net.ipv4.route.gc_elasticity = 8
net.ipv4.route.error_burst = 5000
net.ipv4.route.error_cost = 1000
net.ipv4.route.redirect_silence = 20480
net.ipv4.route.redirect_number = 9
net.ipv4.route.redirect_load = 20
net.ipv4.route.gc_interval = 60
net.ipv4.route.gc_timeout = 300
net.ipv4.route.gc_min_interval_ms = 500
net.ipv4.route.gc_min_interval = 0
net.ipv4.route.max_size = 4194304
net.ipv4.route.gc_thresh = 262144
net.ipv4.route.max_delay = 10
net.ipv4.route.min_delay = 2
net.ipv4.icmp_errors_use_inbound_ifaddr = 0
net.ipv4.icmp_ignore_bogus_error_responses = 1
net.ipv4.icmp_echo_ignore_broadcasts = 1
net.ipv4.icmp_echo_ignore_all = 0
net.ipv4.ip_local_port_range = 32768    61000
net.ipv4.tcp_max_syn_backlog = 1024
net.ipv4.tcp_rfc1337 = 0
net.ipv4.tcp_stdurg = 0
net.ipv4.tcp_abort_on_overflow = 0
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_fin_timeout = 60
net.ipv4.tcp_retries2 = 15
net.ipv4.tcp_retries1 = 3
net.ipv4.tcp_keepalive_intvl = 75
net.ipv4.tcp_keepalive_probes = 9
net.ipv4.tcp_keepalive_time = 7200
net.ipv4.ipfrag_time = 30
net.ipv4.ip_dynaddr = 0
net.ipv4.ipfrag_low_thresh = 196608
net.ipv4.ipfrag_high_thresh = 262144
net.ipv4.tcp_max_tw_buckets = 180000
net.ipv4.tcp_max_orphans = 65536
net.ipv4.tcp_synack_retries = 5
net.ipv4.tcp_syn_retries = 5
net.ipv4.ip_nonlocal_bind = 0
net.ipv4.ip_no_pmtu_disc = 0
net.ipv4.ip_default_ttl = 64
net.ipv4.ip_forward = 0
net.ipv4.tcp_retrans_collapse = 1
net.ipv4.tcp_sack = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_timestamps = 1
net.core.netdev_budget = 300
net.core.somaxconn = 128
net.core.xfrm_larval_drop = 0
net.core.xfrm_acq_expires = 30
net.core.xfrm_aevent_rseqth = 2
net.core.xfrm_aevent_etime = 10
net.core.optmem_max = 20480
net.core.message_burst = 10
net.core.message_cost = 5
net.core.netdev_max_backlog = 1000
net.core.dev_weight = 64
net.core.rmem_default = 129024
net.core.wmem_default = 129024
net.core.rmem_max = 1048576
net.core.wmem_max = 1048576
vm.max_writeback_pages = 1024
vm.flush_mmap_pages = 1
vm.pagecache = 100
vm.min_slab_ratio = 5
vm.min_unmapped_ratio = 1
vm.zone_reclaim_mode = 0
vm.swap_token_timeout = 300     0
vm.legacy_va_layout = 0
vm.vfs_cache_pressure = 100
vm.block_dump = 0
vm.laptop_mode = 0
vm.max_map_count = 65536
vm.percpu_pagelist_fraction = 0
vm.min_free_kbytes = 11499
vm.drop_caches = 0
vm.lowmem_reserve_ratio = 256   256     32
vm.hugetlb_shm_group = 0
vm.nr_hugepages = 2823
vm.balance_factor = 4
vm.swappiness = 10
vm.nr_pdflush_threads = 2
vm.dirty_expire_centisecs = 2999
vm.dirty_writeback_centisecs = 499
vm.mmap_min_addr = 4096
vm.dirty_ratio = 40
vm.dirty_background_ratio = 10
vm.page-cluster = 3
vm.overcommit_ratio = 50
vm.panic_on_oom = 0
vm.overcommit_memory = 0
kernel.vsyscall64 = 1
kernel.max_lock_depth = 1024
kernel.compat-log = 1
kernel.softlockup_panic = 0
kernel.softlockup_thresh = 10
kernel.acpi_video_flags = 0
kernel.randomize_va_space = 1
kernel.bootloader_type = 113
kernel.panic_on_unrecovered_nmi = 0
kernel.unknown_nmi_panic = 0
kernel.ngroups_max = 65536
kernel.printk_ratelimit_burst = 10
kernel.printk_ratelimit = 5
kernel.panic_on_oops = 1
kernel.pid_max = 32768
kernel.overflowgid = 65534
kernel.overflowuid = 65534
kernel.pty.nr = 1
kernel.pty.max = 4096
kernel.random.uuid = 65457e61-ed2a-4958-ba49-2a6baa829152
kernel.random.boot_id = d86384bf-95e8-4d57-bbaf-261d4c8b5650
kernel.random.write_wakeup_threshold = 128
kernel.random.read_wakeup_threshold = 64
kernel.random.entropy_avail = 150
kernel.random.poolsize = 4096
kernel.threads-max = 135168
kernel.cad_pid = 1
kernel.sercons_esc = -1
kernel.sysrq = 0
kernel.sem = 250        32000   32      128
kernel.msgmnb = 65536
kernel.msgmni = 16
kernel.msgmax = 65536
kernel.shmmni = 4096
kernel.shmall = 268435456
kernel.shmmax = 4294967295
kernel.acct = 4 2       30
kernel.hotplug =
kernel.modprobe = /sbin/modprobe
kernel.printk = 3       4       1       7
kernel.ctrl-alt-del = 0
kernel.real-root-dev = 0
kernel.cap-bound = -257
kernel.tainted = 67
kernel.core_pattern = /var/core/%a.bld852.0.core
kernel.core_uses_pid = 0
kernel.core_compress_level = 1
kernel.print-fatal-signals = 0
kernel.exec-shield = 1
kernel.panic = 1
kernel.domainname = (none)
kernel.hostname = test.f5.com
kernel.version = #1 SMP Fri Apr 29 14:30:15 PDT 2011
kernel.osrelease = 2.6.18-164.11.1.el5.1.0.f5app
kernel.ostype = Linux
kernel.sched_interactive = 2
fs.mqueue.msgsize_max = 8192
fs.mqueue.msg_max = 10
fs.mqueue.queues_max = 256
fs.quota.warnings = 1
fs.quota.syncs = 64
fs.quota.free_dquots = 0
fs.quota.allocated_dquots = 0
fs.quota.cache_hits = 0
fs.quota.writes = 0
fs.quota.reads = 0
fs.quota.drops = 0
fs.quota.lookups = 0
fs.suid_dumpable = 0
fs.inotify.max_queued_events = 16384
fs.inotify.max_user_watches = 8192
fs.inotify.max_user_instances = 128
fs.aio-max-nr = 65536
fs.aio-nr = 0
fs.lease-break-time = 45
fs.dir-notify-enable = 1
fs.leases-enable = 1
fs.overflowgid = 65534
fs.overflowuid = 65534
fs.dentry-state = 28139 24451   45      0       0       0
fs.file-max = 794906
fs.file-nr = 2560       0       794906
fs.inode-state = 17807  167     0       0       0       0       0
fs.inode-nr = 17807     167
fs.binfmt_misc.status = enabled

```

## Apache

```
[root@F5:Active] config # curl -I http://localhost
HTTP/1.1 200 OK
Date: Thu, 06 Jan 2011 07:36:35 GMT
Server: Apache
Last-Modified: Fri, 09 Apr 2010 03:56:12 GMT
ETag: "1b7d5-cfb-c6af0f00"
Accept-Ranges: bytes
Content-Length: 3323
Content-Type: text/html; charset=ISO-8859-1

```

### httpd.conf

```

[root@F5:Active] config # cat httpd/conf/httpd.conf
#
# THIS IS AN AUTO-GENERATED FILE -- DO NOT EDIT!!!
#
# Use the bigpipe shell utility to make changes to the system configuration.
# For more information, see bigpipe httpd help.
#
# This is the main Apache server configuration file.  It contains the
# configuration directives that give the server its instructions.
# See <URL:http://httpd.apache.org/docs/2.2/> for detailed information.
# In particular, see
# <URL:http://httpd.apache.org/docs/2.2/mod/directives.html>
# for a discussion of each configuration directive.
#
#
# Do NOT simply read the instructions in here without understanding
# what they do.  They're here only as hints or reminders.  If you are unsure
# consult the online docs. You have been warned.
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
# with ServerRoot set to "/etc/httpd" will be interpreted by the
# server as "/etc/httpd/logs/foo.log".
#

### Section 1: Global Environment
#
# The directives in this section affect the overall operation of Apache,
# such as the number of concurrent requests it can handle or where it
# can find its configuration files.
#

#
# Don't give away too much information about all the subcomponents
# we are running.  Comment out this line if you don't mind remote sites
# finding out what major optional modules you are running
ServerTokens Prod

#
# ServerRoot: The top of the directory tree under which the server's
# configuration, error, and log files are kept.
#
# NOTE!  If you intend to place this on an NFS (or otherwise network)
# mounted filesystem then please read the LockFile documentation
# (available at <URL:http://httpd.apache.org/docs/2.2/mod/mpm_common.html#lockfile>);
# you will save yourself a lot of trouble.
#
# Do NOT add a slash at the end of the directory path.
#
ServerRoot "/etc/httpd"

#
# PidFile: The file in which the server should record its process
# identification number when it starts.
#
PidFile run/httpd.pid

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
MaxKeepAliveRequests 100

#
# KeepAliveTimeout: Number of seconds to wait for the next request from the
# same client on the same connection.
#
KeepAliveTimeout 4

##
## Server-Pool Size Regulation (MPM specific)
##

# prefork MPM
# StartServers: number of server processes to start
# MinSpareServers: minimum number of server processes which are kept spare
# MaxSpareServers: maximum number of server processes which are kept spare
# ServerLimit: maximum value for MaxClients for the lifetime of the server
# MaxClients: maximum number of server processes allowed to start
# MaxRequestsPerChild: maximum number of requests a server process serves
<IfModule prefork.c>
StartServers       1
MinSpareServers    1
MaxSpareServers   10
ServerLimit      256
#MaxClients was 256, but to avoid swapping we default it to 10,
#and add ListenBackLog to 246 to be equivalent.
#Requests will just be handled slower.
MaxClients       10
ListenBackLog    246
MaxRequestsPerChild  1000
</IfModule>

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, in addition to the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to
# prevent Apache from glomming onto all bound IP addresses (0.0.0.0)
#
#Listen 12.34.56.78:80
Listen localhost:80

#
# Dynamic Shared Object (DSO) Support
#
# To be able to use the functionality of a module which was built as a DSO you
# have to place corresponding `LoadModule' lines at this location so the
# directives contained in it are actually available _before_ they are used.
# Statically compiled modules (those listed by `httpd -l') do not need
# to be loaded here.
#
# Example:
# LoadModule foo_module modules/mod_foo.so
#
<IfDefine TrafficShield>
    LoadModule php5_module modules/libphp5.so
</IfDefine>
<IfDefine SAM>
    <IfDefine !TrafficShield>
        LoadModule php5_module modules/libphp5.so
    </IfDefine>
</IfDefine>
LoadModule auth_f5_auth_token_module modules/mod_auth_f5_auth_token.so
LoadModule f5_auth_cookie_module modules/mod_f5_auth_cookie.so
#LoadModule auth_basic_module modules/mod_auth_basic.so
#LoadModule auth_digest_module modules/mod_auth_digest.so
LoadModule authn_file_module modules/mod_authn_file.so
LoadModule authn_alias_module modules/mod_authn_alias.so
LoadModule authn_anon_module modules/mod_authn_anon.so
#LoadModule authn_dbm_module modules/mod_authn_dbm.so
LoadModule authn_default_module modules/mod_authn_default.so
LoadModule authz_host_module modules/mod_authz_host.so
LoadModule authz_user_module modules/mod_authz_user.so
#LoadModule authz_owner_module modules/mod_authz_owner.so
#LoadModule authz_groupfile_module modules/mod_authz_groupfile.so
#LoadModule authz_dbm_module modules/mod_authz_dbm.so
LoadModule authz_default_module modules/mod_authz_default.so
#LoadModule ldap_module modules/mod_ldap.so
#LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
LoadModule include_module modules/mod_include.so
LoadModule log_config_module modules/mod_log_config.so
LoadModule logio_module modules/mod_logio.so
LoadModule env_module modules/mod_env.so
#LoadModule ext_filter_module modules/mod_ext_filter.so
#LoadModule mime_magic_module modules/mod_mime_magic.so
LoadModule expires_module modules/mod_expires.so
LoadModule deflate_module modules/mod_deflate.so
LoadModule headers_module modules/mod_headers.so
#LoadModule usertrack_module modules/mod_usertrack.so
LoadModule setenvif_module modules/mod_setenvif.so
LoadModule mime_module modules/mod_mime.so
#LoadModule dav_module modules/mod_dav.so
#LoadModule status_module modules/mod_status.so
#LoadModule autoindex_module modules/mod_autoindex.so
#LoadModule info_module modules/mod_info.so
#LoadModule dav_fs_module modules/mod_dav_fs.so
#LoadModule vhost_alias_module modules/mod_vhost_alias.so
LoadModule negotiation_module modules/mod_negotiation.so
LoadModule dir_module modules/mod_dir.so
LoadModule actions_module modules/mod_actions.so
#LoadModule speling_module modules/mod_speling.so
#LoadModule userdir_module modules/mod_userdir.so
LoadModule alias_module modules/mod_alias.so
#LoadModule rewrite_module modules/mod_rewrite.so
LoadModule proxy_module modules/mod_proxy.so
#LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
#LoadModule proxy_ftp_module modules/mod_proxy_ftp.so
#LoadModule proxy_http_module modules/mod_proxy_http.so
#LoadModule proxy_connect_module modules/mod_proxy_connect.so
#LoadModule cache_module modules/mod_cache.so
#LoadModule suexec_module modules/mod_suexec.so
#LoadModule disk_cache_module modules/mod_disk_cache.so
#LoadModule file_cache_module modules/mod_file_cache.so
#LoadModule mem_cache_module modules/mod_mem_cache.so
LoadModule cgi_module modules/mod_cgi.so
#LoadModule version_module modules/mod_version.so

#
# The following modules are not loaded by default:
#
#LoadModule cern_meta_module modules/mod_cern_meta.so
#LoadModule asis_module modules/mod_asis.so

#
# Load config files from the config directory "/etc/httpd/conf.d".
#
Include conf.d/*.conf

#
# ExtendedStatus controls whether Apache will generate "full" status
# information (ExtendedStatus On) or just basic information (ExtendedStatus
# Off) when the "server-status" handler is called. The default is Off.
#
#ExtendedStatus On

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
#  don't use Group #-1 on these systems!
#
User apache
Group apache

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
# ServerAdmin: Your address, where problems with the server should be
# e-mailed.  This address appears on some server-generated pages, such
# as error documents.  e.g. admin@your-domain.com
#
ServerAdmin support@f5.com

#
# ServerName gives the name and port that the server uses to identify itself.
# This can often be determined automatically, but we recommend you specify
# it explicitly to prevent problems during startup.
#
# If this is not set to valid DNS name for your host, server-generated
# redirections will not work.  See also the UseCanonicalName directive.
#
# If your host doesn't have a registered DNS name, enter its IP address here.
# You will have to access it by its address anyway, and this will make
# redirections work in a sensible way.
#
#ServerName www.example.com:80

#
# UseCanonicalName: Determines how Apache constructs self-referencing
# URLs and the SERVER_NAME and SERVER_PORT variables.
# When set "Off", Apache will use the Hostname and Port supplied
# by the client.  When set "On", Apache will use the value of the
# ServerName directive.
#
UseCanonicalName Off

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "/usr/local/www"

#
# Each directory to which Apache has access can be configured with respect
# to which services and features are allowed and/or disabled in that
# directory (and its subdirectories).
#
# First, we configure the "default" to be a very restrictive set of
# features.
#
<Directory />
    Options FollowSymLinks
    AllowOverride None

#
# Controls who can get stuff from this server.
#
    Order Deny,Allow
    Allow from All

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
<Directory "/usr/local/www">

#
# Possible values for the Options directive are "None", "All",
# or any combination of:
#   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
#
# Note that "MultiViews" must be named *explicitly* --- "Options All"
# doesn't give it to you.
#
# The Options directive is both complicated and important.  Please see
# http://httpd.apache.org/docs/2.2/mod/core.html#options
# for more information.
#
    Options FollowSymLinks

#
# AllowOverride controls what directives may be placed in .htaccess files.
# It can be "All", "None", or any combination of the keywords:
#   Options FileInfo AuthConfig Limit
#
    AllowOverride None

#
# Controls who can get stuff from this server.
#
#    Order allow,deny
#    Allow from all

</Directory>

#
# WebAccelerator
#
<IfDefine WebAccelerator>
    <Location /waui>

        AuthType Basic
        AuthName "BIG-IP"
        AuthPAM_Enabled on
        AuthPAM_IdleTimeout 1200
        require valid-user

    Order Deny,Allow
    Allow from All

    </Location>

    <Location "/waui/WEB-INF">
        Order Allow,Deny
        Deny from all
    </Location>
</IfDefine>

<Location /tmui>
    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    # Enable content compression by type, disable for browsers with known issues
    <IfModule mod_deflate.c>
     AddOutputFilterByType DEFLATE text/html text/plain application/x-javascript text/css
     BrowserMatch ^Mozilla/4 gzip-only-text/html
     BrowserMatch ^Mozilla/4\.0[678] no-gzip
     BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
    </IfModule>

    Order Deny,Allow
    Allow from All

</Location>

<Location "/tmui/WEB-INF">
    Order Allow,Deny
    Deny from all
</Location>

#
# set caching instructions for static content in tmui
#
<LocationMatch "/tmui/.*\.(js|css)\??$">
    <IfModule mod_expires.c>
     ExpiresActive on
     ExpiresByType application/x-javascript "access plus 120 seconds"
     ExpiresByType text/css "access plus 120 seconds"
    </IfModule>

    <IfModule mod_headers.c>
     Header append Cache-Control must-revalidate
    </IfModule>
</LocationMatch>

#
# rrd stats graphs - these are in /var/www/... because /usr is
# now read-only, and rrd needs to write these files on the fly.
#
Alias /tmui/tmui/system/stats/images/ "/var/www/tmui/tmui/system/stats/images/"

<Directory "/var/www/tmui/tmui/system/stats/images/">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    Order Deny,Allow
    Allow from All
</Directory>

<Location /docs>

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    Order Deny,Allow
    Allow from All

</Location>

#
# TrafficShield
#
<IfDefine TrafficShield>
    AddType     application/x-httpd-php  .php

    # TrafficShield uses .htaccess overrides
    <Directory /usr/local/www/dms>
        AllowOverride All

    Order Deny,Allow
    Allow from All
    </Directory>

    <Location /dms>
        AuthType Basic
        AuthName "BIG-IP"
        AuthPAM_Enabled on
        AuthPAM_IdleTimeout 1200
        require valid-user

    Order Deny,Allow
    Allow from All

    </Location>
    alias /crossdomain.xml "/ts/dms/crossdomain.xml"
</IfDefine>

#
# SAM
#
<IfDefine SAM>
    AddType     application/x-httpd-php  .php

    # SAM uses .htaccess overrides
    <Directory /usr/local/www/sam>
        AllowOverride All

    Order Deny,Allow
    Allow from All
    </Directory>

    # Special hack for Group Policy templates
    <Location /sam/admin/tmui/accesscontrol/grouppolicy/gpo/>
        AddDefaultCharset UTF-16
        AuthType Basic
        AuthName "BIG-IP"
        AuthPAM_Enabled on
        AuthPAM_IdleTimeout 1200
        require valid-user

    Order Deny,Allow
    Allow from All
    </Location>

    <Location /sam>
        AuthType Basic
        AuthName "BIG-IP"
        AuthPAM_Enabled on
        AuthPAM_IdleTimeout 1200
        require valid-user

    Order Deny,Allow
    Allow from All
    </Location>
</IfDefine>

#
# Disable autoindex for the root directory.
#
<LocationMatch "^/$>
    Options -Indexes
</LocationMatch>

#
# UserDir: The name of the directory that is appended onto a user's home
# directory if a ~user request is received.
#
# The path to the end user account 'public_html' directory must be
# accessible to the webserver userid.  This usually means that ~userid
# must have permissions of 711, ~userid/public_html must have permissions
# of 755, and documents contained therein must be world-readable.
# Otherwise, the client will only receive a "403 Forbidden" message.
#
# See also: http://httpd.apache.org/docs/misc/FAQ.html#forbidden
#
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    UserDir disable

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disable" line above, and uncomment
    # the following line instead:
    #
    #UserDir public_html

</IfModule>

#
# Control access to UserDir directories.  The following is an example
# for a site where these directories are restricted to read-only.
#
#<Directory /home/*/public_html>
#    AllowOverride FileInfo AuthConfig Limit
#    Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
#    <Limit GET POST OPTIONS>
#        Order allow,deny
#        Allow from all
#    </Limit>
#    <LimitExcept GET POST OPTIONS>
#        Order deny,allow
#        Deny from all
#    </LimitExcept>
#</Directory>

#
# DirectoryIndex: sets the file that Apache will serve if a directory
# is requested.
#
# The index.html.var file (a type-map) is used to deliver content-
# negotiated documents.  The MultiViews Option can be used for the
# same purpose, but it is much slower.
#
DirectoryIndex index.html index.html.var

#
# AccessFileName: The name of the file to look for in each directory
# for additional configuration directives.  See also the AllowOverride
# directive.
#
AccessFileName .htaccess

#
# The following lines prevent .htaccess and .htpasswd files from being
# viewed by Web clients.
#
<Files ~ "^\.ht">
    Order allow,deny
    Deny from all
</Files>

#
# TypesConfig describes where the mime.types file (or equivalent) is
# to be found.
#
TypesConfig /etc/mime.types

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
#
<IfModule mod_mime_magic.c>
#   MIMEMagicFile /usr/share/magic.mime
    MIMEMagicFile conf/magic
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
# EnableMMAP: Control whether memory-mapping is used to deliver
# files (assuming that the underlying OS supports it).
# The default is on; turn this off if you serve from NFS-mounted
# filesystems.  On some systems, turning it off (regardless of
# filesystem) can improve performance; for details, please see
# http://httpd.apache.org/docs/2.2/mod/core.html#enablemmap
#
#EnableMMAP off

#
# EnableSendfile: Control whether the sendfile kernel support is
# used to deliver files (assuming that the OS supports it).
# The default is on; turn this off if you serve from NFS-mounted
# filesystems.  Please see
# http://httpd.apache.org/docs/2.2/mod/core.html#enablesendfile
#
#EnableSendfile off

#
# ErrorLog: The location of the error log file.
# If you do not specify an ErrorLog directive within a <VirtualHost>
# container, error messages relating to that virtual host will be
# logged here.  If you *do* define an error logfile for a <VirtualHost>
# container, that host's errors will be logged there and not here.
#
ErrorLog syslog:local6

#
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel warn

#
# The following directives define some format nicknames for use with
# a CustomLog directive (see below).
#
#LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
#LogFormat "%h %l %u %t \"%r\" %>s %b" common
#LogFormat "%{Referer}i -> %U" referer
#LogFormat "%{User-agent}i" agent

# "combinedio" includes actual counts of actual bytes received (%I) and sent (%O); this
# requires the mod_logio module to be loaded.
#LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio

LogFormat "[acc] %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" acc_combined
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
#CustomLog logs/access_log common

#
# If you would like to have separate agent and referer logfiles, uncomment
# the following directives.
#
#CustomLog logs/referer_log referer
#CustomLog logs/agent_log agent

#
# For a single logfile with access, agent, and referer information
# (Combined Logfile Format), use the following directive:
#
#CustomLog logs/access_log combined

CustomLog "/var/run/httpd.pipe" acc_combined

#
# Optionally add a line containing the server version and virtual host
# name to server-generated pages (internal error documents, FTP directory
# listings, mod_status and mod_info output etc., but not CGI generated
# documents or custom error documents).
# Set to "EMail" to also include a mailto: link to the ServerAdmin.
# Set to one of:  On | Off | EMail
#
ServerSignature On

#
# Aliases: Add here as many aliases as you need (with no limit). The format is
# Alias fakename realname
#
# Note that if you include a trailing / on fakename then the server will
# require it to be present in the URL.  So "/icons" isn't aliased in this
# example, only "/icons/".  If the fakename is slash-terminated, then the
# realname must also be slash terminated, and if the fakename omits the
# trailing slash, the realname must also omit it.
#
# We include the /icons/ alias for FancyIndexed directory listings.  If you
# do not use FancyIndexing, you may comment this out.
#
#Alias /icons/ "/var/www/icons/"

#<Directory "/var/www/icons">
#    Options Indexes MultiViews
#    AllowOverride None
#    Order allow,deny
#    Allow from all
#</Directory>

#
# WebDAV module configuration section.
#
#<IfModule mod_dav_fs.c>
#    # Location of the WebDAV lock database.
#    DAVLockDB /var/lib/dav/lockdb
#</IfModule>

#
# ScriptAlias: This controls which directories contain server scripts.
# ScriptAliases are essentially the same as Aliases, except that
# documents in the realname directory are treated as applications and
# run by the server when requested rather than as documents sent to the client.
# The same rules about trailing "/" apply to ScriptAlias directives as to
# Alias.
#
ScriptAlias /cgi-bin/ "/usr/local/www/cgi-bin/"

<IfModule mod_cgid.c>
#
# Additional to mod_cgid.c settings, mod_cgid has Scriptsock <path>
# for setting UNIX socket for communicating with cgid.
#
Scriptsock            run/httpd.cgid
</IfModule>

#
# "/var/www/cgi-bin" should be changed to whatever your ScriptAliased
# CGI directory exists, if you have that configured.
#
<Directory "/usr/local/www/cgi-bin">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    Order Deny,Allow
    Allow from All

</Directory>

<Directory "/usr/local/www/html">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    Order Deny,Allow
    Allow from All

</Directory>

#
# Directory for EM update CGI application.
#
# For the moment, we leave this open to all clients.
#

ScriptAlias /emupdate/ "/usr/local/www/emupdate/"

<Directory "/usr/local/www/emupdate">
    AllowOverride None
    Options None
</Directory>

Alias /em_export/ "/shared/em/export/"
<Directory "/shared/em/export/">
    AllowOverride None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user
</Directory>

#
# Unit certificate -- leave this open to all clients
# Eventually we'll just use the webserver certificate for this but its
# key is not currently protected enough
#

Alias /unitkeys/ "/var/www/unitkeys/"

<Directory "/var/www/unitkeys/">
    AllowOverride None
    Options None

    Order Deny,Allow
    Allow from All
</Directory>

#
# BEGIN CGI to service dashboard
#
<Directory "/usr/local/www/rtstats">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    require valid-user

    Order Deny,Allow
    Allow from All
</Directory>

<Directory "/usr/local/www/rrdstats">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    require valid-user

    Order Deny,Allow
    Allow from All
</Directory>

<Directory "/usr/local/www/mcpq">
    AllowOverride None
    Options None

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    require valid-user

    Order Deny,Allow
    Allow from All
</Directory>

ScriptAlias /wdiag/ "/usr/local/www/wdiag/"
<Directory "/usr/local/www/wdiag">
    AllowOverride None
    Options None

    <IfModule mod_deflate.c>
        # Enable content compression by type, disable for browsers with known issues
        AddOutputFilterByType DEFLATE text/html text/plain application/x-javascript text/css
        BrowserMatch ^Mozilla/4 gzip-only-text/html
        BrowserMatch ^Mozilla/4\.0[678] no-gzip
        BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
    </IfModule>

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    require valid-user

    Order Deny,Allow
    Allow from All
</Directory>

# END CGI to service dashboard

#
# Redirect allows you to tell clients about documents which used to exist in
# your server's namespace, but do not anymore. This allows you to tell the
# clients where to look for the relocated document.
# Example:
# Redirect permanent /foo http://www.example.com/bar

#
# Directives controlling the display of server-generated directory listings.
#

#
# IndexOptions: Controls the appearance of server-generated directory
# listings.
#
#IndexOptions FancyIndexing VersionSort NameWidth=* HTMLTable

#
# AddIcon* directives tell the server which icon to show for different
# files or filename extensions.  These are only displayed for
# FancyIndexed directories.
#
#AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip

#AddIconByType (TXT,/icons/text.gif) text/*
#AddIconByType (IMG,/icons/image2.gif) image/*
#AddIconByType (SND,/icons/sound2.gif) audio/*
#AddIconByType (VID,/icons/movie.gif) video/*

#AddIcon /icons/binary.gif .bin .exe
#AddIcon /icons/binhex.gif .hqx
#AddIcon /icons/tar.gif .tar
#AddIcon /icons/world2.gif .wrl .wrl.gz .vrml .vrm .iv
#AddIcon /icons/compressed.gif .Z .z .tgz .gz .zip
#AddIcon /icons/a.gif .ps .ai .eps
#AddIcon /icons/layout.gif .html .shtml .htm .pdf
#AddIcon /icons/text.gif .txt
#AddIcon /icons/c.gif .c
#AddIcon /icons/p.gif .pl .py
#AddIcon /icons/f.gif .for
#AddIcon /icons/dvi.gif .dvi
#AddIcon /icons/uuencoded.gif .uu
#AddIcon /icons/script.gif .conf .sh .shar .csh .ksh .tcl
#AddIcon /icons/tex.gif .tex
#AddIcon /icons/bomb.gif core

#AddIcon /icons/back.gif ..
#AddIcon /icons/hand.right.gif README
#AddIcon /icons/folder.gif ^^DIRECTORY^^
#AddIcon /icons/blank.gif ^^BLANKICON^^

#
# DefaultIcon is which icon to show for files which do not have an icon
# explicitly set.
#
#DefaultIcon /icons/unknown.gif

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
#ReadmeName README.html
#HeaderName HEADER.html

#
# IndexIgnore is a set of filenames which directory indexing should ignore
# and not include in the listing.  Shell-style wildcarding is permitted.
#
#IndexIgnore .??* *~ *# HEADER* README* RCS CVS *,v *,t

#
# DefaultLanguage and AddLanguage allows you to specify the language of
# a document. You can then use content negotiation to give a browser a
# file in a language the user can understand.
#
# Specify a default language. This means that all data
# going out without a specific language tag (see below) will
# be marked with this one. You probably do NOT want to set
# this unless you are sure it is correct for all cases.
#
# * It is generally better to not mark a page as
# * being a certain language than marking it with the wrong
# * language!
#
# DefaultLanguage nl
#
# Note 1: The suffix does not have to be the same as the language
# keyword --- those with documents in Polish (whose net-standard
# language code is pl) may wish to use "AddLanguage pl .po" to
# avoid the ambiguity with the common suffix for perl scripts.
#
# Note 2: The example entries below illustrate that in some cases
# the two character 'Language' abbreviation is not identical to
# the two character 'Country' code for its country,
# E.g. 'Danmark/dk' versus 'Danish/da'.
#
# Note 3: In the case of 'ltz' we violate the RFC by using a three char
# specifier. There is 'work in progress' to fix this and get
# the reference data for rfc1766 cleaned up.
#
# Catalan (ca) - Croatian (hr) - Czech (cs) - Danish (da) - Dutch (nl)
# English (en) - Esperanto (eo) - Estonian (et) - French (fr) - German (de)
# Greek-Modern (el) - Hebrew (he) - Italian (it) - Japanese (ja)
# Korean (ko) - Luxembourgeois* (ltz) - Norwegian Nynorsk (nn)
# Norwegian (no) - Polish (pl) - Portugese (pt)
# Brazilian Portuguese (pt-BR) - Russian (ru) - Swedish (sv)
# Simplified Chinese (zh-CN) - Spanish (es) - Traditional Chinese (zh-TW)
#
AddLanguage ca .ca
AddLanguage cs .cz .cs
AddLanguage da .dk
AddLanguage de .de
AddLanguage el .el
AddLanguage en .en
AddLanguage eo .eo
AddLanguage es .es
AddLanguage et .et
AddLanguage fr .fr
AddLanguage he .he
AddLanguage hr .hr
AddLanguage it .it
AddLanguage ja .ja
AddLanguage ko .ko
AddLanguage ltz .ltz
AddLanguage nl .nl
AddLanguage nn .nn
AddLanguage no .no
AddLanguage pl .po
AddLanguage pt .pt
AddLanguage pt-BR .pt-br
AddLanguage ru .ru
AddLanguage sv .sv
AddLanguage zh-CN .zh-cn
AddLanguage zh-TW .zh-tw

#
# LanguagePriority allows you to give precedence to some languages
# in case of a tie during content negotiation.
#
# Just list the languages in decreasing order of preference. We have
# more or less alphabetized them here. You probably want to change this.
#
LanguagePriority en ca cs da de el eo es et fr he hr it ja ko ltz nl nn no pl pt pt-BR ru sv zh-CN zh-TW

#
# ForceLanguagePriority allows you to serve a result page rather than
# MULTIPLE CHOICES (Prefer) [in case of a tie] or NOT ACCEPTABLE (Fallback)
# [in case no accepted languages matched the available variants]
#
ForceLanguagePriority Prefer Fallback

#
# Specify a default charset for all content served; this enables
# interpretation of all content as UTF-8 by default.  To use the
# default browser choice (ISO-8859-1), or to allow the META tags
# in HTML content to override this choice, comment out this
# directive:
#
#AddDefaultCharset UTF-8
AddDefaultCharset ISO-8859-1

#
# AddType allows you to add to or override the MIME configuration
# file mime.types for specific file types.
#
#AddType application/x-tar .tgz

#
# AddEncoding allows you to have certain browsers uncompress
# information on the fly. Note: Not all browsers support this.
# Despite the name similarity, the following Add* directives have nothing
# to do with the FancyIndexing customization directives above.
#
AddEncoding x-compress .Z
AddEncoding x-gzip .gz .tgz

# If the AddEncoding directives above are commented-out, then you
# probably should define those extensions to indicate media types:
#
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz

#
# AddHandler allows you to map certain file extensions to "handlers":
# actions unrelated to filetype. These can be either built into the server
# or added with the Action directive (see below)
#
# To use CGI scripts outside of ScriptAliased directories:
# (You will also need to add "ExecCGI" to the "Options" directive.)
#
#AddHandler cgi-script .cgi

#
# For files that include their own HTTP headers:
#
#AddHandler send-as-is asis

#
# For type maps (negotiated resources):
# (This is enabled by default to allow the Apache "It Worked" page
#  to be distributed in multiple languages.)
#
AddHandler type-map var

#
# Filters allow you to process content before it is sent to the client.
#
# To parse .shtml files for server-side includes (SSI):
# (You will also need to add "Includes" to the "Options" directive.)
#
AddType text/html .shtml
AddOutputFilter INCLUDES .shtml

#
# Action lets you define media types that will execute a script whenever
# a matching file is called. This eliminates the need for repeated URL
# pathnames for oft-used CGI file processors.
# Format: Action media/type /cgi-script/location
# Format: Action handler-name /cgi-script/location
#

#
# Customizable error responses come in three flavors:
# 1) plain text 2) local redirects 3) external redirects
#
# Some examples:
#ErrorDocument 500 "The server made a boo boo."
#ErrorDocument 404 /missing.html
#ErrorDocument 404 "/cgi-bin/missing_handler.pl"
#ErrorDocument 402 http://www.example.com/subscription_info.html
#
ErrorDocument 403 /error/forbidden.html
ErrorDocument 503 /error/tomcat_unavailable.html

#
# Putting this all together, we can internationalize error responses.
#
# We use Alias to redirect any /error/HTTP_<error>.html.var response to
# our collection of by-error message multi-language collections.  We use
# includes to substitute the appropriate text.
#
# You can modify the messages' appearance without changing any of the
# default HTTP_<error>.html.var files by adding the line:
#
#   Alias /error/include/ "/your/include/path/"
#
# which allows you to create your own set of files by starting with the
# /var/www/error/include/ files and
# copying them to /your/include/path/, even on a per-VirtualHost basis.
#

Alias /error/ "/usr/local/www/error/"

<IfModule mod_negotiation.c>
<IfModule mod_include.c>
    <Directory "/usr/local/www/error">
        AllowOverride None
        Options IncludesNoExec
        AddOutputFilter Includes html
        AddHandler type-map var
        Order allow,deny
        Allow from all
        LanguagePriority en es de fr
        ForceLanguagePriority Prefer Fallback
    </Directory>

    ErrorDocument 400 /error/HTTP_BAD_REQUEST.html.var
    ErrorDocument 401 /error/HTTP_UNAUTHORIZED.html.var
#    ErrorDocument 403 /error/HTTP_FORBIDDEN.html.var
    ErrorDocument 404 /error/HTTP_NOT_FOUND.html.var
    ErrorDocument 405 /error/HTTP_METHOD_NOT_ALLOWED.html.var
    ErrorDocument 408 /error/HTTP_REQUEST_TIME_OUT.html.var
    ErrorDocument 410 /error/HTTP_GONE.html.var
    ErrorDocument 411 /error/HTTP_LENGTH_REQUIRED.html.var
    ErrorDocument 412 /error/HTTP_PRECONDITION_FAILED.html.var
    ErrorDocument 413 /error/HTTP_REQUEST_ENTITY_TOO_LARGE.html.var
    ErrorDocument 414 /error/HTTP_REQUEST_URI_TOO_LARGE.html.var
    ErrorDocument 415 /error/HTTP_UNSUPPORTED_MEDIA_TYPE.html.var
#    ErrorDocument 500 /error/HTTP_INTERNAL_SERVER_ERROR.html.var
    ErrorDocument 501 /error/HTTP_NOT_IMPLEMENTED.html.var
    ErrorDocument 502 /error/HTTP_BAD_GATEWAY.html.var
#    ErrorDocument 503 /error/HTTP_SERVICE_UNAVAILABLE.html.var
    ErrorDocument 506 /error/HTTP_VARIANT_ALSO_VARIES.html.var

</IfModule>
</IfModule>

#
# The following directives modify normal HTTP response behavior to
# handle known problems with browser implementations.
#
BrowserMatch "Mozilla/2" nokeepalive
BrowserMatch "MSIE 4\.0b2;" nokeepalive downgrade-1.0 force-response-1.0
BrowserMatch "RealPlayer 4\.0" force-response-1.0
BrowserMatch "Java/1\.0" force-response-1.0
BrowserMatch "JDK/1\.0" force-response-1.0

#
# The following directive disables redirects on non-GET requests for
# a directory that does not include the trailing slash.  This fixes a
# problem with Microsoft WebFolders which does not appropriately handle
# redirects for folders with DAV methods.
# Same deal with Apple's DAV filesystem and Gnome VFS support for DAV.
#
BrowserMatch "Microsoft Data Access Internet Publishing Provider" redirect-carefully
BrowserMatch "MS FrontPage" redirect-carefully
BrowserMatch "^WebDrive" redirect-carefully
BrowserMatch "^WebDAVFS/1.[0123]" redirect-carefully
BrowserMatch "^gnome-vfs/1.0" redirect-carefully
BrowserMatch "^XML Spy" redirect-carefully
BrowserMatch "^Dreamweaver-WebDAV-SCM1" redirect-carefully

#
# Allow server status reports generated by mod_status,
# with the URL of http://servername/server-status
# Change the ".example.com" to match your domain to enable.
#
#<Location /server-status>
#    SetHandler server-status
#    Order deny,allow
#    Deny from all
#    Allow from .example.com
#</Location>

#
# Allow remote server configuration reports, with the URL of
#  http://servername/server-info (requires that mod_info.c be loaded).
# Change the ".example.com" to match your domain to enable.
#
#<Location /server-info>
#    SetHandler server-info
#    Order deny,allow
#    Deny from all
#    Allow from .example.com
#</Location>

#
# Proxy Server directives. Uncomment the following lines to
# enable the proxy server:
#
#<IfModule mod_proxy.c>
#ProxyRequests On
#
#<Proxy *>
#    Order deny,allow
#    Deny from all
#    Allow from .example.com
#</Proxy>

#
# Enable/disable the handling of HTTP/1.1 "Via:" headers.
# ("Full" adds the server version; "Block" removes all outgoing Via: headers)
# Set to one of: Off | On | Full | Block
#
#ProxyVia On

#
# To enable a cache of proxied content, uncomment the following lines.
# See http://httpd.apache.org/docs/2.2/mod/mod_cache.html for more details.
#
#<IfModule mod_disk_cache.c>
#   CacheEnable disk /
#   CacheRoot "/var/cache/mod_proxy"
#</IfModule>
#

#</IfModule>
# End of proxy directives.

### Section 3: Virtual Hosts
#
# VirtualHost: If you want to maintain multiple domains/hostnames on your
# machine you can setup VirtualHost containers for them. Most configurations
# use only name-based virtual hosts so the server doesn't need to worry about
# IP addresses. This is indicated by the asterisks in the directives below.
#
# Please see the documentation at
# <URL:http://httpd.apache.org/docs/2.2/vhosts/>
# for further details before you try to setup virtual hosts.
#
# You may use the command line option '-S' to verify your virtual host
# configuration.

#
# Use name-based virtual hosting.
#
#NameVirtualHost *:80
#
# NOTE: NameVirtualHost cannot be used without a port specifier
# (e.g. :80) if mod_ssl is being used, due to the nature of the
# SSL protocol.
#

#
# VirtualHost example:
# Almost any Apache directive may go into a VirtualHost container.
# The first VirtualHost section is used for requests without a known
# server name.
#
#<VirtualHost *:80>
#    ServerAdmin webmaster@dummy-host.example.com
#    DocumentRoot /www/docs/dummy-host.example.com
#    ServerName dummy-host.example.com
#    ErrorLog logs/dummy-host.example.com-error_log
#    CustomLog logs/dummy-host.example.com-access_log common
#</VirtualHost>

LoadModule fastcgi_module /usr/lib/httpd/modules/mod_fastcgi.so
<IfModule mod_fastcgi.c>
    AddHandler fastcgi-script .fcgi
    FastCgiIpcDir /var/run/fcgi
    FastCgiServer /usr/local/www/iControl/iControlPortal.cgi -processes 1 -socket iControlPortal -idle-timeout 300
    <Location /iControl/iControlPortal.cgi>
        SetHandler fastcgi-script
    </Location>

    FastCgiServer /usr/local/www/rtstats/rtstats -processes 1 -socket rtstats -idle-timeout 300
    <Location /rtstats/rtstats>
        SetHandler fastcgi-script
        <IfModule mod_deflate.c>
            # Enable content compression, disable for browsers with known issues
            SetOutputFilter DEFLATE
            BrowserMatch ^Mozilla/4 gzip-only-text/html
            BrowserMatch ^Mozilla/4\.0[678] no-gzip
            BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
        </IfModule>
    </Location>

    FastCgiServer /usr/local/www/rrdstats/rrdstats -processes 1 -socket rrdstats -idle-timeout 300
    <Location /rrdstats/rrdstats>
        SetHandler fastcgi-script
        <IfModule mod_deflate.c>
            # Enable content compression, disable for browsers with known issues
            SetOutputFilter DEFLATE
            BrowserMatch ^Mozilla/4 gzip-only-text/html
            BrowserMatch ^Mozilla/4\.0[678] no-gzip
            BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
        </IfModule>
    </Location>

    FastCgiServer /usr/local/www/mcpq/mcpq -processes 1 -socket mcpq -idle-timeout 300
    <Location /mcpq/mcpq>
        SetHandler fastcgi-script
        <IfModule mod_deflate.c>
            # Enable content compression, disable for browsers with known issues
            SetOutputFilter DEFLATE
            BrowserMatch ^Mozilla/4 gzip-only-text/html
            BrowserMatch ^Mozilla/4\.0[678] no-gzip
            BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
        </IfModule>
    </Location>

    <IfDefine EmServer>
        FastCgiServer /usr/local/www/emupdate/getfile
        <Location /emupdate/getfile>
            SetHandler fastcgi-script
        </Location>
        FastCgiServer /usr/local/www/emupdate/subscription
        <Location /emupdate/subscription>
            SetHandler fastcgi-script
        </Location>
    </IfDefine>
</IfModule>

#
# iControl may be accessed from the 127 network without authentication.
# This allows clients on the same system to call iControl without authentication.
#
<Directory "/usr/local/www/iControl">
    # Satisfy any means that a connection may satisfy either the address access
    # restriction or the authentication restriction in order to be authorized to
    # access this directory.
    Satisfy any

    # Access is restricted to traffic from 127.*.*.*
    Order Deny,Allow
    Deny from all
    Allow from 127

    # This is an exact copy of the authentication settings of the document root.
    # If a connection is attempted from anywhere but 127.*.*.*, then it will have
    # to be authenticated.
    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    require valid-user

</Directory>

# >BEGIN CR38263
#this stops screen flicker in IE
BrowserMatch "MSIE" brokenvary=1
BrowserMatch "Mozilla/4.[0-9]{2}" brokenvary=1
BrowserMatch "Opera" !brokenvary
SetEnvIf brokenvary 1 force-no-vary

ExpiresActive On
ExpiresByType image/gif A18000
ExpiresByType image/jpeg A18000
ExpiresByType image/png A18000
# <END CR38263

# Used by mod_deflate for TMUI and XUI pages
<IfModule mod_deflate.c>
 DeflateCompressionLevel 1
</IfModule>

```

### xui.conf

```

[root@F5:Active] config # cat httpd/conf.d/xui.conf
#
# THIS IS AN AUTO-GENERATED FILE -- DO NOT EDIT!!!
#
# Use the bigpipe shell utility to make changes to the system configuration.
# For more information, see bigpipe httpd help.
#
# XUI Module

LoadModule xui_module   /usr/lib/httpd/modules/mod_xui.so

# XUI Container
<Location /xui>
    Order Allow,Deny
    Allow from all

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user

    # Enable content compression by type, disable for browsers with known issues
    <IfModule mod_deflate.c>
     AddOutputFilterByType DEFLATE text/html text/plain application/x-javascript text/css
     BrowserMatch ^Mozilla/4 gzip-only-text/html
     BrowserMatch ^Mozilla/4\.0[678] no-gzip
     BrowserMatch \bMSIE !no-gzip !gzip-only-text/html
    </IfModule>
</Location>

# XUI Modules
AliasMatch /modules/([^/]*)/(.*) /usr/local/xui/modules/$1/web/$2
<Directory "/usr/local/xui/modules/*">
    Order allow,deny
    Allow from all

    AuthType Basic
    AuthName "BIG-IP"
    AuthPAM_Enabled on
    AuthPAM_IdleTimeout 1200
    require valid-user
</Directory>

<LocationMatch "/xui/.*\.(js|css)\??$">
    <IfModule mod_expires.c>
     ExpiresActive on
     ExpiresByType application/x-javascript "access plus 120 seconds"
     ExpiresByType text/css "access plus 120 seconds"
    </IfModule>

    <IfModule mod_headers.c>
     Header append Cache-Control must-revalidate
    </IfModule>
</LocationMatch>

```

### rrdtool

```

[root@F5:Active] config # rrdtool
RRDtool 1.2.27  Copyright 1997-2008 by Tobias Oetiker <tobi@oetiker.ch>
               Compiled Apr  8 2010 19:13:02

Usage: rrdtool [options] command command_options

Valid commands: create, update, updatev, graph, dump, restore,
                last, lastupdate, first, info, fetch, tune,
                resize, xport

RRDtool is distributed under the Terms of the GNU General
Public License Version 2\. (www.gnu.org/copyleft/gpl.html)

For more information read the RRD manpages

```

## Tomcat

```

[root@F5:Active] config # cat /etc/tomcat/server.xml
<?xml version='1.0' encoding='utf-8'?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<!-- Note:  A "Server" is not itself a "Container", so you may not
     define subcomponents such as "Valves" at this level.
     Documentation at /docs/config/server.html
 -->
<Server port="8005" shutdown="SHUTDOWN">

  <!--APR library loader. Documentation at /docs/apr.html -->
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <!--Initialize Jasper prior to webapps are loaded. Documentation at /docs/jasper-howto.html -->
  <Listener className="org.apache.catalina.core.JasperListener" />
  <!-- JMX Support for the Tomcat server. Documentation at /docs/non-existent.html -->
  <Listener className="org.apache.catalina.mbeans.ServerLifecycleListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />

  <!-- Global JNDI resources
       Documentation at /docs/jndi-resources-howto.html
  -->
  <GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
              readonly="true" />
  </GlobalNamingResources>

  <!-- A "Service" is a collection of one or more "Connectors" that share
       a single "Container" Note:  A "Service" is not itself a "Container",
       so you may not define subcomponents such as "Valves" at this level.
       Documentation at /docs/config/service.html
   -->
  <Service name="Catalina">

    <!--The connectors can use a shared executor, you can define one or more named thread pools-->
    <!--
    <Executor name="tomcatThreadPool" namePrefix="catalina-exec-"
        maxThreads="150" minSpareThreads="4"/>
    -->

    <!-- A "Connector" represents an endpoint by which requests are received
         and responses are returned. Documentation at :
         Java HTTP Connector: /docs/config/http.html (blocking & non-blocking)
         Java AJP  Connector: /docs/config/ajp.html
         APR (HTTP/AJP) Connector: /docs/apr.html
         Define a non-SSL HTTP/1.1 Connector on port 8080
    -->
    <!--
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    -->
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    -->
    <!-- Define a SSL HTTP/1.1 Connector on port 8443
         This connector uses the JSSE configuration, when using APR, the
         connector should be using the OpenSSL style configuration
         described in the APR documentation -->
    <!--
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
               maxThreads="150" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" />
    -->

    <!-- Define an AJP 1.3 Connector on port 8009 -->
    <Connector port="8009" protocol="AJP/1.3"
               redirectPort="8443"
               enableLookups="true"
               address="127.0.0.1"
               tomcatAuthentication="false" />

    <!-- An Engine represents the entry point (within Catalina) that processes
         every request.  The Engine implementation for Tomcat stand alone
         analyzes the HTTP headers included with the request, and passes them
         on to the appropriate Host (virtual host).
         Documentation at /docs/config/engine.html -->

    <!-- You should set jvmRoute to support load-balancing via AJP ie :
    <Engine name="Catalina" defaultHost="localhost" jvmRoute="jvm1">
    -->
    <Engine name="Catalina" defaultHost="localhost">

      <!--For clustering, please take a look at documentation at:
          /docs/cluster-howto.html  (simple how to)
          /docs/config/cluster.html (reference documentation) -->
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->

      <!-- The request dumper valve dumps useful debugging information about
           the request and response data received and sent by Tomcat.
           Documentation at: /docs/config/valve.html -->
      <!--
      <Valve className="org.apache.catalina.valves.RequestDumperValve"/>
      -->

      <!-- This Realm uses the UserDatabase configured in the global JNDI
           resources under the key "UserDatabase".  Any edits
           that are performed against this UserDatabase are immediately
           available for use by the Realm.  -->
      <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
             resourceName="UserDatabase"/>

      <!-- Define the default virtual host
           Note: XML Schema validation will not work with Xerces 2.2.
       -->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true"
            xmlValidation="false" xmlNamespaceAware="false">

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log." suffix=".txt" pattern="common" resolveHosts="false"/>
        -->

      </Host>
    </Engine>
  </Service>
</Server>

```

## MySQL

```
[root@F5:Active] config # mysql --help
mysql  Ver 14.12 Distrib 5.0.85, for pc-linux-gnu (i686) using  EditLine wrapper
Copyright (C) 2000-2008 MySQL AB
This is commercial software, and use of this software is governed
by your applicable license agreement with MySQL

```

## startup

```
[root@F5:Active] config # chkconfig --list
anacron         0:off   1:off   2:on    3:on    4:on    5:on    6:off
auditd          0:off   1:off   2:on    3:on    4:on    5:on    6:off
bigstart        0:off   1:off   2:on    3:on    4:on    5:on    6:off
cluster         0:off   1:off   2:on    3:on    4:on    5:on    6:off
crond           0:off   1:off   2:on    3:on    4:on    5:on    6:off
dhclient        0:off   1:off   2:on    3:on    4:on    5:on    6:off
fw_update       0:off   1:off   2:off   3:on    4:off   5:off   6:off
httpd           0:off   1:off   2:on    3:on    4:on    5:on    6:off
httpd_sam       0:off   1:off   2:on    3:on    4:on    5:on    6:off
ip6tables       0:off   1:off   2:on    3:on    4:on    5:on    6:off
iptables        0:off   1:off   2:on    3:on    4:on    5:on    6:off
irqbalance      0:off   1:off   2:on    3:on    4:on    5:on    6:off
lm_sensors      0:off   1:off   2:on    3:on    4:on    5:on    6:off
lvm2-monitor    0:off   1:on    2:on    3:on    4:on    5:on    6:off
mcstrans        0:off   1:off   2:on    3:on    4:on    5:on    6:off
mdmonitor       0:off   1:off   2:on    3:on    4:on    5:on    6:off
mdmpd           0:off   1:off   2:off   3:off   4:off   5:off   6:off
multipathd      0:off   1:off   2:off   3:off   4:off   5:off   6:off
netconsole      0:off   1:off   2:on    3:on    4:on    5:on    6:off
netfs           0:off   1:off   2:off   3:on    4:on    5:on    6:off
netplugd        0:off   1:off   2:off   3:off   4:off   5:off   6:off
network         0:off   1:off   2:on    3:on    4:on    5:on    6:off
ntpd            0:off   1:off   2:on    3:on    4:on    5:on    6:off
postfix         0:off   1:off   2:off   3:off   4:off   5:off   6:off
rawdevices      0:off   1:off   2:off   3:on    4:on    5:on    6:off
rdisc           0:off   1:off   2:off   3:off   4:off   5:off   6:off
restorecond     0:off   1:off   2:on    3:on    4:on    5:on    6:off
rsync           0:off   1:off   2:on    3:on    4:on    5:on    6:off
saslauthd       0:off   1:off   2:off   3:off   4:off   5:off   6:off
smartd          0:off   1:off   2:on    3:on    4:on    5:on    6:off
sshd            0:off   1:off   2:on    3:on    4:on    5:on    6:off
syslog-ng       0:off   1:off   2:on    3:on    4:on    5:on    6:off

```

```

[root@F5:Active] config # cat /etc/rc.local
#!/bin/sh
#
# This script will be executed *after* all the other init scripts.
# You can put your own initialization stuff in here if you don't
# want to do the full Sys V style init stuff.
#
# NOTE:
# - /config/startup is for customer config additions and
#   will be saved in UCS and configsynced.
#
# - /etc/rc.local should *not* be used by customers and
#   can/will be changed by F5

#
# Add /var/log/pam and set the appropriate owner, permissions
#
mkdir /var/log/pam > /dev/null 2>&1
chmod 0770 /var/log/pam
chown root:apache /var/log/pam
touch /var/log/pam/tallylog
chmod 0660 /var/log/pam/tallylog
chown root:apache /var/log/pam/tallylog
chcon -t var_log_pam_t /var/log/pam/tallylog

#
# This callback is required for various Enterprise Manager tasks that
# may be performed on the device.
#
if [ -f /shared/f5_update_action -a -f /usr/bin/f5_update_checker ]; then
        nohup /usr/bin/f5_update_checker >> /var/log/f5_update_checker.log 2>&1
fi

# run customer config additions
if [ -x /config/startup ]; then
    /config/startup
fi

touch /var/lock/subsys/local

```

```
named	 Running for 3 hours
ntpd	 Running...
postfix	 master Stopped
snmpd	 Running for 3 hours
sshd

```

## 目录结构

```

[root@F5:Active] config # ls -l /
total 108
-r-xr-xr-x   1 root root  3120 Apr  9  2010 CONTACTS
-r-xr-xr-x   1 root root  2975 Apr  9  2010 COPYRIGHT
-rw-r--r--   1 root root    55 Jan  5 17:13 PLATFORM
-r-xr-xr-x   1 root root  1269 Apr  9  2010 VENDOR
lrwxrwxrwx   1 root root    11 Jan  5 17:10 VERSION -> VERSION.LTM
-rw-r--r--   1 root root     0 Apr  9  2010 VERSION.ASM
-rw-r--r--   1 root root   153 Jan  5 17:10 VERSION.LTM
-rw-r--r--   1 root root    25 Apr  9  2010 VERSION.WA
-rw-r--r--   1 root root     0 Apr  9  2010 VERSION.WOC
drwxr-xr-x   2 root root  3072 Jan  5 17:10 bin
drwxr-xr-x   3 root root  1024 Jan  5 17:11 boot
drwxr-xr-x   2 root root  1024 Jan  5 17:10 command
drwxr-xr-x  20 root root  1024 Jan  6 15:06 config
lrwxrwxrwx   1 root root    19 Jan  5 17:10 defaults -> /usr/share/defaults
drwxr-xr-x  13 root root  3820 Jan  6 14:38 dev
drwxr-xr-x  55 root root  5120 Jan  6 14:08 etc
lrwxrwxrwx   1 root root    19 Jan  5 17:10 examples -> /usr/share/examples
drwxr-xr-x   3 root root  1024 Jan  5 17:10 home
drwxr-xr-x   2 root root  1024 Apr  9  2010 hotfix
drwxr-xr-x  10 root root  6144 Jan  5 17:10 lib
drwx------   2 root root 12288 Jan  5 17:09 lost+found
drwxr-xr-x   2 root root  1024 Apr  9  2010 media
drwxr-xr-x   4 root root  1024 Jan  5 17:19 mnt
drwxr-xr-x   3 root root  1024 Jan  5 17:10 opt
dr-xr-xr-x 193 root root     0 Jan  6 14:07 proc
drwxr-x---   3 root root  1024 Jan  6 14:30 root
drwxr-xr-x   3 root root  6144 Jan  5 17:11 sbin
drwxr-xr-x   4 root root     0 Jan  6 14:07 selinux
lrwxrwxrwx   1 root root    12 Jan  5 17:10 service -> /var/service
drwxr-xr-x  17 root root  4096 Jan  5 17:19 shared
drwxr-xr-x   2 root root  1024 Apr  9  2010 srv
drwxr-xr-x  11 root root     0 Jan  6 14:07 sys
drwxrwxrwt   5 root root  1024 Jan  6 15:23 tmp
lrwxrwxrwx   1 root root     7 Jan  5 17:10 ts -> /var/ts
drwxr-xr-x  20 root root  4096 Jan  5 17:11 usr
drwxr-xr-x  31 root root  4096 Jan  6 14:08 var

```

1.  /config 配置文件

2.  /service 好象是状态监测脚本

3.  /opt 不清楚装的是什么

    ```
    [root@F5:Active] config # find /opt/
    /opt/
    /opt/F5
    /opt/F5/aom
    /opt/F5/aom/aom_cpld.apollo.jbc
    /opt/F5/aom/cfe_a.md5sum
    /opt/F5/aom/cfe_a.bin
    /opt/F5/aom/f5aom-kernel.bin
    /opt/F5/aom/cfe_a.flash
    /opt/F5/aom/aom_cpld.mercuryII.md5sum
    /opt/F5/aom/aom_cpld.gemini.jbc
    /opt/F5/aom/aom_cpld.mercury.md5sum
    /opt/F5/aom/f5aom.tgz
    /opt/F5/aom/aom_version
    /opt/F5/aom/aom_cpld.mercuryII.jbc
    /opt/F5/aom/aom_cpld.gemini.md5sum
    /opt/F5/aom/packages
    /opt/F5/aom/packages/aomctld_10.1.5-326.0_mipsel.ipk
    /opt/F5/aom/packages/aom_ipkg
    /opt/F5/aom/aom_cpld.apollo.md5sum
    /opt/F5/aom/aom_cpld.mercury.jbc
    /opt/F5/aom/aom_flash
    /opt/F5/aom/f5aom-kernel.md5sum

    ```

## network

```
[root@F5:Active] config # ifconfig
eth0      Link encap:Ethernet  HWaddr 00:01:D7:C3:30:C1
          inet6 addr: fe80::201:d7ff:fec3:30c1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12498 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12572 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1661936 (1.5 MiB)  TX bytes:1736530 (1.6 MiB)
          Interrupt:233 Memory:fdfe0000-fdff0000

eth0.1    Link encap:Ethernet  HWaddr 00:01:D7:C3:30:C1
          inet addr:127.2.0.2  Bcast:127.2.0.255  Mask:255.255.255.0
          inet6 addr: fe80::201:d7ff:fec3:30c1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:11067 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11058 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1166194 (1.1 MiB)  TX bytes:1081395 (1.0 MiB)

eth0:mgmt Link encap:Ethernet  HWaddr 00:01:D7:C3:30:C1
          inet addr:192.168.1.245  Bcast:192.168.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          Interrupt:233 Memory:fdfe0000-fdff0000

inside    Link encap:Ethernet  HWaddr 00:01:D7:C3:30:C3
          inet addr:192.168.3.18  Bcast:192.168.3.255  Mask:255.255.255.0
          inet6 addr: fe80::201:d7ff:fec3:30c3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:69690 errors:0 dropped:0 overruns:0 frame:0
          TX packets:49892 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:17012429 (16.2 MiB)  TX bytes:14199806 (13.5 MiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.255.255.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:352393 errors:0 dropped:0 overruns:0 frame:0
          TX packets:352393 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:40251897 (38.3 MiB)  TX bytes:40251897 (38.3 MiB)

tmm0      Link encap:Ethernet  HWaddr 00:98:76:54:32:10
          inet addr:127.1.1.1  Bcast:127.1.1.255  Mask:255.255.255.0
          inet6 addr: fe80::298:76ff:fe54:3210/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1009 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1634 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:58093 (56.7 KiB)  TX bytes:1999100 (1.9 MiB)

vlan_172  Link encap:Ethernet  HWaddr 00:01:D7:C3:30:C4
          inet6 addr: fe80::201:d7ff:fec3:30c4/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:0 (0.0 b)  TX bytes:468 (468.0 b)

```

### ip route

```
[root@F5:Active] config # ip route
127.1.1.0/24 dev tmm0  proto kernel  scope link  src 127.1.1.1
192.168.3.0/24 dev inside  proto kernel  scope link  src 192.168.3.18
192.168.1.0/24 dev eth0  proto kernel  scope link  src 192.168.1.245
127.2.0.0/24 dev eth0.1  proto kernel  scope link  src 127.2.0.2
10.0.0.0/16 dev inside  proto kernel  scope link  src 10.0.0.151
default via 192.168.3.1 dev inside

```

### tcpdump

```
[root@F5:Active] config # tcpdump
tcpdump: WARNING: eth0: no IPv4 address assigned
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 108 bytes
16:13:23.428694 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:23.431538 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:25.433745 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:25.436138 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:26.426907 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:26.437609 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:27.437792 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48
16:13:27.440174 IP F5.3900.Test.6215 > sccp.6216: UDP, length 48

8 packets captured
8 packets received by filter
0 packets dropped by kernel

```

### iptables

```
[root@F5:Active] config # iptables --version
iptables v1.3.5
[root@F5:Active] config # iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  sccp                 F5.3900.Test
ACCEPT     all  --  F5.3900.Test         F5.3900.Test
DROP       all  --  anywhere             F5.3900.Test

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

```

## vmstat

```
[root@F5:Active] config # vmstat 1
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  0      0 334808  47284 309648    0    0    11     6  276  200  7  3 90  0  0
 1  0      0 334924  47300 309632    0    0     0   312 1110 139916  7  2 91  0  0
 2  0      0 334932  47300 309668    0    0     0     0 1095 139561  7  3 91  0  0
 1  0      0 334932  47300 309668    0    0     0     0 1099 139556  7  2 91  0  0
 1  0      0 334932  47300 309668    0    0     0     0 1099 139559  7  2 91  0  0

```

## Language

### perl

```
[root@F5:Active] config # perl

[root@F5:Active] config # perl -v

This is perl, v5.8.8 built for i386-linux-thread-multi

Copyright 1987-2006, Larry Wall

Perl may be copied only under the terms of either the Artistic License or the
GNU General Public License, which may be found in the Perl 5 source kit.

Complete documentation for Perl, including FAQ lists, should be found on
this system using "man perl" or "perldoc perl".  If you have access to the
Internet, point your browser at http://www.perl.org/, the Perl Home Page.

```

### python

```

[root@F5:Active] config # python
Python 2.4.3 (#1, Apr  8 2010, 19:04:08)
[GCC 4.1.2 20080704 (Red Hat 4.1.2-46)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>

```

### PHP

```
[root@F5:Active] config #  php -v
PHP 5.2.10 (cli) (built: Apr  8 2010 20:58:22)
Copyright (c) 1997-2009 The PHP Group
Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies

```

### lua

```

[root@F5:Active] config # lua
Lua 5.1.2  Copyright (C) 1994-2007 Lua.org, PUC-Rio
>

```

## driver

```
[root@F5:Active] config # lsmod
Module                  Size  Used by
ti_usb_3410_5052      103660  1
usbserial              36720  3 ti_usb_3410_5052
8021q                  25872  0
iptable_filter          5376  1
ip_tables              22696  1 iptable_filter
x_tables               19720  1 ip_tables
nls_ascii               7488  0
nls_cp437               9152  0
vfat                   15744  0
fat                    55216  1 vfat
binfmt_misc            15052  1
vnic                  145152  5
parport                43980  0
lasthop                52688  0
ipv6                  310752  71 lasthop
xfrm_nalgo             12548  1 ipv6
crypto_api             12224  1 xfrm_nalgo
lapict                 18312  4
jiffies                 6288  8
i2c_dev                13192  14
datastor               11096  0
linux_user_bde         22704  0
linux_kernel_bde       36064  1 linux_user_bde
cdc_acm                17888  0
generic                 8516  0 [permanent]
tg3                   121668  0
serio_raw               9732  0
uhci_hcd               26648  0
i2c_i801               11028  7
ehci_hcd               35532  0
i2c_core               25408  2 i2c_dev,i2c_i801
pcspkr                  5504  0
raid1                  25024  0
raid0                   9792  0
mptspi                 21968  0
mptscsih               32192  1 mptspi
mptbase                71620  2 mptspi,mptscsih
scsi_transport_spi     29120  1 mptspi
3w_9xxx                36932  0
mv64xx                232736  0
sata_svw               23640  0
ahci                   37896  1
serverworks            11408  0 [permanent]
sata_sil               13576  0
ata_piix               26244  0
libata                162896  4 sata_svw,ahci,sata_sil,ata_piix
sd_mod                 25856  2
amd74xx                17200  0 [permanent]
piix                   13832  0 [permanent]
ide_disk               18304  3
ide_core              144248  5 generic,serverworks,amd74xx,piix,ide_disk
dm_snapshot            21256  0
dm_mirror              23888  0
dm_log                 14272  1 dm_mirror
dm_mod                 67336  25 dm_snapshot,dm_mirror,dm_log
ext3                  138192  6
jbd                    63664  1 ext3
mbcache                11976  1 ext3

```

## F5 所用到的 rpm 包

可以看出 F5 对 Linux 减肥做的不好

```
[root@test:Active] config # rpm -qa | wc -l
483
[root@test:Active] config # rpm -qa | more
basesystem-8.0-5.1.1.el5.4.0
thrift-0.2-763.0
aceagentsdk-6.1-763.0
schema-java-10.2.2-763.0
commons-fileupload-1.2.1-1.4.0
perl-JSON-RPC-0.96-763.0
jython-2.1-763.0
fwmgr-c106-10.2.2-763.0
f5-common-release-1.0.5-4.0
cn3xxx-1.2.5-763.0
libstdc++-4.1.2-46.el5.4.0
libcpp-10.2.2-763.0
libhalmsg-nonbj-10.2.2-763.0
bigdb-10.2.2-763.0
popt-1.10.2.3-20.el5.4.0
openssl-0.9.8e-12.el5_5.7.4.0
libsepol-1.15.2-2.el5.4.0
device-mapper-1.02.32-1.el5.4.0
expat-1.95.8-8.3.el5_4.2.4.0
boost-1.34.1-13.4.0
libcap-1.10-26.4.0
findutils-4.2.27-6.el5.4.0
Xerces-c-2.8.0-1.4.0
procps-3.2.7-11.1.el5.4.0
perl-5.8.8-18.el5.4.0
vcmp_shm-10.2.2-763.0
perl-Clone-0.28-4.fc9.4.0
apr-1.2.7-11.1.4.0
binutils-2.17.50.0.6-12.el5.4.0
elfutils-libelf-0.137-3.el5.4.0
tar-1.15.1-23.0.1.el5.4.0
perl-Text-Iconv-1.4-4.0
bzip2-1.0.3-6.el5.4.0
cpio-2.6-23.el5.4.0
dialog-1.0.20051107-1.2.2.4.0
iptables-1.3.5-5.3.el5.4.0
file-4.17-15.el5_3.1.4.0
libsysfs-2.0.0-6.4.0
device-mapper-multipath-0.4.7-30.el5.4.0
rsync-2.6.8-3.1.4.0
apr-util-1.2.7-7.el5_3.2.4.0
numactl-0.9.8-8.el5.4.0
audit_forwarder-10.2.2-763.0
xfsprogs-2.9.4-1.4.0
lua-5.1.2-763.0
ed-0.2-39.el5.4.0
bigtop-10.2.2-763.0
log2mail-10.2.2-763.0
apache_auth_token_mod-10.2.2-763.0
libpva-10.2.2-763.0
php-mcp-10.2.2-763.0
checkpolicy-1.33.1-4.el5.4.0
dosfstools-2.11-7.el5.4.0
hesiod-3.1.0-8.4.0
libjpeg-6b-37.4.0
pam_audit-10.2.2-763.0
sgpio-1.2.0_10-2.4.0
vconfig-1.9-2.1.4.0
crontabs-1.10-8.4.0
perl-Config-IniFiles-2.39-4.0
perl-JSON-2.15-4.0
syslinux-3.11-4.4.0
perl-Apache-Admin-Config-0.91-763.0
perl-DBIx-ContextualFetch-1.03-4.0
fwmgr-10.2.2-763.0
nash-5.1.19.6-54.4.0
perl-HTML-Parser-3.55-1.fc6.4.0
krb5-libs-1.7.1-0.4.0
perl-SOAP-Lite-0.71-1.4.0
TS-mng-idl-10.2.2-763.0
geoip-data-1.0.0-20110505.64.0
f5km-10.2.2-763.0
xconfig-10.2.2-763.0
checkcert-10.2.2-763.0
libselinux-python-1.33.4-5.5.el5.4.0
tmstat-10.2.2-763.0
php-5.2.14-763.0
rpm-libs-4.4.2.3-20.el5.4.0
passwd-0.73-1.4.0
samba-client-3.0.33-3.15.el5.4.0
TS-mng-asmcsd-10.2.2-763.0
pam_radius_auth-1.3.17-4.0
jpackage-utils-1.5.25-1jpp.4.0
ntp-4.2.2p1-9.el5.2.1.4.0
vixie-cron-4.1-76.el5.4.0
mdadm-2.6.9-2.el5.4.0
MySQL-server-5.0.85-0.rhel3.763.0
alertd-10.2.2-763.0
ZebOS-7.5.1-763.0
f5mku-10.2.2-763.0
rmonsnmpd-10.2.2-763.0
clusterd-10.2.2-763.0
fpdd-10.2.2-763.0
sod-10.2.2-763.0
wccpd-10.2.2-763.0
openssh-clients-4.3p2-36.el5_4.2.4.0
cyrus-sasl-2.1.22-5.el5.4.0
mcstrans-0.2.11-3.el5.4.0
tmui-10.2.2-763.1
TS-mng-install-10.2.2-763.0
TS-csrf-0.0.1-10.2.2.763.0
gtm-10.2.2-763.0
TS-negsig-10.2.2-763.1
f5-console-serial-10.2.2-763.0
WA-10.2.2-763.0
sam-main-10.2.2-763.0
bind-utils-9.6.2.P3-763.0
kernel-app-2.6.18-164.11.1.el5.4.0
bcmsdk-modules-5.7.1-763.0
sata_svw-app-2.10.7-763.0
f5-platform-family-moonshot-10.2.2-763.0
pciutils-2.2.3-7.el5.4.0
ltm-application-10.2.2-763.0
TS-xml-processor-10.2.2-852.0
net-snmp-5.4.2.1-852.0
mod_ssl-2.2.3-31.el5.852.0
apd-10.2.2-852.0
TS-mng-dcc-10.2.2-852.0
f5base-10.2.2-852.0
datastor-10.2.2-852.0
pam-0.99.6.2-6.el5.8.0
tamd-10.2.2-852.0
iControl-10.2.2-852.0
cs-10.2.2-763.0
filesystem-2.4.0-2.el5.centos.4.0
mcpj-10.2.2-763.0
libgcc-4.1.2-46.el5.4.0
perl-Net-DNS-0.61-4.0
perl-Bit-Vector-6.4-10.2.2.763.0
hsqldb-1.8.0.7-763.0
termcap-5.5-1.20060701.1.4.0
pkg-tools-10.2.2-763.0
mailcap-2.1.23-1.el5.4.0
cracklib-dicts-2.8.9-3.3.4.0
ca-bundle-10.2.2-763.0
fflag-10.2.2-763.0
windlls-1.0-1.0.4.12.0
rrdshim-10.2.2-763.0
perl-XML-SAX-0.12-4.0
mysql-connector-java-5.1.7-763.0
man-pages-f5man-2.39-12.el5.4.0
jacl-1.4.1-4.0
flashrom-0.9.0-4.0
f5-filesys-10.2.2-763.0
cs-config-10.2.2-763.0
alertd-config-10.2.2-763.0
glibc-2.5-49.7.4.0
mcplib-10.2.2-763.0
cppcommon-10.2.2-763.0
chkconfig-1.3.30.1-2.4.0
loki-lib-0.1.6-6.fc9.4.0
isc-10.2.2-763.0
ZThread-2.3.2-763.0
glib2-2.12.3-2.el5.4.0
f5util-10.2.2-763.0
audit-libs-1.7.13-2.el5_3.4.0
bash-3.2-24.el5.4.0
ncurses-5.5-24.20060715.4.0
info-4.8-14.el5.4.0
libselinux-1.33.4-5.5.el5.4.0
tcp_wrappers-7.6-40.7.el5.4.0
e2fsprogs-libs-1.39-23.el5.4.0
openldap-2.3.43-3.el5.4.0
sed-4.1.5-5.el5.4.0
cavium_utils-10.2.2-763.0
grep-2.5.1-55.el5.4.0
gawk-3.1.5-14.el5.4.0
db4-4.3.29-10.el5.4.0
libbigpacket-10.2.2-763.0
protobuf-2.3.0-763.0
fcgi-2.4.0-4.0
diffutils-2.8.1-15.2.3.el5.4.0
nspr-4.7.4-1_el5_3_1.4.0
gdbm-1.8.0-26.2.1.4.0
perl-DBI-1.52-2.el5.4.0
libdatastor-10.2.2-763.0
logrotate-3.7.4-9.4.0
perl-Proc-ProcessTable-0.42-4.0
xalan-c-1.10.0-4.4.0
e2fsprogs-1.39-23.el5.4.0
psmisc-22.2-7.4.0
less-394-6.el5.4.0
libcave-10.2.2-763.0
keyutils-libs-1.2-1.el5.4.0
libacl-2.2.39-3.el5.4.0
perl-File-FnMatch-0.02-4.0
perl-Socket6-0.19-3.4.0
perl-YAML-LibYAML-3.32-4.0
vim-common-7.0.109-6.el5.4.0
log4c-1.2.1-10.2.2.763.0
net-tools-1.60-78.el5.4.0
groff-1.18.1.1-11.1.4.0
mtools-3.9.10-2.el5.4.0
freetype-2.2.1-28.el5.4.0
iputils-20020927-46.el5.4.0
lcdproc-0.5.2-4.fc9.4.0
ethconfig-10.2.2-763.0
ethtool-6-3.el5.4.0
mailx-8.1.1-44.2.2.4.0
unzip-5.52-3.el5.4.0
iptables-ipv6-1.3.5-5.3.el5.4.0
vim-enhanced-7.0.109-6.el5.4.0
vim-minimal-7.0.109-6.el5.4.0
tmpwatch-2.9.7-1.1.el5.2.4.0
hmaccalc-0.9.6-1.4.0
perl-Compress-Zlib-1.42-1.fc6.4.0
grub-0.97-13.5.4.0
php-gui2pb-10.2.2-763.0
openldap-clients-2.3.43-3.el5.4.0
device-mapper-event-1.02.32-1.el5.4.0
ftp-0.17-35.el5.4.0
tftp-hpa-0.48-4.0
lsof-4.78-3.4.0
nano-1.3.12-1.1.4.0
get_dossier-10.2.2-763.0
libedit-2.10-4.0
f5_update_checker-10.2.2-763.0
monitors-genericdb-10.2.2-763.0
isomd5sum-11.1.0.95-4.0
mod_f5_auth_cookie-10.2.2-763.0
nc-1.84-10.el5.4.0
pam_bigip_authz-10.2.2-763.0
mkelfImage-2.7-4.0
TS-mng-fbloader-10.2.2-763.0
libpva10-10.2.2-763.0
daglib-10.2.2-763.0
lm_sensors-2.10.7-4.el5.4.0
eventlog-0.2.5-9.fc9.4.0
hdparm-6.6-2.4.0
libart-2.3.17-4.0
libhugetlbfs-1.3-3.el5.4.0
ghostscript-8.15.2-9.11.el5.4.0
mod_fastcgi-2.4.0-763.0
php-hsl-10.2.2-763.0
setserial-2.17-19.2.2.4.0
srm-1.2.8-4.0
traceroute-2.0.1-5.el5.4.0
perl-URI-1.35-3.4.0
perl-XML-Simple-2.13-4.0
perl-Class-Data-Inheritable-0.04-4.0
perl-Config-General-2.38-1.fc9.4.0
perl-DateManip-5.44-1.2.1.4.0
perl-HTML-Tagset-3.10-2.1.1.4.0
perl-XML-RegExp-0.03-4.fc9.4.0
perl-Class-DBI-3.0.14-4.0
mibs_pack-10.2.2-763.0
lcdproc-mercury-0.5.2-4.fc9.4.0
perl-IO-Socket-INET6-2.51-2.4.0
perl-Ima-DBI-0.34-4.0
perl-Class-Accessor-0.25-4.0
perl-Config-Crontab-1.03-4.0
perl-XML-NamespaceSupport-1.09-4.0
f5mfg-10.2.2-763.0
fwmgr_bios-c106-10.2.2-763.0
jakarta-commons-codec-1.3-7jpp.2.4.0
upgrade-selector-10.2.2-763.0
xerces-j2-2.4.0-3jpp.4.0
anacron-2.3-45.el5.4.0
tm_install-2.7.0-487.0
config-templates-10.2.2-763.0
coreutils-5.97-23.el5.4.0
python-2.4.3-27.el5.4.0
perl-XML-Twig-3.32-4.0
tzdata-2011d-1.el5.4.0
perl-XML-XPath-1.13-4.0
perl-XML-Encoding-1.01-23.763.0
aom-software-1.0.F5-10.1.8.348.0
installer-10.2.2-763.0
curl-7.15.5-2.1.el5_3.5.4.0
rrdtool-1.2.27-4.0
libsemanage-1.9.1-4.4.el5.4.0
msktutil-0.3.16-4.0
rrdtool-perl-1.2.27-4.0
gencert-10.2.2-763.0
audit-libs-python-1.7.13-2.el5_3.4.0
cyrus-sasl-gssapi-2.1.22-5.el5.4.0
audit-1.7.13-2.el5_3.4.0
umem-1.0.1-10.2.2.763.0
shadow-utils-4.0.17-14.el5.4.0
MySQL-shared-5.0.85-0.rhel3.763.0
libcoapi-10.2.2-763.0
udev-095-14.21.el5.4.0
SysVinit-2.86-15.el5.763.0
libuser-0.54.7-2.el5.5.4.0
krb5-workstation-1.7.1-0.4.0
samba-common-3.0.33-3.15.el5.4.0
dmraid-1.0.0.rc13-53.el5.4.0
oprofile-0.9.4-11.el5.4.0
man-1.6d-1.1.4.0
pam_krbdelegate-10.2.2-763.0
pam_tacplus-1.2.9-4.0
dmraid-events-1.0.0.rc13-53.el5.4.0
jre6-1.6.0-24.4.0
domaintool-10.2.2-763.0
initscripts-8.45.30-2.el5.4.0
syslog-ng-2.0.8-1.fc9.4.0
openssh-4.3p2-36.el5_4.2.4.0
postfix-2.3.3-2.1.el5.4.0
runit-1.0.4-763.0
MySQL-client-5.0.85-0.rhel3.763.0
system_check-10.2.2-763.0
shell-10.2.2-763.0
mcpq-10.2.2-763.3
nokiasnmpd-10.2.2-763.0
subsnmpd-10.2.2-763.0
aced-10.2.2-763.0
cssd-10.2.2-763.0
dedup_admin-10.2.2-763.0
lacpd-10.2.2-763.0
logstatd-10.2.2-763.0
promptstatusd-10.2.2-763.0
statsd-10.2.2-763.0
websso-10.2.2-763.0
wocplugin-10.2.2-763.0
openssh-server-4.3p2-36.el5_4.2.4.0
mod_auth_pam-1.1.1-5.fc9.763.0
rrdstats-10.2.2-763.0
xui-10.2.2-763.0
pam_ldap-184-4.0
irqbalance-0.55-15.el5.4.0
portmap-4.0-65.2.2.1.4.0
tomcat-6.0.20-6.2.fc10.4.0
wdiag-1.0-10.2.2.763.3
TS-mng-learning-10.2.2-763.0
TS-cshui-0.0.1-10.2.2.763.0
TS-jsepee-10.2.2-763.0
gtm-big3d-10.2.2-763.0
selinux-policy-2.4.6-255.el5.763.0
dmon-10.2.2-763.0
dashboard-10.2.2-763.0
wa-master-10.2.2-763.0
zrd-10.2.2-763.0
mkinitrd-5.1.19.6-54.4.0
datastor-drv-app-10.2.2-763.0
auto-lasthop-app-10.2.2-763.0
jiffies-app-10.2.2-763.0
sata_odin-app-3.1.0-763.0
tiusb-app-1.1-763.0
f5-platform-common-10.2.2-763.0
f5-platform-id-C106-10.2.2-763.0
ts-application-10.2.2-763.1
ros-application-10.2.2-763.0
tmplugin-10.2.2-852.0
httpd-2.2.3-31.el5.852.0
TS-mng-scripts-10.2.2-852.0
iControl-handlers-10.2.2-852.0
tmsh-10.2.2-852.0
cbrd-10.2.2-852.0
TS-bd-10.2.2-852.0
rewrite-plugin-10.2.2-852.0
tmrouted-10.2.2-852.0
bigd-10.2.2-852.0
tmm-10.2.2-852.0
iControl-wsdl-10.2.2-852.0
TS-database-10.2.2-852.0
halid-10.2.2-852.0
sam-www-10.2.2-852.0
tmm-padc-10.2.2-852.0
tmm-padc-debug-10.2.2-852.0
sam-client-10.2.2-85.0
sam-linux-svpn-10.2.2-85.0
iControl-modules-10.2.2-852.0
setup-2.5.58-7.el5.4.0
TS-asm-config-10.2.2-763.0
halreboot-10.2.2-763.0
xml-commons-apis-1.0.5-4.0
perl-UNIVERSAL-moniker-0.08-4.0
commons-io-1.4-1.4.0
words-3.0-9.1.4.0
rootfiles-8.1-1.1.1.4.0
marketing-names-1-1.0.0.126.0
hotfix-test-10.2.2-763.3
f5-release-info-10.2.2-763.0
dedup-10.2.2-763.0
glibc-common-2.5-49.7.4.0
zlib-1.2.3-3.4.0
libschemadata-10.2.2-763.0
ha_table-10.2.2-763.0
mktemp-1.5-23.2.2.4.0
libtermcap-2.0.8-46.1.4.0
libhal-10.2.2-763.0
readline-5.1-3.el5.4.0
cyrus-sasl-lib-2.1.22-5.el5.4.0
bzip2-libs-1.0.3-6.el5.4.0
pcre-7.9-4.0
libmcp_cpp-10.2.2-763.0
libxml2-2.6.26-2.1.2.8.4.0
libbind-devfs-6.0-763.0
libpng-1.2.10-7.1.el5.2.4.0
geoip-1.0-10.2.2.763.0
perl-DBD-MySQL-3.0007-2.el5.763.0
nss-3.12.3.99.3-1_el5_2.4.0
kpartx-0.4.7-30.el5.4.0
gzip-1.3.5-10.el5.4.0
libattr-2.4.32-1.1.4.0
perl-Net-SSLeay-1.25-4.0
iproute-2.6.18-10.el5.4.0
sqlite-3.3.6-5.4.0
libidn-0.6.5-1.1.4.0
cracklib-2.8.9-3.3.4.0
libconnapi-10.2.2-763.0
libusb-0.1.12-5.1.4.0
mingetty-1.07-5.2.2.4.0
TS-pabnagd-10.2.2-763.0
keyutils-1.2-1.el5.4.0
haltools-moonshot-10.2.2-763.0
perl-Crypt-SSLeay-0.57-1.4.0
mpidump-10.2.2-763.0
lvm2-2.02.46-8.el5.4.0
libselinux-utils-1.33.4-5.5.el5.4.0
time-1.7-27.2.2.4.0
telnet-0.17-39.el5.4.0
octeon-1.7.0-763.0
TS-mng-fbagent-10.2.2-763.0
GeoIP-1.4.5-763.0
beecrypt-4.1.2-10.1.1.4.0
dmidecode-2.9-1.el5.4.0
f5logging-10.2.2-763.0
libdnsshim-10.2.2-763.0
mkisofs-2.01-10.7.el5.4.0
setarch-2.0-1.1.4.0
strace-4.5.18-5.el5.4.0
perl-TimeDate-1.16-5.el5.4.0
perl-Class-Trigger-0.10-4.0
perl-Devel-Symdump-2.08-4.0
perl-version-0.64-4.0
perl-Test-Class-0.28-4.0
perl-IO-Socket-SSL-0.96-4.0
perl-Class-DBI-mysql-1.00-4.0
f5config-aom-10.2.2-763.0
guishell-10.2.2-763.0
woc-utils-10.2.2-763.0
perl-libwww-perl-5.805-1.1.1.4.0
log4j-1.2.13-3jpp.2.4.0
perl-XML-Parser-2.34-6.1.2.2.1.4.0
perl-XML-DOM-1.44-4.fc9.4.0
aom-firmware-1.4-10.1.8.348.0
perl-bigip-10.2.2-763.0
oam-10.2.2-763.0
gnupg-1.4.5-14.4.0
ssldump-0.9b3-763.0
f5config-10.2.2-763.0
util-linux-2.13-0.52.el5.4.0
rpm-4.4.2.3-20.el5.4.0
which-2.16-7.4.0
MAKEDEV-3.23-1.2.4.0
perl-RPM2-0.67-4.0
TS-mng-log_manager-10.2.2-763.0
tcpdump-3.9.4-14.el5.763.0
java-shell-10.2.2-763.0
policycoreutils-1.33.12-14.6.el5.4.0
bigstart-10.2.2-763.0
bigdbd-10.2.2-763.0
stpd-10.2.2-763.0
monitors-10.2.2-763.0
acctd-10.2.2-763.0
csyncd-10.2.2-763.0
lind-10.2.2-763.0
radvd-0.9.1-4.763.0
syscalld-10.2.2-763.0
wocd-10.2.2-763.0
guiutils-10.2.2-763.0
rtstats-10.2.2-763.0
dhclient-3.0.5-21.el5_bz356286.4.0
smartmontools-5.38-2.el5.4.0
tmui-templates-10.2.2-763.1
TS-mng-recovery-10.2.2-763.0
selinux-policy-targeted-2.4.6-255.el5.763.0
module-init-tools-3.3-0.pre3.1.54.el5.4.0
hwdata-0.213.16-1.el5.4.0
lapict-app-10.2.2-763.0
vnic-app-10.2.2-763.0
woc-application-lm-10.2.2-763.3
bind-9.6.4.ESV.R4.P3-852.0
libhal_internal-10.2.2-852.0
chmand-10.2.2-852.0
ntlmconnpool-10.2.2-852.0
bcm56xxd-10.2.2-852.0
epsec-1.0.0-91.0
TS-tsui-10.2.2-852.0
sam-mac-edge-10.2.2-85.0
mcpd-10.2.2-852.0
sam-mac-svpn-10.2.2-85.0

```

## 第 31 章 bigpipe - a command line interface for configuring BIG-IP and VIPRION and displaying configuration data and statistics

## b

```
[root@F5:Active] config # which b
/usr/bin/b
[root@F5:Active] config # ll /usr/bin/b
lrwxrwxrwx 1 root root 7 Jan  5 17:10 /usr/bin/b -> bigpipe

```

## F5 Management Port Setup

```
[root@F5:Active] config # config

```

## Local Traffic

### Profiles

#### profile http

```
b profile http my_http_profile {
   defaults from http-lan-optimized-caching
   compress content type include {
      "text/"
      "application/(xml|x-javascript)"
      "application/pdf"
   }
}

```

例 31.1. Profile HTTP Example

```
[root@F5:Active] ~ # b profile http my_http_profile { \
>    defaults from http-lan-optimized-caching \
>    compress content type include { \
>       "text/" \
>       "application/(xml|x-javascript)" \
>       "application/pdf" \
>    } \
> }

[root@F5:Active] ~ # b profile http my_http_profile list
profile http my_http_profile {
   defaults from http-lan-optimized-caching
   compress content type include {
      "text/"
      "application/(xml|x-javascript)"
      "application/pdf"
   }
}

```

#### profile persist

```
profile persist my_persist_profile {
   defaults from cookie
   mode cookie
}

```

#### profile tcp

```
profile tcp my_lan-optimized_tcp_profile {
   defaults from tcp-lan-optimized
}

```

### Pool

#### b pool show

```
[root@F5:Active] config # b pool show
POOL Pool-Http  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/2
|     (cur, max, limit, tot) = (0, 8, 0, 25)
|     (pkts,bits) in = (393, 459640), out = (584, 5.486M)
+-> POOL MEMBER Pool-Http/10.0.0.41:http   active,up
|   |     session enabled    priority 0    ratio 1
|   |     (cur, max, limit, tot) = (0, 4, 0, 7)
|   |     (pkts,bits) in = (41, 55672), out = (35, 42656)
|   |     requests (total) = 7
+-> POOL MEMBER Pool-Http/10.0.0.51:http   active,up
    |     session enabled    priority 0    ratio 1
    |     (cur, max, limit, tot) = (0, 4, 0, 18)
    |     (pkts,bits) in = (352, 403968), out = (549, 5.444M)
    |     requests (total) = 29

[root@F5:Active] config # b pool show all
POOL Pool-Http  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/2
|     (cur, max, limit, tot) = (0, 8, 0, 27)
|     (pkts,bits) in = (407, 496952), out = (595, 5.507M)
|     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
|     PVA (pkts,bits) in = (0, 0), out = (0, 0)
|     PVA assist conns (tot, curr) = (0, 0)
+-> POOL MEMBER Pool-Http/10.0.0.41:http   active,up
|   |     session enabled    priority 0    ratio 1
|   |     (cur, max, limit, tot) = (0, 4, 0, 7)
|   |     (pkts,bits) in = (41, 55672), out = (35, 42656)
|   |     requests (total) = 7
|   |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
|   |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
|   |     PVA assist conns (tot, curr) = (0, 0)
+-> POOL MEMBER Pool-Http/10.0.0.51:http   active,up
    |     session enabled    priority 0    ratio 1
    |     (cur, max, limit, tot) = (0, 4, 0, 20)
    |     (pkts,bits) in = (366, 441280), out = (560, 5.464M)
    |     requests (total) = 32
    |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
    |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
    |     PVA assist conns (tot, curr) = (0, 0)
[root@F5:Active] config #

```

#### Up / Down

```

b pool <pool_name> member all up
b pool <pool_name> member all down

```

#### create pool

```
[root@F5:Active] config # b pool test member 172.16.0.10:80 add
[root@F5:Active] config # b pool test member 172.16.0.11:80 add
[root@F5:Active] config # b pool test member 172.16.0.12:80 add

[root@F5:Active] config # b pool test show
POOL test  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/0
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
+-> POOL MEMBER test/172.16.0.10:http   active,unchecked
|   |     session enabled    priority 0    ratio 1
|   |     (cur, max, limit, tot) = (0, 0, 0, 0)
|   |     (pkts,bits) in = (0, 0), out = (0, 0)
|   |     requests (total) = 0
+-> POOL MEMBER test/172.16.0.11:http   active,unchecked
|   |     session enabled    priority 0    ratio 1
|   |     (cur, max, limit, tot) = (0, 0, 0, 0)
|   |     (pkts,bits) in = (0, 0), out = (0, 0)
|   |     requests (total) = 0
+-> POOL MEMBER test/172.16.0.12:http   active,unchecked
    |     session enabled    priority 0    ratio 1
    |     (cur, max, limit, tot) = (0, 0, 0, 0)
    |     (pkts,bits) in = (0, 0), out = (0, 0)
    |     requests (total) = 0

```

```
b pool mypool { monitor all http member 10.2.3.11:http member 10.2.3.12:http }

```

#### delete pool

```
b pool test delete

```

#### session enable / disable

```
[root@F5:Active] config # b pool test member 172.16.0.11:80 session enable
[root@F5:Active] config # b pool test member 172.16.0.12:80 session disable

```

### Virtual

#### b virtual show

```
[root@F5:Active] config # b virtual show
VIRTUAL ADDRESS 192.168.3.19   UNIT 1
|     ARP enable
|     (cur, max, limit, tot) = (0, 7, 0, 34)
|     (pkts,bits) in = (857, 711392), out = (1485, 16.17M)
+-> VIRTUAL VS-HTTP   SERVICE http
    |     PVA acceleration none
    |     (cur, max, limit, tot) = (0, 7, 0, 34)
    |     (pkts,bits) in = (857, 711392), out = (1485, 16.17M)
    |     requests (total) = 39
    +-> POOL Pool-Http  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/2
        |     (cur, max, limit, tot) = (0, 8, 0, 27)
        |     (pkts,bits) in = (407, 496952), out = (595, 5.507M)
        +-> POOL MEMBER Pool-Http/10.0.0.41:http   active,up
        |   |     session enabled    priority 0    ratio 1
        |   |     (cur, max, limit, tot) = (0, 4, 0, 7)
        |   |     (pkts,bits) in = (41, 55672), out = (35, 42656)
        |   |     requests (total) = 7
        +-> POOL MEMBER Pool-Http/10.0.0.51:http   active,up
            |     session enabled    priority 0    ratio 1
            |     (cur, max, limit, tot) = (0, 4, 0, 20)
            |     (pkts,bits) in = (366, 441280), out = (560, 5.464M)
            |     requests (total) = 32

[root@F5:Active] config # b virtual show all
VIRTUAL ADDRESS 192.168.3.19   UNIT 1
|     ARP enable
|     (cur, max, limit, tot) = (0, 7, 0, 34)
|     (pkts,bits) in = (857, 711392), out = (1485, 16.17M)
|     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
|     PVA (pkts,bits) in = (0, 0), out = (0, 0)
|     PVA assist conns (tot, curr) = (0, 0)
+-> VIRTUAL VS-HTTP   SERVICE http
    |     PVA acceleration none
    |     CMP enable on none   mode: all
    |     (cur, max, limit, tot) = (0, 7, 0, 34)
    |     (pkts,bits) in = (857, 711392), out = (1485, 16.17M)
    |     requests (total) = 39
    |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
    |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
    |     PVA assist conns (tot, curr) = (0, 0)
    |     ephem (cur, max, limit, tot) = (0, 0, 0, 0)
    |     ephem (pkts,bits) in = (0, 0), out = (0, 0)
    |     no nodes errors = 0
    +-> POOL Pool-Http  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/2
        |     (cur, max, limit, tot) = (0, 8, 0, 27)
        |     (pkts,bits) in = (407, 496952), out = (595, 5.507M)
        |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
        |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
        |     PVA assist conns (tot, curr) = (0, 0)
        +-> POOL MEMBER Pool-Http/10.0.0.41:http   active,up
        |   |     session enabled    priority 0    ratio 1
        |   |     (cur, max, limit, tot) = (0, 4, 0, 7)
        |   |     (pkts,bits) in = (41, 55672), out = (35, 42656)
        |   |     requests (total) = 7
        |   |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
        |   |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
        |   |     PVA assist conns (tot, curr) = (0, 0)
        +-> POOL MEMBER Pool-Http/10.0.0.51:http   active,up
            |     session enabled    priority 0    ratio 1
            |     (cur, max, limit, tot) = (0, 4, 0, 20)
            |     (pkts,bits) in = (366, 441280), out = (560, 5.464M)
            |     requests (total) = 32
            |     PVA (cur, max, limit, tot) = (0, 0, 0, 0)
            |     PVA (pkts,bits) in = (0, 0), out = (0, 0)
            |     PVA assist conns (tot, curr) = (0, 0)
[root@F5:Active] config #

```

```
[root@F5:Active] config # b virtual all destination show
VIRTUAL TEST_HTTP - Destination: 172.16.0.25:http
VIRTUAL VS-HTTP - Destination: 192.168.3.19:http

```

#### create virtual

```
[root@F5:Active] config # b virtual vs_apache { destination 11.11.11.12:80 persist source_addr pool test }

```

```
[root@F5:Active] config # b virtual vs_apache show
VIRTUAL ADDRESS 11.11.11.12   UNIT 1
|     ARP enable
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
+-> VIRTUAL vs_apache   SERVICE http
    |     PVA acceleration none
    |     (cur, max, limit, tot) = (0, 0, 0, 0)
    |     (pkts,bits) in = (0, 0), out = (0, 0)
    |     requests (total) = 0
    +-> POOL test  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/0
        |     (cur, max, limit, tot) = (0, 0, 0, 0)
        |     (pkts,bits) in = (0, 0), out = (0, 0)
        +-> POOL MEMBER test/172.16.0.10:http   active,unchecked
        |   |     session enabled    priority 0    ratio 1
        |   |     (cur, max, limit, tot) = (0, 0, 0, 0)
        |   |     (pkts,bits) in = (0, 0), out = (0, 0)
        |   |     requests (total) = 0
        +-> POOL MEMBER test/172.16.0.11:http   active,unchecked
        |   |     session enabled    priority 0    ratio 1
        |   |     (cur, max, limit, tot) = (0, 0, 0, 0)
        |   |     (pkts,bits) in = (0, 0), out = (0, 0)
        |   |     requests (total) = 0
        +-> POOL MEMBER test/172.16.0.12:http   active,unchecked
            |     session enabled    priority 0    ratio 1
            |     (cur, max, limit, tot) = (0, 0, 0, 0)
            |     (pkts,bits) in = (0, 0), out = (0, 0)
            |     requests (total) = 0

```

#### persist

```
b profile persist (Virtual Server) delete
b profile persist (Virtual Server) {mode cookie cookie mode hash cookie name (cookie) cookie hash offset 0 cookie hash length 4}

```

#### delete

```

b virtual (<virtual key list> | all) delete
b virtual address (<virtual address key list> | all) delete

```

#### enable / disable

```
b virtual address 10.10.10.20 disable

```

### Node

```
[root@F5:Active] config # b node list
node 10.0.0.41 {}
node 10.0.0.51 {}
node 172.16.0.5 {}
node 172.16.0.9 {}
node 192.168.3.9 {}
node 192.168.3.10 {}

[root@F5:Active] config # b node show
NODE 10.0.0.41   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 4, 0, 9)
|     (pkts,bits) in = (75, 115216), out = (85, 498040)
|     requests (total) = 10
NODE 10.0.0.51   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 4, 0, 20)
|     (pkts,bits) in = (366, 441280), out = (560, 5.464M)
|     requests (total) = 32
NODE 172.16.0.5   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
|     requests (total) = 0
NODE 172.16.0.9   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
|     requests (total) = 0
NODE 192.168.3.9   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
|     requests (total) = 0
NODE 192.168.3.10   unchecked   session user enabled
|     (cur, max, limit, tot) = (0, 0, 0, 0)
|     (pkts,bits) in = (0, 0), out = (0, 0)
|     requests (total) = 0

```

### Example

例 31.2. SLB HTTP Example

创建 Pool

```
[root@F5:Active] ~ # b pool mypool { monitor all http member 172.16.0.5:http{priority 1} member 172.16.0.9:http{priority 1} }
[root@F5:Active] ~ # b pool mypool list
pool mypool {
   monitor all http
   members {
      172.16.0.5:http {
         priority 1
      }
      172.16.0.9:http {
         priority 1
      }
   }
}

```

创建 Virtual Server

```

[root@F5:Active] ~ # b virtual myvs { snat automap pool mypool destination 192.168.3.22:80 persist source_addr profiles {  http-wan-optimized-compression-caching {} tcp {} } }

[root@F5:Active] ~ # b virtual myvs list
virtual myvs {
   snat automap
   pool mypool
   destination 192.168.3.22:http
   ip protocol tcp
   persist source_addr
   profiles {
      http-wan-optimized-compression-caching {}
      tcp {}
   }
}

[root@F5:Active] ~ # b virtual myvs show
VIRTUAL ADDRESS 192.168.3.22   UNIT 1
|     ARP enable
|     (cur, max, limit, tot) = (1, 4, 0, 22)
|     (pkts,bits) in = (111, 102056), out = (110, 110272)
+-> VIRTUAL myvs   SERVICE http
    |     PVA acceleration none
    |     (cur, max, limit, tot) = (1, 4, 0, 22)
    |     (pkts,bits) in = (111, 102056), out = (110, 110272)
    |     requests (total) = 0
    +-> POOL mypool  LB METHOD round robin   MIN/CUR ACTIVE MEMBERS 0/1
        |     (cur, max, limit, tot) = (1, 4, 0, 22)
        |     (pkts,bits) in = (111, 102056), out = (110, 110272)
        +-> POOL MEMBER mypool/172.16.0.5:http   active,up
        |   |     session enabled    priority 1    ratio 1
        |   |     (cur, max, limit, tot) = (1, 4, 0, 22)
        |   |     (pkts,bits) in = (111, 102056), out = (110, 110272)
        |   |     requests (total) = 0
        +-> POOL MEMBER mypool/172.16.0.9:http   inactive,down
            |     session enabled    priority 0    ratio 1
            |     (cur, max, limit, tot) = (0, 0, 0, 0)
            |     (pkts,bits) in = (0, 0), out = (0, 0)
            |     requests (total) = 0

```

删除上面配置

```
[root@F5:Active] ~ # b virtual myvs delete
[root@F5:Active] ~ # b pool mypool delete

```

## Network

### Interface

```
[root@F5:Active] config # b interface show
INTERFACE
Key     Speed      Pkts  Pkts   Drop Coll   Bits   Bits Errs Trunk
         Mbps        in   out                 in    out
1.1  UP   100 FD 2.186M 71615 1.953M    0 23.92G 191.9M   23
1.2  DN  1000 FD      0     0      0    0      0      0    0
1.3  DN  1000 FD      0     0      0    0      0      0    0
1.4  DN  1000 FD      0     0      0    0      0      0    0
1.5  DN  1000 FD      0     0      0    0      0      0    0
1.6  DN  1000 FD      0     0      0    0      0      0    0
1.7  DN  1000 FD      0     0      0    0      0      0    0
1.8  DN  1000 FD      0     0      0    0      0      0    0
2.1  MS  1000 FD      0     0      0    0      0      0    0
2.2  MS  1000 FD      0     0      0    0      0      0    0
2.3  MS  1000 FD      0     0      0    0      0      0    0
2.4  MS  1000 FD      0     0      0    0      0      0    0
mgmt DN  1000 FD   1431  1506      0    0 2.030M 4.501M    0
[root@F5:Active] config # b interface show all
INTERFACE 1.1   00:01:D7:C3:30:C4   ENABLED   up   MTU 1500
|     media auto (100baseTX full)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (2.186M, 23.92G, 23), out = (71644, 191.9M, 0)
|     (drop,mcast) in = (1.953M, 167381), out = (0, 104)    coll = 0
INTERFACE 1.2   00:01:D7:C3:30:C5   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.3   00:01:D7:C3:30:C6   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.4   00:01:D7:C3:30:C7   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.5   00:01:D7:C3:30:C8   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.6   00:01:D7:C3:30:C9   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.7   00:01:D7:C3:30:CA   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 1.8   00:01:D7:C3:30:CB   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 2.1   00:01:D7:C3:30:CC   ENABLED   unpopulated   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 2.2   00:01:D7:C3:30:CD   ENABLED   unpopulated   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 2.3   00:01:D7:C3:30:CE   ENABLED   unpopulated   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE 2.4   00:01:D7:C3:30:CF   ENABLED   unpopulated   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (tx rx)
|     (pkts,bits,errs) in = (0, 0, 0), out = (0, 0, 0)
|     (drop,mcast) in = (0, 0), out = (0, 0)    coll = 0
INTERFACE mgmt   00:01:D7:C3:30:C1   ENABLED   down   MTU 1500
|     media auto (none)   aggregation: detached
|     flow control tx rx (none)
|     (pkts,bits,errs) in = (1431, 2.030M, 0), out = (1506, 4.501M, 0)
|     (drop,mcast) in = (0, 13), out = (0, 2)    coll = 0

```

### Route

```
[root@F5:Active] config # b route show
ROUTE default inet
|     GATEWAY 192.168.3.1   static
ROUTE 127.1.1.0/24
|     VLAN tmm0   connected
ROUTE 127.10.0.0/16
|     VLAN tmm_bp   connected
ROUTE 172.16.0.0/24
|     VLAN inside   connected
ROUTE 192.168.3.0/24
|     VLAN inside   connected
ROUTE fe80::/64
|     VLAN tmm0   connected
ROUTE fe80::%vlan4093/64
|     VLAN outside   connected
ROUTE fe80::%vlan4094/64
|     VLAN inside   connected
ROUTE fe80::%vlan4095/64
|     VLAN tmm_bp   connected
ROUTE ff02::/64
|     VLAN tmm0   auto
ROUTE ff02::%vlan4093/64
|     VLAN outside   auto
ROUTE ff02::%vlan4094/64
|     VLAN inside   auto
ROUTE ff02::%vlan4095/64
|     VLAN tmm_bp   auto

```

### VLAN

#### create

```
b vlan myvlan interface 1.2 1.3 1.4

```

#### list

```
b vlan list

```

#### delete

```
b vlan myvlan delete

```

### bigpipe arp show

```
[root@F5:Active] config # bigpipe arp show
ARP 10.0.0.41 - A4:BA:DB:E0:7D:EE   VLAN inside   expire 63s   resolved
ARP 10.0.0.51 - F0:4D:A2:2D:BC:C9   VLAN inside   expire 25s   resolved
ARP 172.16.0.5 - 00:25:64:9A:D7:E0   VLAN inside   expire 284s   resolved
ARP 172.16.0.254 - 50:C5:8D:71:B7:00   VLAN inside   expire 299s   resolved
ARP 192.168.2.29 - C0:CB:38:7A:36:B3   VLAN inside   expire 297s   resolved
ARP 192.168.2.179 - 18:E7:F4:1F:45:0E   VLAN inside   expire 289s   resolved
ARP 192.168.2.251 - 00:14:2A:40:8A:58   VLAN inside   expire 297s   resolved
ARP 192.168.3.1 - 50:C5:8D:71:B7:00   VLAN inside   expire 298s   resolved
ARP 192.168.3.2 - 00:1D:09:EF:4E:14   VLAN inside   expire 294s   resolved
ARP 192.168.3.8 - 00:1D:7D:9F:3E:82   VLAN inside   expire 298s   resolved
ARP 192.168.3.91 - B8:AC:6F:25:D3:8C   VLAN inside   expire 299s   resolved
ARP 192.168.3.94 - F0:4D:A2:2D:74:D7   VLAN inside   expire 284s   resolved
ARP 192.168.3.119 - F0:4D:A2:27:CA:3E   VLAN inside   expire 279s   resolved
ARP 192.168.3.124 - 00:25:64:AE:FB:07   VLAN inside   expire 281s   resolved
ARP 192.168.3.169 - B8:AC:6F:41:F6:64   VLAN inside   expire 280s   resolved
ARP 192.168.3.237 - 00:25:64:A7:55:D0   VLAN inside   expire 293s   resolved
ARP 192.168.3.251 - 00:14:2A:40:8A:58   VLAN inside   expire 295s   resolved
ARP 192.168.5.42 - 00:21:9B:70:8D:79   VLAN inside   expire 292s   resolved
ARP 192.168.5.53 - 00:1D:09:78:6E:CB   VLAN inside   expire 281s   resolved
ARP 192.168.5.141 - 00:13:8F:FE:31:D8   VLAN inside   expire 299s   resolved
ARP 192.168.5.245 - 00:1D:09:7B:36:21   VLAN inside   expire 280s   resolved
ARP 192.168.10.253 - 00:0F:E2:85:0B:10   VLAN inside   expire 278s   resolved

```

### b self show

```
[root@F5:Active] config # b self show
SELF 10.0.0.151   mask 0.0.0.0
|     VLAN inside   floating disable
SELF 172.16.0.30   mask 255.255.255.0
|     VLAN inside   floating disable
SELF 192.168.3.18   mask 255.255.255.0
|     VLAN inside   floating disable

```

## System

### b version

```
[root@F5:Active] config # b version
Kernel:
Linux 2.6.18-164.11.1.el5.1.0.f5app
Package:
BIG-IP Version 10.2.0 1707.0
Final Edition

Enabled Features:
Active Directory/Windows Domain Authentication
LDAP Authentication
RADIUS Authentication
SecurID Authentication
Base Endpoint Security Checks
Antivirus Checks
Firewall Checks
Machine Certificate Checks
Protected Workspace
Secure Virtual Keyboard
Network Access
Access Policy Manager Support
Reverse Proxy
Concurrent Sessions (Limited) 10
Rate Shaping and Rate Class Support
Traffic Classification L4
Traffic Classification iRules+L7
Stochastic Fair Queuing Mode
Priority FIFO (ToS) Queuing Mode
QoS and ToS Tagging
Connection Limits
OneConnect - Switching and Pooling
Connection Rebinding
Connection Timeout
Route Pool
Last Hop Pool
Active Active
Failover
Pool Min Up Members
State Mirroring
VLAN Failsafe
HTTP traffic classifier
iSession
iSNAT - Rules Referencing SNAT Pools
Basic Load Balancing
Dynamic Ratio Load Balancing
Fastest Load Balancing
L3 Addr Load Balancing
Least Connection Load Balancing
Least Sessions Load Balancing
Observed Load Balancing
LB Pools Maximum Nodes unlimited
Predictive Load Balancing
Priority Load Balancing
Ratio Load Balancing
Round Robin Load Balancing
UDP Packet Load Balancing
Web Logic Load Balancing
DIAMETER Monitor
EAV Monitor
FTP Monitor
gateway ICMP Monitor
HTTP Monitor
HTTPS Monitor
ICMP Monitor
IMAP Monitor
Inband Monitor
LDAP Monitor
LDAP Over SSL Monitor
Module Score Monitor
Microsoft SQL Monitor
MySQL Monitor
NNTP Monitor
Oracle Monitor
POP3 Monitor
PostgreSQL Monitor
RADIUS Monitor
RealN Monitor
Reverse Keyword
RPC Monitor
Monitor Rules
SASP Monitor
SCRIPTED Monitor
SIP Monitor
SMB Monitor
SMTP Monitor
SNMP Monitor
Soap Monitor
TCP Monitor
TCP Echo Monitor
TCP Half Open Monitor
Transparent Device Monitor
UDP Monitor
Virtual Location Monitor
WAP Monitor
WMI Monitor
Monitors
Network Address Translation
Persistence
Cookie Persistence
Simple Persistence
SIP Persistence
SSL Session ID Persistence
Sticky Persistence
Universal Persistence
WTS Persistence
Pools
HTTP Content Transformation
Fast L4
FTP
HTTP Header Transformation
HTTP
Probe Control - IDS Traffic Management
HTTP Redirection
SIP
TCP
UDP
RAM Cache
RTSP switching
L4 iRules
L7 iRules
User-Defined Statistics
iRules
SCTP support
SNAT Standard
Address Translation
Port Translation
Transparent Device Load Balancing
Access Policy Manager Limited
Local Traffic Manager
IPv6 DNS Support
IPv6 Gateway Module
Interface Mirroring
Spanning Tree Protocol
PVA Enable
SSL Mbps 2000
CMP SSL
CMP SSL per core
SSL Total TPS 5000
Virtual Edition maximum throughput 1
CMP compression per core
HTTP Compression 50
SSL client certificate authorization via LDAP
DDoS Connection Limits
Dynamic Connection Reaping
Packet Filter
SYN Check
SSL Support
SSL Online Certificate Status Protocol
SSL certificate validation via CRLDP

```

### b platform show

```
[root@F5:Active] config # b platform show
PLATFORM INFORMATION --
|     Marketing Name: BIG-IP 3900
|     BIOS revision: F5 Platform: C106 OBJ-0314-03 BIOS (build: 008) Date: 12/28/09
|     base MAC: 00:01:D7:C3:30:C0
|     Physical memory: 7.832GB
+-> SYSTEM INFORMATION C106
|     Type: C106
|     Chassis   serial: f5-dhte-kayv   Level 200 part: 200-0322-01 REV G
+-> CHASSIS
|   |     Max MAC count: 2
+-> HARDWARE INFO
|   +-> cn0
|   |   |     Type: crypto   Model: Cavium NITROX-PX
|   |   |     version: CNPx-MC-SSL-MAIN-0013
|   +-> hsb_lbb0
|   |   |     Type: net   Model: F5 High Speed Bridge LBB device
|   |   |     version: Build: 1.0.12 lab 1
|   +-> cpld
|   |   |     Type: pic   Model: F5 cpld
|   |   |     version: 0x2a
|   +-> mercury2 mainboard
|   |   |     Type: pic   Model: F5 ARM FPGA Loader
|   |   |     ARM FPGA Loader version: 0.09
|   +-> cpus
|   |   |     Type: base board   Model: Intel(R) Xeon(R) CPU           X3220  @ 2.40GHz
|   |   |     cache size: 4096 KB
|   |   |     cores: 4  (cores/cpu:4)
|   |   |     cpu MHz: 2400.134
+-> CPU 0
|   |     Temp: 36degC   Fan speed: 7336rpm
+-> CHASSIS TEMPERATURE
|   |     Air Inlet(1) 26degC   HSBe(2) 31degC
|   |     TMP421 on die(3) 27degC
+-> CHASSIS FAN
|   |     (1) active - 7031rpm   (2) active - 8035rpm
|   |     (3) active - 7670rpm
+-> POWER SUPPLY
|   |     (1) active   (2) not present
+-> LICENSE Local Traffic Mananger 3900 add ons
|   |     Local Traffic Manager Module
|   |     ADD IPV6 GATEWAY
|   |     ADD RATE SHAPING
|   |     ADD RAMCACHE
|   |     50 MBPS COMPRESSION
|   |     SSL 500 TPS Per Core
|   |     ADD SSL CMP
|   |     ADD ANTI-VIRUS CHECKS
|   |     ADD BASE ENDPOINT SECURITY CHECKS
|   |     ADD FIREWALL CHECKS
|   |     ADD NETWORK ACCESS
|   |     ADD SECURE VIRTUAL KEYBOARD
|   |     ADD WEB APP
|   |     ADD MACHINE CERTIFICATE CHECKS
|   |     ADD PROTECTED WORKSPACE

```

### Memory

```
[root@F5:Active] config # b memory show
MEMORY STATISTICS --
|    (Host) Total = 7.827GB   Used = 7.666GB
|    (TMM)  Total = 6.208GB   Used = 108.7MB
|    SUBSYSTEM                        Alloc     Max    Obj size
|     TCP SNACK                       0  0  40
|     TCP lost segment                0  0  40
|     access_session_batch            0  0  80
|     access_session_items            0  0  80
|     access_uri_info                 0  0  4120
|     access_whitelist_uri_entries    61568  61568  104
|     acl (variable)                  4224  4224  1
|     acl_item                        0  0  32
|     auth (variable)                 2336  2336  1
|     cmp (variable)                  256  256  1
|     cn_key                          0  0  1280
|     connflow                        4608  18432  256
|     dedup_xact_op_ctx               0  0  48
|     devbuf (variable)               376352  376352  1
|     dnssec_pkt                      0  0  40
|     dnssec_rrsig                    0  0  4168
|     drop_policy                     704  704  88
|     filter (variable)               360488  389576  1
|     fred_flow_data                  0  0  32
|     http_header_dictionary_cache    0  0  608
|     ifc                             5376  5376  256
|     inst_entry                      0  0  56
|     isession->hst_cache             0  0  312
|     isession_virt_compress_stats    0  0  32
|     laddr                           3872  3872  88
|     leasepool                       0  0  80
|     leasepool_mbr                   0  0  80
|     listener (variable)             832  960  1
|     loop_nexthop                    0  0  80
|     mco db (variable)               40.00M  40.00M  1
|     mds_btree_nodes                 1216  1216  152
|     mds_conn                        0  0  88
|     memcache request items          0  0  320
|     mpi_request                     10304  10528  56
|     neighbor_advertiser_entry       0  1344  64
|     net_ip                          2720  2720  40
|     peer_route                      0  0  104
|     persist                         0  448  112
|     plugin (variable)               0  0  1
|     pool (variable)                  70464   70464  1
|     poolmbr_ratio                   0  0  40
|     pq                              0  0  24
|     profile (variable)              830560  831520  1
|     proxy exclude (variable)        0  0  1
|     proxy_connect_data              0  576  144
|     proxy_oob                       0  0  40
|     pva                             0  0  192
|     ramcache (variable)             1.240M  1.256M  1
|     ramcache entity                 2600  3016  104
|     rate shaper (variable)          0  0  1
|     rateclass_queue                 0  0  144
|     red_cb                          0  0  88
|     resolv (variable)               896  896  1
|     resolv_query                    0  0  104
|     rt_entry                        9216  9728  128
|     rules (variable)                5312  5312  1
|     session                         320  320  160
|     shaper_domain                   96  96  24
|     snat                            0  0  64
|     ssl_basic                       0  0  1928
|     ssl_hs                          0  0  17048
|     ssl_profile                     0  0  4320
|     ssl_session                     0  0  200
|     sso (variable)                  4224  4224  1
|     string cache (variable)         262336  262336  1
|     tcl_ip_addr                     320  640  40
|     temp (variable)                 529824  532056  1
|     traffic class tables            0  0  4408
|     umem (variable)                 1.930M  1.930M  1
|     wa_resource_item                0  0  112
|     web_application                 0  0  88
|     xfrag                           41.37M  46.49M  2048
|     web_application_item            0  0  32
|     web application (variable)      4224  4224  1
|     vaddr                           4320  4608  72
|     tunnel_nexthop                  0  0  64
|     traffic class                   0  0  72
|     tcl_strcache                    128  128  32
|     tcl (variable)                  5.534M  5.535M  1
|     sso_config                      0  0  128
|     ssl_shim                        0  0  136
|     ssl_rd                          0  0  40
|     ssl_keys                        0  0  1688
|     ssl_cn                          0  0  760
|     ssl (variable)                  4.347M  4.371M  1
|     shared_var_context              256  256  64
|     session (variable)              672  672  1
|     selfip                          3200  3200  80
|     rtm_internal                    128  2432  128
|     rt_dom                          0  0  48
|     resolv_cache                    0  0  64
|     regex (variable)                0  0  1
|     rateshaper                      0  0  32
|     rateclass                       0  0  320
|     ramcache resource               1800  2088  72
|     ramcache document               5600  6496  224
|     queueing_method                 576  576  72
|     proxy_tuple                     0  0  40
|     proxy_ctx                       0  352  88
|     proxy_common_cache              0  0  176
|     proxy (variable)                43840  45440  1
|     private (variable)              0  0  1
|     poolprio                        0  0  56
|     poolmbr                         18496  18496  272
|     pool                            5920  5920  296
|     persistence (variable)          0  0  1
|     peer_woc                        0  0  232
|     packet                          395520  765312  192
|     neighbor_entry                  28896   65632  112
|     nat                             0  0  24
|     method (variable)               0  0  1
|     memcache (variable)             160  160  1
|     mds_cache                       0  0  2416
|     mcp (variable)                  6888  106312  1
|     mac_entry                       0  0  32
|     local_route                     0  0  104
|     listener                        153600  154880  320
|     leasepool (variable)            1.016M  1.016M  1
|     lasthop                         1792  2880  64
|     isession_virt_stat              0  0  32
|     isession_abort_stat             0  0  32
|     isession (variable)             147776  147776  1
|     ifnet (variable)                5.904M  5.904M  1
|     http_persist                    0  0  72
|     http_data                       0  5656  808
|     fred_cb                         0  0  40
|     errdefs (variable)              0  0  1
|     dnssec_sig_cache                0  0  64
|     dnssec_rrset                    0  0  48
|     dns_session                     0  0  32
|     deflate (variable)              0  0  1
|     dedup (variable)                0  0  1
|     cn_req                          0  3424  856
|     cn_io                           0  0  1168
|     cipher_rsa_io                   0  0  2320
|     address_entry                   0  0  40
|     acl_entry                       0  0  112
|     acl                             0  0  56
|     access_uuid_entries             0  0  48
|     access_session_variables        0  0  288
|     access_session_data             0  0  80
|     TCP segment                     0  12864  64
|     TCP SYN cache                   0  288  72
|     TCP SACK                        0  120  40
|     CallFrame                       704  704  176

```

### 查看连接数

```

[root@F5:Active] config # b conn show
172.16.0.8:50168 <-> 192.168.3.18:ssh <-> 192.168.3.18:ssh   tcp 1/2
172.16.0.30:49616 <-> any%65535 <-> 172.16.0.9:http   tcp 1/0
192.168.3.18:42889 <-> any%65535 <-> 192.168.3.9:http   tcp 1/1
192.168.3.18:43009 <-> any%65535 <-> 10.0.0.51:http   tcp 1/1
192.168.3.78:4757 <-> 192.168.3.18:ssh <-> 192.168.3.18:ssh   tcp 1/3
192.168.3.78:4763 <-> 192.168.3.18:ssh <-> 192.168.3.18:ssh   tcp 1/1

```

## config

### list

```
[root@F5:Active] ~ # b list
datastor {
   low water mark 80
   high water mark 90
}
deduplication {}
shell write partition Common
route default inet {
   gateway 192.168.3.1
}
profile http my_HTTP_1_http_profile {
   defaults from http-lan-optimized-caching
   compress content type include {
      "text/"
      "application/(xml|x-javascript)"
      "application/pdf"
   }
}
profile persist my_HTTP_1_persist_profile {
   defaults from cookie
   mode cookie
}
profile tcp my_HTTP_1_lan-optimized_tcp_profile {
   defaults from tcp-lan-optimized
}
node 10.0.0.41 {}
node 10.0.0.51 {}
node 172.16.0.5 {}
node 172.16.0.9 {}
node 172.16.0.10 {}
node 172.16.0.11 {}
node 172.16.0.12 {}
node 192.168.3.5 {}
node 192.168.3.9 {}
node 192.168.3.10 {}
pool Pool-Http {
   monitor all http
   members {
      10.0.0.41:http {}
      10.0.0.51:http {}
   }
}
pool my_HTTP_1_pool {
   monitor all http
   members {
      192.168.3.5:http {
         priority 1
      }
      192.168.3.9:http {
         priority 1
      }
      192.168.3.10:http {
         priority 1
      }
   }
}
pool mypool {
   monitor all http
   members {
      172.16.0.5:http {
         priority 1
      }
      172.16.0.9:http {
         priority 1
      }
   }
}
pool neo {
   monitor all http
   members {
      172.16.0.5:http {
         monitor http
      }
      172.16.0.9:http {}
   }
}
virtual TEST_HTTP {
   snat automap
   pool neo
   destination 172.16.0.25:http
   ip protocol tcp
   profiles {
      http-lan-optimized-caching {}
      tcp {}
   }
}
virtual VS-HTTP {
   snat automap
   pool Pool-Http
   destination 192.168.3.21:http
   ip protocol tcp
   persist source_addr
   profiles {
      http-lan-optimized-caching {}
      tcp {}
   }
}
virtual my_HTTP_1_virtual_server {
   snat automap
   pool my_HTTP_1_pool
   destination 192.168.3.11:http
   ip protocol tcp
   persist my_HTTP_1_persist_profile
   profiles {
      my_HTTP_1_http_profile {}
      my_HTTP_1_lan-optimized_tcp_profile {}
   }
}
virtual myvs {
   snat automap
   pool mypool
   destination 192.168.3.22:http
   ip protocol tcp
   persist source_addr
   profiles {
      http-wan-optimized-compression-caching {}
      tcp {}
   }
}

```

### export

```
[root@test:Active] config # b export /tmp/test.txt

```

```
[root@test:Active] config # cat /tmp/test.txt.scf
provision apm {}
provision asm {}
provision gtm {}
provision lc {}
provision ltm {
   level nominal
}
provision psm {}
provision wam {
   level nominal
}
provision wom {}
provision woml {}
mgmt 192.168.1.245 {
   netmask 255.255.255.0
}
trunk trunk_1-2 {
   interfaces {
      1.1
      1.2
   }
}
stp instance 0 {
   trunks trunk_1-2 {
         external path cost 20000
         internal path cost 20000
      }
   vlans Internal
}
self allow {
   default {
      tcp ssh
      tcp domain
      tcp snmp
      tcp https
      tcp f5-iquery
      udp domain
      udp snmp
      udp efs
      udp cap
      udp f5-iquery
      udp 12400
      udp 12402
      udp 12406
      proto ospf
   }
}
partition Common {
   description "Repository for system objects and shared objects."
}
shell write partition Common
vlan Internal {
   tag 4094
   trunks trunk_1-2
}
self 172.16.0.4 {
   netmask 255.255.255.0
   vlan Internal
   allow all
}
user root {
   password crypt "$1$uNkiFcga$OiOWGbn5Kh58mJTNh1IIl0"
}
user admin {
   password crypt "$1$mZxbi34f$N8nxG2XDZtMG2esku1e1U/"
   group 500
   home "/home/admin"
   shell "/bin/false"
   role administrator in all
}
failover {
   standby link down time 0
}
ntp {
   timezone "Asia/Hong_Kong"
}
system {
   gui setup disable
   hostname "test.f5.com"
   mgmt dhcp disable
}
#  No partition
datastor {
   low water mark 80
   high water mark 90
}
deduplication {}
shell write partition Common
route default inet {
   gateway 172.16.0.254
}
monitor my_HTTP_user_monitor {
   defaults from http
   interval 30
   timeout 91
}
profile httpclass httpclass {
   pool none
   redirect none
   url rewrite none
   asm disable
   wa enable
   hosts none
   paths none
   headers none
   cookies none
}
profile httpclass httpclass_new {
   defaults from httpclass
   pool none
   redirect none
   wa disable
   hosts none
   paths none
   headers none
   cookies none
}
profile http http_new {
   defaults from http-wan-optimized-compression-caching
   ramcache enable
   ramcache size 300mb
   ramcache max entries 10000
   ramcache max age 86400
   ramcache min object size 0
   ramcache max object size 2M
   ramcache ignore client cache control all
   ramcache aging rate 9
   ramcache insert age header enable
   ramcache uri exclude none
   ramcache uri include none
   ramcache uri pinned none
}
profile http my_HTTP_nginx_http_profile {
   defaults from http-wan-optimized-compression
   compress content type include {
      "text/"
      "application/vnd.ms-publisher"
      "application/(xml|x-javascript|javascript|x-ecmascript|ecmascript)"
      "application/(word|doc|msword|winword|ms-word|x-word|x-msword|vnd.word|vnd.msword|vnd.ms-word)"
      "application/(xls|excel|msexcel|ms-excel|x-excel|x-xls|xmsexcel|x-ms-excel|vnd.excel|vnd.msexcel|vnd.ms-excel)"
      "application/(powerpoint|mspowerpoint|ms-powerpoint|x-powerpoint|x-mspowerpoint|vnd.powerpoint|vnd.mspowerpoint |vnd.ms-powerpoint|vnd.ms-pps)"
      "application/(mpp|msproject|x-msproject|x-ms-project|vnd.ms-project)"
      "application/(visio|x-visio|vnd.visio|vsd|x-vsd|x-vsd)"
      "application/(pdf|x-pdf|acrobat|vnd.pdf)"
   }
}
profile http my_HTTP_user_http_profile {
   defaults from http-wan-optimized-compression
   compress content type include {
      "text/"
      "application/vnd.ms-publisher"
      "application/(xml|x-javascript|javascript|x-ecmascript|ecmascript)"
      "application/(word|doc|msword|winword|ms-word|x-word|x-msword|vnd.word|vnd.msword|vnd.ms-word)"
      "application/(xls|excel|msexcel|ms-excel|x-excel|x-xls|xmsexcel|x-ms-excel|vnd.excel|vnd.msexcel|vnd.ms-excel)"
      "application/(powerpoint|mspowerpoint|ms-powerpoint|x-powerpoint|x-mspowerpoint|vnd.powerpoint|vnd.mspowerpoint |vnd.ms-powerpoint|vnd.ms-pps)"
      "application/(mpp|msproject|x-msproject|x-ms-project|vnd.ms-project)"
      "application/(visio|x-visio|vnd.visio|vsd|x-vsd|x-vsd)"
      "application/(pdf|x-pdf|acrobat|vnd.pdf)"
   }
}
profile persist my_HTTP_nginx_persist_profile {
   defaults from cookie
   mode cookie
}
profile persist my_HTTP_user_persist_profile {
   defaults from cookie
   mode cookie
}
profile tcp my_HTTP_nginx_lan-optimized_tcp_profile {
   defaults from tcp-lan-optimized
}
profile tcp my_HTTP_nginx_wan-optimized_tcp_profile {
   defaults from tcp-wan-optimized
}
profile tcp my_HTTP_user_lan-optimized_tcp_profile {
   defaults from tcp-lan-optimized
}
profile tcp my_HTTP_user_wan-optimized_tcp_profile {
   defaults from tcp-wan-optimized
}
profile tcp tcp-lan {
   defaults from tcp-lan-optimized
   keep alive interval 1200
}
profile tcp tcp-wan {
   defaults from tcp-wan-optimized
   keep alive interval 1200
}
node 10.0.0.24 {}
node 10.0.0.25 {}
node 10.0.0.26 {}
node 10.0.0.31 {
   session user disabled
}
node 10.0.0.68 {}
node 10.0.0.69 {}
node 172.16.0.5 {}
node 172.16.0.6 {}
node 172.16.0.22 {
   session user disabled
}
node 172.16.0.23 {
   session user disabled
}
node 192.168.80.197 {
   session user disabled
}
pool my_HTTP_nginx_pool {
   lb method member least conn
   monitor all http
   members {
      10.0.0.68:http {}
      10.0.0.69:http {}
      172.16.0.5:http {
         priority 1
         session user disabled
      }
      172.16.0.6:http {
         priority 1
         session user disabled
      }
   }
}
pool my_HTTP_user_pool {
   lb method member least conn
   monitor all my_HTTP_user_monitor
   members {
      10.0.0.24:http {
         priority 1
      }
      10.0.0.25:http {
         priority 1
      }
      10.0.0.26:http {
         priority 1
      }
   }
}
pool neo-nginx {
   lb method least conn
   monitor all http
   members {
      172.16.0.5:http {}
      172.16.0.6:http {}
   }
}
virtual my_HTTP_nginx_virtual_server {
   snat automap
   pool my_HTTP_nginx_pool
   destination 172.16.0.50:http
   ip protocol tcp
   persist my_HTTP_nginx_persist_profile
   profiles {
      my_HTTP_nginx_http_profile {}
      my_HTTP_nginx_lan-optimized_tcp_profile {
         serverside
      }
      my_HTTP_nginx_wan-optimized_tcp_profile {
         clientside
      }
   }
}
virtual my_HTTP_user_virtual_server {
   snat automap
   pool my_HTTP_user_pool
   destination 172.16.0.51:http
   ip protocol tcp
   persist my_HTTP_user_persist_profile
   profiles {
      my_HTTP_user_http_profile {}
      my_HTTP_user_lan-optimized_tcp_profile {
         serverside
      }
      my_HTTP_user_wan-optimized_tcp_profile {
         clientside
      }
   }
}
virtual neo-nginx-vs {
   pool neo-nginx
   destination 172.16.0.52:http
   ip protocol tcp
   profiles {
      http-wan-optimized-compression-caching {}
      tcp {}
   }
}

```

### import

```
[root@test:Active] config # b import /tmp/test.txt.scf
Saving configuration to /var/local/scf/.backup-0.scf.
Reading configuration from /config/low_profile_base.conf.
Reading configuration from /defaults/config_base.conf.
Reading configuration from /usr/share/monitors/base_monitors.conf.
Reading configuration from /config/profile_base.conf.
Reading configuration from /config/daemon.conf.
Reading configuration from /tmp/test.txt.scf.
Renaming /var/local/scf/.backup-0.scf to /var/local/scf/backup.scf.
Loading the configuration ...

```

## 第 32 章 utility

## bigtop - bigtop is a BIG-IP live statistics display utility.

```
[root@F5:Active] config # bigtop
                     |  bits  since       |  bits  in prior    |  current
                     |  Jan  6 14:07:10   |  4 seconds         |  time
BIG-IP      ACTIVE   |---In----Out---Conn-|---In----Out---Conn-|  17:26:29
F5.3900.Test          326.9M 888.6M  59721   3632   2944      3

VIRTUAL ip:port      |---In----Out---Conn-|---In----Out---Conn-|-Nodes Up--
192.168.3.19:http     1.636M 20.14M    101      0      0      0      0
172.16.0.25:http       45424  14784     15      0      0      0      0

NODE ip:port         |---In----Out---Conn-|---In----Out---Conn-|--State----
10.0.0.51:http        441280 5.464M     20      0      0      0 DOWN
10.0.0.41:http        115216 498040      9      0      0      0 DOWN
172.16.0.5:http            0      0      0      0      0      0 DOWN
172.16.0.9:http            0      0      0      0      0      0 DOWN
192.168.3.9:http           0      0      0      0      0      0 DOWN

```

## qkview - grab diagnostic information from an BIG-IP system.

```
[root@test:Active] config # qkview
Gathering System Diagnostics: Please wait ...
Diagnostic information has been saved in:
/var/tmp/test.f5.com.tgz
Please send this file to F5 support.

```

## tmstat

```
 CPU:   4% busy     1% idle   95% sleep                                                                                                       Fri Jan  7 17:27:28 2011

       Memory Allocated                                                                                                             Packets              53,172 Polls
   28,684,904 / 1,665,138,688                                                                                                New Flows   Old Flows
  [  .  :  .  |  .  :  .  ]                                                                                                          0           7

 Memory Count Object

  105MB   2-- (total)
   41MB    -- mco db (variable)
    6MB    -- ifnet (variable)
    6MB    -- tcl (variable)
    5MB    -- ssl (variable)
    2MB    -- umem (variable)
    2MB    -- ramcache (variable)
    2MB    -- leasepool (variable)
  812kB    -- profile (variable)
  519kB    -- temp (variable)
  387kB    3k packet
  368kB    -- devbuf (variable)
  353kB    -- filter (variable)
  257kB    -- string cache (variable)
  151kB   480 listener
  145kB    -- isession (variable)                                                                                                             0.0
   69kB    -- pool (variable)                                                                                                        0b tx        link             0b rx
   61kB   592 access_whitelist_uri_entries                                                                            [ . : . | . : . ]       0.1      [ . : . | . : . ]
   43kB    -- proxy (variable)                                                                                                   6,560b tx 10,000 link        14,368b rx
   32kB   286 neighbor_entry                                                                                          [ . : . | . : . ]       0.2      [ . : . | . : . ]
   19kB    68 poolmbr                                                                                                            8,912b tx 10,000 link        14,168b rx
   11kB   184 mpi_request                                                                                             [ . : . | . : . ]       0.3      [ . : . | . : . ]
   10kB    72 rt_entry                                                                                                           8,728b tx 10,000 link         7,104b rx
    7kB    -- mcp (variable)                                                                                          [ . : . | . : . ]       0.4      [ . : . | . : . ]
    6kB    20 pool                                                                                                               6,560b tx 10,000 link         6,560b rx
    6kB    22 connflow                                                                                                [ . : . | . : . ]       1.1      [ . : . | . : . ]
    6kB    25 ramcache document                                                                                                  4,904b tx      0 link         9,072b rx
    6kB    21 ifc                                                                                                     [ . : . | . : . ]       1.2      [ . : . | . : . ]
    6kB    -- rules (variable)                                                                                                       0b tx      0 link             0b rx
    5kB    60 vaddr                                                                                                   [ . : . | . : . ]       1.3      [ . : . | . : . ]
    5kB    -- acl (variable)                                                                                                         0b tx      0 link             0b rx
    5kB    -- sso (variable)                                                                                          [ . : . | . : . ]       1.4      [ . : . | . : . ]
    5kB    -- web application (variable)                                                                                             0b tx      0 link             0b rx
    4kB    44 laddr                                                                                                   [ . : . | . : . ]       1.5      [ . : . | . : . ]
   22kB    -- (unseen)                                                                                                               0b tx      0 link             0b rx

```

## physmem

```
[root@F5:Active] config # physmem -m
8192

```