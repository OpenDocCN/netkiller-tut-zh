# 部分 I. System Administrator

## 第 3 章 获取系统信息

## 1. Distribution information

To find your Ubuntu version: lsb_release -a

```

[root@localhost ~]# lsb_release -a
LSB Version:    :core-3.1-ia32:core-3.1-noarch:graphics-3.1-ia32:graphics-3.1-noarch
Distributor ID: CentOS
Description:    CentOS release 5.2 (Final)
Release:        5.2
Codename:       Final

neo@netkiller:~$ lsb_release -a

No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 8.04.1
Release:        8.04
Codename:       hardy

```

```
			$ head -n1 /etc/issue
			Ubuntu 10.04 LTS \n \l

```

## 2. System Infomation

### 2.1. Cpu Bit

```
				neo@netkiller:~$ uname -a
				Linux netkiller 2.6.28-15-server #52-Ubuntu SMP Wed Sep 9 11:34:09 UTC 2009 x86_64 GNU/Linux

				neo@netkiller:~$ getconf LONG_BIT
				64

```

## 3. shutdown

```
			shutdown -h now
			shutdown -h 10:00 10 点关机
			shutdown -h +10 10mins 后关机
			shutdown -r now reboot at once
			shutdown -r +30 'System will reboot in 30mins'
			shutdown -k 'System will reboot'

```

## 4. Profile

### 4.1. shell

```
				$ chsh /bin/bash

```

## 5. 环境默认值

### alternatives - maintain symbolic links determining default commands

### 5.1. 显示所有配置项

```

[root@localhost ~]# alternatives --list
libwbclient.so.0.14-64	auto	/usr/lib64/samba/wbclient/libwbclient.so.0.14
ld	auto	/usr/bin/ld.bfd
cups_backend_smb	auto	/usr/bin/smbspool
mta	auto	/usr/sbin/sendmail.postfix
libnssckbi.so.x86_64	auto	/usr/lib64/pkcs11/p11-kit-trust.so
jre_1.8.0	auto	/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64/jre
jre_1.8.0_openjdk	auto	/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
pgsql-ld-conf	auto	/usr/pgsql-11/share/postgresql-11-libs.conf
dockerd	auto	/usr/bin/dockerd-ce
java	auto	/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64/jre/bin/java
jre_openjdk	auto	/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64/jre
jre_11	auto	/usr/lib/jvm/java-11-openjdk-11.0.3.7-0.el7_6.x86_64
jre_11_openjdk	auto	/usr/lib/jvm/jre-11-openjdk-11.0.3.7-0.el7_6.x86_64

```

### 5.2. 切换版本

```

[root@localhost ~]# alternatives --config java

There are 3 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
   1           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.201.b09-2.el7_6.x86_64/jre/bin/java)
*+ 2           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64/jre/bin/java)
   3           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.3.7-0.el7_6.x86_64/bin/java)

Enter to keep the current selection[+], or type selection number: 3		

```

输入数字 3，切换到 Java 11

```

[root@localhost ~]# java -version
openjdk version "11.0.3" 2019-04-16 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.3+7-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.3+7-LTS, mixed mode, sharing)			

```

### 5.3. 使用 alternatives 管理自己的软件版本

下面 nodejs 是编译版本，我们需要使用 alternatives 来管理版本

```

alternatives --install /usr/local/bin/node node /srv/node-v12.3.1/bin/node  100			

```

查看 node

```

[root@localhost ~]# alternatives --display node
node - status is auto.
 link currently points to /srv/node-v12.3.1/bin/node
/srv/node-v12.3.1/bin/node - priority 100
Current `best' version is /srv/node-v12.3.1/bin/node.			

```

删除 node

```

[root@localhost ~]# alternatives --remove node /srv/node-v12.3.1/bin/node
[root@localhost ~]# alternatives --display node

```

## 第 4 章 Kernel

摘要

## 1. 编译安装内核

```

wget -q -c http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.0.1.tar.bz2
tar jxvf linux-3.0.1.tar.bz2

cd linux-3.0.1
make clean
make mrproper
make menuconfig
make
make modules_install
make install

```

## 2. sysctl - configure kernel parameters at runtime

### 2.1. sysctl.d

```

$ ls /etc/sysctl.d/		
$ cat /etc/sysctl.d/30-postgresql-shm.conf

```

### 2.2. vm.overcommit_memory

内存与交换分区分配相关

https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Performance_Tuning_Guide/s-memory-captun.html

```

vm.overcommit_memory = 1

```

### 2.3. TCP 拥塞控制算法

https://github.com/google/bbr

2017 年之后已经集成近 linux 内核

查看当前算法

```

neo@netkiller ~ % sudo sysctl -a | egrep "net.ipv4.tcp_congestion_control|net.core.default_qdisc"
net.core.default_qdisc = fq_codel
net.ipv4.tcp_congestion_control = cubic

neo@netkiller ~ % cat /proc/sys/net/ipv4/tcp_congestion_control
cubic

```

确认内核已经含有 tcp_bbr 模块

```

root@netkiller ~ % lsmod | grep tcp_bbr
tcp_bbr                20480  1			

```

切换到 bbr 算法

```
			：
sudo -s
sysctl -w "net.core.default_qdisc=fq"
sysctl -w "net.ipv4.tcp_congestion_control=bbr"

```

切回 cubic

```

sysctl -w "net.core.default_qdisc=fq_codel"
sysctl -w "net.ipv4.tcp_congestion_control=cubic"

```

写入 /etc/sysctl.conf 文件

```

echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf

sudo sysctl -p			

```

## 3. /sys

### 3.1. /sys/class/net/

```

$ cat /sys/class/net/eth0/statistics/rx_bytes
$ cat /sys/class/net/eth0/statistics/tx_bytes

```

## 4. /proc

### 4.1. 查看系统版本

```

[root@localhost ~]# cat /proc/version
Linux version 3.10.0-693.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) ) #1 SMP Tue Aug 22 21:09:27 UTC 2017

```

### 4.2. 进程内存监控

/proc/进程 id/smaps

```

# cat /proc/1/smaps

```

查看进程使用交换分区的情况

```

# awk '/^Swap:/ {SWAP+=$2}END{print SWAP" KB"}' /proc/25020/smaps
532 KB

```

### 4.3. ulimit 状态

通过下面命令查看 ulimit 是否对进程起作用。/proc/{pid}/limits pid 是进程 ID

```

# cat /proc/25810/limits

Limit                     Soft Limit           Hard Limit           Units     
Max cpu time              unlimited            unlimited            seconds   
Max file size             unlimited            unlimited            bytes     
Max data size             unlimited            unlimited            bytes     
Max stack size            8388608              unlimited            bytes     
Max core file size        0                    unlimited            bytes     
Max resident set          unlimited            unlimited            bytes     
Max processes             126870               126870               processes 
Max open files            1024                 4096                 files     
Max locked memory         65536                65536                bytes     
Max address space         unlimited            unlimited            bytes     
Max file locks            unlimited            unlimited            locks     
Max pending signals       126870               126870               signals   
Max msgqueue size         819200               819200               bytes     
Max nice priority         0                    0                    
Max realtime priority     0                    0                    
Max realtime timeout      unlimited            unlimited            us  

```

## 第 5 章 Kernel modules

add and remove modules from the Linux Kernel

## 1. modprobe - program to add and remove modules from the Linux Kernel

```
$ modprobe -l
kernel/arch/x86/kernel/cpu/mcheck/mce-inject.ko
kernel/arch/x86/kernel/cpu/cpufreq/powernow-k8.ko
kernel/arch/x86/kernel/cpu/cpufreq/mperf.ko
kernel/arch/x86/kernel/cpu/cpufreq/acpi-cpufreq.ko
kernel/arch/x86/kernel/cpu/cpufreq/pcc-cpufreq.ko
kernel/arch/x86/kernel/cpu/cpufreq/speedstep-lib.ko
kernel/arch/x86/kernel/cpu/cpufreq/p4-clockmod.ko
kernel/arch/x86/kernel/test_nx.ko
kernel/arch/x86/kernel/microcode.ko
kernel/arch/x86/crypto/fpu.ko
kernel/arch/x86/crypto/aes-x86_64.ko
kernel/arch/x86/crypto/twofish-x86_64.ko
kernel/arch/x86/crypto/salsa20-x86_64.ko
kernel/arch/x86/crypto/aesni-intel.ko
kernel/arch/x86/crypto/ghash-clmulni-intel.ko
kernel/arch/x86/crypto/crc32c-intel.ko
kernel/arch/x86/kvm/kvm.ko
kernel/arch/x86/kvm/kvm-intel.ko
kernel/arch/x86/kvm/kvm-amd.ko
kernel/kernel/trace/ring_buffer_benchmark.ko
kernel/mm/hwpoison-inject.ko
kernel/fs/nfs_common/nfs_acl.ko
kernel/fs/nls/nls_cp737.ko
kernel/fs/nls/nls_cp775.ko
kernel/fs/nls/nls_cp850.ko
kernel/fs/nls/nls_cp852.ko
kernel/fs/nls/nls_cp855.ko
kernel/fs/nls/nls_cp857.ko
kernel/fs/nls/nls_cp860.ko
kernel/fs/nls/nls_cp861.ko
kernel/fs/nls/nls_cp862.ko
kernel/fs/nls/nls_cp863.ko
kernel/fs/nls/nls_cp864.ko
kernel/fs/nls/nls_cp865.ko
kernel/fs/nls/nls_cp866.ko
kernel/fs/nls/nls_cp869.ko
kernel/fs/nls/nls_cp874.ko
kernel/fs/nls/nls_cp932.ko
kernel/fs/nls/nls_euc-jp.ko
kernel/fs/nls/nls_cp936.ko
kernel/fs/nls/nls_cp949.ko
kernel/fs/nls/nls_cp950.ko
kernel/fs/nls/nls_cp1250.ko
kernel/fs/nls/nls_cp1251.ko
kernel/fs/nls/nls_iso8859-1.ko
kernel/fs/nls/nls_iso8859-2.ko
kernel/fs/nls/nls_iso8859-3.ko
kernel/fs/nls/nls_iso8859-4.ko
kernel/fs/nls/nls_iso8859-5.ko
kernel/fs/nls/nls_iso8859-6.ko
kernel/fs/nls/nls_iso8859-7.ko
kernel/fs/nls/nls_cp1255.ko
kernel/fs/nls/nls_iso8859-9.ko
kernel/fs/nls/nls_iso8859-13.ko
kernel/fs/nls/nls_iso8859-14.ko
kernel/fs/nls/nls_iso8859-15.ko
kernel/fs/nls/nls_koi8-r.ko
kernel/fs/nls/nls_koi8-u.ko
kernel/fs/nls/nls_koi8-ru.ko
kernel/fs/nls/nls_utf8.ko
kernel/fs/mbcache.ko
kernel/fs/configfs/configfs.ko
kernel/fs/dlm/dlm.ko
kernel/fs/fscache/fscache.ko
kernel/fs/ext3/ext3.ko
kernel/fs/ext2/ext2.ko
kernel/fs/ext4/ext4.ko
kernel/fs/jbd/jbd.ko
kernel/fs/jbd2/jbd2.ko
kernel/fs/cramfs/cramfs.ko
kernel/fs/squashfs/squashfs.ko
kernel/fs/fat/fat.ko
kernel/fs/fat/vfat.ko
kernel/fs/fat/msdos.ko
kernel/fs/ecryptfs/ecryptfs.ko
kernel/fs/nfs/nfs.ko
kernel/fs/nfs/nfs_layout_nfsv41_files.ko
kernel/fs/exportfs/exportfs.ko
kernel/fs/nfsd/nfsd.ko
kernel/fs/lockd/lockd.ko
kernel/fs/cifs/cifs.ko
kernel/fs/jffs2/jffs2.ko
kernel/fs/ubifs/ubifs.ko
kernel/fs/autofs4/autofs4.ko
kernel/fs/fuse/fuse.ko
kernel/fs/fuse/cuse.ko
kernel/fs/udf/udf.ko
kernel/fs/xfs/xfs.ko
kernel/fs/cachefiles/cachefiles.ko
kernel/fs/btrfs/btrfs.ko
kernel/fs/gfs2/gfs2.ko
kernel/crypto/seqiv.ko
kernel/crypto/vmac.ko
kernel/crypto/xcbc.ko
kernel/crypto/crypto_null.ko
kernel/crypto/md4.ko
kernel/crypto/rmd128.ko
kernel/crypto/rmd160.ko
kernel/crypto/rmd256.ko
kernel/crypto/rmd320.ko
kernel/crypto/sha256_generic.ko
kernel/crypto/sha512_generic.ko
kernel/crypto/wp512.ko
kernel/crypto/tgr192.ko
kernel/crypto/gf128mul.ko
kernel/crypto/ecb.ko
kernel/crypto/cbc.ko
kernel/crypto/pcbc.ko
kernel/crypto/cts.ko
kernel/crypto/lrw.ko
kernel/crypto/xts.ko
kernel/crypto/ctr.ko
kernel/crypto/gcm.ko
kernel/crypto/ccm.ko
kernel/crypto/cryptd.ko
kernel/crypto/des_generic.ko
kernel/crypto/fcrypt.ko
kernel/crypto/blowfish.ko
kernel/crypto/twofish_common.ko
kernel/crypto/serpent.ko
kernel/crypto/aes_generic.ko
kernel/crypto/camellia.ko
kernel/crypto/cast5.ko
kernel/crypto/cast6.ko
kernel/crypto/arc4.ko
kernel/crypto/tea.ko
kernel/crypto/khazad.ko
kernel/crypto/anubis.ko
kernel/crypto/seed.ko
kernel/crypto/deflate.ko
kernel/crypto/zlib.ko
kernel/crypto/michael_mic.ko
kernel/crypto/authenc.ko
kernel/crypto/lzo.ko
kernel/crypto/ansi_cprng.ko
kernel/crypto/tcrypt.ko
kernel/crypto/ghash-generic.ko
kernel/crypto/xor.ko
kernel/crypto/async_tx/async_tx.ko
kernel/crypto/async_tx/async_memcpy.ko
kernel/crypto/async_tx/async_xor.ko
kernel/crypto/async_tx/async_pq.ko
kernel/crypto/async_tx/async_raid6_recov.ko
kernel/drivers/pci/pcie/aer/aer_inject.ko
kernel/drivers/pci/hotplug/acpiphp_ibm.ko
kernel/drivers/pci/hotplug/shpchp.ko
kernel/drivers/pci/hotplug/fakephp.ko
kernel/drivers/video/backlight/lcd.ko
kernel/drivers/video/backlight/platform_lcd.ko
kernel/drivers/video/backlight/progear_bl.ko
kernel/drivers/video/backlight/mbp_nvidia_bl.ko
kernel/drivers/video/backlight/wm831x_bl.ko
kernel/drivers/video/display/display.ko
kernel/drivers/video/vgastate.ko
kernel/drivers/video/fb_ddc.ko
kernel/drivers/video/riva/rivafb.ko
kernel/drivers/video/nvidia/nvidiafb.ko
kernel/drivers/video/aty/atyfb.ko
kernel/drivers/video/aty/aty128fb.ko
kernel/drivers/video/aty/radeonfb.ko
kernel/drivers/video/macmodes.ko
kernel/drivers/video/via/viafb.ko
kernel/drivers/video/savage/savagefb.ko
kernel/drivers/video/cirrusfb.ko
kernel/drivers/video/sm501fb.ko
kernel/drivers/video/vga16fb.ko
kernel/drivers/video/vfb.ko
kernel/drivers/video/output.ko
kernel/drivers/idle/i7300_idle.ko
kernel/drivers/acpi/apei/einj.ko
kernel/drivers/acpi/apei/erst-dbg.ko
kernel/drivers/acpi/video.ko
kernel/drivers/acpi/sbshc.ko
kernel/drivers/acpi/sbs.ko
kernel/drivers/acpi/power_meter.ko
kernel/drivers/acpi/acpi_pad.ko
kernel/drivers/xen/evtchn.ko
kernel/drivers/xen/xenfs/xenfs.ko
kernel/drivers/regulator/fixed.ko
kernel/drivers/regulator/userspace-consumer.ko
kernel/drivers/regulator/bq24022.ko
kernel/drivers/regulator/lp3971.ko
kernel/drivers/regulator/max1586.ko
kernel/drivers/regulator/wm831x-dcdc.ko
kernel/drivers/regulator/wm831x-isink.ko
kernel/drivers/regulator/wm831x-ldo.ko
kernel/drivers/regulator/wm8350-regulator.ko
kernel/drivers/regulator/wm8400-regulator.ko
kernel/drivers/regulator/ab3100.ko
kernel/drivers/regulator/tps65023-regulator.ko
kernel/drivers/regulator/tps6507x-regulator.ko
kernel/drivers/char/hw_random/timeriomem-rng.ko
kernel/drivers/char/hw_random/intel-rng.ko
kernel/drivers/char/hw_random/amd-rng.ko
kernel/drivers/char/hw_random/via-rng.ko
kernel/drivers/char/hw_random/virtio-rng.ko
kernel/drivers/char/pcmcia/ipwireless/ipwireless.ko
kernel/drivers/char/pcmcia/cm4000_cs.ko
kernel/drivers/char/pcmcia/cm4040_cs.ko
kernel/drivers/char/tpm/tpm_nsc.ko
kernel/drivers/char/tpm/tpm_atmel.ko
kernel/drivers/char/tpm/tpm_infineon.ko
kernel/drivers/char/cyclades.ko
kernel/drivers/char/nozomi.ko
kernel/drivers/char/synclink.ko
kernel/drivers/char/synclinkmp.ko
kernel/drivers/char/synclink_gt.ko
kernel/drivers/char/n_hdlc.ko
kernel/drivers/char/virtio_console.ko
kernel/drivers/char/uv_mmtimer.ko
kernel/drivers/char/lp.ko
kernel/drivers/char/i8k.ko
kernel/drivers/char/ppdev.ko
kernel/drivers/char/tlclk.ko
kernel/drivers/char/ipmi/ipmi_msghandler.ko
kernel/drivers/char/ipmi/ipmi_devintf.ko
kernel/drivers/char/ipmi/ipmi_si.ko
kernel/drivers/char/ipmi/ipmi_watchdog.ko
kernel/drivers/char/ipmi/ipmi_poweroff.ko
kernel/drivers/char/hangcheck-timer.ko
kernel/drivers/gpu/drm/i2c/ch7006.ko
kernel/drivers/gpu/drm/i2c/sil164.ko
kernel/drivers/gpu/drm/drm_kms_helper.ko
kernel/drivers/gpu/drm/drm.ko
kernel/drivers/gpu/drm/ttm/ttm.ko
kernel/drivers/gpu/drm/r128/r128.ko
kernel/drivers/gpu/drm/radeon/radeon.ko
kernel/drivers/gpu/drm/mga/mga.ko
kernel/drivers/gpu/drm/i915/i915.ko
kernel/drivers/gpu/drm/sis/sis.ko
kernel/drivers/gpu/drm/savage/savage.ko
kernel/drivers/gpu/drm/via/via.ko
kernel/drivers/gpu/drm/nouveau/nouveau.ko
kernel/drivers/serial/serial_cs.ko
kernel/drivers/serial/jsm/jsm.ko
kernel/drivers/block/floppy.ko
kernel/drivers/block/cciss.ko
kernel/drivers/block/pktcdvd.ko
kernel/drivers/block/osdblk.ko
kernel/drivers/block/cryptoloop.ko
kernel/drivers/block/virtio_blk.ko
kernel/drivers/block/sx8.ko
kernel/drivers/block/xen-blkfront.ko
kernel/drivers/misc/eeprom/at24.ko
kernel/drivers/misc/eeprom/eeprom.ko
kernel/drivers/misc/eeprom/max6875.ko
kernel/drivers/misc/eeprom/eeprom_93cx6.ko
kernel/drivers/misc/cb710/cb710.ko
kernel/drivers/misc/ics932s401.ko
kernel/drivers/misc/tifm_core.ko
kernel/drivers/misc/tifm_7xx1.ko
kernel/drivers/misc/ioc4.ko
kernel/drivers/misc/enclosure.ko
kernel/drivers/misc/sgi-xp/xp.ko
kernel/drivers/misc/sgi-xp/xpc.ko
kernel/drivers/misc/sgi-xp/xpnet.ko
kernel/drivers/misc/sgi-gru/gru.ko
kernel/drivers/misc/hpilo.ko
kernel/drivers/misc/isl29003.ko
kernel/drivers/misc/vmware_balloon.ko
kernel/drivers/mfd/sm501.ko
kernel/drivers/mfd/wm8400-core.ko
kernel/drivers/mfd/wm831x.ko
kernel/drivers/mfd/wm8350.ko
kernel/drivers/mfd/wm8350-i2c.ko
kernel/drivers/mfd/mfd-core.ko
kernel/drivers/mfd/ab3100-core.ko
kernel/drivers/mfd/ab3100-otp.ko
kernel/drivers/scsi/device_handler/scsi_dh_rdac.ko
kernel/drivers/scsi/device_handler/scsi_dh_hp_sw.ko
kernel/drivers/scsi/device_handler/scsi_dh_emc.ko
kernel/drivers/scsi/device_handler/scsi_dh_alua.ko
kernel/drivers/scsi/megaraid/megaraid_mm.ko
kernel/drivers/scsi/megaraid/megaraid_mbox.ko
kernel/drivers/scsi/megaraid/megaraid_sas.ko
kernel/drivers/scsi/scsi_tgt.ko
kernel/drivers/scsi/raid_class.ko
kernel/drivers/scsi/scsi_transport_spi.ko
kernel/drivers/scsi/scsi_transport_fc.ko
kernel/drivers/scsi/scsi_transport_iscsi.ko
kernel/drivers/scsi/scsi_transport_sas.ko
kernel/drivers/scsi/libsas/libsas.ko
kernel/drivers/scsi/scsi_transport_srp.ko
kernel/drivers/scsi/libfc/libfc.ko
kernel/drivers/scsi/fcoe/fcoe.ko
kernel/drivers/scsi/fcoe/libfcoe.ko
kernel/drivers/scsi/fnic/fnic.ko
kernel/drivers/scsi/bnx2fc/bnx2fc.ko
kernel/drivers/scsi/libiscsi.ko
kernel/drivers/scsi/libiscsi_tcp.ko
kernel/drivers/scsi/iscsi_tcp.ko
kernel/drivers/scsi/iscsi_boot_sysfs.ko
kernel/drivers/scsi/arcmsr/arcmsr.ko
kernel/drivers/scsi/aic7xxx/aic7xxx.ko
kernel/drivers/scsi/aic7xxx/aic79xx.ko
kernel/drivers/scsi/aacraid/aacraid.ko
kernel/drivers/scsi/aic94xx/aic94xx.ko
kernel/drivers/scsi/isci/isci.ko
kernel/drivers/scsi/ips.ko
kernel/drivers/scsi/qla2xxx/qla2xxx.ko
kernel/drivers/scsi/qla4xxx/qla4xxx.ko
kernel/drivers/scsi/lpfc/lpfc.ko
kernel/drivers/scsi/bfa/bfa.ko
kernel/drivers/scsi/hpsa.ko
kernel/drivers/scsi/sym53c8xx_2/sym53c8xx.ko
kernel/drivers/scsi/mpt2sas/mpt2sas.ko
kernel/drivers/scsi/initio.ko
kernel/drivers/scsi/3w-xxxx.ko
kernel/drivers/scsi/3w-9xxx.ko
kernel/drivers/scsi/3w-sas.ko
kernel/drivers/scsi/ppa.ko
kernel/drivers/scsi/imm.ko
kernel/drivers/scsi/libsrp.ko
kernel/drivers/scsi/hptiop.ko
kernel/drivers/scsi/stex.ko
kernel/drivers/scsi/mvsas/mvsas.ko
kernel/drivers/scsi/cxgbi/libcxgbi.ko
kernel/drivers/scsi/cxgbi/cxgb3i/cxgb3i.ko
kernel/drivers/scsi/cxgbi/cxgb4i/cxgb4i.ko
kernel/drivers/scsi/bnx2i/bnx2i.ko
kernel/drivers/scsi/be2iscsi/be2iscsi.ko
kernel/drivers/scsi/pmcraid.ko
kernel/drivers/scsi/vmw_pvscsi.ko
kernel/drivers/scsi/st.ko
kernel/drivers/scsi/osst.ko
kernel/drivers/scsi/sd_mod.ko
kernel/drivers/scsi/sr_mod.ko
kernel/drivers/scsi/sg.ko
kernel/drivers/scsi/ch.ko
kernel/drivers/scsi/ses.ko
kernel/drivers/scsi/osd/libosd.ko
kernel/drivers/scsi/osd/osd.ko
kernel/drivers/scsi/scsi_debug.ko
kernel/drivers/scsi/scsi_wait_scan.ko
kernel/drivers/ata/ahci.ko
kernel/drivers/ata/sata_svw.ko
kernel/drivers/ata/ata_piix.ko
kernel/drivers/ata/sata_promise.ko
kernel/drivers/ata/sata_qstor.ko
kernel/drivers/ata/sata_sil.ko
kernel/drivers/ata/sata_sil24.ko
kernel/drivers/ata/sata_via.ko
kernel/drivers/ata/sata_vsc.ko
kernel/drivers/ata/sata_sis.ko
kernel/drivers/ata/sata_sx4.ko
kernel/drivers/ata/sata_nv.ko
kernel/drivers/ata/sata_uli.ko
kernel/drivers/ata/sata_mv.ko
kernel/drivers/ata/sata_inic162x.ko
kernel/drivers/ata/pdc_adma.ko
kernel/drivers/ata/pata_ali.ko
kernel/drivers/ata/pata_amd.ko
kernel/drivers/ata/pata_artop.ko
kernel/drivers/ata/pata_atp867x.ko
kernel/drivers/ata/pata_atiixp.ko
kernel/drivers/ata/pata_cmd64x.ko
kernel/drivers/ata/pata_hpt366.ko
kernel/drivers/ata/pata_hpt37x.ko
kernel/drivers/ata/pata_hpt3x2n.ko
kernel/drivers/ata/pata_hpt3x3.ko
kernel/drivers/ata/pata_it821x.ko
kernel/drivers/ata/pata_it8213.ko
kernel/drivers/ata/pata_jmicron.ko
kernel/drivers/ata/pata_netcell.ko
kernel/drivers/ata/pata_ninja32.ko
kernel/drivers/ata/pata_marvell.ko
kernel/drivers/ata/pata_oldpiix.ko
kernel/drivers/ata/pata_pcmcia.ko
kernel/drivers/ata/pata_pdc2027x.ko
kernel/drivers/ata/pata_pdc202xx_old.ko
kernel/drivers/ata/pata_rdc.ko
kernel/drivers/ata/pata_serverworks.ko
kernel/drivers/ata/pata_sil680.ko
kernel/drivers/ata/pata_via.ko
kernel/drivers/ata/pata_sis.ko
kernel/drivers/ata/pata_sch.ko
kernel/drivers/ata/pata_acpi.ko
kernel/drivers/ata/ata_generic.ko
kernel/drivers/mtd/chips/cfi_probe.ko
kernel/drivers/mtd/chips/cfi_util.ko
kernel/drivers/mtd/chips/cfi_cmdset_0020.ko
kernel/drivers/mtd/chips/cfi_cmdset_0002.ko
kernel/drivers/mtd/chips/cfi_cmdset_0001.ko
kernel/drivers/mtd/chips/gen_probe.ko
kernel/drivers/mtd/chips/jedec_probe.ko
kernel/drivers/mtd/chips/map_ram.ko
kernel/drivers/mtd/chips/map_rom.ko
kernel/drivers/mtd/chips/map_absent.ko
kernel/drivers/mtd/lpddr/qinfo_probe.ko
kernel/drivers/mtd/lpddr/lpddr_cmds.ko
kernel/drivers/mtd/maps/esb2rom.ko
kernel/drivers/mtd/maps/ck804xrom.ko
kernel/drivers/mtd/maps/sc520cdp.ko
kernel/drivers/mtd/maps/netsc520.ko
kernel/drivers/mtd/maps/ts5500_flash.ko
kernel/drivers/mtd/maps/pci.ko
kernel/drivers/mtd/maps/scb2_flash.ko
kernel/drivers/mtd/devices/pmc551.ko
kernel/drivers/mtd/devices/mtdram.ko
kernel/drivers/mtd/devices/block2mtd.ko
kernel/drivers/mtd/nand/nand.ko
kernel/drivers/mtd/nand/nand_ecc.ko
kernel/drivers/mtd/nand/nand_ids.ko
kernel/drivers/mtd/nand/diskonchip.ko
kernel/drivers/mtd/nand/nandsim.ko
kernel/drivers/mtd/nand/alauda.ko
kernel/drivers/mtd/mtdconcat.ko
kernel/drivers/mtd/redboot.ko
kernel/drivers/mtd/ar7part.ko
kernel/drivers/mtd/mtdchar.ko
kernel/drivers/mtd/mtd_blkdevs.ko
kernel/drivers/mtd/mtdblock.ko
kernel/drivers/mtd/mtdblock_ro.ko
kernel/drivers/mtd/ftl.ko
kernel/drivers/mtd/nftl.ko
kernel/drivers/mtd/inftl.ko
kernel/drivers/mtd/rfd_ftl.ko
kernel/drivers/mtd/ssfdc.ko
kernel/drivers/mtd/mtdoops.ko
kernel/drivers/mtd/ubi/ubi.ko
kernel/drivers/net/phy/marvell.ko
kernel/drivers/net/phy/davicom.ko
kernel/drivers/net/phy/cicada.ko
kernel/drivers/net/phy/lxt.ko
kernel/drivers/net/phy/qsemi.ko
kernel/drivers/net/phy/smsc.ko
kernel/drivers/net/phy/vitesse.ko
kernel/drivers/net/phy/broadcom.ko
kernel/drivers/net/phy/icplus.ko
kernel/drivers/net/phy/realtek.ko
kernel/drivers/net/phy/et1011c.ko
kernel/drivers/net/phy/mdio-bitbang.ko
kernel/drivers/net/phy/national.ko
kernel/drivers/net/phy/ste10Xp.ko
kernel/drivers/net/wan/hdlc.ko
kernel/drivers/net/wan/hdlc_raw.ko
kernel/drivers/net/wan/hdlc_cisco.ko
kernel/drivers/net/wan/hdlc_fr.ko
kernel/drivers/net/wan/hdlc_ppp.ko
kernel/drivers/net/wan/dlci.ko
kernel/drivers/net/pcmcia/3c589_cs.ko
kernel/drivers/net/pcmcia/3c574_cs.ko
kernel/drivers/net/pcmcia/fmvj18x_cs.ko
kernel/drivers/net/pcmcia/nmclan_cs.ko
kernel/drivers/net/pcmcia/pcnet_cs.ko
kernel/drivers/net/pcmcia/smc91c92_cs.ko
kernel/drivers/net/pcmcia/xirc2ps_cs.ko
kernel/drivers/net/pcmcia/axnet_cs.ko
kernel/drivers/net/wireless/ipw2x00/ipw2100.ko
kernel/drivers/net/wireless/ipw2x00/ipw2200.ko
kernel/drivers/net/wireless/ipw2x00/libipw.ko
kernel/drivers/net/wireless/orinoco/orinoco.ko
kernel/drivers/net/wireless/orinoco/orinoco_cs.ko
kernel/drivers/net/wireless/orinoco/orinoco_plx.ko
kernel/drivers/net/wireless/orinoco/orinoco_pci.ko
kernel/drivers/net/wireless/orinoco/orinoco_tmd.ko
kernel/drivers/net/wireless/orinoco/orinoco_nortel.ko
kernel/drivers/net/wireless/orinoco/spectrum_cs.ko
kernel/drivers/net/wireless/airo.ko
kernel/drivers/net/wireless/airo_cs.ko
kernel/drivers/net/wireless/atmel.ko
kernel/drivers/net/wireless/atmel_pci.ko
kernel/drivers/net/wireless/atmel_cs.ko
kernel/drivers/net/wireless/at76c50x-usb.ko
kernel/drivers/net/wireless/hostap/hostap.ko
kernel/drivers/net/wireless/hostap/hostap_cs.ko
kernel/drivers/net/wireless/hostap/hostap_plx.ko
kernel/drivers/net/wireless/hostap/hostap_pci.ko
kernel/drivers/net/wireless/b43/b43.ko
kernel/drivers/net/wireless/b43legacy/b43legacy.ko
kernel/drivers/net/wireless/zd1211rw/zd1211rw.ko
kernel/drivers/net/wireless/rtl818x/rtl8180.ko
kernel/drivers/net/wireless/rtl818x/rtl8187.ko
kernel/drivers/net/wireless/wl3501_cs.ko
kernel/drivers/net/wireless/rndis_wlan.ko
kernel/drivers/net/wireless/zd1201.ko
kernel/drivers/net/wireless/libertas/libertas.ko
kernel/drivers/net/wireless/libertas/usb8xxx.ko
kernel/drivers/net/wireless/libertas/libertas_cs.ko
kernel/drivers/net/wireless/libertas/libertas_sdio.ko
kernel/drivers/net/wireless/libertas_tf/libertas_tf.ko
kernel/drivers/net/wireless/libertas_tf/libertas_tf_usb.ko
kernel/drivers/net/wireless/adm8211.ko
kernel/drivers/net/wireless/mwl8k.ko
kernel/drivers/net/wireless/iwlwifi/iwlcore.ko
kernel/drivers/net/wireless/iwlwifi/iwlagn.ko
kernel/drivers/net/wireless/iwlwifi/iwl3945.ko
kernel/drivers/net/wireless/rt2x00/rt2x00lib.ko
kernel/drivers/net/wireless/rt2x00/rt2x00pci.ko
kernel/drivers/net/wireless/rt2x00/rt2x00usb.ko
kernel/drivers/net/wireless/rt2x00/rt2400pci.ko
kernel/drivers/net/wireless/rt2x00/rt2500pci.ko
kernel/drivers/net/wireless/rt2x00/rt61pci.ko
kernel/drivers/net/wireless/rt2x00/rt2500usb.ko
kernel/drivers/net/wireless/rt2x00/rt73usb.ko
kernel/drivers/net/wireless/p54/p54common.ko
kernel/drivers/net/wireless/p54/p54usb.ko
kernel/drivers/net/wireless/p54/p54pci.ko
kernel/drivers/net/wireless/ath/ath5k/ath5k.ko
kernel/drivers/net/wireless/ath/ath9k/ath9k.ko
kernel/drivers/net/wireless/ath/ar9170/ar9170usb.ko
kernel/drivers/net/wireless/ath/ath.ko
kernel/drivers/net/wireless/mac80211_hwsim.ko
kernel/drivers/net/wireless/wl12xx/wl1251.ko
kernel/drivers/net/wireless/wl12xx/wl1251_sdio.ko
kernel/drivers/net/wireless/iwmc3200wifi/iwmc3200wifi.ko
kernel/drivers/net/tulip/xircom_cb.ko
kernel/drivers/net/tulip/dmfe.ko
kernel/drivers/net/tulip/winbond-840.ko
kernel/drivers/net/tulip/de2104x.ko
kernel/drivers/net/tulip/tulip.ko
kernel/drivers/net/tulip/de4x5.ko
kernel/drivers/net/tulip/uli526x.ko
kernel/drivers/net/mii.ko
kernel/drivers/net/mdio.ko
kernel/drivers/net/e1000/e1000.ko
kernel/drivers/net/e1000e/e1000e.ko
kernel/drivers/net/igb/igb.ko
kernel/drivers/net/igbvf/igbvf.ko
kernel/drivers/net/ixgbe/ixgbe.ko
kernel/drivers/net/ixgbevf/ixgbevf.ko
kernel/drivers/net/ixgb/ixgb.ko
kernel/drivers/net/ipg.ko
kernel/drivers/net/chelsio/cxgb.ko
kernel/drivers/net/cxgb3/cxgb3.ko
kernel/drivers/net/cxgb4/cxgb4.ko
kernel/drivers/net/can/usb/ems_usb.ko
kernel/drivers/net/can/vcan.ko
kernel/drivers/net/can/can-dev.ko
kernel/drivers/net/can/sja1000/sja1000.ko
kernel/drivers/net/can/sja1000/sja1000_platform.ko
kernel/drivers/net/can/sja1000/ems_pci.ko
kernel/drivers/net/can/sja1000/kvaser_pci.ko
kernel/drivers/net/bonding/bonding.ko
kernel/drivers/net/atlx/atl1.ko
kernel/drivers/net/atlx/atl2.ko
kernel/drivers/net/atl1e/atl1e.ko
kernel/drivers/net/atl1c/atl1c.ko
kernel/drivers/net/tehuti.ko
kernel/drivers/net/enic/enic.ko
kernel/drivers/net/jme.ko
kernel/drivers/net/benet/be2net.ko
kernel/drivers/net/vmxnet3/vmxnet3.ko
kernel/drivers/net/bna/bna.ko
kernel/drivers/net/sunhme.ko
kernel/drivers/net/sungem.ko
kernel/drivers/net/sungem_phy.ko
kernel/drivers/net/cassini.ko
kernel/drivers/net/3c59x.ko
kernel/drivers/net/typhoon.ko
kernel/drivers/net/ne2k-pci.ko
kernel/drivers/net/8390.ko
kernel/drivers/net/pcnet32.ko
kernel/drivers/net/e100.ko
kernel/drivers/net/tlan.ko
kernel/drivers/net/epic100.ko
kernel/drivers/net/smsc9420.ko
kernel/drivers/net/sis190.ko
kernel/drivers/net/sis900.ko
kernel/drivers/net/r6040.ko
kernel/drivers/net/acenic.ko
kernel/drivers/net/natsemi.ko
kernel/drivers/net/ns83820.ko
kernel/drivers/net/fealnx.ko
kernel/drivers/net/tg3.ko
kernel/drivers/net/bnx2.ko
kernel/drivers/net/cnic.ko
kernel/drivers/net/bnx2x/bnx2x.ko
kernel/drivers/net/skge.ko
kernel/drivers/net/sky2.ko
kernel/drivers/net/via-rhine.ko
kernel/drivers/net/via-velocity.ko
kernel/drivers/net/starfire.ko
kernel/drivers/net/sundance.ko
kernel/drivers/net/b44.ko
kernel/drivers/net/forcedeth.ko
kernel/drivers/net/qla3xxx.ko
kernel/drivers/net/qlcnic/qlcnic.ko
kernel/drivers/net/qlge/qlge.ko
kernel/drivers/net/ppp_generic.ko
kernel/drivers/net/ppp_async.ko
kernel/drivers/net/ppp_synctty.ko
kernel/drivers/net/ppp_deflate.ko
kernel/drivers/net/ppp_mppe.ko
kernel/drivers/net/pppox.ko
kernel/drivers/net/pppoe.ko
kernel/drivers/net/pppol2tp.ko
kernel/drivers/net/slip.ko
kernel/drivers/net/slhc.ko
kernel/drivers/net/xen-netfront.ko
kernel/drivers/net/dummy.ko
kernel/drivers/net/ifb.ko
kernel/drivers/net/macvlan.ko
kernel/drivers/net/macvtap.ko
kernel/drivers/net/8139cp.ko
kernel/drivers/net/8139too.ko
kernel/drivers/net/sc92031.ko
kernel/drivers/net/tun.ko
kernel/drivers/net/veth.ko
kernel/drivers/net/dl2k.ko
kernel/drivers/net/r8169.ko
kernel/drivers/net/amd8111e.ko
kernel/drivers/net/s2io.ko
kernel/drivers/net/vxge/vxge.ko
kernel/drivers/net/myri10ge/myri10ge.ko
kernel/drivers/net/mlx4/mlx4_core.ko
kernel/drivers/net/mlx4/mlx4_en.ko
kernel/drivers/net/ethoc.ko
kernel/drivers/net/dnet.ko
kernel/drivers/net/usb/catc.ko
kernel/drivers/net/usb/kaweth.ko
kernel/drivers/net/usb/pegasus.ko
kernel/drivers/net/usb/rtl8150.ko
kernel/drivers/net/usb/hso.ko
kernel/drivers/net/usb/asix.ko
kernel/drivers/net/usb/cdc_ether.ko
kernel/drivers/net/usb/cdc_eem.ko
kernel/drivers/net/usb/dm9601.ko
kernel/drivers/net/usb/smsc95xx.ko
kernel/drivers/net/usb/gl620a.ko
kernel/drivers/net/usb/net1080.ko
kernel/drivers/net/usb/plusb.ko
kernel/drivers/net/usb/rndis_host.ko
kernel/drivers/net/usb/cdc_subset.ko
kernel/drivers/net/usb/zaurus.ko
kernel/drivers/net/usb/mcs7830.ko
kernel/drivers/net/usb/usbnet.ko
kernel/drivers/net/usb/int51x1.ko
kernel/drivers/net/usb/cdc-phonet.ko
kernel/drivers/net/netconsole.ko
kernel/drivers/net/netxen/netxen_nic.ko
kernel/drivers/net/niu.ko
kernel/drivers/net/virtio_net.ko
kernel/drivers/net/sfc/sfc.ko
kernel/drivers/net/wimax/i2400m/i2400m.ko
kernel/drivers/net/wimax/i2400m/i2400m-usb.ko
kernel/drivers/net/wimax/i2400m/i2400m-sdio.ko
kernel/drivers/message/fusion/mptbase.ko
kernel/drivers/message/fusion/mptscsih.ko
kernel/drivers/message/fusion/mptspi.ko
kernel/drivers/message/fusion/mptfc.ko
kernel/drivers/message/fusion/mptsas.ko
kernel/drivers/message/fusion/mptctl.ko
kernel/drivers/message/fusion/mptlan.ko
kernel/drivers/cdrom/cdrom.ko
kernel/drivers/auxdisplay/ks0108.ko
kernel/drivers/auxdisplay/cfag12864b.ko
kernel/drivers/auxdisplay/cfag12864bfb.ko
kernel/drivers/pcmcia/rsrc_nonstatic.ko
kernel/drivers/pcmcia/yenta_socket.ko
kernel/drivers/pcmcia/pd6729.ko
kernel/drivers/usb/otg/nop-usb-xceiv.ko
kernel/drivers/usb/host/whci/whci-hcd.ko
kernel/drivers/usb/host/isp1362-hcd.ko
kernel/drivers/usb/host/xhci-hcd.ko
kernel/drivers/usb/host/sl811-hcd.ko
kernel/drivers/usb/host/u132-hcd.ko
kernel/drivers/usb/host/hwa-hc.ko
kernel/drivers/usb/storage/usb-storage.ko
kernel/drivers/usb/storage/ums-alauda.ko
kernel/drivers/usb/storage/ums-cypress.ko
kernel/drivers/usb/storage/ums-datafab.ko
kernel/drivers/usb/storage/ums-freecom.ko
kernel/drivers/usb/storage/ums-isd200.ko
kernel/drivers/usb/storage/ums-jumpshot.ko
kernel/drivers/usb/storage/ums-karma.ko
kernel/drivers/usb/storage/ums-onetouch.ko
kernel/drivers/usb/storage/ums-sddr09.ko
kernel/drivers/usb/storage/ums-sddr55.ko
kernel/drivers/usb/storage/ums-usbat.ko
kernel/drivers/usb/misc/adutux.ko
kernel/drivers/usb/misc/appledisplay.ko
kernel/drivers/usb/misc/berry_charge.ko
kernel/drivers/usb/misc/emi26.ko
kernel/drivers/usb/misc/emi62.ko
kernel/drivers/usb/misc/ftdi-elan.ko
kernel/drivers/usb/misc/idmouse.ko
kernel/drivers/usb/misc/iowarrior.ko
kernel/drivers/usb/misc/isight_firmware.ko
kernel/drivers/usb/misc/usblcd.ko
kernel/drivers/usb/misc/ldusb.ko
kernel/drivers/usb/misc/usbled.ko
kernel/drivers/usb/misc/legousbtower.ko
kernel/drivers/usb/misc/uss720.ko
kernel/drivers/usb/misc/usbsevseg.ko
kernel/drivers/usb/misc/vstusb.ko
kernel/drivers/usb/misc/sisusbvga/sisusbvga.ko
kernel/drivers/usb/wusbcore/wusbcore.ko
kernel/drivers/usb/wusbcore/wusb-wa.ko
kernel/drivers/usb/wusbcore/wusb-cbaf.ko
kernel/drivers/usb/class/cdc-acm.ko
kernel/drivers/usb/class/usblp.ko
kernel/drivers/usb/class/cdc-wdm.ko
kernel/drivers/usb/class/usbtmc.ko
kernel/drivers/usb/image/mdc800.ko
kernel/drivers/usb/image/microtek.ko
kernel/drivers/usb/serial/usbserial.ko
kernel/drivers/usb/serial/aircable.ko
kernel/drivers/usb/serial/ark3116.ko
kernel/drivers/usb/serial/belkin_sa.ko
kernel/drivers/usb/serial/ch341.ko
kernel/drivers/usb/serial/cp210x.ko
kernel/drivers/usb/serial/cyberjack.ko
kernel/drivers/usb/serial/cypress_m8.ko
kernel/drivers/usb/serial/usb_debug.ko
kernel/drivers/usb/serial/digi_acceleport.ko
kernel/drivers/usb/serial/io_edgeport.ko
kernel/drivers/usb/serial/io_ti.ko
kernel/drivers/usb/serial/empeg.ko
kernel/drivers/usb/serial/ftdi_sio.ko
kernel/drivers/usb/serial/funsoft.ko
kernel/drivers/usb/serial/garmin_gps.ko
kernel/drivers/usb/serial/hp4x.ko
kernel/drivers/usb/serial/ipaq.ko
kernel/drivers/usb/serial/ipw.ko
kernel/drivers/usb/serial/ir-usb.ko
kernel/drivers/usb/serial/iuu_phoenix.ko
kernel/drivers/usb/serial/keyspan.ko
kernel/drivers/usb/serial/keyspan_pda.ko
kernel/drivers/usb/serial/kl5kusb105.ko
kernel/drivers/usb/serial/kobil_sct.ko
kernel/drivers/usb/serial/mct_u232.ko
kernel/drivers/usb/serial/mos7720.ko
kernel/drivers/usb/serial/mos7840.ko
kernel/drivers/usb/serial/moto_modem.ko
kernel/drivers/usb/serial/navman.ko
kernel/drivers/usb/serial/omninet.ko
kernel/drivers/usb/serial/opticon.ko
kernel/drivers/usb/serial/option.ko
kernel/drivers/usb/serial/oti6858.ko
kernel/drivers/usb/serial/pl2303.ko
kernel/drivers/usb/serial/qcserial.ko
kernel/drivers/usb/serial/safe_serial.ko
kernel/drivers/usb/serial/siemens_mpi.ko
kernel/drivers/usb/serial/sierra.ko
kernel/drivers/usb/serial/spcp8x5.ko
kernel/drivers/usb/serial/symbolserial.ko
kernel/drivers/usb/serial/usb_wwan.ko
kernel/drivers/usb/serial/ti_usb_3410_5052.ko
kernel/drivers/usb/serial/visor.ko
kernel/drivers/usb/serial/whiteheat.ko
kernel/drivers/usb/atm/cxacru.ko
kernel/drivers/usb/atm/speedtch.ko
kernel/drivers/usb/atm/ueagle-atm.ko
kernel/drivers/usb/atm/usbatm.ko
kernel/drivers/usb/atm/xusbatm.ko
kernel/drivers/input/serio/serio_raw.ko
kernel/drivers/input/keyboard/adp5588-keys.ko
kernel/drivers/input/keyboard/max7359_keypad.ko
kernel/drivers/input/keyboard/opencores-kbd.ko
kernel/drivers/input/mouse/appletouch.ko
kernel/drivers/input/mouse/bcm5974.ko
kernel/drivers/input/mouse/sermouse.ko
kernel/drivers/input/mouse/synaptics_i2c.ko
kernel/drivers/input/mouse/vsxxxaa.ko
kernel/drivers/input/tablet/acecad.ko
kernel/drivers/input/tablet/aiptek.ko
kernel/drivers/input/tablet/gtco.ko
kernel/drivers/input/tablet/kbtab.ko
kernel/drivers/input/tablet/wacom.ko
kernel/drivers/input/touchscreen/ad7879.ko
kernel/drivers/input/touchscreen/gunze.ko
kernel/drivers/input/touchscreen/eeti_ts.ko
kernel/drivers/input/touchscreen/elo.ko
kernel/drivers/input/touchscreen/fujitsu_ts.ko
kernel/drivers/input/touchscreen/inexio.ko
kernel/drivers/input/touchscreen/mcs5000_ts.ko
kernel/drivers/input/touchscreen/mtouch.ko
kernel/drivers/input/touchscreen/usbtouchscreen.ko
kernel/drivers/input/touchscreen/penmount.ko
kernel/drivers/input/touchscreen/touchit213.ko
kernel/drivers/input/touchscreen/touchright.ko
kernel/drivers/input/touchscreen/touchwin.ko
kernel/drivers/input/touchscreen/tsc2007.ko
kernel/drivers/input/touchscreen/wacom_w8001.ko
kernel/drivers/input/misc/apanel.ko
kernel/drivers/input/misc/ati_remote.ko
kernel/drivers/input/misc/ati_remote2.ko
kernel/drivers/input/misc/atlas_btns.ko
kernel/drivers/input/misc/cm109.ko
kernel/drivers/input/misc/keyspan_remote.ko
kernel/drivers/input/misc/pcspkr.ko
kernel/drivers/input/misc/powermate.ko
kernel/drivers/input/misc/uinput.ko
kernel/drivers/input/misc/wm831x-on.ko
kernel/drivers/input/misc/yealink.ko
kernel/drivers/input/input-polldev.ko
kernel/drivers/rtc/rtc-ab3100.ko
kernel/drivers/rtc/rtc-bq4802.ko
kernel/drivers/rtc/rtc-ds1286.ko
kernel/drivers/rtc/rtc-ds1307.ko
kernel/drivers/rtc/rtc-ds1374.ko
kernel/drivers/rtc/rtc-ds1511.ko
kernel/drivers/rtc/rtc-ds1553.ko
kernel/drivers/rtc/rtc-ds1672.ko
kernel/drivers/rtc/rtc-ds1742.ko
kernel/drivers/rtc/rtc-fm3130.ko
kernel/drivers/rtc/rtc-isl1208.ko
kernel/drivers/rtc/rtc-m41t80.ko
kernel/drivers/rtc/rtc-m48t35.ko
kernel/drivers/rtc/rtc-m48t59.ko
kernel/drivers/rtc/rtc-max6900.ko
kernel/drivers/rtc/rtc-pcf8563.ko
kernel/drivers/rtc/rtc-pcf8583.ko
kernel/drivers/rtc/rtc-rs5c372.ko
kernel/drivers/rtc/rtc-rx8025.ko
kernel/drivers/rtc/rtc-rx8581.ko
kernel/drivers/rtc/rtc-stk17ta8.ko
kernel/drivers/rtc/rtc-v3020.ko
kernel/drivers/rtc/rtc-wm831x.ko
kernel/drivers/rtc/rtc-wm8350.ko
kernel/drivers/rtc/rtc-x1205.ko
kernel/drivers/i2c/busses/i2c-scmi.ko
kernel/drivers/i2c/busses/i2c-amd756.ko
kernel/drivers/i2c/busses/i2c-amd756-s4882.ko
kernel/drivers/i2c/busses/i2c-amd8111.ko
kernel/drivers/i2c/busses/i2c-i801.ko
kernel/drivers/i2c/busses/i2c-isch.ko
kernel/drivers/i2c/busses/i2c-nforce2.ko
kernel/drivers/i2c/busses/i2c-nforce2-s4985.ko
kernel/drivers/i2c/busses/i2c-piix4.ko
kernel/drivers/i2c/busses/i2c-sis96x.ko
kernel/drivers/i2c/busses/i2c-via.ko
kernel/drivers/i2c/busses/i2c-viapro.ko
kernel/drivers/i2c/busses/i2c-simtec.ko
kernel/drivers/i2c/busses/i2c-parport.ko
kernel/drivers/i2c/busses/i2c-parport-light.ko
kernel/drivers/i2c/busses/i2c-tiny-usb.ko
kernel/drivers/i2c/busses/i2c-voodoo3.ko
kernel/drivers/i2c/busses/i2c-pca-platform.ko
kernel/drivers/i2c/busses/i2c-stub.ko
kernel/drivers/i2c/chips/tsl2550.ko
kernel/drivers/i2c/algos/i2c-algo-bit.ko
kernel/drivers/i2c/algos/i2c-algo-pca.ko
kernel/drivers/i2c/i2c-core.ko
kernel/drivers/i2c/i2c-dev.ko
kernel/drivers/media/common/tuners/tuner-xc2028.ko
kernel/drivers/media/common/tuners/tuner-simple.ko
kernel/drivers/media/common/tuners/tuner-types.ko
kernel/drivers/media/common/tuners/mt20xx.ko
kernel/drivers/media/common/tuners/tda8290.ko
kernel/drivers/media/common/tuners/tea5767.ko
kernel/drivers/media/common/tuners/tea5761.ko
kernel/drivers/media/common/tuners/tda9887.ko
kernel/drivers/media/common/tuners/tda827x.ko
kernel/drivers/media/common/tuners/tda18271.ko
kernel/drivers/media/common/tuners/xc5000.ko
kernel/drivers/media/common/tuners/mt2060.ko
kernel/drivers/media/common/tuners/mt2266.ko
kernel/drivers/media/common/tuners/qt1010.ko
kernel/drivers/media/common/tuners/mt2131.ko
kernel/drivers/media/common/tuners/mxl5005s.ko
kernel/drivers/media/common/tuners/mxl5007t.ko
kernel/drivers/media/common/tuners/mc44s803.ko
kernel/drivers/media/common/tuners/max2165.ko
kernel/drivers/media/common/tuners/tda18218.ko
kernel/drivers/media/common/saa7146.ko
kernel/drivers/media/common/saa7146_vv.ko
kernel/drivers/media/rc/keymaps/rc-adstech-dvb-t-pci.ko
kernel/drivers/media/rc/keymaps/rc-alink-dtu-m.ko
kernel/drivers/media/rc/keymaps/rc-anysee.ko
kernel/drivers/media/rc/keymaps/rc-apac-viewcomp.ko
kernel/drivers/media/rc/keymaps/rc-asus-pc39.ko
kernel/drivers/media/rc/keymaps/rc-ati-tv-wonder-hd-600.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-a16d.ko
kernel/drivers/media/rc/keymaps/rc-avermedia.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-cardbus.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-dvbt.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-m135a.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-m733a-rm-k6.ko
kernel/drivers/media/rc/keymaps/rc-avermedia-rm-ks.ko
kernel/drivers/media/rc/keymaps/rc-avertv-303.ko
kernel/drivers/media/rc/keymaps/rc-azurewave-ad-tu700.ko
kernel/drivers/media/rc/keymaps/rc-behold.ko
kernel/drivers/media/rc/keymaps/rc-behold-columbus.ko
kernel/drivers/media/rc/keymaps/rc-budget-ci-old.ko
kernel/drivers/media/rc/keymaps/rc-cinergy-1400.ko
kernel/drivers/media/rc/keymaps/rc-cinergy.ko
kernel/drivers/media/rc/keymaps/rc-dib0700-nec.ko
kernel/drivers/media/rc/keymaps/rc-dib0700-rc5.ko
kernel/drivers/media/rc/keymaps/rc-digitalnow-tinytwin.ko
kernel/drivers/media/rc/keymaps/rc-digittrade.ko
kernel/drivers/media/rc/keymaps/rc-dm1105-nec.ko
kernel/drivers/media/rc/keymaps/rc-dntv-live-dvb-t.ko
kernel/drivers/media/rc/keymaps/rc-dntv-live-dvbt-pro.ko
kernel/drivers/media/rc/keymaps/rc-em-terratec.ko
kernel/drivers/media/rc/keymaps/rc-encore-enltv2.ko
kernel/drivers/media/rc/keymaps/rc-encore-enltv.ko
kernel/drivers/media/rc/keymaps/rc-encore-enltv-fm53.ko
kernel/drivers/media/rc/keymaps/rc-evga-indtube.ko
kernel/drivers/media/rc/keymaps/rc-eztv.ko
kernel/drivers/media/rc/keymaps/rc-flydvb.ko
kernel/drivers/media/rc/keymaps/rc-flyvideo.ko
kernel/drivers/media/rc/keymaps/rc-fusionhdtv-mce.ko
kernel/drivers/media/rc/keymaps/rc-gadmei-rm008z.ko
kernel/drivers/media/rc/keymaps/rc-genius-tvgo-a11mce.ko
kernel/drivers/media/rc/keymaps/rc-gotview7135.ko
kernel/drivers/media/rc/keymaps/rc-hauppauge-new.ko
kernel/drivers/media/rc/keymaps/rc-imon-mce.ko
kernel/drivers/media/rc/keymaps/rc-imon-pad.ko
kernel/drivers/media/rc/keymaps/rc-iodata-bctv7e.ko
kernel/drivers/media/rc/keymaps/rc-kaiomy.ko
kernel/drivers/media/rc/keymaps/rc-kworld-315u.ko
kernel/drivers/media/rc/keymaps/rc-kworld-plus-tv-analog.ko
kernel/drivers/media/rc/keymaps/rc-leadtek-y04g0051.ko
kernel/drivers/media/rc/keymaps/rc-lirc.ko
kernel/drivers/media/rc/keymaps/rc-lme2510.ko
kernel/drivers/media/rc/keymaps/rc-manli.ko
kernel/drivers/media/rc/keymaps/rc-msi-digivox-ii.ko
kernel/drivers/media/rc/keymaps/rc-msi-digivox-iii.ko
kernel/drivers/media/rc/keymaps/rc-msi-tvanywhere.ko
kernel/drivers/media/rc/keymaps/rc-msi-tvanywhere-plus.ko
kernel/drivers/media/rc/keymaps/rc-nebula.ko
kernel/drivers/media/rc/keymaps/rc-nec-terratec-cinergy-xs.ko
kernel/drivers/media/rc/keymaps/rc-norwood.ko
kernel/drivers/media/rc/keymaps/rc-npgtech.ko
kernel/drivers/media/rc/keymaps/rc-pctv-sedna.ko
kernel/drivers/media/rc/keymaps/rc-pinnacle-color.ko
kernel/drivers/media/rc/keymaps/rc-pinnacle-grey.ko
kernel/drivers/media/rc/keymaps/rc-pinnacle-pctv-hd.ko
kernel/drivers/media/rc/keymaps/rc-pixelview.ko
kernel/drivers/media/rc/keymaps/rc-pixelview-mk12.ko
kernel/drivers/media/rc/keymaps/rc-pixelview-002t.ko
kernel/drivers/media/rc/keymaps/rc-pixelview-new.ko
kernel/drivers/media/rc/keymaps/rc-powercolor-real-angel.ko
kernel/drivers/media/rc/keymaps/rc-proteus-2309.ko
kernel/drivers/media/rc/keymaps/rc-purpletv.ko
kernel/drivers/media/rc/keymaps/rc-pv951.ko
kernel/drivers/media/rc/keymaps/rc-rc5-hauppauge-new.ko
kernel/drivers/media/rc/keymaps/rc-rc5-tv.ko
kernel/drivers/media/rc/keymaps/rc-rc6-mce.ko
kernel/drivers/media/rc/keymaps/rc-real-audio-220-32-keys.ko
kernel/drivers/media/rc/keymaps/rc-streamzap.ko
kernel/drivers/media/rc/keymaps/rc-tbs-nec.ko
kernel/drivers/media/rc/keymaps/rc-terratec-cinergy-xs.ko
kernel/drivers/media/rc/keymaps/rc-terratec-slim.ko
kernel/drivers/media/rc/keymaps/rc-tevii-nec.ko
kernel/drivers/media/rc/keymaps/rc-total-media-in-hand.ko
kernel/drivers/media/rc/keymaps/rc-trekstor.ko
kernel/drivers/media/rc/keymaps/rc-tt-1500.ko
kernel/drivers/media/rc/keymaps/rc-twinhan1027.ko
kernel/drivers/media/rc/keymaps/rc-videomate-m1f.ko
kernel/drivers/media/rc/keymaps/rc-videomate-s350.ko
kernel/drivers/media/rc/keymaps/rc-videomate-tv-pvr.ko
kernel/drivers/media/rc/keymaps/rc-winfast.ko
kernel/drivers/media/rc/keymaps/rc-winfast-usbii-deluxe.ko
kernel/drivers/media/rc/rc-core.ko
kernel/drivers/media/rc/lirc_dev.ko
kernel/drivers/media/rc/ir-nec-decoder.ko
kernel/drivers/media/rc/ir-rc5-decoder.ko
kernel/drivers/media/rc/ir-rc6-decoder.ko
kernel/drivers/media/rc/ir-jvc-decoder.ko
kernel/drivers/media/rc/ir-sony-decoder.ko
kernel/drivers/media/rc/ir-rc5-sz-decoder.ko
kernel/drivers/media/rc/ir-lirc-codec.ko
kernel/drivers/media/rc/imon.ko
kernel/drivers/media/rc/mceusb.ko
kernel/drivers/media/rc/nuvoton-cir.ko
kernel/drivers/media/rc/ene_ir.ko
kernel/drivers/media/rc/streamzap.ko
kernel/drivers/media/rc/winbond-cir.ko
kernel/drivers/media/video/videodev.ko
kernel/drivers/media/video/v4l2-int-device.ko
kernel/drivers/media/video/v4l2-compat-ioctl32.ko
kernel/drivers/media/video/v4l2-common.ko
kernel/drivers/media/video/tuner.ko
kernel/drivers/media/video/tvaudio.ko
kernel/drivers/media/video/tda7432.ko
kernel/drivers/media/video/saa6588.ko
kernel/drivers/media/video/saa7115.ko
kernel/drivers/media/video/saa717x.ko
kernel/drivers/media/video/saa7127.ko
kernel/drivers/media/video/tvp5150.ko
kernel/drivers/media/video/msp3400.ko
kernel/drivers/media/video/cs5345.ko
kernel/drivers/media/video/cs53l32a.ko
kernel/drivers/media/video/m52790.ko
kernel/drivers/media/video/wm8775.ko
kernel/drivers/media/video/wm8739.ko
kernel/drivers/media/video/vp27smpx.ko
kernel/drivers/media/video/cx25840/cx25840.ko
kernel/drivers/media/video/upd64031a.ko
kernel/drivers/media/video/upd64083.ko
kernel/drivers/media/video/tveeprom.ko
kernel/drivers/media/video/mt9v011.ko
kernel/drivers/media/video/mt9m001.ko
kernel/drivers/media/video/mt9m111.ko
kernel/drivers/media/video/mt9t031.ko
kernel/drivers/media/video/mt9v022.ko
kernel/drivers/media/video/ov772x.ko
kernel/drivers/media/video/tw9910.ko
kernel/drivers/media/video/bt8xx/bttv.ko
kernel/drivers/media/video/saa7134/saa6752hs.ko
kernel/drivers/media/video/saa7134/saa7134.ko
kernel/drivers/media/video/saa7134/saa7134-empress.ko
kernel/drivers/media/video/saa7134/saa7134-alsa.ko
kernel/drivers/media/video/saa7134/saa7134-dvb.ko
kernel/drivers/media/video/cx88/cx88xx.ko
kernel/drivers/media/video/cx88/cx8800.ko
kernel/drivers/media/video/cx88/cx8802.ko
kernel/drivers/media/video/cx88/cx88-alsa.ko
kernel/drivers/media/video/cx88/cx88-blackbird.ko
kernel/drivers/media/video/cx88/cx88-dvb.ko
kernel/drivers/media/video/cx88/cx88-vp3054-i2c.ko
kernel/drivers/media/video/em28xx/em28xx.ko
kernel/drivers/media/video/em28xx/em28xx-alsa.ko
kernel/drivers/media/video/em28xx/em28xx-dvb.ko
kernel/drivers/media/video/tlg2300/poseidon.ko
kernel/drivers/media/video/cx231xx/cx231xx.ko
kernel/drivers/media/video/cx231xx/cx231xx-alsa.ko
kernel/drivers/media/video/cx231xx/cx231xx-dvb.ko
kernel/drivers/media/video/usbvision/usbvision.ko
kernel/drivers/media/video/pvrusb2/pvrusb2.ko
kernel/drivers/media/video/videobuf-core.ko
kernel/drivers/media/video/videobuf-dma-sg.ko
kernel/drivers/media/video/videobuf-vmalloc.ko
kernel/drivers/media/video/videobuf-dvb.ko
kernel/drivers/media/video/btcx-risc.ko
kernel/drivers/media/video/cx2341x.ko
kernel/drivers/media/video/zr364xx.ko
kernel/drivers/media/video/stkwebcam.ko
kernel/drivers/media/video/pwc/pwc.ko
kernel/drivers/media/video/gspca/gspca_main.ko
kernel/drivers/media/video/gspca/gspca_benq.ko
kernel/drivers/media/video/gspca/gspca_conex.ko
kernel/drivers/media/video/gspca/gspca_cpia1.ko
kernel/drivers/media/video/gspca/gspca_etoms.ko
kernel/drivers/media/video/gspca/gspca_finepix.ko
kernel/drivers/media/video/gspca/gspca_jeilinj.ko
kernel/drivers/media/video/gspca/gspca_konica.ko
kernel/drivers/media/video/gspca/gspca_mars.ko
kernel/drivers/media/video/gspca/gspca_mr97310a.ko
kernel/drivers/media/video/gspca/gspca_ov519.ko
kernel/drivers/media/video/gspca/gspca_ov534.ko
kernel/drivers/media/video/gspca/gspca_ov534_9.ko
kernel/drivers/media/video/gspca/gspca_pac207.ko
kernel/drivers/media/video/gspca/gspca_pac7302.ko
kernel/drivers/media/video/gspca/gspca_pac7311.ko
kernel/drivers/media/video/gspca/gspca_sn9c2028.ko
kernel/drivers/media/video/gspca/gspca_sn9c20x.ko
kernel/drivers/media/video/gspca/gspca_sonixb.ko
kernel/drivers/media/video/gspca/gspca_sonixj.ko
kernel/drivers/media/video/gspca/gspca_spca500.ko
kernel/drivers/media/video/gspca/gspca_spca501.ko
kernel/drivers/media/video/gspca/gspca_spca505.ko
kernel/drivers/media/video/gspca/gspca_spca506.ko
kernel/drivers/media/video/gspca/gspca_spca508.ko
kernel/drivers/media/video/gspca/gspca_spca561.ko
kernel/drivers/media/video/gspca/gspca_spca1528.ko
kernel/drivers/media/video/gspca/gspca_sq905.ko
kernel/drivers/media/video/gspca/gspca_sq905c.ko
kernel/drivers/media/video/gspca/gspca_sq930x.ko
kernel/drivers/media/video/gspca/gspca_sunplus.ko
kernel/drivers/media/video/gspca/gspca_stk014.ko
kernel/drivers/media/video/gspca/gspca_stv0680.ko
kernel/drivers/media/video/gspca/gspca_t613.ko
kernel/drivers/media/video/gspca/gspca_tv8532.ko
kernel/drivers/media/video/gspca/gspca_vc032x.ko
kernel/drivers/media/video/gspca/gspca_xirlink_cit.ko
kernel/drivers/media/video/gspca/gspca_zc3xx.ko
kernel/drivers/media/video/gspca/m5602/gspca_m5602.ko
kernel/drivers/media/video/gspca/stv06xx/gspca_stv06xx.ko
kernel/drivers/media/video/gspca/gl860/gspca_gl860.ko
kernel/drivers/media/video/hdpvr/hdpvr.ko
kernel/drivers/media/video/s2255drv.ko
kernel/drivers/media/video/ivtv/ivtv.ko
kernel/drivers/media/video/ivtv/ivtvfb.ko
kernel/drivers/media/video/cx18/cx18.ko
kernel/drivers/media/video/cx18/cx18-alsa.ko
kernel/drivers/media/video/cx23885/cx23885.ko
kernel/drivers/media/video/soc_camera.ko
kernel/drivers/media/video/soc_mediabus.ko
kernel/drivers/media/video/soc_camera_platform.ko
kernel/drivers/media/video/au0828/au0828.ko
kernel/drivers/media/video/uvc/uvcvideo.ko
kernel/drivers/media/video/saa7164/saa7164.ko
kernel/drivers/media/video/ir-kbd-i2c.ko
kernel/drivers/media/dvb/dvb-core/dvb-core.ko
kernel/drivers/media/dvb/frontends/dvb-pll.ko
kernel/drivers/media/dvb/frontends/stv0299.ko
kernel/drivers/media/dvb/frontends/stb0899.ko
kernel/drivers/media/dvb/frontends/stb6100.ko
kernel/drivers/media/dvb/frontends/sp8870.ko
kernel/drivers/media/dvb/frontends/cx22700.ko
kernel/drivers/media/dvb/frontends/cx24110.ko
kernel/drivers/media/dvb/frontends/tda8083.ko
kernel/drivers/media/dvb/frontends/l64781.ko
kernel/drivers/media/dvb/frontends/dib3000mb.ko
kernel/drivers/media/dvb/frontends/dib3000mc.ko
kernel/drivers/media/dvb/frontends/dibx000_common.ko
kernel/drivers/media/dvb/frontends/dib7000m.ko
kernel/drivers/media/dvb/frontends/dib7000p.ko
kernel/drivers/media/dvb/frontends/dib8000.ko
kernel/drivers/media/dvb/frontends/mt312.ko
kernel/drivers/media/dvb/frontends/ves1820.ko
kernel/drivers/media/dvb/frontends/ves1x93.ko
kernel/drivers/media/dvb/frontends/tda1004x.ko
kernel/drivers/media/dvb/frontends/sp887x.ko
kernel/drivers/media/dvb/frontends/nxt6000.ko
kernel/drivers/media/dvb/frontends/mt352.ko
kernel/drivers/media/dvb/frontends/zl10036.ko
kernel/drivers/media/dvb/frontends/zl10039.ko
kernel/drivers/media/dvb/frontends/zl10353.ko
kernel/drivers/media/dvb/frontends/cx22702.ko
kernel/drivers/media/dvb/frontends/tda10021.ko
kernel/drivers/media/dvb/frontends/tda10023.ko
kernel/drivers/media/dvb/frontends/stv0297.ko
kernel/drivers/media/dvb/frontends/nxt200x.ko
kernel/drivers/media/dvb/frontends/or51211.ko
kernel/drivers/media/dvb/frontends/or51132.ko
kernel/drivers/media/dvb/frontends/bcm3510.ko
kernel/drivers/media/dvb/frontends/s5h1420.ko
kernel/drivers/media/dvb/frontends/lgdt330x.ko
kernel/drivers/media/dvb/frontends/lgdt3305.ko
kernel/drivers/media/dvb/frontends/cx24123.ko
kernel/drivers/media/dvb/frontends/lnbp21.ko
kernel/drivers/media/dvb/frontends/isl6405.ko
kernel/drivers/media/dvb/frontends/isl6421.ko
kernel/drivers/media/dvb/frontends/tda10086.ko
kernel/drivers/media/dvb/frontends/tda826x.ko
kernel/drivers/media/dvb/frontends/tda8261.ko
kernel/drivers/media/dvb/frontends/dib0070.ko
kernel/drivers/media/dvb/frontends/dib0090.ko
kernel/drivers/media/dvb/frontends/tua6100.ko
kernel/drivers/media/dvb/frontends/s5h1409.ko
kernel/drivers/media/dvb/frontends/itd1000.ko
kernel/drivers/media/dvb/frontends/au8522.ko
kernel/drivers/media/dvb/frontends/tda10048.ko
kernel/drivers/media/dvb/frontends/cx24113.ko
kernel/drivers/media/dvb/frontends/s5h1411.ko
kernel/drivers/media/dvb/frontends/lgs8gxx.ko
kernel/drivers/media/dvb/frontends/atbm8830.ko
kernel/drivers/media/dvb/frontends/af9013.ko
kernel/drivers/media/dvb/frontends/cx24116.ko
kernel/drivers/media/dvb/frontends/si21xx.ko
kernel/drivers/media/dvb/frontends/stv0288.ko
kernel/drivers/media/dvb/frontends/stb6000.ko
kernel/drivers/media/dvb/frontends/s921.ko
kernel/drivers/media/dvb/frontends/stv6110.ko
kernel/drivers/media/dvb/frontends/stv0900.ko
kernel/drivers/media/dvb/frontends/stv090x.ko
kernel/drivers/media/dvb/frontends/stv6110x.ko
kernel/drivers/media/dvb/frontends/isl6423.ko
kernel/drivers/media/dvb/frontends/ec100.ko
kernel/drivers/media/dvb/frontends/ds3000.ko
kernel/drivers/media/dvb/frontends/mb86a20s.ko
kernel/drivers/media/dvb/frontends/ix2505v.ko
kernel/drivers/media/dvb/ttpci/ttpci-eeprom.ko
kernel/drivers/media/dvb/ttpci/budget-core.ko
kernel/drivers/media/dvb/ttpci/budget.ko
kernel/drivers/media/dvb/ttpci/budget-av.ko
kernel/drivers/media/dvb/ttpci/budget-ci.ko
kernel/drivers/media/dvb/ttpci/budget-patch.ko
kernel/drivers/media/dvb/ttpci/dvb-ttpci.ko
kernel/drivers/media/dvb/ttusb-dec/ttusb_dec.ko
kernel/drivers/media/dvb/ttusb-dec/ttusbdecfe.ko
kernel/drivers/media/dvb/ttusb-budget/dvb-ttusb-budget.ko
kernel/drivers/media/dvb/b2c2/b2c2-flexcop.ko
kernel/drivers/media/dvb/b2c2/b2c2-flexcop-pci.ko
kernel/drivers/media/dvb/b2c2/b2c2-flexcop-usb.ko
kernel/drivers/media/dvb/bt8xx/bt878.ko
kernel/drivers/media/dvb/bt8xx/dvb-bt8xx.ko
kernel/drivers/media/dvb/bt8xx/dst.ko
kernel/drivers/media/dvb/bt8xx/dst_ca.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-vp7045.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-vp702x.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-gp8psk.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dtt200u.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dibusb-common.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-a800.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dibusb-mb.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dibusb-mc.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-nova-t-usb2.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-umt-010.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-m920x.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-gl861.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-au6610.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-digitv.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-cxusb.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-ttusb2.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dib0700.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-opera.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-af9005.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-af9005-remote.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-anysee.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dw2102.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-dtv5100.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-af9015.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-cinergyT2.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-ce6230.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-friio.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-ec168.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-az6027.ko
kernel/drivers/media/dvb/dvb-usb/dvb-usb-lmedm04.ko
kernel/drivers/media/dvb/pluto2/pluto2.ko
kernel/drivers/media/dvb/siano/smsmdtv.ko
kernel/drivers/media/dvb/siano/smsdvb.ko
kernel/drivers/media/dvb/siano/smsusb.ko
kernel/drivers/media/dvb/siano/smssdio.ko
kernel/drivers/media/dvb/dm1105/dm1105.ko
kernel/drivers/media/dvb/pt1/earth-pt1.ko
kernel/drivers/media/dvb/ngene/ngene.ko
kernel/drivers/media/dvb/firewire/firedtv.ko
kernel/drivers/power/wm831x_power.ko
kernel/drivers/power/wm8350_power.ko
kernel/drivers/power/bq27x00_battery.ko
kernel/drivers/power/max17040_battery.ko
kernel/drivers/hwmon/hwmon-vid.ko
kernel/drivers/hwmon/asus_atk0110.ko
kernel/drivers/hwmon/asb100.ko
kernel/drivers/hwmon/w83627hf.ko
kernel/drivers/hwmon/w83792d.ko
kernel/drivers/hwmon/w83793.ko
kernel/drivers/hwmon/w83781d.ko
kernel/drivers/hwmon/w83791d.ko
kernel/drivers/hwmon/abituguru.ko
kernel/drivers/hwmon/abituguru3.ko
kernel/drivers/hwmon/ad7414.ko
kernel/drivers/hwmon/ad7418.ko
kernel/drivers/hwmon/adm1021.ko
kernel/drivers/hwmon/adm1025.ko
kernel/drivers/hwmon/adm1026.ko
kernel/drivers/hwmon/adm1029.ko
kernel/drivers/hwmon/adm1031.ko
kernel/drivers/hwmon/adm9240.ko
kernel/drivers/hwmon/ads7828.ko
kernel/drivers/hwmon/adt7462.ko
kernel/drivers/hwmon/adt7470.ko
kernel/drivers/hwmon/adt7473.ko
kernel/drivers/hwmon/adt7475.ko
kernel/drivers/hwmon/applesmc.ko
kernel/drivers/hwmon/atxp1.ko
kernel/drivers/hwmon/coretemp.ko
kernel/drivers/hwmon/dme1737.ko
kernel/drivers/hwmon/ds1621.ko
kernel/drivers/hwmon/f71805f.ko
kernel/drivers/hwmon/f71882fg.ko
kernel/drivers/hwmon/f75375s.ko
kernel/drivers/hwmon/fschmd.ko
kernel/drivers/hwmon/g760a.ko
kernel/drivers/hwmon/gl518sm.ko
kernel/drivers/hwmon/gl520sm.ko
kernel/drivers/hwmon/hdaps.ko
kernel/drivers/hwmon/i5k_amb.ko
kernel/drivers/hwmon/ibmaem.ko
kernel/drivers/hwmon/ibmpex.ko
kernel/drivers/hwmon/it87.ko
kernel/drivers/hwmon/k8temp.ko
kernel/drivers/hwmon/k10temp.ko
kernel/drivers/hwmon/lis3lv02d.ko
kernel/drivers/hwmon/hp_accel.ko
kernel/drivers/hwmon/lm63.ko
kernel/drivers/hwmon/lm75.ko
kernel/drivers/hwmon/lm77.ko
kernel/drivers/hwmon/lm78.ko
kernel/drivers/hwmon/lm80.ko
kernel/drivers/hwmon/lm83.ko
kernel/drivers/hwmon/lm85.ko
kernel/drivers/hwmon/lm87.ko
kernel/drivers/hwmon/lm90.ko
kernel/drivers/hwmon/lm92.ko
kernel/drivers/hwmon/lm93.ko
kernel/drivers/hwmon/lm95241.ko
kernel/drivers/hwmon/ltc4215.ko
kernel/drivers/hwmon/ltc4245.ko
kernel/drivers/hwmon/max1619.ko
kernel/drivers/hwmon/max6650.ko
kernel/drivers/hwmon/pc87360.ko
kernel/drivers/hwmon/pc87427.ko
kernel/drivers/hwmon/pcf8591.ko
kernel/drivers/hwmon/sis5595.ko
kernel/drivers/hwmon/smsc47b397.ko
kernel/drivers/hwmon/smsc47m1.ko
kernel/drivers/hwmon/smsc47m192.ko
kernel/drivers/hwmon/thmc50.ko
kernel/drivers/hwmon/tmp401.ko
kernel/drivers/hwmon/tmp421.ko
kernel/drivers/hwmon/via-cputemp.ko
kernel/drivers/hwmon/via686a.ko
kernel/drivers/hwmon/vt1211.ko
kernel/drivers/hwmon/vt8231.ko
kernel/drivers/hwmon/w83627ehf.ko
kernel/drivers/hwmon/w83l785ts.ko
kernel/drivers/hwmon/w83l786ng.ko
kernel/drivers/hwmon/wm831x-hwmon.ko
kernel/drivers/hwmon/wm8350-hwmon.ko
kernel/drivers/watchdog/pcwd_pci.ko
kernel/drivers/watchdog/wdt_pci.ko
kernel/drivers/watchdog/pcwd_usb.ko
kernel/drivers/watchdog/alim1535_wdt.ko
kernel/drivers/watchdog/alim7101_wdt.ko
kernel/drivers/watchdog/sbc_fitpc2_wdt.ko
kernel/drivers/watchdog/ib700wdt.ko
kernel/drivers/watchdog/ibmasr.ko
kernel/drivers/watchdog/i6300esb.ko
kernel/drivers/watchdog/iTCO_wdt.ko
kernel/drivers/watchdog/iTCO_vendor_support.ko
kernel/drivers/watchdog/it8712f_wdt.ko
kernel/drivers/watchdog/it87_wdt.ko
kernel/drivers/watchdog/hpwdt.ko
kernel/drivers/watchdog/sch311x_wdt.ko
kernel/drivers/watchdog/w83627hf_wdt.ko
kernel/drivers/watchdog/w83697hf_wdt.ko
kernel/drivers/watchdog/w83697ug_wdt.ko
kernel/drivers/watchdog/w83877f_wdt.ko
kernel/drivers/watchdog/w83977f_wdt.ko
kernel/drivers/watchdog/machzwd.ko
kernel/drivers/watchdog/wm831x_wdt.ko
kernel/drivers/watchdog/wm8350_wdt.ko
kernel/drivers/watchdog/softdog.ko
kernel/drivers/md/linear.ko
kernel/drivers/md/raid0.ko
kernel/drivers/md/raid1.ko
kernel/drivers/md/raid10.ko
kernel/drivers/md/raid456.ko
kernel/drivers/md/faulty.ko
kernel/drivers/md/dm-mod.ko
kernel/drivers/md/dm-crypt.ko
kernel/drivers/md/dm-delay.ko
kernel/drivers/md/dm-flakey.ko
kernel/drivers/md/dm-multipath.ko
kernel/drivers/md/dm-round-robin.ko
kernel/drivers/md/dm-queue-length.ko
kernel/drivers/md/dm-service-time.ko
kernel/drivers/md/dm-snapshot.ko
kernel/drivers/md/dm-mirror.ko
kernel/drivers/md/dm-log.ko
kernel/drivers/md/dm-region-hash.ko
kernel/drivers/md/dm-log-userspace.ko
kernel/drivers/md/dm-zero.ko
kernel/drivers/md/dm-raid.ko
kernel/drivers/md/dm-raid45.ko
kernel/drivers/md/dm-memcache.ko
kernel/drivers/md/dm-replicator.ko
kernel/drivers/md/dm-repl-log-ringbuffer.ko
kernel/drivers/md/dm-repl-slink-blockdev.ko
kernel/drivers/md/dm-registry.ko
kernel/drivers/isdn/hardware/avm/b1pci.ko
kernel/drivers/isdn/hardware/avm/b1.ko
kernel/drivers/isdn/hardware/avm/b1dma.ko
kernel/drivers/isdn/hardware/avm/b1pcmcia.ko
kernel/drivers/isdn/hardware/avm/avm_cs.ko
kernel/drivers/isdn/hardware/avm/t1pci.ko
kernel/drivers/isdn/hardware/avm/c4.ko
kernel/drivers/isdn/hardware/mISDN/hfcpci.ko
kernel/drivers/isdn/hardware/mISDN/hfcmulti.ko
kernel/drivers/isdn/hardware/mISDN/hfcsusb.ko
kernel/drivers/isdn/hardware/mISDN/avmfritz.ko
kernel/drivers/isdn/hardware/mISDN/speedfax.ko
kernel/drivers/isdn/hardware/mISDN/mISDNinfineon.ko
kernel/drivers/isdn/hardware/mISDN/w6692.ko
kernel/drivers/isdn/hardware/mISDN/netjet.ko
kernel/drivers/isdn/hardware/mISDN/mISDNipac.ko
kernel/drivers/isdn/hardware/mISDN/mISDNisar.ko
kernel/drivers/isdn/i4l/isdn.ko
kernel/drivers/isdn/i4l/isdnhdlc.ko
kernel/drivers/isdn/capi/kernelcapi.ko
kernel/drivers/isdn/capi/capi.ko
kernel/drivers/isdn/capi/capidrv.ko
kernel/drivers/isdn/capi/capifs.ko
kernel/drivers/isdn/mISDN/mISDN_core.ko
kernel/drivers/isdn/mISDN/mISDN_dsp.ko
kernel/drivers/isdn/mISDN/l1oip.ko
kernel/drivers/isdn/divert/dss1_divert.ko
kernel/drivers/isdn/hisax/hisax.ko
kernel/drivers/isdn/hisax/sedlbauer_cs.ko
kernel/drivers/isdn/hisax/elsa_cs.ko
kernel/drivers/isdn/hisax/avma1_cs.ko
kernel/drivers/isdn/hisax/teles_cs.ko
kernel/drivers/isdn/hisax/hisax_st5481.ko
kernel/drivers/isdn/hisax/hfc4s8s_l1.ko
kernel/drivers/isdn/hisax/hisax_isac.ko
kernel/drivers/isdn/hisax/hisax_fcpcipnp.ko
kernel/drivers/isdn/hysdn/hysdn.ko
kernel/drivers/isdn/gigaset/gigaset.ko
kernel/drivers/isdn/gigaset/usb_gigaset.ko
kernel/drivers/isdn/gigaset/bas_gigaset.ko
kernel/drivers/isdn/gigaset/ser_gigaset.ko
kernel/drivers/edac/edac_core.ko
kernel/drivers/edac/edac_mce_amd.ko
kernel/drivers/edac/i5000_edac.ko
kernel/drivers/edac/i5100_edac.ko
kernel/drivers/edac/i5400_edac.ko
kernel/drivers/edac/i7300_edac.ko
kernel/drivers/edac/i7core_edac.ko
kernel/drivers/edac/sb_edac.ko
kernel/drivers/edac/e752x_edac.ko
kernel/drivers/edac/i82975x_edac.ko
kernel/drivers/edac/i3000_edac.ko
kernel/drivers/edac/i3200_edac.ko
kernel/drivers/edac/x38_edac.ko
kernel/drivers/edac/amd64_edac_mod.ko
kernel/drivers/cpufreq/cpufreq_stats.ko
kernel/drivers/cpufreq/cpufreq_powersave.ko
kernel/drivers/cpufreq/cpufreq_ondemand.ko
kernel/drivers/cpufreq/cpufreq_conservative.ko
kernel/drivers/cpufreq/freq_table.ko
kernel/drivers/leds/leds-alix2.ko
kernel/drivers/leds/leds-lp3944.ko
kernel/drivers/leds/leds-clevo-mail.ko
kernel/drivers/leds/leds-wm831x-status.ko
kernel/drivers/leds/leds-wm8350.ko
kernel/drivers/leds/ledtrig-timer.ko
kernel/drivers/leds/ledtrig-heartbeat.ko
kernel/drivers/leds/ledtrig-backlight.ko
kernel/drivers/leds/ledtrig-default-on.ko
kernel/drivers/firmware/edd.ko
kernel/drivers/firmware/dell_rbu.ko
kernel/drivers/firmware/dcdbas.ko
kernel/drivers/firmware/iscsi_ibft.ko
kernel/drivers/crypto/padlock-aes.ko
kernel/drivers/crypto/padlock-sha.ko
kernel/drivers/crypto/hifn_795x.ko
kernel/drivers/dma/ioat/ioatdma.ko
kernel/drivers/hid/hid-wacom.ko
kernel/drivers/staging/zram/zram.ko
kernel/drivers/platform/x86/asus-laptop.ko
kernel/drivers/platform/x86/msi-laptop.ko
kernel/drivers/platform/x86/compal-laptop.ko
kernel/drivers/platform/x86/dell-laptop.ko
kernel/drivers/platform/x86/dell-wmi.ko
kernel/drivers/platform/x86/acer-wmi.ko
kernel/drivers/platform/x86/hp-wmi.ko
kernel/drivers/platform/x86/sony-laptop.ko
kernel/drivers/platform/x86/thinkpad_acpi.ko
kernel/drivers/platform/x86/fujitsu-laptop.ko
kernel/drivers/platform/x86/panasonic-laptop.ko
kernel/drivers/platform/x86/wmi.ko
kernel/drivers/platform/x86/topstar-laptop.ko
kernel/drivers/platform/x86/toshiba_acpi.ko
kernel/drivers/platform/x86/intel_ips.ko
kernel/drivers/platform/x86/mxm-wmi.ko
kernel/drivers/ieee802154/fakehard.ko
kernel/drivers/virtio/virtio.ko
kernel/drivers/virtio/virtio_ring.ko
kernel/drivers/virtio/virtio_pci.ko
kernel/drivers/virtio/virtio_balloon.ko
kernel/drivers/parport/parport.ko
kernel/drivers/parport/parport_pc.ko
kernel/drivers/parport/parport_serial.ko
kernel/drivers/parport/parport_cs.ko
kernel/drivers/target/target_core_mod.ko
kernel/drivers/target/target_core_iblock.ko
kernel/drivers/target/target_core_file.ko
kernel/drivers/target/target_core_pscsi.ko
kernel/drivers/target/loopback/tcm_loop.ko
kernel/drivers/target/tcm_fc/tcm_fc.ko
kernel/drivers/atm/atmtcp.ko
kernel/drivers/firewire/firewire-core.ko
kernel/drivers/firewire/firewire-ohci.ko
kernel/drivers/firewire/firewire-sbp2.ko
kernel/drivers/firewire/firewire-net.ko
kernel/drivers/uio/uio.ko
kernel/drivers/uio/uio_cif.ko
kernel/drivers/uio/uio_pdrv.ko
kernel/drivers/uio/uio_pdrv_genirq.ko
kernel/drivers/uio/uio_smx.ko
kernel/drivers/uio/uio_aec.ko
kernel/drivers/uio/uio_sercos3.ko
kernel/drivers/uio/uio_pci_generic.ko
kernel/drivers/block/aoe/aoe.ko
kernel/drivers/uwb/uwb.ko
kernel/drivers/uwb/wlp/wlp.ko
kernel/drivers/uwb/umc.ko
kernel/drivers/uwb/whci.ko
kernel/drivers/uwb/whc-rc.ko
kernel/drivers/uwb/hwa-rc.ko
kernel/drivers/uwb/i1480/dfu/i1480-dfu-usb.ko
kernel/drivers/uwb/i1480/i1480-est.ko
kernel/drivers/uwb/i1480/i1480u-wlp/i1480u-wlp.ko
kernel/drivers/pps/pps_core.ko
kernel/drivers/bluetooth/hci_vhci.ko
kernel/drivers/bluetooth/hci_uart.ko
kernel/drivers/bluetooth/bcm203x.ko
kernel/drivers/bluetooth/bpa10x.ko
kernel/drivers/bluetooth/bfusb.ko
kernel/drivers/bluetooth/dtl1_cs.ko
kernel/drivers/bluetooth/bt3c_cs.ko
kernel/drivers/bluetooth/bluecard_cs.ko
kernel/drivers/bluetooth/btuart_cs.ko
kernel/drivers/bluetooth/btusb.ko
kernel/drivers/bluetooth/btsdio.ko
kernel/drivers/bluetooth/btmrvl.ko
kernel/drivers/bluetooth/btmrvl_sdio.ko
kernel/drivers/mmc/core/mmc_core.ko
kernel/drivers/mmc/card/mmc_block.ko
kernel/drivers/mmc/card/sdio_uart.ko
kernel/drivers/mmc/host/sdhci.ko
kernel/drivers/mmc/host/sdhci-pci.ko
kernel/drivers/mmc/host/ricoh_mmc.ko
kernel/drivers/mmc/host/sdhci-pltfm.ko
kernel/drivers/mmc/host/tifm_sd.ko
kernel/drivers/mmc/host/sdricoh_cs.ko
kernel/drivers/mmc/host/cb710-mmc.ko
kernel/drivers/mmc/host/via-sdmmc.ko
kernel/drivers/memstick/core/memstick.ko
kernel/drivers/memstick/core/mspro_block.ko
kernel/drivers/memstick/host/tifm_ms.ko
kernel/drivers/memstick/host/jmb38x_ms.ko
kernel/drivers/memstick/host/r592.ko
kernel/drivers/infiniband/core/ib_core.ko
kernel/drivers/infiniband/core/ib_mad.ko
kernel/drivers/infiniband/core/ib_sa.ko
kernel/drivers/infiniband/core/ib_cm.ko
kernel/drivers/infiniband/core/iw_cm.ko
kernel/drivers/infiniband/core/ib_addr.ko
kernel/drivers/infiniband/core/rdma_cm.ko
kernel/drivers/infiniband/core/ib_umad.ko
kernel/drivers/infiniband/core/ib_uverbs.ko
kernel/drivers/infiniband/core/ib_ucm.ko
kernel/drivers/infiniband/core/rdma_ucm.ko
kernel/drivers/infiniband/hw/mthca/ib_mthca.ko
kernel/drivers/infiniband/hw/ipath/ib_ipath.ko
kernel/drivers/infiniband/hw/qib/ib_qib.ko
kernel/drivers/infiniband/hw/cxgb3/iw_cxgb3.ko
kernel/drivers/infiniband/hw/cxgb4/iw_cxgb4.ko
kernel/drivers/infiniband/hw/mlx4/mlx4_ib.ko
kernel/drivers/infiniband/hw/nes/iw_nes.ko
kernel/drivers/infiniband/ulp/ipoib/ib_ipoib.ko
kernel/drivers/infiniband/ulp/srp/ib_srp.ko
kernel/drivers/infiniband/ulp/iser/ib_iser.ko
kernel/drivers/dca/dca.ko
kernel/drivers/ssb/ssb.ko
kernel/drivers/vhost/vhost_net.ko
kernel/sound/soundcore.ko
kernel/sound/core/snd.ko
kernel/sound/core/snd-hwdep.ko
kernel/sound/core/snd-timer.ko
kernel/sound/core/snd-hrtimer.ko
kernel/sound/core/snd-pcm.ko
kernel/sound/core/snd-page-alloc.ko
kernel/sound/core/snd-rawmidi.ko
kernel/sound/core/seq/snd-seq.ko
kernel/sound/core/seq/snd-seq-device.ko
kernel/sound/core/seq/snd-seq-midi-event.ko
kernel/sound/core/seq/oss/snd-seq-oss.ko
kernel/sound/core/seq/snd-seq-dummy.ko
kernel/sound/core/seq/snd-seq-virmidi.ko
kernel/sound/core/seq/snd-seq-midi.ko
kernel/sound/core/seq/snd-seq-midi-emul.ko
kernel/sound/i2c/other/snd-ak4xxx-adda.ko
kernel/sound/i2c/other/snd-ak4114.ko
kernel/sound/i2c/other/snd-pt2258.ko
kernel/sound/i2c/snd-cs8427.ko
kernel/sound/i2c/snd-i2c.ko
kernel/sound/drivers/snd-dummy.ko
kernel/sound/drivers/snd-aloop.ko
kernel/sound/drivers/snd-virmidi.ko
kernel/sound/drivers/snd-mtpav.ko
kernel/sound/drivers/mpu401/snd-mpu401-uart.ko
kernel/sound/drivers/mpu401/snd-mpu401.ko
kernel/sound/drivers/vx/snd-vx-lib.ko
kernel/sound/drivers/pcsp/snd-pcsp.ko
kernel/sound/isa/sb/snd-sb-common.ko
kernel/sound/isa/sb/snd-sb16-dsp.ko
kernel/sound/pci/snd-ad1889.ko
kernel/sound/pci/snd-atiixp.ko
kernel/sound/pci/snd-atiixp-modem.ko
kernel/sound/pci/snd-bt87x.ko
kernel/sound/pci/snd-cs5530.ko
kernel/sound/pci/snd-ens1370.ko
kernel/sound/pci/snd-ens1371.ko
kernel/sound/pci/snd-es1968.ko
kernel/sound/pci/snd-intel8x0.ko
kernel/sound/pci/snd-intel8x0m.ko
kernel/sound/pci/snd-maestro3.ko
kernel/sound/pci/snd-rme32.ko
kernel/sound/pci/snd-rme96.ko
kernel/sound/pci/snd-via82xx.ko
kernel/sound/pci/snd-via82xx-modem.ko
kernel/sound/pci/ac97/snd-ac97-codec.ko
kernel/sound/pci/ali5451/snd-ali5451.ko
kernel/sound/pci/au88x0/snd-au8810.ko
kernel/sound/pci/au88x0/snd-au8820.ko
kernel/sound/pci/au88x0/snd-au8830.ko
kernel/sound/pci/ctxfi/snd-ctxfi.ko
kernel/sound/pci/ca0106/snd-ca0106.ko
kernel/sound/pci/cs46xx/snd-cs46xx.ko
kernel/sound/pci/cs5535audio/snd-cs5535audio.ko
kernel/sound/pci/lx6464es/snd-lx6464es.ko
kernel/sound/pci/echoaudio/snd-darla20.ko
kernel/sound/pci/echoaudio/snd-gina20.ko
kernel/sound/pci/echoaudio/snd-layla20.ko
kernel/sound/pci/echoaudio/snd-darla24.ko
kernel/sound/pci/echoaudio/snd-gina24.ko
kernel/sound/pci/echoaudio/snd-layla24.ko
kernel/sound/pci/echoaudio/snd-mona.ko
kernel/sound/pci/echoaudio/snd-mia.ko
kernel/sound/pci/echoaudio/snd-echo3g.ko
kernel/sound/pci/echoaudio/snd-indigo.ko
kernel/sound/pci/echoaudio/snd-indigoio.ko
kernel/sound/pci/echoaudio/snd-indigodj.ko
kernel/sound/pci/echoaudio/snd-indigoiox.ko
kernel/sound/pci/echoaudio/snd-indigodjx.ko
kernel/sound/pci/emu10k1/snd-emu10k1.ko
kernel/sound/pci/emu10k1/snd-emu10k1-synth.ko
kernel/sound/pci/emu10k1/snd-emu10k1x.ko
kernel/sound/pci/hda/snd-hda-codec.ko
kernel/sound/pci/hda/snd-hda-codec-realtek.ko
kernel/sound/pci/hda/snd-hda-codec-cmedia.ko
kernel/sound/pci/hda/snd-hda-codec-analog.ko
kernel/sound/pci/hda/snd-hda-codec-idt.ko
kernel/sound/pci/hda/snd-hda-codec-si3054.ko
kernel/sound/pci/hda/snd-hda-codec-cirrus.ko
kernel/sound/pci/hda/snd-hda-codec-ca0110.ko
kernel/sound/pci/hda/snd-hda-codec-ca0132.ko
kernel/sound/pci/hda/snd-hda-codec-conexant.ko
kernel/sound/pci/hda/snd-hda-codec-via.ko
kernel/sound/pci/hda/snd-hda-codec-hdmi.ko
kernel/sound/pci/hda/snd-hda-intel.ko
kernel/sound/pci/ice1712/snd-ice1712.ko
kernel/sound/pci/ice1712/snd-ice17xx-ak4xxx.ko
kernel/sound/pci/ice1712/snd-ice1724.ko
kernel/sound/pci/korg1212/snd-korg1212.ko
kernel/sound/pci/mixart/snd-mixart.ko
kernel/sound/pci/oxygen/snd-oxygen-lib.ko
kernel/sound/pci/oxygen/snd-hifier.ko
kernel/sound/pci/oxygen/snd-oxygen.ko
kernel/sound/pci/oxygen/snd-virtuoso.ko
kernel/sound/pci/pcxhr/snd-pcxhr.ko
kernel/sound/pci/rme9652/snd-rme9652.ko
kernel/sound/pci/rme9652/snd-hdsp.ko
kernel/sound/pci/rme9652/snd-hdspm.ko
kernel/sound/pci/trident/snd-trident.ko
kernel/sound/pci/vx222/snd-vx222.ko
kernel/sound/synth/snd-util-mem.ko
kernel/sound/synth/emux/snd-emux-synth.ko
kernel/sound/usb/snd-usb-audio.ko
kernel/sound/usb/snd-usb-lib.ko
kernel/sound/usb/usx2y/snd-usb-usx2y.ko
kernel/sound/usb/usx2y/snd-usb-us122l.ko
kernel/sound/usb/caiaq/snd-usb-caiaq.ko
kernel/sound/ac97_bus.ko
kernel/arch/x86/oprofile/oprofile.ko
kernel/net/core/pktgen.ko
kernel/net/802/p8022.ko
kernel/net/802/psnap.ko
kernel/net/802/stp.ko
kernel/net/802/garp.ko
kernel/net/sched/act_police.ko
kernel/net/sched/act_gact.ko
kernel/net/sched/act_mirred.ko
kernel/net/sched/act_ipt.ko
kernel/net/sched/act_nat.ko
kernel/net/sched/act_pedit.ko
kernel/net/sched/act_simple.ko
kernel/net/sched/act_skbedit.ko
kernel/net/sched/sch_cbq.ko
kernel/net/sched/sch_htb.ko
kernel/net/sched/sch_hfsc.ko
kernel/net/sched/sch_red.ko
kernel/net/sched/sch_gred.ko
kernel/net/sched/sch_ingress.ko
kernel/net/sched/sch_dsmark.ko
kernel/net/sched/sch_sfq.ko
kernel/net/sched/sch_tbf.ko
kernel/net/sched/sch_teql.ko
kernel/net/sched/sch_prio.ko
kernel/net/sched/sch_multiq.ko
kernel/net/sched/sch_atm.ko
kernel/net/sched/sch_netem.ko
kernel/net/sched/sch_drr.ko
kernel/net/sched/cls_u32.ko
kernel/net/sched/cls_route.ko
kernel/net/sched/cls_fw.ko
kernel/net/sched/cls_rsvp.ko
kernel/net/sched/cls_tcindex.ko
kernel/net/sched/cls_rsvp6.ko
kernel/net/sched/cls_basic.ko
kernel/net/sched/cls_flow.ko
kernel/net/sched/em_cmp.ko
kernel/net/sched/em_nbyte.ko
kernel/net/sched/em_u32.ko
kernel/net/sched/em_meta.ko
kernel/net/sched/em_text.ko
kernel/net/netfilter/nfnetlink.ko
kernel/net/netfilter/nfnetlink_queue.ko
kernel/net/netfilter/nfnetlink_log.ko
kernel/net/netfilter/nf_conntrack.ko
kernel/net/netfilter/nf_conntrack_proto_dccp.ko
kernel/net/netfilter/nf_conntrack_proto_gre.ko
kernel/net/netfilter/nf_conntrack_proto_sctp.ko
kernel/net/netfilter/nf_conntrack_proto_udplite.ko
kernel/net/netfilter/nf_conntrack_netlink.ko
kernel/net/netfilter/nf_conntrack_amanda.ko
kernel/net/netfilter/nf_conntrack_ftp.ko
kernel/net/netfilter/nf_conntrack_h323.ko
kernel/net/netfilter/nf_conntrack_irc.ko
kernel/net/netfilter/nf_conntrack_broadcast.ko
kernel/net/netfilter/nf_conntrack_netbios_ns.ko
kernel/net/netfilter/nf_conntrack_snmp.ko
kernel/net/netfilter/nf_conntrack_pptp.ko
kernel/net/netfilter/nf_conntrack_sane.ko
kernel/net/netfilter/nf_conntrack_sip.ko
kernel/net/netfilter/nf_conntrack_tftp.ko
kernel/net/netfilter/nf_tproxy_core.ko
kernel/net/netfilter/xt_set.ko
kernel/net/netfilter/xt_AUDIT.ko
kernel/net/netfilter/xt_CHECKSUM.ko
kernel/net/netfilter/xt_CLASSIFY.ko
kernel/net/netfilter/xt_CONNMARK.ko
kernel/net/netfilter/xt_CONNSECMARK.ko
kernel/net/netfilter/xt_DSCP.ko
kernel/net/netfilter/xt_HL.ko
kernel/net/netfilter/xt_LED.ko
kernel/net/netfilter/xt_MARK.ko
kernel/net/netfilter/xt_NFLOG.ko
kernel/net/netfilter/xt_NFQUEUE.ko
kernel/net/netfilter/xt_NOTRACK.ko
kernel/net/netfilter/xt_RATEEST.ko
kernel/net/netfilter/xt_SECMARK.ko
kernel/net/netfilter/xt_TPROXY.ko
kernel/net/netfilter/xt_TCPMSS.ko
kernel/net/netfilter/xt_TCPOPTSTRIP.ko
kernel/net/netfilter/xt_TRACE.ko
kernel/net/netfilter/xt_cluster.ko
kernel/net/netfilter/xt_comment.ko
kernel/net/netfilter/xt_connbytes.ko
kernel/net/netfilter/xt_connlimit.ko
kernel/net/netfilter/xt_connmark.ko
kernel/net/netfilter/xt_conntrack.ko
kernel/net/netfilter/xt_dccp.ko
kernel/net/netfilter/xt_dscp.ko
kernel/net/netfilter/xt_esp.ko
kernel/net/netfilter/xt_hashlimit.ko
kernel/net/netfilter/xt_helper.ko
kernel/net/netfilter/xt_hl.ko
kernel/net/netfilter/xt_iprange.ko
kernel/net/netfilter/xt_length.ko
kernel/net/netfilter/xt_limit.ko
kernel/net/netfilter/xt_mac.ko
kernel/net/netfilter/xt_mark.ko
kernel/net/netfilter/xt_multiport.ko
kernel/net/netfilter/xt_osf.ko
kernel/net/netfilter/xt_owner.ko
kernel/net/netfilter/xt_physdev.ko
kernel/net/netfilter/xt_pkttype.ko
kernel/net/netfilter/xt_policy.ko
kernel/net/netfilter/xt_quota.ko
kernel/net/netfilter/xt_rateest.ko
kernel/net/netfilter/xt_realm.ko
kernel/net/netfilter/xt_recent.ko
kernel/net/netfilter/xt_sctp.ko
kernel/net/netfilter/xt_socket.ko
kernel/net/netfilter/xt_state.ko
kernel/net/netfilter/xt_statistic.ko
kernel/net/netfilter/xt_string.ko
kernel/net/netfilter/xt_tcpmss.ko
kernel/net/netfilter/xt_time.ko
kernel/net/netfilter/xt_u32.ko
kernel/net/netfilter/ipset/ip_set.ko
kernel/net/netfilter/ipset/ip_set_bitmap_ip.ko
kernel/net/netfilter/ipset/ip_set_bitmap_ipmac.ko
kernel/net/netfilter/ipset/ip_set_bitmap_port.ko
kernel/net/netfilter/ipset/ip_set_hash_ip.ko
kernel/net/netfilter/ipset/ip_set_hash_ipport.ko
kernel/net/netfilter/ipset/ip_set_hash_ipportip.ko
kernel/net/netfilter/ipset/ip_set_hash_ipportnet.ko
kernel/net/netfilter/ipset/ip_set_hash_net.ko
kernel/net/netfilter/ipset/ip_set_hash_netport.ko
kernel/net/netfilter/ipset/ip_set_list_set.ko
kernel/net/netfilter/ipvs/ip_vs.ko
kernel/net/netfilter/ipvs/ip_vs_rr.ko
kernel/net/netfilter/ipvs/ip_vs_wrr.ko
kernel/net/netfilter/ipvs/ip_vs_lc.ko
kernel/net/netfilter/ipvs/ip_vs_wlc.ko
kernel/net/netfilter/ipvs/ip_vs_lblc.ko
kernel/net/netfilter/ipvs/ip_vs_lblcr.ko
kernel/net/netfilter/ipvs/ip_vs_dh.ko
kernel/net/netfilter/ipvs/ip_vs_sh.ko
kernel/net/netfilter/ipvs/ip_vs_sed.ko
kernel/net/netfilter/ipvs/ip_vs_nq.ko
kernel/net/netfilter/ipvs/ip_vs_ftp.ko
kernel/net/ipv4/netfilter/nf_conntrack_ipv4.ko
kernel/net/ipv4/netfilter/nf_nat.ko
kernel/net/ipv4/netfilter/nf_defrag_ipv4.ko
kernel/net/ipv4/netfilter/nf_nat_amanda.ko
kernel/net/ipv4/netfilter/nf_nat_ftp.ko
kernel/net/ipv4/netfilter/nf_nat_h323.ko
kernel/net/ipv4/netfilter/nf_nat_irc.ko
kernel/net/ipv4/netfilter/nf_nat_pptp.ko
kernel/net/ipv4/netfilter/nf_nat_sip.ko
kernel/net/ipv4/netfilter/nf_nat_snmp_basic.ko
kernel/net/ipv4/netfilter/nf_nat_tftp.ko
kernel/net/ipv4/netfilter/nf_nat_proto_dccp.ko
kernel/net/ipv4/netfilter/nf_nat_proto_gre.ko
kernel/net/ipv4/netfilter/nf_nat_proto_udplite.ko
kernel/net/ipv4/netfilter/nf_nat_proto_sctp.ko
kernel/net/ipv4/netfilter/ip_tables.ko
kernel/net/ipv4/netfilter/iptable_filter.ko
kernel/net/ipv4/netfilter/iptable_mangle.ko
kernel/net/ipv4/netfilter/iptable_nat.ko
kernel/net/ipv4/netfilter/iptable_raw.ko
kernel/net/ipv4/netfilter/iptable_security.ko
kernel/net/ipv4/netfilter/ipt_addrtype.ko
kernel/net/ipv4/netfilter/ipt_ah.ko
kernel/net/ipv4/netfilter/ipt_ecn.ko
kernel/net/ipv4/netfilter/ipt_CLUSTERIP.ko
kernel/net/ipv4/netfilter/ipt_ECN.ko
kernel/net/ipv4/netfilter/ipt_LOG.ko
kernel/net/ipv4/netfilter/ipt_MASQUERADE.ko
kernel/net/ipv4/netfilter/ipt_NETMAP.ko
kernel/net/ipv4/netfilter/ipt_REDIRECT.ko
kernel/net/ipv4/netfilter/ipt_REJECT.ko
kernel/net/ipv4/netfilter/ipt_ULOG.ko
kernel/net/ipv4/netfilter/arp_tables.ko
kernel/net/ipv4/netfilter/arpt_mangle.ko
kernel/net/ipv4/netfilter/arptable_filter.ko
kernel/net/ipv4/netfilter/ip_queue.ko
kernel/net/ipv4/ipip.ko
kernel/net/ipv4/ip_gre.ko
kernel/net/ipv4/ah4.ko
kernel/net/ipv4/esp4.ko
kernel/net/ipv4/ipcomp.ko
kernel/net/ipv4/xfrm4_tunnel.ko
kernel/net/ipv4/xfrm4_mode_beet.ko
kernel/net/ipv4/tunnel4.ko
kernel/net/ipv4/xfrm4_mode_transport.ko
kernel/net/ipv4/xfrm4_mode_tunnel.ko
kernel/net/ipv4/inet_diag.ko
kernel/net/ipv4/tcp_diag.ko
kernel/net/ipv4/tcp_bic.ko
kernel/net/ipv4/tcp_westwood.ko
kernel/net/ipv4/tcp_highspeed.ko
kernel/net/ipv4/tcp_hybla.ko
kernel/net/ipv4/tcp_htcp.ko
kernel/net/ipv4/tcp_vegas.ko
kernel/net/ipv4/tcp_veno.ko
kernel/net/ipv4/tcp_scalable.ko
kernel/net/ipv4/tcp_lp.ko
kernel/net/ipv4/tcp_yeah.ko
kernel/net/ipv4/tcp_illinois.ko
kernel/net/xfrm/xfrm_ipcomp.ko
kernel/net/ipv6/netfilter/ip6_tables.ko
kernel/net/ipv6/netfilter/ip6table_filter.ko
kernel/net/ipv6/netfilter/ip6table_mangle.ko
kernel/net/ipv6/netfilter/ip6_queue.ko
kernel/net/ipv6/netfilter/ip6table_raw.ko
kernel/net/ipv6/netfilter/ip6table_security.ko
kernel/net/ipv6/netfilter/nf_conntrack_ipv6.ko
kernel/net/ipv6/netfilter/nf_defrag_ipv6.ko
kernel/net/ipv6/netfilter/ip6t_ah.ko
kernel/net/ipv6/netfilter/ip6t_eui64.ko
kernel/net/ipv6/netfilter/ip6t_frag.ko
kernel/net/ipv6/netfilter/ip6t_ipv6header.ko
kernel/net/ipv6/netfilter/ip6t_mh.ko
kernel/net/ipv6/netfilter/ip6t_hbh.ko
kernel/net/ipv6/netfilter/ip6t_rt.ko
kernel/net/ipv6/netfilter/ip6t_LOG.ko
kernel/net/ipv6/netfilter/ip6t_REJECT.ko
kernel/net/ipv6/ipv6.ko
kernel/net/ipv6/ah6.ko
kernel/net/ipv6/esp6.ko
kernel/net/ipv6/ipcomp6.ko
kernel/net/ipv6/xfrm6_tunnel.ko
kernel/net/ipv6/tunnel6.ko
kernel/net/ipv6/xfrm6_mode_transport.ko
kernel/net/ipv6/xfrm6_mode_tunnel.ko
kernel/net/ipv6/xfrm6_mode_ro.ko
kernel/net/ipv6/xfrm6_mode_beet.ko
kernel/net/ipv6/mip6.ko
kernel/net/ipv6/sit.ko
kernel/net/ipv6/ip6_tunnel.ko
kernel/net/8021q/8021q.ko
kernel/net/wireless/cfg80211.ko
kernel/net/wireless/lib80211.ko
kernel/net/wireless/lib80211_crypt_wep.ko
kernel/net/wireless/lib80211_crypt_ccmp.ko
kernel/net/wireless/lib80211_crypt_tkip.ko
kernel/net/ieee802154/nl802154.ko
kernel/net/ieee802154/af_802154.ko
kernel/net/ieee802154/wpan-class.ko
kernel/net/llc/llc.ko
kernel/net/key/af_key.ko
kernel/net/bridge/bridge.ko
kernel/net/bridge/netfilter/ebtables.ko
kernel/net/bridge/netfilter/ebtable_broute.ko
kernel/net/bridge/netfilter/ebtable_filter.ko
kernel/net/bridge/netfilter/ebtable_nat.ko
kernel/net/bridge/netfilter/ebt_802_3.ko
kernel/net/bridge/netfilter/ebt_among.ko
kernel/net/bridge/netfilter/ebt_arp.ko
kernel/net/bridge/netfilter/ebt_ip.ko
kernel/net/bridge/netfilter/ebt_ip6.ko
kernel/net/bridge/netfilter/ebt_limit.ko
kernel/net/bridge/netfilter/ebt_mark_m.ko
kernel/net/bridge/netfilter/ebt_pkttype.ko
kernel/net/bridge/netfilter/ebt_stp.ko
kernel/net/bridge/netfilter/ebt_vlan.ko
kernel/net/bridge/netfilter/ebt_arpreply.ko
kernel/net/bridge/netfilter/ebt_mark.ko
kernel/net/bridge/netfilter/ebt_dnat.ko
kernel/net/bridge/netfilter/ebt_redirect.ko
kernel/net/bridge/netfilter/ebt_snat.ko
kernel/net/bridge/netfilter/ebt_log.ko
kernel/net/bridge/netfilter/ebt_ulog.ko
kernel/net/bridge/netfilter/ebt_nflog.ko
kernel/net/can/can.ko
kernel/net/can/can-raw.ko
kernel/net/can/can-bcm.ko
kernel/net/bluetooth/bluetooth.ko
kernel/net/bluetooth/l2cap.ko
kernel/net/bluetooth/sco.ko
kernel/net/bluetooth/rfcomm/rfcomm.ko
kernel/net/bluetooth/bnep/bnep.ko
kernel/net/bluetooth/cmtp/cmtp.ko
kernel/net/bluetooth/hidp/hidp.ko
kernel/net/sunrpc/sunrpc.ko
kernel/net/sunrpc/auth_gss/auth_rpcgss.ko
kernel/net/sunrpc/auth_gss/rpcsec_gss_krb5.ko
kernel/net/sunrpc/auth_gss/rpcsec_gss_spkm3.ko
kernel/net/sunrpc/xprtrdma/xprtrdma.ko
kernel/net/sunrpc/xprtrdma/svcrdma.ko
kernel/net/atm/atm.ko
kernel/net/atm/clip.ko
kernel/net/atm/br2684.ko
kernel/net/atm/lec.ko
kernel/net/atm/pppoatm.ko
kernel/net/phonet/phonet.ko
kernel/net/phonet/pn_pep.ko
kernel/net/dccp/dccp.ko
kernel/net/dccp/dccp_ipv4.ko
kernel/net/dccp/dccp_ipv6.ko
kernel/net/dccp/dccp_diag.ko
kernel/net/dccp/dccp_probe.ko
kernel/net/sctp/sctp.ko
kernel/net/rds/rds.ko
kernel/net/rds/rds_rdma.ko
kernel/net/rds/rds_tcp.ko
kernel/net/mac80211/mac80211.ko
kernel/net/rfkill/rfkill.ko
kernel/net/9p/9pnet.ko
kernel/net/9p/9pnet_virtio.ko
kernel/net/9p/9pnet_rdma.ko
kernel/net/wimax/wimax.ko
kernel/lib/crc-ccitt.ko
kernel/lib/crc-t10dif.ko
kernel/lib/crc-itu-t.ko
kernel/lib/crc7.ko
kernel/lib/libcrc32c.ko
kernel/lib/zlib_deflate/zlib_deflate.ko
kernel/lib/reed_solomon/reed_solomon.ko
kernel/lib/lzo/lzo_compress.ko
kernel/lib/lzo/lzo_decompress.ko
kernel/lib/raid6/raid6_pq.ko
kernel/lib/ts_kmp.ko
kernel/lib/ts_bm.ko
kernel/lib/ts_fsm.ko

```

## 第 6 章 Package Management

## 1. APT 包管理

包管理工具

### 1.1. apt-cache

#### 1.1.1. search

```

# apt-cache search tcpdump

```

#### 1.1.2. depends

```
# apt-cache depends tcpdump

```

#### 1.1.3. policy

```
$ apt-cache policy tcpdump
tcpdump:
  Installed: 4.0.0-6ubuntu3
  Candidate: 4.0.0-6ubuntu3
  Version table:
 *** 4.0.0-6ubuntu3 0
        500 http://us.archive.ubuntu.com/ubuntu/ lucid/main Packages
        100 /var/lib/dpkg/status

```

#### 1.1.4. 查看所属镜像

```

neo@m-1d41c853af58:~$ apt-cache madison docker
    docker |      1.5-2 | http://mirrors.ustc.edu.cn/ubuntu cosmic/universe amd64 Packages
    docker |      1.5-2 | http://mirrors.ustc.edu.cn/ubuntu cosmic/universe i386 Packages			

```

### 1.2. Apt-Get

apt 命令默认从 cdrom 安装

注释/etc/apt/sources.list 中的 deb cdrom 项, apt 会从互联网上安装

```
netkiller@Linux-server:~$ sudo vi /etc/apt/sources.list
# deb cdrom:[Ubuntu-Server 6.10 _Edgy Eft_ - Release i386 (20061025.1)]/ edgy main restricted

```

**apt-setup**

安装是首先会下载包到/var/cache/apt/archives/目录

#### 1.2.1. Search

Searching a software package

```
$ apt-cache search package

```

列出软件包的详细信息:

```
$ apt-cache show package

```

列出软件包的依赖关系:

```
$ apt-cache depends package

```

列出软件包, 以及逆向依赖的软件包的详细版本信息:

```
$ apt-cache showpkg package

```

#### 1.2.2. Installation

```
$ sudo apt install package

```

*.deb

```
sudo dpkg -i *.deb

```

#### 1.2.3. Update

```
$ apt update
$ apt upgrade

```

Smart software update

```
$ apt dist-upgrade

```

#### 1.2.4. Remove

删除系统中的 foo 软件包

```
$ sudo apt remove foo

```

删除系统中的 package 软件包及其配置文件

```
$ sudo apt remove --purge package

```

#### 1.2.5. purge

```
sudo apt purge package			

```

### 1.3. aptitude

管理软件包

```
neo@kerberos:~$ tasksel
neo@kerberos:~$ aptitude

```

### 1.4. Automatic Updates

```
sudo apt-get install unattended-upgrades

```

/etc/apt/apt.conf.d/50unattended-upgrades

Notifications

```
sudo apt-get install apticron

```

/etc/apticron/apticron.conf

```
EMAIL="root@example.com"

```

#### 1.4.1. 升级过程中链接中断怎么办?

```
Ubuntu 16.04 升级到 16.10 过程中 SSH 中断

我猜测 do-release-upgrade 一定会有恢复方案，应该是 screen. 经过查看果然是 screen

开始尝试恢复 screen 提示

neo@netkiller:~$ screen -ls
No Sockets found in /var/run/screen/S-neo.

后来想到应该是 root 而不是当前用户，再次查看

neo@netkiller:~$ sudo screen -ls
[sudo] password for neo:
There is a screen on:
 1955.ubuntu-release-upgrade-screen-window (11/25/2016 07:44:50 PM)       (Detached)
1 Socket in /var/run/screen/S-root.

的确如猜测一样，现在回复窗口吧。

neo@netkiller:~$ sudo screen -r 1955

继续			

```

### 1.5. 更换 api 源镜像

备份 /etc/apt/sources.list 文件，然后覆盖即可

```

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse		

```

#### 1.5.1. Ubuntu 18.04

```

sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
cat > /etc/apt/sources.list << EOF 
deb http://mirrors.aliyun.com/ubuntu/ xenial main 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main 
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main 
deb http://mirrors.aliyun.com/ubuntu/ xenial universe 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe 
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe 
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main 
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe 
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe 
EOF			

```

### 1.6. dpkg

#### 1.6.1. -i|--install 安装.deb 包

```
$ sudo dpkg -i netkiller-1.0.deb

```

#### 1.6.2. -r|--remove 卸载.deb 包

```
$ sudo dpkg -r netkiller

```

#### 1.6.3. -L|--listfiles <package> ... List files `owned' by package(s). 列出包中的文件

```
$ dpkg -L netkiller|more
/.
/opt
/opt/neo
/opt/neo/netkiller-1.0
/opt/neo/netkiller-1.0/linux
/opt/neo/netkiller-1.0/linux/docbook.css
/opt/neo/netkiller-1.0/linux/apas03.html
/opt/neo/netkiller-1.0/linux/shell

```

#### 1.6.4. -l|--list [<pattern> ...] List packages concisely. 列出.deb 包

```

$ sudo dpkg -l netkiller
$ dpkg -l netkiller
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name           Version        Description
+++-==============-==============-============================================
un  netkiller      <none>         (no description available)

```

#### 1.6.5. Status

系统上装了哪些软件包

```
要查看 Debian 系统上安装的所有软件包的状态, 运行

     dpkg --list

输出每个软件包的一行简单介绍, 2 字符的状态标志, 包名, 所安装版本, 和简要描述.

查看以 "foo" 开头的软件包的状态, 执行:

     dpkg --list 'foo*'

要得到某个软件包的更详细信息, 执行:

     dpkg --status packagename

```

List of installed software packages

```
$ dpkg-query -W

```

Description of installed software packages

```
$ dpkg -l

```

找出一个文件的归属包

```
dpkg --search cachemgr
squid3-cgi: /usr/lib/cgi-bin/cachemgr3.cgi
squid3-cgi: /usr/share/man/man8/cachemgr3.cgi.8.gz
squid3-cgi: /etc/squid3/cachemgr.conf

```

#### 1.6.6. dpkg-deb - Debian package archive (.deb) manipulation tool

##### 1.6.6.1. -X, --vextract archive directory Extract and display the filenames contained by a package

```
$ dpkg -X dmd_2.057-0_amd64.deb dmd_2.057-0_amd64

```

##### 1.6.6.2. -e, --control archive [directory] Extracts the control information files from a package archive into the specified directory.

```
$ dpkg -e dmd_2.057-0_amd64.deb

$ find DEBIAN/
DEBIAN/
DEBIAN/conffiles
DEBIAN/md5sum
DEBIAN/control

```

##### 1.6.6.3. -b, --build directory [archive|directory]

在你的目录下创建 DEBIAN 目录与 control 文件

```

mkdir DEBIAN/

cat >> DEBIAN/control <<EOF
Package: netkiller
Version: 1.0-0
Architecture: amd64
Maintainer: Neo Chen <netkiller@msn.com>
Installed-Size: 51196
Depends: libc6-dev, gcc, gcc-multilib, libc6 (>= 2.11), libgcc1 (>= 1:4.1.1), libstdc++6 (>= 4.1.1)
Section: devel
Priority: optional
Description: Netkiller ebook
 .
 Main designer: Neo Chen
 .
 Homepage: http://netkiller.github.com/
 .
EOF

```

```

$ dpkg -b dlang dlang.deb
dpkg-deb: building package `netkiller' in `dlang.deb'.		

$ dpkg --info dlang.deb
 new debian package, version 2.0.
 size 263266 bytes: control archive= 371 bytes.
     354 bytes,    14 lines      control              
 Package: netkiller
 Version: 1.0-0
 Architecture: amd64
 Maintainer: Neo Chen <netkiller@msn.com>
 Installed-Size: 51196
 Depends: libc6-dev, gcc, gcc-multilib, libc6 (>= 2.11), libgcc1 (>= 1:4.1.1), libstdc++6 (>= 4.1.1)
 Section: devel
 Priority: optional
 Description: Netkiller ebook
  .
  Main designer: Neo Chen
  .
  Homepage: http://netkiller.github.com/
  .

$ dpkg --contents dlang.deb
drwxr-xr-x neo/neo           0 2012-02-06 11:22 ./
-rw-r--r-- neo/neo         144 2012-02-01 16:35 ./hello.lst
-rwxr-xr-x neo/neo         321 2012-01-08 21:25 ./test.d
-rw-r--r-- neo/neo         207 2012-02-01 15:57 ./d4py.d
-rwxr-xr-x neo/neo      919366 2012-02-01 16:28 ./hello
-rw-r--r-- neo/neo        6452 2012-02-01 16:28 ./hello.o
-rwxr--r-- neo/neo          80 2012-01-08 21:28 ./hello.d

```

#### 1.6.7. dpkg-reconfigure

```
$ sudo dpkg-reconfigure package

```

所有未完成的配置步骤，主要是你在安装过程中出现中断后，可以使用下面命令补救。

```
$ sudo dpkg --configure -a			

```

### 1.7. Upgrading

升级到最新开发版

#### 1.7.1. GUI

```
update-manager --devel-release

```

#### 1.7.2. CLI

```
$ sudo do-release-upgrade

$ lsb_release -a

```

升级到最新开发版

```
vim /etc/update-manager/release-upgrades 文件，把里面的
Prompt=lts
改为
Prompt=normal

```

```
sudo do-release-upgrade -d

```

#### 1.7.3. CDROM

```
$ sudo mount -t iso9660 -o loop ~/maverick-alternate-i386.iso /cdrom
$ sudo /cdrom/cdromupgrade

```

### 1.8. 制作.deb 安装包

#### 1.8.1. checkinstall — Track installation of local software, and produce a binary manageable with your package management software.

```
$ tar xxx.tar.gz
$ cd xxx
$ ./configure
$ make			

```

```
$ sudo apt-get install checkinstall		

```

#### 1.8.2. dh_make - prepare Debian packaging for an original source archive

http://www.debian.org/doc/manuals/maint-guide/index.zh-cn.html

http://www.debian.org/doc/manuals/maint-guide/

#### 1.8.3. control

Architecture: any | amd64 | i386 The generated binary package is an architecture dependent one usually in a compiled language.

Architecture: all The generated binary package is an architecture independent one usually consisting of text, images, or scripts in an interpreted language.

## 2. snap - Tool to interact with snaps

### 2.1. 安装 snap

```

[root@netkiller test]# yum info snapd
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Available Packages
Name        : snapd
Arch        : x86_64
Version     : 2.37.4
Release     : 2.el7
Size        : 14 M
Repo        : epel/x86_64
Summary     : A transactional software package manager
URL         : https://github.com/snapcore/snapd
License     : GPLv3
Description : Snappy is a modern, cross-distribution, transactional package manager
            : designed for working with self-contained, immutable packages.

[root@netkiller ~]# yum install -y snapd		
[root@netkiller ~]# systemctl enable --now snapd.socket
Created symlink from /etc/systemd/system/sockets.target.wants/snapd.socket to /usr/lib/systemd/system/snapd.socket.
[root@netkiller ~]# systemctl start snapd

[root@netkiller ~]# snap install hello-world
2019-03-09T11:44:14+08:00 INFO Waiting for restart...
hello-world 6.3 from Canonical✓ installed

[root@netkiller ~]# snap list
Name         Version    Rev   Tracking  Publisher   Notes
core         16-2.37.2  6405  stable    canonical✓  core
hello-world  6.3        27    stable    canonical✓  -

```

### 2.2. 列出已经安装的 snap 包

```

neo@ubuntu:~$ snap list
Name  Version    Rev   Tracking  Publisher   Notes
core  16-2.37.2  6405  stable    canonical✓  core
go    1.12       3318  stable    mwhudson    classic		

```

### 2.3. 搜索要安装的 snap 包

```

sudo snap find <text to search>		

```

### 2.4. 安装 snap 包

```

sudo snap install <snap name>

```

### 2.5. 更新 snap 包

更新 snap 包，如果你后面不加包的名字的话那就是更新所有的 snap 包

```

sudo snap refresh <snap name>		

```

### 2.6. 把一个包还原到以前安装的版本

```

sudo snap revert <snap name>		

```

### 2.7. 删除 snap 包

删除一个 snap 包

```

sudo snap remove <snap name>

```

### 2.8. 查询最近做的操作日志

```

$ snap changes		

```

```

neo@ubuntu:~$ snap changes
ID   Status  Spawn               Ready               Summary
2    Done    today at 11:11 CST  today at 12:15 CST  Install "go" snap
3    Done    today at 11:11 CST  today at 11:11 CST  Initialize device		

```

## 3. DNF 包管理

### 3.1. 安装 epel-release 包

#### Extra Packages for Enterprise Linux repository configuration

使用下面命令安装企业版扩展包

```

dnf -y install epel-release		

```

安装演示

```

[root@localhost ~]# dnf search epel-release
Last metadata expiration check: 0:01:57 ago on Thu 05 Dec 2019 09:06:55 PM CST.
==================================================================================================== Name Exactly Matched: epel-release ====================================================================================================
epel-release.noarch : Extra Packages for Enterprise Linux repository configuration
[root@localhost ~]# 
[root@localhost ~]# dnf -y install epel-release
Last metadata expiration check: 0:02:41 ago on Thu 05 Dec 2019 09:06:55 PM CST.
Dependencies resolved.
============================================================================================================================================================================================================================================
 Package                                                      Arch                                                   Version                                                   Repository                                              Size
============================================================================================================================================================================================================================================
Installing:
 epel-release                                                 noarch                                                 8-5.el8                                                   extras                                                  22 k

Transaction Summary
============================================================================================================================================================================================================================================
Install  1 Package

Total download size: 22 k
Installed size: 30 k
Downloading Packages:
epel-release-8-5.el8.noarch.rpm                                                                                                                                                                              16 kB/s |  22 kB     00:01    
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                       7.5 kB/s |  22 kB     00:02     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                    1/1 
  Installing       : epel-release-8-5.el8.noarch                                                                                                                                                                                        1/1 
  Running scriptlet: epel-release-8-5.el8.noarch                                                                                                                                                                                        1/1 
  Verifying        : epel-release-8-5.el8.noarch                                                                                                                                                                                        1/1 

Installed:
  epel-release-8-5.el8.noarch                                                                                                                                                                                                               

Complete!

```

### 3.2. DNF 软件库管理

```

[root@localhost ~]# dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
Adding repo from: https://download.docker.com/linux/centos/docker-ce.repo	

```

### 3.3. 显示系统中可用的 DNF 软件库

```

[root@localhost ~]# dnf repolist
Last metadata expiration check: 0:00:25 ago on Sat 23 Nov 2019 11:06:18 AM EST.
repo id             repo name                                                 status
AppStream           CentOS-8 - AppStream                                      5,089
BaseOS              CentOS-8 - Base                                           2,843
*epel               Extra Packages for Enterprise Linux 8 - x86_64            3,328
extras              CentOS-8 - Extras                                             3

```

```

dnf repolist -v		

```

#### 3.3.1. 查看系统中所有的 DNF 软件库(包括禁用状态)

```

[root@localhost ~]# dnf repolist all
Last metadata expiration check: 0:01:45 ago on Sat 23 Nov 2019 11:06:18 AM EST.
repo id                   repo name                                   status
AppStream                 CentOS-8 - AppStream                        enabled: 5,089
AppStream-source          CentOS-8 - AppStream Sources                disabled
BaseOS                    CentOS-8 - Base                             enabled: 2,843
BaseOS-source             CentOS-8 - BaseOS Sources                   disabled
PowerTools                CentOS-8 - PowerTools                       disabled
base-debuginfo            CentOS-8 - Debuginfo                        disabled
c8-media-AppStream        CentOS-AppStream-8 - Media                  disabled
c8-media-BaseOS           CentOS-BaseOS-8 - Media                     disabled
centosplus                CentOS-8 - Plus                             disabled
centosplus-source         CentOS-8 - Plus Sources                     disabled
cr                        CentOS-8 - cr                               disabled
*epel                     Extra Packages for Enterprise Linux 8 - x86 enabled: 3,328
epel-debuginfo            Extra Packages for Enterprise Linux 8 - x86 disabled
epel-playground           Extra Packages for Enterprise Linux 8 - Pla disabled
epel-playground-debuginfo Extra Packages for Enterprise Linux 8 - Pla disabled
epel-playground-source    Extra Packages for Enterprise Linux 8 - Pla disabled
epel-source               Extra Packages for Enterprise Linux 8 - x86 disabled
epel-testing              Extra Packages for Enterprise Linux 8 - Tes disabled
epel-testing-debuginfo    Extra Packages for Enterprise Linux 8 - Tes disabled
epel-testing-source       Extra Packages for Enterprise Linux 8 - Tes disabled
extras                    CentOS-8 - Extras                           enabled:     3
extras-source             CentOS-8 - Extras Sources                   disabled
fasttrack                 CentOS-8 - fasttrack                        disabled

```

### 3.4. 列出所有 RPM 包

用于列出系统上所有软件包

```

[root@localhost ~]# dnf list |more
Last metadata expiration check: 0:04:15 ago on Sat 23 Nov 2019 11:06:18 AM EST.
Installed Packages
GeoIP.x86_64                                         1.5.0-14.el7                                            @System      
NetworkManager.x86_64                                1:1.18.0-5.el7_7.1                                      @System      
NetworkManager-libnm.x86_64                          1:1.18.0-5.el7_7.1                                      @System      
NetworkManager-team.x86_64                           1:1.18.0-5.el7_7.1                                      @System      
NetworkManager-tui.x86_64                            1:1.18.0-5.el7_7.1                                      @System      
NetworkManager-wifi.x86_64                           1:1.18.0-5.el7_7.1                                      @System      
acl.x86_64                                           2.2.51-14.el7                                           @System      
adwaita-cursor-theme.noarch                          3.28.0-1.el7                                            @System      
adwaita-icon-theme.noarch                            3.28.0-1.el7                                            @System      
aic94xx-firmware.noarch                              30-6.el7                                                @System      
alsa-firmware.noarch                                 1.0.28-2.el7                                            @System      
alsa-lib.x86_64                                      1.1.8-1.el7                                             @System      
alsa-tools-firmware.x86_64                           1.1.0-1.el7                                             @System      
at-spi2-atk.x86_64                                   2.26.2-1.el7                                            @System      
at-spi2-core.x86_64                                  2.28.0-1.el7                                            @System      
atk.x86_64                                           2.28.1-1.el7                                            @System      
audit.x86_64                                         2.8.5-4.el7                                             @System      
audit-libs.x86_64                                    2.8.5-4.el7                                             @System      
audit-libs-python.x86_64                             2.8.5-4.el7                                             @System      
authconfig.x86_64                                    6.2.8-30.el7                                            @System      
autoconf.noarch                                      2.69-11.el7                                             @System      
--More--

```

列出制定包

```

[root@localhost ~]# dnf list nginx
Last metadata expiration check: 0:10:05 ago on Sat 23 Nov 2019 11:06:18 AM EST.
Available Packages
nginx.x86_64                                   1:1.14.1-9.module_el8.0.0+184+e34fea82                                   AppStream		

```

#### 3.4.1. 查看已经安装包

用于列出系统上所有已经安装的软件包

```

[root@localhost ~]# dnf list installed | more
Installed Packages
GeoIP.x86_64                       1.5.0-14.el7                    @System      
NetworkManager.x86_64              1:1.18.0-5.el7_7.1              @System      
NetworkManager-libnm.x86_64        1:1.18.0-5.el7_7.1              @System      
NetworkManager-team.x86_64         1:1.18.0-5.el7_7.1              @System      
NetworkManager-tui.x86_64          1:1.18.0-5.el7_7.1              @System      
NetworkManager-wifi.x86_64         1:1.18.0-5.el7_7.1              @System      
acl.x86_64                         2.2.51-14.el7                   @System      
adwaita-cursor-theme.noarch        3.28.0-1.el7                    @System      
adwaita-icon-theme.noarch          3.28.0-1.el7                    @System      
aic94xx-firmware.noarch            30-6.el7                        @System      
alsa-firmware.noarch               1.0.28-2.el7                    @System      
alsa-lib.x86_64                    1.1.8-1.el7                     @System      
alsa-tools-firmware.x86_64         1.1.0-1.el7                     @System      
at-spi2-atk.x86_64                 2.26.2-1.el7                    @System      
at-spi2-core.x86_64                2.28.0-1.el7                    @System      
atk.x86_64                         2.28.1-1.el7                    @System      
audit.x86_64                       2.8.5-4.el7                     @System      
audit-libs.x86_64                  2.8.5-4.el7                     @System      
audit-libs-python.x86_64           2.8.5-4.el7                     @System      
authconfig.x86_64                  6.2.8-30.el7                    @System      
autoconf.noarch                    2.69-11.el7                     @System      
automake.noarch                    1.13.4-3.el7                    @System      
--More--

```

#### 3.4.2. 列出可用的软件包

```

[root@localhost ~]# dnf list available | more
Last metadata expiration check: 0:07:35 ago on Sat 23 Nov 2019 11:06:18 AM EST.
Available Packages
3proxy.x86_64                                        0.8.13-1.el8                                            epel     
BackupPC.x86_64                                      4.3.1-3.el8                                             epel     
BackupPC-XS.x86_64                                   0.59-3.el8                                              epel     
CGSI-gSOAP.x86_64                                    1.3.11-7.el8                                            epel     
CGSI-gSOAP-devel.x86_64                              1.3.11-7.el8                                            epel     
CUnit.i686                                           2.1.3-17.el8                                            AppStream
CUnit.x86_64                                         2.1.3-17.el8                                            AppStream
Field3D.x86_64                                       1.7.2-16.el8                                            epel     
Field3D-devel.x86_64                                 1.7.2-16.el8                                            epel     
GConf2.i686                                          3.2.6-22.el8                                            AppStream
GConf2.x86_64                                        3.2.6-22.el8                                            AppStream
GraphicsMagick.x86_64                                1.3.33-1.el8                                            epel     
GraphicsMagick-c++.x86_64                            1.3.33-1.el8                                            epel     
GraphicsMagick-c++-devel.x86_64                      1.3.33-1.el8                                            epel     
GraphicsMagick-devel.x86_64                          1.3.33-1.el8                                            epel     
GraphicsMagick-doc.noarch                            1.3.33-1.el8                                            epel     
GraphicsMagick-perl.x86_64                           1.3.33-1.el8                                            epel     
HepMC.x86_64                                         2.06.10-1.el8                                           epel     
HepMC-devel.x86_64                                   2.06.10-1.el8                                           epel     
HepMC-doc.noarch                                     2.06.10-1.el8                                           epel     
HepMC3.x86_64                                        3.1.2-1.el8                                             epel     
--More--

```

#### 3.4.3. 显示重复内容

```

dnf list docker-ce --showduplicates | sort -r

```

### 3.5. 搜索软件库中的包

```

[root@localhost ~]# dnf search mysql
Last metadata expiration check: 0:11:11 ago on Sat 23 Nov 2019 11:06:18 AM EST.
================================================= Name & Summary Matched: mysql =================================================
mysql.x86_64 : MySQL client programs and shared libraries
libnss-mysql.x86_64 : NSS library for MySQL
postfix-mysql.x86_64 : Postfix MySQL map support
rsyslog-mysql.x86_64 : MySQL support for rsyslog
collectd-mysql.x86_64 : MySQL plugin for collectd
libdbi-dbd-mysql.x86_64 : MySQL plugin for libdbi
dovecot-mysql.x86_64 : MySQL back end for dovecot
pdns-backend-mysql.x86_64 : MySQL backend for pdns
perl-DBD-MySQL.x86_64 : A MySQL interface for Perl
root-sql-mysql.x86_64 : MySQL client plugin for ROOT
freeradius-mysql.x86_64 : MySQL support for freeradius
voms-mysql-plugin.x86_64 : VOMS server plugin for MySQL
mysql-server.x86_64 : The MySQL server and related files
nagios-plugins-mysql.x86_64 : Nagios Plugin - check_mysql
zabbix40-web-mysql.noarch : Zabbix web frontend for MySQL
mysql-test.x86_64 : The test suite distributed with MySQL
python2-PyMySQL.noarch : Pure-Python MySQL client library
python3-PyMySQL.noarch : Pure-Python MySQL client library
apr-util-mysql.x86_64 : APR utility library MySQL DBD driver
qt5-qtbase-mysql.i686 : MySQL driver for Qt5's SQL classes
qt5-qtbase-mysql.x86_64 : MySQL driver for Qt5's SQL classes
rubygem-mysql2-doc.noarch : Documentation for rubygem-mysql2
zabbix40-proxy-mysql.x86_64 : Zabbix proxy compiled to use MySQL
mysql-devel.x86_64 : Files for development of MySQL applications
zabbix40-server-mysql.x86_64 : Zabbix server compiled to use MySQL
mysql-libs.x86_64 : The shared libraries required for MySQL clients
preludedb-mysql.x86_64 : Plugin to use prelude with a MySQL database
pcp-pmda-mysql.x86_64 : Performance Co-Pilot (PCP) metrics for MySQL
mysql-errmsg.x86_64 : The error messages files required by MySQL server
mysql80-community-release.noarch : MySQL repository configuration for yum
perl-DateTime-Format-MySQL.noarch : Parse and format MySQL dates and times
mysql-common.x86_64 : The shared files required for MySQL server and client
php-mysqlnd.x86_64 : A module for PHP applications that use MySQL databases
mysql-community-client.x86_64 : MySQL database client applications and tools
rubygem-mysql2.x86_64 : A simple, fast Mysql library for Ruby, binding to libmysql
mysql-community-libs.x86_64 : Shared libraries for MySQL database client applications
mysql-community-common.x86_64 : MySQL database common files for server and client libs
lighttpd-mod_mysql_vhost.x86_64 : Virtual host module for lighttpd that uses a MySQL database
lighttpd-mod_authn_mysql.x86_64 : Authentication module for lighttpd that uses a MySQL database
mysql-community-libs-compat.x86_64 : Shared compat libraries for MySQL 5.6.45 database client applications
====================================================== Name Matched: mysql ======================================================
zabbix40-dbfiles-mysql.noarch : Zabbix database schemas, images, data and patches
==================================================== Summary Matched: mysql =====================================================
innotop.noarch : A MySQL and InnoDB monitor program
mariadb-devel.x86_64 : Files for development of MariaDB/MySQL applications
mariadb-server-utils.x86_64 : Non-essential server utilities for MariaDB/MySQL applications
mariadb-java-client.noarch : Connects applications developed in Java to MariaDB and MySQL databases

```

### 3.6. 查看软件包详情

```

[root@localhost ~]# dnf info redis
Last metadata expiration check: 0:13:10 ago on Sat 23 Nov 2019 11:06:18 AM EST.
Available Packages
Name         : redis
Version      : 5.0.3
Release      : 1.module_el8.0.0+6+ab019c03
Arch         : x86_64
Size         : 927 k
Source       : redis-5.0.3-1.module_el8.0.0+6+ab019c03.src.rpm
Repo         : AppStream
Summary      : A persistent key-value database
URL          : http://redis.io
License      : BSD and MIT
Description  : Redis is an advanced key-value store. It is often referred to as a data
             : structure server since keys can contain strings, hashes, lists, sets and
             : sorted sets.
             : 
             : You can run atomic operations on these types, like appending to a string;
             : incrementing the value in a hash; pushing to a list; computing set
             : intersection, union and difference; or getting the member with highest
             : ranking in a sorted set.
             : 
             : In order to achieve its outstanding performance, Redis works with an
             : in-memory dataset. Depending on your use case, you can persist it either
             : by dumping the dataset to disk every once in a while, or by appending
             : each command to a log.
             : 
             : Redis also supports trivial-to-setup master-slave replication, with very
             : fast non-blocking first synchronization, auto-reconnection on net split
             : and so forth.
             : 
             : Other features include Transactions, Pub/Sub, Lua scripting, Keys with a
             : limited time-to-live, and configuration settings to make Redis behave like
             : a cache.
             : 
             : You can use Redis from most programming languages also.

```

### 3.7. 查找某一文件的提供者

```

[root@localhost ~]# dnf provides /bin/bash
Last metadata expiration check: 0:11:58 ago on Sat 23 Nov 2019 11:06:18 AM EST.
bash-4.2.46-33.el7.x86_64 : The GNU Bourne Again shell
Repo        : @System
Matched from:
Provide    : /bin/bash

bash-4.4.19-7.el8.i686 : The GNU Bourne Again shell
Repo        : BaseOS
Matched from:
Provide    : /bin/bash

bash-4.4.19-7.el8.x86_64 : The GNU Bourne Again shell
Repo        : BaseOS
Matched from:
Provide    : /bin/bash

bash-4.4.19-8.el8_0.x86_64 : The GNU Bourne Again shell
Repo        : BaseOS
Matched from:
Provide    : /bin/bash

```

### 3.8. 删除软件包

```

[root@localhost ~]# dnf remove nginx

```

## 4. yum - Yellowdog Updater Modified 包管理

### 4.1. Yum Resource & Yum Mirror

#### 4.1.1. fastestmirror

```

yum install yum-plugin-fastestmirror	

```

CentOS 5:

```

yum install yum-fastestmirror -y

```

#### 4.1.2. Fedora resource

http://fedoraproject.org/wiki/EPEL

##### 4.1.2.1. Fedora 5.4

5.4

```
rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm
rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm

```

##### 4.1.2.2. Fedora 6.x

6.x

```
rpm -Uvh http://download.fedora.redhat.com/pub/epel/6/i386/epel-release-6-5.noarch.rpm
rpm -Uvh http://download.fedora.redhat.com/pub/epel/6/x86_64/epel-release-6-5.noarch.rpm

```

上面的地址已经停用，新地址在：http://mirrors.fedoraproject.org/publiclist

```
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-7.noarch.rpm

```

epel-release-6-7.noarch.rpm 升级为 epel-release-6-8.noarch.rpm

```
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

```

##### 4.1.2.3. Fedora 7.x

http://ftp.cuhk.edu.hk/pub/linux/fedora-epel/7/x86_64/repoview/epel-release.html

```
yum localinstall -y http://ftp.cuhk.edu.hk/pub/linux/fedora-epel/7/x86_64/e/epel-release-7-5.noarch.rpm

```

#### 4.1.3. rpmforge-release

http://wiki.centos.org/AdditionalResources/Repositories/RPMForge

##### 4.1.3.1. CentOS 5.x

```
http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.i386.rpm
http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm

```

```
# wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm
# rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt
# rpm -K rpmforge-release-0.5.2-2.el5.rf.*.rpm
# rpm -i rpmforge-release-0.5.2-2.el5.rf.*.rpm

```

##### 4.1.3.2. CentOS 6.x

```
i686 http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.i686.rpm
x86_64 http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm

rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt
rpm -K http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm
rpm -i http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm

```

CentOS 6.5

```
http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.i686.rpm
http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm

```

##### 4.1.3.3. CentALT

http://centos.alt.ru

```
http://centos.alt.ru/repository/centos/5/i386/centalt-release-5-3.noarch.rpm
http://centos.alt.ru/repository/centos/5/x86_64/centalt-release-5-3.noarch.rpm

```

```
http://centos.alt.ru/repository/centos/6/i386/centalt-release-6-1.noarch.rpm
http://centos.alt.ru/repository/centos/6/x86_64/centalt-release-6-1.noarch.rpm

```

含 php-fpm 等包

```
rpm -Uvh http://centos.alt.ru/repository/centos/6/x86_64/centalt-release-6-1.noarch.rpm

```

#### 4.1.4. atomic

```
http://www6.atomicorp.com/channels/atomic/centos/5/x86_64/RPMS/atomic-release-1.0-14.el5.art.noarch.rpm

```

#### 4.1.5. famillecollet

```
rpm --import http://rpms.famillecollet.com/RPM-GPG-KEY-remi
rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

```

#### 4.1.6. rpmfind.net

http://rpmfind.net

#### 4.1.7. pkgs.org

http://pkgs.org/

#### 4.1.8. China Resource

[`mirrors.163.com/`](http://mirrors.163.com/) [`mirrors.sohu.com/`](http://mirrors.sohu.com/)

#### 4.1.9. 制作本地共享源

我使用 Ubuntu + vsftpd 为 Redhat 提供源

将光盘 Mount 到/mnt，或使用 iso 文件 Mount 到 /mnt

```
sudo mount /dev/cdrom /mnt/
or
sudo mount -o loop rhel-server-5.6-i386-dvd.iso /mnt

```

将整个光盘复制到 ftp 的 anonymous 目录或者 http 目录

```
sudo rsync -auvP /mnt/* /srv/ftp/

```

一般完整 DVD 光盘复制，不需要做此步骤。如果你的 RPM 看来自非官方，需要运行 createrepo

```
cd /srv/ftp/
sudo apt-get install createrepo
sudo createrepo -g repodata/comps-rhel5-server-core.xml Server

```

FTP 方式

```

cat > /etc/yum.repos.d/rhel-source-dvd.repo <<EOF
[rhel-source-dvd]
name=Red Hat Enterprise Linux $releasever - Source
baseurl=ftp://172.16.1.2/Server
enabled=1
gpgcheck=1
gpgkey=ftp://172.16.1.2/RPM-GPG-KEY-redhat-release
EOF

```

HTTP 方式

```

cat > /etc/yum.repos.d/rhel-source-dvd.repo <<EOF
[rhel-source-dvd]
name=Red Hat Enterprise Linux $releasever - Source
baseurl=http://172.16.1.2/Server
enabled=1
gpgcheck=1
gpgkey=http://172.16.1.2/RPM-GPG-KEY-redhat-release
EOF

```

还可以使用本地文件或者光盘 Mount 目录

```

cat > /etc/yum.repos.d/rhel-source-dvd.repo <<EOF
[rhel-source-dvd]
name=Red Hat Enterprise Linux $releasever - Source
baseurl=file:///mnt/Server
enabled=1
gpgcheck=1
gpgkey=file:///mnt/RPM-GPG-KEY-redhat-release
EOF

```

```
yum clean all
yum list updates

```

### 4.2. yum - Yellowdog Updater Modified

#### 4.2.1. YUM 源管理

列出所有 yum 源

```
# yum repolist all

```

查看启用 YUM 源

```
# yum repolist enabled

```

查看禁用 YUM 源

```
# yum repolist disabled

```

禁用 YUM 源

```
# yum-config-manager --disable mysql-connectors-community

```

启用 YUM 源

```
sudo yum-config-manager --enable mysql57-community-dmr

```

或者修改/etc/yum.repos.d/文件也能实现相同的作用 enabled=0 为禁用 enabled=1 启用

#### 4.2.2. install

有效的包名称

```
name
name.arch
name-ver
name-ver-rel
name-ver-rel.arch
name-epoch:ver-rel.arch
epoch:name-ver-rel.arch			

```

```
yum -y install package

```

指定 yum 源

```
yum -y install epel:nginx.x86_64			

```

reinstall

```
yum -y reinstall package

```

#### 4.2.3. localinstall

yum localinstall 可以代替 rpm -ivh 并且会自己安装依赖包

```
# yum localinstall asymptote-2.08-1.fc12.i686.rpm

```

#### 4.2.4. list

```

yum list

```

列出已经安装的包

```

yum list installed

yum list installed | wc -l

yum list installed ntp

yum list installed mysql\*

```

```
yum list updates
yum list extras

```

默认 yum 只显示最新版本的包，使用 --showduplicates 可以显示历史包

```
root@netkiller /var/log % yum --showduplicates list nginx | expand
Repository epel is listed more than once in the configuration
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
Installed Packages
nginx.x86_64                      1:1.12.1-1.el7.ngx                      @nginx
Available Packages
nginx.x86_64                      1:1.8.0-1.el7.ngx                       nginx 
nginx.x86_64                      1:1.8.1-1.el7.ngx                       nginx 
nginx.x86_64                      1:1.10.0-1.el7.ngx                      nginx 
nginx.x86_64                      1:1.10.1-1.el7.ngx                      nginx 
nginx.x86_64                      1:1.10.2-1.el7                          epel  
nginx.x86_64                      1:1.10.2-1.el7.ngx                      nginx 
nginx.x86_64                      1:1.10.3-1.el7.ngx                      nginx 
nginx.x86_64                      1:1.12.0-1.el7.ngx                      nginx 
nginx.x86_64                      1:1.12.1-1.el7.ngx                      nginx 

```

#### 4.2.5. search

```
yum search mysql

```

#### 4.2.6. update / upgrade

check update

```

[root@development ~]# yum check-update
[root@development ~]# yum -y update

```

upgrade

```
# yum upgrade

```

#### 4.2.7. remove

```
#yum remove httpd

```

#### 4.2.8. installed

```
# yum list installed

```

#### 4.2.9. group

##### 4.2.9.1. grouplist

```
[root@localhost ~]#  yum grouplist
Loaded plugins: fastestmirror
Setting up Group Process
Loading mirror speeds from cached hostfile
 * addons: mirrors.163.com
 * base: mirrors.163.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
Installed Groups:
   Administration Tools
   Development Libraries
   Dialup Networking Support
   Editors
   Mail Server
   Network Servers
   Office/Productivity
   Server Configuration Tools
   System Tools
   Text-based Internet
   Web Server
   Yum Utilities
Available Groups:
   Authoring and Publishing
   Base
   Beagle
   Cluster Storage
   Clustering
   DNS Name Server
   Development Tools
   Emacs
   Engineering and Scientific
   FTP Server
   FreeNX and NX
   GNOME Desktop Environment
   GNOME Software Development
   Games and Entertainment
   Graphical Internet
   Graphics
   Horde
   Java
   Java Development
   KDE (K Desktop Environment)
   KDE Software Development
   KVM
   Legacy Network Server
   Legacy Software Development
   Legacy Software Support
   Mono
   MySQL Database
   News Server
   OpenFabrics Enterprise Distribution
   PostgreSQL Database
   Printing Support
   Ruby
   Sound and Video
   Tomboy
   Virtualization
   Windows File Server
   X Software Development
   X Window System
   XFCE-4.4
Done

```

##### 4.2.9.2. groupinfo

```
# yum groupinfo "Server Configuration Tools"
Loaded plugins: fastestmirror
Setting up Group Process
Loading mirror speeds from cached hostfile
 * addons: centos.ustc.edu.cn
 * base: centos.ustc.edu.cn
 * extras: centos.ustc.edu.cn
 * updates: centos.ustc.edu.cn

Group: Server Configuration Tools
 Description: This group contains all of CentOS's custom server configuration tools.
 Default Packages:
   system-config-httpd
   system-config-nfs
   system-config-printer-gui
   system-config-samba
   system-config-securitylevel
   system-config-services
 Optional Packages:
   system-config-bind
   system-config-boot
   system-switch-mail-gnome

```

##### 4.2.9.3. groupinstall

```
#yum groupinstall 'X Window System'  -y

安装 GNOME 桌面环境
#yum groupinstall  'GNOME Desktop Environment' -y

安装 KDE 桌面环境
#yum groupinstall 'KDE (K Desktop Environment)' -y

```

##### 4.2.9.4. groupremove

```
卸载 GNOME 桌面环境
#yum groupremove "GNOME Desktop Environment"

卸载 KDE 桌面环境
#yum groupremove "KDE (K Desktop Environment)"

```

```
yum groupremove "GNOME Desktop Environment" "Games and Entertainment" "Graphical Internet" "Graphics" "Office/Productivity" "Printing Support" "Sound and Video" "Web Server" "X Window System"

```

#### 4.2.10. 查看包的依赖关系

```
# yum deplist libcurl

```

#### 4.2.11. provides / whatprovides

查询 pg_config 命令在那一个包中

```
# yum provides "*/pg_config"

```

```
# yum provides "*/libpq-fe.h"

```

```
# yum whatprovides mysql_config			

```

### 4.3. rpm - RPM Package Manager

#### 4.3.1. install/upgrade/remove

```
1.安装一个包
# rpm -ivh

2.升级一个包
# rpm -Uvh

3.删除一个包
# rpm -e

```

不检查依赖性关系

```
rpm -ivh --nodeps

```

强制安装

```
rpm -ivh --force --nodeps

```

##### 4.3.1.1. --prefix

安装到指定目录

```
rpm -ivh --prefix=/opt/usr your.rpm

```

同时修改多个路径：

```
rpm xxx.rpm --relocate=/usr=/opt/usr --relocate=/etc=/usr/etc

```

#### 4.3.2. query

查询一个包是否被安装

```
[root@database ~]# rpm -q mysql
mysql-5.0.77-3.el5
mysql-5.0.77-3.el5

```

安装的包的信息

```
[root@database ~]# rpm -qi nginx
Name        : nginx                        Relocations: (not relocatable)
Version     : 0.6.39                            Vendor: Fedora Project
Release     : 2.el5                         Build Date: Sat 05 Dec 2009 05:31:02 AM HKT
Install Date: Mon 28 Dec 2009 02:36:36 PM HKT      Build Host: x86-6.fedora.phx.redhat.com
Group       : System Environment/Daemons    Source RPM: nginx-0.6.39-2.el5.src.rpm
Size        : 772477                           License: BSD
Signature   : DSA/SHA1, Mon 07 Dec 2009 07:06:40 AM HKT, Key ID 119cc036217521f6
Packager    : Fedora Project
URL         : http://nginx.net/
Summary     : Robust, small and high performance http and reverse proxy server
Description :
Nginx [engine x] is an HTTP(S) server, HTTP(S) reverse proxy and IMAP/POP3
proxy server written by Igor Sysoev.

One third party module, nginx-upstream-fair, has been added.

```

列出该包中有哪些文件

```
[root@database ~]# rpm -ql cacti.noarch |more
/etc/cacti
/etc/cacti/db.php
/etc/cron.d/cacti
/etc/httpd/conf.d/cacti.conf
/etc/logrotate.d/cacti
/usr/share/cacti
/usr/share/cacti/about.php
/usr/share/cacti/auth_changepassword.php
/usr/share/cacti/auth_login.php
/usr/share/cacti/cdef.php
/usr/share/cacti/cmd.php
/usr/share/cacti/color.php
/usr/share/cacti/data_input.php
/usr/share/cacti/data_queries.php
/usr/share/cacti/data_sources.php
/usr/share/cacti/data_templates.php
/usr/share/cacti/gprint_presets.php
/usr/share/cacti/graph.php
/usr/share/cacti/graph_image.php
/usr/share/cacti/graph_settings.php
/usr/share/cacti/graph_templates.php
/usr/share/cacti/graph_templates_inputs.php
/usr/share/cacti/graph_templates_items.php

```

列出一个文件属于哪一个 RPM 包

```
[root@database ~]# rpm -qf /usr/bin/svn
subversion-1.4.2-4.el5_3.1

```

```
rpm -q --qf '%{NAME}-%{VERSION}-%{RELEASE} (%{ARCH})\n' \
gcc
gcc-c++

rpm -qa --qf '%{NAME} %{VENDOR}\n'

```

列出所有被安装的 RPM 包

```
[root@database ~]# rpm -qa |more
pciutils-devel-2.2.3-7.el5
rmt-0.4b41-4.el5
bsh-manual-1.3.0-9jpp.1
libgcc-4.1.2-46.el5
libICE-1.0.1-2.1
popt-1.10.2.3-18.el5
libXau-1.0.1-3.1
nspr-4.7.4-1.el5_3.1
libjpeg-6b-37
libogg-1.1.3-3.el5
libXdmcp-1.0.1-2.1
iproute-2.6.18-10.el5
libraw1394-1.3.0-1.el5
libbonobo-2.16.0-1.fc6
libavc1394-0.5.3-1.fc6
ttmkfdir-3.0.9-23.el5
cdrecord-2.01-10.7.el5
grep-2.5.1-55.el5
dmidecode-2.9-1.el5
nspr-4.7.4-1.el5_3.1
ncurses-5.5-24.20060715
libgcrypt-1.4.4-5.el5
keyutils-libs-1.2-1.el5

```

##### 4.3.2.1. changelog 查看变更日志

查看变更日志

```
rpm -q --changelog openssl-1.0.1e				

```

从变更日志中找出 CVE-2014-0160 漏洞的修复情况

```

$ rpm -q --changelog openssl-1.0.1e | grep -B 1 CVE-2014-0160
* Tue Apr 08 2014 Tomáš Mráz <tmraz@redhat.com> 1.0.1e-34
- fix CVE-2014-0160 - information disclosure in TLS heartbeat extension

```

### 4.4. rpmbuild - Build RPM Package(s)

安装 rpmbuild,我们将使用它来制作 rpm 包

```
yum search rpm-build
yum install -y rpm-build

```

Debian/Ubuntu: sudo apt-get install rpm

rpm 工作空间，默认是/usr/src/redhat/

```

mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}

echo "%_topdir /home/neo/rpmbuild" >> ~/.rpmmacros
echo "%packager Test User <test@example.com>" >> ~/.rpmmacros
cat ~/.rpmmacros

touch ~/rpmbuild/SPECS/package-1.0.spec

```

准备好你的文件包

```
tar zcvf %{name}-%{version}.tar.gz your_dir

```

编辑 spec 文件

```

vim ~/rpmbuild/SPECS/package-1.0.spec

Summary: My Test Package
Name: test
Version: 1.0
Release: 1.0
License: BSD
# group you want your package in, mostly for GUI package browsers
# some example groups used by vendors:
# http://www.rpmfind.net/linux/RPM/Groups.html
Group: Networking/Daemons
# your name for example
Packager: Neo Chen <openunix@163.com>
#
#Source: http://full.url.to.the/package/%{name}-%{version}.tar.gz
Source: %{name}-%{version}.tar.gz
# list all your patches here:
#Patch:
# list all packages required to build this package
#BuildRequires:
#Provides:
# list all packages that conflict with this one:
#BuildRoot: %{_tmppath}/%{name}-%{version}-build
BuildRoot: %{_tmppath}/%{name}-%{version}

####
# full length description
%description

description

#####
# this prepares a fresh build directory in ~/build/BUILD, useful macros here
# are:
# %setup - cleans any previous builds and untargzips the source
# %patch - applies patches
# any other commands here are executed as standard sh commands
%prep

%setup
#%patch

#####
# this tells rpmbuild how to build your package, rpmbuild runs it as a sh
# script
%build
#make

#####
# all the steps necessary to install your package into $RPM_BUILD_ROOT
# first step is to clear $RPM_BUILD_ROOT
%install
[ "$RPM_BUILD_ROOT" != "/" ] && rm -rf $RPM_BUILD_ROOT
cp -r ../* %{_tmppath}
#install all files under RPM_BUILD_ROOT
#make install DESTDIR=$RPM_BUILD_ROOT
# now you can remove uneeded stuff
#rm -f $RPM_BUILD_ROOT{_prefix}

#####
# NOTE: this section is optional
# commands run just before the package is installed
%pre
#/usr/sbin/useradd -c "test user" -r -s /bin/false -u 666 -d / neo 2> /dev/null

#####
# NOTE: this section is optional
# commands run before uninstalling the software
%preun
#/sbin/service test stop > /dev/null 2>&1
#/sbin/chkconfig --del test

#####
# NOTE: this section is optional
# commands run after installing the package
%post
#/sbin/chkconfig -add test
#touch /var/log/test

#####
# NOTE: this section is optional
# commands run after unistalling the package
%postun
#/sbin/service test stop
#/usr/sbin/userdel test

#####
# list all the files that are part of the package. If a file is not in the
# list rpmbuild won't put it in the package
# see below on how to automate the process of creating this list.
# some useful macros here:
# %doc /path/to/filename - installs filename into /path/to/filename and marks
# it as being documentation
# %config /etc/config_file - similar for configuration files
# %attr(mode, user, group) file - lets you specify file attributes applied
# during installation, use - if you want to use defaults
%files
#/usr/bin/test
#/usr/sbin/test
# this will package the dir and all directories inside it
#/example/of/a/dir
/srv/neo
# this will package only the 'dir' directory
#%dir /example/of/a/dir

#####
# document changes between package releases
%changelog

```

开始制作 rpm 文件

```
rpmbuild -ba ~/rpmbuild/SPECS/package-1.0.spec

```

查看你的 rpm 文件包含文件列表

```
rpm -qpl /usr/src/redhat/RPMS/x86_64/test-1.0-1.0.x86_64.rpm
/srv
/srv/bin
/srv/bin/console
/srv/bin/nodekeeper
/srv/etc
/srv/etc/commands.cfg
/srv/etc/nodekeeper.cfg
/srv/etc/protocol.cfg
/srv/logs
/srv/logs/nodekeeper.log
/srv/run
/srv/run/nodekeeper.pid

```

安装 RPM

```
# rpm -Uvh --nodeps /tmp/test-1.0-1.0.x86_64.rpm
Preparing...                ########################################### [100%]
   1:test                   ########################################### [100%]

```

也可以使用 yum 安装

```
yum localinstall /tmp/test-1.0-1.0.x86_64.rpm

```

查看安装信息

```

# rpm -qi test
Name        : test                         Relocations: (not relocatable)
Version     : 1.0                               Vendor: (none)
Release     : 1.0                           Build Date: Wed 21 Sep 2011 05:50:54 PM CST
Install Date: Wed 21 Sep 2011 05:46:50 PM CST      Build Host: dev3.example.com
Group       : Networking/Daemons            Source RPM: test-1.0-1.0.src.rpm
Size        : 20373                            License: BSD
Signature   : (none)
Packager    : Neo Chen <openunix@163.com>
Summary     : My Test Package
Description :

description

```

是使用 yum info 查看信息

```
# yum info test
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.163.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
Installed Packages
Name       : test
Arch       : x86_64
Version    : 1.0
Release    : 1.0
Size       : 20 k
Repo       : installed
Summary    : My Test Package
License    : BSD
Description:
           : description

```

#### 4.4.1. RPM_directory_macros

http://fedoraproject.org/wiki/Packaging/RPMMacros#RPM_directory_macros

```

%{_sysconfdir}        /etc

%{_prefix}            /usr

%{_exec_prefix}       %{_prefix}

%{_bindir}            %{_exec_prefix}/bin

%{_lib}               lib (lib64 on 64bit systems)

%{_libdir}            %{_exec_prefix}/%{_lib}

%{_libexecdir}        %{_exec_prefix}/libexec

%{_sbindir}           %{_exec_prefix}/sbin

%{_sharedstatedir}    /var/lib

%{_datadir}           %{_prefix}/share

%{_includedir}        %{_prefix}/include

%{_oldincludedir}     /usr/include

%{_infodir}           /usr/share/info

%{_mandir}            /usr/share/man

%{_localstatedir}     /var

%{_initddir}          %{_sysconfdir}/rc.d/init.d

Note: On releases older than Fedora 10 (and EPEL), %{_initddir} does not exist. Instead, you should use the deprecated%{_initrddir} macro.
RPM directory macros
%{_topdir}            %{getenv:HOME}/rpmbuild

%{_builddir}          %{_topdir}/BUILD

%{_rpmdir}            %{_topdir}/RPMS

%{_sourcedir}         %{_topdir}/SOURCES

%{_specdir}           %{_topdir}/SPECS

%{_srcrpmdir}         %{_topdir}/SRPMS

%{_buildrootdir}      %{_topdir}/BUILDROOT

Note: On releases older than Fedora 10 (and EPEL), %{_buildrootdir} does not exist.
Build flags macros
%{_global_cflags}     -O2 -g -pipe

%{_optflags}          %{__global_cflags} -m32 -march=i386 -mtune=pentium4 # if redhat-rpm-config is installed

Other macros
%{_var}               /var

%{_tmppath}           %{_var}/tmp

%{_usr}               /usr

%{_usrsrc}            %{_usr}/src

%{_docdir}            %{_datadir}/doc

```

#### 4.4.2. --define 专递模板变量

spec 文件中定义宏默认值

```

%define <variable> <value>

```

另一种是在外部传递变量值

```
rpmbuild -ba SPECS/bacula.spec --define "build_su110 1" --define "build_mysql4 1"

```

注意：当两种同时使用时，外部--define 无法替代%define 的定义。

#### 4.4.3. defattr

```

%defattr(-,root,root,-)		

```

#### 4.4.4. GPG 签名

创建证书

```

$ % gpg --gen-key			

```

查看 GPG 证书

```

$ gpg --list-keys
/home/neo/.gnupg/pubring.gpg
----------------------------
pub   1024R/63268A35 2013-09-11
uid                  Neo Chen (netkiller) <netkiller@msn.com>
sub   1024R/F4F946F9 2013-09-11

```

设置 _gpg_name 宏，与上面查看结果需一致

```

cat << EOF >> ~/.rpmmacros
%_signature gpg
%_gpg_name Neo Chen (netkiller) <netkiller@msn.com>
%_gpgpath ~/.gnupg
%_gpgbin /usr/bin/gpg
EOF

```

建立 RPM

```
rpmbuild --define "_topdir /path/to/macrodir" -bb --sign spec

```

如果你的证书有 Passphrase，会提示你输入了密码。

```
Enter pass phrase:
Pass phrase is good.

```

#### 4.4.5. 使用 CMake3 编译并创建 RPM 包

```

root@VM_7_221_centos ~/mysql-outfile-plugin % cat Outfile.spec 
Name: mysql-outfile-plugin		
Version: 1.0
Release:	1%{?dist}
Summary: MySQL outfile plugin	

Group: MySQL Database server
License: CC 3.0
URL: http://www.netkiller.cn	
Source0: https://github.com/netkiller/mysql-outfile-plugin/archive/v1.0.tar.gz

BuildRequires: cmake3 mysql-community-devel	
Requires: gcc	

%description

%prep
%setup -q

%build
cmake3 .
make %{?_smp_mflags}

%install
make install DESTDIR=%{buildroot}

%files
/usr/lib64/mysql/plugin/liboutfile.so
%doc

%changelog

```

#### 4.4.6. FAQ

error: File /home/neo/rpmbuild/SOURCES/netkiller-docbook-1.0.1.tar.gz: No such file or directory

Source 定义的文件不存在，如果你需要忽略 Source 可以使用 %setup -T

## 5. 清理安装包

```

package-cleanup --leaves
package-cleanup --orphans		

```

## 第 7 章 Device information 设备信息

## 1. dmesg - print or control the kernel ring buffer

**dmesg**

```
neo@shenzhen:~/doc/Linux/xhtml$ dmesg

```

## 2. smartctl - Control and Monitor Utility for SMART Disks

```
# smartctl -i /dev/sda
smartctl version 5.38 [x86_64-redhat-linux-gnu] Copyright (C) 2002-8 Bruce Allen
Home page is http://smartmontools.sourceforge.net/

=== START OF INFORMATION SECTION ===
Model Family:     Western Digital Caviar Second Generation Serial ATA family
Device Model:     WDC WD1600AAJS-75M0A0
Serial Number:    WD-WCAV35616755
Firmware Version: 02.03E02
User Capacity:    160,000,000,000 bytes
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   8
ATA Standard is:  Exact ATA specification draft version not indicated
Local Time is:    Wed May  5 13:05:18 2010 CST
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

```

如果 SMART support is: Disabled 使用下面命令启用

```
# smartctl --smart=on --offlineauto=on --saveauto=on /dev/hdb

```

健康情况

```
# smartctl -H /dev/sda
smartctl version 5.38 [x86_64-redhat-linux-gnu] Copyright (C) 2002-8 Bruce Allen
Home page is http://smartmontools.sourceforge.net/

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED

```

PASSED，这表示硬盘健康状态良好,Failure 最好立刻给服务器更换硬盘

使用 -a 参数显示所有信息

Seagate

```

[root@manager ~]# smartctl -a /dev/sda
smartctl 5.43 2012-06-30 r3573 [x86_64-linux-2.6.32-358.11.1.el6.x86_64] (local build)
Copyright (C) 2002-12 by Bruce Allen, http://smartmontools.sourceforge.net

=== START OF INFORMATION SECTION ===
Model Family:     Seagate Constellation ES (SATA)
Device Model:     ST31000524NS
Serial Number:    9WK4B5RH
LU WWN Device Id: 5 000c50 035116f81
Firmware Version: SN12
User Capacity:    1,000,204,886,016 bytes [1.00 TB]
Sector Size:      512 bytes logical/physical
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   8
ATA Standard is:  ATA-8-ACS revision 4
Local Time is:    Thu Dec 19 09:30:59 2013 HKT
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED

General SMART Values:
Offline data collection status:  (0x82)	Offline data collection activity
					was completed without error.
					Auto Offline Data Collection: Enabled.
Self-test execution status:      (   0)	The previous self-test routine completed
					without error or no self-test has ever 
					been run.
Total time to complete Offline 
data collection: 		(  617) seconds.
Offline data collection
capabilities: 			 (0x7b) SMART execute Offline immediate.
					Auto Offline data collection on/off support.
					Suspend Offline collection upon new
					command.
					Offline surface scan supported.
					Self-test supported.
					Conveyance Self-test supported.
					Selective Self-test supported.
SMART capabilities:            (0x0003)	Saves SMART data before entering
					power-saving mode.
					Supports SMART auto save timer.
Error logging capability:        (0x01)	Error logging supported.
					General Purpose Logging supported.
Short self-test routine 
recommended polling time: 	 (   1) minutes.
Extended self-test routine
recommended polling time: 	 ( 172) minutes.
Conveyance self-test routine
recommended polling time: 	 (   2) minutes.
SCT capabilities: 	       (0x10bd)	SCT Status supported.
					SCT Error Recovery Control supported.
					SCT Feature Control supported.
					SCT Data Table supported.

SMART Attributes Data Structure revision number: 10
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x000f   082   063   044    Pre-fail  Always       -       187232024
  3 Spin_Up_Time            0x0003   096   095   000    Pre-fail  Always       -       0
  4 Start_Stop_Count        0x0032   100   100   020    Old_age   Always       -       219
  5 Reallocated_Sector_Ct   0x0033   100   100   036    Pre-fail  Always       -       0
  7 Seek_Error_Rate         0x000f   078   060   030    Pre-fail  Always       -       65487640
  9 Power_On_Hours          0x0032   077   077   000    Old_age   Always       -       20444
 10 Spin_Retry_Count        0x0013   100   100   097    Pre-fail  Always       -       0
 12 Power_Cycle_Count       0x0032   100   100   020    Old_age   Always       -       125
184 End-to-End_Error        0x0032   100   100   099    Old_age   Always       -       0
187 Reported_Uncorrect      0x0032   100   100   000    Old_age   Always       -       0
188 Command_Timeout         0x0032   100   100   000    Old_age   Always       -       0
189 High_Fly_Writes         0x003a   100   100   000    Old_age   Always       -       0
190 Airflow_Temperature_Cel 0x0022   067   055   045    Old_age   Always       -       33 (Min/Max 24/39)
191 G-Sense_Error_Rate      0x0032   100   100   000    Old_age   Always       -       0
192 Power-Off_Retract_Count 0x0032   100   100   000    Old_age   Always       -       103
193 Load_Cycle_Count        0x0032   100   100   000    Old_age   Always       -       219
194 Temperature_Celsius     0x0022   033   045   000    Old_age   Always       -       33 (0 22 0 0 0)
195 Hardware_ECC_Recovered  0x001a   031   022   000    Old_age   Always       -       187232024
197 Current_Pending_Sector  0x0012   100   100   000    Old_age   Always       -       0
198 Offline_Uncorrectable   0x0010   100   100   000    Old_age   Offline      -       0
199 UDMA_CRC_Error_Count    0x003e   200   200   000    Old_age   Always       -       0

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
No self-tests have been logged.  [To run self-tests, use: smartctl -t]

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.

```

Western Digital

```

# smartctl -a /dev/sdb
smartctl 5.43 2012-06-30 r3573 [x86_64-linux-2.6.32-358.11.1.el6.x86_64] (local build)
Copyright (C) 2002-12 by Bruce Allen, http://smartmontools.sourceforge.net

=== START OF INFORMATION SECTION ===
Model Family:     Western Digital RE4 Serial ATA
Device Model:     WDC WD1003FBYX-01Y7B1
Serial Number:    WD-WMAW30176328
LU WWN Device Id: 5 0014ee 206836654
Firmware Version: 01.01V02
User Capacity:    1,000,204,886,016 bytes [1.00 TB]
Sector Size:      512 bytes logical/physical
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   8
ATA Standard is:  Exact ATA specification draft version not indicated
Local Time is:    Thu Dec 19 09:31:03 2013 HKT
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED

General SMART Values:
Offline data collection status:  (0x84)	Offline data collection activity
					was suspended by an interrupting command from host.
					Auto Offline Data Collection: Enabled.
Self-test execution status:      (   0)	The previous self-test routine completed
					without error or no self-test has ever 
					been run.
Total time to complete Offline 
data collection: 		(15900) seconds.
Offline data collection
capabilities: 			 (0x7b) SMART execute Offline immediate.
					Auto Offline data collection on/off support.
					Suspend Offline collection upon new
					command.
					Offline surface scan supported.
					Self-test supported.
					Conveyance Self-test supported.
					Selective Self-test supported.
SMART capabilities:            (0x0003)	Saves SMART data before entering
					power-saving mode.
					Supports SMART auto save timer.
Error logging capability:        (0x01)	Error logging supported.
					General Purpose Logging supported.
Short self-test routine 
recommended polling time: 	 (   2) minutes.
Extended self-test routine
recommended polling time: 	 ( 156) minutes.
Conveyance self-test routine
recommended polling time: 	 (   5) minutes.
SCT capabilities: 	       (0x303f)	SCT Status supported.
					SCT Error Recovery Control supported.
					SCT Feature Control supported.
					SCT Data Table supported.

SMART Attributes Data Structure revision number: 16
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x002f   200   200   051    Pre-fail  Always       -       0
  3 Spin_Up_Time            0x0027   180   175   021    Pre-fail  Always       -       3983
  4 Start_Stop_Count        0x0032   100   100   000    Old_age   Always       -       81
  5 Reallocated_Sector_Ct   0x0033   200   200   140    Pre-fail  Always       -       0
  7 Seek_Error_Rate         0x002e   200   200   000    Old_age   Always       -       0
  9 Power_On_Hours          0x0032   084   084   000    Old_age   Always       -       12404
 10 Spin_Retry_Count        0x0032   100   253   000    Old_age   Always       -       0
 11 Calibration_Retry_Count 0x0032   100   253   000    Old_age   Always       -       0
 12 Power_Cycle_Count       0x0032   100   100   000    Old_age   Always       -       79
192 Power-Off_Retract_Count 0x0032   200   200   000    Old_age   Always       -       70
193 Load_Cycle_Count        0x0032   200   200   000    Old_age   Always       -       10
194 Temperature_Celsius     0x0022   112   097   000    Old_age   Always       -       35
196 Reallocated_Event_Count 0x0032   200   200   000    Old_age   Always       -       0
197 Current_Pending_Sector  0x0032   200   200   000    Old_age   Always       -       0
198 Offline_Uncorrectable   0x0030   200   200   000    Old_age   Offline      -       0
199 UDMA_CRC_Error_Count    0x0032   200   200   000    Old_age   Always       -       0
200 Multi_Zone_Error_Rate   0x0008   200   200   000    Old_age   Offline      -       0

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
No self-tests have been logged.  [To run self-tests, use: smartctl -t]

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.		

```

## 3. CPU 资源管理

### 3.1. lscpu - display information about the CPU architecture

查看 CPU 信息

```
# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                1
On-line CPU(s) list:   0
Thread(s) per core:    1
Core(s) per socket:    1
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 13
Stepping:              3
CPU MHz:               2400.084
BogoMIPS:              4800.16
Hypervisor vendor:     KVM
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              4096K
NUMA node0 CPU(s):     0

```

### 3.2. chcpu - configure CPUs

禁用谋个 CPU(含超线程)

```
# chcpu -d 3
CPU 3 disabled

# lscpu -c --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
3   -    -      -    :::           no

# lscpu -b --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   0    0      1    1:1:1:0       yes
2   0    0      2    2:2:2:0       yes
4   0    1      3    3:3:3:1       yes
5   0    1      4    4:4:4:1       yes
6   0    1      5    5:5:5:1       yes
7   0    1      6    6:6:6:1       yes

# lscpu --all --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   0    0      1    1:1:1:0       yes
2   0    0      2    2:2:2:0       yes
3   -    -      -    :::           no
4   0    1      3    3:3:3:1       yes
5   0    1      4    4:4:4:1       yes
6   0    1      5    5:5:5:1       yes
7   0    1      6    6:6:6:1       yes

# chcpu -d 3
CPU 3 is already disabled

# chcpu -d 1
CPU 1 disabled

# chcpu -d 3
CPU 3 disabled

# chcpu -d 5
CPU 5 disabled

# chcpu -d 7
CPU 7 disabled

# lscpu --all --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   -    -      -    :::           no
2   0    0      1    1:1:1:0       yes
3   -    -      -    :::           no
4   0    1      2    2:2:2:1       yes
5   -    -      -    :::           no
6   0    1      3    3:3:3:1       yes
7   -    -      -    :::           no

```

启用谋个 CPU

```
# chcpu -e 3
CPU 3 enabled

# lscpu --all --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   0    0      1    1:1:1:0       yes
2   0    0      2    2:2:2:0       yes
3   0    0      3    3:3:3:0       yes
4   0    1      4    4:4:4:1       yes
5   0    1      5    5:5:5:1       yes
6   0    1      6    6:6:6:1       yes
7   0    1      7    7:7:7:1       yes

```

0 号 CPU 不允许禁用

```
# lscpu --all --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   -    -      -    :::           no
2   -    -      -    :::           no
3   -    -      -    :::           no
4   -    -      -    :::           no
5   -    -      -    :::           no
6   -    -      -    :::           no
7   -    -      -    :::           no

# chcpu -d 0
CPU 0 is not hot pluggable

```

1 号处于启用状态，0 号仍然不能禁用

```
# lscpu --all --extended
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE
0   0    0      0    0:0:0:0       yes
1   0    0      1    1:1:1:0       yes
2   -    -      -    :::           no
3   -    -      -    :::           no
4   -    -      -    :::           no
5   -    -      -    :::           no
6   -    -      -    :::           no
7   -    -      -    :::           no

# chcpu -d 0
CPU 0 is not hot pluggable

```

## 4. lspci - list all PCI devices

```
$ lspci
00:00.0 Host bridge: Intel Corporation 82945G/GZ/P/PL Memory Controller Hub (rev 02)
00:02.0 VGA compatible controller: Intel Corporation 82945G/GZ Integrated Graphics Controller (rev 02)
00:1b.0 Audio device: Intel Corporation 82801G (ICH7 Family) High Definition Audio Controller (rev 01)
00:1c.0 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 1 (rev 01)
00:1c.2 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 3 (rev 01)
00:1c.3 PCI bridge: Intel Corporation 82801G (ICH7 Family) PCI Express Port 4 (rev 01)
00:1d.0 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #1 (rev 01)
00:1d.1 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #2 (rev 01)
00:1d.2 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #3 (rev 01)
00:1d.3 USB Controller: Intel Corporation 82801G (ICH7 Family) USB UHCI Controller #4 (rev 01)
00:1d.7 USB Controller: Intel Corporation 82801G (ICH7 Family) USB2 EHCI Controller (rev 01)
00:1e.0 PCI bridge: Intel Corporation 82801 PCI Bridge (rev e1)
00:1f.0 ISA bridge: Intel Corporation 82801GB/GR (ICH7 Family) LPC Interface Bridge (rev 01)
00:1f.1 IDE interface: Intel Corporation 82801G (ICH7 Family) IDE Controller (rev 01)
00:1f.2 IDE interface: Intel Corporation 82801GB/GR/GH (ICH7 Family) SATA IDE Controller (rev 01)
00:1f.3 SMBus: Intel Corporation 82801G (ICH7 Family) SMBus Controller (rev 01)
01:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168B PCI Express Gigabit Ethernet controller (rev 02)
04:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL-8139/8139C/8139C+ (rev 10)

```

```
$ lspci -tv
-[0000:00]-+-00.0  Intel Corporation 82945G/GZ/P/PL Memory Controller Hub
           +-02.0  Intel Corporation 82945G/GZ Integrated Graphics Controller
           +-1b.0  Intel Corporation N10/ICH 7 Family High Definition Audio Controller
           +-1c.0-[0000:01]----00.0  Realtek Semiconductor Co., Ltd. RTL8111/8168B PCI Express Gigabit Ethernet controller
           +-1c.2-[0000:02]--
           +-1c.3-[0000:03]--
           +-1d.0  Intel Corporation N10/ICH7 Family USB UHCI Controller #1
           +-1d.1  Intel Corporation N10/ICH 7 Family USB UHCI Controller #2
           +-1d.2  Intel Corporation N10/ICH 7 Family USB UHCI Controller #3
           +-1d.3  Intel Corporation N10/ICH 7 Family USB UHCI Controller #4
           +-1d.7  Intel Corporation N10/ICH 7 Family USB2 EHCI Controller
           +-1e.0-[0000:04]----00.0  Realtek Semiconductor Co., Ltd. RTL-8139/8139C/8139C+
           +-1f.0  Intel Corporation 82801GB/GR (ICH7 Family) LPC Interface Bridge
           +-1f.1  Intel Corporation 82801G (ICH7 Family) IDE Controller
           +-1f.2  Intel Corporation N10/ICH7 Family SATA IDE Controller
           \-1f.3  Intel Corporation N10/ICH 7 Family SMBus Controller

```

## 5. lshw - list hardware

```
$ sudo lshw 
[sudo] password for neo: 
neo-presario-c700-notebook-pc
    description: Notebook
    product: Presario C700 Notebook PC (GS095PA#AB5)
    vendor: Hewlett-Packard
    version: F.08
    serial: CND7492C7R
    width: 64 bits
    capabilities: smbios-2.4 dmi-2.4 vsyscall32
    configuration: boot=normal chassis=notebook family=103C_5335KV sku=GS095PA#AB5 uuid=A7C95A0A-99FE-11DC-933C-001B38BAB11A
  *-core
       description: Motherboard
       product: 30D9
       vendor: Hewlett-Packard
       physical id: 0
       version: 83.19
       serial: CND7492C7R
       slot: Base Board Chassis Location
     *-firmware
          description: BIOS
          vendor: Hewlett-Packard
          physical id: 0
          version: F.08
          date: 09/13/2007
          size: 1MiB
          capabilities: pci upgrade shadowing cdboot bootselect socketedrom edd int13floppynec int13floppytoshiba int13floppy360 int13floppy1200 int13floppy720 int13floppy2880 int9keyboard int10video acpi usb
     *-cpu
          description: CPU
          product: Intel(R) Pentium(R) Dual  CPU  T2310  @ 1.46GHz
          vendor: Intel Corp.
          physical id: e
          bus info: cpu@0
          version: Intel(R) Pentium(R) Dual  CPU  T2310  @ 1.46GHz
          serial: NotSupport
          slot: CPU
          size: 800MHz
          capacity: 800MHz
          width: 64 bits
          clock: 533MHz
          capabilities: fpu fpu_exception wp vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx x86-64 constant_tsc arch_perfmon pebs bts rep_good nopl aperfmperf pni dtes64 monitor ds_cpl est tm2 ssse3 cx16 xtpr pdcm lahf_lm dtherm cpufreq
        *-cache:0
             description: L2 cache
             physical id: f
             slot: Unknown
             size: 1MiB
             capacity: 1MiB
             capabilities: asynchronous internal write-back unified
        *-cache:1
             description: L1 cache
             physical id: 11
             slot: Unknown
             size: 32KiB
             capacity: 32KiB
             capabilities: asynchronous internal write-back data
     *-cache
          description: L1 cache
          physical id: 10
          slot: Unknown
          size: 32KiB
          capacity: 32KiB
          capabilities: asynchronous internal write-back instruction
     *-memory
          description: System Memory
          physical id: 12
          slot: System board or motherboard
          size: 2GiB
          capacity: 2GiB
        *-bank:0
             description: SODIMM DDR2 Synchronous 533 MHz (1.9 ns)
             product: 0x393930353239352D3031392E4148304C4600
             vendor: Kingston
             physical id: 0
             serial: 0x690A0D82
             slot: DIMM0
             size: 1GiB
             width: 64 bits
             clock: 533MHz (1.9ns)
        *-bank:1
             description: SODIMM DDR2 Synchronous 533 MHz (1.9 ns)
             product: 0x393930353239352D3031392E4148304C4600
             vendor: Kingston
             physical id: 1
             serial: 0x690AE381
             slot: DIMM2
             size: 1GiB
             width: 64 bits
             clock: 533MHz (1.9ns)
     *-pci
          description: Host bridge
          product: Mobile PM965/GM965/GL960 Memory Controller Hub
          vendor: Intel Corporation
          physical id: 100
          bus info: pci@0000:00:00.0
          version: 03
          width: 32 bits
          clock: 33MHz
          configuration: driver=agpgart-intel
          resources: irq:0
        *-display:0
             description: VGA compatible controller
             product: Mobile GM965/GL960 Integrated Graphics Controller (primary)
             vendor: Intel Corporation
             physical id: 2
             bus info: pci@0000:00:02.0
             version: 03
             width: 64 bits
             clock: 33MHz
             capabilities: msi pm vga_controller bus_master cap_list rom
             configuration: driver=i915 latency=0
             resources: irq:42 memory:91000000-910fffff memory:80000000-8fffffff ioport:30d0(size=8)
        *-display:1 UNCLAIMED
             description: Display controller
             product: Mobile GM965/GL960 Integrated Graphics Controller (secondary)
             vendor: Intel Corporation
             physical id: 2.1
             bus info: pci@0000:00:02.1
             version: 03
             width: 64 bits
             clock: 33MHz
             capabilities: pm bus_master cap_list
             configuration: latency=0
             resources: memory:91100000-911fffff
        *-multimedia
             description: Audio device
             product: 82801H (ICH8 Family) HD Audio Controller
             vendor: Intel Corporation
             physical id: 1b
             bus info: pci@0000:00:1b.0
             version: 03
             width: 64 bits
             clock: 33MHz
             capabilities: pm msi pciexpress bus_master cap_list
             configuration: driver=snd_hda_intel latency=0
             resources: irq:43 memory:92400000-92403fff
        *-pci:0
             description: PCI bridge
             product: 82801H (ICH8 Family) PCI Express Port 1
             vendor: Intel Corporation
             physical id: 1c
             bus info: pci@0000:00:1c.0
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pci pciexpress msi pm normal_decode bus_master cap_list
             configuration: driver=pcieport
             resources: irq:40 ioport:2000(size=4096) memory:91300000-923fffff ioport:90000000(size=16777216)
           *-network
                description: Network controller
                product: BCM4311 802.11b/g WLAN
                vendor: Broadcom Corporation
                physical id: 0
                bus info: pci@0000:01:00.0
                version: 02
                width: 64 bits
                clock: 33MHz
                capabilities: pm msi pciexpress bus_master cap_list
                configuration: driver=b43-pci-bridge latency=0
                resources: irq:16 memory:91300000-91303fff
        *-usb:0
             description: USB controller
             product: 82801H (ICH8 Family) USB UHCI Controller #1
             vendor: Intel Corporation
             physical id: 1d
             bus info: pci@0000:00:1d.0
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: uhci bus_master
             configuration: driver=uhci_hcd latency=0
             resources: irq:21 ioport:3080(size=32)
        *-usb:1
             description: USB controller
             product: 82801H (ICH8 Family) USB UHCI Controller #2
             vendor: Intel Corporation
             physical id: 1d.1
             bus info: pci@0000:00:1d.1
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: uhci bus_master
             configuration: driver=uhci_hcd latency=0
             resources: irq:20 ioport:3060(size=32)
        *-usb:2
             description: USB controller
             product: 82801H (ICH8 Family) USB UHCI Controller #3
             vendor: Intel Corporation
             physical id: 1d.2
             bus info: pci@0000:00:1d.2
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: uhci bus_master
             configuration: driver=uhci_hcd latency=0
             resources: irq:19 ioport:3040(size=32)
        *-usb:3
             description: USB controller
             product: 82801H (ICH8 Family) USB2 EHCI Controller #1
             vendor: Intel Corporation
             physical id: 1d.7
             bus info: pci@0000:00:1d.7
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: pm debug ehci bus_master cap_list
             configuration: driver=ehci-pci latency=0
             resources: irq:23 memory:92404800-92404bff
        *-pci:1
             description: PCI bridge
             product: 82801 Mobile PCI Bridge
             vendor: Intel Corporation
             physical id: 1e
             bus info: pci@0000:00:1e.0
             version: f3
             width: 32 bits
             clock: 33MHz
             capabilities: pci subtractive_decode bus_master cap_list
             resources: ioport:1000(size=4096) memory:91200000-912fffff
           *-network
                description: Ethernet interface
                product: RTL-8100/8101L/8139 PCI Fast Ethernet Adapter
                vendor: Realtek Semiconductor Co., Ltd.
                physical id: 1
                bus info: pci@0000:02:01.0
                logical name: eth0
                version: 10
                serial: 00:1b:38:ba:b1:1a
                size: 100Mbit/s
                capacity: 100Mbit/s
                width: 32 bits
                clock: 33MHz
                capabilities: pm bus_master cap_list ethernet physical tp mii 10bt 10bt-fd 100bt 100bt-fd autonegotiation
                configuration: autonegotiation=on broadcast=yes driver=8139too driverversion=0.9.28 duplex=full ip=192.168.6.2 latency=64 link=yes maxlatency=64 mingnt=32 multicast=yes port=MII speed=100Mbit/s
                resources: irq:16 ioport:1000(size=256) memory:91200000-912000ff
        *-isa
             description: ISA bridge
             product: 82801HM (ICH8M) LPC Interface Controller
             vendor: Intel Corporation
             physical id: 1f
             bus info: pci@0000:00:1f.0
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: isa bus_master cap_list
             configuration: driver=lpc_ich latency=0
             resources: irq:0
        *-ide
             description: IDE interface
             product: 82801HM/HEM (ICH8M/ICH8M-E) IDE Controller
             vendor: Intel Corporation
             physical id: 1f.1
             bus info: pci@0000:00:1f.1
             version: 03
             width: 32 bits
             clock: 33MHz
             capabilities: ide bus_master
             configuration: driver=ata_piix latency=0
             resources: irq:19 ioport:1f0(size=8) ioport:3f6 ioport:170(size=8) ioport:376 ioport:30a0(size=16)
        *-storage
             description: SATA controller
             product: 82801HM/HEM (ICH8M/ICH8M-E) SATA Controller [AHCI mode]
             vendor: Intel Corporation
             physical id: 1f.2
             bus info: pci@0000:00:1f.2
             version: 03
             width: 32 bits
             clock: 66MHz
             capabilities: storage msi pm ahci_1.0 bus_master cap_list
             configuration: driver=ahci latency=0
             resources: irq:41 ioport:30b8(size=8) ioport:30dc(size=4) ioport:30b0(size=8) ioport:30d8(size=4) ioport:3020(size=32) memory:92404000-924047ff
        *-serial UNCLAIMED
             description: SMBus
             product: 82801H (ICH8 Family) SMBus Controller
             vendor: Intel Corporation
             physical id: 1f.3
             bus info: pci@0000:00:1f.3
             version: 03
             width: 32 bits
             clock: 33MHz
             configuration: latency=0
             resources: memory:92404c00-92404cff ioport:3000(size=32)
     *-scsi:0
          physical id: 1
          logical name: scsi0
          capabilities: emulated
        *-cdrom
             description: DVD reader
             product: CDRWDVD CRX890A
             vendor: Optiarc
             physical id: 0.0.0
             bus info: scsi@0:0.0.0
             logical name: /dev/cdrom
             logical name: /dev/cdrw
             logical name: /dev/dvd
             logical name: /dev/sr0
             version: P802
             capabilities: removable audio cd-r cd-rw dvd
             configuration: ansiversion=5 status=nodisc
     *-scsi:1
          physical id: 2
          logical name: scsi2
          capabilities: emulated
        *-disk
             description: ATA Disk
             product: Hitachi HTS54161
             vendor: Hitachi
             physical id: 0.0.0
             bus info: scsi@2:0.0.0
             logical name: /dev/sda
             version: C7KP
             serial: SB348DHRJR71GH
             size: 149GiB (160GB)
             capabilities: partitioned partitioned:dos
             configuration: ansiversion=5 sectorsize=512 signature=0004d306
           *-volume:0
                description: Linux filesystem partition
                physical id: 1
                bus info: scsi@2:0.0.0,1
                logical name: /dev/sda1
                logical name: /
                logical name: /home
                logical name: /var/lib/docker/btrfs
                capacity: 53GiB
                capabilities: primary bootable
                configuration: mount.fstype=btrfs mount.options=rw,relatime,space_cache state=mounted
           *-volume:1
                description: Extended partition
                physical id: 2
                bus info: scsi@2:0.0.0,2
                logical name: /dev/sda2
                size: 95GiB
                capacity: 95GiB
                capabilities: primary extended partitioned partitioned:extended
              *-logicalvolume:0
                   description: Linux swap / Solaris partition
                   physical id: 5
                   logical name: /dev/sda5
                   capacity: 2037MiB
                   capabilities: nofs
              *-logicalvolume:1
                   description: Linux filesystem partition
                   physical id: 6
                   logical name: /dev/sda6
                   logical name: /srv
                   capacity: 93GiB
                   configuration: mount.fstype=btrfs mount.options=rw,relatime,space_cache state=mounted
     *-scsi:2
          physical id: 3
          bus info: usb@1:5
          logical name: scsi5
          capabilities: emulated scsi-host
          configuration: driver=usb-storage
        *-disk
             description: SCSI Disk
             physical id: 0.0.0
             bus info: scsi@5:0.0.0
             logical name: /dev/sdb
             configuration: sectorsize=512
  *-network DISABLED
       description: Wireless interface
       physical id: 1
       logical name: wlan0
       serial: 00:1a:73:de:5f:d5
       capabilities: ethernet physical wireless
       configuration: broadcast=yes driver=b43 driverversion=3.16.0-25-generic firmware=666.2 link=no multicast=yes wireless=IEEE 802.11bg		

```

### 5.1. only show a certain class of hardware

```
$ sudo lshw -C network
[sudo] password for neo: 
  *-network               
       description: Network controller
       product: BCM4311 802.11b/g WLAN
       vendor: Broadcom Corporation
       physical id: 0
       bus info: pci@0000:01:00.0
       version: 02
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress bus_master cap_list
       configuration: driver=b43-pci-bridge latency=0
       resources: irq:16 memory:91300000-91303fff
  *-network
       description: Ethernet interface
       product: RTL-8100/8101L/8139 PCI Fast Ethernet Adapter
       vendor: Realtek Semiconductor Co., Ltd.
       physical id: 1
       bus info: pci@0000:02:01.0
       logical name: eth0
       version: 10
       serial: 00:1b:38:ba:b1:1a
       size: 100Mbit/s
       capacity: 100Mbit/s
       width: 32 bits
       clock: 33MHz
       capabilities: pm bus_master cap_list ethernet physical tp mii 10bt 10bt-fd 100bt 100bt-fd autonegotiation
       configuration: autonegotiation=on broadcast=yes driver=8139too driverversion=0.9.28 duplex=full ip=172.30.5.73 latency=64 link=yes maxlatency=64 mingnt=32 multicast=yes port=MII speed=100Mbit/s
       resources: irq:16 ioport:1000(size=256) memory:91200000-912000ff
  *-network:0 DISABLED
       description: Ethernet interface
       physical id: 1
       logical name: virbr0-nic
       serial: 52:54:00:5c:25:d6
       size: 10Mbit/s
       capabilities: ethernet physical
       configuration: autonegotiation=off broadcast=yes driver=tun driverversion=1.6 duplex=full link=no multicast=yes port=twisted pair speed=10Mbit/s
  *-network:1 DISABLED
       description: Wireless interface
       physical id: 2
       logical name: wlan0
       serial: 00:1a:73:de:5f:d5
       capabilities: ethernet physical wireless
       configuration: broadcast=yes driver=b43 driverversion=3.19.0-26-generic firmware=666.2 link=no multicast=yes wireless=IEEE 802.11bg
  *-network:2 DISABLED
       description: Wireless interface
       physical id: 3
       bus info: usb@1:3
       logical name: wlan1
       serial: 5c:63:bf:27:3f:b6
       capabilities: ethernet physical wireless
       configuration: broadcast=yes driver=ath9k_htc driverversion=3.19.0-26-generic firmware=1.3 link=no multicast=yes wireless=IEEE 802.11bgn

```

## 6. hwinfo - Hardware Information

## 7. dmidecode - DMI table decoder

**dmidecode**

```
# dmidecode |more

# dmidecode 2.2
SMBIOS 2.4 present.
62 structures occupying 3161 bytes.
Table at 0xCFFBC000.
Handle 0xDA00
        DMI type 218, 11 bytes.
        OEM-specific Type
                Header And Data:
                        DA 0B 00 DA B2 00 17 00 0E 20 00
Handle 0x0000
        DMI type 0, 24 bytes.
        BIOS Information
                Vendor: Dell Inc.
                Version: 1.2.0
                Release Date: 10/18/2006
                Address: 0xF0000
                Runtime Size: 64 kB
                ROM Size: 1024 kB
                Characteristics:
                        ISA is supported
                        PCI is supported
                        PNP is supported
                        BIOS is upgradeable
                        BIOS shadowing is allowed
                        ESCD support is available
                        Boot from CD is supported
                        Selectable boot is supported
                        EDD is supported
                        Japanese floppy for Toshiba 1.2 MB is supported (int 13h)
                        5.25"/360 KB floppy services are supported (int 13h)
                        5.25"/1.2 MB floppy services are supported (int 13h)
                        3.5"/720 KB floppy services are supported (int 13h)
                        Print screen service is supported (int 5h)
                        8042 keyboard services are supported (int 9h)
                        Serial services are supported (int 14h)
                        Printer services are supported (int 17h)
                        CGA/mono video services are supported (int 10h)
                        ACPI is supported
                        USB legacy is supported
                        BIOS boot specification is supported
                        Function key-initiated network boot is supported

```

## 8. ethtool - Display or change ethernet card settings

```
# ethtool eth0
Settings for eth0:
	Supported ports: [ TP ]
	Supported link modes:   10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	                        1000baseT/Full
	Supports auto-negotiation: Yes
	Advertised link modes:  10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	                        1000baseT/Full
	Advertised auto-negotiation: Yes
	Speed: 1000Mb/s
	Duplex: Full
	Port: Twisted Pair
	PHYAD: 2
	Transceiver: internal
	Auto-negotiation: on
	Supports Wake-on: pumbg
	Wake-on: g
	Current message level: 0x00000001 (1)
	Link detected: yes

```

鉴别 eth(x)

```
 简单的方法：
一个插网线，一个不插，运行 mii-tool 或 ethtool eth0，看状态是否连接

另一种方法是：
tail -f /var/log/messages，当你向其中一个网口做插拔网线的动作时，屏幕上会看到提示信息

```

最好的方法是将 mac 地址写在启动脚本内.

## 9. usb device

**lsusb**

```
neo@netkiller:~$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 005 Device 002: ID 0dda:0301 Integrated Circuit Solution, Inc. MP3 Player
Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

```

```
$ lsusb -tv
/:  Bus 05.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 04.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 03.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=ehci_hcd/8p, 480M

```

```

$ sudo lsusb -v

Bus 005 Device 001: ID 0000:0000
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         1 Single TT
  bMaxPacketSize0        64
  idVendor           0x0000
  idProduct          0x0000
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.24-22-generic ehci_hcd
  iProduct                2 EHCI Host Controller
  iSerial                 1 0000:00:1d.7
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           25
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0 Unused
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0004  1x 4 bytes
        bInterval              12
Hub Descriptor:
  bLength              11
  bDescriptorType      41
  nNbrPorts             8
  wHubCharacteristic 0x000a
    No power switching (usb 1.0)
    Per-port overcurrent protection
    TT think time 8 FS bits
  bPwrOn2PwrGood       10 * 2 milli seconds
  bHubContrCurrent      0 milli Ampere
  DeviceRemovable    0x00 0x00
  PortPwrCtrlMask    0xff 0xff
 Hub Port Status:
   Port 1: 0000.0100 power
   Port 2: 0000.0100 power
   Port 3: 0000.0100 power
   Port 4: 0000.0100 power
   Port 5: 0000.0100 power
   Port 6: 0000.0100 power
   Port 7: 0000.0100 power
   Port 8: 0000.0100 power
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled

Bus 004 Device 001: ID 0000:0000
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  idVendor           0x0000
  idProduct          0x0000
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.24-22-generic uhci_hcd
  iProduct                2 UHCI Host Controller
  iSerial                 1 0000:00:1d.3
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           25
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0 Unused
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0002  1x 2 bytes
        bInterval             255
Hub Descriptor:
  bLength               9
  bDescriptorType      41
  nNbrPorts             2
  wHubCharacteristic 0x000a
    No power switching (usb 1.0)
    Per-port overcurrent protection
  bPwrOn2PwrGood        1 * 2 milli seconds
  bHubContrCurrent      0 milli Ampere
  DeviceRemovable    0x00
  PortPwrCtrlMask    0xff
 Hub Port Status:
   Port 1: 0000.0100 power
   Port 2: 0000.0100 power
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled

Bus 003 Device 001: ID 0000:0000
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  idVendor           0x0000
  idProduct          0x0000
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.24-22-generic uhci_hcd
  iProduct                2 UHCI Host Controller
  iSerial                 1 0000:00:1d.2
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           25
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0 Unused
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0002  1x 2 bytes
        bInterval             255
Hub Descriptor:
  bLength               9
  bDescriptorType      41
  nNbrPorts             2
  wHubCharacteristic 0x000a
    No power switching (usb 1.0)
    Per-port overcurrent protection
  bPwrOn2PwrGood        1 * 2 milli seconds
  bHubContrCurrent      0 milli Ampere
  DeviceRemovable    0x00
  PortPwrCtrlMask    0xff
 Hub Port Status:
   Port 1: 0000.0100 power
   Port 2: 0000.0100 power
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled

Bus 002 Device 001: ID 0000:0000
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  idVendor           0x0000
  idProduct          0x0000
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.24-22-generic uhci_hcd
  iProduct                2 UHCI Host Controller
  iSerial                 1 0000:00:1d.1
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           25
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0 Unused
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0002  1x 2 bytes
        bInterval             255
Hub Descriptor:
  bLength               9
  bDescriptorType      41
  nNbrPorts             2
  wHubCharacteristic 0x000a
    No power switching (usb 1.0)
    Per-port overcurrent protection
  bPwrOn2PwrGood        1 * 2 milli seconds
  bHubContrCurrent      0 milli Ampere
  DeviceRemovable    0x00
  PortPwrCtrlMask    0xff
 Hub Port Status:
   Port 1: 0000.0100 power
   Port 2: 0000.0100 power
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled

Bus 001 Device 001: ID 0000:0000
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass            9 Hub
  bDeviceSubClass         0 Unused
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  idVendor           0x0000
  idProduct          0x0000
  bcdDevice            2.06
  iManufacturer           3 Linux 2.6.24-22-generic uhci_hcd
  iProduct                2 UHCI Host Controller
  iSerial                 1 0000:00:1d.0
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength           25
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower                0mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0 Unused
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0002  1x 2 bytes
        bInterval             255
Hub Descriptor:
  bLength               9
  bDescriptorType      41
  nNbrPorts             2
  wHubCharacteristic 0x000a
    No power switching (usb 1.0)
    Per-port overcurrent protection
  bPwrOn2PwrGood        1 * 2 milli seconds
  bHubContrCurrent      0 milli Ampere
  DeviceRemovable    0x00
  PortPwrCtrlMask    0xff
 Hub Port Status:
   Port 1: 0000.0100 power
   Port 2: 0000.0100 power
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled

```

## 10. lsscsi - list SCSI devices (or hosts) and their attributes

```
# yum install lsscsi 

```

lsscsi

```
# lsscsi 
[1:0:0:0]    disk    ATA      WDC WD10EZEX-00R 80.0  /dev/sda 
[2:0:0:0]    disk    ATA      WDC WD10EZEX-00R 80.0  /dev/sdb 
[5:0:0:0]    cd/dvd  PIONEER  DVD-ROM DVD-231  1.02  /dev/sr0

# lsscsi -L
[1:0:0:0]    disk    ATA      WDC WD10EZEX-00R 80.0  /dev/sda 
  device_blocked=0
  iocounterbits=32
  iodone_cnt=0x4cdcf62
  ioerr_cnt=0x3
  iorequest_cnt=0x4d27c32
  queue_depth=31
  queue_type=simple
  scsi_level=6
  state=running
  timeout=30
  type=0
[2:0:0:0]    disk    ATA      WDC WD10EZEX-00R 80.0  /dev/sdb 
  device_blocked=0
  iocounterbits=32
  iodone_cnt=0x4ac8a9c
  ioerr_cnt=0x3
  iorequest_cnt=0x4b11222
  queue_depth=31
  queue_type=simple
  scsi_level=6
  state=running
  timeout=30
  type=0
[5:0:0:0]    cd/dvd  PIONEER  DVD-ROM DVD-231  1.02  /dev/sr0 
  device_blocked=0
  iocounterbits=32
  iodone_cnt=0x1a
  ioerr_cnt=0x0
  iorequest_cnt=0x44
  queue_depth=1
  queue_type=none
  scsi_level=6
  state=running
  timeout=30
  type=5		

```

```
# cat /proc/scsi/scsi
Attached devices:
Host: scsi0 Channel: 02 Id: 00 Lun: 00
  Vendor: DELL     Model: PERC H700        Rev: 2.10
  Type:   Direct-Access                    ANSI SCSI revision: 05
Host: scsi0 Channel: 02 Id: 01 Lun: 00
  Vendor: DELL     Model: PERC H700        Rev: 2.10
  Type:   Direct-Access                    ANSI SCSI revision: 05

```

## 11. HBA

```
# dmesg | grep QLogic
QLogic Fibre Channel HBA Driver: 8.03.01.05.06.0-k8
 QLogic Fibre Channel HBA Driver: 8.03.01.05.06.0-k8
  QLogic QLE2562 - PCI-Express Dual Channel 8Gb Fibre Channel HBA
 QLogic Fibre Channel HBA Driver: 8.03.01.05.06.0-k8
  QLogic QLE2562 - PCI-Express Dual Channel 8Gb Fibre Channel HBA

# dmesg | grep qla
qla2xxx 0000:04:00.0: PCI INT A -> GSI 38 (level, low) -> IRQ 38
qla2xxx 0000:04:00.0: Found an ISP2532, irq 38, iobase 0xffffc90016e76000
qla2xxx 0000:04:00.0: irq 61 for MSI/MSI-X
qla2xxx 0000:04:00.0: irq 62 for MSI/MSI-X
qla2xxx 0000:04:00.0: Configuring PCI space...
qla2xxx 0000:04:00.0: setting latency timer to 64
qla2xxx 0000:04:00.0: Configure NVRAM parameters...
qla2xxx 0000:04:00.0: Verifying loaded RISC code...
qla2xxx 0000:04:00.0: firmware: requesting ql2500_fw.bin
qla2xxx 0000:04:00.0: FW: Loading via request-firmware...
qla2xxx 0000:04:00.0: Allocated (64 KB) for FCE...
qla2xxx 0000:04:00.0: Allocated (64 KB) for EFT...
qla2xxx 0000:04:00.0: Allocated (1350 KB) for firmware dump...
qla2xxx 0000:04:00.0: Unable to read FCP priority data.
scsi0 : qla2xxx
qla2xxx 0000:04:00.0:
qla2xxx 0000:04:00.1: PCI INT B -> GSI 45 (level, low) -> IRQ 45
qla2xxx 0000:04:00.1: Found an ISP2532, irq 45, iobase 0xffffc90016e06000
qla2xxx 0000:04:00.1: irq 63 for MSI/MSI-X
qla2xxx 0000:04:00.1: irq 64 for MSI/MSI-X
qla2xxx 0000:04:00.1: Configuring PCI space...
qla2xxx 0000:04:00.1: setting latency timer to 64
qla2xxx 0000:04:00.1: Configure NVRAM parameters...
qla2xxx 0000:04:00.1: Verifying loaded RISC code...
qla2xxx 0000:04:00.1: FW: Loading via request-firmware...
qla2xxx 0000:04:00.1: Allocated (64 KB) for FCE...
qla2xxx 0000:04:00.1: Allocated (64 KB) for EFT...
qla2xxx 0000:04:00.1: Allocated (1350 KB) for firmware dump...
qla2xxx 0000:04:00.1: Unable to read FCP priority data.
scsi1 : qla2xxx
qla2xxx 0000:04:00.1:
qla2xxx 0000:04:00.0: LIP reset occurred (f700).
qla2xxx 0000:04:00.1: LIP reset occurred (f700).
qla2xxx 0000:04:00.0: LOOP UP detected (8 Gbps).
qla2xxx 0000:04:00.1: LOOP UP detected (8 Gbps).

```

## 12. lsblk - list block devices

```
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 149.1G  0 disk 
├─sda1   8:1    0    54G  0 part /home
├─sda2   8:2    0     1K  0 part 
├─sda5   8:5    0     2G  0 part [SWAP]
└─sda6   8:6    0  93.1G  0 part /srv
sr0     11:0    1  1024M  0 rom  		

```

```
# lsblk 
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sdb           8:16   0 931.5G  0 disk  
?..md126       9:126  0 931.5G  0 raid1 
  ?..md126p1 259:0    0   500M  0 md    /boot
  ?..md126p2 259:1    0  48.8G  0 md    /
  ?..md126p3 259:2    0  14.7G  0 md    [SWAP]
  ?..md126p4 259:3    0     1K  0 md    
  ?..md126p5 259:4    0 390.6G  0 md    /www
  ?..md126p6 259:5    0 476.9G  0 md    /opt
sda           8:0    0 931.5G  0 disk  
?..md126       9:126  0 931.5G  0 raid1 
  ?..md126p1 259:0    0   500M  0 md    /boot
  ?..md126p2 259:1    0  48.8G  0 md    /
  ?..md126p3 259:2    0  14.7G  0 md    [SWAP]
  ?..md126p4 259:3    0     1K  0 md    
  ?..md126p5 259:4    0 390.6G  0 md    /www
  ?..md126p6 259:5    0 476.9G  0 md    /opt
sr0          11:0    1  1024M  0 rom   

```

## 13. kudzu - detects and configures new and/or changed hardware on a system

```
# kudzu -p | more

```

network

```
# kudzu --class=network -p

```

## 14. numactl - Control NUMA policy for processes or shared memory

```

neo@ubuntu:~$ sudo apt install numactl

neo@ubuntu:~$ numactl --hardware
available: 1 nodes (0)
node 0 cpus: 0 1 2 3 4 5 6 7
node 0 size: 16039 MB
node 0 free: 10675 MB
node distances:
node   0 
  0:  10 

```

## 15. udev - Linux dynamic device management

## 第 8 章 区域/语言/时间

## 1. Ubuntu

### 1.1. time zone

选择用户时区

```

$ tzselect
Please identify a location so that time zone rules can be set correctly.
Please select a continent or ocean.
 1) Africa
 2) Americas
 3) Antarctica
 4) Arctic Ocean
 5) Asia
 6) Atlantic Ocean
 7) Australia
 8) Europe
 9) Indian Ocean
10) Pacific Ocean
11) none - I want to specify the time zone using the Posix TZ format.
#?

```

**tzconfig**

```

netkiller@shenzhen:~$ tzconfig
Your current time zone is set to US/Eastern
Do you want to change that? [n]: y

Please enter the number of the geographic area in which you live:

        1) Africa                       7) Australia

        2) America                      8) Europe

        3) US time zones                9) Indian Ocean

        4) Canada time zones            10) Pacific Ocean

        5) Asia                         11) Use System V style time zones

        6) Atlantic Ocean               12) None of the above

Then you will be shown a list of cities which represent the time zone
in which they are located. You should choose a city in your time zone.

Number: 5

Aden Almaty Amman Anadyr Aqtau Aqtobe Ashgabat Ashkhabad Baghdad Bahrain
Baku Bangkok Beirut Bishkek Brunei Calcutta Choibalsan Chongqing Chungking
Colombo Dacca Damascus Dhaka Dili Dubai Dushanbe Gaza Harbin Hong_Kong
Hovd Irkutsk Istanbul Jakarta Jayapura Jerusalem Kabul Kamchatka Karachi
Kashgar Katmandu Krasnoyarsk Kuala_Lumpur Kuching Kuwait Macao Macau
Magadan Makassar Manila Muscat Nicosia Novosibirsk Omsk Oral Phnom_Penh
Pontianak Pyongyang Qatar Qyzylorda Rangoon Riyadh Riyadh87 Riyadh88
Riyadh89 Saigon Sakhalin Samarkand Seoul Shanghai Singapore Taipei
Tashkent Tbilisi Tehran Tel_Aviv Thimbu Thimphu Tokyo Ujung_Pandang
Ulaanbaatar Ulan_Bator Urumqi Vientiane Vladivostok Yakutsk Yekaterinburg
Yerevan

Please enter the name of one of these cities or zones
You just need to type enough letters to resolve ambiguities
Press Enter to view all of them again
Name: [] Harbin
Your default time zone is set to 'Asia/Harbin'.
Local time is now:      Tue Mar 11 10:46:46 CST 2008.
Universal Time is now:  Tue Mar 11 02:46:46 UTC 2008.

```

tzdata

**dpkg-reconfigure tzdata**

```

$ sudo dpkg-reconfigure tzdata

```

### 1.2. to change system date/time

date

e.g. date -s month/day/year

```

# date -s 1/18/2008

```

time

e.g. date -s hour:minute:second

```

# date -s 11:12:00

```

writing CMOS

```

# clock -w

```

#### 1.2.1. NTP Server

更新网络时间

ntpdate - client for setting system time from NTP servers

```

$ sudo ntpdate asia.pool.ntp.org
21 May 10:34:18 ntpdate[6687]: adjust time server 203.185.69.60 offset 0.031079 sec		
$ sudo hwclock -w

```

### 1.3. Language

默认语言

```

export LANG=en_US
export LC_ALL=en_US		

```

永久更改

```

sudo vi /etc/default/locale

LANG="en_US.UTF-8"
LANGUAGE="en_US:en"

```

改为中文环境

```

sudo apt-get install language-support-zh
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh"

```

## 2. CentOS 区域设置

### 2.1. 时区设置 CentOS 6

timeconfig

system-config-date

#### 2.1.1. 查看当前时区 /etc/sysconfig/clock

```

[root@ntp ~]# cat /etc/sysconfig/clock
ZONE="Asia/Harbin"
UTC=true
ARC=false

```

#### 2.1.2. tzselect - select a timezone

```

# tzselect
Please identify a location so that time zone rules can be set correctly.
Please select a continent or ocean.
 1) Africa
 2) Americas
 3) Antarctica
 4) Arctic Ocean
 5) Asia
 6) Atlantic Ocean
 7) Australia
 8) Europe
 9) Indian Ocean
10) Pacific Ocean
11) none - I want to specify the time zone using the Posix TZ format.
#?

```

重新启动后生效

#### 2.1.3. 修改时区并立即生效

可用时区 /usr/share/zoneinfo

```

[root@ntp ~]# ls /usr/share/zoneinfo
Africa      Asia       Canada   Cuba   EST      Factory  GMT0       Hongkong  Iran         Japan      Mexico   Navajo   Poland      PRC      ROK        Universal  W-SU
America     Atlantic   CET      EET    EST5EDT  GB       GMT-0      HST       iso3166.tab  Kwajalein  Mideast  NZ       Portugal    PST8PDT  Singapore  US         zone.tab
Antarctica  Australia  Chile    Egypt  Etc      GB-Eire  GMT+0      Iceland   Israel       Libya      MST      NZ-CHAT  posix       right    Turkey     UTC        Zulu
Arctic      Brazil     CST6CDT  Eire   Europe   GMT      Greenwich  Indian    Jamaica      MET        MST7MDT  Pacific  posixrules  ROC      UCT        WET

```

执行 hwclock 后会立即生效

```

# cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# hwclock

```

演示如下，你可以看到时区从 EDT 变为 CST

```

# cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# date
Fri Jul  4 05:57:25 EDT 2014

# hwclock
Fri 04 Jul 2014 06:12:14 PM CST  -0.219192 seconds

# date
Fri Jul  4 18:12:17 CST 2014

```

### 2.2. 时区设置 CentOS 7

列出时区

```

# timedatectl list-timezones

```

设置时区

```

# timedatectl set-timezone Asia/Hong_Kong

```

查看时区

```

# timedatectl

```

### 2.3. 日期、时间

```

# date -s "2008-7-19"
# date -s 18:10

```

#### 2.3.1. rdate - get the time via the network

```

# rdate time-a.nist.gov 查看
# rdate -s time-a.nist.gov 设置

```

### 2.4. 语言

**system-config-language**

```

vim /etc/sysconfig/i18n
LANG="en_US.UTF-8"

```

查看语言

```

locale -a

```

## 第 9 章 console / terminal 控制台与终端

## 1. serial console

gurb

```
$ sudo vim /boot/grub/menu.lst

title           Ubuntu 8.04.1, kernel 2.6.24-21-generic
root            (hd0,5)
kernel          /boot/vmlinuz-2.6.24-21-generic root=UUID=3d5dd6c0-bbd2-4ddf-9b71-1c7b78e8de3b ro quiet splash 

console=tty0 console=ttyS0,38400
initrd          /boot/initrd.img-2.6.24-21-generic
quiet	

```

tty6

```
$ sudo vim /etc/event.d/tty6

respawn
#exec /sbin/getty 38400 tty6
exec /sbin/getty -L /dev/ttyS0 38400 vt100	

```

other terminal: VT100, VT220, VT320, VT420

securetty

```
$ cat /etc/securetty
# for people with serial port consoles
ttyS0

```

## 2. console timeout

查看当前的$TMOUT 环境变量设置

**echo $TMOUT**

TMOUT=3600

export TMOUT

**netkiller@Linux-server:~$ sudo dpkg-reconfigure en_US.UTF-8**

## 3. TUI (Text User Interface)

SVGATextMode

```
$ sudo apt-get install svgatextmode
$ SVGATextMode 80x25x9

```

## 4. framebuffer

在 grub.conf 中的 kernel 行后面写上 vga=0x317 就行了，也可以用 vga=ask，让系统启动的时候询问你用多大的分辨率

## 第 10 章 Harddisk 磁盘管理

主分区最多 4 个

逻辑分区:

*   SCSI 最多 16 个

*   IDE 最多 63 个

## 1. 查看分区分区 UUID

```

$ blkid
/dev/sda1: UUID="a457213b-e72d-4c9c-953d-b438ec554d3c" SEC_TYPE="ext2" TYPE="ext3"
/dev/sda5: UUID="cc2c1be9-a6e0-4494-a5f0-76b39d3fc1f0" TYPE="swap"
/dev/sda6: UUID="3c9a1484-1295-4fb9-9c94-f9c69ae7e770" TYPE="ext3"
/dev/sda7: UUID="ade7b5e7-a311-45de-9b24-e16be73de715" TYPE="swap"

$ ls -l /dev/disk/by-uuid
total 0
lrwxrwxrwx 1 root root 10 2009-07-11 00:52 3c9a1484-1295-4fb9-9c94-f9c69ae7e770 -> ../../sda6
lrwxrwxrwx 1 root root 10 2009-07-11 00:52 a457213b-e72d-4c9c-953d-b438ec554d3c -> ../../sda1
lrwxrwxrwx 1 root root 10 2009-07-11 00:52 ade7b5e7-a311-45de-9b24-e16be73de715 -> ../../sda7
lrwxrwxrwx 1 root root 10 2009-07-11 00:52 cc2c1be9-a6e0-4494-a5f0-76b39d3fc1f0 -> ../../sda5

```

## 2. 通过 UUID 或 标签 查询设备文件

### findfs - find a filesystem by label or UUID

```

[root@localhost ~]# findfs UUID=dcccfdaf-ad35-4610-b1a9-3b274e51de5b
/dev/sda2		

```

## 3. Label

### 3.1. Ext2

e2label - Change the label on an ext2/ext3 filesystem

#### 3.1.1. 查看卷标

```

# e2label /dev/sda1
/boot

```

#### 3.1.2. 更改卷标

```

# man e2label
# e2label /dev/sda5 /www

# e2label /dev/sda5
/www

```

测试

```

# mount /app

```

## 4. swap 交换分区

查看交换分区信息

```

$ swapon -s
Filename				Type		Size	Used	Priority
/dev/md127p3                            partition	15359992	1654332	-1

```

新增交换分区

```

dd if=/dev/zero of=/root/swap0 bs=1M count=2048
mkswap /root/swap0
swapon /root/swap0

```

例 10.1. 增加交换分区

```

# fallocate -l 4G swap0
# chmod 600 swap0
# mkswap swap0
# swapon /www/swap0

```

### 4.1. swapon failed: Invalid argument

```

[root@netkiller www]# swapon /www/swap0
swapon: /www/swap0: swapon failed: Invalid argument

```

注意：swap 不能建立在 btrfs 分区上

```

[root@netkiller www]# df -T /www
Filesystem     Type  1K-blocks     Used Available Use% Mounted on
/dev/vdb1      btrfs 209714176 26751256 181122984  13% /www

```

/www 分区使用 btrfs 所以当运行 swapon 时出现问题。

## 5. Show partition

show all of disk and partition

```

neo@master:~$ sudo sfdisk -s
/dev/sda:   8388608
/dev/sdb:   2097152
total: 10485760 blocks

```

or

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

Disk /dev/sdb doesn't contain a valid partition table
neo@master:~$

```

show partition /dev/sda

```

neo@master:~$ sudo fdisk -l /dev/sda

Disk /dev/sda: 8589 MB, 8589934592 bytes
255 heads, 63 sectors/track, 1044 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Disk identifier: 0x000301bd

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1         993     7976241   83  Linux
/dev/sda2             994        1044      409657+   5  Extended
/dev/sda5             994        1044      409626   82  Linux swap / Solaris
neo@master:~$

```

## 6. Create partition

```

$ sudo cfdisk /dev/sdb

```

```

Command (m for help): p

Disk /dev/sda: 146.1 GB, 146163105792 bytes
255 heads, 63 sectors/track, 17769 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          25      200781   83  Linux
/dev/sda2              26        3849    30716280   83  Linux
/dev/sda3            3850       17769   111812400   83  Linux

Command (m for help): d
Partition number (1-4): 3

Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 3
First cylinder (3850-17769, default 3850):
Using default value 3850
Last cylinder or +size or +sizeM or +sizeK (3850-17769, default 17769): +32000M

Command (m for help): p

Disk /dev/sda: 146.1 GB, 146163105792 bytes
255 heads, 63 sectors/track, 17769 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          25      200781   83  Linux
/dev/sda2              26        3849    30716280   83  Linux
/dev/sda3            3850        7740    31254457+  83  Linux

```

## 7. Clone partition

/dev/sda 克隆到 /dev/sdb

```

$ sudo dd if=/dev/sda of=/dev/sdb

```

备份 mbr 主引导记录

```

$ dd if=/dev/sda of=/root/disk.mbr bs=512 count=1

```

```

$ dd if=/root/disk.mbr of=/dev/sda bs=512 count=1

```

软盘镜像

```

$ dd if=/dev/fd0 of=floppy.img bs=1440k

```

## 8. estimate disk / directory / file space usage

total for a directory

```

du -h --max-depth=0

```

## 9. Convert from ext3 to ext4 File system

step 1

```

$ sudo tune2fs -O extents,uninit_bg,dir_index /dev/sda7
tune2fs 1.41.4 (27-Jan-2009)

Please run e2fsck on the filesystem.

```

step 2

```

$ sudo e2fsck -fD /dev/sda7
e2fsck 1.41.4 (27-Jan-2009)
/dev/sda7 is mounted.

WARNING!!!  Running e2fsck on a mounted filesystem may cause
SEVERE filesystem damage.

Do you really want to continue (y/n)? yes

/dev/sda7: recovering journal
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 3A: Optimizing directories
Pass 4: Checking reference counts
Pass 5: Checking group summary information
Block bitmap differences:  -3913734 +3925302
Fix<y>? yes

/dev/sda7: ***** FILE SYSTEM WAS MODIFIED *****
/dev/sda7: 77282/2293760 files (15.7% non-contiguous), 4584313/9163066 blocks

```

step 3

```

$ sudo cp /etc/fstab /etc/fstab.old
$ sudo vim /etc/fstab

# /dev/sda7
UUID=16089544-6fbf-400e-a63a-fa6159e271e5 /home           ext4    relatime,errors=remount-ro 0 1

```

step 4

```

$ sudo reboot

```

## 10. GPT

```

$ sudo parted /dev/sda
GNU Parted 2.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)

```

### 10.1. 设置 GTP 磁盘

```

(parted) mklabel gpt                                                      
Warning: The existing disk label on /dev/xvdb will be destroyed and all data on this disk will be lost. Do you want to continue?
Yes/No? Yes			

```

### 10.2. 查看分区

```

(parted) print
Model: DELL PERC 6/i (scsi)
Disk /dev/sda: 2498GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system     Name     Flags
 1      1049kB  50.0GB  50.0GB  ext4                     boot
 2      50.0GB  66.0GB  16.0GB  linux-swap(v1)
 3      66.0GB  2498GB  2432GB  ext4            /backup

```

空闲空间

```

(parted) print free
Model: DELL PERC 6/i (scsi)
Disk /dev/sda: 2498GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system     Name     Flags
        17.4kB  1049kB  1031kB  Free Space
 1      1049kB  50.0GB  50.0GB  ext4                     boot
 2      50.0GB  66.0GB  16.0GB  linux-swap(v1)
 3      66.0GB  2498GB  2432GB  ext4            /backup
        2498GB  2498GB  1032kB  Free Space

```

### 10.3. 创建分区

创建主分区

```

(parted) mkpart primary
File system type?  [ext2]?                                                
Start? 0GB                                                                
End? 280GB                                                                
(parted) p                                                                
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 784GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End    Size   File system  Name     Flags
 1      1049kB  280GB  280GB               primary			

```

创建扩展分区

```

(parted) mkpart extended                                                 
File system type?  [ext2]?                                                
Start? 280GB                                                              
End? 100%                                                                 
(parted) p                                                                
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 784GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End    Size   File system  Name      Flags
 1      1049kB  280GB  280GB               primary
 2      280GB   784GB  504GB               extended			

```

创建分区

```

(parted) mkpart
Partition name?  []? /www
File system type?  [ext2]?
Start? 10GB
End? 50GB

```

例 10.2. GPT Example

```

(parted) print devices
/dev/sdb (9999GB)
/dev/sda (2498GB)

(parted) select /dev/sdb
Using /dev/sdb

(parted) mklabel gpt
Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be
lost. Do you want to continue?
Yes/No? yes

(parted) mkpart
Partition name?  []? /md1200
File system type?  [ext2]? ext4
Start? 0GB
End? 9999GB

(parted) print list
Model: DELL PERC H800 (scsi)
Disk /dev/sdb: 9999GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system  Name     Flags
 1      1049kB  9999GB  9999GB               /md1200

Model: DELL PERC 6/i (scsi)
Disk /dev/sda: 2498GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system     Name     Flags
 1      1049kB  50.0GB  50.0GB  ext4                     boot
 2      50.0GB  66.0GB  16.0GB  linux-swap(v1)
 3      66.0GB  2498GB  2432GB  ext4            /backup

(parted)

```

例 10.3. 创建扩展分区

查看可用空间

```

(parted) print free
Model: HP LOGICAL VOLUME (scsi)
Disk /dev/sda: 1200GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type     File system     Flags
        32.3kB  1049kB  1016kB           Free Space
 1      1049kB  525MB   524MB   primary  ext4            boot
 2      525MB   105GB   105GB   primary  ext4
 3      105GB   139GB   33.6GB  primary  linux-swap(v1)
        139GB   1200GB  1061GB           Free Space

```

创建扩展分区

```

(parted) mkpart                                                           
Partition type?  primary/extended? extended                               
Start? 139GB                                                              
End? 1200GB                                                               
(parted) p                                                                
Model: HP LOGICAL VOLUME (scsi)
Disk /dev/sda: 1200GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type      File system     Flags
 1      1049kB  525MB   524MB   primary   ext4            boot
 2      525MB   105GB   105GB   primary   ext4
 3      105GB   139GB   33.6GB  primary   linux-swap(v1)
 4      139GB   1200GB  1061GB  extended                  lba				

```

创建逻辑卷

```

(parted) mkpart                                                           
Partition type?  [logical]? logical                                       
File system type?  [ext2]?                                                
Start? 139GB                                                              
End? 200GB                                                                
(parted) mkpart logical                                                   
File system type?  [ext2]?                                                
Start? 200GB                                                              
End? 1200GB                                                               
(parted) p                                                                
Model: HP LOGICAL VOLUME (scsi)
Disk /dev/sda: 1200GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type      File system     Flags
 1      1049kB  525MB   524MB   primary   ext4            boot
 2      525MB   105GB   105GB   primary   ext4
 3      105GB   139GB   33.6GB  primary   linux-swap(v1)
 4      139GB   1200GB  1061GB  extended                  lba
 5      139GB   200GB   61.1GB  logical
 6      200GB   1200GB  1000GB  logical

(parted) quit                                                             
Information: You may need to update /etc/fstab.

```

查看分区

```

# fdisk -l

Disk /dev/sda: 1199.9 GB, 1199865640960 bytes, 2343487580 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000c7511

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048   205826047   102400000   83  Linux
/dev/sda3       205826048   271362047    32768000   82  Linux swap / Solaris
/dev/sda4       271362048  2343487487  1036062720    f  W95 Ext'd (LBA)
/dev/sda5       271364096   390625279    59630592   83  Linux
/dev/sda6       390627328  2343487487   976430080   83  Linux				

```

### 10.4. 删除分区

使用 rm 删除分区

```

# parted /dev/xvdb
GNU Parted 2.1
Using /dev/xvdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 784GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos

Number  Start   End     Size    Type     File system  Flags
 1      32.3kB  4113MB  4113MB  primary  ext4
 2      4113MB  784GB   780GB   primary

(parted) rm
Partition number? 1

(parted) p
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 784GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos

Number  Start   End    Size   Type     File system  Flags
 2      4113MB  784GB  780GB  primary

(parted) rm 2
(parted) p
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 784GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos

Number  Start  End  Size  Type  File system  Flags

```

删除扩展分区将自动删除逻辑卷

### 10.5. 退出

```

(parted) quit

```

### 10.6. mount

```

neo@backup:~$ sudo blkid
[sudo] password for neo:
/dev/sda1: UUID="2fc411ec-9f6e-4e04-9270-11d23a9b0668" TYPE="ext4"
/dev/sda2: UUID="f5175b7a-4c87-471c-ab9f-9d601bc5e6e2" TYPE="swap"
/dev/sda3: UUID="3217bdd9-1beb-494a-a428-8d1c09eaa1af" TYPE="ext4"

neo@backup:~$ sudo vim /etc/fstab
UUID=3217bdd9-1beb-494a-a428-8d1c09eaa1af /backup ext4 errors=remount-ro 0       1

```

## 11. loop devices

If you are using the loadable module you must have the module loaded first with the command:

```

$ sudo modprobe loop

```

The following commands can be used as an example of using the loop device.

```

$ dd if=/dev/zero of=file bs=1k count=100
100+0 records in
100+0 records out
102400 bytes (102 kB) copied, 0.00126554 s, 80.9 MB/s

$ sudo losetup /dev/loop0 file

$ sudo mkfs.ext3 /dev/loop0
mke2fs 1.40.8 (13-Mar-2008)
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
16 inodes, 100 blocks
5 blocks (5.00%) reserved for the super user
First data block=1
1 block group
8192 blocks per group, 8192 fragments per group
16 inodes per group

Writing inode tables: done

Filesystem too small for a journal
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 24 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.

```

mount loop device

```

$ sudo mkdir /mnt/loop
$ sudo mount /dev/loop0 /mnt/loop

```

Now！ you can using it as harddisk.

umount loop device

```

$ sudo umount /mnt/loop/
$ sudo losetup -d /dev/loop0

```

Maybe also encryption modules are needed.

```

$ sudo modprobe cryptoloop
$ sudo modprobe des

```

enable data encryption

```

$ dd if=/dev/zero of=encryption_file bs=1k count=100
100+0 records in
100+0 records out
102400 bytes (102 kB) copied, 0.00130537 s, 78.4 MB/s

$ sudo losetup -e des /dev/loop0 encryption_file

```

If you are using the loadable module you may remove the module with the command

```

$ sudo rmmod loop des cryptoloop

```

### 11.1. losetup - set up and control loop devices

EXAMPLE

```

       If you are using the loadable module you must have the module loaded first with the command

              # insmod loop.o

       Maybe also encryption modules are needed.

              # insmod des.o # insmod cryptoloop.o

       The following commands can be used as an example of using the loop device.

              # dd if=/dev/zero of=/file bs=1k count=100
              # losetup -e des /dev/loop0 /file
              Password:
              Init (up to 16 hex digits):
              # mkfs -t ext2 /dev/loop0 100
              # mount -t ext2 /dev/loop0 /mnt
               ...
              # umount /dev/loop0
              # losetup -d /dev/loop0

       If you are using the loadable module you may remove the module with the command

              # rmmod loop

```

## 第 11 章 Removable Storage

eject - eject removable media

```

$ eject

```

## 1. usb flash

mount NTFS filesystem

```

sudo mount -t ntfs-3g /dev/sdb1 /mnt/usbflash/ -o force

```

## 2. CD / DVD

### 2.1. Mount an ISO file

To mount the ISO image file.iso to the mount point /media/cdrom use this :

```

$ mount -o loop -t iso9660 file.iso /media/cdrom

```

### 2.2. create iso file from CD

```

$ dd if=/dev/cdrom of=isofile.iso

```

### 2.3. burner

### 2.4. ISO Mirror

```

$ mkisofs -V LABEL -r /mnt/cdrom | gzip > cdrom.iso.gz

```

mount iso file

```

$ mount -t iso9660 -o loop cdrom.iso /mnt/cdrom

```

## 第 12 章 File System 文件系统

## 1. /etc/fstab

```

# <file system> <mount point>   <type>  <options>       <dump>  <pass>

```

mount point

```
该字段描述希望的文件系统加载的目录，对于 swap 设备，该字段为 none

```

file system

```
例如/dev/cdrom 或/dev/sdb,除了使用设备名，你可以使用设备的 UUID 或设备的卷标签，例如，LABAL=root 或 UUID=7f91104e-8187-4ccf-8215-6e2e641f32e3

```

type

定义了该设备上的文件系统,系统可用文件系统

```
$ cat /proc/filesystems
nodev   sysfs
nodev   rootfs
nodev   bdev
nodev   proc
nodev   cgroup
nodev   cpuset
nodev   tmpfs
nodev   devtmpfs
nodev   debugfs
nodev   securityfs
nodev   sockfs
nodev   pipefs
nodev   anon_inodefs
nodev   inotifyfs
nodev   devpts
        ext3
        ext2
        ext4
nodev   ramfs
nodev   hugetlbfs
nodev   ecryptfs
nodev   fuse
        fuseblk
nodev   fusectl
nodev   mqueue
nodev   rpc_pipefs
nodev   nfs
nodev   nfs4
        reiserfs
        xfs
        jfs
        msdos
        vfat
        ntfs
        minix
        hfs
        hfsplus
        qnx4
        ufs
        btrfs
        iso9660

```

options

```
选项　　　　　　　　　　　　　　含义
defaults  使用默认设置。	等于 rw,suid,dev,exec,auto,nouser,async，

rw   挂载为读写权限
ro　　　　以只读模式加载该文件系统

exec    是一个默认设置项，它使在那个分区中的可执行的二进制文件能够执行。
noexec	二进制文件不允许执行。

sync　　　不对该设备的写操作进行缓冲处理，这可以防止在非正常关机时情况下破坏文件系统，但是却降低了计算机速度
async  	所有的 I/O 将以异步方式进行

user　　　允许普通用户加载该文件系统
nouser  只允许 root 用户挂载。这是默认设置。

quota　　　强制在该文件系统上进行磁盘定额限制
noauto　　不再使用 mount -a 命令（例如系统启动时）加载该文件系统

noatime/nodiratime	禁止更新访问时间

```

dump

```
dump　-　该选项被"dump"命令使用来检查一个文件系统应该以多快频率进行转储，若不需要转储就设置该字段为 0

```

pass

```
该字段被 fsck 命令用来决定在启动时需要被扫描的文件系统的顺序，根文件系统"/"对应该字段的值应该为 1，其他文件系统应该为 2。若该文件系统无需在启动时扫描则设置该字段为 0

```

noatime/nodiratime

```
/dev/sda2 /data ext3 defaults 0 2
/dev/sda2 /data ext3 defaults,noatime,nodiratime 0 2

```

```
mount -o remount /data
mount -o noatime -o nodiratime -o remount /data

```

### 1.1. /etc/fstab 例子

/etc/fstab btrfs 实例

```

neo@netkiller:~$ cat /etc/fstab 
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=d103e33f-7f9f-4911-918e-32eae42e229c /               btrfs   defaults,subvol=@ 0       1
# /home was on /dev/sda1 during installation
UUID=d103e33f-7f9f-4911-918e-32eae42e229c /home           btrfs   defaults,subvol=@home 0       2
# /opt was on /dev/sda6 during installation
UUID=63d0b776-3bbd-490f-8284-f148b255185e /opt            btrfs   noatime,nodiratime,noexec 0       2
# swap was on /dev/sda5 during installation
UUID=ff8945bf-fa45-49e5-b3d2-bb833bc6dc9c none            swap    sw              0       0

```

## 2. Mount partition

### 2.1. Mount

```
sudo mount /dev/sdb1 /mnt/mount1

```

支持 UTF-8

```
mount -o iocharset=utf8 /dev/sda5 /mnt/usb

```

禁止文件与目录的访问时间

```
mount -o noatime,nodiratime /dev/drbd0  /mnt

```

### 2.2. Umount

umount - unmount file systems

```
sudo umount /mnt/mount1

```

### 2.3. bind directory

```
mount --bind /foo /home/neo/foo

```

挂载目录将不能被删除，但目录下文件可以删除

```
# rm -rf /home/neo/foo
rm: cannot remove directory '/home/neo/foo': Device or resource busy

```

/etc/fstab

```
/foo /home/neo/foo    none    bind    0 0

```

## 3. ext2

```
neo@netkiller:~# mkfs.ext2 /dev/sdb1		

```

ext2 是早期 Linux 使用的文件系统存在很多缺陷，建议不要在使用。

## 4. ext3

format /dev/sdb1

```
neo@netkiller:~$ sudo mkfs.ext3 /dev/sdb1

```

## 5. ReiserFS

you also can using other file system

reiserfs

```
neo@netkiller:~$ sudo mkfs.reiserfs /dev/sdb1

```

## 6. EXT4

### 6.1. install

```
# yum install e4fsprogs

```

### 6.2. format

```
# mkfs.ext4 /dev/sda2

```

### 6.3. label

```
# e4label /dev/sda2 /www

# mkdir /www

# cat /etc/fstab |grep ext4
LABEL=/www              /www                    ext4    defaults        1 2

```

### 6.4. mount/umount

```
# mount /www
# umount /www

```

### 6.5. LVM 卷

```
# mkfs.ext4 /dev/VolGroup00/LogVol02

[root@images ~]# cat /etc/fstab
/dev/VolGroup00/LogVol00 /                       ext3    defaults        1 1
/dev/VolGroup00/LogVol02 /images                 ext4    defaults        1 2
LABEL=/boot             /boot                   ext3    defaults        1 2
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0
/dev/VolGroup00/LogVol01 swap                    swap    defaults        0 0

# mount -a

```

## 7. LVM

请参考《Netkiller Storage 手札》·LVM 相关章节

## 8. Btrfs

The btrfs utility is a toolbox for managing btrfs filesystems. There are command groups to work with subvolumes, devices, for whole filesystem or other specific actions.

### 8.1. /etc/fstab

```
[root@netkiller ~]# cat /etc/fstab 

#
# /etc/fstab
# Created by anaconda on Fri Nov 21 18:16:53 2014
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=6634633e-001d-43ba-8fab-202f1df93339 / ext4 defaults,barrier=0 1 1
UUID=786f570d-fe5c-4d5f-832a-c1b0963dd4e6 /srv btrfs defaults 1 1
UUID=786f570d-fe5c-4d5f-832a-c1b0963dd4e6 /var/lib/mongo  btrfs   noatime,nodiratime,subvol=@mongo 0 2
UUID=786f570d-fe5c-4d5f-832a-c1b0963dd4e6 /var/lib/mysql  btrfs   noatime,nodiratime,subvol=@mysql 0 2
UUID=786f570d-fe5c-4d5f-832a-c1b0963dd4e6 /www  btrfs   noatime,nodiratime,subvol=www 0 2

```

```
[root@netkiller ~]# df -T
Filesystem     Type     1K-blocks    Used Available Use% Mounted on
/dev/vda1      ext4      41151808 4902608  34135768  13% /
devtmpfs       devtmpfs   8127268       0   8127268   0% /dev
tmpfs          tmpfs      8133908       4   8133904   1% /dev/shm
tmpfs          tmpfs      8133908  849468   7284440  11% /run
tmpfs          tmpfs      8133908       0   8133908   0% /sys/fs/cgroup
/dev/vdb1      btrfs    104856576 1404116 101369420   2% /srv
/dev/vdb1      btrfs    104856576 1404116 101369420   2% /var/lib/mongo
/dev/vdb1      btrfs    104856576 1404116 101369420   2% /var/lib/mysql
tmpfs          tmpfs      1626784       0   1626784   0% /run/user/0
/dev/vdb1      btrfs    104856576 1404116 101369420   2% /www

```

```
[root@netkiller ~]# ll /srv/
total 0
drwxr-xr-x 1 www    www    132 2016-12-08 15:17 apache-tomcat-8.5.8
drwxr-xr-x 1 root   root   132 2016-12-08 16:11 apache-tomcat-www
drwxr-xr-x 1 mongod mongod 608 2016-12-26 09:18 @mongo
drwxr-x--x 1 mysql  mysql  448 2016-12-26 09:16 @mysql
drwxr-xr-x 1 root   root     0 2016-11-30 13:24 @redis
drwxr-xr-x 1 root   root    22 2016-12-08 16:32 sbin
drwxr-xr-x 1 www    www     24 2016-12-08 16:54 www

```

### 8.2. btrfs

```
yum install btrfs-progs

```

```
# mkfs.btrfs /dev/sdb1

```

指定卷标

```
# mkfs.btrfs /dev/sdb2 -L /backup

```

### 8.3. Mount Btrfs

```
# mkdir /mnt/btrfs
# mount /dev/sdb1 /mnt/btrfs

```

查看挂载是否成功

```
# df -Th
Filesystem    Type    Size  Used Avail Use% Mounted on
/dev/sda1     ext4     49G   15G   32G  32% /
tmpfs        tmpfs     32G  264K   32G   1% /dev/shm
/dev/sda3     ext4     52G  1.3G   48G   3% /var
/dev/sdb1    btrfs    2.0T   14G  2.0T   1% /mnt/btrfs

```

```
 针对 SSD 的优化:
# mount –t btrfs –o SSD /dev/sda5 /btrfsdisk

打开压缩功能：
# mount –t btrfs –o compress /dev/sda5 /btrfsdisk

```

#### 8.3.1. Mount Snap

mount -t btrfs -o subvol=your_snapshot /dev/sdb2 /mnt/snap

```
mount -t btrfs -o subvol=aaa /dev/md127p5 /mnt/snap

```

#### 8.3.2. fstab

##### 8.3.2.1. btrfs-show

```
[root@r610 ~]# btrfs-show
Label: none  uuid: 0b097eeb-1f0b-476a-955b-52122ef42bfc
        Total devices 1 FS bytes used 13.03GB
        devid    1 size 2.00TB used 24.04GB path /dev/sdb1

Btrfs Btrfs v0.19

```

##### 8.3.2.2. /etc/fstab

```
UUID=0b097eeb-1f0b-476a-955b-52122ef42bfc /opt    btrfs   defaults 1 2

```

### 8.4. subvolumes

```
# df -T
Filesystem     Type  1K-blocks      Used Available Use% Mounted on
/dev/md126p2   ext4   50395844  19952780  27883064  42% /
tmpfs          tmpfs   4024944       800   4024144   1% /dev/shm
/dev/md126p1   ext4     495844    172140    298104  37% /boot
/dev/md126p6   btrfs 500084736 360835636 119893924  76% /opt
/dev/md126p5   btrfs 409600000  24927332 368284612   7% /www

# btrfs subvolume create /www/git
Create subvolume '/www/git'

# btrfs subvolume list /www
ID 641 gen 21351 top level 5 path git

```

fstab 挂在子卷

```
$ cat /etc/fstab 

#
# /etc/fstab
# Created by anaconda on Thu Oct 18 13:53:45 2012
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=88ec1ccf-7d8d-4107-a143-1ed0ec64a572 /                       ext4    defaults        1 1
UUID=c0786771-1c85-45be-a9ab-ef3ee16fccb4 /boot                   ext4    defaults        1 2
UUID=e1b89740-21f0-4507-97e9-a658cd7d3716 /opt                    btrfs   defaults        1 2
UUID=76e46795-ebaf-4d2d-8996-1e15979bf3c8 /www                    btrfs   defaults        1 2
UUID=76e46795-ebaf-4d2d-8996-1e15979bf3c8 /home/git               btrfs   defaults,subvol=git        1 2
UUID=c578f1b3-4bbe-4f48-b3d3-3929c65cb99c swap                    swap    defaults        0 0
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0

```

### 8.5. snapshot

```
创建快照
# btrfs subvolume snapshot /www /www/backup_2012
查看快照
# btrfs subvolume list -a /www
挂在快照
# mount -t btrfs -o subvol=backup_2012 /dev/md127p5 /mnt/snap
删除快照
# btrfs subvolume delete /www/backup_2012
Delete subvolume '/www/backup_2012'

```

### 8.6. btrfsctl

#### 8.6.1. Resizes the filesystem

#### 8.6.2. Snapshot

Btrfs v0.19

```
# touch /mnt/btrfs/test1
# touch /mnt/btrfs/test2
# ls /mnt/btrfs/test?
/mnt/btrfs/test1  /mnt/btrfs/test2

```

```
# echo 'This is a test' > /mnt/btrfs/test1
# btrfsctl –s snap1 /mnt/btrfs
#vi test1
 Test1 is modified
#cd /mnt/btrfs/snap1
#cat test1
 This is a test

```

### 8.7. btrfs-vol

```
# btrfs-vol –a /dev/sdc1 /mnt/btrfs

```

### 8.8. btrfs-convert

```
btrfs-convert /dev/sdb1

```

### 8.9. btrfsck

```
# btrfsck /dev/sdb1
found 13994164224 bytes used err is 0
total csum bytes: 13588316
total tree bytes: 79728640
total fs tree bytes: 28860416
btree space waste bytes: 10282024
file data blocks allocated: 13931024384
 referenced 13906980864
Btrfs Btrfs v0.19

```

### 8.10. btrfs-debug-tree

```
[root@r610 ~]# btrfs-debug-tree /dev/sdb1 |head
root tree
leaf 49463296 items 9 free space 2349 generation 298 owner 1
fs uuid 0b097eeb-1f0b-476a-955b-52122ef42bfc
chunk uuid 2826f868-c775-4835-8690-1020a2a9fbf5
        item 0 key (EXTENT_TREE ROOT_ITEM 0) itemoff 3756 itemsize 239
                root data bytenr 49446912 level 2 dirid 0 refs 1
        item 1 key (DEV_TREE ROOT_ITEM 0) itemoff 3517 itemsize 239
                root data bytenr 36139008 level 0 dirid 0 refs 1
        item 2 key (FS_TREE INODE_REF 6) itemoff 3500 itemsize 17
                inode ref index 0 namelen 7 name: default

```

## 9. zfs

```
# yum info zfs-fuse

Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * addons: mirrors.163.com
 * base: mirrors.163.com
 * epel: mirror01.idc.hinet.net
 * extras: mirrors.163.com
 * updates: mirrors.163.com
Available Packages
Name       : zfs-fuse
Arch       : x86_64
Version    : 0.6.9_beta3
Release    : 0.el5
Size       : 1.5 M
Repo       : epel
Summary    : ZFS ported to Linux FUSE
URL        : http://zfs-fuse.net/
License    : CDDL
Description: ZFS is an advanced modern general-purpose filesystem from Sun
           : Microsystems, originally designed for Solaris/OpenSolaris.
           :
           : This project is a port of ZFS to the FUSE framework for the Linux
           : operating system.
           :
           : Project home page is at http://zfs-fuse.net/

```

## 10. iSCSI

iSCSI 需要与 GFS 配合使用，其他文件系统不能实现数据同步。

过程 12.1. iSCSI Example

1.  install.

    ```
    # yum install iscsi-initiator-utils -y
    # rpm -ql iscsi-initiator-utils
    # rpm -q --scripts iscsi-initiator-utils
    postinstall scriptlet (using /bin/sh):
    /sbin/ldconfig

    if [ ! -f /etc/iscsi/initiatorname.iscsi ]; then
            echo "InitiatorName=`/sbin/iscsi-iname`" > /etc/iscsi/initiatorname.iscsi
    fi
    /sbin/chkconfig --add iscsid
    /sbin/chkconfig --add iscsi
    preuninstall scriptlet (using /bin/sh):
    if [ "$1" = "0" ]; then
        /sbin/chkconfig --del iscsi
        /sbin/chkconfig --del iscsid
    fi
    postuninstall scriptlet (using /bin/sh):
    /sbin/ldconfig

    ```

2.  config

    ```
    # cat /etc/iscsi/initiatorname.iscsi
    InitiatorName=iqn.1994-05.com.redhat:9b2024102698

    ```

3.  starting service.

    ```
    # chkconfig iscsi on
    # chkconfig iscsid on

    # service iscsi start
    iscsid is stopped
    Starting iSCSI daemon:                                     [  OK  ]
                                                               [  OK  ]
    Setting up iSCSI targets: iscsiadm: No records found!
                                                               [  OK  ]
    # service iscsi status
    iscsid (pid  17501) is running...
    # service iscsid status
    iscsid (pid  17501) is running...

    ```

4.  discovery targets.

    ```
    # iscsiadm -m discovery -t sendtargets -p 172.16.0.30:3260
    172.16.0.30:3260,1 iqn.2010-09.com.openfiler:tsn.c7a241688f35

    ```

    or

    ```
    iscsiadm --mode discovery --type sendtargets --portal 172.16.0.30:3260

    iscsiadm -m discovery -t st -p 172.16.0.30:3260

    ```

5.  login / logout

    ```
    # iscsiadm -m node --loginall=all
    Logging in to [iface: default, target: iqn.2010-09.com.openfiler:tsn.c7a241688f35, portal: 172.16.0.30,3260]
    Login to [iface: default, target: iqn.2010-09.com.openfiler:tsn.c7a241688f35, portal: 172.16.0.30,3260]: successful

    ```

    or

    ```
    iscsiadm --mode node --targetname iqn.2010-09.com.openfiler:tsn.c7a241688f35 --portal 192.168.0.10:3260 --login

    ```

    logout

    ```
    # iscsiadm -m node --logoutall=all

    ```

6.  分区设置

    ```
    fdisk -l
    fdisk /dev/sdb #依次选 p n 1 w
    mkfs.ext4 /dev/sdb1

    挂载
    mkdir /iscsi
    mount /dev/sdb1 /iscsi

    设自动挂载
    vi /etc/fstab
    /dev/sdb1 /iscsi ext3 _netdev 0 0

    ```

auth

```
# cp /etc/iscsi/iscsid.conf /etc/iscsi/iscsid.conf.old
# vim /etc/iscsi/iscsid.conf

```

show node

```
]# iscsiadm -m node
172.16.0.30:3260,1 iqn.2006-01.com.openfiler:tsn.0b232d1cc3ee
172.16.0.30:3260,1 iqn.2010-09.com.openfiler:tsn.c7a241688f35

```

delete node

```
iscsiadm -m node -o delete -T iqn.2006-01.com.openfiler:tsn.0b232d1cc3ee

```

### 10.1. GFS

```
[root@dev2 ~]# /etc/init.d/iscsi start
iscsid is stopped
Starting iSCSI daemon:                                     [  OK  ]
                                                           [  OK  ]
Setting up iSCSI targets: iscsiadm: No records found!
                                                           [  OK  ]
[root@dev2 ~]# iscsiadm -m discovery -t st -p 192.168.3.194
192.168.3.194:3260,1 iqn.2007-09.jp.ne.peach.istgt:disk0
[root@dev2 ~]# iscsiadm -m node -l
Logging in to [iface: default, target: iqn.2007-09.jp.ne.peach.istgt:disk0, portal: 192.168.3.194,3260]
Login to [iface: default, target: iqn.2007-09.jp.ne.peach.istgt:disk0, portal: 192.168.3.194,3260]: successful

```

```
# fdisk -l

Disk /dev/sda: 250.0 GB, 250000000000 bytes
255 heads, 63 sectors/track, 30394 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          13      104391   83  Linux
/dev/sda2              14       30394   244035382+  8e  Linux LVM

Disk /dev/sdb: 499.5 GB, 499558383616 bytes
255 heads, 63 sectors/track, 60734 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

Disk /dev/sdb doesn't contain a valid partition table

fdisk /dev/sdb

# fdisk -l /dev/sdb

Disk /dev/sdb: 499.5 GB, 499558383616 bytes
255 heads, 63 sectors/track, 60734 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1               1       60734   487845823+   5  Extended
/dev/sdb5               1       60734   487845792   83  Linux

# mkfs.gfs2 -p lock_dlm -t edb_ha:gfs1 -j 3 /dev/sdb5
This will destroy any data on /dev/sdb5.

Are you sure you want to proceed? [y/n] y

Device:                    /dev/sdb5
Blocksize:                 4096
Device Size                465.25 GB (121961448 blocks)
Filesystem Size:           465.25 GB (121961446 blocks)
Journals:                  3
Resource Groups:           1861
Locking Protocol:          "lock_dlm"
Lock Table:                "edb_ha:gfs1"
UUID:                      A75C4963-85A2-A28B-4099-07FD7E3379D6

```

## 11. GFS - Cluster Storage

[`docs.redhat.com/docs/en-US/Red_Hat_Enterprise_Linux/6/html/Global_File_System_2/index.html`](http://docs.redhat.com/docs/en-US/Red_Hat_Enterprise_Linux/6/html/Global_File_System_2/index.html)

```
yum groupinstall "Cluster Storage"
# egrep 'GFS2|DLM' /boot/config-2.6.32-*
/boot/config-2.6.32-131.2.1.el6.x86_64:CONFIG_GFS2_FS=m
/boot/config-2.6.32-131.2.1.el6.x86_64:CONFIG_GFS2_FS_LOCKING_DLM=y
/boot/config-2.6.32-131.2.1.el6.x86_64:CONFIG_DLM=m
/boot/config-2.6.32-131.2.1.el6.x86_64:CONFIG_DLM_DEBUG=y
/boot/config-2.6.32-71.el6.x86_64:CONFIG_GFS2_FS=m
/boot/config-2.6.32-71.el6.x86_64:CONFIG_GFS2_FS_LOCKING_DLM=y
/boot/config-2.6.32-71.el6.x86_64:CONFIG_DLM=m
/boot/config-2.6.32-71.el6.x86_64:CONFIG_DLM_DEBUG=y

```

```
pvcreate /dev/sdb3
vgcreate vg01 /dev/sdb3
lvcreate -L 500G -n lvol0 vg01
mkfs.gfs2 -p lock_dlm -t alpha:mydata1 -j 8 /dev/vg01/lvol0

```

```
# pvcreate /dev/sdb3
  Physical volume "/dev/sdb3" successfully created
# vgcreate vg01 /dev/sdb3
  Volume group "vg01" successfully created
# lvcreate -L 500G -n lvol0 vg01
  Logical volume "lvol0" created

# mkfs.gfs2 -p lock_dlm -t alpha:mydata1 -j 8 /dev/vg01/lvol0
This will destroy any data on /dev/vg01/lvol0.
It appears to contain: symbolic link to `../dm-6'

Are you sure you want to proceed? [y/n] y

Device:                    /dev/vg01/lvol0
Blocksize:                 4096
Device Size                500.00 GB (131072000 blocks)
Filesystem Size:           500.00 GB (131071998 blocks)
Journals:                  8
Resource Groups:           2000
Locking Protocol:          "lock_dlm"
Lock Table:                "alpha:mydata1"
UUID:                      4FC256A0-00BD-2087-15DF-8EA4366481AA

```

```
# mkdir /mnt/gfs2
# mount -t gfs2 -o noatime /dev/mapper/vg01-lvol0 /mnt/gfs2
gfs_controld join connect error: Connection refused
error mounting lockproto lock_dlm

```

## 12. glusterfs

```
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/6/x86_64/epel-release-6-5.noarch.rpm
# yum install glusterfs glusterfs-fuse glusterfs-rdma glusterfs-server glusterfs-vim

# gzip /etc/glusterfs/*

```

```
# chkconfig glusterd off
# chkconfig glusterfsd on

# service glusterd stop
# service glusterfsd start

or

/etc/init.d/glusterd start
/etc/init.d/glusterfsd start

```

```

mkdir -p /home/export

cat >> /etc/hosts <<EOF
192.168.80.107  gluster1
192.168.80.33   gluster2
192.168.80.1    gluster3
EOF

# glusterfs-volgen --name store1 --raid 1 gluster1:/home/export gluster2:/home/export
Generating server volfiles.. for server gluster2 as '/root/gluster2-store1-export.vol'
Generating server volfiles.. for server gluster1 as '/root/gluster1-store1-export.vol'
Generating client volfiles.. '/root/store1-tcp.vol'

cp ./store1-tcp.vol /etc/glusterfs/glusterfs.vol
scp gluster1-store1-export.vol root@gluster1:/etc/glusterfs/glusterfsd.vol
scp gluster2-store1-export.vol root@gluster2:/etc/glusterfs/glusterfsd.vol

ssh root@gluster1 service glusterfsd restart
ssh root@gluster2 service glusterfsd restart

# mkdir -p /mnt/glusterfs
# glusterfs /mnt/glusterfs/
or
# glusterfs -l /tmp/glusterfs.log -f /etc/glusterfs/glusterfs.vol /mnt/glusterfs/

```

Umount

```

umount /mnt/glusterfs

```

注意：请使用 umount 卸载，不要 kill glusterfs 进程

## 13. RAM FS

```
# mkdir -p /mnt/ram1
# mount -t ramfs none /mnt/ram1 -o maxsize=10000

```

## 14. tmpfs

```
# mkdir -p /mnt/tmpfs
# mount tmpfs /mnt/tmpfs -t tmpfs
# mount tmpfs /mnt/tmpfs -t tmpfs -o size=32m

```

## 15. ftp fs

安装

```
sudo apt-get install curlftpfs

```

挂载

```
sudo curlftpfs ftp://username:password@172.16.0.1 /mnt/ftp

```

卸载

```
sudo fusermount -u /mnt/ftp

```

权限设置

```
sudo curlftpfs -o rw,allow_other,uid=500,gid=500 ftp://neo:chen@172.16.1.1 /mnt/ftp
sudo curlftpfs ftp://host/sub_dir mount_point -o user="ftp_username:ftp_password", uid=user_id, gid=group_id, allow_other

```

fstab 开机自动挂载

```
sudo echo "curlftpfs#username:password@172.16.0.1 /mnt/ftp fuse allow_other,uid=userid,gid=groupid 0 0" >> /etc/fstab

```

## 16. SSHFS (sshfs - filesystem client based on SSH File Transfer Protocol)

```
$ sudo apt-get install sshfs
$ sudo sshfs root@172.16.0.5:/home/neo /mnt
$ sudo fusermount -u /mnt

```

## 17. davfs2 - mount a WebDAV resource as a regular file system

install

```
$ sudo apt-get install davfs2

```

mount a webdav to directory

```
$ sudo mount.davfs https://opensvn.csie.org/netkiller /mnt/davfs/
Please enter the username to authenticate with server
https://opensvn.csie.org/netkiller or hit enter for none.
Username: svn
Please enter the password to authenticate user svn with server
https://opensvn.csie.org/netkiller or hit enter for none.
Password:
mount.davfs: the server certificate is not trusted
  issuer:      CSIE.org, CSIE.org, Taipei, Taiwan, TW
  subject:     CSIE.org, CSIE.org, Taipei, TW
  identity:    *.csie.org
  fingerprint: e6:05:eb:fb:69:5d:25:4e:11:3c:83:e8:7c:44:ee:bf:a9:85:a3:64
You only should accept this certificate, if you can
verify the fingerprint! The server might be faked
or there might be a man-in-the-middle-attack.
Accept certificate for this session? [y,N] y

```

test

```
$ ls davfs/
branches  lost+found  tags  trunk

```

## 18. redisfs

Redis Filesystem

```
redisfs --host=localhost --port=6379 --mount=/mnt/redis  [--read-only] [--debug] [--prefix=skx]

```

创建快照

```
redisfs-snapsot --from=skx --to=snap			

```

Mount 快照

```
mkdir /tmp/snapsot
redisfs --prefix=snap --mount=/tmp/snapsot

```

## 19. File system test

写写空文件

```
$ dd bs=1 seek=2TB if=/dev/null of=test
$ time dd if=/dev/zero of=/srv/file bs=1M count=1000

```

写随机文件

```
$ time dd if=/dev/urandom of=test.txt bs=1M count=1000		

```

### 19.1. ext4 vs btrfs

```

$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid -o value -s UUID' to print the universally unique identifier
# for a device; this may be used with UUID= as a more robust way to name
# devices that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
proc            /proc           proc    nodev,noexec,nosuid 0       0
/dev/sda1       /               ext4    errors=remount-ro 0       1
# /opt was on /dev/sda7 during installation
UUID=5ce518a4-0f46-4688-8002-84bac2330282 /opt            btrfs   defaults        0       2
# /srv was on /dev/sda6 during installation
UUID=19573a64-f0a6-4250-a9fd-532e3d4e3477 /srv            ext4   defaults        0       2
# swap was on /dev/sda5 during installation
UUID=0f2e2f50-d989-47bf-afb7-7593888222cf none            swap    sw              0       0

```

```

neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/srv/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.500941 s, 209 MB/s

real	0m0.521s
user	0m0.000s
sys	0m0.140s
neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/srv/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.672553 s, 156 MB/s

real	0m0.698s
user	0m0.000s
sys	0m0.160s
neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/opt/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.0987276 s, 1.1 GB/s

real	0m0.133s
user	0m0.000s
sys	0m0.120s
neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/opt/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.101664 s, 1.0 GB/s

real	0m0.134s
user	0m0.000s
sys	0m0.140s

neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/srv/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 11.8609 s, 88.4 MB/s

real	0m11.914s
user	0m0.010s
sys	0m1.360s
neo@neo-Vostro-3400:~$ time dd if=/dev/zero of=/opt/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 9.80331 s, 107 MB/s

real	0m9.860s
user	0m0.000s
sys	0m0.880s
neo@neo-Vostro-3400:~$

```

### 19.2. xfs vs jfs vs reiserfs

```

$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
proc            /proc           proc    defaults        0       0
# /dev/sda2       /               ext3    errors=remount-ro 0       1
UUID=8ce10c79-f97e-4585-8c07-75f64f043137       /               ext3    errors=remount-ro 0       1
# /dev/sda1       /boot           ext3    defaults        0       2
UUID=35705945-65ed-437b-9f79-fd0e014d100c       /boot           ext3    defaults        0       2
# /dev/sda5       /home           reiserfs defaults        0       2
UUID=f376adb7-e943-4805-892a-4fa457150b66       /home           reiserfs defaults        0       2
# /dev/sda7       /srv            xfs     defaults        0       2
UUID=2ee5c516-707f-47dc-a6a6-0d49d5dc9829       /srv            xfs     defaults        0       2
# /dev/sda8       /var            jfs     defaults        0       2
UUID=2928ba86-72fb-4b60-adc2-0f7d47e62d03       /var            jfs     defaults        0       2
# /dev/sda6       /var/www        ext3    defaults        0       2
UUID=34d3890f-a682-42ea-bdca-815f442e6539       /var/www        ext3    defaults        0       2
# /dev/sda3       none            swap    sw              0       0
UUID=bf605f47-70bc-4653-be98-c8659f959e25       none            swap    sw              0       0
/dev/scd0       /media/cdrom0   udf,iso9660 user,noauto     0       0

```

```

# XFS

neo@deployment:~$ time dd if=/dev/zero of=/srv/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.0897329 s, 1.2 GB/s

real	0m0.117s
user	0m0.000s
sys	0m0.088s
neo@deployment:~$ time dd if=/dev/zero of=/srv/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 4.86382 s, 216 MB/s

real	0m4.885s
user	0m0.000s
sys	0m0.960s

# JFS

neo@deployment:~$ time dd if=/dev/zero of=/var/tmp/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.127605 s, 822 MB/s

real	0m0.157s
user	0m0.000s
sys	0m0.100s
neo@deployment:~$ time dd if=/dev/zero of=/var/tmp/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 9.58573 s, 109 MB/s

real	0m9.597s
user	0m0.000s
sys	0m0.988s

# reiserfs

neo@deployment:~$ time dd if=/dev/zero of=/home/neo/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.392038 s, 267 MB/s

real	0m0.430s
user	0m0.000s
sys	0m0.252s
neo@deployment:~$ time dd if=/dev/zero of=/home/neo/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 4.62378 s, 227 MB/s

real	0m4.663s
user	0m0.000s
sys	0m2.592s

# EXT3

neo@deployment:~$ time dd if=/dev/zero of=/var/www/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.189314 s, 554 MB/s

real	0m0.207s
user	0m0.004s
sys	0m0.176s
neo@deployment:~$ time dd if=/dev/zero of=/var/www/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 6.46036 s, 162 MB/s

real	0m7.460s
user	0m0.008s
sys	0m1.832s

```

### 19.3. RAID10 (146G*8) vs EMC VNX 5300 (8G Fibre Channel)

服务器 RAID 卡带宽是 6G，而 Fibre Channel 目前是 8G，ISCSI 与 FCoE 可以提供 10G 带宽，InfiniBand 可以提供 120G 带宽。

```
# cat /etc/fstab
LABEL=/                 /                       ext3    defaults        1 1
LABEL=/boot             /boot                   ext3    defaults        1 2
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0
LABEL=SWAP-sda3         swap                    swap    defaults        0 0
/dev/sda4               /home/oracle/rman       ext3    defaults        0 2
/dev/sdb1               /opt/oracle     ext3    defaults        0 2
/dev/emcpowerj1         /u02                    ext3    defaults        0 0

# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda2              95G  7.8G   82G   9% /
/dev/sda1             1.9G   42M  1.8G   3% /boot
tmpfs                  63G  534M   63G   1% /dev/shm
/dev/sda4             924G  619G  258G  71% /home/oracle/rman
/dev/sdb1             1.1T  309G  735G  30% /opt/oracle
/dev/emcpowerj1       296G   20G  261G   7% /u02

```

IBM X3850 G5

```
# time dd if=/dev/zero of=file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.152702 seconds, 687 MB/s

real	0m0.154s
user	0m0.001s
sys	0m0.153s
# time dd if=/dev/zero of=file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 1.6589 seconds, 632 MB/s

real	0m1.710s
user	0m0.009s
sys	0m1.657s

# time dd if=/dev/zero of=file bs=1G count=2
2+0 records in
2+0 records out
2147483648 bytes (2.1 GB) copied, 3.48809 seconds, 616 MB/s

real	0m5.899s
user	0m0.000s
sys	0m5.594s

```

EMC

```
# time dd if=/dev/zero of=file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.175535 seconds, 597 MB/s

real	0m0.178s
user	0m0.001s
sys	0m0.171s
# time dd if=/dev/zero of=file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 1.67429 seconds, 626 MB/s

real	0m1.718s
user	0m0.002s
sys	0m1.664s
# time dd if=/dev/zero of=file bs=1G count=2
2+0 records in
2+0 records out
2147483648 bytes (2.1 GB) copied, 3.46919 seconds, 619 MB/s

real	0m3.757s
user	0m0.002s
sys	0m3.656s

```

### 19.4. Dell 2950(RAID5 500G SATA * 6) vs MD1200

```

# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid -o value -s UUID' to print the universally unique identifier
# for a device; this may be used with UUID= as a more robust way to name
# devices that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
proc            /proc           proc    nodev,noexec,nosuid 0       0
# / was on /dev/sda1 during installation
UUID=2fc411ec-9f6e-4e04-9270-11d23a9b0668 /               ext4    errors=remount-ro 0       1
# swap was on /dev/sda2 during installation
UUID=f5175b7a-4c87-471c-ab9f-9d601bc5e6e2 none            swap    sw              0       0
UUID=3217bdd9-1beb-494a-a428-8d1c09eaa1af /backup ext4 errors=remount-ro 0       1
UUID=9bed3b85-bbc5-4aec-8c9a-8911712ea0c6 /backup1 ext4 errors=remount-ro 0       1
UUID=24bec385-2074-4eb5-8d24-22c33dc245d8 /backup2 ext4 errors=remount-ro 0       1

# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              46G  8.6G   35G  20% /
none                  2.0G  212K  2.0G   1% /dev
none                  2.0G     0  2.0G   0% /dev/shm
none                  2.0G  9.0M  2.0G   1% /var/run
none                  2.0G     0  2.0G   0% /var/lock
none                   46G  8.6G   35G  20% /var/lib/ureadahead/debugfs
/dev/sda3             2.2T  2.0T   89G  96% /backup
/dev/sdc1             7.2T  6.0T  887G  88% /backup2
/dev/sdb1             9.0T  7.0T  1.6T  83% /backup1

```

/dev/sda 是 2950 RAID5 500G*6 1000RPM

/dev/sdb 是 MD1200 RAID5 2T*6 7200RPM

/dev/sdc 是 MD1200 RAID50 2T*6 7200RPM

```
root@backup:~# time dd if=/dev/zero of=/backup/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.156596 s, 670 MB/s

real	0m0.242s
user	0m0.010s
sys	0m0.150s
root@backup:~# time dd if=/dev/zero of=/backup/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 4.61282 s, 227 MB/s

real	0m4.763s
user	0m0.000s
sys	0m1.640s
root@backup:~# time dd if=/dev/zero of=/backup/file bs=1G count=5
5+0 records in
5+0 records out
5368709120 bytes (5.4 GB) copied, 33.7263 s, 159 MB/s

real	0m34.685s
user	0m0.000s
sys	0m13.070s
root@backup:~# time dd if=/dev/zero of=/backup1/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.130451 s, 804 MB/s

real	0m0.290s
user	0m0.000s
sys	0m0.130s
root@backup:~# time dd if=/dev/zero of=/backup1/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 57.1654 s, 18.3 MB/s

real	0m57.206s
user	0m0.000s
sys	0m1.580s
root@backup:~# time dd if=/dev/zero of=/backup1/file bs=1G count=5
5+0 records in
5+0 records out
5368709120 bytes (5.4 GB) copied, 309.194 s, 17.4 MB/s

real	5m9.762s
user	0m0.000s
sys	0m10.820s
root@backup:~# time dd if=/dev/zero of=/backup2/file bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB) copied, 0.145224 s, 722 MB/s

real	0m0.333s
user	0m0.000s
sys	0m0.130s
root@backup:~# time dd if=/dev/zero of=/backup2/file bs=1M count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 41.9185 s, 25.0 MB/s

real	0m41.979s
user	0m0.010s
sys	0m1.930s
root@backup:~# time dd if=/dev/zero of=/backup2/file bs=1G count=5
5+0 records in
5+0 records out
5368709120 bytes (5.4 GB) copied, 200.384 s, 26.8 MB/s

real	3m20.850s
user	0m0.000s
sys	0m12.490s

```

## 20. 磁盘占用 100%删除文件后不是放的解决方法

首先查看 delete 状态的进程，然后 kill 进程，或者重启

```
lsof | grep delete

```

## 第 13 章 Networking 网络管理

## 1. hosts

```

# cat -n /etc/hosts
     1  # Do not remove the following line, or various programs
     2  # that require network functionality will fail.
     3  127.0.0.1               development.domain.org development netkiller.localdomain netkiller
     4  ::1             localhost6.localdomain6 localhost6

```

### 1.1. /etc/hostname

```
# cat /etc/hostname
web1.example.com

```

查看 IP 地址

```

[root@localhost ~]# hostname --ip-address
::1 127.0.0.1			

```

### 1.2. hostnamectl - Control the system hostname

```
[root@netkiller ~]# hostnamectl
   Static hostname: netkiller.localdomain
         Icon name: computer-desktop
           Chassis: desktop
        Machine ID: 072e88a0fdd2447296554f3cd5129076
           Boot ID: a978056f50544355abd723b328a89b6f
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-229.el7.x86_64
      Architecture: x86_64

```

设置 hostname

```

[root@netkiller ~]# hostnamectl set-hostname master

```

### 1.3. /etc/host.conf

解析顺序配置文件

```
[root@development bin]# cat /etc/host.conf
order hosts,bind

```

首先在/etc/hosts 文件中寻找，如果不存在，再去 DNS 服务器中寻找

### 1.4. /etc/hosts

IP 地址后面 TAB 符，然后写主机地址

```
127.0.0.1       localhost.localdomain localhost
::1             localhost6.localdomain6 localhost6
192.168.1.10	development.example.com development

```

### 1.5. hosts.allow / hosts.deny

/etc/hosts.allow 和 /etc/hosts.deny

许可 IP／禁止 IP，相当于黑白名单

### 1.6. /etc/resolv.conf

```

search example.com
nameserver 208.67.222.222
nameserver 208.67.220.220

```

## 2. Hostname

## 3. Network adapter 网络适配器

**ethtool eth1**

```
neo@shenzhen:~/doc/Linux/xhtml$ sudo ethtool eth1
Settings for eth1:
        Supported ports: [ TP MII ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
        Advertised auto-negotiation: Yes
        Speed: 100Mb/s
        Duplex: Full
        Port: MII
        PHYAD: 32
        Transceiver: internal
        Auto-negotiation: on
        Supports Wake-on: pumbg
        Wake-on: d
        Current message level: 0x00000007 (7)
        Link detected: yes

```

**mii-tool**

```
neo@shenzhen:~/doc/Linux/xhtml$ sudo mii-tool
eth1: negotiated 100baseTx-FD, link ok

```

### 3.1. 接口名称

Linux 网卡默认接口名称是 eth0，如果你想定义其他名称可以更改下面文件。

/etc/udev/rules.d/70-persistent-net.rules

```
cat /etc/udev/rules.d/70-persistent-net.rules

# This file maintains persistent names for network interfaces.
# See udev(7) for syntax.
#
# Entries are automatically added by the 75-persistent-net-generator.rules
# file; however you are also free to add your own entries.

# PCI device 0x10ec:0x8136 (r8169)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:1d:92:f0:37:58", ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"

```

双网卡实例

```
# cat /etc/udev/rules.d/70-persistent-net.rules
# This file was automatically generated by the /lib/udev/write_net_rules
# program, run by the persistent-net-generator.rules rules file.
#
# You can modify it, as long as you keep each rule on a single
# line, and change only the value of the NAME= key.

# PCI device 0x8086:0x10d3 (e1000e)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:25:90:35:91:36", ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"

# PCI device 0x8086:0x10d3 (e1000e)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:25:90:35:91:37", ATTR{type}=="1", KERNEL=="eth*", NAME="eth1"

```

## 4. Ethernet Interfaces 以太网接口

restart

```
sudo /etc/init.d/networking restart

```

### 4.1. ifquery

```
$ sudo ifquery --list
lo
eth0
eth1

```

### 4.2. DHCP

DHCP

```
sudo vi /etc/network/interfaces

# The primary network interface - use DHCP to find our address
auto eth0
iface eth0 inet dhcp

```

### 4.3. Static IP

Static IP

```
# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.3.90
gateway 192.168.3.1
netmask 255.255.255.0
network 192.168.3.0
broadcast 192.168.3.255

dns-nameservers 8.8.8.8 4.4.4.4

```

Setting up Second IP address or Virtual IP address in Ubuntu

```
sudo vi /etc/network/interfaces

auto eth0:1
iface eth0:1 inet static
address 192.168.1.60
netmask 255.255.255.0
network x.x.x.x
broadcast x.x.x.x
gateway x.x.x.x

dns-nameservers 8.8.8.8 4.4.4.4

```

## 5. Mask 子网掩码

举例说明该算法。
例：给定一 class c address : 192.168.5.0 ，要求划分 20 个子网，每个子网 5 个主机。
解：因为 4 <5 < 8 ，用 256－8＝248 ---->即是所求的子网掩码，对应的子网数也就出来了。这是针对 C 类地址。
针对 B 类地址的做法。对于 B 类地址，假如主机数小于或等于 254，与 C 类地址算法相同。对于主机数大于 254 的，如需主机 700 台，50 个子网（相当大了），512 < 700< 1024
256－（1024/256）=256－4＝252 ---->即是所求的子网掩码，对应的子网数也就出来了。上面 256－4 中的 4（2 的 2 次幂）是指主机数用 2 进制表示时超过 8 位的位数，即超过 2 位，掩码为剩余的前 6 位，即子网数为 2（6）－2＝62 个。

```
Append :Host/Subnet Quantities Table

----------------------------------------------------------------------
Class A                   Effective  Effective
# bits        Mask         Subnets     Hosts
-------  ---------------  ---------  ---------
  2      255.192.0.0            2      4194302
  3      255.224.0.0            6      2097150
  4      255.240.0.0           14      1048574
  5      255.248.0.0           30       524286
  6      255.252.0.0           62       262142
  7      255.254.0.0          126       131070
  8      255.255.0.0          254        65536
  9      255.255.128.0        510        32766
  10     255.255.192.0       1022        16382
  11     255.255.224.0       2046         8190
  12     255.255.240.0       4094         4094
  13     255.255.248.0       8190         2046
  14     255.255.252.0      16382         1022
  15     255.255.254.0      32766          510
  16     255.255.255.0      65536          254
  17     255.255.255.128   131070          126
  18     255.255.255.192   262142           62
  19     255.255.255.224   524286           30
  20     255.255.255.240  1048574           14
  21     255.255.255.248  2097150            6
  22     255.255.255.252  4194302            2

Class B                   Effective  Effective
# bits        Mask         Subnets     Hosts
-------  ---------------  ---------  ---------
  2      255.255.192.0           2     16382
  3      255.255.224.0           6      8190
  4      255.255.240.0          14      4094
  5      255.255.248.0          30      2046
  6      255.255.252.0          62      1022
  7      255.255.254.0         126       510
  8      255.255.255.0         254       254
  9      255.255.255.128       510       126
  10     255.255.255.192      1022        62
  11     255.255.255.224      2046        30
  12     255.255.255.240      4094        14
  13     255.255.255.248      8190         6
  14     255.255.255.252     16382         2

Class C                   Effective  Effective
# bits        Mask         Subnets     Hosts
-------  ---------------  ---------  ---------
  2      255.255.255.192      2         62
  3      255.255.255.224      6         30
  4      255.255.255.240     14         14
  5      255.255.255.248     30          6
  6      255.255.255.252     62          2

*Subnet all zeroes and all ones excluded.
*Host all zeroes and all ones excluded.

```

## 6. Gateway 设置默认网关

default gateway

```
$ sudo route add default gw 172.16.0.1

```

```
$ sudo ip route default via 172.16.0.1 dev eth0

```

## 7. Configuring Name Server Lookups

Setting up DNS

```
When it comes to DNS setup Ubuntu doesn’t differ from other distributions. You can add hostname and IP addresses to the file /etc/hosts for static lookups.

To cause your machine to consult with a particular server for name lookups you simply add their addresses to /etc/resolv.conf.

For example a machine which should perform lookups from the DNS server at IP address 192.168.3.2 would have a resolv.conf file looking like this

sudo vi /etc/resolv.conf

enter the following details

search test.com
nameserver 192.168.3.2

```

```
domain domain.com
search www.domain.com domain.com
nameserver 202.96.128.86
nameserver 202.96.134.133

```

## 8. IP forwarding(IP 转发)

enable IP forwarding

```
neo@shenzhen:~$ sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1

```

```
# enable IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

```

ubuntu

```
sysctl -w net.ipv4.ip_forward=1

```

## 9. bonding

绑定的前提条件：芯片组型号相同，而且网卡应该具备自己独立的 BIOS 芯片。

**#vi ifcfg-bond0**

```
# cat ifcfg-bond0
DEVICE=bond0
BOOTPROTO=static
IPADDR=172.16.0.1
NETMASK=255.255.252.0
BROADCAST=172.16.3.254
ONBOOT=yes
TYPE=Ethernet

```

这里要主意，不要指定单个网卡的 IP 地址、子网掩码。将上述信息指定到虚拟适配器(bonding)中即可

```
[root@rhas-13 network-scripts]# cat ifcfg-eth0
DEVICE=eth0
ONBOOT=yes
BOOTPROTO=dhcp

[root@rhas-13 network-scripts]# cat ifcfg-eth1
DEVICE=eth1
ONBOOT=yes
BOOTPROTO=dhcp

```

编辑 /etc/modules.conf 文件，加入如下一行内容，以使系统在启动时加载 bonding 模块，对外虚拟网络接口设备为 bond0.加入下列两行:

*** /etc/modules.conf 文件已经不再使用**

```

cat >> /etc/modprobe.d/bonding.conf <<EOF
alias bond0 bonding
options bond0 miimon=100 mode=1
EOF

```

说明：miimon 是用来进行链路监测的。比如:miimon=100，那么系统每 100ms 监测一次链路连接状态，如果有一条线路不通就转入另一条线路；mode 的值表示工作模式，他共有 0， 1,2,3 四种模式，常用的为 0,1 两种。mode=0 表示 load balancing (round-robin)为负载均衡方式，两块网卡都工作。mode=1 表示 fault-tolerance (active-backup)提供冗余功能，工作方式是主备的工作方式,也就是说默认情况下只有一块网卡工作,另一块做备份。bonding 只能提供链路监测，即从主机到交换机的链路是否接通。如果只是交换机对外的链路 down 掉了，而交换机本身并没有故障，那么 bonding 会认为链路没有问题而继续使用。

**# vi /etc/rc.d/rc.local**

```
ifenslave bond0 eth0 eth1
route add -net 172.31.3.254 netmask 255.255.255.0 bond0

```

到这时已经配置完毕 重新启动机器。重启会看见以下信息就表示配置成功了

```
................
Bringing up interface bond0 OK
Bringing up interface eth0 OK
Bringing up interface eth1 OK
................

```

mode=1 工作在主备模式下,这时 eth1 作为备份网卡是 no arp 的 [root@rhas-13 network-scripts]# ifconfig 验证网卡的配置信息

那也就是说在主备模式下,当一个网络接口失效时(例如主交换机掉电等),不回出现网络中断, 系统会按照 cat /etc/rc.d/rc.local 里指定网卡的顺序工作,机器仍能对外服务,起到了失效保护的功能。在 mode=0 负载均衡工作模式,他能提供两倍的带宽,下我们来看一下网卡的配置信息：

在这种情况下出现一块网卡失效,仅仅会是服务器出口带宽下降,也不会影响网络使用。通过查看 bond0 的工作状态查询能详细的掌握 bonding 的工作状态

Linux 下通过网卡邦定技术既增加了服务器的可靠性,又增加了可用网络带宽,为用户提供不间断的关键服务。

### 9.1. Ubuntu

ifenslave

```
apt-get install ifenslave-2.6

```

/etc/modules

```
bonding

```

modprobe bonding

/etc/modprobe.d/aliases

```
alias bond0 bonding
options bonding mode=0 miimon=100

or

options bonding mode=1 miimon=100 downdelay=200 updelay=200

```

例 13.1. bonding example

/etc/network/interfaces

```
auto lo
iface lo inet loopback

iface eth0 inet dhcp
iface eth1 inet dhcp

auto bond0
iface bond0 inet static
address 172.16.0.1
netmask 255.255.255.0
gateway 172.16.0.254
up ifenslave bond0 eth0 eth1
down ifenslave -d bond0 eth0 eth1

```

## 10. Wireless - WiFi 配置

### 10.1. rfkill - tool for enabling and disabling wireless devices

```
$ rfkill list all
0: phy0: Wireless LAN
	Soft blocked: no
	Hard blocked: yes

```

锁定无线设备

```
$ rfkill block 0

$ rfkill list 
0: phy0: Wireless LAN
	Soft blocked: yes
	Hard blocked: yes			

```

解锁无线设备

```
$ rfkill unblock all

$ rfkill list all
0: phy0: Wireless LAN
	Soft blocked: no
	Hard blocked: yes

```

### 10.2. iwlist - Get more detailed wireless information from a wireless interface

```
$ sudo iwlist wlan1 scanning |more

wlan1     Scan completed :
          Cell 01 - Address: 04:A1:51:99:0A:25
                    Channel:1
                    Frequency:2.412 GHz (Channel 1)
                    Quality=43/70  Signal level=-67 dBm  
                    Encryption key:on
                    ESSID:"szgw-p5"
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s
                              9 Mb/s; 12 Mb/s; 18 Mb/s
                    Bit Rates:24 Mb/s; 36 Mb/s; 48 Mb/s; 54 Mb/s
                    Mode:Master
                    Extra:tsf=0000000904486387
                    Extra: Last beacon: 1068ms ago
                    IE: Unknown: 0007737A67772D7035
                    IE: Unknown: 010882848B960C121824
                    IE: Unknown: 030101
                    IE: Unknown: 0706434E20010D14
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : CCMP TKIP
                        Authentication Suites (1) : PSK
                    IE: WPA Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : CCMP TKIP
                        Authentication Suites (1) : PSK
                    IE: Unknown: 2A0100
                    IE: Unknown: 32043048606C
                    IE: Unknown: DD180050F20201018D0003A4000027A4000042435E0062322F00
                    IE: Unknown: DD1E00904C33CE111BFFFF000000000000000000000000000000000000000000
                    IE: Unknown: 2D1ACE111BFFFF000000000000000000000000000000000000000000
                    IE: Unknown: DD1A00904C34010D0A00000000000000000000000000000000000000
                    IE: Unknown: 3D16010D0A00000000000000000000000000000000000000
                    IE: Unknown: 4A0E14000A002C01C800140005001900
                    IE: Unknown: 7F0101
                    IE: Unknown: DD0900037F01010000FF7F
                    IE: Unknown: DD0A00037F04010000004000

```

搜索 SSID

```
$ sudo iwlist wlan1 scanning | grep ESSID
                    ESSID:"product"
                    ESSID:"wifi123456"
                    ESSID:"ChinaNet-zNNs"
                    ESSID:"ChinaNet-dqar"
                    ESSID:"360WiFi-SEM"
                    ESSID:"360\xE5\x85\x8D\xE8\xB4\xB9WiFi-A5"
                    ESSID:"\xE5\x8F\x96\xE4\xB8\xAA\xE4\xBB\x80\xE4\xB9\x88\xE5\x90\x8D\xE5\xAD\x97\xE5\x91\xA2\xEF\xBC\x9F"
                    ESSID:""
                    ESSID:""			

```

### 10.3. iwconfig - configure a wireless network interface

```

$ sudo iwconfig eth1 essid <ESSID> key <PASSWORD>			

```

### 10.4. /proc/net/wireless

```
$ cat /proc/net/wireless
Inter-| sta-|   Quality        |   Discarded packets               | Missed | WE
 face | tus | link level noise |  nwid  crypt   frag  retry   misc | beacon | 22			

```

例 13.2. 命令行建立 WiFi 链接步骤

```
$ sudo rfkill unblock all
$ sudo ifconfig wlan1 up
$ sudo iwlist wlan1 scanning | grep ESSID
$ sudo iwconfig wlan1 essid Netkiller key 66535215

```

## 11. CentOS 网络配置

**system-config-network**

```
ifconfig eth0 192.168.0.10 netmask 255.255.255.0
or
ip addr add 192.168.0.10 dev eth0

```

ifcfg-eth0,ifcfg-eth1,ifcfg-eth2 ... ifcfg-eth(n)

```

[root@development httpd]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
# Broadcom Corporation NetLink BCM5784M Gigabit Ethernet PCIe
DEVICE=eth0
BOOTPROTO=static
BROADCAST=192.168.3.255
HWADDR=00:25:64:A3:59:BF
IPADDR=192.168.3.40
IPV6INIT=yes
IPV6_AUTOCONF=yes
NETMASK=255.255.255.0
NETWORK=192.168.3.0
ONBOOT=yes

```

eth0:1

```

[root@development httpd]# cp /etc/sysconfig/network-scripts/ifcfg-eth0 /etc/sysconfig/network-scripts/ifcfg-eth0:1
[root@development httpd]# vi /etc/sysconfig/network-scripts/ifcfg-eth0:1
# Broadcom Corporation NetLink BCM5784M Gigabit Ethernet PCIe
DEVICE=eth0:1
BOOTPROTO=static
BROADCAST=192.168.3.255
HWADDR=00:25:64:A3:59:BF
IPADDR=192.168.3.41
IPV6INIT=yes
IPV6_AUTOCONF=yes
NETMASK=255.255.255.0
NETWORK=192.168.3.0
ONBOOT=yes

```

reload network

```

[root@development ~]# /etc/init.d/network reload
Shutting down interface eth0:                              [  OK  ]
Shutting down loopback interface:                          [  OK  ]
Bringing up loopback interface:                            [  OK  ]
Bringing up interface eth0:

```

### 11.1. Gateway

```

[root@development ~]# cat /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=yes
HOSTNAME=development.domain.org
GATEWAY=192.168.3.1

```

### 11.2. bonding

```

cat >> /etc/modprobe.d/bonding.conf <<EOF
alias bond0 bonding
options bond0 mode=balance-alb miimon=1000
EOF

cat > /etc/sysconfig/network-scripts/ifcfg-eth0 <<EOF
DEVICE="eth0"
ONBOOT="yes"
BOOTPROTO="none"
USERCTL="no"
NM_CONTROLLED="no"
EOF

cat > /etc/sysconfig/network-scripts/ifcfg-eth1 <<EOF
DEVICE="eth1"
ONBOOT="yes"
BOOTPROTO="none"
USERCTL="no"
NM_CONTROLLED="no"
EOF

cat > /etc/sysconfig/network-scripts/ifcfg-bond0 <<EOF
DEVICE="bond0"
ONBOOT="yes"
BOOTPROTO="none"
TYPE="Ethernet"
IPADDR=172.16.0.5
NETMASK=255.255.255.0
NETWORK=172.16.0.0
USERCTL="no"
NM_CONTROLLED="no"
EOF

modprobe bonding mode=balance-alb miimon=1000
ifconfig bond0 up
ifconfig bond0 172.16.0.5 netmask 255.255.255.0 up
ip route add default via 172.16.0.254 dev bond0
ifenslave bond0 eth0
ifenslave bond0 eth1

cat >> /etc/rc.local <<EOF
#-------------------------
ifenslave bond0 eth0
ifenslave bond0 eth1
ip route add default via 172.16.0.254 dev bond0
#-------------------------
EOF

more /proc/net/bonding/bond0

```

### 11.3. brctl

Linux 系统 4 个物理网卡的名称则分别为 eth0，eth1，eth2，eth3。我们将四个网口桥接到 br0 端口。

你可以这样理解 vlan 2, vlan ip 192.168.0.1，然后将 4 个接口划分到 vlan2, 这时这 4 个接口可以通过 vlan 2 访问其他用户。我只是做了一个比喻，让你能够理解。

```
# brctl addbr br0

# brctl addif br0 eth0
# brctl addif br0 eth1
# brctl addif br0 eth2
# brctl addif br0 eth3

# ifconfig eth0 0.0.0.0
# ifconfig eth1 0.0.0.0
# ifconfig eth2 0.0.0.0
# ifconfig eth3 0.0.0.0

# ifconfig br0 192.168.0.1

```

## 12. 网络检查命令

### 12.1. ping

```

-f: 发送洪水请求,每个请求打印一个点,每个响应删除一个点.如果网络存在丢包,那么会呈现出一长串不断增加的点.

-n: 选项,加上之后可以阻止 ping 程序去进行反向 dns 查询
    当每次 ping 完得到响应之后,ping 程序会尝试一次反向 dns 查询(reverse dns lookup)来获取“64 bytes from”后面的域名,如果查询速度很慢的话,就会给人似乎延迟很大的感觉,其实这也是 ping 感觉慢,但是每次 ping 的响应时间却并不慢的原因.

```

### 12.2. Finding optimal MTU

```

$ ping -c 1 -s $((1500-28)) -M do www.debian.org
PING www.debian.org (140.112.8.139) 1472(1500) bytes of data.
1480 bytes from linux3.cc.ntu.edu.tw (140.112.8.139): icmp_seq=1 ttl=47 time=52.7 ms

--- www.debian.org ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 52.778/52.778/52.778/0.000 ms

```

Try 1454 instead of 1500

### 12.3. ss - another utility to investigate sockets

```

ss 是 Socket Statistics 的缩写
    ss 命令可以用来获取 socket 统计信息,它可以显示和 netstat 类似的内容;但 ss 的优势在于它能够显示更多更详细的有关 TCP 和连接状态的信息,而且比 netstat 更快速更高效.

    当服务器的 socket 连接数量变得非常大时,无论是使用 netstat 命令还是直接 cat /proc/net/tcp,执行速度都会很慢;ss 快的秘诀在于,它利用到了 TCP 协议栈中 tcp_diag . tcp_diag 是一个用于分析统计的模块, 用 netfilter 来获取第 Linux 内核中第一手的信息,这就确保了 ss 的快捷高效;如果你的系统中没有 tcp_diag,ss 也可以正常运行,只是效率会变得稍慢.

    netstat 命令是 net-tools 工具集中的一员,而 ss 命令是 iproute 工具集中的一员.
    yum install iproute iproute-doc

#### ss 过滤器

ss 的过滤器分为两种:
     state
         状态:established,syn-sent,syn-recv,fin-wait-1,fin-wait-2,time-wait,closed,close-wait,last-ack,listen,closing
         除了这 13 种状态之外,还有几个聚类的状态:
             all – for all the states
             bucket – 显示状态为 maintained as minisockets,如：time-wait 和 syn-recv
             big – 和 bucket 相反
             connected – 除了 listen and closed 的所有状态
             synchronized – 所有已连接的状态除了 syn-sent
     addr+port
         地址和端口可以使用表达式,类似于 tcpdump 中的用法,关键字有:
             dst ADDRESS_PATTERN – matches remote address and port
             src ADDRESS_PATTERN – matches local address and port
             dport RELOP PORT – compares remote port to a number
             sport RELOP PORT – compares local port to a number
             autobound – checks that socket is bound to an ephemeral port

#### ss usage

ss [ OPTIONS ] [ FILTER ]
        OPTIONS:
            -p 显示每个进程的名字和 pid
            -s 列出当前 socket 详细信息
            -n 不解析服务名称
            -r 解析主机名
            -a 显示所有套接字(sockets)
            -o 显示计时器信息(timer)
            -l 显示监听状态的套接字(sockets)
            -e 显示详细的套接字(sockets)信息
            -m 显示套接字(sockets)的内存使用情况
            -i 显示 TCP 内部信息
            -4 仅显示 IPv4 的套接字(sockets)
            -6 仅显示 IPv6 的套接字(sockets)
            -0 显示 PACKET 套接字(sockets)
            -t 仅显示 TCP 套接字(sockets)
            -u 仅显示 UCP 套接字(sockets)
            -d 仅显示 DCCP 套接字(sockets)
            -w 仅显示 RAW 套接字(sockets)
            -x 仅显示 Unix 套接字(sockets)
            -f --family=FAMILY  显示 FAMILY 类型的套接字(sockets),FAMILY 可选,支持 unix, inet, inet6, link, netlink
            -D --diag=FILE     将原始 TCP 套接字(sockets)信息转储到文件
            -F --filter=FILE   从文件中都去过滤器信息 FILTER := [ state TCP-STATE ] [ EXPRESSION ]

#### Recv And Send

[root@netkiller ~]# ss -anp | column -c1
State      Recv-Q Send-Q        Local Address:Port          Peer Address:Port
LISTEN     0      128               127.0.0.1:9000                     *:*      users:(("php-fpm",1481,9),("php-fpm",1482,0),("php-fpm",1483,0),("php-fpm",1484,0),("php-fpm",1485,0),("php-fpm",1486,0),("php-fpm",1487,0),("php-fpm",1488,0),("php-fpm",1489,0),("php-fpm",1490,0),("php-fpm",1491,0))
LISTEN     0      50                        *:3306                     *:*      users:(("mysqld",2680,11))
LISTEN     0      128                       *:443                      *:*      users:(("nginx",1743,8),("nginx",1744,8),("nginx",1745,8))
LISTEN     0      128              10.1.17.17:2812                     *:*      users:(("monit",2030,6))
TIME-WAIT  0      0                 127.0.0.1:43251            127.0.0.1:80
TIME-WAIT  0      0                 127.0.0.1:43248            127.0.0.1:80
ESTAB      0      0                10.1.17.17:22              10.1.17.18:51752  users:(("sshd",3122,3))
ESTAB      0      0                10.1.17.17:22              10.1.20.70:51531  users:(("sshd",19093,3))

处于 LISTEN 状态的 socket:
    Recv-Q 表示了 current listen backlog 队列元素数目(等待用户调用 accept 的完成 3 次握手的 socket)
    Send-Q 表示了 listen socket 最大能容纳的 backlog.这个数目由 listen 时指定,且不能大于 /proc/sys/net/ipv4/tcp_max_syn_backlog;

对于非 LISTEN socket:
    Recv-Q 表示了 receive queue 中的字节数目(等待接收的下一个 tcp 段的序号-尚未从内核空间 copy 到用户空间的段最前面的一个序号)
    Send-Q 表示发送 queue 中容纳的字节数(已加入发送队列中最后一个序号-输出段中最早一个未确认的序号)

#### Sockets State
>1 Listen

[root@netkiller ~]# ss -lnp | column -c1
State      Recv-Q Send-Q        Local Address:Port          Peer Address:Port
LISTEN     0      128               127.0.0.1:9000                     *:*      users:(("php-fpm",1481,9),("php-fpm",1482,0),("php-fpm",1483,0),("php-fpm",1484,0),("php-fpm",1485,0),("php-fpm",1486,0),("php-fpm",1487,0),("php-fpm",1488,0),("php-fpm",1489,0),("php-fpm",1490,0),("php-fpm",1491,0))
LISTEN     0      50                        *:3306                     *:*      users:(("mysqld",2680,11))
LISTEN     0      50                        *:3307                     *:*      users:(("mysqld",2564,11))

>2 Established

[root@netkiller ~]# ss -onp state established | column -c1
Recv-Q Send-Q             Local Address:Port               Peer Address:Port
0      0                     10.1.17.17:22                   10.1.17.18:51752  timer:(keepalive,70min,0) users:(("sshd",3122,3))
0      0                     10.1.17.17:22                   10.1.20.70:51531  timer:(keepalive,69min,0) users:(("sshd",19093,3))

>3 Sockets Summary

[root@netkiller ~]# ss -s
Total: 93 (kernel 150)
TCP:   106 (estab 10, closed 88, orphaned 0, synrecv 0, timewait 88/0), ports 41

Transport Total     IP        IPv6
*	  150       -         -
RAW	  0         0         0
UDP	  1         1         0
TCP	  18        18        0
INET	  19        19        0
FRAG	  0         0         0

>4 Expand

1 显示所有状态为 established 的 ssh 连接
[root@netkiller ~]# ss -o state established '( dport = :ssh or sport = :ssh )'
Recv-Q Send-Q                                      Local Address:Port                                          Peer Address:Port
0      0                                              10.1.17.17:ssh                                             10.1.17.18:51752    timer:(keepalive,109min,0)
0      0                                              10.1.17.17:ssh                                             10.1.20.70:51531    timer:(keepalive,103min,0)

#### ***timer user mem rto*** 

------在另外一个终端执行 ssh 10.1.2.103-----
然后在本终端执行如下命令
[root@netkiller ~]# ss -eimpn '( dport = :22 )' -o
State      Recv-Q Send-Q                                                          Local Address:Port                                                            Peer Address:Port
ESTAB      0      0                                                                   10.1.2.23:44107                                                             10.1.2.103:22     timer:(keepalive,28min,0) users:(("ssh",9545,4)) ino:21970248 sk:ffff88013c2e5900
	 mem:(r0,w0,f4096,t0) sack cubic wscale:7,8 rto:203 rtt:3.25/1.75 ato:40 cwnd:10 send 35.9Mbps rcv_rtt:33427 rcv_space:113592

------在另外一个终端执行 telnet 27.111.200.86 15672-----
然后在本终端执行如下命令
[root@netkiller ~]# ss -eimpn '( dport = :15672 )' -o
State      Recv-Q Send-Q                                                          Local Address:Port                                                            Peer Address:Port
ESTAB      0      2                                                                   10.1.2.23:57531                                                          27.111.200.86:15672  timer:(on,614ms,0) users:(("telnet",10163,4)) ino:21983807 sk:ffff8800378ba040
	 mem:(r0,w554,f3542,t0) sack cubic wscale:7,8 cwnd:10 rcv_space:14600

> timer

-o 显示计时器信息(timer),linux 对一个 tcp socket 总共有 7 个定时器,通过 4 个 timer 实现
    通过 icsk_retransmit_timer 实现的重传定时器,零窗口探测定时器;
    通过 sk_timer 实现的连接建立定时器,保活定时器和 FIN_WAIT_2 定时器;
    通过 icsk_delack_timer 实现的延时 ack 定时器以及 TIME_WAIT 定时器.

timer 这个输出描述的是 tcp socket 上的定时器
timer 的输出含义就是(类型,过期时间,重试次数)
    off: 当前 socket 没有 timer
    on: 重传 timer
    keepalive：连接建立 timer or fin_wait_2 timer or 保活 timer;具体是那个 timer,可以根据连接的状态来确定.
    timewait: TIME_WAITtimer
    persist：零窗口探测 timer 

> user

ss -p 输出 users 项里会出现三个参数:
    第一个是进程名
    第二个为 pid
    第三项该进程文件描述符的使用数量

> mem

mem:(r0,w554,f3542,t0)
r  the read (inbound) buffer
w  the write (outbound) buffer
f  the "forward allocated memory" (memory available to the socket)
t  the transmit queue (stuff waiting to be sent or waiting on an ACK)

> socket information

sack cubic wscale
rto
rtt
cwnd
send
rcv_space

#### Notice
>1 ss process name and pid

only name

ss -tp | grep -v Recv-Q | sed -e 's/.*users:(("//' -e 's/".*$//' | sort | uniq

only  pid
[root@netkiller ~]# ss -tp | grep -v Recv-Q | sed -e 's/.*users:((.*",//' -e 's/,.*$//'  | sort | uniq

name  and pid
# ss -tp | grep -v Recv-Q | sed -e 's/.*users:(("\(.*\)",\(.*\),.*$/\1:\2/' | sort | uniq
f_e_related_dat:4695
mysqld:4289
salt-minion:4001
sshd:25161

```

## 13. Ubuntu netplan (Ubuntu 18.04 之后才用 netplan 管理网络)

https://netplan.io/examples 参考例子

配置 DHCP

例 13.3. netplan dhcp 例子

```

neo@netkiller ~ % cat /etc/netplan/interfaces.yaml 
network:
  version: 2
  renderer: networkd
  ethernets:
    enp2s1:
      dhcp4: true			

```

启用生效

```

neo@netkiller ~ % sudo netplan apply			

```

## 14. Linux IP And Router

### 14.1. netmask

子网掩码快速算法 大家都应该知道 2 的 x 次方值吧？下面是 2 的 0 次到 10 次方的计算值分别是： 1 2 4 8 16 32 64 128 256 512 1024。 实例 如果你希望每个子网中只有 5 个 ip 地址可以给机器用，那么你就最少需要准备给每个子网 7 个 ip 位址，因为需要加上两头的不可用的网络和广播 ip，所以你需要选比 7 多的最近的那位，也就是 8，就是说选每个子网 8 个 ip。到这一步，你就可以算屏蔽了。 这个方法就是：最后一位屏蔽就是 256 减去你每个子网所需要的 ip 位元址的数量，那么这个例子就是 256-8=248，那么算出这个，你就可以知道那些 ip 是不能用的了， 依此类推：0-7,8-15,16-23,24-31，……，写在上面的 0、7、8、15、16、23、24、31……都是不能用的，你应该用某两个数字之间的 IP，那个就是一个子网可用的 IP。 再试验一下，就拿 200 台机器分成 4 个子网来做例子吧。 200 台机器，4 个子网，那么就是每个子网 50 台机器，设定为 192.168.10.0，C 类的 IP，大子网掩码应为 255.255.255.0，对吧，但是我们要分子网，所以按照上面的，我们用 32 个 IP 一个子网内不够，应该每个子网用 64 个 IP（其中 62 位可用，足够了吧），然后用我的办法：子网掩码应该是 256-64=192，那么总的子网掩码应该为：255.255.255.192。不相信？算算：0-63，64-127，128-191，192-255，这样你就可以把四个区域分别设定到四个子网的机器上了。

#### 14.1.1. iptab

```
# iptab
+----------------------------------------------+
| addrs   bits   pref   class  mask            |
+----------------------------------------------+
|     1      0    /32          255.255.255.255 |
|     2      1    /31          255.255.255.254 |
|     4      2    /30          255.255.255.252 |
|     8      3    /29          255.255.255.248 |
|    16      4    /28          255.255.255.240 |
|    32      5    /27          255.255.255.224 |
|    64      6    /26          255.255.255.192 |
|   128      7    /25          255.255.255.128 |
|   256      8    /24      1C  255.255.255.0   |
|   512      9    /23      2C  255.255.254.0   |
|    1K     10    /22      4C  255.255.252.0   |
|    2K     11    /21      8C  255.255.248.0   |
|    4K     12    /20     16C  255.255.240.0   |
|    8K     13    /19     32C  255.255.224.0   |
|   16K     14    /18     64C  255.255.192.0   |
|   32K     15    /17    128C  255.255.128.0   |
|   64K     16    /16      1B  255.255.0.0     |
|  128K     17    /15      2B  255.254.0.0     |
|  256K     18    /14      4B  255.252.0.0     |
|  512K     19    /13      8B  255.248.0.0     |
|    1M     20    /12     16B  255.240.0.0     |
|    2M     21    /11     32B  255.224.0.0     |
|    4M     22    /10     64B  255.192.0.0     |
|    8M     23     /9    128B  255.128.0.0     |
|   16M     24     /8      1A  255.0.0.0       |
|   32M     25     /7      2A  254.0.0.0       |
|   64M     26     /6      4A  252.0.0.0       |
|  128M     27     /5      8A  248.0.0.0       |
|  256M     28     /4     16A  240.0.0.0       |
|  512M     29     /3     32A  224.0.0.0       |
| 1024M     30     /2     64A  192.0.0.0       |
| 2048M     31     /1    128A  128.0.0.0       |
| 4096M     32     /0    256A  0.0.0.0         |
+----------------------------------------------+

```

#### 14.1.2. netmask - a netmask generation and conversion program

```
$ sudo apt-get install netmask

```

-s, --standard Output address/netmask pairs

```
$ netmask -s 192.168.1.0/28
    192.168.1.0/255.255.255.240

$ netmask -s 192.168.1.0/24
    192.168.1.0/255.255.255.0  

$ netmask -s 192.168.1.0/24
    192.168.1.0/255.255.255.0  

$ netmask -s 192.168.1.0/26
    192.168.1.0/255.255.255.192

[root@netkiller src]# netmask -s  11.111.195.211/27
 11.111.195.192/255.255.255.224

```

-c, --cidr Output CIDR format address lists

```
$ netmask -c 192.168.1.0/255.255.255.252
    192.168.1.0/30

$ netmask -c 192.168.1.0/255.255.255.192
    192.168.1.0/26

$ netmask -c 192.168.1.0/255.255.255.240
    192.168.1.0/28

```

-i, --cisco Output Cisco style address lists 思科风格的反子网掩码计算

```
$ netmask  -i 192.168.1.0/255.255.255.0
    192.168.1.0 0.0.0.255      

$ netmask  -i 192.168.1.0/255.255.255.252
    192.168.1.0 0.0.0.3        

$ netmask  -i 192.168.1.0/24
    192.168.1.0 0.0.0.255      

$ netmask  -i 192.168.1.0/28
    192.168.1.0 0.0.0.15  

```

-r, --range Output ip address ranges 输出地址范围

计算子网掩码位数

```
[root@netkiller src]# netmask  11.111.195.211/255.255.255.224
 11.111.195.192/27			

```

```
$ netmask  -r 192.168.1.0/255.255.255.0
    192.168.1.0-192.168.1.255   (256)

$ netmask  -r 192.168.1.0/255.255.255.192
    192.168.1.0-192.168.1.63    (64)

$ netmask  -r 192.168.1.0/255.255.255.252
    192.168.1.0-192.168.1.3     (4)

$ netmask  -r 192.168.1.0/28
    192.168.1.0-192.168.1.15    (16)

$ netmask  -r 192.168.1.0/24
    192.168.1.0-192.168.1.255   (256)

```

```
$ netmask -r 192.168.1.0/255.255.255.252
    192.168.1.0-192.168.1.3     (4)

$ netmask -r 192.168.1.2/255.255.255.252
    192.168.1.0-192.168.1.3     (4)

$ netmask -r 192.168.1.6/255.255.255.252
    192.168.1.4-192.168.1.7     (4)

$ netmask -r 192.168.1.12/255.255.255.252
   192.168.1.12-192.168.1.15    (4)

$ netmask -r 192.168.1.13/255.255.255.252
   192.168.1.12-192.168.1.15    (4)

$ netmask -r 192.168.1.100/255.255.255.252
  192.168.1.100-192.168.1.103   (4)

$ netmask -r 192.168.1.100/255.255.255.240
   192.168.1.96-192.168.1.111   (16)

$ netmask -r 192.168.1.50/255.255.255.240
   192.168.1.48-192.168.1.63    (16)			

```

-b, --binary Output address/netmask pairs in binary 二进制

```
$ netmask -b 192.168.1.0/255.255.255.240
11000000 10101000 00000001 00000000 / 11111111 11111111 11111111 11110000

$ netmask -b 172.16.0.0/255.255.252.0
10101100 00010000 00000000 00000000 / 11111111 11111111 11111100 00000000

```

### 14.2. arp - manipulate the system ARP cache

#### 14.2.1. display hosts

display (all) hosts in alternative (BSD) style

```
[root@dev2 ~]# arp -a
? (192.168.3.253) at 00:1D:0F:82:05:DC [ether] on eth0
? (192.168.3.48) at 00:25:64:9A:D7:CC [ether] on eth0
? (192.168.3.101) at 00:25:64:A3:65:93 [ether] on eth0
nis.example.com (192.168.3.5) at 00:25:64:9A:D7:E0 [ether] on eth0
? (192.168.3.1) at 00:0F:E2:71:8E:FB [ether] on eth0
? (192.168.3.153) at B8:AC:6F:25:D2:2E [ether] on eth0			

```

display (all) hosts in default (Linux) style

```
[root@dev2 ~]# arp -e
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.3.48             ether   00:25:64:9A:D7:CC   C                     eth0
192.168.3.101            ether   00:25:64:A3:65:93   C                     eth0
nis.example.com          ether   00:25:64:9A:D7:E0   C                     eth0
192.168.3.1              ether   00:0F:E2:71:8E:FB   C                     eth0
10.0.0.1                 ether   00:1F:12:55:A9:02   C                     eth0
192.168.3.153            ether   B8:AC:6F:25:D2:2E   C                     eth0

```

don't resolve names

```
[root@dev2 ~]# arp -a -n
? (192.168.3.253) at 00:1D:0F:82:05:DC [ether] on eth0
? (192.168.3.48) at 00:25:64:9A:D7:CC [ether] on eth0
? (192.168.3.101) at 00:25:64:A3:65:93 [ether] on eth0
? (192.168.3.5) at 00:25:64:9A:D7:E0 [ether] on eth0
? (192.168.3.1) at 00:0F:E2:71:8E:FB [ether] on eth0
? (192.168.3.153) at B8:AC:6F:25:D2:2E [ether] on eth0

```

#### 14.2.2. delete a specified entry

```
[root@dev2 ~]# arp -d 192.168.3.101
[root@dev2 ~]# arp -i eth1 -d 10.0.0.1

```

#### 14.2.3. /proc/net/arp

```
[root@dev2 ~]# cat /proc/net/arp
IP address       HW type     Flags       HW address            Mask     Device
192.168.3.48     0x1         0x2         00:25:64:9A:D7:CC     *        eth0
192.168.3.101    0x1         0x2         00:1E:7A:E0:47:40     *        eth0
192.168.3.5      0x1         0x2         00:25:64:9A:D7:E0     *        eth0
192.168.3.1      0x1         0x2         00:0F:E2:71:8E:FB     *        eth0
192.168.3.153    0x1         0x2         B8:AC:6F:25:D2:2E     *        eth0

```

#### 14.2.4. /etc/ethers

```
# Ethernet-address  IP-number
00:25:64:9A:D7:CC	192.168.3.48

```

read new entries from file or from /etc/ethers

```
# arp -f

```

### 14.3. iproute2

```
add 增加路由
del 删除路由
via 网关出口 IP 地址
dev 网关出口 物理设备名

```

#### 14.3.1. 

```

sudo ip link set eth0 down
sudo ip link set eth0 up			

```

#### 14.3.2. 添加路由

```
ip route add 192.168.0.0/24 via 192.168.0.1
ip route add 192.168.1.1 dev 192.168.0.1			

```

#### 14.3.3. 删除路由

```
ip route del 192.168.0.0/24 via 192.168.0.1			

```

#### 14.3.4. 变更路由

```
[root@router ~]# ip route
192.168.5.0/24 dev eth0  proto kernel  scope link  src 192.168.5.47
192.168.3.0/24 dev eth0  proto kernel  scope link  src 192.168.3.47
default via 192.168.3.1 dev eth0

[root@router ~]# ip route change default via 192.168.5.1 dev eth0

[root@router ~]# ip route list
192.168.5.0/24 dev eth0  proto kernel  scope link  src 192.168.5.47
192.168.3.0/24 dev eth0  proto kernel  scope link  src 192.168.3.47
default via 192.168.5.1 dev eth0

```

#### 14.3.5. 替换已有的路由

```
 ip route replace

```

#### 14.3.6. 增加默认路由

192.168.0.1 是我的默认路由器

```
ip route add default via 192.168.0.1 dev eth0

```

#### 14.3.7. cache

```
ip route flush cache			

```

### 14.4. 策略路由

```

比如我们的 LINUX 有 3 个网卡
eth0: 192.168.1.1　　　（局域网）
eth1: 172.17.1.2　　　 （default gw=172.17.1.1，可以上 INTERNET）
eth2: 192.168.10.2　　 （连接第二路由 192.168.10.1，也可以上 INTERNET）

实现两个目的
1、让 192.168.1.66 从第二路由上网，其他人走默认路由
2、让所有人访问 192.168.1.1 的 FTP 时，转到 192.168.10.96 上

配置方法：
vi /etc/iproute2/rt_tables

#
# reserved values
#
255     local
254     main
253     default
100     ROUTE2

# ip route default via 172.17.1.1 dev eth1
# ip route default via 192.168.10.1 dev eth2 table ROUTE2
# ip rule add from 192.168.1.66 pref 1001 table ROUTE2
# ip rule add to 192.168.10.96 pref 1002 table ROUTE2
# echo 1 >; /proc/sys/net/ipv4/ip_forward
# iptables -t nat -A POSTROUTING -j MASQUERADE
# iptables -t nat -A PREROUTING -d 192.168.1.1 -p tcp --dport 21 -j DNAT --to 192.168.10.96
# ip route flush cache	

```

```

http://phorum.study-area.org/viewtopic.php?t=10085
引用：# 對外網卡 
EXT_IF="eth0" 

# HiNet IP 
EXT_IP1="111.111.111.111" 
EXT_MASK1="24" 
GW1="111.111.111.1" 

# SeedNet IP 
EXT_IP2="222.222.222.222" 
EXT_MASK2="24" 
GW2="222.222.222.1" 

# ?#93;定 ip 
ip addr add $EXT_IP1/$EXT_MASK1 dev $EXT_IF 
ip addr add $EXT_IP2/$EXT_MASK2 dev $EXT_IF 

# ?#93;定 HiNet routing 
ip rule add to $EXT_IP1/$EXT_MASK1 lookup 201 
ip route add default via $GW1 dev $EXT_IF table 201 

# ?#93;定 SeedNet routing 
ip rule add to $EXT_IP2/$EXT_MASK2 lookup 202 
ip route add default via $GW2 dev $EXT_IF table 202 

# ?#93;定 Default route 
ip route replace default equalize \ 
   nexthop via $GW1 dev $EXT_IF \ 
   nexthop via $GW2 dev $EXT_IF 

# 清除 route cache 
ip route flush cache    

它这里的 ip rule 也是这么使用的		

```

### 14.5. 负载均衡

```
ip route add default scope global nexthop dev ppp0 nexthop dev ppp1		

```

```
neo@debian:~$ sudo ip route add default scope global nexthop via 192.168.3.1 dev eth0 weight 1 \
nexthop via 192.168.5.1 dev eth2 weight 1

neo@debian:~$ sudo ip route
192.168.5.0/24 dev eth1  proto kernel  scope link  src 192.168.5.9
192.168.4.0/24 dev eth0  proto kernel  scope link  src 192.168.4.9
192.168.3.0/24 dev eth0  proto kernel  scope link  src 192.168.3.9
172.16.0.0/24 dev eth2  proto kernel  scope link  src 172.16.0.254
default
        nexthop via 192.168.3.1  dev eth0 weight 1
        nexthop via 192.168.5.1  dev eth1 weight 1

```

```
ip route add default scope global nexthop via $P1 dev $IF1 weight 1 \
nexthop via $P2 dev $IF2 weight 1			

```

### 14.6. MASQUERADE

```
iptables–tnat–APOSTROUTING–d192.168.1.0/24–s0/0–oppp0–jMASQUERD
iptables–tnat–APOSTROUTING–s192.168.1.0/24-jSNAT–to202.103.224.58	
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j MASQUERADE 	

```

```
#ip route add via ppp0 dev eth0
#ip route add via 202.103.224.58 dev eth0		

```

### 14.7. ip tunnel

ipip 是 IP 隧道模块

过程 13.1. ip tunnel IP 隧道配置步骤

1.  server 1

    ```
    modprobe ipip
    ip tunnel add mytun mode ipip remote 220.201.35.11 local 211.100.37.167 ttl 255
    ifconfig mytun 10.42.1.1
    route add -net 10.42.1.0/24 dev mytun

    ```

2.  server 2

    ```
    modprobe ipip
    ip tunnel add mytun mode ipip remote 211.100.37.167 local 220.201.35.11 ttl 255
    ifconfig mytun 10.42.1.2
    route add -net 10.42.1.0/24 dev mytun

    ```

3.  nat

    ```
    /sbin/iptables -t nat -A POSTROUTING -s 10.42.1.0/24 -j MASQUERADE
    /sbin/iptables -t nat -A POSTROUTING -s 211.100.37.0/24 -j MASQUERADE

    ```

删除路由表

```
route del -net 10.42.1.0/24 dev mytun

```

修改 IP 隧道的 IP

```
ifconfig mytun 10.10.10.220
route add -net 10.10.10.0/24 dev mytun

```

ip 伪装

```
/sbin/iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -j MASQUERADE

```

### 14.8. VLAN

首先需确保加载了内核模块 802.1q

```
[root@development ~]# lsmod | grep 8021q
[root@development ~]# modprobe 8021q		

```

加载后会生成目录/proc/net/vlan

```
[root@development ~]# cat /proc/net/vlan/config
VLAN Dev name    | VLAN ID
Name-Type: VLAN_NAME_TYPE_RAW_PLUS_VID_NO_PAD

```

### 14.9. Zebra

http://www.zebra.org/

## 第 14 章 Logging 日志

## 1. rsyslog

www.rsyslog.com

目前 rsyslog 已经成为 Linux 标配之日程序，默认会安装，如果没有安装请使用下面命令安装。

```
yum install rsyslog

```

### 1.1. rsyslog.conf

```
$ cat /etc/rsyslog.conf 
#  /etc/rsyslog.conf	Configuration file for rsyslog.
#
#			For more information see
#			/usr/share/doc/rsyslog-doc/html/rsyslog_conf.html
#
#  Default logging rules can be found in /etc/rsyslog.d/50-default.conf

#################
#### MODULES ####
#################

$ModLoad imuxsock # provides support for local system logging
$ModLoad imklog   # provides kernel logging support
#$ModLoad immark  # provides --MARK-- message capability

# provides UDP syslog reception
#$ModLoad imudp
#$UDPServerRun 514

# provides TCP syslog reception
#$ModLoad imtcp
#$InputTCPServerRun 514

# Enable non-kernel facility klog messages
$KLogPermitNonKernelFacility on

###########################
#### GLOBAL DIRECTIVES ####
###########################

#
# Use traditional timestamp format.
# To enable high precision timestamps, comment out the following line.
#
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

# Filter duplicated messages
$RepeatedMsgReduction on

#
# Set the default permissions for all log files.
#
$FileOwner syslog
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
$PrivDropToUser syslog
$PrivDropToGroup syslog

#
# Where to place spool and state files
#
$WorkDirectory /var/spool/rsyslog

#
# Include all config files in /etc/rsyslog.d/
#
$IncludeConfig /etc/rsyslog.d/*.conf			

```

## 2. logrotate - rotates, compresses, and mails system logs

```
logrotate 的配置文件是 /etc/logrotate.conf。主要参数如下表：

参数 					功能
compress 				通过 gzip 压缩转储以后的日志
nocompress 				不需要压缩时，用这个参数
copytruncate			用于还在打开中的日志文件，把当前日志备份并截断
nocopytruncate 			备份日志文件但是不截断
create mode owner group 转储文件，使用指定的文件模式创建新的日志文件
nocreate 				不建立新的日志文件
delaycompress 和 compress 一起使用时，转储的日志文件到下一次转储时才压缩
nodelaycompress 		覆盖 delaycompress 选项，转储同时压缩。
errors address			专储时的错误信息发送到指定的 Email 地址
ifempty 				即使是空文件也转储，这个是 logrotate 的缺省选项。
notifempty 				如果是空文件的话，不转储
mail address 			把转储的日志文件发送到指定的 E-mail 地址
nomail 					转储时不发送日志文件
olddir directory 		转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统
noolddir 				转储后的日志文件和当前日志文件放在同一个目录下
prerotate/endscript 	在转储以前需要执行的命令，这两个关键字必须单独成行
postrotate/endscript 	在转储以后需要执行的命令，这两个关键字必须单独成行
daily 					指定转储周期为每天
weekly 					指定转储周期为每周
monthly 				指定转储周期为每月
rotate count 			指定日志文件删除之前转储的次数，0 指没有备份，5 指保留 5 个备份
tabootext [+] list 		让 logrotate 不转储指定扩展名的文件，缺省的扩展名是：.rpm-orig, .rpmsave, v, 和 ~ 
size size 				当日志文件到达指定的大小时才转储，Size 可以指定 bytes (缺省)以及 KB (sizek)或者 MB (sizem).

```

logrotate 是 linux 系统自带的日志分割与压缩程序，通过 crontab 每日运行一次。

### 2.1. /etc/logrotate.conf

```
$ cat /etc/cron.daily/logrotate
#!/bin/sh

test -x /usr/sbin/logrotate || exit 0
/usr/sbin/logrotate /etc/logrotate.conf

```

```
$ cat /etc/logrotate.conf
# see "man logrotate" for details
# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# uncomment this if you want your log files compressed
#compress

# packages drop log rotation information into this directory
include /etc/logrotate.d

# no packages own wtmp, or btmp -- we'll rotate them here
/var/log/wtmp {
    missingok
    monthly
    create 0664 root utmp
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0660 root utmp
    rotate 1
}

# system-specific logs may be configured here

```

### 2.2. /etc/logrotate.d/

#### 2.2.1. 日志配置

配置多个日志每行写一个条，是用绝对路径

```

/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    postrotate
	/bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}

```

通配符匹配

例如 /var/log/nginx/*.log 匹配所有 nginx 日志

```

/var/log/nginx/*.log {
        daily
        missingok
        rotate 52
        compress
        delaycompress
        notifempty
        create 640 nginx adm
        sharedscripts
        postrotate
                [ -f /var/run/nginx.pid ] && kill -USR1 `cat /var/run/nginx.pid`
        endscript
}

```

```

$ cat /etc/logrotate.d/apache2
/var/log/apache2/*.log {
        weekly
        missingok
        rotate 52
        compress
        delaycompress
        notifempty
        create 640 root adm
        sharedscripts
        postrotate
                if [ -f "`. /etc/apache2/envvars ; echo ${APACHE_PID_FILE:-/var/run/apache2.pid}`" ]; then
                        /etc/init.d/apache2 reload > /dev/null
                fi
        endscript
}

```

#### 2.2.2. create 创建日志文件，指定用于与访问权限

```

$ cat /etc/logrotate.d/mysql-server
# - I put everything in one block and added sharedscripts, so that mysql gets
#   flush-logs'd only once.
#   Else the binary logs would automatically increase by n times every day.
# - The error log is obsolete, messages go to syslog now.
/var/log/mysql.log /var/log/mysql/mysql.log /var/log/mysql/mysql-slow.log {
        daily
        rotate 7
        missingok
        create 640 mysql adm
        compress
        sharedscripts
        postrotate
                test -x /usr/bin/mysqladmin || exit 0
                # If this fails, check debian.conf!
                MYADMIN="/usr/bin/mysqladmin --defaults-file=/etc/mysql/debian.cnf"
                if [ -z "`$MYADMIN ping 2>/dev/null`" ]; then
                  # Really no mysqld or rather a missing debian-sys-maint user?
                  # If this occurs and is not a error please report a bug.
                  #if ps cax | grep -q mysqld; then
                  if killall -q -s0 -umysql mysqld; then
                    exit 1
                  fi
                else
                  $MYADMIN flush-logs
                fi
        endscript
}

```

#### 2.2.3. postrotate

日志切割后运行脚本，通常用于通知父进程，日志已经改变。

```

/var/log/httpd/*log {
    missingok
    notifempty
    sharedscripts
    postrotate
        /sbin/service httpd reload > /dev/null 2>/dev/null || true
    endscript
}

```

```
/var/log/cacti/*.log {
        weekly
        missingok
        rotate 52
        compress
        notifempty
        create 640 www-data www-data
        sharedscripts
}

```

## 3. syslog-ng

syslog-ng 与 rsyslog 功能类似，但是没有成为主流。

## 4. syslog, klogctl - read and/or clear kernel message ring buffer; set console_loglevel

### 注意

很多 2011 年前很多 Linux 发行版使用 syslog，但自 2011 之后，各种 Linux 发行版逐步向 rsyslog 迁移。rsyslog 成为主流。

### 4.1. /etc/sysconfig/syslog

enables logging from remote machines

```
# vim /etc/sysconfig/syslog

#SYSLOGD_OPTIONS="-m 0"
SYSLOGD_OPTIONS="-r -m 0"

```

```
# /etc/init.d/syslog restart
Shutting down kernel logger:                               [  OK  ]
Shutting down system logger:                               [  OK  ]
Starting system logger:                                    [  OK  ]
Starting kernel logger:                                    [  OK  ]

```

### 4.2. /etc/syslog.conf

```
*.*			@172.16.0.9

```

所有日志将被重定向到 172.16.0.9

```
[root@dev1 test]# service syslog restart
Shutting down kernel logger:                               [  OK  ]
Shutting down system logger:                               [  OK  ]
Starting system logger:                                    [  OK  ]
Starting kernel logger:                                    [  OK  ]
[root@dev1 test]#

```

### 4.3. logger

日志的级别

```
emerg 系统已经不可用，级别为紧急
alert 警报，需要立即处理和解决
crit 既将发生，得需要预防。事件就要发生
warnig 警告
err 错误信息，普通的错误信息
notice 提醒信息，很重要的信息
info 通知信息，属于一般信息
debug 这是调试类信息

```

```
#vi /etc/syslog.conf

# Log anything (except mail) of level info or higher.
# Don't log private authentication messages!
*.info;mail.none;authpriv.none;cron.none;local1.none;local3.none /var/log/messages

#my log
local3.* /var/log/my.log

```

```
# service syslog restart
Shutting down kernel logger:                               [  OK  ]
Shutting down system logger:                               [  OK  ]
Starting system logger:                                    [  OK  ]
Starting kernel logger:                                    [  OK  ]

```

```
ping 192.168.0.1 | logger -it logger_test -p local3.notice

```

```
# cat /var/log/my.log
Jan 12 18:06:03 dev1 logger_test[10991]: PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.
Jan 12 18:06:03 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=1 ttl=64 time=0.746 ms
Jan 12 18:06:04 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=2 ttl=64 time=0.713 ms
Jan 12 18:06:05 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=3 ttl=64 time=0.924 ms
Jan 12 18:06:06 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=4 ttl=64 time=0.819 ms
Jan 12 18:06:08 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=5 ttl=64 time=0.667 ms
Jan 12 18:06:09 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=6 ttl=64 time=0.626 ms
Jan 12 18:06:10 dev1 logger_test[10991]: 64 bytes from 192.168.0.1: icmp_seq=7 ttl=64 time=0.665 ms

```

### 4.4. To Log Messages Over UDP Network

## 5. 挂载日志卷

对于有些应用，日志基表庞大，并且需要长期保留日志，这种情况我们通常使用独立卷存储日志。下面的例子我们使用 btrfs 为 tomcat 提供日志卷服务。

### 5.1. 子卷挂载

将 /srv/apache-tomcat/logs 日志目录挂载到 /www/logs 子卷

```

[root@iZ62sreab5qZ ~]#  btrfs subvolume snapshot /www /www/logs
Create a snapshot of '/www' in '/www/logs'

UUID=6b2d5cbf-0b0f-42df-b697-7280671ea847 /srv/apache-tomcat/logs btrfs defaults,subvol=logs 1 1

```

### 5.2. 使用过个子卷

挂载多个子卷

```

[root@iZ62sreab5qZ ~]#  btrfs subvolume snapshot /www /www/logs
Create a snapshot of '/www' in '/www/logs'
[root@iZ62sreab5qZ ~]#  btrfs subvolume snapshot /www /www/logs/admin
Create a snapshot of '/www' in '/www/logs/admin'
[root@iZ62sreab5qZ ~]#  btrfs subvolume snapshot /www /www/logs/m
Create a snapshot of '/www' in '/www/logs/m'
[root@iZ62sreab5qZ ~]#  btrfs subvolume snapshot /www /www/logs/www
Create a snapshot of '/www' in '/www/logs/www'

```

### 5.3. /etc/fstab 配置

```

UUID=9936c1b9-44ea-46b7-ae7c-2486c4859116 /srv/apache-tomcat-www/logs btrfs defaults,subvol=logs/www 1 1
UUID=9936c1b9-44ea-46b7-ae7c-2486c4859116 /srv/apache-tomcat-admin/logs btrfs defaults,subvol=logs/admin 1 1
UUID=9936c1b9-44ea-46b7-ae7c-2486c4859116 /srv/apache-tomcat-m/logs btrfs defaults,subvol=logs/m 1 1

```

## 第 15 章 服务管理

## 1. systemd, init - systemd system and service manager

```

# systemctl stop postfix
# systemctl stop avahi-daemon
# systemctl disable postfix
# systemctl disable avahi-daemon

```

### 1.1. 电源管理

systemd 处理某些电源相关的 ACPI 事件，可以通过从 /etc/systemd/logind.conf 以下选项进行配置

ACPI 事件

1.  HandlePowerKey 按下电源键后的行为，默认 power off
2.  HandleSleepKey 按下挂起键后的行为，默认 suspend
3.  HandleHibernateKey 按下休眠键后的行为，默认 hibernate
4.  HandleLidSwitch 合上笔记本盖后的行为，默认 suspend

触发的行为可以有

1.  ignore（什么都不做）
2.  poweroff（关机）
3.  reboot（重新启动）
4.  halt（关机，和 poweroff 有什么区别，需要手动断开电源？）
5.  suspend（待机挂起）
6.  hibernate（休眠）
7.  hybrid-sleep(同时休眠到内存与硬盘)
8.  lock 锁屏
9.  kexec 调用内核"kexec"函数

如果要合盖不休眠只需要把 HandleLidSwitch 选项设置为如下即可：

去掉 HandleLidSwitch 前面的注释符号#，并把它的值从 suspend 修改为 ignore 或者 lock。

```

[root@localhost ~]# vim /etc/systemd/logind.conf
HandleLidSwitch=lock

```

注意：设置完成保存后运行下列命令才生效。systemctl restart systemd-logind

```

[root@localhost ~]# cat /etc/systemd/logind.conf

#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See logind.conf(5) for details.

[Login]
#NAutoVTs=6
#ReserveVT=6
#KillUserProcesses=no
#KillOnlyUsers=
#KillExcludeUsers=root
#InhibitDelayMaxSec=5
#HandlePowerKey=poweroff
#HandleSuspendKey=suspend
#HandleHibernateKey=hibernate
#HandleLidSwitch=suspend
HandleLidSwitch=ignore
#HandleLidSwitchDocked=ignore
#PowerKeyIgnoreInhibited=no
#SuspendKeyIgnoreInhibited=no
#HibernateKeyIgnoreInhibited=no
#LidSwitchIgnoreInhibited=yes
#IdleAction=ignore
#IdleActionSec=30min
#RuntimeDirectorySize=10%
#RemoveIPC=no
#UserTasksMax=

[root@localhost ~]# systemctl restart systemd-logind			

```

### 1.2. rc.local

```

$ chmod +x /etc/rc.d/rc.local
$ systemctl enable rc-local
$ systemctl start rc-local
$ systemctl status rc-local

```

### 1.3. is-enabled 查看当前服务的启用状态

```

[root@www.netkiller.cn ~]# systemctl is-enabled mongod
enabled
[root@www.netkiller.cn ~]# systemctl is-enabled spring
disabled

```

### 1.4. 重载 systemd

```

systemctl daemon-reload

```

### 1.5. 列出启动失败的服务

```

# systemctl --failed
  UNIT           LOAD   ACTIVE SUB    DESCRIPTION
● spring.service loaded failed failed Spring Boot Application

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

1 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.	

```

### 1.6. list-units

```

# systemctl list-unit-files
UNIT FILE                                   STATE   
proc-sys-fs-binfmt_misc.automount           static  
dev-hugepages.mount                         static  
dev-mqueue.mount                            static  
proc-sys-fs-binfmt_misc.mount               static  
sys-fs-fuse-connections.mount               static  
sys-kernel-config.mount                     static  
sys-kernel-debug.mount                      static  
tmp.mount                                   disabled
brandbot.path                               disabled
systemd-ask-password-console.path           static  
systemd-ask-password-plymouth.path          static  
systemd-ask-password-wall.path              static  
session-1.scope                             static  
session-2.scope                             static  
session-3.scope                             static  
session-4.scope                             static  
auditd.service                              enabled 
autovt@.service                             disabled
avahi-daemon.service                        enabled 
blk-availability.service                    disabled
brandbot.service                            static  
console-getty.service                       disabled
console-shell.service                       disabled
cpupower.service                            disabled
crond.service                               enabled 
dbus-org.fedoraproject.FirewallD1.service   enabled 
dbus-org.freedesktop.Avahi.service          enabled 
dbus-org.freedesktop.hostname1.service      static  
dbus-org.freedesktop.locale1.service        static  
dbus-org.freedesktop.login1.service         static  
dbus-org.freedesktop.machine1.service       static  
dbus-org.freedesktop.NetworkManager.service enabled 
dbus-org.freedesktop.nm-dispatcher.service  enabled 
dbus-org.freedesktop.timedate1.service      static  
dbus.service                                static  
debug-shell.service                         disabled
dm-event.service                            disabled
dnsmasq.service                             disabled
dracut-cmdline.service                      static  
dracut-initqueue.service                    static  
dracut-mount.service                        static  
dracut-pre-mount.service                    static  
dracut-pre-pivot.service                    static  
dracut-pre-trigger.service                  static  
dracut-pre-udev.service                     static  
dracut-shutdown.service                     static  
ebtables.service                            disabled
emergency.service                           static  
firewalld.service                           enabled 
getty@.service                              enabled 
halt-local.service                          static  
initrd-cleanup.service                      static  
initrd-parse-etc.service                    static  
initrd-switch-root.service                  static  
initrd-udevadm-cleanup-db.service           static  
irqbalance.service                          enabled 
kdump.service                               enabled 
kmod-static-nodes.service                   static  
lvm2-lvmetad.service                        disabled
lvm2-monitor.service                        enabled 
lvm2-pvscan@.service                        static  
messagebus.service                          static  
microcode.service                           enabled 
NetworkManager-dispatcher.service           enabled 
NetworkManager-wait-online.service          disabled
NetworkManager.service                      enabled 
plymouth-halt.service                       disabled
plymouth-kexec.service                      disabled
plymouth-poweroff.service                   disabled
plymouth-quit-wait.service                  disabled
plymouth-quit.service                       disabled
plymouth-read-write.service                 disabled
plymouth-reboot.service                     disabled
plymouth-start.service                      disabled
plymouth-switch-root.service                static  
polkit.service                              static  
postfix.service                             enabled 
quotaon.service                             static  
rc-local.service                            static  
rdisc.service                               disabled
rescue.service                              static  
rhel-autorelabel-mark.service               static  
rhel-autorelabel.service                    static  
rhel-configure.service                      static  
rhel-dmesg.service                          disabled
rhel-domainname.service                     disabled
rhel-import-state.service                   static  
rhel-loadmodules.service                    static  
rhel-readonly.service                       static  
rsyslog.service                             enabled 
serial-getty@.service                       disabled
sshd-keygen.service                         static  
sshd.service                                enabled 
sshd@.service                               static  
systemd-ask-password-console.service        static  
systemd-ask-password-plymouth.service       static  
systemd-ask-password-wall.service           static  
systemd-backlight@.service                  static  
systemd-binfmt.service                      static  
systemd-fsck-root.service                   static  
systemd-fsck@.service                       static  
systemd-halt.service                        static  
systemd-hibernate.service                   static  
systemd-hostnamed.service                   static  
systemd-hybrid-sleep.service                static  
systemd-initctl.service                     static  
systemd-journal-flush.service               static  
systemd-journald.service                    static  
systemd-kexec.service                       static  
systemd-localed.service                     static  
systemd-logind.service                      static  
systemd-machined.service                    static  
systemd-modules-load.service                static  
systemd-nspawn@.service                     disabled
systemd-poweroff.service                    static  
systemd-quotacheck.service                  static  
systemd-random-seed.service                 static  
systemd-readahead-collect.service           enabled 
systemd-readahead-done.service              static  
systemd-readahead-drop.service              enabled 
systemd-readahead-replay.service            enabled 
systemd-reboot.service                      static  
systemd-remount-fs.service                  static  
systemd-shutdownd.service                   static  
systemd-suspend.service                     static  
systemd-sysctl.service                      static  
systemd-timedated.service                   static  
systemd-tmpfiles-clean.service              static  
systemd-tmpfiles-setup-dev.service          static  
systemd-tmpfiles-setup.service              static  
systemd-udev-settle.service                 static  
systemd-udev-trigger.service                static  
systemd-udevd.service                       static  
systemd-update-utmp-runlevel.service        static  
systemd-update-utmp.service                 static  
systemd-user-sessions.service               static  
systemd-vconsole-setup.service              static  
teamd@.service                              static  
tuned.service                               enabled 
wpa_supplicant.service                      disabled
-.slice                                     static  
machine.slice                               static  
system.slice                                static  
user.slice                                  static  
avahi-daemon.socket                         enabled 
dbus.socket                                 static  
dm-event.socket                             enabled 
lvm2-lvmetad.socket                         enabled 
sshd.socket                                 disabled
syslog.socket                               static  
systemd-initctl.socket                      static  
systemd-journald.socket                     static  
systemd-shutdownd.socket                    static  
systemd-udevd-control.socket                static  
systemd-udevd-kernel.socket                 static  
basic.target                                static  
bluetooth.target                            static  
cryptsetup.target                           static  
ctrl-alt-del.target                         disabled
default.target                              enabled 
emergency.target                            static  
final.target                                static  
getty.target                                static  
graphical.target                            disabled
halt.target                                 disabled
hibernate.target                            static  
hybrid-sleep.target                         static  
initrd-fs.target                            static  
initrd-root-fs.target                       static  
initrd-switch-root.target                   static  
initrd.target                               static  
kexec.target                                disabled
local-fs-pre.target                         static  
local-fs.target                             static  
multi-user.target                           enabled 
network-online.target                       static  
network.target                              static  
nss-lookup.target                           static  
nss-user-lookup.target                      static  
paths.target                                static  
poweroff.target                             disabled
printer.target                              static  
reboot.target                               disabled
remote-fs-pre.target                        static  
remote-fs.target                            enabled 
rescue.target                               disabled
rpcbind.target                              static  
runlevel0.target                            disabled
runlevel1.target                            disabled
runlevel2.target                            disabled
runlevel3.target                            disabled
runlevel4.target                            disabled
runlevel5.target                            disabled
runlevel6.target                            disabled
shutdown.target                             static  
sigpwr.target                               static  
sleep.target                                static  
slices.target                               static  
smartcard.target                            static  
sockets.target                              static  
sound.target                                static  
suspend.target                              static  
swap.target                                 static  
sysinit.target                              static  
system-update.target                        static  
time-sync.target                            static  
timers.target                               static  
umount.target                               static  
systemd-readahead-done.timer                static  
systemd-tmpfiles-clean.timer                static  

210 unit files listed.	

```

```

$ systemctl list-units --type=target
UNIT                  LOAD   ACTIVE SUB    DESCRIPTION
basic.target          loaded active active Basic System
cryptsetup.target     loaded active active Encrypted Volumes
getty.target          loaded active active Login Prompts
local-fs-pre.target   loaded active active Local File Systems (Pre)
local-fs.target       loaded active active Local File Systems
multi-user.target     loaded active active Multi-User System
network-online.target loaded active active Network is Online
network.target        loaded active active Network
paths.target          loaded active active Paths
slices.target         loaded active active Slices
sockets.target        loaded active active Sockets
swap.target           loaded active active Swap
sysinit.target        loaded active active System Initialization
timers.target         loaded active active Timers

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

14 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.

```

```

$ systemctl list-units | more
UNIT                                             LOAD   ACTIVE SUB       DESCRIPTION
proc-sys-fs-binfmt_misc.automount                loaded active running   Arbitrary Executable File Formats File System Automount Point
sys-devices-platform-serial8250-tty-ttyS0.device loaded active plugged   /sys/devices/platform/serial8250/tty/ttyS0
sys-devices-platform-serial8250-tty-ttyS1.device loaded active plugged   /sys/devices/platform/serial8250/tty/ttyS1
sys-devices-platform-serial8250-tty-ttyS2.device loaded active plugged   /sys/devices/platform/serial8250/tty/ttyS2
sys-devices-platform-serial8250-tty-ttyS3.device loaded active plugged   /sys/devices/platform/serial8250/tty/ttyS3
sys-devices-vbd\x2d51728-block-xvdb-xvdb1.device loaded active plugged   /sys/devices/vbd-51728/block/xvdb/xvdb1
sys-devices-vbd\x2d51728-block-xvdb.device       loaded active plugged   /sys/devices/vbd-51728/block/xvdb
sys-devices-vbd\x2d768-block-xvda-xvda1.device   loaded active plugged   /sys/devices/vbd-768/block/xvda/xvda1
sys-devices-vbd\x2d768-block-xvda.device         loaded active plugged   /sys/devices/vbd-768/block/xvda
sys-devices-vif\x2d0-net-eth0.device             loaded active plugged   /sys/devices/vif-0/net/eth0
sys-devices-vif\x2d1-net-eth1.device             loaded active plugged   /sys/devices/vif-1/net/eth1
sys-devices-virtual-net-tun0.device              loaded active plugged   /sys/devices/virtual/net/tun0
sys-module-configfs.device                       loaded active plugged   /sys/module/configfs
sys-subsystem-net-devices-eth0.device            loaded active plugged   /sys/subsystem/net/devices/eth0
sys-subsystem-net-devices-eth1.device            loaded active plugged   /sys/subsystem/net/devices/eth1
sys-subsystem-net-devices-tun0.device            loaded active plugged   /sys/subsystem/net/devices/tun0
-.mount                                          loaded active mounted   /
dev-hugepages.mount                              loaded active mounted   Huge Pages File System
dev-mqueue.mount                                 loaded active mounted   POSIX Message Queue File System
opt.mount                                        loaded active mounted   /opt
proc-sys-fs-binfmt_misc.mount                    loaded active mounted   Arbitrary Executable File Formats File System
proc-xen.mount                                   loaded active mounted   /proc/xen
run-user-0.mount                                 loaded active mounted   /run/user/0
sys-kernel-config.mount                          loaded active mounted   Configuration File System
sys-kernel-debug.mount                           loaded active mounted   Debug File System
brandbot.path                                    loaded active waiting   Flexible branding
systemd-ask-password-plymouth.path               loaded active waiting   Forward Password Requests to Plymouth Directory Watch
systemd-ask-password-wall.path                   loaded active waiting   Forward Password Requests to Wall Directory Watch
session-231.scope                                loaded active running   Session 231 of user root
session-571.scope                                loaded active running   Session 571 of user root
aegis.service                                    loaded active running   LSB: aegis update.
agentwatch.service                               loaded active running   SYSV: Starts and stops guest agent
cloudmonitor.service                             loaded active running   LSB: @app.long.name@
crond.service                                    loaded active running   Command Scheduler
dbus.service                                     loaded active running   D-Bus System Message Bus
exim.service                                     loaded active running   Exim Mail Transport Agent
getty@tty1.service                               loaded active running   Getty on tty1
gitlab-runsvdir.service                          loaded active running   GitLab Runit supervision process
iptables.service                                 loaded active exited    IPv4 firewall with iptables
jexec.service                                    loaded active exited    LSB: Supports the direct execution of binary formats.
kmod-static-nodes.service                        loaded active exited    Create list of required static device nodes for the current kernel
lvm2-lvmetad.service                             loaded active running   LVM2 metadata daemon
lvm2-monitor.service                             loaded active exited    Monitoring of LVM2 mirrors, snapshots etc. using dmeventd or progress polling
mysqld.service                                   loaded active running   MySQL Server
network.service                                  loaded active exited    LSB: Bring up/down networking
nscd.service                                     loaded active running   Name Service Cache Daemon
ntpd.service                                     loaded active running   Network Time Service
openvpn@server.service                           loaded active running   OpenVPN Robust And Highly Flexible Tunneling Application On server
rhel-dmesg.service                               loaded active exited    Dump dmesg to /var/log/dmesg
rhel-import-state.service                        loaded active exited    Import network configuration from initramfs
rhel-readonly.service                            loaded active exited    Configure read-only root support
rsyslog.service                                  loaded active running   System Logging Service
--More--

```

## 2. 定时器单元

```

neo@netkiller ~ % systemctl list-timers
NEXT                         LEFT          LAST                         PASSED       UNIT                         
Wed 2018-11-14 20:43:46 HST  4min 47s left Wed 2018-11-14 16:31:32 HST  4h 7min ago  motd-news.timer              
Wed 2018-11-14 22:07:11 HST  1h 28min left Wed 2018-11-14 15:04:00 HST  5h 34min ago apt-daily.timer              
Thu 2018-11-15 00:00:00 HST  3h 21min left Wed 2018-11-14 17:33:09 HST  3h 5min ago  logrotate.timer              
Thu 2018-11-15 06:19:21 HST  9h left       Wed 2018-11-14 15:04:00 HST  5h 34min ago apt-daily-upgrade.timer      
Thu 2018-11-15 20:01:56 HST  23h left      Wed 2018-11-14 20:01:56 HST  37min ago    systemd-tmpfiles-clean.timer 
Mon 2018-11-19 00:00:00 HST  4 days left   Mon 2018-11-12 01:31:25 HST  2 days ago   fstrim.timer                 
n/a                          n/a           Wed 2018-11-14 19:49:46 HST  49min ago    ureadahead-stop.timer        

7 timers listed.
Pass --all to see loaded but inactive timers, too.
lines 1-11/11 (END)

```

## 3. Debian/Ubuntu

### 3.1. update-rc.d - install and remove System-V style init script links

for example:

```
Insert links using the defaults:
   update-rc.d foobar defaults
Equivalent command using explicit argument sets:
   update-rc.d foobar start 20 2 3 4 5 . stop 20 0 1 6 .
More typical command using explicit argument sets:
   update-rc.d foobar start 30 2 3 4 5 . stop 70 0 1 6 .
Insert links at default runlevels when B requires A
   update-rc.d script_for_A defaults 80 20
   update-rc.d script_for_B defaults 90 10
Insert a link to a service that (presumably) will not be needed by any other daemon
   update-rc.d top_level_app defaults 98 02
Insert links for a script that requires services that start/stop at sequence number 20
   update-rc.d script_depends_on_svc20 defaults 21 19
Remove all links for a script (assuming foobar has been deleted already):
   update-rc.d foobar remove
Example of disabling a service:
   update-rc.d -f foobar remove
   update-rc.d foobar stop 20 2 3 4 5 .
Example of a command for installing a system initialization-and-shutdown script:
   update-rc.d foobar start 45 S . stop 31 0 6 .
Example of a command for disabling a system initialization-and-shutdown script:
   update-rc.d -f foobar remove
   update-rc.d foobar stop 45 S .

```

set default

```
update-rc.d nginx defaults

```

remove

```
update-rc.d -f lighttpd remove
$ sudo update-rc.d -f avahi-daemon remove

```

### 3.2. invoke-rc.d - executes System-V style init script actions

```
$ sudo invoke-rc.d mysql restart

```

### 3.3. runlevel

```
$ runlevel
N 2

# runlevel
N 3

```

```
$ sudo vim /etc/init.d/rcS
#! /bin/sh
#
# rcS
#
# Call all S??* scripts in /etc/rcS.d/ in numerical/alphabetical order
#

exec /etc/init.d/rc S

```

the default is S (/etc/rcS.d/)

the redhat linux in the /etc/inittab

switch runlevel

```
/etc/init.d/rc 3

```

### 3.4. sysv-rc-conf

(ubuntu 下 sysv-rc-conf 命令等同 redhat 下 chkconfig 命令)

```
$ sudo apt-get install sysv-rc-conf

```

进入 sysv-rc-conf TUI 用户界面，你可以使用键盘方向键切换，使用空格键选择“X”表示选中，这个软件也支持鼠标操作。

```
$ sudo sysv-rc-conf

```

```
sysv-rc-conf gmond on
sysv-rc-conf --list gmond

```

### 3.5. xinetd - replacement for inetd with many enhancements

```
$ sudo apt-get install xinetd

```

#### 3.5.1. tftpd

```
apt-get install xinetd
apt-get install tftpd tftp

```

/etc/xinetd.d/tftp

```
service tftp
{
	disable=no
	socket_type=dgram
	protocol =udp
	wait=yes
	user=root
	server=/usr/sbin/in.tftpd
	server_args =-s /home/neo/tftpboot -c
	per_source=11
	cps=100 2
	flags=IPv4
}

```

### 3.6. Scheduled Tasks

#### 3.6.1. crontab - maintain crontab files for individual users

To see what crontabs are currently running on your system, you can open a terminal and run:

```
$  crontab -l
# m h  dom mon dow   command
#* */30 * * * /home/neo/dyndns

```

if you want to see root user, please add 'sudo' in the prefix.

To edit the list of cron jobs you can run:

```
$ crontab -e

```

As you can see there are 5 stars. The stars represent different date parts in the following order:

1.  minute (from 0 to 59)

2.  hour (from 0 to 23)

3.  day of month (from 1 to 31)

4.  month (from 1 to 12)

5.  day of week (from 0 to 6) (0=Sunday)

By default cron jobs sends a email to the user account executing the cronjob. If this is not needed put the following command At the end of the cron job line .

```

>/dev/null 2>&1

```

#### 3.6.2. at, batch, atq, atrm - queue, examine or delete jobs for later execution

### 3.7. sv - control and manage services monitored by runsv

services directory /etc/service/

```
$ sudo sv start git-daemon
ok: run: git-daemon: (pid 10323) 1s

$ sudo sv restart git-daemon
ok: run: git-daemon: (pid 10327) 1s

$ sudo sv stop git-daemon
ok: down: git-daemon: 1s, normally up

```

#### 3.7.1. runsv

```
$ sudo runsv git-daemon

```

#### 3.7.2. runsvdir

运行/etc/service 目录下的所有服务

```

$sudo runsvdir /etc/service &

```

## 4. CentOS 6

### 4.1. service

```
# service nginx
Usage: nginx {start|stop|restart|condrestart|try-restart|force-reload|upgrade|reload|status|help|configtest}

# service nginx stop
# service nginx start
# service nginx restart

```

```
[ ] NetworkManager   自动在多种网络连接中进行转换，如果你的电脑有 Wireless WiFi 和 Ethernet 多种网络连接类型的话，可以选择开启。
[ ] acpid            （Advanced Configuration and Power Interface）是为替代传统的 APM 电源管理标准而推出的新型电源管理标准。通常笔记本电脑需要启动电源进行管理。
[*] anacron          自动化运行任务守护进程
[*] atd              自动化运行任务守护进程
[ ] auditd           审核信息，将消息写入控制台以及 audit_warn 电子邮件别名。用于存放内核生成的系统审查记录，这些记录会被一些程序使用。特别是对于 SELinux 用户来说。
[ ] autofs           自动挂载/卸载文件系统服务，可以自动挂载想访问但还未挂载的文件系统,自动卸载长期不访问的文件系统,自动安装管理进程 automount，与 NFS 相关，依赖于 NIS
[ ] avahi-daemon     Zeroconf service discovery 守护进程，Avahi 是 zeroconf 协议的实现。它可以在没有 DNS 服务的局域网里发现基于 zeroconf 协议的设备和服务。它跟 mDNS 一样。除非你有兼容的设备或使用 zeroconf 协议的服务，否则就可以关闭。
[ ] avahi-dnsconfd   /etc/avahi/dnsconf.action 脚本守护进程
[ ] bluetooth        蓝牙
[ ] conman           控制台管理
[ ] cpuspeed         监测系统空闲百分比，降低或加快 CPU 时钟速度和电压
[*] crond            一个传统的 UNIX 程序 crontab，可以周期地运行用户调度的任务。
[ ] cups             通用 UNIX 打印守护进程，（Common UNIX Printing System）公共 UNIX 打印支持，为 Linux 提供打印功能。 安装打印机时需要的服务。
[ ] dnsmasq          Dns cache server 守护进程
[ ] dund             蓝牙拨号网络
[ ] firstboot        安装完之后的用户配置向导，用于第一次设置系统
[ ] gpm              为文本模式下的 Linux 程序提供鼠标支持、拷贝、粘贴操作、弹出式菜单
[ ] haldaemon        硬件监控系统
[ ] hidd             蓝牙 H.I.D.服务器
[ ] httpd            Apache 服务器
[ ] ip6tables        防火墙守护进程
[*] iptables         防火墙守护进程
[ ] irda             红外端口守护进程
[*] irqbalance       多系统处理器环境下的系统中断请求进行负载平衡，单 CPU 无用
[ ] kudzu            硬件自动检测程序，如不增加新硬件，可以关闭
[ ] lvm2-monitor     LVM2 mirror devices 守护进程
[ ] mcstrans         SELinux Context Translation System Daemon
[ ] mdmonitor        RAID 相关设备的守护程序
[ ] mdmpd            RAID 相关设备的守护程序
[*] messagebus       事件监控服务，在必要时向所有用户发送广播信息
[ ] microcode_ctl    可编码以及发送新微代码到内核以更新 Intel IA32 系列处理器守护进程
[ ] multipathd       Manage device-mapper multipath devices
[ ] netconsole       Initializes network console logging
[ ] netfs            安装和卸载 NFS、SAMBA 和 NCP 网络文件系统
[ ] netplugd         服务监控网络界面,根据信号关闭或启动它,用于手提电脑
[*] network          激活已配置网络接口的脚本程序
[ ] nfs              网络文件系统守护进程
[ ] nfslock          NFS 文件锁定功能
[ ] nscd             密码与群查找服务
[ ] ntpd             网络时间同步
[ ] oddjobd
[ ] pand             蓝牙个人区域网络
[ ] pcscd            智能卡支持
[ ] portmap          用来支持 RPC 连接，RPC 被用于 NFS 以及 NIS 等服务
[ ] psacct           进程审计守护进程
[ ] rawdevices		 rawdevices	to block devices。Oracle 数据库使用
[ ] rdisc            discovers routers 守护进程
[ ] readahead_early  开机内存载入优化
[ ] readahead_later  开机内存载入优化
[ ] restorecond      SELinux 相关联
[ ] rpcgssd          manages RPCSEC GSS contexts for the NFSv4 server
[ ] rpcidmapd        rpcidmapd for NFSv4 that maps user names to UID and GID nu
[ ] rpcsvcgssd       rpcsvcgssd manages RPCSEC GSS contexts for the NFSv4 server
[ ] saslauthd        使用 SASL 的认证守护进程
[*] sendmail         邮件服务器 sendmail 守护进程
[*] smartd           监控硬盘故障
[*] sshd             OpenSSH 服务器守护进程
[*] syslog           系统日志
[ ] winbind          用于 Samba 服务器
[ ] wpa_supplicant   无线设备支持
[ ] xfs              X Window 字型服务器守护进程，为本地和远程 X 服务器提供字型集
[ ] ypbind           为 NIS 客户机激活 ypbind 服务进程
[ ] yum-updatesd	 RPM 操作系统自动升级和软件包管理守护进程

```

#### 4.1.1. chkconfig

```
chkconfig acpid off

```

```
[root@development ~]# chkconfig --add mysqld 		[在服务清单中添加 mysql 服务]
[root@development ~]# chkconfig mysqld on			[设置 mysql 服务开机启动]
[root@development ~]# chkconfig --list mysqld		[设置 mysql 启动级别]
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off

```

```
chkconfig --level 3 mysqld on
chkconfig --level 3 mysqld off

```

### 4.2. xinetd.d

```
# yum -y install xinetd

```

#### 4.2.1. tftpd

```
# yum install -y tftp-server tftp

```

/etc/xinetd.d/tftp

```
# vim /etc/xinetd.d/tftp
# default: off
# description: The tftp server serves files using the trivial file transfer \
#       protocol.  The tftp protocol is often used to boot diskless \
#       workstations, download configuration files to network-aware printers, \
#       and to start the installation process for some operating systems.
service tftp
{
        socket_type             = dgram
        protocol                = udp
        wait                    = yes
        user                    = root
        server                  = /usr/sbin/in.tftpd
        server_args             = -s /tftpboot
        disable                 = yes
        per_source              = 11
        cps                     = 100 2
        flags                   = IPv4
}

```

disable = yes 改为 disable = no

```
mkdir /tftpboot
/etc/init.d/xinetd restart

```

##### 4.2.1.1. atftp-server

```
# yum install -y atftp-server atftp

```

/etc/xinetd.d/tftp

```
# cat /etc/xinetd.d/tftp
# default: off
# description: The tftp server serves files using the trivial file transfer protocol. The tftp protocol is often used to boot diskless workstations, download configuration files to network-aware printers, and to start the installation process for some operating systems.
service tftp
{
    disable         = no
    socket_type     = dgram
    protocol        = udp
    wait            = yes
    user            = root
    server          = /usr/sbin/in.tftpd
    server_args     = /tftpboot
    per_source      = 11
    cps             = 100 2
    flags           = IPv4
}

```

atftp-server 是一个可以不依赖 xinetd 的 tftp 服务器

#### 4.2.2. rsync

```
# vim /etc/xinetd.d/rsync
# default: off
# description: The rsync server is a good addition to an ftp server, as it \
#       allows crc checksumming etc.
service rsync
{
        disable = no
        socket_type     = stream
        wait            = no
        user            = root
        server          = /usr/bin/rsync
        server_args     = --daemon
        log_on_failure  += USERID
}

```

#### 4.2.3. rshd

/etc/xinetd.d/rsh

```
# cat  /etc/xinetd.d/rsh
# default: on
# description: The rshd server is the server for the rcmd(3) routine and, \
#	consequently, for the rsh(1) program.  The server provides \
#	remote execution facilities with authentication based on \
#	privileged port numbers from trusted hosts.
service shell
{
	socket_type		= stream
	wait			= no
	user			= root
	log_on_success		+= USERID
	log_on_failure 		+= USERID
	server			= /usr/sbin/in.rshd
	disable			= no
}

```

访问权限配置

```
# cat /etc/hosts.allow
#
# hosts.allow	This file describes the names of the hosts which are
#		allowed to use the local INET services, as decided
#		by the '/usr/sbin/tcpd' server.
#
in.rshd : your.example.com 192.168.0.1

```

```
# cat /etc/hosts.deny
#
# hosts.deny	This file describes the names of the hosts which are
#		*not* allowed to use the local INET services, as decided
#		by the '/usr/sbin/tcpd' server.
#
# The portmap line is redundant, but it is left to remind you that
# the new secure portmap uses hosts.deny and hosts.allow.  In particular
# you should know that NFS uses portmap!
all : all

```

访问主机设置

```
# cat ~/.rhosts
your.example.com user
192.168.0.1	user

```

### 4.3. rpcinfo

```
# rpcinfo -p 192.168.187.75
   program vers proto   port
    100000    2   tcp    111  portmapper
    100000    2   udp    111  portmapper
    100024    1   udp    697  status
    100024    1   tcp    700  status
    100011    1   udp    864  rquotad
    100011    2   udp    864  rquotad
    100011    1   tcp    867  rquotad
    100011    2   tcp    867  rquotad
    100003    2   udp   2049  nfs
    100003    3   udp   2049  nfs
    100003    4   udp   2049  nfs
    100003    2   tcp   2049  nfs
    100003    3   tcp   2049  nfs
    100003    4   tcp   2049  nfs
    100021    1   udp  32778  nlockmgr
    100021    3   udp  32778  nlockmgr
    100021    4   udp  32778  nlockmgr
    100021    1   tcp  35837  nlockmgr
    100021    3   tcp  35837  nlockmgr
    100021    4   tcp  35837  nlockmgr
    100005    1   udp    880  mountd
    100005    1   tcp    883  mountd
    100005    2   udp    880  mountd
    100005    2   tcp    883  mountd
    100005    3   udp    880  mountd
    100005    3   tcp    883  mountd

```

### 4.4. SELINUX

禁用 SElinux 编辑/etc/selinux/config，修改如下内容：

```
SELINUX=disabled

```

使用命令

```
getenforce
setenforce 0

```

```
lokkit --selinux=disabled

```

## 第 16 章 Process 进程管理

## 1. top - display Linux tasks

top 命令算是最直观、好用的查看服务器负载的命令了。它实时动态刷新显示服务器状态信息，且可以通过交互式命令自定义显示内容，非常强大。

```

> 进程信息

PID：进程的 ID 
USER：进程所有者 
PR：进程的优先级别，越小越优先被执行 
NInice：值 
VIRT：进程占用的虚拟内存 
RES：进程占用的物理内存 
SHR：进程使用的共享内存 
S：进程的状态。S 表示休眠，R 表示正在运行，Z 表示僵死状态，N 表示该进程优先值为负数 
%CPU：进程占用 CPU 的使用率 
%MEM：进程使用的物理内存和总内存的百分比 
TIME+：该进程启动后占用的总的 CPU 时间，即占用 CPU 使用时间的累加值。 
COMMAND：进程启动命令名称

### top 交互

s：设置刷新时间间隔
c：显示命令完全模式
t:：显示或隐藏进程和 CPU 状态信息
m：显示或隐藏内存状态信息
l：显示或隐藏 uptime 信息
f：增加或减少进程显示标志
S：累计模式，会把已完成或退出的子进程占用的 CPU 时间累计到父进程的 MITE+
P：按%CPU 使用率排行
T：按 MITE+排行
M：按%MEM 排行
u：指定显示用户进程
r：修改进程 renice 值
kkill：进程
i：只显示正在运行的进程
W：保存对 top 的设置到文件~/.toprc，下次启动将自动调用 toprc 文件的设置。
h：帮助命令。
q：退出

如果想看每一个 cpu 的处理情况，按 1 即可；折叠，再次按 1

按键 b 打开或关闭 运行中进程的高亮效果

按键 x 打开或关闭 排序列的高亮效果

shift + > 或 shift + < 可以向右或左改变排序列

f 键，可以进入编辑要显示字段的视图，有 号的字段会显示，无 号不显示，可根据页面提示选择或取消字段。		

```

```

$ top
top - 22:30:02 up 14:24,  1 user,  load average: 0.17, 0.15, 0.10
Tasks: 240 total,   2 running, 238 sleeping,   0 stopped,   0 zombie
Cpu0  :  2.0%us,  4.1%sy,  0.0%ni, 92.9%id,  1.0%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu1  :  1.5%us,  3.7%sy,  0.1%ni, 94.6%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu2  :  2.2%us,  5.6%sy,  0.0%ni, 92.2%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu3  :  2.1%us,  6.3%sy,  0.0%ni, 91.6%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   2048012k total,  1138504k used,   909508k free,   139292k buffers
Swap:  1951856k total,        0k used,  1951856k free,   603728k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
 4686 neo       20   0 19264 1440  980 R   11  0.1   0:00.10 top
 4698 neo       20   0  9440 1572 1044 S   11  0.1   0:00.27 sitemaps
    6 root      RT  -5     0    0    0 S    4  0.0   0:14.38 migration/1
    1 root      20   0 19320 1600 1132 S    0  0.1   0:01.50 init
    2 root      15  -5     0    0    0 S    0  0.0   0:00.00 kthreadd
    3 root      RT  -5     0    0    0 S    0  0.0   0:10.41 migration/0

```

### 1.1. 查找内存消耗最大的进程

```

[root@localhost ~]# top -c -b -o +%MEM | head -n 20 | tail -15

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 1457 root      20   0  661572  66732  14552 R  62.5  3.5   1:32.09 /usr/bin/python2 /usr/bin/dnf upgrade
  531 root      20   0  358748  29468   7036 S   0.0  1.5   0:00.91 /usr/bin/python2 -Es /usr/sbin/firewa+
 1042 root      20   0  574200  19552   6112 S   0.0  1.0   0:00.34 /usr/bin/python2 -Es /usr/sbin/tuned +
  491 polkitd   20   0  613016  11916   4896 S   0.0  0.6   0:00.04 /usr/lib/polkit-1/polkitd --no-debug
 1046 root      20   0  424064  11420   9052 S   0.0  0.6   0:00.09 /usr/sbin/smbd --foreground --no-proc+
  542 root      20   0  701996   9568   7052 S   0.0  0.5   0:00.41 /usr/sbin/NetworkManager --no-daemon
 1215 root      20   0  158924   5668   4336 S   0.0  0.3   0:00.06 sshd: www [priv]
 1542 root      20   0  158924   5668   4336 S   0.0  0.3   0:00.02 sshd: www [priv]
  755 root      20   0  102896   5492   3428 S   0.0  0.3   0:00.01 /sbin/dhclient -d -q -sf /usr/libexec+
 1045 root      20   0  216420   4744   3308 S   0.0  0.2   0:00.33 /usr/sbin/rsyslogd -n
  654 root      20   0   78812   4636   3640 S   0.0  0.2   0:00.04 /usr/sbin/wpa_supplicant -u -f /var/l+
 1044 root      20   0  112920   4292   3268 S   0.0  0.2   0:00.00 /usr/sbin/sshd -D
 1150 postfix   20   0   90348   4240   3160 S   0.0  0.2   0:00.00 qmgr -l -t unix -u		

```

仅显示命令,不显示参数

```

[root@localhost ~]# top -b -o +%MEM | head -n 20 | tail -15

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 1457 root      20   0  661564  66728  14552 R  43.8  3.5   2:08.37 dnf
  531 root      20   0  358748  29468   7036 S   0.0  1.5   0:00.91 firewalld
 1042 root      20   0  574200  19552   6112 S   0.0  1.0   0:00.35 tuned
  491 polkitd   20   0  613016  11916   4896 S   0.0  0.6   0:00.04 polkitd
 1046 root      20   0  424064  11420   9052 S   0.0  0.6   0:00.09 smbd
  542 root      20   0  701996   9568   7052 S   0.0  0.5   0:00.43 NetworkManager
 1215 root      20   0  158924   5668   4336 S   0.0  0.3   0:00.06 sshd
 1542 root      20   0  158924   5668   4336 S   0.0  0.3   0:00.02 sshd
  755 root      20   0  102896   5492   3428 S   0.0  0.3   0:00.01 dhclient
 1045 root      20   0  216420   4796   3360 S   0.0  0.3   0:00.35 rsyslogd
  654 root      20   0   78812   4636   3640 S   0.0  0.2   0:00.04 wpa_supplicant
 1044 root      20   0  112920   4292   3268 S   0.0  0.2   0:00.00 sshd
 1150 postfix   20   0   90348   4240   3160 S   0.0  0.2   0:00.00 qmgr			

```

## 2. ps - report a snapshot of the current processes

```

ps 命令能够给出当前系统中进程的快照。它能捕获系统在某一时间的进程状态。如果你想不断更新查看的这个状态，可以使用 top 命令。

### Display all processes

1) 使用 -a 参数
    -a 代表 all,同时加上 x 参数会显示没有控制终端的进程.
ps aux

[root@netkiller ~]# ps aux | head -n 1
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND

2) 参数 -e 显示所有进程信息,-f 参数来查看全格式的信息列表
ps -ef

[root@netkiller ~]# ps -ef | head -n 1
UID        PID  PPID  C STIME TTY          TIME CMD

Use the "u" option or "-f" option to display detailed information about the processes

3) -F You can get even more columns .
 ps -eF

[root@netkiller ~]# ps -eF | head -n 1
UID        PID  PPID  C    SZ   RSS PSR STIME TTY          TIME CMD

The extra columns are SZ, RSS and PSR. 
    SZ is the size of the process
    RSS is the real memory size 
    PSR is the processor the command is assigned to

### Selecting Specific Processes Using The ps Command

1) 通过进程名过滤
使用 -C 参数,后面跟你要找的进程的名字.如果想要看到更多的细节,我们可以使用-f 参数来查看全格式的信息列表：
// 比如想显示一个名为 mysqld 的进程的信息,就可以使用下面的命令：
ps -f  -C mysqld

2) -p  pid|pids

3) -U  username 

如果我们想知道特定进程的线程，可以使用 -L 参数，后面加上特定的 PID.
-L 参数显示进程，并尽量显示其 LWP(线程 ID)和 NLWP(线程的个数)

ps -Lf -p  1036

> --no-header 

--no-header  print no header line at all

[root@netkiller ~]# ps -C mysqld  --no-header
 1036 ?        01:20:48 mysqld

### Formatting ps Command Output

ps -e --format <format>
The formats available are as follows:
    %cpu    - cpu utilisation
    %mem    - memory percentage utilisation
    args    - The command with all its arguments
    c       - processor utilisation
    cmd     - The command
    comm    - The command name only
    cp      - CPU Usage
    cputime - CPU Time
    egid    - Effective group id
    egroup  - Effective group
    etime   - Elapsed time
    euid    - Effective user id
    euser   - Effective user
    gid     - Group id
    group   - Group name
    pgid    - Process group id
    pgrp    - Process group
    ppid    - Parent Process ID
    start   - Time the process started
    sz      - Size in physical pages
    thcount - Threads owned by the process
    time    - Cumulative time
    uid     - User Id
    uname   - User name

ps -e --format="uid uname cmd time"  // eq
ps -eo uid,uname,cmd,time

### Sorting Output

ps -ef --sort <sortcolumns>

--sort 参数则是指定排序的依据栏位，预设会依照数值由小到大排序，若要由大到小的方式排序的话，可以在栏位名称前加上一个负号('-')

The choice of sort options are as follows:
    cmd    - Executable name
    pcpu   - CPU utilisation
    flags  - Flags
    pgrp   - Process group id
    cutime - Cumulative user time
    cstime - Cumulative system time
    utime  - User time
    pid    - Process ID
    ppid   - Parent process ID
    size   - Size
    uid    - User ID
    user   - User Name

ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu

1) 根据 CPU 使用来升序排序
ps aux --sort -pcpu

2)  内存使用 来升序排序
ps aux --sort -pmem

3) 合并前面两个命令,并通过管道显示前 10 个结果
ps aux --sort -pcpu,+pmem  | head

### example
> CPU 占用最多的前 10 个进程

1) ps aux | sort -k3nr | head

2) top （然后按下 P，注意大写）

3) ps -eo user,pid,ppid,tid,time,%cpu,cmd --sort=-%cpu

>  获取特定进程的线程信息

ps -Lf -p  1036

```

ps aux

```

$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0   4020   888 ?        Ss   08:50   0:01 /sbin/init
root         2  0.0  0.0      0     0 ?        S<   08:50   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S<   08:50   0:00 [migration/0]
root         4  0.0  0.0      0     0 ?        S<   08:50   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   08:50   0:00 [watchdog/0]
root         6  0.0  0.0      0     0 ?        S<   08:50   0:00 [migration/1]
root         7  0.0  0.0      0     0 ?        S<   08:50   0:00 [ksoftirqd/1]
root         8  0.0  0.0      0     0 ?        S<   08:50   0:00 [watchdog/1]
root         9  0.0  0.0      0     0 ?        S<   08:50   0:00 [migration/2]
root        10  0.0  0.0      0     0 ?        S<   08:50   0:00 [ksoftirqd/2]
root        11  0.0  0.0      0     0 ?        S<   08:50   0:00 [watchdog/2]
root        12  0.0  0.0      0     0 ?        S<   08:50   0:00 [migration/3]
root        13  0.0  0.0      0     0 ?        S<   08:50   0:00 [ksoftirqd/3]
root        14  0.0  0.0      0     0 ?        S<   08:50   0:00 [watchdog/3]
root        15  0.0  0.0      0     0 ?        S<   08:50   0:00 [events/0]
root        16  0.0  0.0      0     0 ?        S<   08:50   0:00 [events/1]
root        17  0.0  0.0      0     0 ?        S<   08:50   0:00 [events/2]
root        18  0.0  0.0      0     0 ?        S<   08:50   0:00 [events/3]
root        19  0.0  0.0      0     0 ?        S<   08:50   0:00 [khelper]
root        54  0.0  0.0      0     0 ?        S<   08:50   0:00 [kblockd/0]
root        55  0.0  0.0      0     0 ?        S<   08:50   0:00 [kblockd/1]
root        56  0.0  0.0      0     0 ?        S<   08:50   0:00 [kblockd/2]
root        57  0.0  0.0      0     0 ?        S<   08:50   0:00 [kblockd/3]
root        60  0.0  0.0      0     0 ?        S<   08:50   0:00 [kacpid]
root        61  0.0  0.0      0     0 ?        S<   08:50   0:00 [kacpi_notify]
root       136  0.0  0.0      0     0 ?        S<   08:50   0:00 [kseriod]
root       193  0.0  0.0      0     0 ?        S    08:50   0:00 [pdflush]
root       194  0.0  0.0      0     0 ?        S    08:50   0:00 [pdflush]
root       195  0.0  0.0      0     0 ?        S<   08:50   0:00 [kswapd0]
root       238  0.0  0.0      0     0 ?        S<   08:50   0:00 [aio/0]
root       239  0.0  0.0      0     0 ?        S<   08:50   0:00 [aio/1]
root       240  0.0  0.0      0     0 ?        S<   08:50   0:00 [aio/2]
root       241  0.0  0.0      0     0 ?        S<   08:50   0:00 [aio/3]
root      1468  0.0  0.0      0     0 ?        S<   08:50   0:00 [ksuspend_usbd]
root      1471  0.0  0.0      0     0 ?        S<   08:50   0:00 [khubd]
root      1559  0.0  0.0      0     0 ?        S<   08:50   0:00 [ata/0]
root      1560  0.0  0.0      0     0 ?        S<   08:50   0:00 [ata/1]
root      1561  0.0  0.0      0     0 ?        S<   08:50   0:00 [ata/2]
root      1562  0.0  0.0      0     0 ?        S<   08:50   0:00 [ata/3]
root      1563  0.0  0.0      0     0 ?        S<   08:50   0:00 [ata_aux]
root      1743  0.0  0.0      0     0 ?        S<   08:50   0:00 [scsi_eh_0]
root      1744  0.0  0.0      0     0 ?        S<   08:50   0:00 [scsi_eh_1]
root      1878  0.0  0.0      0     0 ?        S<   08:50   0:00 [scsi_eh_2]
root      1879  0.0  0.0      0     0 ?        S<   08:50   0:00 [scsi_eh_3]
root      2508  0.0  0.0      0     0 ?        S<   08:50   0:00 [kjournald]
root      2707  0.0  0.0  17188  1284 ?        S<s  08:50   0:00 /sbin/udevd --daemon
root      3055  0.0  0.0      0     0 ?        S<   08:50   0:00 [kpsmoused]
dhcp      4223  0.0  0.0  15108   840 ?        S<s  08:50   0:00 dhclient3 -e IF_METRIC=100 -pf /var
root      4311  0.0  0.0      0     0 ?        S<   08:50   0:00 [kjournald]
root      4585  0.0  0.0   3864   596 tty4     Ss+  08:50   0:00 /sbin/getty 38400 tty4
root      4586  0.0  0.0   3864   596 tty5     Ss+  08:50   0:00 /sbin/getty 38400 tty5
root      4588  0.0  0.0   3864   592 tty2     Ss+  08:50   0:00 /sbin/getty 38400 tty2
root      4591  0.0  0.0   3864   596 tty3     Ss+  08:50   0:00 /sbin/getty 38400 tty3
root      4592  0.0  0.0  45700  1328 ttyS0    Ss   08:50   0:00 /bin/login --
root      4792  0.0  0.0  13076  1752 ?        Ss   08:50   0:00 /usr/sbin/acpid -c /etc/acpi/events
root      4859  0.0  0.0      0     0 ?        S<   08:50   0:00 [kondemand/0]
root      4860  0.0  0.0      0     0 ?        S<   08:50   0:00 [kondemand/1]
root      4861  0.0  0.0      0     0 ?        S<   08:50   0:00 [kondemand/2]
root      4862  0.0  0.0      0     0 ?        S<   08:50   0:00 [kondemand/3]
syslog    4926  0.0  0.0  12296   784 ?        Ss   08:50   0:00 /sbin/syslogd -u syslog
root      4980  0.0  0.0   8132   592 ?        S    08:50   0:00 /bin/dd bs 1 if /proc/kmsg of /var/
klog      4982  0.0  0.1   6184  2876 ?        Ss   08:50   0:00 /sbin/klogd -P /var/run/klogd/kmsg
108       5004  0.0  0.0  21320  1104 ?        Ss   08:50   0:00 /usr/bin/dbus-daemon --system
root      5020  0.0  0.1  40112  2084 ?        Ss   08:50   0:00 /usr/sbin/NetworkManager --pid-file
root      5034  0.0  0.0  24128  1256 ?        Ss   08:50   0:00 /usr/sbin/NetworkManagerDispatcher
root      5047  0.0  0.0  35192  1220 ?        Ss   08:50   0:00 /usr/bin/system-tools-backends
root      5069  0.0  0.0  50916  1204 ?        Ss   08:50   0:00 /usr/sbin/sshd
avahi     5090  0.0  0.0  29708  1508 ?        Ss   08:50   0:00 avahi-daemon: running [netkiller.lo
avahi     5091  0.0  0.0  29580   508 ?        Ss   08:50   0:00 avahi-daemon: chroot helper
postgres  5117  0.0  0.3 101164  6196 ?        S    08:50   0:01 /usr/lib/postgresql/8.3/bin/postgre
postgres  5121  0.0  0.0 101164  1624 ?        Ss   08:50   0:00 postgres: writer process
postgres  5122  0.0  0.0 101164  1436 ?        Ss   08:50   0:00 postgres: wal writer process
postgres  5123  0.0  0.0 101304  1684 ?        Ss   08:50   0:00 postgres: autovacuum launcher proce
postgres  5124  0.0  0.0  71628  1432 ?        Ss   08:50   0:00 postgres: stats collector process
root      5167  0.0  0.1  72312  2704 ?        Ss   08:50   0:00 /usr/sbin/cupsd
115       5423  0.0  0.0  47552  1052 ?        Ss   08:50   0:00 /usr/sbin/exim4 -bd -q30m
gnump3d   5431  0.0  0.8  54728 17744 ?        S    08:50   0:00 /usr/bin/perl -w /usr/bin/gnump3d
root      5481  0.0  0.0  10444   888 ?        S    08:50   0:00 /usr/bin/rsync --no-detach --daemon
root      5500  0.0  0.0  54048  1484 ?        Ss   08:50   0:00 /usr/sbin/nmbd -D
root      5502  0.0  0.1  74548  2788 ?        Ss   08:50   0:00 /usr/sbin/smbd -D
root      5573  0.0  0.0  19332   940 ?        Ss   08:50   0:00 /usr/sbin/xinetd -pidfile /var/run/
root      5574  0.0  0.0   6272   840 ?        Ss   08:50   0:00 /usr/sbin/dhcdbd --system
111       5593  0.0  0.2  35804  4396 ?        Ss   08:50   0:00 /usr/sbin/hald
root      5596  0.0  0.1  30528  2384 ?        Ssl  08:50   0:00 /usr/sbin/console-kit-daemon
root      5658  0.0  0.0  17820  1164 ?        S    08:50   0:00 hald-runner
root      5660  0.0  0.0  74548  1280 ?        S    08:50   0:00 /usr/sbin/smbd -D
root      5690  0.0  0.0  19928  1148 ?        S    08:50   0:00 hald-addon-input: Listening on /dev
111       5693  0.0  0.0  16672   992 ?        S    08:50   0:00 hald-addon-acpi: listening on acpid
root      5722  0.0  0.0  13532  1300 ?        Ss   08:50   0:00 /usr/sbin/hcid -x -s
root      5730  0.0  0.0      0     0 ?        S<   08:50   0:00 [btaddconn]
root      5732  0.0  0.0      0     0 ?        S<   08:50   0:00 [btdelconn]
root      5744  0.0  0.0  13428  1352 ?        S    08:50   0:00 /usr/lib/bluetooth/bluetoothd-servi
root      5745  0.0  0.0  13352  1140 ?        S    08:50   0:00 /usr/lib/bluetooth/bluetoothd-servi
root      5755  0.0  0.0      0     0 ?        S<   08:50   0:00 [krfcommd]
root      5791  0.0  0.0 116168  1860 ?        Ss   08:50   0:00 /usr/sbin/gdm
nagios    5847  0.0  0.0  34276  1852 ?        SNsl 08:50   0:00 /usr/sbin/nagios2 -d /etc/nagios2/n
daemon    5884  0.0  0.0  16428   432 ?        Ss   08:50   0:00 /usr/sbin/atd
root      5898  0.0  0.0  18616   980 ?        Ss   08:50   0:00 /usr/sbin/cron
www-data  5929  0.0  0.1  58976  2380 ?        S    08:50   0:00 /usr/sbin/lighttpd -f /etc/lighttpd
www-data  5940  0.0  0.2  83492  6124 ?        Ss   08:50   0:00 /usr/bin/php-cgi
www-data  5967  0.0  0.2  83492  6124 ?        Ss   08:50   0:00 /usr/bin/php-cgi
root      6016  0.0  0.0   3864   592 tty1     Ss+  08:50   0:00 /sbin/getty 38400 tty1
www-data  6022  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6023  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6024  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6025  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6026  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6027  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6028  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
www-data  6029  0.0  0.1  83492  2764 ?        S    08:50   0:00 /usr/bin/php-cgi
root      6058  0.0  0.0 116168  1840 ?        T    08:50   0:00 /usr/sbin/gdm
root      6062  0.0  0.0      0     0 ?        Z    08:50   0:00 [kill] <defunct>
root      6102  0.0  0.0  17336   920 ?        S    08:50   0:00 xinit /etc/gdm/failsafeXinit /etc/X
root      6104  0.0  0.3  76076  7644 tty7     S<s+ 08:50   0:01 /usr/bin/X :0 -auth /var/lib/gdm/:0
root      6111  0.0  0.0   3944   584 ?        S    08:51   0:00 /bin/sh /etc/gdm/failsafeXinit /etc
root      6112  0.0  0.2 126768  5000 ?        S    08:51   0:00 /usr/bin/gksu -u root /usr/bin/xfai
root      6114  0.0  0.2  41308  5516 ?        S    08:51   0:00 /usr/lib/libgconf2-4/gconfd-2 5
neo       6115  0.0  0.1  20944  3888 ttyS0    S    08:51   0:00 -bash
root      6131  0.0  1.0 156296 21096 ?        S    08:51   0:00 /usr/bin/python /usr/bin/xfailsafed
neo       6164  0.0  0.1  74896  3664 ?        S    08:52   0:00 /usr/sbin/smbd -D
neo       7949  0.0  0.0   8696  1268 ttyS0    S+   11:19   0:00 man ps
neo       7957  0.0  0.0   9552  1008 ttyS0    S+   11:19   0:00 pager -s
root      7971  0.0  0.1  70028  3028 ?        Ss   11:20   0:00 sshd: neo [priv]
neo       7978  0.0  0.0  70028  1716 ?        S    11:20   0:00 sshd: neo@pts/0
neo       7979  0.2  0.1  20944  3852 pts/0    Ss   11:20   0:00 -bash
neo       8006  0.0  0.0  15064  1092 pts/0    R+   11:22   0:00 ps aux

```

ps ax

```

neo@netkiller:~$ ps ax
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:01 /sbin/init
    2 ?        S<     0:00 [kthreadd]
    3 ?        S<     0:00 [migration/0]
    4 ?        S<     0:00 [ksoftirqd/0]
    5 ?        S<     0:00 [watchdog/0]
    6 ?        S<     0:00 [migration/1]
    7 ?        S<     0:00 [ksoftirqd/1]
    8 ?        S<     0:00 [watchdog/1]
    9 ?        S<     0:00 [migration/2]
   10 ?        S<     0:00 [ksoftirqd/2]
   11 ?        S<     0:00 [watchdog/2]
   12 ?        S<     0:00 [migration/3]
   13 ?        S<     0:00 [ksoftirqd/3]
   14 ?        S<     0:00 [watchdog/3]
   15 ?        S<     0:00 [events/0]
   16 ?        S<     0:00 [events/1]
   17 ?        S<     0:00 [events/2]
   18 ?        S<     0:00 [events/3]
   19 ?        S<     0:00 [khelper]
   54 ?        S<     0:00 [kblockd/0]
   55 ?        S<     0:00 [kblockd/1]
   56 ?        S<     0:00 [kblockd/2]
   57 ?        S<     0:00 [kblockd/3]
   60 ?        S<     0:00 [kacpid]
   61 ?        S<     0:00 [kacpi_notify]
  136 ?        S<     0:00 [kseriod]
  193 ?        S      0:00 [pdflush]
  194 ?        S      0:00 [pdflush]
  195 ?        S<     0:00 [kswapd0]
  238 ?        S<     0:00 [aio/0]
  239 ?        S<     0:00 [aio/1]
  240 ?        S<     0:00 [aio/2]
  241 ?        S<     0:00 [aio/3]
 1468 ?        S<     0:00 [ksuspend_usbd]
 1471 ?        S<     0:00 [khubd]
 1559 ?        S<     0:00 [ata/0]
 1560 ?        S<     0:00 [ata/1]
 1561 ?        S<     0:00 [ata/2]
 1562 ?        S<     0:00 [ata/3]
 1563 ?        S<     0:00 [ata_aux]
 1743 ?        S<     0:00 [scsi_eh_0]
 1744 ?        S<     0:00 [scsi_eh_1]
 1878 ?        S<     0:00 [scsi_eh_2]
 1879 ?        S<     0:00 [scsi_eh_3]
 2508 ?        S<     0:00 [kjournald]
 2707 ?        S<s    0:00 /sbin/udevd --daemon
 3055 ?        S<     0:00 [kpsmoused]
 4223 ?        S<s    0:00 dhclient3 -e IF_METRIC=100 -pf /var/run/dhclient.eth0.pid -lf /var/lib/dh
 4311 ?        S<     0:00 [kjournald]
 4585 tty4     Ss+    0:00 /sbin/getty 38400 tty4
 4586 tty5     Ss+    0:00 /sbin/getty 38400 tty5
 4588 tty2     Ss+    0:00 /sbin/getty 38400 tty2
 4591 tty3     Ss+    0:00 /sbin/getty 38400 tty3
 4592 ttyS0    Ss     0:00 /bin/login --
 4792 ?        Ss     0:00 /usr/sbin/acpid -c /etc/acpi/events -s /var/run/acpid.socket
 4859 ?        S<     0:00 [kondemand/0]
 4860 ?        S<     0:00 [kondemand/1]
 4861 ?        S<     0:00 [kondemand/2]
 4862 ?        S<     0:00 [kondemand/3]
 4926 ?        Ss     0:00 /sbin/syslogd -u syslog
 4980 ?        S      0:00 /bin/dd bs 1 if /proc/kmsg of /var/run/klogd/kmsg
 4982 ?        Ss     0:00 /sbin/klogd -P /var/run/klogd/kmsg
 5004 ?        Ss     0:00 /usr/bin/dbus-daemon --system
 5020 ?        Ss     0:00 /usr/sbin/NetworkManager --pid-file /var/run/NetworkManager/NetworkManage
 5034 ?        Ss     0:00 /usr/sbin/NetworkManagerDispatcher --pid-file /var/run/NetworkManager/Net
 5047 ?        Ss     0:00 /usr/bin/system-tools-backends
 5069 ?        Ss     0:00 /usr/sbin/sshd
 5090 ?        Ss     0:00 avahi-daemon: running [netkiller.local]
 5091 ?        Ss     0:00 avahi-daemon: chroot helper
 5117 ?        S      0:01 /usr/lib/postgresql/8.3/bin/postgres -D /var/lib/postgresql/8.3/main -c c
 5121 ?        Ss     0:00 postgres: writer process
 5122 ?        Ss     0:00 postgres: wal writer process
 5123 ?        Ss     0:00 postgres: autovacuum launcher process
 5124 ?        Ss     0:00 postgres: stats collector process
 5167 ?        Ss     0:00 /usr/sbin/cupsd
 5423 ?        Ss     0:00 /usr/sbin/exim4 -bd -q30m
 5431 ?        S      0:00 /usr/bin/perl -w /usr/bin/gnump3d
 5481 ?        S      0:00 /usr/bin/rsync --no-detach --daemon --config /etc/rsyncd.conf
 5500 ?        Ss     0:00 /usr/sbin/nmbd -D
 5502 ?        Ss     0:00 /usr/sbin/smbd -D
 5573 ?        Ss     0:00 /usr/sbin/xinetd -pidfile /var/run/xinetd.pid -stayalive -inetd_compat
 5574 ?        Ss     0:00 /usr/sbin/dhcdbd --system
 5593 ?        Ss     0:00 /usr/sbin/hald
 5596 ?        Ssl    0:00 /usr/sbin/console-kit-daemon
 5658 ?        S      0:00 hald-runner
 5660 ?        S      0:00 /usr/sbin/smbd -D
 5690 ?        S      0:00 hald-addon-input: Listening on /dev/input/event3 /dev/input/event2
 5693 ?        S      0:00 hald-addon-acpi: listening on acpid socket /var/run/acpid.socket
 5722 ?        Ss     0:00 /usr/sbin/hcid -x -s
 5730 ?        S<     0:00 [btaddconn]
 5732 ?        S<     0:00 [btdelconn]
 5744 ?        S      0:00 /usr/lib/bluetooth/bluetoothd-service-audio
 5745 ?        S      0:00 /usr/lib/bluetooth/bluetoothd-service-input
 5755 ?        S<     0:00 [krfcommd]
 5791 ?        Ss     0:00 /usr/sbin/gdm
 5847 ?        SNsl   0:00 /usr/sbin/nagios2 -d /etc/nagios2/nagios.cfg
 5884 ?        Ss     0:00 /usr/sbin/atd
 5898 ?        Ss     0:00 /usr/sbin/cron
 5929 ?        S      0:00 /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
 5940 ?        Ss     0:00 /usr/bin/php-cgi
 5967 ?        Ss     0:00 /usr/bin/php-cgi
 6016 tty1     Ss+    0:00 /sbin/getty 38400 tty1
 6022 ?        S      0:00 /usr/bin/php-cgi
 6023 ?        S      0:00 /usr/bin/php-cgi
 6024 ?        S      0:00 /usr/bin/php-cgi
 6025 ?        S      0:00 /usr/bin/php-cgi
 6026 ?        S      0:00 /usr/bin/php-cgi
 6027 ?        S      0:00 /usr/bin/php-cgi
 6028 ?        S      0:00 /usr/bin/php-cgi
 6029 ?        S      0:00 /usr/bin/php-cgi
 6058 ?        T      0:00 /usr/sbin/gdm
 6062 ?        Z      0:00 [kill] <defunct>
 6102 ?        S      0:00 xinit /etc/gdm/failsafeXinit /etc/X11/xorg.conf.failsafe with-gdm -- /usr
 6104 tty7     S<s+   0:01 /usr/bin/X :0 -auth /var/lib/gdm/:0.Xauth -nolisten tcp vt7 -br -once -co
 6111 ?        S      0:00 /bin/sh /etc/gdm/failsafeXinit /etc/X11/xorg.conf.failsafe with-gdm
 6112 ?        S      0:00 /usr/bin/gksu -u root /usr/bin/xfailsafedialog
 6114 ?        S      0:00 /usr/lib/libgconf2-4/gconfd-2 5
 6115 ttyS0    S      0:00 -bash
 6131 ?        S      0:00 /usr/bin/python /usr/bin/xfailsafedialog
 6164 ?        S      0:00 /usr/sbin/smbd -D
 7949 ttyS0    S+     0:00 man ps
 7957 ttyS0    S+     0:00 pager -s
 7971 ?        Ss     0:00 sshd: neo [priv]
 7978 ?        S      0:00 sshd: neo@pts/0
 7979 pts/0    Ss     0:00 -bash
 7997 pts/0    R+     0:00 ps ax

```

### 2.1. 完整的显示命令参数

ps axww

```

$ ps axww
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:01 /sbin/init
    2 ?        S<     0:00 [kthreadd]
    3 ?        S<     0:00 [migration/0]
    4 ?        S<     0:00 [ksoftirqd/0]
    5 ?        S<     0:00 [watchdog/0]
    6 ?        S<     0:00 [migration/1]
    7 ?        S<     0:00 [ksoftirqd/1]
    8 ?        S<     0:00 [watchdog/1]
    9 ?        S<     0:00 [migration/2]
   10 ?        S<     0:00 [ksoftirqd/2]
   11 ?        S<     0:00 [watchdog/2]
   12 ?        S<     0:00 [migration/3]
   13 ?        S<     0:00 [ksoftirqd/3]
   14 ?        S<     0:00 [watchdog/3]
   15 ?        S<     0:00 [events/0]
   16 ?        S<     0:00 [events/1]
   17 ?        S<     0:00 [events/2]
   18 ?        S<     0:00 [events/3]
   19 ?        S<     0:00 [khelper]
   54 ?        S<     0:00 [kblockd/0]
   55 ?        S<     0:00 [kblockd/1]
   56 ?        S<     0:00 [kblockd/2]
   57 ?        S<     0:00 [kblockd/3]
   60 ?        S<     0:00 [kacpid]
   61 ?        S<     0:00 [kacpi_notify]
  136 ?        S<     0:00 [kseriod]
  193 ?        S      0:00 [pdflush]
  194 ?        S      0:00 [pdflush]
  195 ?        S<     0:00 [kswapd0]
  238 ?        S<     0:00 [aio/0]
  239 ?        S<     0:00 [aio/1]
  240 ?        S<     0:00 [aio/2]
  241 ?        S<     0:00 [aio/3]
 1468 ?        S<     0:00 [ksuspend_usbd]
 1471 ?        S<     0:00 [khubd]
 1559 ?        S<     0:00 [ata/0]
 1560 ?        S<     0:00 [ata/1]
 1561 ?        S<     0:00 [ata/2]
 1562 ?        S<     0:00 [ata/3]
 1563 ?        S<     0:00 [ata_aux]
 1743 ?        S<     0:00 [scsi_eh_0]
 1744 ?        S<     0:00 [scsi_eh_1]
 1878 ?        S<     0:00 [scsi_eh_2]
 1879 ?        S<     0:00 [scsi_eh_3]
 2508 ?        S<     0:00 [kjournald]
 2707 ?        S<s    0:00 /sbin/udevd --daemon
 3055 ?        S<     0:00 [kpsmoused]
 4223 ?        S<s    0:00 dhclient3 -e IF_METRIC=100 -pf /var/run/dhclient.eth0.pid -lf /var/lib/dhcp3/dhclient.eth0.leases eth0
 4311 ?        S<     0:00 [kjournald]
 4585 tty4     Ss+    0:00 /sbin/getty 38400 tty4
 4586 tty5     Ss+    0:00 /sbin/getty 38400 tty5
 4588 tty2     Ss+    0:00 /sbin/getty 38400 tty2
 4591 tty3     Ss+    0:00 /sbin/getty 38400 tty3
 4592 ttyS0    Ss     0:00 /bin/login --
 4792 ?        Ss     0:00 /usr/sbin/acpid -c /etc/acpi/events -s /var/run/acpid.socket
 4859 ?        S<     0:00 [kondemand/0]
 4860 ?        S<     0:00 [kondemand/1]
 4861 ?        S<     0:00 [kondemand/2]
 4862 ?        S<     0:00 [kondemand/3]
 4926 ?        Ss     0:00 /sbin/syslogd -u syslog
 4980 ?        S      0:00 /bin/dd bs 1 if /proc/kmsg of /var/run/klogd/kmsg
 4982 ?        Ss     0:00 /sbin/klogd -P /var/run/klogd/kmsg
 5004 ?        Ss     0:00 /usr/bin/dbus-daemon --system
 5020 ?        Ss     0:00 /usr/sbin/NetworkManager --pid-file /var/run/NetworkManager/NetworkManager.pid
 5034 ?        Ss     0:00 /usr/sbin/NetworkManagerDispatcher --pid-file /var/run/NetworkManager/NetworkManagerDispatcher.pid
 5047 ?        Ss     0:00 /usr/bin/system-tools-backends
 5069 ?        Ss     0:00 /usr/sbin/sshd
 5090 ?        Ss     0:00 avahi-daemon: running [netkiller.local]
 5091 ?        Ss     0:00 avahi-daemon: chroot helper
 5117 ?        S      0:01 /usr/lib/postgresql/8.3/bin/postgres -D /var/lib/postgresql/8.3/main -c config_file=/etc/postgresql/8.3/main/postgresql.conf
 5121 ?        Ss     0:00 postgres: writer process
 5122 ?        Ss     0:00 postgres: wal writer process
 5123 ?        Ss     0:00 postgres: autovacuum launcher process
 5124 ?        Ss     0:00 postgres: stats collector process
 5167 ?        Ss     0:00 /usr/sbin/cupsd
 5423 ?        Ss     0:00 /usr/sbin/exim4 -bd -q30m
 5431 ?        S      0:00 /usr/bin/perl -w /usr/bin/gnump3d
 5481 ?        S      0:00 /usr/bin/rsync --no-detach --daemon --config /etc/rsyncd.conf
 5500 ?        Ss     0:00 /usr/sbin/nmbd -D
 5502 ?        Ss     0:00 /usr/sbin/smbd -D
 5573 ?        Ss     0:00 /usr/sbin/xinetd -pidfile /var/run/xinetd.pid -stayalive -inetd_compat
 5574 ?        Ss     0:00 /usr/sbin/dhcdbd --system
 5593 ?        Ss     0:00 /usr/sbin/hald
 5596 ?        Ssl    0:00 /usr/sbin/console-kit-daemon
 5658 ?        S      0:00 hald-runner
 5660 ?        S      0:00 /usr/sbin/smbd -D
 5690 ?        S      0:00 hald-addon-input: Listening on /dev/input/event3 /dev/input/event2
 5693 ?        S      0:00 hald-addon-acpi: listening on acpid socket /var/run/acpid.socket
 5722 ?        Ss     0:00 /usr/sbin/hcid -x -s
 5730 ?        S<     0:00 [btaddconn]
 5732 ?        S<     0:00 [btdelconn]
 5744 ?        S      0:00 /usr/lib/bluetooth/bluetoothd-service-audio
 5745 ?        S      0:00 /usr/lib/bluetooth/bluetoothd-service-input
 5755 ?        S<     0:00 [krfcommd]
 5791 ?        Ss     0:00 /usr/sbin/gdm
 5847 ?        SNsl   0:00 /usr/sbin/nagios2 -d /etc/nagios2/nagios.cfg
 5884 ?        Ss     0:00 /usr/sbin/atd
 5898 ?        Ss     0:00 /usr/sbin/cron
 5929 ?        S      0:00 /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
 5940 ?        Ss     0:00 /usr/bin/php-cgi
 5967 ?        Ss     0:00 /usr/bin/php-cgi
 6016 tty1     Ss+    0:00 /sbin/getty 38400 tty1
 6022 ?        S      0:00 /usr/bin/php-cgi
 6023 ?        S      0:00 /usr/bin/php-cgi
 6024 ?        S      0:00 /usr/bin/php-cgi
 6025 ?        S      0:00 /usr/bin/php-cgi
 6026 ?        S      0:00 /usr/bin/php-cgi
 6027 ?        S      0:00 /usr/bin/php-cgi
 6028 ?        S      0:00 /usr/bin/php-cgi
 6029 ?        S      0:00 /usr/bin/php-cgi
 6058 ?        T      0:00 /usr/sbin/gdm
 6062 ?        Z      0:00 [kill] <defunct>
 6102 ?        S      0:00 xinit /etc/gdm/failsafeXinit /etc/X11/xorg.conf.failsafe with-gdm -- /usr/bin/X :0 -auth /var/lib/gdm/:0.Xauth -nolisten tcp vt7 -br -once -config /etc/X11/xorg.conf.failsafe
 6104 tty7     S<s+   0:01 /usr/bin/X :0 -auth /var/lib/gdm/:0.Xauth -nolisten tcp vt7 -br -once -config /etc/X11/xorg.conf.failsafe
 6111 ?        S      0:00 /bin/sh /etc/gdm/failsafeXinit /etc/X11/xorg.conf.failsafe with-gdm
 6112 ?        S      0:00 /usr/bin/gksu -u root /usr/bin/xfailsafedialog
 6114 ?        S      0:00 /usr/lib/libgconf2-4/gconfd-2 5
 6115 ttyS0    S      0:00 -bash
 6131 ?        S      0:00 /usr/bin/python /usr/bin/xfailsafedialog
 6164 ?        S      0:00 /usr/sbin/smbd -D
 7949 ttyS0    S+     0:00 man ps
 7957 ttyS0    S+     0:00 pager -s
 7971 ?        Ss     0:00 sshd: neo [priv]
 7978 ?        S      0:00 sshd: neo@pts/0
 7979 pts/0    Ss     0:00 -bash
 8012 pts/0    R+     0:00 ps axww

```

### 2.2. 显示进程之间的关系

ps auxf

```

www-data 18743  0.0  0.1  82520  3776 ?        S<   11:18   0:02 /usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
www-data 18744  0.0  0.4 240904  9376 ?        S<s  11:18   0:00  \_ /usr/bin/php-cgi
www-data 18748  0.0  0.2 240904  4296 ?        S<   11:18   0:00      \_ /usr/bin/php-cgi
www-data 18749  0.0  0.2 240904  4296 ?        S<   11:18   0:00      \_ /usr/bin/php-cgi
www-data 18750  0.0  0.2 240904  4296 ?        S<   11:18   0:00      \_ /usr/bin/php-cgi

```

### 2.3. ps axef

```

[root@development ~]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD

```

```

# ps axef

```

### 2.4. ps -eo pid,cmd

```

$ ps -eo pid,cmd

```

### 2.5. ps jax

```

# ps jax
 PPID   PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
    0     1     1     1 ?           -1 Ss       0   1:18 /sbin/init
    0     2     0     0 ?           -1 S        0   0:00 [kthreadd]
    2     3     0     0 ?           -1 S        0   3:32 [ksoftirqd/0]
    2     4     0     0 ?           -1 S        0  14:15 [migration/0]
    2     5     0     0 ?           -1 S        0   0:00 [watchdog/0]
    2     6     0     0 ?           -1 S        0  16:12 [migration/1]
    2     7     0     0 ?           -1 S        0   3:00 [ksoftirqd/1]
    2     8     0     0 ?           -1 S        0   0:00 [watchdog/1]
    2     9     0     0 ?           -1 S        0   1:01 [migration/2]
    2    10     0     0 ?           -1 S        0   3:40 [ksoftirqd/2]
    2    11     0     0 ?           -1 S        0   0:00 [watchdog/2]
    2    12     0     0 ?           -1 S        0   0:44 [migration/3]
    2    13     0     0 ?           -1 S        0   3:08 [ksoftirqd/3]
    2    14     0     0 ?           -1 S        0   0:00 [watchdog/3]
    2    15     0     0 ?           -1 S        0  28:37 [events/0]
    2    16     0     0 ?           -1 S        0  25:09 [events/1]
    2    17     0     0 ?           -1 S        0  65:53 [events/2]
    2    18     0     0 ?           -1 S        0  68:14 [events/3]
    2    19     0     0 ?           -1 S        0   0:00 [cpuset]
    2    20     0     0 ?           -1 S        0   0:00 [khelper]
    2    21     0     0 ?           -1 S        0   9:49 [netns]
    2    22     0     0 ?           -1 S        0   0:00 [async/mgr]
    2    23     0     0 ?           -1 S        0   0:00 [pm]
    2    25     0     0 ?           -1 S        0   0:43 [sync_supers]
    2    26     0     0 ?           -1 S        0   1:18 [bdi-default]
    2    27     0     0 ?           -1 S        0   0:00 [kintegrityd/0]
    2    28     0     0 ?           -1 S        0   0:00 [kintegrityd/1]
    2    29     0     0 ?           -1 S        0   0:00 [kintegrityd/2]
    2    30     0     0 ?           -1 S        0   0:00 [kintegrityd/3]
    2    31     0     0 ?           -1 S        0   0:40 [kblockd/0]
    2    32     0     0 ?           -1 S        0   0:38 [kblockd/1]
    2    33     0     0 ?           -1 S        0   0:24 [kblockd/2]
    2    34     0     0 ?           -1 S        0   0:24 [kblockd/3]
    2    35     0     0 ?           -1 S        0   0:00 [kacpid]
    2    36     0     0 ?           -1 S        0   0:00 [kacpi_notify]
    2    37     0     0 ?           -1 S        0   0:00 [kacpi_hotplug]
    2    38     0     0 ?           -1 S        0   0:00 [ata_aux]
    2    39     0     0 ?           -1 S        0   0:00 [ata_sff/0]
    2    40     0     0 ?           -1 S        0   0:00 [ata_sff/1]
    2    41     0     0 ?           -1 S        0   0:00 [ata_sff/2]
    2    42     0     0 ?           -1 S        0   0:00 [ata_sff/3]
    2    43     0     0 ?           -1 S        0   0:00 [khubd]
    2    44     0     0 ?           -1 S        0   0:00 [kseriod]
    2    45     0     0 ?           -1 S        0   0:00 [kmmcd]
    2    46     0     0 ?           -1 S        0   0:06 [khungtaskd]
    2    47     0     0 ?           -1 S        0 329:34 [kswapd0]
    2    48     0     0 ?           -1 SN       0   0:00 [ksmd]
    2    49     0     0 ?           -1 S        0   0:00 [aio/0]
    2    50     0     0 ?           -1 S        0   0:00 [aio/1]
    2    51     0     0 ?           -1 S        0   0:00 [aio/2]
    2    52     0     0 ?           -1 S        0   0:00 [aio/3]
    2    53     0     0 ?           -1 S        0   0:00 [ecryptfs-kthrea]
    2    54     0     0 ?           -1 S        0   0:00 [crypto/0]
    2    55     0     0 ?           -1 S        0   0:00 [crypto/1]
    2    56     0     0 ?           -1 S        0   0:00 [crypto/2]
    2    57     0     0 ?           -1 S        0   0:00 [crypto/3]
    2    62     0     0 ?           -1 S        0   0:00 [scsi_eh_0]
    2    63     0     0 ?           -1 S        0   0:00 [scsi_eh_1]
    2    66     0     0 ?           -1 S        0   0:00 [kstriped]
    2    67     0     0 ?           -1 S        0   0:00 [kmpathd/0]
    2    68     0     0 ?           -1 S        0   0:00 [kmpathd/1]
    2    69     0     0 ?           -1 S        0   0:00 [kmpathd/2]
    2    70     0     0 ?           -1 S        0   0:00 [kmpathd/3]
    2    71     0     0 ?           -1 S        0   0:00 [kmpath_handlerd]
    2    72     0     0 ?           -1 S        0   0:00 [ksnapd]
    2    73     0     0 ?           -1 S        0   0:00 [kondemand/0]
    2    74     0     0 ?           -1 S        0   0:00 [kondemand/1]
    2    75     0     0 ?           -1 S        0   0:00 [kondemand/2]
    2    76     0     0 ?           -1 S        0   0:00 [kondemand/3]
    2    77     0     0 ?           -1 S        0   0:00 [kconservative/0]
    2    78     0     0 ?           -1 S        0   0:00 [kconservative/1]
    2    79     0     0 ?           -1 S        0   0:00 [kconservative/2]
    2    80     0     0 ?           -1 S        0   0:00 [kconservative/3]
    2   205     0     0 ?           -1 S        0   0:00 [scsi_eh_2]
    2   255     0     0 ?           -1 S        0   0:00 [scsi_eh_3]
    2   283     0     0 ?           -1 S        0   0:00 [usbhid_resumer]
    2   289     0     0 ?           -1 S        0   4:24 [jbd2/sda1-8]
    2   290     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   291     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   292     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   293     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    1   337   336   336 ?           -1 S        0   0:31 upstart-udev-bridge --daemon
    1   343   343   343 ?           -1 S<s      0   0:20 udevd --daemon
    2   598     0     0 ?           -1 S        0   0:00 [kpsmoused]
    2   603     0     0 ?           -1 S        0   8:21 [edac-poller]
    1   675   675   675 ?           -1 Ss       1   0:00 portmap
    2   692     0     0 ?           -1 S        0   0:00 [radeon/0]
    2   693     0     0 ?           -1 S        0   0:00 [radeon/1]
    2   694     0     0 ?           -1 S        0   0:00 [radeon/2]
    2   695     0     0 ?           -1 S        0   0:00 [radeon/3]
    2   697     0     0 ?           -1 S        0   0:00 [ttm_swap]
    1   698   698   698 ?           -1 Ss     112   0:00 rpc.statd -L
    2   700     0     0 ?           -1 S        0   0:00 [rpciod/0]
    2   701     0     0 ?           -1 S        0   0:00 [rpciod/1]
    2   702     0     0 ?           -1 S        0   0:00 [rpciod/2]
    2   703     0     0 ?           -1 S        0   0:00 [rpciod/3]
    2   714     0     0 ?           -1 S<       0   0:25 [kslowd000]
    2   715     0     0 ?           -1 S<       0   0:20 [kslowd001]
    2   814     0     0 ?           -1 S        0 102:38 [flush-8:0]
    2   823     0     0 ?           -1 S        0  12:12 [jbd2/sda3-8]
    2   824     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   825     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   826     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   827     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   880     0     0 ?           -1 S        0  30:54 [jbd2/sdb1-8]
    2   881     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   882     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   883     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    2   884     0     0 ?           -1 S        0   0:00 [ext4-dio-unwrit]
    1   944   894   894 ?           -1 Sl     101   2:08 rsyslogd -c4
    2   958     0     0 ?           -1 S        0   0:00 [nfsiod]
    1   960   960   960 ?           -1 Ss       0   0:40 /usr/sbin/sshd
    1   972   972   972 ?           -1 Ss       0   0:02 rpc.idmapd
    1   975   975   975 tty4       975 Ss+      0   0:00 /sbin/getty -8 38400 tty4
    1   992   992   992 tty5       992 Ss+      0   0:00 /sbin/getty -8 38400 tty5
    1   997   997   997 tty3       997 Ss+      0   0:00 /sbin/getty -8 38400 tty3
    1  1000  1000  1000 tty6      1000 Ss+      0   0:00 /sbin/getty -8 38400 tty6
    1  1009  1009  1009 ?           -1 Ss       1   0:00 atd
    1  1058  1058  1058 ?           -1 Ss     106  20:42 /usr/sbin/nrpe -c /etc/nagios/nrpe.cfg -d
    1  1081  1081  1081 ?           -1 Ss       0  14:35 /usr/sbin/munin-node
    2  1087     0     0 ?           -1 S        0   0:00 [lockd]
    2  1088     0     0 ?           -1 S        0   0:06 [nfsd4]
    2  1089     0     0 ?           -1 S        0   0:00 [nfsd4_callbacks]
    2  1090     0     0 ?           -1 S        0   1:29 [nfsd]
    2  1091     0     0 ?           -1 S        0   1:29 [nfsd]
    2  1092     0     0 ?           -1 S        0   1:34 [nfsd]
    2  1093     0     0 ?           -1 S        0   1:35 [nfsd]
    2  1094     0     0 ?           -1 S        0   1:31 [nfsd]
    2  1095     0     0 ?           -1 S        0   1:31 [nfsd]
    2  1096     0     0 ?           -1 S        0   1:30 [nfsd]
    2  1097     0     0 ?           -1 S        0   1:30 [nfsd]
    1  1101  1101  1101 ?           -1 Ss       0   0:11 /usr/sbin/rpc.mountd --manage-gids
    1  1500  1499  1499 ?           -1 S      105  39:47 /usr/sbin/snmpd -Lsd -Lf /dev/null -u snmp -g snmp -I -smux -p /var/run/snmpd.pid
    1  2066  2066  2066 tty2      2066 Ss+      0   0:00 /sbin/getty -8 38400 tty2
    1  2068  2068  2068 tty1      2068 Ss+      0   0:00 /sbin/getty -8 38400 tty1
    1  5243  5243  5243 ?           -1 Ss       0   0:15 /usr/sbin/vsftpd
    1  6058  6058  6058 ?           -1 Ss       0   0:00 /bin/sh -c test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
 6058  6060  6058  6058 ?           -1 S        0   0:00 run-parts --report /etc/cron.daily
 6060  6062  6058  6058 ?           -1 Z        0   0:00 [apt] <defunct>
    1  8627  8627  8627 ?           -1 Ss     115  12:06 /usr/sbin/gmond
    1  8674  8674  8674 ?           -1 Ssl    102   0:09 /usr/sbin/named -u bind
    1  9027  9027  9027 ?           -1 Ss       0   0:02 cron
    2 12690     0     0 ?           -1 S        0   0:00 [xfs_mru_cache]
    2 12691     0     0 ?           -1 S        0   0:00 [xfslogd/0]
    2 12692     0     0 ?           -1 S        0   0:00 [xfslogd/1]
    2 12693     0     0 ?           -1 S        0   0:00 [xfslogd/2]
    2 12694     0     0 ?           -1 S        0   0:00 [xfslogd/3]
    2 12695     0     0 ?           -1 S        0   0:00 [xfsdatad/0]
    2 12696     0     0 ?           -1 S        0   0:00 [xfsdatad/1]
    2 12697     0     0 ?           -1 S        0   0:00 [xfsdatad/2]
    2 12698     0     0 ?           -1 S        0   0:00 [xfsdatad/3]
    2 12699     0     0 ?           -1 S        0   0:00 [xfsconvertd/0]
    2 12700     0     0 ?           -1 S        0   0:00 [xfsconvertd/1]
    2 12701     0     0 ?           -1 S        0   0:00 [xfsconvertd/2]
    2 12702     0     0 ?           -1 S        0   0:00 [xfsconvertd/3]
    2 12710     0     0 ?           -1 S        0   0:00 [jfsIO]
    2 12711     0     0 ?           -1 S        0   0:00 [jfsCommit]
    2 12712     0     0 ?           -1 S        0   0:00 [jfsCommit]
    2 12713     0     0 ?           -1 S        0   0:00 [jfsCommit]
    2 12714     0     0 ?           -1 S        0   0:00 [jfsCommit]
    2 12715     0     0 ?           -1 S        0   0:00 [jfsSync]
    1 13841 13841 13841 ?           -1 Ss    1000 249:23 ./boinc --daemon
    1 14479 14479 14479 ?           -1 Ss       0   0:10 /usr/lib/postfix/master
14479 14481 14479 14479 ?           -1 S      111   0:02 qmgr -l -t fifo -u
17136 16953 17136 17136 ?           -1 S        0  27:11 smbd -F
    1 17136 17136 17136 ?           -1 Ss       0   0:16 smbd -F
    1 17143 17143 17143 ?           -1 Ss       0  14:42 nmbd -D
17136 17145 17136 17136 ?           -1 S        0   0:00 smbd -F
    1 18572 18566 18566 ?           -1 S        0   0:03 rsync -auz -e ssh root@172.16.2.10:/www/* /md1200/www/Thursday/
18572 18616 18566 18566 ?           -1 S        0   0:02 ssh -l root 172.16.2.10 rsync --server --sender -ulogDtprze.iLsf . /www/*
13841 19071 13841 13841 ?           -1 SNl   1000  87:53 ../../projects/setiathome.berkeley.edu/setiathome-5.28.x86_64-pc-linux-gnu
13841 19072 13841 13841 ?           -1 SNl   1000  88:08 ../../projects/setiathome.berkeley.edu/setiathome-5.28.x86_64-pc-linux-gnu
13841 19073 13841 13841 ?           -1 SNl   1000  88:04 ../../projects/setiathome.berkeley.edu/setiathome-5.28.x86_64-pc-linux-gnu
13841 19074 13841 13841 ?           -1 SNl   1000  87:42 ../../projects/setiathome.berkeley.edu/setiathome-5.28.x86_64-pc-linux-gnu
    1 22633 22632 22632 ?           -1 SN     114   0:00 /usr/sbin/zabbix_agentd
22633 22635 22632 22632 ?           -1 SN     114 483:39 /usr/sbin/zabbix_agentd
22633 22636 22632 22632 ?           -1 SN     114  45:23 /usr/sbin/zabbix_agentd
22633 22637 22632 22632 ?           -1 SN     114  44:51 /usr/sbin/zabbix_agentd
22633 22638 22632 22632 ?           -1 SN     114  44:45 /usr/sbin/zabbix_agentd
22633 22639 22632 22632 ?           -1 SN     114  45:02 /usr/sbin/zabbix_agentd
22633 22640 22632 22632 ?           -1 SN     114  44:36 /usr/sbin/zabbix_agentd
22633 22641 22632 22632 ?           -1 SN     114   6:09 /usr/sbin/zabbix_agentd
14479 25203 14479 14479 ?           -1 S      111   0:00 pickup -l -t fifo -u -c
    1 27680 27680 27680 ?           -1 Ss     113  14:34 /usr/sbin/ntpd -p /var/run/ntpd.pid -g -u 113:122
  960 28801 28801 28801 ?           -1 Ss       0   0:00 sshd: root@pts/0
28801 28866 28866 28866 pts/0    29991 Ss       0   0:00 -bash
  343 29055   343   343 ?           -1 S<       0   0:19 udevd --daemon
28866 29991 29991 28866 pts/0    29991 S+       0   0:00 ssh 172.16.1.3
  960 29992 29992 29992 ?           -1 Ss       0   0:00 sshd: root@pts/1
29992 30057 30057 30057 pts/1    30109 Ss       0   0:00 -bash
30057 30109 30109 30057 pts/1    30109 R+       0   0:00 ps jax

```

### 2.6. 僵尸进程

zombie process

```

ps aux | awk '{ print $8 " " $2 }' | grep -w Z

```

### 2.7. 查找内存消耗最大的进程

```

[root@localhost ~]# ps aux --sort -rss | head
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root       531  0.3  1.5 358748 29468 ?        Ssl  08:50   0:00 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid
root      1042  0.1  1.0 574200 19552 ?        Ssl  08:50   0:00 /usr/bin/python2 -Es /usr/sbin/tuned -l -P
polkitd    491  0.0  0.6 613016 11916 ?        Ssl  08:50   0:00 /usr/lib/polkit-1/polkitd --no-debug
root      1046  0.0  0.5 424064 11420 ?        Ss   08:50   0:00 /usr/sbin/smbd --foreground --no-process-group
root       542  0.1  0.5 701996  9568 ?        Ssl  08:50   0:00 /usr/sbin/NetworkManager --no-daemon
root      1215  0.0  0.2 158924  5668 ?        Ss   08:51   0:00 sshd: www [priv]
root       755  0.0  0.2 102896  5492 ?        S    08:50   0:00 /sbin/dhclient -d -q -sf /usr/libexec/nm-dhcp-helper -pf /var/run/dhclient-wlp5s0.pid -lf /var/lib/NetworkManager/dhclient-13693dd0-b518-4662-bb00-3d6b39fda3f3-wlp5s0.lease -cf /var/lib/NetworkManager/dhclient-wlp5s0.conf wlp5s0
root       654  0.0  0.2  78812  4636 ?        Ss   08:50   0:00 /usr/sbin/wpa_supplicant -u -f /var/log/wpa_supplicant.log -c /etc/wpa_supplicant/wpa_supplicant.conf -P /var/run/wpa_supplicant.pid
root      1045  0.0  0.2 216420  4452 ?        Ssl  08:50   0:00 /usr/sbin/rsyslogd -n			

```

### 2.8. 指定输出项

```

[root@localhost ~]# ps -eo pid,ppid,%mem,%cpu,cmd --sort=-%mem | head
  PID  PPID %MEM %CPU CMD
 1457  1244  3.3  8.4 /usr/bin/python2 /usr/bin/dnf upgrade
  531     1  1.5  0.2 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid
 1042     1  1.0  0.0 /usr/bin/python2 -Es /usr/sbin/tuned -l -P
  491     1  0.6  0.0 /usr/lib/polkit-1/polkitd --no-debug
 1046     1  0.5  0.0 /usr/sbin/smbd --foreground --no-process-group
  542     1  0.5  0.1 /usr/sbin/NetworkManager --no-daemon
 1215  1044  0.2  0.0 sshd: www [priv]
 1542  1044  0.2  0.1 sshd: www [priv]

```

仅仅现实命令,不显示参数

```

[root@localhost ~]# ps -eo pid,ppid,%mem,%cpu,comm --sort=-%mem | head
  PID  PPID %MEM %CPU COMMAND
 1457  1244  3.4 19.7 dnf
  531     1  1.5  0.2 firewalld
 1042     1  1.0  0.0 tuned
  491     1  0.6  0.0 polkitd
 1046     1  0.5  0.0 smbd
  542     1  0.5  0.0 NetworkManager
 1215  1044  0.2  0.0 sshd
 1542  1044  0.2  0.0 sshd
  755   542  0.2  0.0 dhclient			

```

## 3. mpstat

```

# mpstat -P ALL 60
Linux 2.6.18-194.el5 (localhost)        09/20/2010

05:48:55 PM  CPU   %user   %nice    %sys %iowait    %irq   %soft  %steal   %idle    intr/s
05:49:55 PM  all   17.42    0.00    0.25    0.21    0.04    0.34    0.00   81.74   2622.21
05:49:55 PM    0    5.85    0.00    0.27    0.25    0.00    0.05    0.00   93.58   1000.50
05:49:55 PM    1    7.55    0.00    0.15    0.33    0.02    0.07    0.00   91.88      7.54
05:49:55 PM    2   13.64    0.00    0.23    0.03    0.00    0.10    0.00   86.00      0.00
05:49:55 PM    3   14.05    0.00    0.23    0.45    0.00    0.08    0.00   85.18      0.00
05:49:55 PM    4    7.72    0.00    0.20    0.28    0.00    0.05    0.00   91.74      9.59
05:49:55 PM    5    2.83    0.00    0.13    0.02    0.00    0.05    0.00   96.97      0.00
05:49:55 PM    6   11.79    0.00    0.22    0.28    0.02    0.25    0.00   87.45     90.90
05:49:55 PM    7   75.96    0.00    0.60    0.02    0.25    2.05    0.00   21.12   1513.67

05:49:55 PM  CPU   %user   %nice    %sys %iowait    %irq   %soft  %steal   %idle    intr/s
05:50:55 PM  all    8.49    0.00    0.85    0.25    0.03    0.21    0.00   90.17   2193.66
05:50:55 PM    0    2.33    0.00    0.28    0.18    0.00    0.02    0.00   97.18   1000.67
05:50:55 PM    1    2.05    0.00    0.27    0.55    0.02    0.03    0.00   97.08      8.60
05:50:55 PM    2    2.85    0.00    0.73    0.38    0.00    0.10    0.00   95.93      0.00
05:50:55 PM    3    2.67    0.00    2.18    0.12    0.00    0.02    0.00   95.02      0.00
05:50:55 PM    4    4.77    0.00    0.67    0.58    0.02    0.03    0.00   93.93     11.29
05:50:55 PM    5    1.63    0.00    1.42    0.13    0.00    0.02    0.00   96.80      0.00
05:50:55 PM    6    2.20    0.00    0.58    0.00    0.05    0.18    0.00   96.98    245.62
05:50:55 PM    7   49.41    0.00    0.63    0.08    0.17    1.28    0.00   48.42    927.50

05:50:55 PM  CPU   %user   %nice    %sys %iowait    %irq   %soft  %steal   %idle    intr/s
05:51:55 PM  all   36.61    0.00    0.46    0.19    0.06    0.64    0.00   62.03   3566.81
05:51:55 PM    0   25.53    0.00    0.43    0.03    0.00    0.23    0.00   73.77   1000.52
05:51:55 PM    1   17.64    0.00    0.33    0.28    0.02    0.12    0.00   81.61      7.75
05:51:55 PM    2   40.56    0.00    0.48    0.30    0.00    0.30    0.00   58.35      0.00
05:51:55 PM    3   46.88    0.00    0.52    0.15    0.00    0.27    0.00   52.18      0.00
05:51:55 PM    4   29.60    0.00    0.45    0.52    0.00    0.22    0.00   69.21      8.99
05:51:55 PM    5   10.72    0.00    0.37    0.17    0.00    0.12    0.00   88.63      0.00
05:51:55 PM    6   40.83    0.00    0.48    0.05    0.03    0.35    0.00   58.25    111.15
05:51:55 PM    7   81.11    0.00    0.63    0.02    0.42    3.57    0.00   14.25   2438.40

```

## 4. pid

### 4.1. pgrep, pkill - look up or signal processes based on name and other attributes

pgrep

```

$ pgrep lighttpd
6045

```

pkill

```

$ sudo pkill lighttpd

```

kill TTY

```

[root@development ~]# w
 16:07:37 up 1 day,  6:23,  6 users,  load average: 0.00, 0.06, 0.26
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
develope pts/0    192.168.3.33     16:01    5:45   0.01s  0.01s -bash
jeecen   pts/1    192.168.3.129    09:30    7:40   0.00s  0.00s -bash
jeson    pts/2    192.168.3.101    11:27   42:47   0.03s  0.03s -bash
develope pts/3    192.168.3.31     16:03    4:33   0.00s  0.00s -bash
root     pts/5    172.16.0.1       14:55    1:03m  0.01s  0.01s -bash
root     pts/6    172.16.0.1       15:47    0.00s  0.03s  0.00s w
[root@development ~]# pkill -kill -t pts/3

```

### 4.2. pidof -- find the process ID of a running program.

```

# pidof httpd
31935 21542 15010 15009 15008 15007 15006 15005 15004 15003 6068 6042 6041 6040 3284

# pidof -s httpd
31935

```

## 5. jobs

### 5.1. &

usage: command &

```

$ grep -r 'neo' / > result &
[1] 10414

```

### 5.2. Ctrl + Z

```

vim

$ vim

[2]+  Stopped                 vim

mutt

$ mutt

[3]+  Stopped                 mutt

```

### 5.3. jobs

```

$ jobs
[1]   Running                 grep -r 'neo' / > result &
[2]-  Stopped                 vim
[3]+  Stopped                 mutt

```

### 5.4. fg / bg

usage: fg [job_spec]

```

$ fg 2

```

usage: bg [job_spec ...]

```

$ cp -r /usr/ /tmp/
Ctrl + Z
[1]+  Stopped                 cp -r /usr/ /tmp/

$ bg
[1]+ cp -r /usr/ /tmp/ &

$ fg
cp -r /usr/ /tmp/

```

### 5.5. nohup - run a command immune to hangups, with output to a non-tty

```

nohup command > myout.file 2>&1 &
nohup command >/dev/null 2>/dev/null &
nohup command &>/dev/null

```

You may using 'jobs' to display task.

and using 'fg %n' to close that.

### 5.6. wait 等待后台任务运行结束

```

neo@MacBook-Pro ~ % sleep 10 &
[1] 2967

neo@MacBook-Pro ~ % wait
[1]  + 2967 done       sleep 10			

```

wait 将一只停留，等待 sleep 10 运行完毕。

## 6. ionice - get/set program io scheduling class and priority

```

EXAMPLES
       # ionice -c3 -p89

       Sets process with PID 89 as an idle io process.

       # ionice -c2 -n0 bash

       Runs ’bash’ as a best-effort program with highest priority.

       # ionice -p89

       Returns the class and priority of the process with PID 89.

```

## 7. Utilities for managing processes on your system

CentOS 7 默认没有安装 psmisc

```

[root@localhost ~]# yum install -y psmisc
[root@localhost ~]# rpm -ql psmisc
/usr/bin/killall
/usr/bin/peekfd
/usr/bin/prtstat
/usr/bin/pstree
/usr/bin/pstree.x11
/usr/sbin/fuser

```

### 7.1. pstree - display a tree of processes

```

$ pstree
init─┬─NetworkManager
     ├─NetworkManagerD
     ├─acpid
     ├─atd
     ├─avahi-daemon───avahi-daemon
     ├─console-kit-dae───61*[{console-kit-dae}]
     ├─cron
     ├─cupsd
     ├─dbus-daemon
     ├─dd
     ├─dhcdbd
     ├─dhclient3
     ├─exim4
     ├─gconfd-2
     ├─gdm───gdm───kill
     ├─5*[getty]
     ├─gnump3d
     ├─hald───hald-runner─┬─hald-addon-acpi
     │                    └─hald-addon-inpu
     ├─hcid───2*[bluetoothd-serv]
     ├─klogd
     ├─lighttpd───2*[php-cgi───4*[php-cgi]]
     ├─login───bash───pstree
     ├─nmbd
     ├─postgres───4*[postgres]
     ├─rsync
     ├─smbd───2*[smbd]
     ├─sshd
     ├─syslogd
     ├─system-tools-ba
     ├─udevd
     ├─xinetd
     └─xinit─┬─Xorg
             └─sh───gksu───xfailsafedialog

```

查看 PID

```

# pstree -p 3158
sshd(3158)─┬─sshd(9409)───bash(9411)
           ├─sshd(15241)───bash(15247)
           ├─sshd(15243)───bash(15275)
           ├─sshd(15245)───bash(15303)───pstree(30050)
           └─sshd(22786)───bash(22788)

```

### 7.2. fuser - identify processes using files or sockets

```

[root@localhost ~]# fuser -u /usr/sbin/sshd
/usr/sbin/sshd:       3549e(root) 13275e(root) 13426e(root) 13721e(root) 13919e(root) 32616e(root)			

```

## 8. /proc 目录与进程的关系

### 8.1. /proc/进程 ID

每个进程会对应一个/proc 下的一个目录: /proc/进程 ID

```

[root@www.netkiller.cn ~]# ls /proc/
1     122   1449  18    1891  1942  20    2306  2507  36    44   63   75  96           ioports       schedstat
10    123   1450  180   19    1943  2015  2308  2509  37    45   631  76  97           ipmi          scsi
100   124   1451  1802  190   1944  2016  2327  2519  38    46   632  77  976          irq           self
101   125   1452  182   1912  1945  203   2354  2521  3892  47   633  78  98           kallsyms      slabinfo
102   126   1453  1825  1921  1946  2057  2359  2526  3893  48   634  79  99           kcore         softirqs
103   127   1454  183   1922  1947  2060  2368  26    39    49   635  8   acpi         keys          stat
104   128   1455  184   1923  1948  2077  2370  27    3918  5    636  80  asound       key-users     swaps
105   129   1456  1843  1924  1949  2094  2372  2725  3966  50   637  81  buddyinfo    kmsg          sys
1057  13    1457  185   1925  1950  21    2395  2727  3980  51   638  82  bus          kpagecount    sysrq-trigger
106   1368  1458  1852  1926  1951  2109  24    2792  4     52   639  83  cgroups      kpageflags    sysvipc
107   14    1459  1858  1927  1952  2118  2465  28    40    53   64   84  cmdline      loadavg       timer_list
108   1437  146   186   1928  1953  2132  2466  2804  4056  532  65   85  cpuinfo      locks         timer_stats
109   1438  1460  187   1930  1954  2159  2467  2805  4085  54   66   86  crypto       mdstat        tty
11    1439  1461  188   1931  1955  22    2470  29    4087  544  67   87  devices      meminfo       uptime
110   1440  1462  1880  1932  1956  2218  2476  3     41    55   68   88  diskstats    misc          version
111   1441  1463  1881  1934  1957  2233  2489  30    42    56   69   89  dma          modules       vmallocinfo
112   1442  147   1882  1935  1958  2236  2493  31    43    57   7    9   driver       mounts        vmstat
113   1443  15    1883  1936  1959  2241  2495  3100  434   58   70   90  execdomains  mtd           zoneinfo
114   1444  1547  1884  1937  1962  2247  25    32    435   59   71   91  fb           mtrr
115   1445  16    1885  1938  1974  2251  2502  33    436   6    72   92  filesystems  net
116   1446  17    1886  1939  1985  2267  2503  3387  437   60   721  93  fs           pagetypeinfo
117   1447  177   1887  1940  1986  2293  2505  34    438   61   73   94  interrupts   partitions
12    1448  1786  189   1941  2     23    2506  35    439   62   74   95  iomem        sched_debug

```

### 8.2. /proc/*/fd/ 进程所打开的文件

查看进程所打开的文件

```

[root@www.netkiller.cn ~]# ps ax | grep rsyslogd
 2076 ?        Sl     0:00 /sbin/rsyslogd -i /var/run/syslogd.pid -c 5
12774 pts/0    S+     0:00 grep rsyslogd

[root@www.netkiller.cn ~]# ls -l /proc/2076/fd
total 0
lrwx------ 1 root root 64 May  9 18:02 0 -> socket:[13103]
l-wx------ 1 root root 64 May  9 18:02 1 -> /var/log/messages
l-wx------ 1 root root 64 May  9 18:02 2 -> /var/log/cron
lr-x------ 1 root root 64 May  9 18:02 3 -> /proc/kmsg
l-wx------ 1 root root 64 May  9 18:02 4 -> /var/log/secure

```

## 第 17 章 Permission 权限管理

## 1. User 用户管理

### 1.1. 添加用户

```

$ adduser neo			

```

```

## 添加一个账号和 root 有一样的权限
useradd -o -u 0 -g 0  admin

## 指定家目录和 shell
useradd -o -u 0 -g 0  -d /root -s /bin/bash admin

## 添加 root 用户并且设置密码
useradd -o -u 0 -g 0 admin
echo redhat | passwd admin --stdin
// 上面两条同下面一条
useradd -o -u 0 -g 0 -p $(openssl passwd -1 redhat@@neo) admin

## 添加普通用户并且设置密码
useradd -p $(openssl passwd -1 redhat@@admin) admin

##  添加普通用户指定其第二用户组
useradd -G root  -p $(openssl passwd -1 redhat@@admin) admin			

```

### 1.2. 删除用户

remove an existed user, but keeping directory /home/neo

```

$ userdel neo			

```

delete user's directory under /home when removing an existed user

```

$ userdel -r neo

```

### 1.3. 修改用户组

#### usermod - modify a user account

```

usermod -G group -a user

[root@scientific ~]# groupadd vm
[root@scientific ~]# adduser xen
[root@scientific ~]# usermod -G vm -a xen
[root@scientific ~]# usermod -G vm -a kvm
[root@scientific ~]# id xen
uid=501(xen) gid=502(xen) groups=502(xen),501(vm)

```

将 www 加入 wheel 组，www 用户可以使用 sudo 命令

```

[root@localhost ~]# usermod -aG wheel www

[www@localhost ~]$ id www
uid=80(www) gid=80(www) groups=80(www),10(wheel)			

```

### 1.4. 账号加锁与解锁

lock / unlock

```

passwd -l neo

```

```

passwd -u neo

```

#### 1.4.1. /etc/passwd

```

[root@test ~]# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
news:x:9:13:news:/etc/news:
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
vcsa:x:69:69:virtual console memory owner:/dev:/sbin/nologin
pcap:x:77:77::/var/arpwatch:/sbin/nologin
rpc:x:32:32:Portmapper RPC user:/:/sbin/nologin
mailnull:x:47:47::/var/spool/mqueue:/sbin/nologin
smmsp:x:51:51::/var/spool/mqueue:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
nfsnobody:x:4294967294:4294967294:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
avahi:x:70:70:Avahi daemon:/:/sbin/nologin
haldaemon:x:68:68:HAL daemon:/:/sbin/nologin
avahi-autoipd:x:100:102:avahi-autoipd:/var/lib/avahi-autoipd:/sbin/nologin
neo:x:500:500::/home/neo:/bin/bash
mysql:x:501:501::/home/mysql:/bin/bash

```

## 2. Group

### 2.1. Add a new group

```

$ groupadd newgroup

```

### 2.2. Add a user to the group

```

$ groupadd mygroup
$ sudo usermod -a -G mygroup user

```

### 2.3. /etc/group

```

[root@test ~]# cat /etc/group
root:x:0:root
bin:x:1:root,bin,daemon
daemon:x:2:root,bin,daemon
sys:x:3:root,bin,adm
adm:x:4:root,adm,daemon
tty:x:5:
disk:x:6:root
lp:x:7:daemon,lp
mem:x:8:
kmem:x:9:
wheel:x:10:root
mail:x:12:mail
news:x:13:news
uucp:x:14:uucp
man:x:15:
games:x:20:
gopher:x:30:
dip:x:40:
ftp:x:50:
lock:x:54:
nobody:x:99:
users:x:100:
nscd:x:28:
floppy:x:19:
vcsa:x:69:
pcap:x:77:
utmp:x:22:
utempter:x:35:
slocate:x:21:
audio:x:63:
rpc:x:32:
mailnull:x:47:
smmsp:x:51:
ecryptfs:x:101:
sshd:x:74:
rpcuser:x:29:
nfsnobody:x:4294967294:
dbus:x:81:
avahi:x:70:
haldaemon:x:68:
avahi-autoipd:x:102:
neo:x:500:
mysql:x:501:

```

### 2.4. gpasswd - administer /etc/group and /etc/gshadow

当前用户添加到 docker 组

```

# sudo gpasswd -a ${USER} docker

```

添加 jenkins 用户到 docker 组

```

[root@localhost ~]# gpasswd -a jenkins docker
Adding user jenkins to group docker

[root@localhost ~]# cat /etc/group | grep ^docker
docker:x:993:jenkins			

```

## 3. umask

```

[root@development ~]# umask
0022
[root@development ~]# umask -S
u=rwx,g=rx,o=rx		

```

设置

```

umask 002		

```

## 4. Access Permissions

### 4.1. chown - change file owner and group

chown - change file owner and group

```

[root@test ~]# touch test
[root@test ~]# adduser neo
[root@test ~]# chown neo test
[root@test ~]# ll test
-rw-r--r-- 1 neo root 0 Apr 19 18:15 test

```

### 4.2. chgrp - change group ownership

chgrp - change group ownership

```

# chgrp daemon -R *

# ll
drwxrwxr-x  3 neo  daemon      4096 Apr 16 18:23 user 		

```

### 4.3. chmod - change file access permissions

option

```

u = user
g = group
o = other
a = all

r = 4
w = 2	
x = 1

```

```

[root@test ~]# ll test
-rwxr--r-- 1 neo root 0 Apr 19 18:15 test
[root@test ~]# chmod g=x test
[root@test ~]# ll test
-rwx--xr-- 1 neo root 0 Apr 19 18:15 test
[root@test ~]# chmod go+w test
[root@test ~]# ll test
-rwx-wxrw- 1 neo root 0 Apr 19 18:15 test
[root@test ~]# chmod u-wx test
[root@test ~]# ll test
-r---wxrw- 1 neo root 0 Apr 19 18:15 test
[root@test ~]# chmod u=rwx test
[root@test ~]# ll test
-rwx-wxrw- 1 neo root 0 Apr 19 18:15 test
[root@test ~]# chmod a=rwx test
[root@test ~]# ll test
-rwxrwxrwx 1 neo root 0 Apr 19 18:15 test

```

## 5. chattr - change file attributes on a Linux second extended file system

```

[root@development ~]# chattr +i /etc/passwd
[root@development ~]# lsattr /etc/passwd
----i-------- /etc/passwd		

```

```

[root@development ~]# chattr -i /etc/passwd
[root@development ~]# lsattr /etc/passwd
------------- /etc/passwd

```

## 6. su - run a shell with substitute user and group IDs

Change the effective user id and group id to that of USER.

```

[neo@development ~]$ su - root

```

```

[neo@development ~]$ su root -c "rm -rf linux/"	

```

```

su - www -c "/srv/apache-tomcat-www/bin/startup.sh"
su - www -c "/srv/apache-tomcat-m/bin/startup.sh"

su - www -c "/srv/java/bin/java -jar /www/netkiller.cn/api.netkiller.cn/api.netkiller.cn-0.0.2-SNAPSHOT.jar &"

```

## 7. runuser - run a command with substitute user and group ID

```

[root@netkiller ~]# runuser -l www -c 'ulimit -SHa'
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 63473
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 65535
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 4096
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited

```

## 8. sudo, sudoedit - execute a command as another user

```

debian:~# apt-get install sudo		

```

### 8.1. /etc/sudoers

sudo 的配置文件是/etc/sudoers,visudo 修改时会锁住 sudoers 文件,保存修改到临时文件,然后检查文件格式,确保正确后才会覆盖 sudoers 文件. 必须保证 sudoers 格式正确,否则 sudo 将无法运行.

/etc/sudoers

```

# /etc/sudoers
#
# This file MUST be edited with the 'visudo' command as root.
#
# See the man page for details on how to write a sudoers file.
#

Defaults        env_reset

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL) ALL

# Uncomment to allow members of group sudo to not need a password
# (Note that later entries override this, so you might need to move
# it further down)
%sudo ALL=NOPASSWD: ALL

```

sudo group

```

neo@debian:/etc/mysql$ cat /etc/group | grep 'sudo'
sudo:x:27:neo		

```

### 8.2. /etc/sudoers

visudo 调用的默认编辑器是 vi,如果要临时使用其他编辑器,在该命令前加上 EDITOR 环境变量即可.

```

[root@netkiller ~]# EDITOR=vim visudo

```

### 8.3. 设置示例

```

>1 允许 neo 用户从任何主机登录,以 root 的身份执行/usr/sbin/useradd 命令

neo   ALL=(root) /usr/sbin/useradd 

>2 允许 jam 用户从任何主机登录,以 root 的身份无密码使用 sudo 执行/sbin/iptables -n -t filter -L 

jam   ALL=(ALL) NOPASSWD: /sbin/iptables -n -t filter -L 

>3 neo 用户从任何主机登录,以 root 的身份执行自定义命令链里面的命令

Cmnd_Alias USERCOMMAND = /sbin/route,/sbin/ifconfig,/bin/ping,/sbin/dhclient,/usr/bin/net,/sbin/iptables,/usr/bin/rfcomm,/usr/bin/wvdial,/sbin/iwconfig
neo   ALL=(root)    USERCOMMAND

```

### 8.4. NOPASSWD

ubuntu NOPASSWD sudo 的时候不需要输入密码

组

```

%admin ALL=(ALL)ALL
改为
%admin ALL=(ALL) NOPASSWD: NOPASSWD: ALL	

```

用户

```

www localhost=NOPASSWD: /bin/cat, /bin/ls			

```

### 8.5. 允许或禁止命令

命令前面加‘!’可以禁止用户运行该命令

```

neo ALL = （root） /bin/mount, /bin/umount, !/bin/mount /data0
dba ALL = /bin/mount /u0[1-5], /bin/umount /u0[1-5]

```

### 8.6. Cmnd_Alias 用法

Cmnd_Alias 定义命令别名

```

Cmnd_Alias WEBMASTER = /srv/nginx/sbin/nginx, /srv/php/sbin/php-fpm, !/srv/mysql/bin/mysql
www localhost = NETWORKING, SERVICES, DELEGATING, PROCESSES, WEBMASTER

```

自定义用户组(以所有的身份)执行自定义的命令链里的命令

```

Cmnd_Alias USERCOMMAND = /sbin/route,/sbin/ifconfig,/bin/ping,/usr/sbin/mtr,/bin/traceroute,/usr/bin/top,/bin/df,/usr/bin/free,/usr/bin/du,/bin/ls,/bin/date,/usr/bin/less
User_Alias    ADMINS = user1, user2
ADMINS	ALL=(ALL)	USERCOMMAND			

```

### 8.7. wheel 组

```

## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
neo     ALL=(ALL)       ALL
%wheel  ALL=(ALL)       ALL

```

将用户加入到 wheel 组

```

[root@localhost ~]# usermod -aG wheel www

```

### 8.8. 注意事项

```

1 修改 sudo 记录密码的时间

Defaults:用户名 timestamp_timeout=20
eg:
Defaults:redhat timestamp_timeout=20

2 默认 sudo 命令只能在 tty 上执行,注释掉下面选项可以使程序调用 sudo 命令

Defaults    requiretty			

```

## 9. ACL - Access Control List

```

$ sudo modprobe loop
$ dd if=/dev/zero of=file bs=1k count=100
$ sudo losetup /dev/loop0 file
$ sudo mkfs.ext3 /dev/loop0
$ sudo mkdir /mnt/loop	
$ sudo mount -o rw,acl /dev/loop0 /mnt/loop/
$ sudo chown neo.neo -R /mnt/loop
$ cd /mnt/loop/	

```

### 9.1. getfacl - get file access control lists

UGO

```

$ touch file
$ ls -l file
-rw-r--r-- 1 neo neo 0 2008-12-22 15:28 file

```

ACL

```

$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
group::r--
other::r--		

```

display the default access control list only

```

neo@netkiller:/mnt/loop$ getfacl  dir
# file: dir
# owner: neo
# group: neo
user::rwx
group::r-x
other::r-x
default:user::rwx
default:user:svnroot:rw-
default:group::r-x
default:group:nagios:rw-
default:mask::rwx
default:other::r-x

neo@netkiller:/mnt/loop$ getfacl -d dir
# file: dir
# owner: neo
# group: neo
user::rwx
user:svnroot:rw-
group::r-x
group:nagios:rw-
mask::rwx
other::r-x

```

recurse into subdirectories

```

$ getfacl -R dir
# file: dir
# owner: neo
# group: neo
user::rwx
group::r-x
other::r-x
default:user::rwx
default:user:svnroot:rw-
default:group::r-x
default:group:nagios:rw-
default:mask::rwx
default:other::r-x

# file: dir/file1
# owner: neo
# group: neo
user::rw-
user:svnroot:rw-
group::r-x                      #effective:r--
group:nagios:rw-
mask::rw-
other::r--		

```

### 9.2. setfacl - set file access control lists

#### 9.2.1. set

add a user svnroot to file

```

neo@netkiller:/mnt/loop$ setfacl -m u:svnroot:rw file

```

if you can see a '+' at last, it's successed

```

$ ls -l file
-rw-rw-r--+ 1 neo neo 0 2008-12-22 15:44 file		

```

let me see acl.

```

neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:svnroot:rw-
group::r--
mask::rw-
other::r--

```

add a user cvsroot to file again

```

neo@netkiller:/mnt/loop$ setfacl -m u:cvsroot:rw file
neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:cvsroot:rw-
user:svnroot:rw-
group::r--
mask::rw-
other::r--		

```

add a user and group for that

```

neo@netkiller:/mnt/loop$ setfacl -m u:gnump3d:rwx,g:nagios:r file
neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:gnump3d:rwx
user:cvsroot:rw-
user:svnroot:rw-
group::r--
group:nagios:r--
mask::rwx
other::r--		

```

modify the current ACL(s) of file(s)

```

neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:svnroot:rw-
group::r--
mask::rw-
other::r--

neo@netkiller:/mnt/loop$ setfacl -m u:svnroot:r-x file
neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:svnroot:r-x
group::r--
mask::r-x
other::r--			

```

#### 9.2.2. default

```

neo@netkiller:/mnt/loop$ setfacl -d -m u:svnroot:rw dir/
neo@netkiller:/mnt/loop$ getfacl dir/
# file: dir
# owner: neo
# group: neo
user::rwx
group::r-x
other::r-x
default:user::rwx
default:user:svnroot:rw-
default:group::r-x
default:mask::rwx
default:other::r-x

neo@netkiller:/mnt/loop$ setfacl -d -m g:nagios:rw dir/
neo@netkiller:/mnt/loop$ getfacl dir/
# file: dir
# owner: neo
# group: neo
user::rwx
group::r-x
other::r-x
default:user::rwx
default:user:svnroot:rw-
default:group::r-x
default:group:nagios:rw-
default:mask::rwx
default:other::r-x			

```

the file1 will inherit acl by default.

```

neo@netkiller:/mnt/loop$ touch dir/file1
neo@netkiller:/mnt/loop$ getfacl dir/file1
# file: dir/file1
# owner: neo
# group: neo
user::rw-
user:svnroot:rw-
group::r-x                      #effective:r--
group:nagios:rw-
mask::rw-
other::r--

```

#### 9.2.3. remove

remove entries from the ACL(s) of file(s)

```

neo@netkiller:/mnt/loop$ setfacl -x u:cvsroot file
neo@netkiller:/mnt/loop$ setfacl -x g:nagios file
neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
user:gnump3d:rwx
user:svnroot:rw-
group::r--
mask::rwx
other::r--

```

remove all extended ACL entries

```

neo@netkiller:/mnt/loop$ setfacl -b file
neo@netkiller:/mnt/loop$ getfacl file
# file: file
# owner: neo
# group: neo
user::rw-
group::r--
	other::r--		

```

#### 9.2.4. backup and restore

backup

```

$ getfacl -R dir > dir.acl		

```

restore

```

$ setfacl --restore dir.acl		

```

## 第 18 章 /etc

## 1. getent 用来察看系统的数据库中的相关记录

支持数据库

```

ahosts ahostsv4 ahostsv6 aliases ethers group gshadow hosts initgroups
netgroup networks passwd protocols rpc services shadow    	

```

### 1.1. 主机名

查找主机名

```

[root@localhost ~]# getent hosts localhost
::1             localhost localhost.localdomain localhost6 localhost6.localdomain6		

[root@localhost ~]# getent hosts localhost.localdomain
::1             localhost localhost.localdomain localhost6 localhost6.localdomain6	

```

### 1.2. 用户组

查看用户

```

[root@localhost ~]# getent passwd halt
halt:x:7:0:halt:/sbin:/sbin/halt		

[root@localhost ~]# getent passwd `whoami`
root:x:0:0:root:/root:/bin/bash	

```

通过 UID 查看用户信息

```

[root@localhost ~]# getent passwd 65534
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin			

```

判定用户组 test 是否存在：如果存在就退出，不存在就创建

```

[root@localhost ~]# getent group test || groupadd test

[root@localhost ~]# getent group zabbix > /dev/null || groupadd -r zabbix
[root@localhost ~]# getent passwd zabbix > /dev/null || useradd -r -g zabbix -s /sbin/nologin -c "Zabbix Monitoring System" zabbix	

```

### 1.3. 查看端口

```

[root@localhost ~]# getent services 22
ssh                   22/tcp
[root@localhost ~]# getent services 80
http                  80/tcp www www-http
[root@localhost ~]# getent services 443
https                 443/tcp			

```

### 1.4. shadow 密码

```

[root@localhost ~]# getent shadow root
root:$6$PlAA9lHTPmwOO8TL$1cjrer572Zbw.1nR4TvWRZRdRFuNgNxJayh4snUtqGZ6brTZNOyzWHfFUFptXUGjDgxqdrAtweeIuWbvbmtuQ1::0:99999:7:::

[root@localhost ~]# getent shadow sshd
sshd:!!:18229::::::			

```

## 2. /etc/inputrc

键盘映射配置文件,用户不同终端下的键盘定义

```

% grep -v '^#' /etc/inputrc | grep -v "^$"
set input-meta on
set output-meta on
$if mode=emacs
"\e[1~": beginning-of-line
"\e[4~": end-of-line
"\e[3~": delete-char
"\e[2~": quoted-insert
"\e[1;5C": forward-word
"\e[1;5D": backward-word
"\e[5C": forward-word
"\e[5D": backward-word
"\e\e[C": forward-word
"\e\e[D": backward-word
$if term=rxvt
"\e[8~": end-of-line
"\eOc": forward-word
"\eOd": backward-word
$endif
$endif 

```

## 3. /etc/shells

系统有效地登陆 Shell

```

% cat /etc/shells 
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh         

```

## 第 19 章 kickstart

摘要

Kickstart 无人值守安装

## 1. install kickstart

```
# yum search kickstart
Loaded plugins: refresh-packagekit
========================================================================== Matched: kickstart ==========================================================================
pykickstart.noarch : A python library for manipulating kickstart files
system-config-kickstart.noarch : A graphical interface for making kickstart files
sl-revisor-configs.noarch : Kickstart and config files for creating your own SL Spins

# yum install system-config-kickstart

```

## 2. ks.cfg

```
# cat ks.cfg
#platform=x86, AMD64, or Intel EM64T
#version=DEVEL
# Firewall configuration
firewall --disabled
# Install OS instead of upgrade
install
# Use CDROM installation media
cdrom
# Root password
rootpw --iscrypted $1$5T/3LoJq$0B3n9sC.NuuGslFqkWDSw/
# Network information
network  --bootproto=dhcp --device=eth0 --onboot=on
# System authorization information
auth  --useshadow  --passalgo=md5
# Use graphical install
graphical
firstboot --disable
# System keyboard
keyboard us
# System language
lang en_US
# SELinux configuration
selinux --disabled
# Installation logging level
logging --level=info

# System timezone
timezone  Asia/Harbin
# System bootloader configuration
bootloader --location=mbr
# Clear the Master Boot Record
zerombr
# Partition clearing information
clearpart --all --initlabel
# Disk partitioning information
part /boot --fstype="ext4" --size=500
part swap --fstype="swap" --size=32000
part / --fstype="ext4" --size=50000
part /opt --fstype="ext4" --grow --size=1

```

## 3. boot 参数

```

boot:

linux ks=floppy
linux ks=floppy:/<path>
linux ks=hd:fd0:/ks.cfg

linux ks=hd:<device>:/<file>
linux ks=hd:sda3:/ks.cfg

linux ks=nfs:<server:>/<path> ksdevice=eth1
linux ks=nfs:<server>:/<path>

linux ks=http://<server>/<path>

linux ks=file:/<file>
linux ks=cdrom:/<path>
linux ks=cdrom:/ks.cfg

USB:

linux install ks=hd:sda:/anaconda-ks.cfg
or
linux install ks=hd:sda1:/anaconda-ks.cfg

```

## 第 20 章 System Utilities 配置工具

## 1. nmtui - Text User Interface for controlling NetworkManager

```

# yum install NetworkManager-tui
# nmtui		

```

```

        ┌─┤ NetworkManager TUI ├──┐
        │                         │ 
        │ Please select an option │ 
        │                         │ 
        │ Edit a connection       │ 
        │ Activate a connection   │ 
        │ Set system hostname     │ 
        │                         │ 
        │ Quit                    │ 
        │                         │ 
        │                    <OK> │ 
        │                         │ 
        └─────────────────────────┘ 

```

```

# nmtui
# nmtui edit eno16777736
# nmtui connect eno1677773

```

## 2. CentOS 6

setup

timeconfig

system-config-cluster

system-config-httpd

system-config-nfs

system-config-samba

### 2.1. system-config-date

```
				┌────────┤ Timezone Selection ├────────┐
				│ │
				│ Select the timezone for the system. │
				│ │
				│ Asia/Bahrain ↑ │
				│ Asia/Baku ▒ │
				│ Asia/Bangkok ▒ │
				│ Asia/Beirut ▮ │
				│ Asia/Bishkek ▒ │
				│ Asia/Brunei ▒ │
				│ Asia/Choibalsan ▒ │
				│ Asia/Chongqing ↓ │
				│ │
				│ [*] System clock uses UTC │
				│ │
				│ ┌────┐ ┌────────┐ │
				│ │ OK │ │ Cancel │ │
				│ └────┘ └────────┘ │
				│ │
				│ │
				└──────────────────────────────────────┘

```

### 2.2. system-config-firewall

```
				┌───────────┤ Firewall Configuration ├───────────┐
				│ │
				│ A firewall protects against unauthorized │
				│ network intrusions. Enabling a firewall blocks │
				│ all incoming connections. Disabling a firewall │
				│ allows all connections and is not recommended. │
				│ │
				│ Firewall: [*] Enabled │
				│ │
				│ ┌────┐ ┌───────────┐ ┌────────┐ │
				│ │ OK │ │ Customize │ │ Cancel │ │
				│ └────┘ └───────────┘ └────────┘ │
				│ │
				│ │
				└────────────────────────────────────────────────┘

```

### 2.3. system-config-securitylevel

```
				┌────────────┤ Firewall Configuration ├────────────┐
				│ │
				│ A firewall protects against unauthorized │
				│ network intrusions. Enabling a firewall blocks │
				│ all incoming connections. Disabling a firewall │
				│ allows all connections and is not recommended. │
				│ │
				│ Security Level: ( ) Enabled (*) Disabled │
				│ │
				│ SELinux: Enforcing │
				│ Permissive │
				│ Disabled │
				│ │
				│ ┌────┐ ┌───────────┐ ┌────────┐ │
				│ │ OK │ │ Customize │ │ Cancel │ │
				│ └────┘ └───────────┘ └────────┘ │
				│ │
				│ │
				└──────────────────────────────────────────────────┘

```

### 2.4. system-config-language

```
				┌──────────┤ Language Selection ├──────────┐
				│ │
				│ Select the language for the system. │
				│ │
				│ English (Hong Kong) ↑ │
				│ English (India) ▒ │
				│ English (Ireland) ▮ │
				│ English (New Zealand) ▒ │
				│ English (Philippines) ▒ │
				│ English (Singapore) ▒ │
				│ English (South Africa) ▒ │
				│ English (USA) ↓ │
				│ │
				│ ┌────┐ ┌────────┐ │
				│ │ OK │ │ Cancel │ │
				│ └────┘ └────────┘ │
				│ │
				│ │
				└──────────────────────────────────────────┘

```

### 2.5. system-config-keyboard

```
				┌─────────────┤ Keyboard Selection ├──────────────┐
				│ │
				│ Select the appropriate keyboard for the system. │
				│ │
				│ Slovenian ↑ │
				│ Serbian ▒ │
				│ Serbian (latin) ▒ │
				│ Swedish ▒ │
				│ Turkish ▒ │
				│ Ukrainian ▮ │
				│ United Kingdom ▒ │
				│ U.S. English ↓ │
				│ │
				│ ┌────┐ ┌────────┐ │
				│ │ OK │ │ Cancel │ │
				│ └────┘ └────────┘ │
				│ │
				│ │
				└─────────────────────────────────────────────────┘

```

### 2.6. system-config-network

system-config-network

```

          ┌──────┤ Select Action ├──────┐
          │                             │
          │    Device configuration     │
          │    DNS configuration        │
          │                             │
          │                             │
          │                             │
          │  ┌───────────┐   ┌──────┐   │
          │  │ Save&Quit │   │ Quit │   │
          │  └───────────┘   └──────┘   │
          │                             │
          │                             │
          └─────────────────────────────┘

```

system-config-network-cmd

```
				[root@r910 ~]# system-config-network-cmd
				DeviceList.Ethernet.eth0.AllowUser=False
				DeviceList.Ethernet.eth0.BootProto=dhcp
				DeviceList.Ethernet.eth0.Device=eth0
				DeviceList.Ethernet.eth0.DeviceId=eth0
				DeviceList.Ethernet.eth0.HardwareAddress=78:2B:CB:3B:4B:74
				DeviceList.Ethernet.eth0.IPv6Init=False
				DeviceList.Ethernet.eth0.NMControlled=True
				DeviceList.Ethernet.eth0.OnBoot=False
				DeviceList.Ethernet.eth0.Type=Ethernet
				DeviceList.Ethernet.eth1.AllowUser=False
				DeviceList.Ethernet.eth1.BootProto=dhcp
				DeviceList.Ethernet.eth1.Device=eth1
				DeviceList.Ethernet.eth1.DeviceId=eth1
				DeviceList.Ethernet.eth1.HardwareAddress=78:2B:CB:3B:4B:76
				DeviceList.Ethernet.eth1.IPv6Init=False
				DeviceList.Ethernet.eth1.NMControlled=True
				DeviceList.Ethernet.eth1.OnBoot=False
				DeviceList.Ethernet.eth1.Type=Ethernet
				DeviceList.Ethernet.eth2.AllowUser=False
				DeviceList.Ethernet.eth2.BootProto=dhcp
				DeviceList.Ethernet.eth2.Device=eth2
				DeviceList.Ethernet.eth2.DeviceId=eth2
				DeviceList.Ethernet.eth2.HardwareAddress=78:2B:CB:3B:4B:78
				DeviceList.Ethernet.eth2.IPv6Init=False
				DeviceList.Ethernet.eth2.NMControlled=True
				DeviceList.Ethernet.eth2.OnBoot=False
				DeviceList.Ethernet.eth2.Type=Ethernet
				DeviceList.Ethernet.eth3.AllowUser=False
				DeviceList.Ethernet.eth3.BootProto=dhcp
				DeviceList.Ethernet.eth3.Device=eth3
				DeviceList.Ethernet.eth3.DeviceId=eth3
				DeviceList.Ethernet.eth3.HardwareAddress=78:2B:CB:3B:4B:7A
				DeviceList.Ethernet.eth3.IPv6Init=False
				DeviceList.Ethernet.eth3.NMControlled=True
				DeviceList.Ethernet.eth3.OnBoot=False
				DeviceList.Ethernet.eth3.Type=Ethernet
				HardwareList.Ethernet.eth3.Card.ModuleName=bnx2
				HardwareList.Ethernet.eth3.Description=Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet
				HardwareList.Ethernet.eth3.Name=eth3
				HardwareList.Ethernet.eth3.Status=system
				HardwareList.Ethernet.eth3.Type=Ethernet
				HardwareList.Ethernet.eth2.Card.ModuleName=bnx2
				HardwareList.Ethernet.eth2.Description=Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet
				HardwareList.Ethernet.eth2.Name=eth2
				HardwareList.Ethernet.eth2.Status=system
				HardwareList.Ethernet.eth2.Type=Ethernet
				HardwareList.Ethernet.eth1.Card.ModuleName=bnx2
				HardwareList.Ethernet.eth1.Description=Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet
				HardwareList.Ethernet.eth1.Name=eth1
				HardwareList.Ethernet.eth1.Status=system
				HardwareList.Ethernet.eth1.Type=Ethernet
				HardwareList.Ethernet.eth0.Card.ModuleName=bnx2
				HardwareList.Ethernet.eth0.Description=Broadcom Corporation NetXtreme II BCM5709 Gigabit Ethernet
				HardwareList.Ethernet.eth0.Name=eth0
				HardwareList.Ethernet.eth0.Status=system
				HardwareList.Ethernet.eth0.Type=Ethernet
				ProfileList.default.Active=True
				ProfileList.default.ActiveDevices.1=eth0
				ProfileList.default.ActiveDevices.2=eth1
				ProfileList.default.ActiveDevices.3=eth2
				ProfileList.default.ActiveDevices.4=eth3
				ProfileList.default.DNS.Domainname=
				ProfileList.default.DNS.Hostname=r910.example.com
				ProfileList.default.DNS.PrimaryDNS=202.96.134.133
				ProfileList.default.DNS.SearchList.1=example.com
				ProfileList.default.DNS.SecondaryDNS=202.96.128.68
				ProfileList.default.DNS.TertiaryDNS=
				ProfileList.default.HostsList.1.AliasList.1=r910
				ProfileList.default.HostsList.1.Hostname=r910.example.com
				ProfileList.default.HostsList.1.IP=192.168.80.33
				ProfileList.default.HostsList.2.AliasList.1=localhost
				ProfileList.default.HostsList.2.Hostname=localhost.localdomain
				ProfileList.default.HostsList.2.IP=127.0.0.1
				ProfileList.default.HostsList.3.AliasList.1=r910
				ProfileList.default.HostsList.3.AliasList.2=localhost6.localdomain6
				ProfileList.default.HostsList.3.AliasList.3=localhost6
				ProfileList.default.HostsList.3.Hostname=r910.example.com
				ProfileList.default.HostsList.3.IP=::1
				ProfileList.default.ProfileName=default

```

### 2.7. ntsysv

ntsysv

```
				┌──────────────────┤ Services ├──────────────────┐
				│ │
				│ What services should be automatically started? │
				│ │
				│ [*] NetworkManager ↑ │
				│ [*] acpid ▮ │
				│ [*] atd ▒ │
				│ [ ] auditd ▒ │
				│ [*] autofs ▒ │
				│ [*] avahi-daemon ▒ │
				│ [*] bluetooth ▒ │
				│ [ ] certmonger ▒ │
				│ [ ] cgconfig ▒ │
				│ [ ] cgred ▒ │
				│ [*] cpuspeed ▒ │
				│ [*] crond ▒ │
				│ [*] cups ▒ │
				│ [ ] dnsmasq ▒ │
				│ [ ] firstboot ▒ │
				│ [*] haldaemon ↓ │
				│ │
				│ ┌────┐ ┌────────┐ │
				│ │ Ok │ │ Cancel │ │
				│ └────┘ └────────┘ │
				│ │
				│ │
				└────────────────────────────────────────────────┘

```

### 2.8. lokkit

lokkit

禁用 Iptable 与 SELinux

```
				lokkit --disabled --selinux=disabled

```

### 2.9. system-config-kdump

### 2.10. system-config-services

system-config-services

### 2.11. authconfig-tui

authconfig-tui

## 第 21 章 crontab 定时任务

## 1. /etc/crontab

```

neo@netkiller ~/workspace/Linux % cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#		

```

演示实例

```

1) 每 10 分钟检查次 mysql 主从同步
*/10 * * * * /bin/bash /usr/local/bin/monitor/mysql_check_slave.sh > /dev/null 2>&1

2) 每天重启下 nsac 服务
* * */1 * * /etc/init.d/nsca restart

3) 每天的 20 时 50 分删除指定目录下 30 天前文件
50 20 * * * find /var/log/rsyncxk015log/ -type f -ctime +30 -delete /dev/null 2>&1

4) 每月的 1、11、21、31 日是的 6 点 30 分执行一次 ls 命令
30 6 */10 * * ls

5) 周一到周五每天的 16 点 0 分做一次 svn 日备份（自己写的 svn 备份脚本）
0 16 * * 1-5    /bin/bash /usr/local/bin/shell/svn_hotcopy.sh day  > /dev/null 2>&1

6) 每月 1 号 17 点 0 分做一次 svn 月备份
0 17 1 * *      /bin/bash /usr/local/bin/shell/svn_hotcopy.sh month > /dev/null 2>&1

7) 每周周六的 16 点 0 分做一次 svn 周备份
0 16 * * 6      /bin/bash /usr/local/bin/shell/svn_hotcopy.sh week > /dev/null 2>&1

8) 每隔两周,在周 6 的 22 点 30 分执行一次 mysql 完全备份,注意%在 crontab 下要转义
30 22 * * 6 [ $(/usr/bin/expr $(/bin/date +\%W) % 2) -eq 1 ] && /usr/local/bin/backup_shell/mysql_fullback.sh

9) 每个月,在最后一周的周 6 的 22 点 30 分执行一次 mysql 完全备份,注意%在 crontab 下要转义
30 22 * * 6 [ $(date -d "+7 days" +\%d) -gt  23 ] && /usr/local/bin/backup_shell/mysql_fullback.sh

每个月,在第一周的周 6 的 22 点 30 分执行一次 mysql 完全备份
30 22 * * 6 [ $(date -d "+7 days" +\%d) -lt  14 ] && /usr/local/bin/cron_mysql_feeds_db.sh &> /tmp/cron_mysql_feeds_db.log

10)  一个随机时间执行脚本 如签到 . 下面例子依赖 atd 服务
0 7 * * * source /etc/profile && /bin/echo '/usr/local/bin/casperjs /root/51ca.js' | at now + $(shuf -i 2-59 -n 1) min	

```