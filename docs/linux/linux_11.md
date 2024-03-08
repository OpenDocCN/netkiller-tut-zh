# 部分 IX. Distributed Computing

## 第 135 章 Open Source Distributed Computing

## 1. Boinc (berkeley 分布式计算平台)

下载 Boinc

**$ wget http://boinc.berkeley.edu/dl/boinc_5.6.4_i686-pc-linux-gnu.sh**

```
netkiller@Linux-server:~$ wget http://boinc.berkeley.edu/dl/boinc_5.6.4_i686-pc-linux-gnu.sh
--11:02:36--  http://boinc.berkeley.edu/dl/boinc_5.6.4_i686-pc-linux-gnu.sh
           => `boinc_5.6.4_i686-pc-linux-gnu.sh'
Resolving boinc.berkeley.edu... 128.32.18.189
Connecting to boinc.berkeley.edu|128.32.18.189|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3,205,541 (3.1M) [application/x-sh]

100%[====================================>] 3,205,541      8.95K/s    ETA 00:00

11:08:45 (8.53 KB/s) - `boinc_5.6.4_i686-pc-linux-gnu.sh' saved [3205541/3205541]

```

**$ chmod +x boinc_5.6.4_i686-pc-linux-gnu.sh** **$ ./boinc_5.6.4_i686-pc-linux-gnu.sh**

```
netkiller@Linux-server:~$ chmod +x boinc_5.6.4_i686-pc-linux-gnu.sh
netkiller@Linux-server:~$ ./boinc_5.6.4_i686-pc-linux-gnu.sh
use /home/netkiller/BOINC/run_manager to start BOINC
netkiller@Linux-server:~$ ls
BOINC  boinc_5.6.4_i686-pc-linux-gnu.sh  public_html  www
netkiller@Linux-server:~$ cd BOINC/
netkiller@Linux-server:~/BOINC$ ls
binstall.sh  boincmgr            boincmgr.8x8.png  run_client
boinc        boincmgr.16x16.png  ca-bundle.crt     run_manager
boinc_cmd    boincmgr.32x32.png  locale
netkiller@Linux-server:~/BOINC$

```

添加项目

```
$ ./boinc --attach_project http://setiathome.berkeley.edu/ 3d996959b1f88df43048f87c3c0c999f

```

运行 Boinc

**./boinc -daemon -no_gui_rpc**

### 1.1. rc.local

```
/home/neo/BOINC/run_client --daemon

```

## 2. ubuntu apt-get 安装

```

sudo apt install boinctui boinc-client
sudo systemctl enable boinc-client
sudo systemctl start boinc-client

sudo boinctui		

```

Ubuntu 早起版本

```
netkiller@shenzhen:~/BOINC$ apt-cache search boinc
boinc-app-seti - SETI@home application for the BOINC client
boinc-client - core client for the BOINC distributed computing infrastructure
boinc-dev - development files to build applications for BOINC projects
boinc-manager - GUI to control and monitor the BOINC core client
kboincspy - monitoring utility for the BOINC client
kboincspy-dev - development files for KBoincSpy plugins
netkiller@shenzhen:~/BOINC$

安装
netkiller@shenzhen:~/BOINC$ sudo apt-get install boinc-client

拷贝现有的 account 文件
netkiller@shenzhen:~/BOINC$ cp account_* /var/lib/boinc-client/

重新启动
netkiller@shenzhen:~/BOINC$ /etc/init.d/boinc-client restart

```

## 3. CentOS 安装

```
yum install boinc-client

chkconfig boinc-client on

```

Xwindows 管理界面

```
yum install boinc-manager

```

添加计算项目

```
cd /var/lib/boinc
boinc --attach_project http://einstein.phys.uwm.edu/ f9d5ee6d433a6949599f91dd7d9ceb8e	

chown boinc:boinc -R *
service boinc-client start	

```

## 4. boinccmd

```
# ./boinccmd

usage: boinccmd [--host hostname] [--passwd passwd] command

Commands:
 --lookup_account URL email passwd
 --create_account URL email passwd name
 --project_attach URL auth          attach to project
 --join_acct_mgr URL name passwd    attach account manager
 --quit_acct_mgr                    quit current account manager
 --get_state                        show entire state
 --get_results                      show results
 --get_simple_gui_info              show status of projects and active results
 --get_file_transfers               show file transfers
 --get_project_status               show status of all attached projects
 --get_disk_usage                   show disk usage
 --get_proxy_settings
 --get_messages [ seqno ]           show messages > seqno
 --get_message_count                show largest message seqno
 --get_host_info
 --version, -V                      show core client version
 --result url result_name op        job operation
   op = suspend | resume | abort | graphics_window | graphics_fullscreen
 --project URL op                   project operation
   op = reset | detach | update | suspend | resume | nomorework | allowmorework
 --file_transfer URL filename op    file transfer operation
   op = retry | abort
 --set_run_mode mode duration       set run mode for given duration
   mode = always | auto | never
 --set_gpu_mode mode duration       set GPU run mode for given duration
   mode = always | auto | never
 --set_network_mode mode duration
 --set_proxy_settings
 --run_benchmarks
 --read_global_prefs_override
 --quit
 --read_cc_config
 --set_debts URL1 std1 ltd1 [URL2 std2 ltd2 ...]
 --get_project_config URL
 --get_project_config_poll
 --network_available
 --get_cc_status

```

### 4.1. attach_project

添加计算项目

```
$ ./boinc --attach_project http://setiathome.berkeley.edu/ 3d996959b1f88df43048f87c3c0c999f
$ ./boinc --attach_project www.worldcommunitygrid.org dad152cf8f8fbdc52b04d4eeaa43e1ca
$ ./boinc --attach_project http://climateprediction.net/ 4070a202cd5a559ec9d044cffc156fa4
$ ./boinc --attach_project http://einstein.phys.uwm.edu/ f9d5ee6d433a6949599f91dd7d9ceb8e
$ ./boinc --attach_project http://milkyway.cs.rpi.edu/milkyway/ f2fa96fb4f72df925cba92c34031768d
$ ./boinc --attach_project http://boinc.iaik.tugraz.at/sha1_coll_search/ 0017d38d9c4a944caa8dad0b82b3f6a6
$ ./boinc --attach_project http://lhcathome.cern.ch/lhcathome/ 132e3b1b159af3c36c98056f9197dd8a
$ ./boinc --attach_project http://boinc.bakerlab.org/rosetta/ 6ed4722aa62a9df5dd341e0b3b77d812

```

通过 boinccmd 添加项目

```
./boinccmd --project_attach http://einstein.phys.uwm.edu/ f9d5ee6d433a6949599f91dd7d9ceb8e
./boinccmd --project_attach http://boinc.bakerlab.org/rosetta/ 6ed4722aa62a9df5dd341e0b3b77d812

```

### 4.2. nomorework | allowmorework 禁止下载任务 / 允许下载任务

```
./boinccmd --project http://boinc.bakerlab.org/rosetta/ nomorework
./boinccmd --project http://milkyway.cs.rpi.edu/milkyway/ nomorework
./boinccmd --project http://einstein.phys.uwm.edu/ nomorework
./boinccmd --project http://setiathome.berkeley.edu/ nomorework

```

```
./boinccmd --project http://setiathome.berkeley.edu/ allowmorework

```

## 第 136 章 High performance Computing

### *Distributed Computing & Parallel Computing*

## 1. Distributed Computing

### 1.1. OpenMosix

### 1.2. OpenSSI

## 2. Parallel Computing

### 2.1. EnFusion

### 2.2. SCore

### 2.3. Beowulf

## 第 137 章 HPCC Systems (High Performance Computing Cluster)

## 第 138 章 Tachyon

Tachyon 是一个高容错的分布式文件系统，允许文件以内存的速度在集群框架中进行可靠的共享，类似 Spark 和 MapReduce。通过利用 lineage 信息，积极地使用内存，Tachyon 的吞吐量要比 HDFS 高 300 多倍。Tachyon 都是在内存中处理缓存文件，并且让不同的 Jobs/Queries 以及框架都能内存的速度来访问缓存文件。

## 第 139 章 Apache ZooKeeper

https://zookeeper.apache.org/

## 1. 安装配置

安装 Apache ZooKeeper

### 1.1. 单节点安装

```

cd /usr/local/src
wget http://ftp.cuhk.edu.hk/pub/packages/apache.org/zookeeper/stable/zookeeper-3.4.8.tar.gz
tar zxf zookeeper-3.4.8.tar.gz 
mkdir /var/lib/zookeeper

cat >> zookeeper-3.4.8/conf/zoo.cfg <<EOF
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/var/lib/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1

EOF

zookeeper-3.4.8 /srv/

```

启动 ZooKeeper

```

[root@localhost srv]# /srv/zookeeper-3.4.8/bin/zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /srv/zookeeper-3.4.8/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED

```

### 1.2. 多节点安装

```

tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
initLimit=5
syncLimit=2
server.1=zoo1:2888:3888
server.2=zoo2:2888:3888
server.3=zoo3:2888:3888

```

## 2. 管理 ZooKeeper

链接 ZooKeeper

bin/zkCli.sh -server 127.0.0.1:2181

```

[root@localhost zookeeper-3.4.8]# bin/zkCli.sh -server 127.0.0.1:2181
Connecting to 127.0.0.1:2181
2016-05-27 22:19:10,785 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.8--1, built on 02/06/2016 03:18 GMT
2016-05-27 22:19:10,788 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=localhost
2016-05-27 22:19:10,788 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.6.0_45
2016-05-27 22:19:10,789 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Sun Microsystems Inc.
2016-05-27 22:19:10,789 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/srv/jdk1.6.0_45/jre
2016-05-27 22:19:10,789 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/srv/zookeeper-3.4.8/bin/../build/classes:/srv/zookeeper-3.4.8/bin/../build/lib/*.jar:/srv/zookeeper-3.4.8/bin/../lib/slf4j-log4j12-1.6.1.jar:/srv/zookeeper-3.4.8/bin/../lib/slf4j-api-1.6.1.jar:/srv/zookeeper-3.4.8/bin/../lib/netty-3.7.0.Final.jar:/srv/zookeeper-3.4.8/bin/../lib/log4j-1.2.16.jar:/srv/zookeeper-3.4.8/bin/../lib/jline-0.9.94.jar:/srv/zookeeper-3.4.8/bin/../zookeeper-3.4.8.jar:/srv/zookeeper-3.4.8/bin/../src/java/lib/*.jar:/srv/zookeeper-3.4.8/bin/../conf:/srv/java/lib:/srv/java/jre/lib:/lib:
2016-05-27 22:19:10,789 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/srv/jdk1.6.0_45/jre/lib/amd64/server:/srv/jdk1.6.0_45/jre/lib/amd64:/srv/jdk1.6.0_45/jre/../lib/amd64:/usr/java/packages/lib/amd64:/usr/lib64:/lib64:/lib:/usr/lib
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=3.10.0-327.10.1.el7.x86_64
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2016-05-27 22:19:10,790 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/srv/zookeeper-3.4.8
2016-05-27 22:19:10,791 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=127.0.0.1:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@6d8dfef8
Welcome to ZooKeeper!
2016-05-27 22:19:10,844 [myid:] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1032] - Opening socket connection to server 127.0.0.1/127.0.0.1:2181\. Will not attempt to authenticate using SASL (java.lang.SecurityException: Unable to locate a login configuration)
JLine support is enabled
2016-05-27 22:19:10,848 [myid:] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@876] - Socket connection established to 127.0.0.1/127.0.0.1:2181, initiating session
[zk: 127.0.0.1:2181(CONNECTING) 0] 2016-05-27 22:19:10,894 [myid:] - INFO  [main-SendThread(127.0.0.1:2181):ClientCnxn$SendThread@1299] - Session establishment complete on server 127.0.0.1/127.0.0.1:2181, sessionid = 0x154f526d8300000, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

```

### 2.1. help

```

[zk: 127.0.0.1:2181(CONNECTED) 0] help
ZooKeeper -server host:port cmd args
	connect host:port
	get path [watch]
	ls path [watch]
	set path data [version]
	rmr path
	delquota [-n|-b] path
	quit 
	printwatches on|off
	create [-s] [-e] path data acl
	stat path [watch]
	close 
	ls2 path [watch]
	history 
	listquota path
	setAcl path acl
	getAcl path
	sync path
	redo cmdno
	addauth scheme auth
	delete path [version]
	setquota -n|-b val path			

```

### 2.2. ls

```

[zk: 127.0.0.1:2181(CONNECTED) 1] ls /
[zookeeper]			

```

### 2.3. create

```

[zk: 127.0.0.1:2181(CONNECTED) 4] create /product product       
Created /product

[zk: 127.0.0.1:2181(CONNECTED) 6] ls /       
[product, zookeeper]	

```

### 2.4. get

```

[zk: 127.0.0.1:2181(CONNECTED) 7] get /

cZxid = 0x0
ctime = Wed Dec 31 19:00:00 EST 1969
mZxid = 0x0
mtime = Wed Dec 31 19:00:00 EST 1969
pZxid = 0x4
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 2

[zk: 127.0.0.1:2181(CONNECTED) 8] get /product
product
cZxid = 0x4
ctime = Fri May 27 22:27:55 EDT 2016
mZxid = 0x4
mtime = Fri May 27 22:27:55 EDT 2016
pZxid = 0x4
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0

```

### 2.5. set

```

[zk: 127.0.0.1:2181(CONNECTED) 9] set /product nickname=netkiller
cZxid = 0x4
ctime = Fri May 27 22:27:55 EDT 2016
mZxid = 0x5
mtime = Fri May 27 22:37:28 EDT 2016
pZxid = 0x4
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 18
numChildren = 0

[zk: 127.0.0.1:2181(CONNECTED) 10] get /product                   
nickname=netkiller
cZxid = 0x4
ctime = Fri May 27 22:27:55 EDT 2016
mZxid = 0x5
mtime = Fri May 27 22:37:28 EDT 2016
pZxid = 0x4
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 18
numChildren = 0

```

### 2.6. delete

```

[zk: 127.0.0.1:2181(CONNECTED) 11] delete /product
[zk: 127.0.0.1:2181(CONNECTED) 12] ls /
[zookeeper]

```