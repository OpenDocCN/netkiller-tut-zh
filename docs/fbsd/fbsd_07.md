# 第 21 章 MySQL

## Installation

FreeBSD 10.0 以上的版本使用 pkg 安装

```
pkg search mysql
pkg install mysql56-server		

```

FreeBSD 9.2 或更早的版本

```

pkg_add -r mysql56-server
pkg_add -r mysql56-client

/usr/local/etc/rc.d/mysql-server onestart
/usr/local/bin/mysqladmin -u root password 'newpassword'

cat >> /etc/rc.conf <<EOF
mysql_enable="YES"
EOF

```

To install using the ports system:

```
cd /usr/ports/databases/mysql56-server/
make install clean

cd /usr/ports/databases/mysql56-client/
make install clean

```