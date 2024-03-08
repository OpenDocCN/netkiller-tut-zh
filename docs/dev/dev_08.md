# 第 33 章 Message Queuing & RPC

## RabbitMQ

[RabbitMQ](http://www.rabbitmq.com/)

### 安装 RabbitMQ

running on 127.0.0.1 (localhost) on port 5672 (standard AMQP port).

#### Ubuntu

```
$ sudo apt-get install rabbitmq-server

```

#### CentOS

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

#### OSCM 一键安装

```
curl -s https://raw.githubusercontent.com/oscm/shell/master/mq/rabbitmq/rabbitmq-server-3.6.10.sh | bash

rabbitmqctl add_user admin admin123
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"

```

#### 检查端口

```
[root@netkiller ~]# ss -lnt | grep 5672
LISTEN     0      128          *:25672                    *:*                  
LISTEN     0      128         :::5672                    :::*

```

### rabbitmqctl - command line tool for managing a RabbitMQ broker

```
rabbitmqctl status

```

#### change_password

```

rabbitmqctl change_password admin <new_password>

```

#### list_users

```
# rabbitmqctl list_users
Listing users ...
guest	[administrator]
...done.

```

#### 虚拟机管理

```
$ rabbitmqctl add_vhost test
$ rabbitmqctl add_user testuser password
$ rabbitmqctl set_permissions -p test testuser ".*" ".*" ".*"			

```

#### list_queues

```
# rabbitmqctl list_queues
Listing queues ...
amq.gen-RhBwbb9EdZ8Fgk_heGZQ2w	0
bb	0
customer	276930
demo	0
email	0
example	0
hello	1
members_id	282
new_members_id	0
q_linvo	0
real	0
...done.

```

#### list_exchanges

```
# rabbitmqctl list_exchanges
Listing exchanges ...
	direct
amq.direct	direct
amq.fanout	fanout
amq.headers	headers
amq.match	headers
amq.rabbitmq.log	topic
amq.rabbitmq.trace	topic
amq.topic	topic
email	direct
...done.

```

### rabbitmq-plugins - command line tool for managing RabbitMQ broker plugins

启用插件

```
rabbitmq-plugins enable rabbitmq_management		

```

#### rabbitmq_management

RabbitMQ Management HTTP API (https://cdn.rawgit.com/rabbitmq/rabbitmq-management/rabbitmq_v3_6_0/priv/www/api/index.html)

启用插件 Management and Monitoring 插件

```
rabbitmq-plugins enable rabbitmq_management
systemctl restart rabbitmq-server			

```

```
# curl -u guest:guest http://localhost:15672/api/overview
{"management_version":"3.3.5","statistics_level":"fine","exchange_types":[{"name":"topic","description":"AMQP topic exchange, as per the AMQP specification","enabled":true},{"name":"fanout","description":"AMQP fanout exchange, as per the AMQP specification","enabled":true},{"name":"direct","description":"AMQP direct exchange, as per the AMQP specification","enabled":true},{"name":"headers","description":"AMQP headers exchange, as per the AMQP specification","enabled":true}],"rabbitmq_version":"3.3.5","cluster_name":"rabbit@iZ623qr3xctZ","erlang_version":"R16B03-1","erlang_full_version":"Erlang R16B03-1 (erts-5.10.4) [source] [64-bit] [smp:8:8] [async-threads:30] [hipe] [kernel-poll:true]","message_stats":{},"queue_totals":{"messages":0,"messages_details":{"rate":0.0},"messages_ready":0,"messages_ready_details":{"rate":0.0},"messages_unacknowledged":0,"messages_unacknowledged_details":{"rate":0.0}},"object_totals":{"consumers":1,"queues":3,"exchanges":10,"connections":1,"channels":1},"node":"rabbit@iZ623qr3xctZ","statistics_db_node":"rabbit@iZ623qr3xctZ","listeners":[{"node":"rabbit@iZ623qr3xctZ","protocol":"amqp","ip_address":"::","port":5672},{"node":"rabbit@iZ623qr3xctZ","protocol":"clustering","ip_address":"::","port":25672}],"contexts":[{"node":"rabbit@iZ623qr3xctZ","description":"RabbitMQ Management","path":"/","port":15672}]}			

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

### Python - Pika

[`pika.github.com/`](http://pika.github.com/)

```
sudo apt-get install python-setuptools python-pip git-core
sudo pip install pika

sudo easy_install pika

```

### Ruby amqp

```
$ sudo gem install amqp

```

例 33.1. Ruby on RabbitMQ

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

## ZeroMQ

[ZeroMQ](http://www.zeromq.org/)

```
$ sudo apt-get install zeromq-bin libzmq0 libzmq-dev libzmq-dbg

```

### python-zeromq

```
sudo add-apt-repository ppa:chris-lea/zeromq
sudo apt-get update

```

```
sudo apt-get install python-zeromq

```

#### pyzmq

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

#### example

例 33.2. server.py

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

例 33.3. client.py

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

### ruby zmq

```
sudo gem install zmq

```

## nanomsg

[`nanomsg.org/`](http://nanomsg.org/)

nanomsg 是 zeromq 的 C 实现，zeromq 使用 C++语言开发，作者意识到有很多问题与设计缺陷，决定使用 C 重新实现。

## Gearman

http://gearman.org/

### Getting Started with Gearman

#### CentOS

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

#### Ubuntu

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

#### 防火墙设置

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

### gearman

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

### Gearman PHP Extension

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

## Apache Kafka is a distributed publish-subscribe messaging system

http://kafka.apache.org/

### 安装 Kafka 用于开发与测试环境

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
/srv/kafka/bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
/srv/kafka/bin/kafka-server-start.sh -daemon /srv/kafka/config/server.properties

```

-daemon 表示守护进程方式在后台启动

停止 Kafka 服务

```
/srv/kafka/bin/kafka-server-stop.sh
/srv/kafka/bin/zookeeper-server-stop.sh

```

### 安装 Kafka 适用于 IDC

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

### Kafka 日志

查看 server 日志

```
tailf /srv/kafka/logs/server.log

```

### 测试 Kafka

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

### 配置 Kafka

#### 外网访问

默认 kafka 对 localhost 提供访问，如果开放外面的 IP 进来你需要配置 config/server.properties

```
listeners = PLAINTEXT://147.189.135.55:9092

```

以及

```
advertised.host.name=147.189.135.55

```

#### group.id

查看 group.id 配置

```
# cat config/consumer.properties  | grep "group\.id"
group.id=test-consumer-group

```

### 管理 Kafka

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

### FAQ

#### WARN Error while fetching metadata with correlation id 1 : {test=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)

解决方法

```

echo "advertised.host.name=localhost" >> /srv/kafka/config/server.properties

```

## Celery

http://www.celeryproject.org/

## ActiveMQ

[Apache ActiveMQ](http://activemq.apache.org/)

## http://kr.github.io/beanstalkd/

## gRPC

http://www.grpc.io/