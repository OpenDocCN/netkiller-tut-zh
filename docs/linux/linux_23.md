# 第 205 章 FAQ

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

```