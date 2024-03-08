# 第 140 章 Message Queuing & RPC

## 1. RabbitMQ

[RabbitMQ](http://www.rabbitmq.com/)

### 1.1. 安装 RabbitMQ

running on 127.0.0.1 (localhost) on port 5672 (standard AMQP port).

#### 1.1.1. Ubuntu

```

$ sudo apt-get install rabbitmq-server			

```

#### 1.1.2. CentOS

```

# yum install -y rabbitmq-server
# chkconfig rabbitmq-server on
# service rabbitmq-server start			

```

添加用户, 添加权限, 删除 guest 用户

```

# rabbitmqctl add_user rabbit password
# rabbitmqctl set_permissions -p "/" rabbit ".*" ".*" ".*"
# rabbitmqctl delete_user guest			

```

#### 1.1.3. OSCM 一键安装

```

curl -s https://raw.githubusercontent.com/oscm/shell/master/mq/rabbitmq/rabbitmq-server-3.6.10.sh | bash

rabbitmqctl add_user admin admin123
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"			

```

#### 1.1.4. 检查端口

```

[root@netkiller ~]# ss -lnt | grep 5672
LISTEN 0 128 *:25672 *:*
LISTEN 0 128 :::5672 :::*			

```

### 1.2. 配置 RabbitMQ

创建配置文件，默认情况/etc/rabbitmq/下面什么都没有。你需要从共享文档中复制一份配置文件过去。

```

cp /usr/share/doc/rabbitmq-server-3.6.10/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config

```

#### 1.2.1. 监听所有适配器地址

默认 RabbitMQ 监听 localhost 如果你需要让外部机器连接进来，需要配置 tcp_listeners 0.0.0.0

```

 {tcp_listeners, [{"0.0.0.0",5672}]}		

```

### 1.3. rabbitmqctl - command line tool for managing a RabbitMQ broker

```
			rabbitmqctl status

```

#### 1.3.1. change_password

```

rabbitmqctl change_password admin <new_password>

```

#### 1.3.2. list_users

```

# rabbitmqctl list_users
Listing users ...
guest [administrator]
...done.			

```

#### 1.3.3. 虚拟机管理

```

$ rabbitmqctl add_vhost test
$ rabbitmqctl add_user testuser password
$ rabbitmqctl set_permissions -p test testuser ".*" ".*" ".*"			

```

#### 1.3.4. list_queues

```

# rabbitmqctl list_queues
Listing queues ...
amq.gen-RhBwbb9EdZ8Fgk_heGZQ2w 0
bb 0
customer 276930
demo 0
email 0
example 0
hello 1
members_id 282
new_members_id 0
q_linvo 0
real 0
...done.

```

#### 1.3.5. list_exchanges

```

# rabbitmqctl list_exchanges
Listing exchanges ...
direct
amq.direct direct
amq.fanout fanout
amq.headers headers
amq.match headers
amq.rabbitmq.log topic
amq.rabbitmq.trace topic
amq.topic topic
email direct
...done.

```

### 1.4. rabbitmq-plugins - command line tool for managing RabbitMQ broker plugins

启用插件

```

rabbitmq-plugins enable rabbitmq_management		

```

#### 1.4.1. rabbitmq_management

RabbitMQ Management HTTP API (https://cdn.rawgit.com/rabbitmq/rabbitmq-management/rabbitmq_v3_6_0/priv/www/api/index.html)

启用插件 Management and Monitoring 插件

```

rabbitmq-plugins enable rabbitmq_management
systemctl restart rabbitmq-server

```

```

# curl -u guest:guest http://localhost:15672/api/overview
{"management_version":"3.3.5","statistics_level":"fine","exchange_types":[{"name":"topic","description":"AMQP topic exchange, as per the AMQP specification","enabled":true},{"name":"fanout","description":"AMQP fanout exchange, as per the AMQP specification","enabled":true},{"name":"direct","description":"AMQP direct exchange, as per the AMQP specification","enabled":true},{"name":"headers","description":"AMQP headers exchange, as per the
AMQP specification","enabled":true}],"rabbitmq_version":"3.3.5","cluster_name":"rabbit@iZ623qr3xctZ","erlang_version":"R16B03-1","erlang_full_version":"Erlang R16B03-1
(erts-5.10.4) [source] [64-bit] [smp:8:8] [async-threads:30] [hipe]
[kernel-poll:true]","message_stats":{},"queue_totals":{"messages":0,"messages_details":{"rate":0.0},"messages_ready":0,"messages_ready_details":{"rate":0.0},"messages_unacknowledged":0,"messages_unacknowledged_details":{"rate":0.0}},"object_totals":{"consumers":1,"queues":3,"exchanges":10,"connections":1,"channels":1},"node":"rabbit@iZ623qr3xctZ","statistics_db_node":"rabbit@iZ623qr3xctZ","listeners":[{"node":"rabbit@iZ623qr3xctZ","protocol":"amqp","ip_address":"::","port":5672},{"node":"rabbit@iZ623qr3xctZ","protocol":"clustering","ip_address":"::","port":25672}],"contexts":[{"node":"rabbit@iZ623qr3xctZ","description":"RabbitMQ
Management","path":"/","port":15672}

```

vhosts

```

	# curl -u guest:guest http://localhost:15672/api/vhosts
				[{"messages":0,"messages_details":{"rate":0.0},"messages_ready":0,"messages_ready_details":{"rate":0.0},"messages_unacknowledged":0,"messages_unacknowledged_details":{"rate":0.0},"recv_oct":617,"recv_oct_details":{"rate":0.0},"send_oct":625,"send_oct_details":{"rate":0.0},"name":"/","tracing":false}]			

```

queues

```

# curl -s -u guest:guest http://localhost:15672/api/queues/%2f/example | sed 's/,/,\n/g'
{"message_stats":{"ack":817,
"ack_details":{"rate":0.8},
"deliver":829,
"deliver_details":{"rate":0.8},
"deliver_get":829,
"deliver_get_details":{"rate":0.8},
"publish":33700,
"publish_details":{"rate":22.4},
"redeliver":9,
"redeliver_details":{"rate":0.0}},
"messages":32884,
"messages_details":{"rate":39.2},
"messages_ready":32881,
"messages_ready_details":{"rate":39.2},
"messages_unacknowledged":3,
"messages_unacknowledged_details":{"rate":0.0},
"policy":"",
"exclusive_consumer_tag":"",
"consumers":1,
"consumer_utilisation":0.00005551817727208515,
"memory":34387224,
"backing_queue_status":{"q1":0,
"q2":0,
"delta":["delta",
0,
0,
0],
"q3":0,
"q4":32881,
"len":32881,
"pending_acks":3,
"target_ram_count":"infinity",
"ram_msg_count":32881,
"ram_ack_count":3,
"next_seq_id":33700,
"persistent_count":0,
"avg_ingress_rate":31.071205055112543,
"avg_egress_rate":0.7083061832348867,
"avg_ack_ingress_rate":0.7083061832348867,
"avg_ack_egress_rate":0.7083061832348867},
"state":"running",
"incoming":[{"stats":{"publish":33700,
"publish_details":{"rate":22.4}},
"exchange":{"name":"email",
"vhost":"/"}}],
"deliveries":[{"stats":{"redeliver":3,
"redeliver_details":{"rate":0.0},
"deliver_get":348,
"deliver_get_details":{"rate":0.8},
"deliver":348,
"deliver_details":{"rate":0.8},
"ack":345,
"ack_details":{"rate":0.8}},
"channel_details":{"name":"127.0.0.1:41033 -> 127.0.0.1:5672 (1)",
"number":1,
"connection_name":"127.0.0.1:41033 -> 127.0.0.1:5672",
"peer_port":41033,
"peer_host":"127.0.0.1"}}],
"consumer_details":[{"channel_details":{"name":"127.0.0.1:41033 -> 127.0.0.1:5672 (1)",
"number":1,
"connection_name":"127.0.0.1:41033 -> 127.0.0.1:5672",
"peer_port":41033,
"peer_host":"127.0.0.1"},
"queue":{"name":"example",
"vhost":"/"},
"consumer_tag":"amq.ctag-6BSkZzt3eWgBG5Jn2nl4QA",
"exclusive":false,
"ack_required":true,
"prefetch_count":3,
"arguments":{}}],
"name":"example",
"vhost":"/",
"durable":true,
"auto_delete":false,
"arguments":{},
"node":"rabbit@iZ623qr3xctZ"}		

```

### 1.5. Python - Pika

[`pika.github.com/`](http://pika.github.com/)

```

sudo apt-get install python-setuptools python-pip git-core
sudo pip install pika

sudo easy_install pika		

```

### 1.6. Ruby amqp

```

$ sudo gem install amqp		

```

例 140.1. Ruby on RabbitMQ

subscriber.rb

```

$ cat subscriber.rb
require 'rubygems'
require 'amqp'

EM.run {
amq = MQ.new
amq.queue("logins").subscribe do |login|
puts login
end
}			

```

producer.rb

```

$ cat producer.rb
require 'rubygems'
require 'amqp'

EM.run {
amq = MQ.new
queue = amq.queue("logins")
%w[scott nic robi].each { |login|
queue.publish(login)
}
}			

```

test

```

$ ruby subscriber.rb
$ ruby producer.rb			

```

## 2. ZeroMQ

[ZeroMQ](http://www.zeromq.org/)

```
$ sudo apt-get install zeromq-bin libzmq0 libzmq-dev libzmq-dbg

```

### 2.1. python-zeromq

```
sudo add-apt-repository ppa:chris-lea/zeromq
sudo apt-get update

```

```
sudo apt-get install python-zeromq

```

#### 2.1.1. pyzmq

[`zeromq.github.com/pyzmq/`](http://zeromq.github.com/pyzmq/)

```
$ sudo apt-get install autoconf automake
$ sudo pip install pyzmq

```

```
$ git clone git://github.com/zeromq/pyzmq.git
$ cd pyzmq
$ python setup.py configure --zmq=/path/to/zmq/prefix
$ python setup.py install

```

```
easy_install pyzmq

```

#### 2.1.2. example

例 140.2. server.py

```
$ cat server.py
import zmq
context = zmq.Context()
socket = context.socket(zmq.REP)
socket.bind("tcp://127.0.0.1:5000")

while True:
    msg = socket.recv()
    print "Got", msg
    socket.send(msg)

```

例 140.3. client.py

```
$ cat client.py
import zmq
context = zmq.Context()
socket = context.socket(zmq.REQ)
socket.connect("tcp://127.0.0.1:5000")

for i in range(10):
    msg = "msg %s" % i
    socket.send(msg)
    print "Sending", msg
    msg_in = socket.recv()

```

### 2.2. ruby zmq

```
sudo gem install zmq

```

## 3. nanomsg

[`nanomsg.org/`](http://nanomsg.org/)

nanomsg 是 zeromq 的 C 实现，zeromq 使用 C++语言开发，作者意识到有很多问题与设计缺陷，决定使用 C 重新实现。

## 4. Gearman

http://gearman.org/

### 4.1. Getting Started with Gearman

#### 4.1.1. CentOS

```
rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

yum install gearmand -y
chkconfig gearmand on
service gearmand start

```

配置启动参数

```

cat >> /etc/sysconfig/gearmand <<EOF

OPTIONS="--log-file=/var/log/gearman.log --threads=512"
EOF

```

#### 4.1.2. Ubuntu

```
$ apt-cache search gearman | grep gearman
drizzle-plugin-gearman-udf - Gearman User Defined Functions for Drizzle
drizzle-plugin-logging-gearman - Gearman Logging for Drizzle
gearman - Distributed job queue
gearman-job-server - Job server for the Gearman distributed job queue
gearman-server - Gearman distributed job server and Perl interface
gearman-tools - Tools for the Gearman distributed job queue
libgearman-client-async-perl - asynchronous client for the Gearman distributed job system
libgearman-client-perl - client for the Gearman distributed job system
libgearman-dbg - Debug symbols for the Gearman Client Library
libgearman-dev - Development files for the Gearman Library
libgearman-doc - API Documentation for the Gearman Library
libgearman6 - Library providing Gearman client and worker functions
mod-gearman-doc - Documentation and examples for Mod-Gearman
mod-gearman-module - Nagios/Icinga event broker module for Mod-Gearman
mod-gearman-tools - Tools for mod-gearman
mod-gearman-worker - Worker agent for Mod-Gearman
python-gearman - Python interface to the Gearman system
python-gearman.libgearman - Python wrapper of libgearman
python3-gearman.libgearman - Python 3 wrapper of libgearman

```

#### 4.1.3. 防火墙设置

查看 gearman 工作端口

```
# grep gearman /etc/services
gearman         4730/tcp                # Gearman Job Queue System
gearman         4730/udp                # Gearman Job Queue System

```

iptables 设置

```
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 4730 -j ACCEPT

```

### 4.2. gearman

控制台 A

```
gearman -w -f wc -- wc -l

```

控制台 B

```

#wc -l < /etc/passwd
30

# wc -l < /etc/passwd
30

```

停止 gearman 进程再试

```

# /etc/init.d/gearmand stop
Stopping gearmand:                                         [  OK  ]

[root@haproxy ~]# gearman -f wc < /etc/passwd
gearman:gearman_client_run_tasks:gearman_connection_flush:could not connect

```

压力测试

```

find / -type f | awk '{ print "gearman -f wc < " $1 }' | bash

```

### 4.3. Gearman PHP Extension

```

rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

yum install libgearman-devel
pecl install channel://pecl.php.net/gearman-0.8.3

cat >> /srv/php/etc/conf.d/gearman.ini <<EOF
extension=gearman.so
EOF

```

测试安装

```
# php -r 'printf("%s \r\n", gearman_version());'
0.14

```

## 5. Apache Kafka is a distributed publish-subscribe messaging system

http://kafka.apache.org/

### 5.1. 安装 Kafka

#### 5.1.1. 安装 Kafka 用于开发与测试环境

如果你是开发或测试环境使用，可以使用内置 zookeeper

```

cd /usr/local/src
wget http://apache.communilink.net/kafka/0.10.2.0/kafka_2.12-0.10.2.0.tgz
tar zxvf kafka_2.12-0.10.2.0.tgz
mv kafka_2.12-0.10.2.0 /srv/
cp /srv/kafka_2.12-0.10.2.0/config/server.properties{,.original}
echo "advertised.host.name=localhost" >> /srv/kafka_2.12-0.10.2.0/config/server.properties
ln -s /srv/kafka_2.12-0.10.2.0 /srv/kafka
/srv/kafka/bin/zookeeper-server-start.sh config/zookeeper.properties
/srv/kafka/bin/kafka-server-start.sh /srv/kafka/config/server.properties

```

启动 Kafka 服务

```

/srv/kafka/bin/zookeeper-server-start.sh -daemon /srv/kafka/config/zookeeper.properties
/srv/kafka/bin/kafka-server-start.sh -daemon /srv/kafka/config/server.properties

```

-daemon 表示守护进程方式在后台启动

停止 Kafka 服务

```
/srv/kafka/bin/kafka-server-stop.sh
/srv/kafka/bin/zookeeper-server-stop.sh

```

#### 5.1.2. 安装 Kafka 适用于 IDC

如果是生产环境安装脚本如下，独立安装 zookeeper.

```

#!/bin/bash

cd /usr/local/src
wget http://apache.communilink.net/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
tar zxvf zookeeper-3.4.9.tar.gz
cp zookeeper-3.4.9/conf/zoo_sample.cfg zookeeper-3.4.9/conf/zoo.cfg
vim zookeeper-3.4.9/conf/zoo.cfg
mv zookeeper-3.4.9 /srv/
ln -s /srv/zookeeper-3.4.9 /srv/zookeeper
#cd zookeeper-3.4.9
/srv/zookeeper/bin/zkServer.sh start

cd /usr/local/src
wget http://apache.communilink.net/kafka/0.10.2.0/kafka_2.12-0.10.2.0.tgz
tar zxvf kafka_2.12-0.10.2.0.tgz
mv kafka_2.12-0.10.2.0 /srv/
cp /srv/kafka_2.12-0.10.2.0/config/server.properties{,.original}
echo "advertised.host.name=localhost" >> /srv/kafka_2.12-0.10.2.0/config/server.properties
ln -s /srv/kafka_2.12-0.10.2.0 /srv/kafka
/srv/kafka/bin/kafka-server-start.sh /srv/kafka/config/server.properties

```

启动 zookeeper

```

$ /srv/zookeeper/bin/zkServer.sh start

```

停止 zookeeper

```

$ /srv/zookeeper/bin/zkServer.sh stop
ZooKeeper JMX enabled by default
Using config: /srv/zookeeper/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED

```

#### 5.1.3. Kafka 日志

查看 server 日志

```

tailf /srv/kafka/logs/server.log			

```

#### 5.1.4. 检查 Kafka 线程

使用 jps 命令监控 Kafka 线程是否正确启动。

```

root@netkiller /srv/kafka/logs % jps | grep Kafka
32246 Kafka			

```

### 5.2. 测试 Kafka

```
$ cd /srv/kafka		

```

创建 Topic

```
$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test	
Created topic "test".

```

查看 Topic

```
$ bin/kafka-topics.sh --list --zookeeper localhost:2181
test

```

启动 Producer 生产消息

```
$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
This is a message
This is another message

```

启动 Consumer 消费消息

```
$ bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
This is a message
This is another message

```

### 5.3. 配置 Kafka

#### 5.3.1. server.properties

```

############################# System #############################
#唯一标识在集群中的 ID，要求是正数。
broker.id=0
#服务端口，默认 9092
port=9092
#监听地址，不设为所有地址
host.name=netkiller01

# 处理网络请求的最大线程数
num.network.threads=2
# 处理磁盘 I/O 的线程数
num.io.threads=8
# 一些后台线程数
background.threads = 4
# 等待 IO 线程处理的请求队列最大数
queued.max.requests = 500

#  socket 的发送缓冲区（SO_SNDBUF）
socket.send.buffer.bytes=1048576
# socket 的接收缓冲区 (SO_RCVBUF) 
socket.receive.buffer.bytes=1048576
# socket 请求的最大字节数。为了防止内存溢出，message.max.bytes 必然要小于
socket.request.max.bytes = 104857600

############################# Topic #############################
# 每个 topic 的分区个数，更多的 partition 会产生更多的 segment file
num.partitions=2
# 是否允许自动创建 topic ，若是 false，就需要通过命令创建 topic
auto.create.topics.enable =true
# 一个 topic ，默认分区的 replication 个数 ，不能大于集群中 broker 的个数。
default.replication.factor =1
# 消息体的最大大小，单位是字节
message.max.bytes = 1000000

############################# ZooKeeper #############################
# Zookeeper quorum 设置。如果有多个使用逗号分割
zookeeper.connect=netkiller01:2181,netkiller02,netkiller03
# 连接 zk 的超时时间
zookeeper.connection.timeout.ms=1000000
# ZooKeeper 集群中 leader 和 follower 之间的同步实际
zookeeper.sync.time.ms = 2000

############################# Log #############################
#日志存放目录，多个目录使用逗号分割
log.dirs=/var/log/kafka

# 当达到下面的消息数量时，会将数据 flush 到日志文件中。默认 10000
#log.flush.interval.messages=10000
# 当达到下面的时间(ms)时，执行一次强制的 flush 操作。interval.ms 和 interval.messages 无论哪个达到，都会 flush。默认 3000ms
#log.flush.interval.ms=1000
# 检查是否需要将日志 flush 的时间间隔
log.flush.scheduler.interval.ms = 3000

# 日志清理策略（delete|compact）
log.cleanup.policy = delete
# 日志保存时间 (hours|minutes)，默认为 7 天（168 小时）。超过这个时间会根据 policy 处理数据。bytes 和 minutes 无论哪个先达到都会触发。
log.retention.hours=168
# 日志数据存储的最大字节数。超过这个时间会根据 policy 处理数据。
#log.retention.bytes=1073741824

# 控制日志 segment 文件的大小，超出该大小则追加到一个新的日志 segment 文件中（-1 表示没有限制）
log.segment.bytes=536870912
# 当达到下面时间，会强制新建一个 segment
log.roll.hours = 24*7
# 日志片段文件的检查周期，查看它们是否达到了删除策略的设置（log.retention.hours 或 log.retention.bytes）
log.retention.check.interval.ms=60000

# 是否开启压缩
log.cleaner.enable=false
# 对于压缩的日志保留的最长时间
log.cleaner.delete.retention.ms = 1 day

# 对于 segment 日志的索引文件大小限制
log.index.size.max.bytes = 10 * 1024 * 1024
#y 索引计算的一个缓冲区，一般不需要设置。
log.index.interval.bytes = 4096

############################# replica #############################
# partition management controller 与 replicas 之间通讯的超时时间
controller.socket.timeout.ms = 30000
# controller-to-broker-channels 消息队列的尺寸大小
controller.message.queue.size=10
# replicas 响应 leader 的最长等待时间，若是超过这个时间，就将 replicas 排除在管理之外
replica.lag.time.max.ms = 10000
# 是否允许控制器关闭 broker ,若是设置为 true,会关闭所有在这个 broker 上的 leader，并转移到其他 broker
controlled.shutdown.enable = false
# 控制器关闭的尝试次数
controlled.shutdown.max.retries = 3
# 每次关闭尝试的时间间隔
controlled.shutdown.retry.backoff.ms = 5000

# 如果 relicas 落后太多,将会认为此 partition relicas 已经失效。而一般情况下,因为网络延迟等原因,总会导致 replicas 中消息同步滞后。如果消息严重滞后,leader 将认为此 relicas 网络延迟较大或者消息吞吐能力有限。在 broker 数量较少,或者网络不足的环境中,建议提高此值.
replica.lag.max.messages = 4000
#leader 与 relicas 的 socket 超时时间
replica.socket.timeout.ms= 30 * 1000
# leader 复制的 socket 缓存大小
replica.socket.receive.buffer.bytes=64 * 1024
# replicas 每次获取数据的最大字节数
replica.fetch.max.bytes = 1024 * 1024
# replicas 同 leader 之间通信的最大等待时间，失败了会重试
replica.fetch.wait.max.ms = 500
# 每一个 fetch 操作的最小数据尺寸,如果 leader 中尚未同步的数据不足此值,将会等待直到数据达到这个大小
replica.fetch.min.bytes =1
# leader 中进行复制的线程数，增大这个数值会增加 relipca 的 IO
num.replica.fetchers = 1
# 每个 replica 将最高水位进行 flush 的时间间隔
replica.high.watermark.checkpoint.interval.ms = 5000

# 是否自动平衡 broker 之间的分配策略
auto.leader.rebalance.enable = false
# leader 的不平衡比例，若是超过这个数值，会对分区进行重新的平衡
leader.imbalance.per.broker.percentage = 10
# 检查 leader 是否不平衡的时间间隔
leader.imbalance.check.interval.seconds = 300
# 客户端保留 offset 信息的最大空间大小
offset.metadata.max.bytes = 1024

```

##### 5.3.1.1. 外网访问

默认 kafka 对 localhost 提供访问，如果开放外面的 IP 进来你需要配置 config/server.properties

```
listeners = PLAINTEXT://147.189.135.55:9092

```

以及

```
advertised.host.name=147.189.135.55

```

#### 5.3.2. consumer.properties

```

#############################Consumer #############################
# Consumer 端核心的配置是 group.id、zookeeper.connect
# 决定该 Consumer 归属的唯一组 ID，By setting the same group id multiple processes indicate that they are all part of the same consumer group.
group.id
# 消费者的 ID，若是没有设置的话，会自增
consumer.id
# 一个用于跟踪调查的 ID ，最好同 group.id 相同
client.id = <group_id>

# 对于 zookeeper 集群的指定，必须和 broker 使用同样的 zk 配置
zookeeper.connect=netkiller01:2182,netkiller02:2182,netkiller03:2182
# zookeeper 的心跳超时时间，查过这个时间就认为是无效的消费者
zookeeper.session.timeout.ms = 6000
# zookeeper 的等待连接时间
zookeeper.connection.timeout.ms = 6000
# zookeeper 的 follower 同 leader 的同步时间
zookeeper.sync.time.ms = 2000
# 当 zookeeper 中没有初始的 offset 时，或者超出 offset 上限时的处理方式 。
# smallest ：重置为最小值 
# largest:重置为最大值 
# anything else：抛出异常给 consumer
auto.offset.reset = largest

# socket 的超时时间，实际的超时时间为 max.fetch.wait + socket.timeout.ms.
socket.timeout.ms= 30 * 1000
# socket 的接收缓存空间大小
socket.receive.buffer.bytes=64 * 1024
#从每个分区 fetch 的消息大小限制
fetch.message.max.bytes = 1024 * 1024

# true 时，Consumer 会在消费消息后将 offset 同步到 zookeeper，这样当 Consumer 失败后，新的 consumer 就能从 zookeeper 获取最新的 offset
auto.commit.enable = true
# 自动提交的时间间隔
auto.commit.interval.ms = 60 * 1000

# 用于消费的最大数量的消息块缓冲大小，每个块可以等同于 fetch.message.max.bytes 中数值
queued.max.message.chunks = 10

# 当有新的 consumer 加入到 group 时,将尝试 reblance,将 partitions 的消费端迁移到新的 consumer 中, 该设置是尝试的次数
rebalance.max.retries = 4
# 每次 reblance 的时间间隔
rebalance.backoff.ms = 2000
# 每次重新选举 leader 的时间
refresh.leader.backoff.ms

# server 发送到消费端的最小数据，若是不满足这个数值则会等待直到满足指定大小。默认为 1 表示立即接收。
fetch.min.bytes = 1
# 若是不满足 fetch.min.bytes 时，等待消费端请求的最长等待时间
fetch.wait.max.ms = 100
# 如果指定时间内没有新消息可用于消费，就抛出异常，默认-1 表示不受限
consumer.timeout.ms = -1

```

##### 5.3.2.1. group.id

查看 group.id 配置

```
# cat config/consumer.properties  | grep "group\.id"
group.id=test-consumer-group

```

#### 5.3.3. producer.properties

```

#############################Producer#############################
# 核心的配置包括：
# metadata.broker.list
# request.required.acks
# producer.type
# serializer.class

# 消费者获取消息元信息(topics, partitions and replicas)的地址,配置格式是：host1:port1,host2:port2，也可以在外面设置一个 vip
metadata.broker.list

#消息的确认模式
# 0：不保证消息的到达确认，只管发送，低延迟但是会出现消息的丢失，在某个 server 失败的情况下，有点像 TCP
# 1：发送消息，并会等待 leader 收到确认后，一定的可靠性
# -1：发送消息，等待 leader 收到确认，并进行复制操作后，才返回，最高的可靠性
request.required.acks = 0

# 消息发送的最长等待时间
request.timeout.ms = 10000
# socket 的缓存大小
send.buffer.bytes=100*1024
# key 的序列化方式，若是没有设置，同 serializer.class
key.serializer.class
# 分区的策略，默认是取模
partitioner.class=kafka.producer.DefaultPartitioner
# 消息的压缩模式，默认是 none，可以有 gzip 和 snappy
compression.codec = none
# 可以针对默写特定的 topic 进行压缩
compressed.topics=null
# 消息发送失败后的重试次数
message.send.max.retries = 3
# 每次失败后的间隔时间
retry.backoff.ms = 100
# 生产者定时更新 topic 元信息的时间间隔 ，若是设置为 0，那么会在每个消息发送后都去更新数据
topic.metadata.refresh.interval.ms = 600 * 1000
# 用户随意指定，但是不能重复，主要用于跟踪记录消息
client.id=""

# 异步模式下缓冲数据的最大时间。例如设置为 100 则会集合 100ms 内的消息后发送，这样会提高吞吐量，但是会增加消息发送的延时
queue.buffering.max.ms = 5000
# 异步模式下缓冲的最大消息数，同上
queue.buffering.max.messages = 10000
# 异步模式下，消息进入队列的等待时间。若是设置为 0，则消息不等待，如果进入不了队列，则直接被抛弃
queue.enqueue.timeout.ms = -1
# 异步模式下，每次发送的消息数，当 queue.buffering.max.messages 或 queue.buffering.max.ms 满足条件之一时 producer 会触发发送。
batch.num.messages=200	

```

### 5.4. 管理 Kafka

进入控制台

```
bin/zookeeper-shell.sh localhost:2181

```

删除 Topic

```
$ /srv/kafka/bin/kafka-run-class.sh kafka.admin.TopicCommand --delete --topic kafkatopic --zookeeper localhost:2181

```

查看 Topic 的 offset

```
$ /srv/kafka/bin/kafka-consumer-offset-checker.sh  --zookeeper localhost:2181 --topic kafkatopic --group consumer	

```

### 5.5. FAQ

#### 5.5.1. WARN Error while fetching metadata with correlation id 1 : {test=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)

解决方法

```

echo "advertised.host.name=localhost" >> /srv/kafka/config/server.properties

```

#### 5.5.2. Error while executing topic command : Replication factor: 1 larger than available brokers: 0.

```

root@VM_7_221_centos /srv/kafka % bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
Error while executing topic command : Replication factor: 1 larger than available brokers: 0.
[2017-11-26 10:55:11,532] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 1 larger than available brokers: 0.
 (kafka.admin.TopicCommand$)			

```

检查 broker.id 配置 broker.id 必须大于 0

```

root@netkiller /srv/kafka % cat config/server.properties | grep broker.id
broker.id=1			

```

#### 5.5.3. WARN Connection to node -1 could not be established. Broker may not be available. (org.apache.kafka.clients.NetworkClient)

Kafka 在防火墙后面，防火墙上面配置 NAT 规则映射到服务器

```

# bind 任何 IP 地址
listeners=PLAINTEXT://:9092
# Wan IP 地址
advertised.host.name=223.207.161.225

```

### 提示

修改 advertised.host.name 后要删除 /tmp/kafka-logs 中的日志文件，否则无论如何你你都难以配置成功

```

rm -rf /tmp/kafka-logs			

```

## 6. Celery

http://www.celeryproject.org/

## 7. ActiveMQ

[Apache ActiveMQ](http://activemq.apache.org/)

## 8. http://kr.github.io/beanstalkd/

## 9. gRPC

http://www.grpc.io/