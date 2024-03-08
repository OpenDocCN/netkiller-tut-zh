# 第 1 章 FreeBSD Install

## Welcome

| ![](img/freebsd-boot.png) |

回车即可

| ![](img/freebsd-welcome.png) |

选择 Install 按钮后回车

| ![](img/freebsd-keymap-selection.png) |

选择 No

| ![](img/freebsd-hostname.png) |

输入 Hostname

## partitioning

| ![](img/freebsd-distribution-select.png) |

回车即可

| ![](img/freebsd-partitioning.png) |

选择 Guided

| ![](img/freebsd-partition-entire-disk.png) |

选择 Entire Dist 便是使用整个磁盘，

| ![](img/freebsd-partitioning-editor.png) |

使用光标键移动按钮到 Auto 然后回车，表示自动分区

| ![](img/freebsd-partitioning-finish.png) |

选择 Finish 回车，完成分区设置

再选择 Commit 确认，最后就会格式化磁盘

| ![](img/freebsd-archive-extraction.png) |

开始安装

## password

| ![](img/freebsd-new-password.png) |

设置 root 密码

| ![](img/freebsd-retype-new-password.png) |

确认密码，再输入一次

## network

| ![](img/freebsd-network-configuration.png) |

网络接口选择，这是只有一个网卡，如果你有多个网卡，这里应该是一个选择列表

| ![](img/freebsd-network-configuration-ipv4.png) |

问你是否开启 IPv4 选 Yes

| ![](img/freebsd-network-configuration-dhcp.png) |

问题是否使用 DHCP 获取 IP 地址。这里选择 No

| ![](img/freebsd-network-configuration-ip.png) |

输入 IP 地址，子网掩码，网关

| ![](img/freebsd-network-configuration-dns.png) |

设置 DNS 服务器

## timezone

| ![](img/freebsd-local.png) |

问你是否使用 UTC 时间，选择 No

| ![](img/freebsd-timezone.png) |

选择时区，我一般使用 Hongkong 或者 Harbin

| ![](img/freebsd-timezone-asia-hongkong.png) |

回车

| ![](img/freebsd-timezone-confirmation.png) |

选择 Yes

## complete

额外的软件包安装

| ![](img/freebsd-system.png) |

sshd 当然需要，还有 ntpd, moused 也不错可以在控制台上使用鼠标，可以快速复制粘贴控制台上的内容。

| ![](img/freebsd-dumpdev.png) |

对于普通用户，这个基本用不到 No

| ![](img/freebsd-adduser.png) |

添加一个普通帐号，选择 Yes

| ![](img/freebsd-adduser-ok.png) |

根据提示输入即可

| ![](img/freebsd-final.png) |

到此 FreeBSD 安装完成，如果你想修改前面那些配置可以在这里修改。这个界面让很多菜鸟搞得蒙头转向，挑不出安全程序，一边又一遍的安装 FreeBSD. 这里你放心的 Exit 菜单上回车即可。

| ![](img/freebsd-manual-configuration.png) |

选择 Yes 回车

| ![](img/freebsd-complete.png) |

选择 Reboot 回车

## FreeBSD 初始化设置

刚刚添加了一个 neo 用户，但这个用户并不能 su - root， 默认配置下 FreeBSD 在安全性方面做得比 Linux 好很多，你需要做下面的操作将 neo 添加到 wheel 组才能使用 su 命令

```
# pw usermod neo -G wheel
# id neo
uid=1001(neo) gid=1001(neo) groups=1001(neo),0(wheel)

```

## sysinstall，bsdinstall 与 bsdconfig 工具

### 从 FreeBSD 10 开始 sysinstall 被 bsdinstall 与 bsdconfig 所替代

例如安装 FreeBSD src 源码包

```
sysinstall -> Configure -> Distributions -> src -> ALL -> Install from a FreeBSD CD/DVD

```