# 部分 III. Web Server

## 第 15 章 Lighttpd

## install lighttpd

```

# cd /usr/ports/www/
# pkg_add -r lighttpd

```

/etc/rc.conf

```

[root@freebsd ~]# echo lighttpd_enable=\"YES\" >> /etc/rc.conf

[root@freebsd ~]# cat /etc/rc.conf
lighttpd_enable="YES"

```

```
fastcgi.server             = ( ".php" =>
                               ( "localhost" =>
                                 (
                                   "socket" => "/tmp/php-fastcgi.socket",
                                   "bin-path" => "/usr/local/bin/php-cgi"
                                 )
                               )
                            )

```

```
server.document-root        = "/www/htdocs/"
server.errorlog             = "/www/logs/lighttpd.error.log"
accesslog.filename          = "/www/logs/access.log"

chown -R www:www /www/
# touch /www/logs/lighttpd.error.log
# chown www:www /www/logs/lighttpd.error.log

```

start

```

freebsd0# /usr/local/etc/rc.d/spawn-fcgi start
Starting spawn_fcgi.
spawn-fcgi: child spawned successfully: PID: 79056
freebsd0# /usr/local/etc/rc.d/spawn-fcgi stop
Stopping spawn_fcgi.
Waiting for PIDS: 79056.
freebsd0# /usr/local/etc/rc.d/spawn-fcgi start
Starting spawn_fcgi.
spawn-fcgi: child spawned successfully: PID: 79084
freebsd0# /usr/local/etc/rc.d/spawn-fcgi restart
Stopping spawn_fcgi.
Starting spawn_fcgi.
spawn-fcgi: child spawned successfully: PID: 79109

# /usr/local/etc/rc.d/lighttpd start
Starting lighttpd.

```

## install php5

```
# pkg_add -r php5
# pkg_add -r php5-extensions
# pkg_add -r php5-curl
# pkg_add -r php5-mcrypt
# pkg_add -r php5-mbstring

# pkg_add -r php5-mysql
# pkg_add -r php5-gd

# pkg_add -r php5-zlib
# pkg_add -r php5-zip

```

## xcache

```
# portsnap fetch update
# cd /usr/ports/www/xcache
# make install clean

```

Enable xache

```
# cp /usr/local/share/examples/xcache/xcache.ini /usr/local/etc/php/
# cd /usr/local/etc/php/
# vi xcache.ini

```

Type the following command to restart lighttpd

```
# /usr/local/etc/rc.d/lighttpd restart

```

## Zend Optimizer

```

# cd /usr/ports/devel/ZendOptimizer/
# make
# cd /usr/ports/devel/ZendOptimizer/work/ZendOptimizer-*
#./install-tty

```

不要选择 apache

php.ini

```
# vi /usr/local/etc/php.ini

[Zend]
zend_extension_manager.optimizer=/usr/local/Zend/lib/Optimizer-3.3.0
zend_extension_manager.optimizer_ts=/usr/local/Zend/lib/Optimizer_TS-3.3.0
zend_optimizer.version=3.3.0a
zend_extension=/usr/local/Zend/lib/ZendExtensionManager.so
zend_extension_ts=/usr/local/Zend/lib/ZendExtensionManager_TS.so

```

## 第 16 章 nginx

```
pkg_add -r nginx

location / {
        root   /usr/local/www/nginx;
        index  index.html index.htm;
}

location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /usr/local/www/nginx$fastcgi_script_name;
        include        fastcgi_params;
}

```

## port install

```
# cd /usr/ports/www/nginx

# make install

HTTP_MODULE
HTTP_REWRITE_MODULE
HTTP_STATUS_MODULE

```

### php

ports 安装 php-fpm 适合 php-5.2.10, 高于这个版本请跳过这节, 采用编译安装。

```
# cd /usr/ports/lang/php5
# make install

```

extensions

```
# cd /usr/ports/lang/php5-extensions/
# make install
# ln -s /usr/local/etc/php.ini-production /usr/local/etc/php.ini

```

php-fpm - FastCGI Process Manager

homepage: http://php-fpm.org/downloads/freebsd-port/

```

# tar xvzf php-5.2.10-fpm-0.5.13.tar.gz --directory=/usr/ports/lang
x php5-fpm/
x php5-fpm/files/
x php5-fpm/Makefile
x php5-fpm/distinfo
x php5-fpm/pkg-descr
x php5-fpm/pkg-plist
x php5-fpm/files/php-fpm.sh.in
x php5-fpm/files/patch-scripts::phpize.in
x php5-fpm/files/patch-TSRM_threads.m4
x php5-fpm/files/patch-Zend::zend.h
x php5-fpm/files/patch-Zend_zend_list.c
x php5-fpm/files/patch-Zend_zend_list.h
x php5-fpm/files/patch-ext_standard_array.c
x php5-fpm/files/patch-ext_standard_basic_functions.c
x php5-fpm/files/patch-ext_standard_dns.h
x php5-fpm/files/patch-ext_standard_image.c
x php5-fpm/files/patch-php.ini-dist
x php5-fpm/files/patch-php.ini-recommended
x php5-fpm/files/patch-main::php_config.h.in
x php5-fpm/files/patch-main_SAPI.c
x php5-fpm/files/patch-acinclude.m4
x php5-fpm/files/patch-configure.in

# cd /usr/ports/lang/php5-fpm/ && make install

```

#### php-fpm

```

                        Unix user of processes
                        <value name="user">www</value>

                        Unix group of processes
                        <value name="group">www</value>

```

### /etc/rc.conf

```
vim /etc/rc.conf
php_fpm_enable="YES"
nginx_enable="YES"

```

### /usr/local/etc/nginx/nginx.conf

```
ee /usr/local/etc/nginx/nginx.conf

        location / {
            root   /www;
            index  index.html index.htm index.php;
        }

        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  /www$fastcgi_script_name;
            include        fastcgi_params;
        }

```

### start

```
/usr/local/etc/rc.d/php-fpm start
/usr/local/etc/rc.d/nginx start

```

## 编译安装 php 与 php-fpm

### 提示

PHP 5.3.3 后续版本已经集成 php-fpm 不需要打补丁再安装.

### php-5.2.x

http://php-fpm.org/downloads/

```

[root@freebsd1:~] cd /usr/src/
[root@freebsd1:/usr/src]

wget http://php-fpm.org/downloads/php-5.2.14-fpm-0.5.14.diff.gz
wget http://www.php.net/get/php-5.2.14.tar.gz/from/cn.php.net/mirror

[root@freebsd1:/usr/src] tar zxf php-5.2.14.tar.gz

gzip -cd php-5.2.14-fpm-0.5.14.diff.gz | patch -d php-5.2.14 -p1

[root@freebsd1:/usr/src] cd php-5.2.14

./configure --prefix=/usr/local/php-5.2.14 \
--with-config-file-path=/usr/local/php-5.2.14/etc \
--enable-fastcgi --enable-fpm \
--with-curl \
--with-gd \
--with-jpeg-dir=/usr/lib64 \
--with-iconv \
--with-mcrypt \
--with-zlib \
--with-pear \
--with-xmlrpc \
--with-openssl \
--with-mysql \
--with-mysqli \
--with-pdo-mysql \
--enable-zip \
--enable-sockets \
--enable-soap \
--enable-mbstring \
--enable-magic-quotes \
--enable-inline-optimization \
--enable-xml \
--enable-ftp

make && make install

```

配置 php.ini 与 php-fpm.conf

```

cp php.ini-dist /usr/local/php-5.2.14/etc/php.ini
/usr/local/php-5.2.14/etc/php.ini

include_path = ".:/www/includes:/usr/local/php-5.2.14/lib/php"

vim /usr/local/php-5.2.14/etc/php-fpm.conf

    <value name="owner">www</value>
    <value name="group">www</value>

<value name="user">www</value>
<value name="group">www</value>

/usr/local/php-5.2.14/sbin/php-fpm start

```

### php-5.3.x

```
安装 zlib
===========================================================
./configure
make test
make install

安装 gd
===========================================================
cd /usr/ports/graphic/gd
make install

安装 libpng
===========================================================
cd /usr/ports/graphics/png
make install
===========================================================

安装 jpeg
===========================================================
cd /usr/ports/graphics/jpeg
make install
===========================================================

安装 freetype
===========================================================
cd /usr/ports/print/freetype
make install

```

```
./configure --prefix=/usr/local/php-5.3.5 \
--with-config-file-path=/usr/local/php-5.3.5/etc \
--with-config-file-scan-dir=/usr/local/php-5.3.5/etc/conf.d \
--enable-fpm \
--with-fpm-user=www \
--with-fpm-group=www \
--with-pear \
--with-curl \
--with-gd \
--with-jpeg-dir \
--with-png-dir \
--with-freetype-dir \
--with-iconv \
--with-mcrypt \
--with-mhash \
--with-zlib \
--with-xmlrpc \
--with-xsl \
--with-openssl \
--with-mysql \
--with-mysqli \
--with-pdo-mysql \

--disable-debug \
--enable-zip \
--enable-sockets \
--enable-soap \
--enable-mbstring \
--enable-magic-quotes \
--enable-inline-optimization \
--enable-memory-limit
--enable-xml \

--enable-ftp \
--enable-exif \
--enable-wddx \
--enable-bcmath \
--enable-calendar \
--enable-sqlite-utf8 \
--enable-shmop \
--enable-dba \
--enable-sysvsem \
--enable-sysvshm \
--enable-sysvmsg

make
make install

```

php.ini

```
include_path=.:/usr/local/php-5.3.5/lib/php

```

php-fpm.conf

```
cp /usr/local/php-5.3.5/etc/php-fpm.conf.default /usr/local/php-5.3.5/etc/php-fpm.conf
cp /usr/src/php-5.3.5/sapi/fpm/init.d.php-fpm /usr/local/etc/rc.d/php-fpm
chmod +x /usr/local/etc/rc.d/php-fpm

vim /usr/local/php-5.3.5/etc/php-fpm.conf
pid = run/php-fpm.pid

user = www
group = www

pm.start_servers = 20
pm.min_spare_servers = 5
pm.max_spare_servers = 35

```

## 第 17 章 Apache

## install

```

# cd /usr/ports/www/
# pkg_add -r apache

```

## 第 18 章 mysql

## install

## install

```
freebsd# pkg_add -r mysql51-server
freebsd# /usr/local/bin/mysql_install_db 
freebsd# chown -R mysql /var/db/mysql 

```

/etc/rc.conf

```
freebsd# vi /etc/rc.conf
mysql_enable="YES"

```

start

```
freebsd# /usr/local/etc/rc.d/mysql-server start
Starting mysql.

```

set password

```
freebsd# /usr/local/bin/mysqladmin -u root password 'chen'

```

```

freebsd# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.1.39 FreeBSD port: mysql-server-5.1.39

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

```

## 第 19 章 log

## cronolog

```
cd /usr/ports/sysutils/cronolog/
make install clean

```

### Apache

```
CustomLog "|/usr/local/sbin/cronolog /data/log/domain/%Y%m%d.log" combined
CustomLog "|/usr/local/sbin/cronolog /var/log/www/%Y%m/%d_access_log" combined

```

### Lighttpd

```
accesslog.filename = "| /usr/local/sbin/cronolog /var/log/lighttpd/%Y/%m/%d/access.log"

```

## logrotate

## 第 20 章 Cache

## Memcache

```
cd /usr/ports/databases/memcached; make install clean

```

```
vi /etc/rc.conf
# start memcached
memcached_enable="YES"

```

你也可不使用 vi，使用下面命令快速添加

```
echo "memcached_enable=\"YES\"" >> /etc/rc.conf

```

Memcache

```
/usr/local/etc/rc.d/memcached start

```

## PHP Memcache

```
cd /usr/ports/databases/pecl-memcache; make install clean

```

确认 Memcache 是否安装成功

```
cat /usr/local/etc/php/extensions.ini | grep memcache

php -m | grep memcache
memcache

```