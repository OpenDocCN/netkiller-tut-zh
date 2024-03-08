# 第 3 章 Systems architecture(系统架构)

### *Systems architecture*

```

                                       .---> media [mp3, wma, wmv, rmvb, asf, divx]-\
                                      /                                       +------------+
                                     .-----> photo [gif, jpg, png, swf] ----> | Raid Array | <--.
    /------------------- <---------\/                                         +------------+     \
user -> dns -> load balancing -> squid -> [cache] <----[html]----\                  /            |
                 \ \ \______<______/\                      +-------------+         /             |
                  \ \                \-----> web app ----> | html        |--------^              |
                   \ \____________________________/\       | php,jsp,cgi |                       |
                    \                               \      +-------------+                       |
                     \                               `-----> memcached [node1, node2, node(n)]   |
                      \__________________________________________/\                              |
                                                                   `------> Database cluster     |
    +--------------------------------------+                                      \              |
    | Author: neo chen <openunix#163.com>  |                                       `-------------+
    | Nickname: netkiller                  |
    | Homepage: http://netkiller.github.io |
    +--------------------------------------+

```

## 集群(Cluster)

集群有很多实现方法，分为硬件和软件，集群可以在不同网络层面上实现

1.  实现 IP 轮循（Bind DNS）

2.  硬件四层交换（硬件负载均衡设备 F5 BIG IP）

3.  软件四层交换（linux virtual server）

4.  应用层上实现（tomcat）

越是低层性能越好，越是上层功能更强

集群的分类

1.  高可用性集群

2.  负载均衡集群

3.  超级计算集群

网站一般用到两种集群分别是高可用性集群和负载均衡集群

### 负载均衡

#### DNS 负载均衡

这是早期出现的负载均衡技术，直到现在，很多网站仍然使用 DNS 负载均衡。

你可通过 ping 命令观看它是如何工作的，例如你可反复 ping 个网域名。

```

C:\>ping www.163.com

Pinging www.cache.split.netease.com [220.181.28.52] with 32 bytes of data:

Reply from 220.181.28.52: bytes=32 time=226ms TTL=53
Reply from 220.181.28.52: bytes=32 time=225ms TTL=53
Reply from 220.181.28.52: bytes=32 time=226ms TTL=53
Reply from 220.181.28.52: bytes=32 time=226ms TTL=53

Ping statistics for 220.181.28.52:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 225ms, Maximum = 226ms, Average = 225ms

C:\>ping www.163.com

Pinging www.cache.split.netease.com [220.181.28.53] with 32 bytes of data:

Reply from 220.181.28.53: bytes=32 time=52ms TTL=52
Reply from 220.181.28.53: bytes=32 time=53ms TTL=52
Reply from 220.181.28.53: bytes=32 time=52ms TTL=52
Reply from 220.181.28.53: bytes=32 time=52ms TTL=52

Ping statistics for 220.181.28.53:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 52ms, Maximum = 53ms, Average = 52ms

C:\>ping www.163.com

Pinging www.cache.split.netease.com [220.181.28.50] with 32 bytes of data:

Reply from 220.181.28.50: bytes=32 time=51ms TTL=53
Reply from 220.181.28.50: bytes=32 time=52ms TTL=53
Reply from 220.181.28.50: bytes=32 time=52ms TTL=53
Reply from 220.181.28.50: bytes=32 time=51ms TTL=53

Ping statistics for 220.181.28.50:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 51ms, Maximum = 52ms, Average = 51ms

C:\>

```

DNS 负载均衡主要优点

1.  技术简单，容易实现，灵活，方便，成本低

2.  Web 服务器可以位于互联网的任意位置上，无地理限制。

3.  DNS 的主从结构非常稳定

4.  可以有效的分散 DDOS 攻击。

5.  你甚至可以在 DNS 服务商那里实现，自己不需要添加设备。而且没有带宽开销。

DNS 负载均衡主要缺点

1.  DNS 负载均衡采用的是简单的轮循负载算法，不能够按照服务器节点的处理能力分配负载。

2.  不支持故障转移(failover)和自动恢复 failback ,如果某台服务器拓机，DNS 仍会将用户解析到这台故障服务器上，导致不能响应客户端。

3.  如果添加节点或撤出节点，不能即时更新到省市级 DNS,可导致部分地区不能访问。

4.  占用大量静态 IP。

#### 软件四层交换负载均衡

软件四层交换负载均衡为我们解决了几个问题

1.  能够按照服务器节点的处理能力分配负载。

2.  支持故障转移(failover)和自动恢复 failback ,如果某节点拓机，调度器自动将它剔除，不响应客户端访问，当节点故障排除调度器立即恢复节点。

3.  可以随时添加节点或撤出节点，即时生效,方便网站扩容。

软件四层交换负载均衡优点

1.  仅仅需要一个静态 IP。

2.  节点位于私有网络上与 WAN 隔离，用户面对的只是调度器。

3.  可以随时添加节点或撤出节点。

4.  通过端口可以组建多个集群。

#### 应用层负载均衡

Tomcat balancer

mod_proxy_balancer.so ,tomcat mod_jk.so

MySQL proxy / MySQL-LB

### 高可用性集群

俗称：双机热备份

关键词：心跳线

两部服务器，或多部服务器，形成一个集群，当主服务器崩溃是，立即切换到其它节点上。

两部服务器要做到，内容实时同步，保持数据一直。

一般用 heartbeat + DRBD 实现。heartbeat 负责切换服务器，DRBD 用于同步数据。

### 负载均衡设备

负载均衡成熟产品

1.  F5 Big IP

2.  Array

这些设备可提供 3,4,7 层负载均衡 HA，硬件已经压缩，HTTP 头改写，URL 改写...

其中 3 层交换部分多采用硬件实现。

更多关于 F5 与 Array 资料点击进入

### 会话保持

### 健康状态检查

## 缓存技术

首先要说明，很多缓存技术依赖静态化。下面展示了缓存可能出现的位置。

用户 user -> 浏览器缓存 IE/Firefox Cache -> 逆向代理缓存 Reverse proxy Cache -> WEB 服务器缓存 Apache cache -> 应用程序缓存 php cache -> 数据库缓存 database cache

当然交换机，网络适配器，硬盘上也有 Cache 但这不是我们要讨论的范围。

缓存存储方式主要是内存和文件两种，后者是存于硬盘中。

网站上使用的缓存主要包括五种：

1.  浏览器 缓存

2.  逆向代理/CDN 缓存

3.  WEB 服务器缓存

4.  应用程序缓存

5.  数据库缓存

将上面的缓存合理地，有选择性的使用可大大提高网站的访问能力。

总之，想让你的网站更快，更多并发，答案是 cache,cache 再 cache

### 浏览器缓存

#### Expires

只要向浏览器输出过期时间 HTTP 协议头，不论是 html 还是动态脚本，都能被缓存。

HTML META

```

<meta http-equive="Expires" content=" Mon, 10 Jan 2000 00:00:00 GMT"/>
<meta http-equive="Cache-Control" content="max-age=300"/>
<meta http-equive="Cache-Control" content="no-cache"/>

```

动态脚本

```
Expires: Mon, 10 Jan 2000 00:00:00 GMT
Cache-Control: max-age=300
Cache-Control: no-cache

header("Expires: " .gmdate ("D, d M Y H:i:s", time() + 3600 * 24 * 7). " GMT");
header("Cache-Control: max-age=300");
header("Cache-Control: no-cache");

```

很多 web server 都提供 Expires 模块

### 提示

有些浏览器可能不支持。

#### If-Modified-Since / Last-Modified

If-Modified-Since 小于 Last-Modified 返回 200

```
neo@neo-OptiPlex-780:/tmp$ curl -I http://www.163.com/
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html; charset=GBK
Transfer-Encoding: chunked
Vary: Accept-Encoding
Expires: Mon, 16 May 2011 08:12:05 GMT
Cache-Control: max-age=80
Vary: User-Agent
Vary: Accept
Age: 38
X-Via: 1.1 ls100:8106 (Cdn Cache Server V2.0), 1.1 lydx156:8106 (Cdn Cache Server V2.0)
Connection: keep-alive
Date: Mon, 16 May 2011 08:11:23 GMT

```

If-Modified-Since 大于 Last-Modified 返回 304

```
neo@neo-OptiPlex-780:/tmp$ curl -H "If-Modified-Since: Fri, 12 May 2012 18:53:33 GMT"  -I http://www.163.com/
HTTP/1.0 304 Not Modified
Content-Type: text/html; charset=GBK
Cache-Control: max-age=80
Age: 41
X-Via: 1.0 ls119:80 (Cdn Cache Server V2.0), 1.0 lydx154:8106 (Cdn Cache Server V2.0)
Connection: keep-alive
Date: Mon, 16 May 2011 08:11:14 GMT
Expires: Mon, 16 May 2011 08:11:14 GMT

```

#### ETag / If-None-Match

```
neo@neo-OptiPlex-780:/tmp$ curl -I http://images.example.com/test/test.html
HTTP/1.1 200 OK
Cache-Control: s-maxage=7200, max-age=900
Expires: Mon, 16 May 2011 09:48:45 GMT
Content-Type: text/html
Accept-Ranges: bytes
ETag: "1984705864"
Last-Modified: Mon, 16 May 2011 09:01:07 GMT
Content-Length: 22
Date: Mon, 16 May 2011 09:33:45 GMT
Server: lighttpd/1.4.26

```

```
neo@neo-OptiPlex-780:/tmp$ curl -H 'If-None-Match: "1984705864"' -I http://images.example.com/test/test.html
HTTP/1.1 304 Not Modified
Cache-Control: s-maxage=7200, max-age=900
Expires: Mon, 16 May 2011 09:48:32 GMT
Content-Type: text/html
Accept-Ranges: bytes
ETag: "1984705864"
Last-Modified: Mon, 16 May 2011 09:01:07 GMT
Date: Mon, 16 May 2011 09:33:32 GMT
Server: lighttpd/1.4.26

```

### CDN/逆向代理缓存

具有代表性的逆向代理服务器：

1.  Squid

2.  Nginx

3.  Varnish

4.  Apache cache module

其它逆向代理服务器

1.  一些提供 cache 的硬件设备

2.  最近几年出现了的 China Cache 服务商，也称 CDN

很多 CDN 厂商使用 Squid 二次开发做为 CDN 节点，通过全球负载均衡使用分发

这些 CDN 厂商主要做了一下二次开发

1.  logs 日志集中

2.  流量限制

3.  push,pull 操作

4.  url 刷新

s-maxage 与 max-age 用法类似，s-maxage 针对代理服务器缓存。同样适用于 CDN

s-maxage 与 max-age 组合使用可以提高 CDN 性能

### 负载均衡设备

F5 Big-IP, Array 等设备都提供硬件加速，其原理与 squid, apache 提供的功能大同小异

其中 Array 页面压缩采用硬件压缩卡实现，SSL 加速也采用硬件实现

### WEB 服务器缓存

例如，通过配置 apache 实现自身 cache

### 应用程序缓存

在这个领域百花齐放，相信你一定能找到适合你的。这些 cache 会为你提供一些 api，来访问它。

代表性的 memcached 据我所是 sina 广泛使用，腾讯也曾经使用过后来开发了 TC(Tencent Cache)，台湾雅虎则使用 APC Cache。

另外模板引擎也有自己的缓存系统

### 数据库缓存

数据库本身就有这个配置选项，如果需要你仍然可以在数据库前面加一道 Cache。

例如 PostgreSQL, MySQL 都提供参数可以将 memcached 编译到它内部

## 静态化

静态化方法包括：

1.  生成方式

2.  抓取方式

3.  伪静态化

4.  混合方式

静态化可以改善 SEO

### 生成方式

主要由程序实现

例如

```

content = "<html><title>my static</title><body>hello world</body></html>"
file = open( your static file)
file.write(content)
file.close()

```

### 抓取方式

主要由程序实现

程序中抓取

```

content = get_url('http://netkiller.8800.org/index.php')
file = open( index.html)
file.write(content)
file.close()

```

使用软件抓取，不仅限于 wget。

```

wget http://netkiller.8800.org/index.php -O index.html

```

这时只给出简单例子，使用复杂参数实现更复杂的拾取，然后将脚本加入 crontab 中可。

### 伪静态化

伪静态化是主要是通过在 URL 上做一些手脚，使你看去是静态的，实质上它是动态脚本。

伪静态化实现主要包括两种方法：

1.  Rewrite rule

2.  path_info

下面是一个 PATH_INFO 例子

http://netkiller.8800.org/zh-cn/photography/browse/2009.html

根本就不存在这个目录'zh-cn/photography/browse/'和文件'2009.html'

下面是一个 Rewrite 例子

http://example.org/bbs/thread-1003872-1-1.html

### 混合方式

其实目前网站使用的基本上都是上面几种方法混合方式。

例如首先将动态 url(example.org/news.php?cid=1&id=1) 通过 rewrite 转换为 (example.org/new_1_1.html)

接下来就比较容易解决了，一种方法是使用 wget example.org/new_1_1.html，另一种方法你无需静态化，直接使用 squid 规则配置让他永不过期

### 静态化中的动态内容

在静态化页面中有一些内容是无法实现静态的。像登录信息，用户评论等等

我们用三种方法实现静态中嵌入动态内容：

1.  iframe - 灵活性差

2.  SSI - 消耗 web 服务器资源

3.  Ajax - 依赖浏览器，稳定性差

## 多媒体数据分离

### 图片服务器分离

为什么要将图片服务器分离出来？

1.  图片通常比较大，下载需要更长的时间，而 web 容器并发数也是相当宝贵的仅次于数据库。

2.  传统浏览器一个窗口只占用一个链接数，目前主流浏览器都支持多线程下载，下载 HTML 页面同时，采用多线程下载其它多媒体数据。

我们举一个例子，你的服务器并发能力只用 1000，早期浏览器不支持多线程，所以同一时刻，你的服务器可以承受 1000 个人同时访问。 但现在不同了，基本所有的浏览器都支持多线程，假如你的页面中有 9 张小图片,同一时刻你的服务器仅仅能应付 1000/10 = 100 个用户。

所以我们要将图片和其他多媒体文件分离出来，单独使用一台服务器处理请求。

### 提示

图片服务器建议使用 lighttpd 与 squid 缓存配合使用效果更好或购买 CDN 的服务。

### 目录层次规划

日期有利于归档

```
/www/images
/www/images/2008
/www/images/2008/01
/www/images/2008/01/01

```

分类不同用途的文件

```
/www/images
/www/images/theme/2009

# article id 000001
/www/images/article/2009/01/000001

# product id 00001
/www/images/product/2009/01/01/00001

# member name neo
/www/images/member/2009/01/01/neo

```

根据你的数据量，创建目录深度, 并且目录深度有规律可循。

虽然 64bit 文件系统不限制文件数量与目录深度，但是我还是建议按我的方式规划目录。

这样规划目录便于缓存控制，如：

images/2008/* 永久缓存

images/2009/* 缓存一个月

images/2010/* 缓存一小时

images/2010/06/* 缓存 5 分钟

### 多域名访问

部分浏览器（IE）相同域名只能创建 2 个线程，在页面中使用多个域名可以解决这个限制

```
img1.example.com IN CNAME images.example.com.
img2.example.com IN CNAME images.example.com.
img3.example.com IN CNAME images.example.com.
...
imgN.example.com IN CNAME images.example.com.

```

## 压缩数据传输

服务器将 html 或脚本输出压缩，用户从服务器取得数据后由浏览器解压

压缩数据传输实现方法：

1.  apache mod_deflate

2.  lighttpd compress module

## 时间同步

将通信网上各种通信设备或计算机设备的时间信息(年月日时分秒)基于 UTC（协调世界时）时间偏差限定在足够小的范围内（如 100ms），这种同步过程叫做时间同步。

关于时间同步我个人的解决方法：

1.  使用 UTC 时间，用户加时区来解决。

2.  保证所有服务器的时间是同步的

```
$ sudo ntpdate asia.pool.ntp.org
21 May 10:34:18 ntpdate[6687]: adjust time server 203.185.69.60 offset 0.031079 sec

```

## 邮件系统

邮件系统：

1.  站内邮件。

2.  电子邮局服务

3.  订阅/推广邮件

### Mailing List

邮件列表系统：

1.  订阅功能

2.  确认订阅功能

3.  退订功能

4.  群发功能

5.  浏览功能(国内基本不需要)

## 日志集中管理

传统做法是文件型日志，分布在各个服务器上。在大规模部署服务器后代来很多不便，增加很多管理成本，所有我们需要集中管理服务器产生的所有日志，我们叫他日志中心服务器

### 系统日志

```
rsyslog/syslog-ng 实现日志集中管理
```

### 应用程序日志

应用程序中没有比较大量记录日志，当开启 debug 模式时才记录大量日志。

但是很多国内开发太过于依赖日志，导致日记非常臃肿

程序日志解决方案，请看软件架构相关章节

## SSL

SSL 加密传输，为电子商务提供交易安全保护，什么时候该使用 SSL 呢：

使用 SSL

1.  用户登录

2.  购物流程

3.  支付

什么时候不使用 SSL? 经过 SSL 加密后，你就失去了很多功能，你不能在对页面做 Cache/CDN，SSL 加密与解密需要耗费你的服务器 CPU 与内存资源，能不使用尽量不使用。

对于 SSL 消耗你服务器资源这方面有两个方案解决

1.  将 SSL 证书安装到 CDN 上，目前蓝讯，网宿等等 CDN 厂商都提供 SSL 服务。我与上两家技术人员沟通过，也安装了证书实际测试一下，你可以放心是使用。

2.  将 SSL 证书安装到负载均衡设备，这些设备都采用专用硬件处理 SSL 请求，我测试过 F5，Array，Banggoo

采用上面两种方案，无需改变你目前的服务器配置，他们的原理是

```

user (https://www.example.com) --> CDN or SLB (SSL) --> http://www.example.com

```

用户访问 https,到达 CDN 或者负载均衡，CDN/SLB 通过 http://请求源站，然后将内容 SSL 加密，返回给用户，这样用户得到的是加密内容。

用户提交数据，交给 CDN/SLB，CDN/SLB 将 SSL 加密数据卸载证书，然后将解密后数据发回源站。

CDN 与 SLB 加载卸载证书原理很简单，不难理解。

我来教你 DIY 一个，你可以使用 Squid，Nginx，Apache 等等反向代理服务器，将证书安装在反向代理上，请求源站仍然采用 http。

### SSL 注意事项

你如果认为把 SSL 挂载到网站前端就，大功告成，完事了，那你错了。

幸运的话你会成功，但有时的时候你发现你的证书不被信任。如果你是个细心的人，你会发现单个图片，或者你创建换成测试文件 echo helloworld > index.html 证书都是 OK 的。

这个问题出在你的 html 页面中，安装有 SSL 证书的网站，不能有外链 js,flash 等等不安全内容。

## Storage 存储

### 存储种类

DAS、NAS、SAN

#### Direct Attached Storage

PC + Raid Card ====== Array

#### Network-attached storage

NAS 说白了就是一个嵌入式电脑，经过精简内核的 Linux,通过 samba,nfs,WebDav,ftp...等等方式实现共享存储

如果你有兴趣，可以 DIY 一个 NAS，使用 Openfiler

#### Storage area network

只要你有￥什么都好说

##### FC SAN

FC 是光纤通道网络存储，需要专用交换机与 HBA 卡

提供 6G/8G 数据传输

##### IP SAN

1G/10G iSCSI，采用 TCP/IP 协议传输 SCSI 指令

客户端不需要专门的 HBA 卡，专业 iSCSI HBA 目前非常昂贵

##### FCoE (Fibre Channel over Ethernet)

因为 iSCSI 很廉价，FC 市场被 iSCSI 蚕食，传统 FC 收到 iSCSI 压力。推出新一代协议，希望能在现有光纤通道的成功基础上，借助于以太网的力量重新保持自身在数据中心存储局域网中的霸主地位。

iSCSI 通过 TCP/IP 协议在可能产生损耗或阻塞的局域网和宽带网上传送数据存储块。相比之下，FCoE 则只是利用了以太网的拓展性，并保留了光纤通道在高可靠性和高效率方面的优势。

### RAID

#### 缓存服务器

全部采用 RAID 0

一旦出现问题，立即将其从集群中踢出去，带节点故障排除后，恢复它的功能。

#### Web 服务器

采用 RAID 1

服务器仅仅存放脚本程序，数据建议放在外挂存储上。

#### 数据库

主服务器：建议采用 RAID 10

数据库节点：建议采用 RAID 10

数据库应尽量避免使用 RAID 5，RAID 5 在做校验过程时，效率会很低。

数据库节点一旦出现问题，立即从集群中撤出，排除故障后，在回复使用。

#### 数据备份

数据备份服务器建议采用 RAID 5/6

RAID 5 阵列容量计算公式 ：

可用容量 =（n-1）/n 的总磁盘容量（n 为磁盘数）

### File System 文件系统

我个人推荐使用 ext4, xfs 或 reiserfs

zfs 也不错

#### Distributed File System(DFS)

RAID 0 提高吞吐能力是有限的，IO 也会有瓶颈，NAS 吞吐能力一样有限，SAN 价格不菲。

DFS 是一个不错的选择

### 数据访问协议

```
• 光纤通道管理
• iSCSI
• IP/RDMA
• iSER
• SRP
• NFS v3 和 v4
• CIFS
• HTTP
• WebDAV
• FTP
• NDMP v4

```

### 数据管理

#### Share 共享

#### Mirror 远程镜像同步

#### 压缩与重复数据消除

EMC Data Domain

开源 Opendedup

#### Backup 备份与恢复

Bacula/Zmanda

#### 故障报告

## 磁盘快照

下面流程是自动化完成，这里分部讲解

过程 3.1. 升级操作流程

1.  数据备份

    通常绝大多数人，备份还采用 cp / tar / 以及稍微有点技术含量的 rsync 做差异备份 例如

    ```

    cp -r /www/example.com/www.example.com /backup/www.example.com-2016-05-23
    tar zcvf www.example.com-2016-05-23.tgz /www/example.com/www.example.com

    rsync -auzv /www/example.com/www.example.com /backup/www.example.com-2016-05-23

    ```

    这种备份适合比较小的软件包，对于图片服务器什么的就比较耗时。我很早就开始尝试使用快照备份当时使用 LVM，后来转为 Btrfs 文件系统，到 2010 的时候 btrfs 快照已经非常成熟.

    ```

    [root@www.netkiller.cn www]# btrfs subvolume snapshot /www /www/backup_2016-05-23
    Create a snapshot of '/www' in '/www/backup_2016-05-23'

    ```

    快照瞬间建立，使用下面命令查看快照

    ```

    [root@www.netkiller.cn www]# btrfs subvolume list /www
    ID 284 gen 18583 top level 5 path backup_2016-05-23

    ```

    挂载快照

    ```

    [root@www.netkiller.cn www]# mount -t btrfs -o subvol=backup_2016-05-23 /dev/xvdb1 /mnt
    [root@www.netkiller.cn www]# ll /mnt/

    ```

    关于 BTRFS 详细使用方法，请参考 [《Netkiller Linux 手札》](http://www.netkiller.cn/linux/index.html)