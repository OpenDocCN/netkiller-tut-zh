# 部分 V. Mail Server

## 第 59 章 Mail server constituent

Mail Transfer Agent (MTA) : sendmail, Postfix, and Exim

Mail Delivery Agent (MDA) : procmail and maildrop

Mail User Agent (MUA) : An e-mail client

## 第 60 章 mail user agent (MUA)

## 1. mail

mail 默认使用 sendmail 命令发送邮件

```
cat /etc/fstab | mail -s "Hello" netkiller@msn.com

```

通过 SMTP 发送邮件，创建 /etc/mail.rc 配置文件

```
vim /etc/mail.rc 

--- 增加如下内容 ---

set from=yourname@your-domain.com
set smtp=mail.your-domain.com
set smtp-auth-user=yourname
set smtp-auth-password=yourpasswd
set smtp-auth=login

```

## 2. mutt - text-based mailreader supporting MIME, GPG, PGP and threading

install

```

$ sudo apt-get install mutt

```

how to use the Maildir format with the Mutt

```

$ vim ~/.muttrc

alias rooty Cron Daemony <root@netkiller>
set mbox_type=Maildir
set folder="~/Maildir"
set mask="!^\\.[^.]"
set mbox="~/Maildir"
set record="+.Sent"
set postponed="+.Drafts"
set spoolfile="~/Maildir"

mailboxes `echo -n "+ "; find ~/Maildir -maxdepth 1 -type d -name ".*" -printf "+'%f' "`
macro index c "<change-folder>?<toggle-mailboxes>" "open a different folder"
macro pager c "<change-folder>?<toggle-mailboxes>" "open a different folder"
macro index C "<copy-message>?<toggle-mailboxes>" "copy a message to a mailbox"
macro index M "<save-message>?<toggle-mailboxes>" "move a message to a mailbox"
macro compose A "<attach-message>?<toggle-mailboxes>" "attach message(s) to this message"

```

### 2.1. 发送邮件

同时携带附件.

```
mutt -s "helloworld" user@example.com -a /opt/backup/file.tar.gz

```

### 2.2. 设置自定义 From

```

#设置邮件编码方式
set charset="utf-8" 

#自定义发件人信息 
set envelope_from=yes
set use_from=yes 
set from=netkiller@netkiller.cn
set realname="Neo Chen"			

```

## 3. alpine - Text-based email client, friendly for novices but powerful

```
$ sudo apt-get install alpine

```

## 4. fetchmail - SSL enabled POP3, APOP, IMAP mail gatherer/forwarder

## 5. GPG4WIN

http://www.gpg4win.org/

## 6. Evolution

http://www.gpg4win.org/

## 第 61 章 exim - meta-package to ease Exim MTA (v4) installation

## 1. install

### 1.1. ubuntu/debian

```
$ sudo apt-get install exim4

```

#### 1.1.1. configure

```
$ sudo dpkg-reconfigure exim4-config

```

### 1.2. CentOS/Redhat

```
# yum install exim
# chkconfig exim on
# cp /etc/exim/exim.conf{,.original}

```

切换默认 MTA

```
# alternatives --config mta

There are 2 programs which provide 'mta'.

  Selection    Command
-----------------------------------------------
*  1           /usr/sbin/sendmail.postfix
 + 2           /usr/sbin/sendmail.exim

Enter to keep the current selection[+], or type selection number: 

```

## 2. exim 命令

### 2.1. 帮助信息

```

[root@localhost ~]# exim -bV
Exim version 4.91 #2 built 22-Aug-2018 14:16:00
Copyright (c) University of Cambridge, 1995 - 2018
(c) The Exim Maintainers and contributors in ACKNOWLEDGMENTS file, 2007 - 2018
Berkeley DB: Berkeley DB 5.3.21: (May 11, 2012)
Support for: crypteq iconv() IPv6 PAM Perl Expand_dlfunc OpenSSL Content_Scanning DKIM DNSSEC Event OCSP PRDR TCP_Fast_Open
Lookups (built-in): lsearch wildlsearch nwildlsearch iplsearch cdb dbm dbmjz dbmnz dnsdb dsearch ldap ldapdn ldapm nis nis0 nisplus passwd sqlite
Authenticators: cram_md5 cyrus_sasl dovecot gsasl plaintext spa tls
Routers: accept dnslookup ipliteral manualroute queryprogram redirect
Transports: appendfile/maildir/mailstore/mbx autoreply lmtp pipe smtp
Malware: f-protd f-prot6d drweb fsecure sophie clamd avast sock cmdline
Fixed never_users: 0
Configure owner: 0:0
Size of off_t: 8
Configuration file is /etc/exim/exim.conf			

```

### 2.2. 测试发送邮件

```

[root@localhost ~]# exim -v root
This is a test message
Ctrl + D 结束			

```

### 2.3. 刷新邮件队列

```
exim -qff ; tail -f /var/log/exim/main.log				

```

## 3. 配置 exim

### 3.1. /etc/aliases 别名配置

发往 root 的邮件会重定向到 me@example.com

```
vim /etc/aliases
root:		me@example.com

```

## 4. FAQ

### 4.1. Mailing to remote domains not supported

```
$ sudo vim /etc/exim4/update-exim4.conf.conf

#dc_eximconfig_configtype='local'
dc_eximconfig_configtype='internet'

```

## 第 62 章 postfix - High-performance mail transport agent

[Postfix 主页](http://www.postfix.org/)

## 1. install

### 1.1. Ubuntu

```
				$ sudo apt install postfix

```

configure

```
				$ sudo dpkg-reconfigure postfix-config

```

### 1.2. CentOS

```
				# yum install -y postfix

```

```
				myhostname = mail.example.com
				mydomain = example.com
				myorigin = $mydomain
				inet_interfaces = all
				mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
				#mynetworks = 192.168.0.0/24, 127.0.0.0/8
				#relay_domains =
				home_mailbox = Maildir/

```

### 1.3. OSCM 通过配置管理脚本安装

```
				Postfix Install

				# Centos Init
				curl -s https://raw.githubusercontent.com/oscm/shell/master/os/centos7.sh | bash
				curl -s https://raw.githubusercontent.com/oscm/shell/master/os/selinux.sh | bash
				curl -s https://raw.githubusercontent.com/oscm/shell/master/os/iptables/iptables.sh | bash
				curl -s https://raw.githubusercontent.com/oscm/shell/master/os/ntpd/ntp.sh | bash
				curl -s https://raw.githubusercontent.com/oscm/shell/master/os/ssh/sshd_config.sh | bash

				# Install Postfix
				curl -s
				https://raw.githubusercontent.com/oscm/shell/master/mail/postfix/postfix.sh | bash

```

## 2. 配置 Postfix

### 2.1. 转发配置

修改配置文件

```
				vim /etc/postfix/main.cf

				inet_interfaces = all

				mydestination =

				mydomain = example.com

				myhostname = mail.example.com

				mynetworks = 0.0.0.0/0

				mynetworks_style = subnet

				smtpd_reject_unlisted_recipient = no

				transport_maps = hash:/etc/postfix/transport

```

转发配置，设置域名和地址的关系：

```
				vim transport：

				your.com relay: [10.10.0.1]

```

生成相应的 db 文件

```
				postmap transport

```

例如当收件人为 users@your.com 时，postfix 会将邮件转发到指定的服务器

### 2.2. 拒收垃圾邮件

编辑/etc/postfix/main.cf 文件,在文件中添加下面一行文字，你可以把它插入到文件末尾。

```
				sudo vim /etc/postfix/main.cf

				smtpd_recipient_restrictions = check_sender_access hash:/etc/postfix/check_sender_access

```

然后在/etc/postfix/目录下创建一个 check_sender_access 文件，内容如下

```
				example.com REJECT
				your.com OK

				.example.com REJECT
				.your.com OK

				user@example.com REJECT

```

将域名的特定邮箱地址添加到黑名单,也可以将某个二级域名添加到黑名单或白名单，只要在域名前面加上一个小数点就行了。邮箱与域名后面输入 OK 表示将这个域名添加到白名单，域名后面添加 REJECT 表示将这个域名添加到黑名单。

使用 postmap 命令创建/etc/postfix/sender_checks.db 数据库文件

```
				postmap /etc/postfix/check_sender_access

```

最后重新加载 Postfix 配置文件

```
				sudo /etc/init.d/postfix reload

```

### 2.3. 收件箱配置

Postfix 提供三种收件箱，第一种是 Mailbox,第二种是 Maildir, 第三种是 Unix 风格的收件想/var/spool/mail

如你有 POP/IMAP 服务请使用 Mailbox 或者 Maildir。否则仅仅是在 unix 上阅读纯文本邮件可以使用/var/spool/mail

#### 2.3.1. Mailbox 配置

```
					home_mailbox = Mailbox

```

#### 2.3.2. Maildir 配置

```
					home_mailbox = Maildir/

```

#### 2.3.3. 传统 Unix 风格邮箱配置

```
					mail_spool_directory = /var/mail

```

```
					mail_spool_directory = /var/spool/mail

```

### 2.4. 邮件投递

邮件投递是指从你的 Postfix 服务器将邮件投到目的地邮件服务器，即 SMTP 对 SMTP，而非用户到的 SMTP 配置。

配置主要涉及邮件投递频率，如果过高，会被退回也可能被封锁一段时间。

```
				* initial_destination_concurrency：到目标主机的初始化并发连接数。
				* default_destination_concurrency_limit：初始化连接后对同一目标主机的最大并发连接数目。
				* local_destination_concurrency_limit：控制对同一本地收件人的最大同时投递的邮件数目。

```

默认值可以通过 $ postconf | grep local_destination_concurrency_limit 命令查看

```
				initial_destination_concurrency = 5
				default_destination_concurrency_limit = 20
				local_destination_concurrency_limit = 2

```

### 2.5. 队列配置

queue_run_delay 配置间隔多长时间重新发送一次 deferred 队列的邮件

```
				# postconf | grep queue_run_delay
				queue_run_delay = 300s

```

deferred 邮件队列中的生存时间

```
				# postconf | grep maximal_queue_lifetime
				maximal_queue_lifetime = 5d

```

队列尺寸

```
				# postconf | grep qmgr_
				qmgr_clog_warn_time = 300s
				qmgr_daemon_timeout = 1000s
				qmgr_fudge_factor = 100
				qmgr_ipc_timeout = 60s
				qmgr_message_active_limit = 20000
				qmgr_message_recipient_limit = 20000
				qmgr_message_recipient_minimum = 10

```

### 2.6. 客户端

smtpd_client_connection_count_limit 配置邮件客户端链接数，例如 Outlook 用户数量

```
				# postconf | grep smtpd_client_connection_count_limit
				postscreen_client_connection_count_limit = $smtpd_client_connection_count_limit
				smtpd_client_connection_count_limit = 50

```

控制接收邮件频率

```
				# postconf | grep smtpd_client_connection_rate_limit
				smtpd_client_connection_rate_limit = 0

```

### 2.7. SMTP 发送权限相关配置

```

neo@netkiller ~ % postconf -n|egrep 'smtpd_recipient_restrictions|smtpd_relay_restrictions'
smtpd_recipient_restrictions = permit_mynetworks
smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination permit_inet_interfaces			

```

## 3. aliases

查找别名文件地址

```
			# postconf alias_maps
			alias_maps = hash:/etc/aliases

```

增加别名

```
			# vim /etc/aliases

			neo: netkiller@msn.com

```

newaliases - rebuild the data base for the mail aliases file

## 4. dkim

DKIM(DomainKeys Identified Mail) 是一种电子邮件的验证技术，使用密码学的基础提供了签名与验证的功能。DKIM 能增加你邮件的信任度。

安装 OpenDKIM 环境是 CentOS 7

```
			yum install -y opendkim

```

查看配置文件

```
			[root@mail.netkiller.cn ~]# egrep -v "^#|^$" /etc/opendkim.conf
			PidFile /var/run/opendkim/opendkim.pid
			Mode sv
			Syslog yes
			SyslogSuccess yes
			LogWhy yes
			UserID opendkim:opendkim
			Socket inet:8891@localhost
			Umask 002
			SendReports yes
			SoftwareHeader yes
			Canonicalization relaxed/relaxed
			Selector default
			MinimumKeyBits 1024
			KeyFile /etc/opendkim/keys/default.private
			KeyTable /etc/opendkim/KeyTable
			SigningTable refile:/etc/opendkim/SigningTable
			InternalHosts refile:/etc/opendkim/TrustedHosts
			OversignHeaders From

```

生成公钥和私钥 example.com 替换成你的域名

```
			mkdir /etc/opendkim/keys/example.com
			opendkim-genkey -D /etc/opendkim/keys/example.com/ -d example.com -s default
			chown -R opendkim: /etc/opendkim/keys/example.com
			ln -s /etc/opendkim/keys/example.com/default.private /etc/opendkim/keys/default.private

```

将你域名 example.com 添加到/etc/opendkim/KeyTable 格式如下：

```
			default._domainkey.example.com example.com:default:/etc/opendkim/keys/example.com/default.private

```

接下来修改 /etc/opendkim/SigningTable 并添加如下记录

```
			*@example.com default._domainkey.example.com

```

添加信任主机到/etc/opendkim/TrustedHosts，通常是 example.com / mail.example.com

```
			example.com
			mail.example.com

```

注意：TrustedHosts 是发送邮件机器的 IP，不是邮件服务器的 IP，例如你的 WEB 服务器连接到邮件服务器发送电子邮件，那么 TrustedHosts 就是你的 WEB 服务器 IP 地址。

至此 opendkim 已经配置完毕。

现在需要配置域名 TXT 记录解析，开打文件 /etc/opendkim/keys/example.com/default.txt 参照下面配置

```
			cat /etc/opendkim/keys/example.com/default.txt
			default._domainkey IN TXT ( "v=DKIM1; k=rsa; "
			"p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC5anjIUkTgJT8DSBL2tiydi6DZLIMnPnveFBcyKshwIuGeRzIN2PwQW5F/bvQWdatPLGuw0w5mKXtATJtarbWXy89BgjcJgAGrPSr8GdzsNH0RXRqTy1A21BQyGER3Mx2Fbr6J62reTG2i7jY0w3/cxzuFIGlSn/RP/KrlMze4zQIDAQAB" ) ; ----- DKIM key default for example.com

```

接下来配置 postfix 把 OpenDKIM 整合到 Postfix 修改/etc/postfix/main.cf

```
			smtpd_milters = inet:127.0.0.1:8891
			non_smtpd_milters = $smtpd_milters
			milter_default_action = accept
			milter_protocol = 2

```

启动 opendkim，重启 postfix

```
			systemctl enable opendkim.service
			systemctl start opendkim.service
			systemctl restart postfix.service

```

检查 opendkim 状态与端口

```
			# systemctl status opendkim.service
			● opendkim.service - DomainKeys Identified Mail (DKIM) Milter
			Loaded: loaded (/usr/lib/systemd/system/opendkim.service; enabled; vendor preset: disabled)
			Active: active (running) since Thu 2016-08-25 02:07:42 EDT; 6s ago
			Docs: man:opendkim(8)
			man:opendkim.conf(5)
			man:opendkim-genkey(8)
			man:opendkim-genzone(8)
			man:opendkim-testadsp(8)
			man:opendkim-testkey
			http://www.opendkim.org/docs.html
			Process: 12577 ExecStart=/usr/sbin/opendkim $OPTIONS (code=exited, status=0/SUCCESS)
			Main PID: 12578 (opendkim)
			CGroup: /system.slice/opendkim.service
			└─12578 /usr/sbin/opendkim -x /etc/opendkim.conf -P /var/run/opendkim/opendkim.pid

			Aug 25 02:07:42 localhost.localdomain systemd[1]: Starting DomainKeys Identified Mail (DKIM) Milter...
			Aug 25 02:07:42 localhost.localdomain systemd[1]: Started DomainKeys Identified Mail (DKIM) Milter.
			Aug 25 02:07:42 localhost.localdomain opendkim[12578]: OpenDKIM Filter v2.10.3 starting (args: -x /etc/opendkim.conf -P /var/run/opendkim/opendkim.pid)

			# ss -lnt |
			grep 8891
			LISTEN 0 128 127.0.0.1:8891 *:*

```

### 4.1. 增加域名

创建证书

```
				mkdir /etc/opendkim/keys/mydomain.com
				opendkim-genkey -D /etc/opendkim/keys/mydomain.com/ -r -d mydomain.com
				chown -R opendkim: /etc/opendkim/keys/mydomain.com

```

配置 KeyTable

```
				default._domainkey.mydomain.com mydomain.com:default:/etc/opendkim/keys/mydomain.com/default.private

```

配置 SigningTable

```
				*@mydomain.com default._domainkey.mydomain.com

```

### 4.2. 测试

/var/log/maillog

```

Aug 26 03:02:03 localhost postfix/smtpd[5837]: connect from unknown[155.133.82.144]
Aug 26 03:02:03 localhost opendkim[5762]: configuration reloaded from /etc/opendkim.conf
Aug 26 03:02:04 localhost postfix/smtpd[5837]: lost connection after AUTH from unknown[155.133.82.144]
Aug 26 03:02:04 localhost postfix/smtpd[5837]: disconnect from unknown[155.133.82.144]
Aug 26 03:02:09 localhost postfix/smtpd[5837]: connect from unknown[202.130.101.34]
Aug 26 03:02:10 localhost postfix/smtpd[5837]: 27EEC802C1C5: client=unknown[202.130.101.34]
Aug 26 03:02:10 localhost postfix/cleanup[5843]: 27EEC802C1C5: message-id=<1770496307.0.1472194929612@Server>
Aug 26 03:02:10 localhost opendkim[5762]: 27EEC802C1C5: DKIM-Signature field added (s=default, d=mydomain.com)
Aug 26 03:02:10 localhost postfix/qmgr[4605]: 27EEC802C1C5: from=<neo@netkiller.cn>, size=531, nrcpt=1 (queue active)
Aug 26 03:02:10 localhost postfix/smtpd[5837]: disconnect from unknown[202.130.101.34]
Aug 26 03:02:10 localhost postfix/smtp[5844]: connect to gmail-smtp-in.l.google.com[2607:f8b0:400e:c03::1b]:25: Network is unreachable
Aug 26 03:02:11 localhost postfix/smtp[5844]: 27EEC802C1C5: to=<netkiller@msn.com>, relay=gmail-smtp-in.l.google.com[74.125.25.26]:25, delay=1.6, delays=0.58/0.01/0.48/0.49, dsn=2.0.0, status=sent (250 2.0.0 OK 1472194931 om6si19759602pac.41 - gsmtp)
Aug 26 03:02:11 localhost postfix/qmgr[4605]: 27EEC802C1C5: removed			

```

查看原件原文，如果正常会显示 DKIM-Filter 和 DKIM-Signature 两项

```

Delivered-To: netkiller@msn.com
Received: by 10.28.169.3 with SMTP id s3csp180808wme;
        Fri, 26 Aug 2016 00:02:11 -0700 (PDT)
X-Received: by 10.66.10.234 with SMTP id l10mr3141577pab.69.1472194931522;
        Fri, 26 Aug 2016 00:02:11 -0700 (PDT)
Return-Path: <neo@netkiller.cn>
Received: from mail.mydomain.com ([104.243.134.186])
        by mx.google.com with ESMTP id om6si19759602pac.41.2016.08.26.00.02.11
        for <netkiller@msn.com>;
        Fri, 26 Aug 2016 00:02:11 -0700 (PDT)
Received-SPF: pass (google.com: domain of neo@netkiller.cn designates 104.243.134.186 as permitted sender) client-ip=104.243.134.186;
Authentication-Results: mx.google.com;
       dkim=temperror (no key for signature) header.i=@mydomain.com;
       spf=pass (google.com: domain of neo@netkiller.cn designates 104.243.134.186 as permitted sender) smtp.mailfrom=neo@netkiller.cn
Received: from Server (unknown [202.130.101.34])
	by mail.mydomain.com (Postfix) with ESMTP id 27EEC802C1C5
	for <netkiller@msn.com>; Fri, 26 Aug 2016 03:02:09 -0400 (EDT)
DKIM-Filter: OpenDKIM Filter v2.10.3 mail.mydomain.com 27EEC802C1C5
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed; d=mydomain.com;
	s=default; t=1472194930;
	bh=aTYsMuMwFaanDPkTLEncpu/hxKsNsCaozbJRmQJ6aho=;
	h=Date:From:To:Subject:From;
	b=qPYy2TPDv+zxHQ2gqGOwVsgRm42E3p6WvSxdXgUaLtkY6LH6657cdEa96HYJLVqHC
	 EygkTz+3n7WePhGH9jAJrb/PBrGIK1XVCREz4ayfUxc3QUwFSQ9o+5ULkExxdhyRUu
	 4TiCbkcUMbYI3YXJqGiU0OBCyTq655trOaWBby+k=
Date: Fri, 26 Aug 2016 15:02:09 +0800 (CST)
From: neo@netkiller.cn
To: netkiller@msn.com
Message-ID: <1770496307.0.1472194929612@Server>
Subject: =?UTF-8?B?5Li76aKY77ya566A5Y2V6YKu5Lu2?=
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: base64

5rWL6K+V6YKu5Lu25YaF5a65			

```

## 5. Rspamd

Rspamd 是一个反垃圾邮件系统，因为使用事件模型和正则表达式优化，其设计工作速度比 SpamAssassin 还 要快。目前推出的功能： regexp 规则过滤的不同部分的信息;一些内置的功能分析的信息;模糊哈希支持; SURBL 滤波器;电子邮件和性质表支持;控制界面进行远程管理和统计信息收集，一个 Perl 和卢阿插件系统;统计支持（定向结构刨花板/簸扬） ;兼容 SpamAssassin ;和一个客户端程序的电子邮件扫描。类似的规则， rspamd 约 10 倍 SpamAssassin 。

## 6. /var/log/maillog

邮件正常发送时的日志

```

# grep '7905611F797'  maillog
Nov  2 16:07:58 smtp2.example.com postfix/pickup[7377]: 7905611F797: uid=0 from=<root>
Nov  2 16:07:58 smtp2.example.com postfix/cleanup[7683]: 7905611F797: message-id=<20151102080758.GA7677@smtp2.example.com>
Nov  2 16:07:58 smtp2.example.com postfix/qmgr[21697]: 7905611F797: from=<root@mail2.example.com>, size=461, nrcpt=1 (queue active)
Nov  2 16:08:08 smtp2.example.com postfix/smtp[7674]: 7905611F797: to=<skyline.chen@icloud.com>, relay=mx3.mail.icloud.com[17.172.34.64]:25, delay=10, delays=0.04/0/6.2/4.1, dsn=2.5.0, status=sent (250 2.5.0 Ok.)
Nov  2 16:08:08 smtp2.example.com postfix/qmgr[21697]: 7905611F797: removed

```

被封 IP 地址

```

Nov  2 15:25:57 smtp2.example.com postfix/cleanup[6993]: C17AC11F78C: message-id=<20151102072557.C17AC11F78C@mail2.example.com>
Nov  2 15:25:57 smtp2.example.com postfix/bounce[6992]: 0E6FE11F777: sender non-delivery notification: C17AC11F78C
Nov  2 15:25:57 smtp2.example.com postfix/qmgr[21697]: C17AC11F78C: from=<>, size=17147, nrcpt=1 (queue active)
Nov  2 15:25:58 smtp2.example.com postfix/smtp[6928]: C17AC11F78C: to=<cs@example.com>, relay=mx.qiye.163.com[123.125.50.217]:25, delay=0.96, delays=0/0/0.53/0.42, dsn=5.0.0, status=bounced (host mx.qiye.163.com[123.125.50.217] said: 554 DT:SPM mx6, Q9OowEC5hUgGEDdWRyf1AQ--.1S2 1446449158 http://mail.163.com/help/help_spam_16.htm?ip=202.82.201.90&hostid=mx6&time=1446449158 (in reply to end of DATA command))
Nov  2 15:25:58 smtp2.example.com postfix/qmgr[21697]: C17AC11F78C: removed

```

发送密度过高

```

Nov  2 15:24:25 smtp2.example.com postfix/smtpd[6940]: 6D21E11F76A: client=unknown[172.18.52.137]
Nov  2 15:24:25 smtp2.example.com postfix/cleanup[6945]: 6D21E11F76A: message-id=<17f164cf2441ad60eb2ce794db4959bf@localhost.localdomain>
Nov  2 15:24:25 smtp2.example.com postfix/qmgr[21697]: 6D21E11F76A: from=<cs@example.com>, size=15050, nrcpt=1 (queue active)
Nov  2 15:24:25 smtp2.example.com postfix/smtp[6922]: 6D21E11F76A: lost connection with mx3.QQ.com[103.7.30.40] while performing the HELO handshake
Nov  2 15:24:30 smtp2.example.com postfix/smtp[6922]: 6D21E11F76A: to=<1141096962@qq.com>, relay=mx2.QQ.com[184.105.206.86]:25, delay=5.2, delays=0.01/0/4.9/0.35, dsn=5.0.0, status=bounced (host mx2.QQ.com[184.105.206.86] said: 550 Connection frequency limited. http://service.mail.qq.com/cgi-bin/help?subtype=1&&id=20022&&no=1000722 (in reply to MAIL FROM command))
Nov  2 15:24:30 smtp2.example.com postfix/bounce[6946]: 6D21E11F76A: sender non-delivery notification: A76A511F777
Nov  2 15:24:30 smtp2.example.com postfix/qmgr[21697]: 6D21E11F76A: removed

```

虚假地址，产生 Connection timed out

```

Nov  2 16:32:21 smtp2.example.com postfix/smtp[7732]: 1DCD811F940: to=<1608014274@qqq.com>, relay=none, delay=368099, delays=368069/0.05/30/0, dsn=4.4.1, status=deferred (connect to qqq.com[60.190.249.48]:25: Connection timed out)		

```

### 6.1. 计算每分钟发送数量日志统计

计算每分钟发送数量

```
				# grep 'to=' maillog | grep '15:25:' | wc -l
				592

```

### 6.2. 虚假地址统计

计算每分钟发送数量

```

# egrep -o "to=<(.*)>, .* Connection timed out" maillog | sed -e "s/to=<\(.*\)>.*/\1/"

```

## 7. Post 命令

### 7.1. postconf - Postfix configuration utility

Postfix 提供了 postconf 配置工具,配置 Postfix 有两种方法，第一种方法是使用文本编辑工具修改 main.cf 和 master.cf 两个配置文件，第二种方法就是使用 postconf 命令

修改配置项

```
				postconf -e "myhostname=mail.netkiller.cn"

```

### 7.2. postsuper

删除队列中待发邮件

```
				# mailq
				-Queue ID- --Size-- ----Arrival Time---- -Sender/Recipient-------
				CB71F8022974 3038 Wed Oct 19 01:57:03 MAILER-DAEMON
				(connect to example.com[2606:2800:220:1:248:1893:25c8:1946]:25: Network is unreachable)
				root@example.com

				-- 3 Kbytes in 1 Request.

				# postsuper -d CB71F8022974 deferred
				postsuper: CB71F8022974: removed
				postsuper: Deleted: 1 message

				# mailq
				Mail queue is empty

```

删除队列中所有待发邮件

```
				postsuper -d ALL deferred

```

### 7.3. postqueue - Postfix queue control

#### 7.3.1. 列出队列

列出队列,等效 mailq

```
					# postqueue -p

```

#### 7.3.2. 刷新队列

-f Flush the queue: attempt to deliver all queued mail.

```
					postqueue -f

```

### 7.4. postmulti - Postfix multi-instance manager

#### 7.4.1. 绑定 IP 地址

将所有 IP 地址绑定到服务器上

```
					cd /etc/sysconfig/network-scripts

					vim ifcfg-enp2s0

```

```
					# cat ifcfg-enp2s0
					TYPE="Ethernet"
					BOOTPROTO="none"
					DEFROUTE="yes"
					IPV4_FAILURE_FATAL="no"
					IPV6INIT="yes"
					IPV6_AUTOCONF="yes"
					IPV6_DEFROUTE="yes"
					IPV6_FAILURE_FATAL="no"
					NAME="enp2s0"
					UUID="c27c6ef8-ab82-4019-af0a-9f3a70b2d230"
					DEVICE="enp2s0"
					ONBOOT="yes"
					DNS1="8.8.8.8"
					IPADDR="192.168.0.1"
					...
					...
					IPADDR247="192.168.0.250"
					PREFIX="26"
					PERFIX0="24"
					GATEWAY="192.168.0.254"
					IPV6_PEERDNS="yes"
					IPV6_PEERROUTES="yes"
					IPV6_PRIVACY="no"

```

IP 范围 192.168.0.1-192.168.0.250，接口是 enp2s0，enp2s0:1 ~ enp2s0:250

#### 7.4.2. postfix 多实例配置

初始化 postfix 多实例

```
					postmulti -e init

```

创建 postfix 实例

```
					postmulti -I postfix-1 -G mta -e create
					...
					...
					postmulti -I postfix-250 -G mta -e create

```

启用 postfix 实例

```
					postmulti -i postfix-1 -e enable
					...
					...
					postmulti -i postfix-250 -e enable

```

配置 postfix 实例

```
					postmulti -i postfix-1 -x postconf -e "master_service_disable =" "authorized_submit_users = root" "minimal_backoff_time= 30d" "maximal_backoff_time = 300d" "mynetworks = 127.0.0.0/8,192.168.0.0/24" "inet_interfaces = \$myhostname" "mailbox_size_limit = 0" "message_size_limit = 0" "myhostname = mail.example.com" "myorigin = mail.example.com" "mydomain = example.com" "smtp_bind_address = 192.168.0.1"
					...
					...
					postmulti -i postfix-250 -x postconf -e "master_service_disable =" "authorized_submit_users =
					root" "minimal_backoff_time= 30d" "maximal_backoff_time = 300d" "mynetworks = 127.0.0.0/8,192.168.0.0/24" "inet_interfaces = \$myhostname" "mailbox_size_limit = 0" "message_size_limit = 0" "myhostname = mail.example.com" "myorigin = mail.example.com" "mydomain = example.com" "smtp_bind_address = 192.168.0.250"

```

#### 7.4.3. 配置 iptables 让 SMTPD 发送邮件时依次轮询外发 IP 地址，这样就不会被封锁。

```
					iptables -t nat -I POSTROUTING -m state --state NEW -p tcp --dport 25 -o eth0 -m statistic --mode nth --every 250 -j SNAT --to-source 192.168.0.1
					...
					...
					iptables -t nat -I POSTROUTING -m state --state NEW -p tcp --dport 25 -o eth0 -m statistic --mode nth --every 250 -j SNAT --to-source 192.168.0.250

```

注意，不要使用下面的方式配置 iptables，经过测试这种 192.168.0.1-192.168.0.250 方式，不会轮换 IP 地址。

```
					iptables -t nat -I POSTROUTING -o enp2s0f0 -p tcp -m state --state NEW -m tcp -m statistic --mode nth --every 5 --packet 0 -j SNAT --to-source 192.168.0.1-192.168.0.250

```

测试 iptables 使用 curl 每次请求你将看到一个全新的 IP 地址。

```
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：173.254.223.57 来自：美国 QuadraNet
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：173.254.223.54 来自：美国 QuadraNet
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：107.167.40.137 来自：美国
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：173.254.223.55 来自：美国 QuadraNet
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：107.167.40.134 来自：美国
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：173.254.223.56 来自：美国 QuadraNet
					[root@www.netkiller.cn ~]# curl
					http://ip.cn
					当前 IP：173.254.223.54 来自：美国 QuadraNet
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：107.167.40.132 来自：美国
					[root@www.netkiller.cn ~]# curl http://ip.cn
					当前 IP：173.254.223.53 来自：美国 QuadraNet

```

使用 netkiller-firewall 替代原来的 iptables，传统的 iptables 规则不容易书写，也不容易阅读。

```
					# unzip firewall-master.zip
					# yum install -y python34
					# bash install.sh
					# /etc/init.d/firewall
					Usage: /etc/init.d/firewall {start|stop|status|restart}

```

```

					RULE=www
					改为
					RULE=smtp

					# cat /etc/init.d/firewall | grep RULE
					RULE=smtp

					# cat /etc/sysconfig/firewall
					LIBEXEC=/srv/firewall/libexec
					RULE=smtp

```

编辑 ACL 规则

```

# vim /srv/firewall/libexec/smtp.py

#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#
#  example.py
#  
#  Copyright 2013 neo <netkiller@msn.com>
#  
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 2 of the License, or
#  (at your option) any later version.
#  
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#  
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, write to the Free Software
#  Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston,
#  MA 02110-1301, USA.
#  
#  

from firewall import * 

######################################## 
# Web Application
######################################## 

smtp = Firewall()
smtp.flush()
smtp.policy(smtp.INPUT,smtp.ACCEPT)
smtp.policy(smtp.OUTPUT,smtp.ACCEPT)
smtp.policy(smtp.FORWARD,smtp.ACCEPT)
smtp.policy(smtp.POSTROUTING,smtp.ACCEPT)
smtp.input().state(('RELATED','ESTABLISHED')).accept()
smtp.input().protocol('icmp').accept()
smtp.input().interface('-i','lo').accept()
smtp.input().protocol('tcp').state('NEW').dport('22').accept()
smtp.input().protocol('tcp').state('NEW').dport(('25','110')).accept()
#smtp.input().protocol('tcp').dport(('3306','5432')).reject()
smtp.input().reject('--reject-with icmp-host-prohibited')
smtp.forward().reject('--reject-with icmp-host-prohibited')

for ip in range(53,58):
	smtp.postrouting().outbound('enp2s0').protocol('tcp').state('NEW').statistic('5').snat('--to-source 173.24.223.'+str(ip))
for ip in range(130,191):
	smtp.postrouting().outbound('enp2s0').protocol('tcp').state('NEW').statistic('5').snat('--to-source 107.17.40.'+str(ip))
for ip in range(2,63):
	smtp.postrouting().outbound('enp2s0').protocol('tcp').state('NEW').statistic('5').snat('--to-source 107.18.142.'+str(ip))
for ip in range(130,191):
	smtp.postrouting().outbound('enp2s0').protocol('tcp').state('NEW').statistic('5').snat('--to-source 146.71.38.'+str(ip))
for ip in range(194,255):
	smtp.postrouting().outbound('enp2s0').protocol('tcp').state('NEW').statistic('5').snat('--to-source 104.20.164.'+str(ip))

def start():
	smtp.start()
def stop():
	smtp.stop()
def restart():
	smtp.stop()
	smtp.start()
def show():
	smtp.show()
def status():
	smtp.status()
def main():
	show()
	return( 0 )

if __name__ == '__main__':
	main()

```

启动 firewall

```
					systemctl enable firewall
					systemctl start firewall

```

CentOS 6.x 之前的版本请使用 /etc/init.d/firewall 脚本

## 8. Example

### 8.1. 站内电邮发送

背景，网站通常需要一个电子邮件服务器，用于认证邮件真实性，给用户发送通知，订阅邮件等等。

这个邮件系统只需要外发邮件，并不需要接收邮件，配置如下。

```

[root@netkiller postfix]# postconf -n
alias_database = hash:/etc/aliases
alias_maps = hash:/etc/aliases
command_directory = /usr/sbin
config_directory = /etc/postfix
daemon_directory = /usr/libexec/postfix
data_directory = /var/lib/postfix
debug_peer_level = 2
debugger_command = PATH=/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin ddd $daemon_directory/$process_name $process_id & sleep 5
home_mailbox = Maildir/
html_directory = no
inet_interfaces = all
inet_protocols = ipv4
mail_owner = postfix
mailq_path = /usr/bin/mailq.postfix
manpage_directory = /usr/share/man
milter_default_action = accept
milter_protocol = 2
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mydomain = netkiller.cn
myhostname = mail.netkiller.cn
mynetworks = 203.88.18.17, 202.130.11.34, 147.89.27.78, 219.90.13.18
myorigin = $mydomain
newaliases_path = /usr/bin/newaliases.postfix
non_smtpd_milters = $smtpd_milters
queue_directory = /var/spool/postfix
readme_directory = /usr/share/doc/postfix-2.10.1/README_FILES
sample_directory = /usr/share/doc/postfix-2.10.1/samples
sendmail_path = /usr/sbin/sendmail.postfix
setgid_group = postdrop
smtpd_milters = inet:127.0.0.1:8891
unknown_local_recipient_reject_code = 550

```

### 8.2. EDM 服务器

EDM 服务器建议配置

```
				postconf -e "default_destination_concurrency_limit=5"
				postconf -e "queue_run_delay = 12h"
				postconf -e "maximal_queue_lifetime = 1d"

```

首先投递目的主机不能并发太多，发送失败的邮件一天只需要重发一次就可以，隔天是吧队列直接抛弃无需保留。

### 8.3. SMTP 邮件发送服务器

```

neo@netkiller ~ % postconf -n
alias_database = hash:/etc/aliases
alias_maps = hash:/etc/aliases
append_dot_mydomain = no
biff = no
compatibility_level = 2
inet_interfaces = all
inet_protocols = all
mailbox_size_limit = 0
message_size_limit = 1024000000
mydestination = $myhostname, netkiller.cn, netkiller.netkiller.com, localhost.netkiller.com, localhost
myhostname = netkiller.netkiller.com
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128
myorigin = /etc/mailname
readme_directory = no
recipient_delimiter = +
relayhost =
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination
smtpd_tls_cert_file = /etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file = /etc/ssl/private/ssl-cert-snakeoil.key
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtpd_use_tls = yes			

```

## 9. FAQ

### 9.1. SMTP ERROR: RCPT TO command failed: 501 5.1.3 Bad recipient address syntax

客户端反馈

```

SMTP ERROR: RCPT TO command failed: 501 5.1.3 Bad recipient address syntax
2015-09-23 08:06:12	SMTP Error: The following recipients failed: root@example.com: Bad recipient address syntax
<strong>SMTP Error: The following recipients failed: root@example.com: Bad recipient address syntax			

```

/var/log/maillog

```

Sep 23 16:12:00 smtp1 postfix/smtpd[982]: NOQUEUE: reject: RCPT from unknown[202.130.101.34]: 554 5.7.1 <netkiller@msn.com>: Relay access denied; from=<root@mail.example.com> to=<netkiller@msn.com> proto=ESMTP helo=<localhost.localdomain>

```

问题原因是 mynetworks 配置项没有放行客户端

```
				[root@netkiller.github.io ~]# postconf | grep permit_mynetworks
				smtpd_recipient_restrictions = permit_mynetworks, reject_unauth_destination

```

设置 mynetworks 配置项，允许 IP 使用 SMTP 发送邮件

```
				[root@netkiller.github.io ~]# postconf -n | grep mynetworks
				mynetworks = 202.130.101.34

```

### 9.2. connect to gmail-smtp-in.l.google.com[2607:f8b0:400e:c00::1a]:25: Network is unreachable

问题分析，上面 2607:f8b0:400e:c00::1a 是 IPv6 地址，在 google 默认是 ipv6，但大陆机房几乎不支持 ipv6.

```
				Aug 26 03:19:52 localhost postfix/smtp[6468]: connect to gmail-smtp-in.l.google.com[2607:f8b0:400e:c00::1a]:25: Network is unreachable
				Aug 26 03:19:53 localhost postfix/smtpd[6151]: connect from unknown[175.43.242.13]

```

解决方法禁用 ipv6

```
				postconf -e "inet_protocols = ipv4"
				systemctl reload postfix

```

### 9.3. opendkim[5762]: 3012A802C1DD: [49.213.11.18] [49.213.11.18] not internal

发送电子邮件并进行 DKIM 签名的前提是你邮件客户端的 IP 地址在 TrustedHosts 列表中

```
				Aug 26 03:52:36 localhost opendkim[5762]: 3012A802C1DD: [49.213.11.18] [49.213.11.18] not internal
				Aug 26 03:52:36 localhost opendkim[5762]: 3012A802C1DD: not authenticated
				Aug 26 03:52:36 localhost opendkim[5762]: 3012A802C1DD: no signature data

```

解决方法

添加 not internal IP 地址到 /etc/opendkim/TrustedHosts 文件中，然后 reload opendkim 进程。

### 9.4. opendkim[12578]: 4CC5C802C382: no signature data

```

Aug 26 02:46:52 localhost postfix/smtpd[5441]: connect from unknown[202.130.101.34]
Aug 26 02:46:53 localhost postfix/smtpd[5441]: 4CC5C802C382: client=unknown[202.130.101.34]
Aug 26 02:46:53 localhost postfix/cleanup[5445]: 4CC5C802C382: message-id=<860176544.0.1472194012792@Server>
Aug 26 02:46:53 localhost opendkim[12578]: 4CC5C802C382: [202.130.101.34] [202.130.101.34] not internal
Aug 26 02:46:53 localhost opendkim[12578]: 4CC5C802C382: not authenticated
Aug 26 02:46:53 localhost opendkim[12578]: 4CC5C802C382: no signature data
Aug 26 02:46:53 localhost postfix/qmgr[4605]: 4CC5C802C382: from=<neo@netkiller.cn>, size=530, nrcpt=1 (queue active)
Aug 26 02:46:53 localhost postfix/smtpd[5441]: disconnect from unknown[202.130.101.34]
Aug 26 02:46:54 localhost postfix/smtp[5446]: connect to gmail-smtp-in.l.google.com[2607:f8b0:400e:c00::1b]:25: Network is unreachable
Aug 26 02:46:54 localhost postfix/smtp[5446]: 4CC5C802C382: to=<netkiller@msn.com>, relay=gmail-smtp-in.l.google.com[74.125.25.27]:25, delay=1.3, delays=0.57/0.01/0.41/0.27, dsn=2.0.0, status=sent (250 2.0.0 OK 1472194014 m185si19680934pfc.265 - gsmtp)
Aug 26 02:46:54 localhost postfix/qmgr[4605]: 4CC5C802C382: removed

```

解决方案

```
				[root@localhost ~]# egrep -v "^#|^$" /etc/opendkim.conf
				PidFile /var/run/opendkim/opendkim.pid
				Mode sv
				Syslog yes
				SyslogSuccess yes
				LogWhy yes
				UserID opendkim:opendkim
				Socket inet:8891@localhost
				Umask 002
				SendReports yes
				SoftwareHeader yes
				Canonicalization relaxed/relaxed
				Selector default
				MinimumKeyBits 1024
				KeyFile /etc/opendkim/keys/default.private
				KeyTable /etc/opendkim/KeyTable
				SigningTable refile:/etc/opendkim/SigningTable
				InternalHosts refile:/etc/opendkim/TrustedHosts
				OversignHeaders From

```

注意下面几项配置

```
				Mode sv (这里默认是 v 便是校验邮件但不签名，s 表示签名邮件)
				KeyFile /etc/opendkim/keys/default.private
				KeyTable /etc/opendkim/KeyTable
				SigningTable refile:/etc/opendkim/SigningTable
				InternalHosts refile:/etc/opendkim/TrustedHosts

```

### 9.5. /etc/opendkim/keys/default.private: open(): No such file or directory

如果无法启动请查看启动日志

```
				# grep opendkim /var/log/messages
				Aug 25 01:24:57 localhost yum[10052]: Installed: libopendkim-2.10.3-7.el7.x86_64
				Aug 25 01:25:00 localhost yum[10052]: Installed: opendkim-2.10.3-7.el7.x86_64
				Aug 25 01:55:08 localhost opendkim: /etc/opendkim/keys/default.private: open(): No such file or directory
				Aug 25 01:55:08 localhost opendkim: opendkim: /etc/opendkim.conf: /etc/opendkim/keys/default.private: open(): No such file or directory
				Aug 25 01:55:08 localhost systemd: opendkim.service: control process
				exited, code=exited status=78
				Aug 25 01:55:08 localhost systemd: Unit opendkim.service entered failed state.
				Aug 25 01:55:08 localhost systemd: opendkim.service failed.
				Aug 25 01:56:10 localhost opendkim: /etc/opendkim/keys/default.private: open(): No such file or directory
				Aug 25 01:56:10 localhost opendkim: opendkim: /etc/opendkim.conf: /etc/opendkim/keys/default.private: open(): No such file or directory
				Aug 25 01:56:10 localhost systemd: opendkim.service: control process exited, code=exited status=78
				Aug
				25 01:56:10 localhost systemd: Unit opendkim.service entered failed state.
				Aug 25 01:56:10 localhost systemd: opendkim.service failed.

```

修改配置文件，指向你的密钥文件

```
				KeyFile /etc/opendkim/keys/default.private

```

### 9.6. fatal: parameter inet_interfaces: no local interface found for ::1

```

# Enable IPv4, and IPv6 if supported
inet_protocols = all
# 改为
inet_protocols = ipv4

```

### 9.7. NOQUEUE: reject: MAIL from unknown[192.168.3.31]: 552 5.3.4 Message size exceeds fixed limit;

```

NOQUEUE: reject: MAIL from unknown[192.168.3.31]: 552 5.3.4 Message size exceeds fixed limit;			

```

查看 message_size_limit 配置，默认是 10MB

```

neo@netkiller ~ % postconf -d | grep message_size_limit
message_size_limit = 10240000		

```

```

neo@netkiller ~ % sudo postconf -e 'message_size_limit = 1024000000'
neo@netkiller ~ % sudo systemctl reload postfix

```

### 9.8. 452 4.3.1 Insufficient system storage

message_size_limit 设置不合理

```

neo@netkiller ~ % sudo postconf -e 'message_size_limit = 10240000000'	

```

### 9.9. 454 Relay access denied

```

Jul 10 08:22:43 netkiller postfix/smtpd[2820]: NOQUEUE: reject: RCPT from unknown[192.168.3.31]: 454 4.7.1 <netkiller@kindle.cn>: Relay access denied; from=<neo@netkiller.cn> to=<netkiller@kindle.cn> proto=ESMTP helo=<1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa>			

```

```

neo@netkiller ~ % sudo postconf -e 'smtpd_recipient_restrictions=permit_mynetworks' 

```

配置 permit_mynetworks 后，需要将网卡的 IP 地址配置到 mynetworks，这里是 192.168.3.0/24

```

mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 192.168.3.0/24

```

例 62.1. SMTP 服务器配置实例

配置例子

```

neo@netkiller ~ % postconf -n
alias_database = hash:/etc/aliases
alias_maps = hash:/etc/aliases
append_dot_mydomain = no
biff = no
compatibility_level = 2
inet_interfaces = all
inet_protocols = all
mailbox_size_limit = 0
message_size_limit = 10240000000
mydestination = $myhostname, netkiller.cn, mail.netkiller.cn, localhost
myhostname = mail.netkiller.cn
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 192.168.3.0/24
myorigin = /etc/mailname
readme_directory = no
recipient_delimiter = +
relayhost =
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
smtpd_recipient_restrictions = permit_mynetworks
smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination permit_inet_interfaces
smtpd_tls_cert_file = /etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file = /etc/ssl/private/ssl-cert-snakeoil.key
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtpd_use_tls = yes			

```

## 第 63 章 邮件原文

## 1. Subject Unicode

=?encode?B?Subject?=

B = BASE64

例 63.1. Subject Unicode

=?UTF-8?B?U3ViamVjdAo?=

## 2. TO/CC/BCC

```

To: Neo Chen <neo.chen@example.com>
Cc: =?UTF-8?B?U3ViamVjdAo?= <sky.lv@example.com>
Bcc: xinying.wen@example.com

```

## 3. 正文

```

# cat mail.sh
#!/bin/bash
subject=$(echo "测试邮件"|base64)
mail=`cat /tmp/mail.txt | base64`
/usr/sbin/sendmail -t <<EOF
From: system@example.com
To: chao.zhang@example.com
Cc: sky.lv@example.com
Bcc: xinying.wen@example.com
Subject: =?utf-8?B?$subject?=
Content-Language: zh-cn
Content-type:txt/plain;charset=UTF-8
Content-Transfer-Encoding: base64

$mail

EOF

```

## 4. POP Sniffer

```

#!/usr/bin/python3
# Author: neo chan
# Homepage: http://netkiller.8800.org

import socketserver,sys
import threading

class ThreadedTCPRequestHandler(socketserver.BaseRequestHandler):

	def setup(self):
		print(self.client_address[0], 'connected!')
		self.request.send(b'+OK Welcome to coremail Mail Pop3 Server \r\n')

	def handle(self):
    	# self.request is the TCP socket connected to the client
		while True:
			self.data = self.request.recv(1024).strip()
			if self.data == b'QUIT':
				return
			if self.data == b'AUTH':
				self.request.send(b'-ERR Not support ntlm auth method\r\n')
			print("%s wrote: " % self.client_address[0])
			print (self.data)
			# just send back the same data, but upper-cased
			# self.request.send(self.data.upper())
			self.request.send(b'+OK 0 message(s) [0 byte(s)]\r\n')

	def finish(self):
		print( self.client_address[0], 'disconnected!')
		self.request.send(b'Goodbye! \r\n')

class ThreadedTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
	pass

if __name__ == "__main__":
	HOST, PORT = "172.16.0.1", 110

	# Create the server, binding to localhost on port 110
	# server = socketserver.TCPServer((HOST, PORT), MyTCPHandler)
	# server.serve_forever()

	# Activate the server; this will keep running until you
	# interrupt the program with Ctrl-C
	try:
		server = ThreadedTCPServer((HOST, PORT), ThreadedTCPRequestHandler)
		# Start a thread with the server -- that thread will then start one
		# more thread for each request
		server_thread = threading.Thread(target=server.serve_forever)
		# Exit the server thread when the main thread terminates
		# server_thread.setDaemon(True)
		server_thread.start()
	except KeyboardInterrupt:
		sys.exit(0)

```

## 5. PHP mail()

```

# cat mail.php
<?php

$to = "neo.chen@example.com";
$subject = "My subject";
$txt = "Hello world!";
$headers = "From: webmaster@example.com" . "\r\n";
//. "CC: somebodyelse@example.com";

mail($to,$subject,$txt,$headers);
?>

```

## 第 64 章 反垃圾邮件相关

### *Anti-Spam*

[`www.openspf.org/`](http://www.openspf.org/)

## 1. Sender Policy Framework

### 1.1. 分析 SPF 记录

从主域开始查看 txt 记录

```
neo@netkiller:~$ nslookup -type=txt 163.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
163.com	text = "v=spf1 include:spf.163.com -all"

Authoritative answers can be found from:			

```

找到 spf.163.com 域名，再查看它的 txt 记录

```
neo@netkiller:~$ nslookup -type=txt spf.163.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
spf.163.com	text = "v=spf1 include:a.spf.163.com include:b.spf.163.com include:c.spf.163.com include:d.spf.163.com -all"

Authoritative answers can be found from:			

```

一次查看 a.spf.163.com ~ d.spf.163.com 几个域名

```
neo@netkiller:~$ nslookup -type=txt a.spf.163.com
Server:		8.8.8.8
Address:	8.8.8.8#53

Non-authoritative answer:
a.spf.163.com	text = "v=spf1 ip4:220.181.12.0/22 ip4:220.181.31.0/24 ip4:123.125.50.0/24 ip4:220.181.72.0/24 ip4:123.58.178.0/24 ip4:123.58.177.0/24 ip4:113.108.225.0/24 ip4:218.107.63.0/24 ip4:123.58.189.128/25 -all"

Authoritative answers can be found from:			

```

这样就可以获得 163.com 所有邮件服务器的 IP 地址

下面我们使用 dig 演示此过程

```

neo@netkiller:~$ dig -t txt google.com

; <<>> DiG 9.9.5-11ubuntu1.2-Ubuntu <<>> -t txt google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 55272
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.			IN	TXT

;; ANSWER SECTION:
google.com.		3599	IN	TXT	"v=spf1 include:_spf.google.com ~all"

;; Query time: 40 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Wed Feb 24 11:12:01 HKT 2016
;; MSG SIZE  rcvd: 87

neo@netkiller:~$ dig -t txt _spf.google.com

; <<>> DiG 9.9.5-11ubuntu1.2-Ubuntu <<>> -t txt _spf.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 24347
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;_spf.google.com.		IN	TXT

;; ANSWER SECTION:
_spf.google.com.	299	IN	TXT	"v=spf1 include:_netblocks.google.com include:_netblocks2.google.com include:_netblocks3.google.com ~all"

;; Query time: 45 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Wed Feb 24 11:12:07 HKT 2016
;; MSG SIZE  rcvd: 160

neo@netkiller:~$ dig -t txt _netblocks.google.com

; <<>> DiG 9.9.5-11ubuntu1.2-Ubuntu <<>> -t txt _netblocks.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59355
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;_netblocks.google.com.		IN	TXT

;; ANSWER SECTION:
_netblocks.google.com.	3599	IN	TXT	"v=spf1 ip4:64.18.0.0/20 ip4:64.233.160.0/19 ip4:66.102.0.0/20 ip4:66.249.80.0/20 ip4:72.14.192.0/18 ip4:74.125.0.0/16 ip4:108.177.8.0/21 ip4:173.194.0.0/16 ip4:207.126.144.0/20 ip4:209.85.128.0/17 ip4:216.58.192.0/19 ip4:216.239.32.0/19 ~all"

;; Query time: 42 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Wed Feb 24 11:12:13 HKT 2016
;; MSG SIZE  rcvd: 304

```

## 2. DKIM

## 3. 邮件被拒收处理方法

### 3.1. NetEase

网易客服:服务热线 020-83568090-1

全国 24 小时客服电话: 020-83568090 (163/126 免费邮箱、188 邮箱、免费相册、博客等)

### 3.2. Sohu

搜狐客服:

webmaster@vip.sohu.com

热线电话： 010-58511234

[`mail.sohu.com/info/policy/`](http://mail.sohu.com/info/policy/)

### 3.3. Tom

[`pr.tom.com/about/about_contact_1.htm`](http://pr.tom.com/about/about_contact_1.htm)

```
				1.发送频率,包括一次性发信数量,每次发信的频率间隔多长时间.
				2.系统发送日志,例如您们发信系统发送不成功的一些日志.
				3.对 tom 邮箱 telnet 一次,把测试结果返回.(telnet tommx.163.net 25)
				4.提供发信失败的具体时间和发件人和收件人地址.
				5.如有退信请提供完整的退信内容.
				6.请提供贵司的域名和发信 IP. 

```

test_tom163@163.com

### 3.4. QQ

客服电话：0755-83765566

 **申请“他域互通” http://openmail.qq.com/**  **### 3.5. 21CN

咨询热线: 020-38733114 （7*24 小时服务）,咨询邮箱:webmaster@21cn.com

垃圾邮件处理专题[`mail.21cn.com/help/spam.htm`](http://mail.21cn.com/help/spam.htm)

退信专题:[`mail.21cn.com/help/tuixin_index1.htm`](http://mail.21cn.com/help/tuixin_index1.htm)**  **## 第 65 章 Fax

## 1. HylaFAX

http://www.hylafax.org/

## 第 66 章 FAQ

## 1. 通过 SSH 与控制台不能登录

通过 SSH 与控制台不能登录，登录后立即退出。

我在做压力测试的时候将所有用户的 nofile 设置为 1050000 导致 SSH 与控制台均不能登录 Linux 系统。

```
# cat /etc/security/limits.conf |tail
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#@student        -       maxlogins       4

# End of file
* soft nofile 1050000
* hard nofile 1050000

```

后来发现/var/log/secure 日志，提示 Could not set limit for 'nofile': Operation not permitted

```
# tail -f /var/log/secure

Aug  6 04:07:56 r510 sshd[20858]: Accepted password for root from 192.168.80.129 port 51798 ssh2
Aug  6 04:07:56 r510 sshd[20858]: pam_limits(sshd:session): Could not set limit for 'nofile': Operation not permitted
Aug  6 04:07:56 r510 sshd[20858]: pam_unix(sshd:session): session opened for user root by (uid=0)
Aug  6 04:07:56 r510 sshd[20858]: error: PAM: pam_open_session(): Permission denied

```**