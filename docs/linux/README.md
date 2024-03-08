# Netkiller Linux 手札

## Netkiller Linux Cookbook

ISBN# 

### Mr. Neo Chan, 陈景峯(BG7NYT)

中国广东省深圳市望海路半岛城邦三期
518067
+86 13113668890

`<netkiller@msn.com>`

MMDVM Hotspot:   YSF80337 - CN China 1 - W24166/TG46001 BM_China_46001 - DMR Radio ID 4600441 

2017-02-13

电子书最近一次更新于 2020-04-10 05:55:07 .

版权 © 2006-2020 Netkiller(Neo Chan). All rights reserved.

**版权声明**

转载请与作者联系，转载时请务必标明文章原始出处和作者信息及本声明。

|  &#124; ![ &#124;](http://creativecommons.org/licenses/by/3.0/)  |  

&#124; [`www.netkiller.cn`](http://www.netkiller.cn) &#124;
&#124; [`netkiller.github.io`](http://netkiller.github.io/) &#124;
&#124; [`netkiller.sourceforge.net`](http://netkiller.sourceforge.net/) &#124;

 |  &#124; ![ &#124;](img/weixin.jpg)  |  

&#124; 微信订阅号 netkiller-ebook (微信扫描二维码） &#124;
&#124; QQ：13721218 请注明“读者” &#124;
&#124; QQ 群：128659835 请注明“读者” &#124;

 |

$Date$

内容摘要

本文档讲述 Linux 系统涵盖了系统管理与配置包括：

### 对初学 Linux 的爱好者忠告

玩 Linux 最忌 reboot（重新启动）这是 windows 玩家坏习惯

Linux 只要接上电源你就不要再想用 reboot,shutdown,halt,poweroff 命令,Linux 系统和应用软件一般备有 reload,reconfigure,restart/start/stop...不需要安装软件或配置服务器后使用 reboot 重新引导计算机

在 Linux 系统里 SIGHUP 信号被定义为刷新配置文件,有些程序没有提供 reload 参数,你可以给进程发送 HUP 信号,让它刷新配置文件,而不用 restart.通过 pkill,killall,kill 都可以发送 HUP 信号例如: pkill -HUP httpd

我的系列文档:

操作系统

| Netkiller Linux 手札 | Netkiller FreeBSD 手札 | Netkiller Shell 手札 |
| Netkiller Security 手札 | Netkiller Web 手札 | Netkiller Monitoring 手札 |
| Netkiller Storage 手札 | Netkiller Mail 手札 | Netkiller Virtualization 手札 |

以下文档停止更新合并到 《Netkiller Linux 手札》

| Netkiller Debian 手札 | Netkiller CentOS 手札 | Netkiller Multimedia 手札 |   |   |   |

* * *

# 致读者

Netkiller 系列电子书始于 2000 年，风风雨雨走过 20 年，将在 2020 年终结，之后不在更新。作出这种决定原因很多，例如现在的阅读习惯已经转向短视频，我个人的时间，身体健康情况等等......

感谢读者粉丝这 20 年的支持

虽然电子书不再更新，后面我还会活跃在知乎社区和微信公众号