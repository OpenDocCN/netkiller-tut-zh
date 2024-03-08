# 部分 V. D Programming Language

## 第 31 章 D Lang

[`dlang.org/`](http://dlang.org/)

## dmd install

Ubuntu

```
$ wget http://ftp.digitalmars.com/dmd_2.061-0_amd64.deb
$ sudo apt-get install libc6-dev

$ sudo dpkg -i dmd_2.061-0_amd64.deb

```

CentOS

```
wget http://ftp.digitalmars.com/dmd-2.062-0.fedora.x86_64.rpm
yum localinstall dmd-2.062-0.fedora.x86_64.rpm

```

## helloworld

```
$ cat hello.d
import std.stdio;
void main() {
  writeln("Hello, world!");
}

```

```
$ chmod u+x hello.d
$ ./hello.d
Hello, world!

```

```
$ dmd hello.d
$ ./hello
Hello, world!

```

```
$ scp hello root@172.16.0.3:/tmp

# cd /tmp/
# ./hello
Hello, world!

```

## dmd - Digital Mars D2.x Compiler

### -cov do code coverage analysis

$ dmd hello.d -cov

```
$ cat hello.d
#!/usr/bin/rdmd
import std.stdio;
void main() {
    writeln("Hello, world!");
}

$ dmd hello.d -cov

$ ./hello
Hello, world!

$ cat hello.lst
       |#!/usr/bin/rdmd
       |import std.stdio;
       |void main() {
      1|    writeln("Hello, world!");
       |}
hello.d is 100% covered

```

## Open Source Development for the D Programming Language

[`www.dsource.org/`](http://www.dsource.org/)

### DDBI - A database independent interface.

```
$ git clone http://github.com/aaronc/ddbi/

```

## 第 32 章 FAQ

## /lib64/libc.so.6: version `GLIBC_2.14' not found

```
# strings /lib64/libc.so.6 |grep GLIBC_
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_PRIVATE

```