# 部分 IV. C/C++

## 第 25 章 Build tool

## make - GNU make utility to maintain groups of programs

Makefile

```
$ sudo apt-get install make

```

使用 make 命令测试

### autoconf - Generate configuration scripts

autoconf

```
$ sudo apt-get install autoconf

```

automake

```
$ sudo apt-get install automake

```

example

过程 25.1. autoconf and automake step by step

1.  create directory

    ```
    % mkdir devel
    % cd devel
    % mkdir hello
    % cd hello

    ```

    create a file

    ```
    vim hello.c

    　　#include
    　　int main(int argc, char** argv)
    　　{
    　　printf(``Hello, GNU!\n'');
    　　return 0;
    　　}

    ```

2.  autoscan

    ```
    neo@debian:~/workspace/devel/hello$ autoscan
    neo@debian:~/workspace/devel/hello$ ls
    autoscan.log  configure.scan  hello.c

    ```

3.  configure.in

    ```
    cp configure.scan configure.in

    neo@debian:~/workspace/devel/hello$ aclocal
    neo@debian:~/workspace/devel/hello$ autoconf
    neo@debian:~/workspace/devel/hello$ ls
    autom4te.cache  autoscan.log  configure  configure.in  configure.scan  hello.c

    ```

4.  Makefile.am

    ```
    neo@debian:~/workspace/devel/hello$ vim Makefile.am
    neo@debian:~/workspace/devel/hello$ cat Makefile.am
    AUTOMAKE_OPTIONS= foreign
    bin_PROGRAMS= hello
    hello_SOURCES= hello.c
    neo@debian:~/workspace/devel/hello$

    ```

    ```
    $ automake --add-missing
    configure.in: no proper invocation of AM_INIT_AUTOMAKE was found.
    configure.in: You should verify that configure.in invokes AM_INIT_AUTOMAKE,
    configure.in: that aclocal.m4 is present in the top-level directory,
    configure.in: and that aclocal.m4 was recently regenerated (using aclocal).
    automake: no `Makefile.am' found for any configure output
    automake: Did you forget AC_CONFIG_FILES([Makefile]) in configure.in?

    ```

## CMake

[`www.cmake.org/`](http://www.cmake.org/)

### helloworld

安装 CMake

```
$ sudo yum install gcc gcc-c++
$ sudo yum install make

$ sudo yum install cmake28
$ sudo ln -s /usr/bin/cmake28 /usr/bin/cmake
$ cmake --version
cmake version 2.8.9

```

创建 CMakeLists.txt 文件

```
$ cat CMakeLists.txt
PROJECT(example)
ADD_EXECUTABLE(example main.c)

```

创建 main.c 文件

```

$ cat main.c
#include <stdio.h>
int main() {
   printf("helloworld!\n");
   return 0;
}

```

编译程序

```
$ cmake .
-- Configuring done
-- Generating done
-- Build files have been written to: /home/neo/example

$ make
Scanning dependencies of target example
[100%] Building C object CMakeFiles/example.dir/main.c.o
Linking C executable example
[100%] Built target example

$ ./example
helloworld!

```

### cmake_minimum_required

```
cmake_minimum_required(VERSION 2.8.7)

```

### SET

```
SET(CMAKE_INSTALL_PREFIX /usr/local)

```

改变 CMAKE_INSTALL_PREFIX 变量

```
cmake -DCMAKE_INSTALL_PREFIX=/usr ..

```

### ADD_SUBDIRECTORY

```
ADD_SUBDIRECTORY(src bin)

```

### INCLUDE_DIRECTORIES

```
INCLUDE_DIRECTORIES(/usr/include/xen)

```

相当于 gcc -I/usr/include/xen

### 编译文件

#### ADD_EXECUTABLE 编译可执行

```
SET(SRC_LIST main.cc
        src/file1.c
        src/file2.c
        )

ADD_EXECUTABLE(hello ${SRC_LIST})

```

#### ADD_LIBRARY 编译库文件

编译 *.a 文件

```
$ cat  CMakeLists.txt 
cmake_minimum_required(VERSION 2.8)
PROJECT(zeromq)
ADD_LIBRARY(zeromq zeromq.c)
INCLUDE_DIRECTORIES(/usr/include/mysql)
TARGET_LINK_LIBRARIES(zeromq zmq)

```

编译共享库 *.so 文件

```
$ cat  CMakeLists.txt 
cmake_minimum_required(VERSION 2.8)
PROJECT(zeromq)
ADD_LIBRARY(zeromq SHARED zeromq.c)
INCLUDE_DIRECTORIES(/usr/include/mysql)
TARGET_LINK_LIBRARIES(zeromq zmq)

```

### EXECUTABLE_OUTPUT_PATH / LIBRARY_OUTPUT_PATH

```
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

```

### TARGET_LINK_LIBRARIES

```
TARGET_LINK_LIBRARIES(hello log4cpp)
TARGET_LINK_LIBRARIES(hello zmq)

```

相当于 gcc -lzmq

### INSTALL

```
INSTALL(PROGRAMS hello DESTINATION bin)

INSTALL(FILES COPYRIGHT README DESTINATION share/doc/hello)

INSTALL(DIRECTORY doc/ DESTINATION share/doc/hello)

```

## scons - a software construction tool

[`www.scons.org/`](http://www.scons.org/)

创建一个 hello.c 测试文件

```

#include<stdio.h>

main()
{
    printf("Hello World!\n");
}

```

创建 SConstruct 文件（相当于 Makefile）

```
$ cat SConstruct
Program('hello.c')

```

开始编译

```
$ scons
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Building targets ...
gcc -o hello.o -c hello.c
gcc -o hello hello.o
scons: done building targets.

```

编译后产生的文件，尝试运行 hello 程序

```
$ ls
hello  hello.c  hello.o  SConstruct

$ ./hello
Hello World!

```

下面操作想当于 make clean

```
$ scons -c
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Cleaning targets ...
Removed hello.o
Removed hello
scons: done cleaning targets.

$ ls
hello.c  SConstruct

```

## Phing

http://www.phing.info/

```
$ pear channel-discover pear.phing.info
$ pear install phing/phing

```

## 第 26 章 C

## compiler

### gcc - The GNU C compiler

$ sudo apt-get install gcc

```
$ sudo apt-get install gcc

```

### clang - Low-Level Virtual Machine (LLVM), C language family frontend

```
$ apt-cache search clang
llvm-3.0 - Low-Level Virtual Machine (LLVM)
clang - Low-Level Virtual Machine (LLVM), C language family frontend
libclang-common-dev - clang library - Common development package
libclang-dev - clang library - Development package
libclang1 - clang library
libsclang1 - SuperCollider language interpreter library
llvm-2.8 - Low-Level Virtual Machine (LLVM)
llvm-2.9 - Low-Level Virtual Machine (LLVM)

```

```
$ apt-get install clang

```

例 26.1. clang helloworld

```

$ cat hello.c
#include <stdio.h>
int main(int argc, char **argv) { printf("hello world\n"); }

```

```
$ vim hello.c
$ clang hello.c -o hello
$ ./hello
hello world

```

## ldconfig

```
[root@localhost src]# ldconfig  -p | grep mysql
        libmysqlclient_r.so.15 (libc6,x86-64) => /usr/lib64/mysql/libmysqlclient_r.so.15
        libmysqlclient_r.so.15 (libc6) => /usr/lib/mysql/libmysqlclient_r.so.15
        libmysqlclient_r.so (libc6,x86-64) => /usr/lib64/mysql/libmysqlclient_r.so
        libmysqlclient_r.so (libc6) => /usr/lib/mysql/libmysqlclient_r.so
        libmysqlclient.so.15 (libc6,x86-64) => /usr/lib64/mysql/libmysqlclient.so.15
        libmysqlclient.so.15 (libc6) => /usr/lib/mysql/libmysqlclient.so.15
        libmysqlclient.so (libc6,x86-64) => /usr/lib64/mysql/libmysqlclient.so
        libmysqlclient.so (libc6) => /usr/lib/mysql/libmysqlclient.so

```

## C Library

### lib

#### syslog.h

```

# cat syslog.c
#include <stdio.h>
#include <unistd.h>
#include <syslog.h>

int main(void) {

 openlog("slog", LOG_PID|LOG_CONS, LOG_USER);
 syslog(LOG_INFO, "A different kind of Hello world ... ");
 closelog();

 return 0;
}

```

```
[root@dev1 test]# gcc syslog.c
[root@dev1 test]# ls
a.out  syslog.c
[root@dev1 test]# ./a.out

[root@dev1 test]# tail /var/log/messages
Jan 11 23:52:27 dev1 slog[5056]: A different kind of Hello world ...

```

#### stdio.h

##### fscanf/fprintf

```

#include<stdio.h>
int main()
{
	FILE *file;
	char name[20][20];

	int a[10]={0};

	int i,j;
	if((file=fopen("1.txt","rt"))==NULL)
	{
		printf("Cannot open file strike any key exit!");
		return 0;
	}
	i=0;
	while(fscanf(file,"%s %d\n",name[i],&a[i])!=EOF)
	{
		i++;
	}
	i=0;
	while(a[i]!=0)
	{
		printf("%s %d\n",name[i],a[i]);
		i++;
	}
	fclose(file);
}

```

```

#include <stdio.h>
#include <string.h>

typedef struct _Address
{
	char *name;
	int age;
}Address;

int main()
{
	FILE *file;
	int num;
	char str[256];
	int i;
	char *p;
	Address addr[10]={0};

	if((file=fopen("1.txt","rt"))==NULL)
	{
		printf("Cannot open file strike any key exit!");
		return 0;
	}
	i=0;

	while(fscanf(file,"%s %d\n",str,&num)!=EOF)
	{
		asprintf(&addr[i].name, "%s", str);
		addr[i].age = num;
		i++;
	}
	fclose(file);
	addr[i].name = NULL;

	i=0;
	while(1){
		if(addr[i].name == NULL) break;
		printf("%d: %s %d\n",i, addr[i].name,addr[i].age);
		i++;
	}
}

```

### libssh2

[`www.libssh2.org/`](http://www.libssh2.org/)

### libconfig – C/C++ Configuration File Library

http://www.hyperrealm.com/main.php?s=libconfig

libconfig 可用于处理 *.conf 配置文件

### libuv

提供 Socket,进程线程处理等等

http://nikhilm.github.io/uvbook/

### newt

https://fedorahosted.org/newt/

http://sourcecodebrowser.com/newt/0.52.10/windows_8c.html

http://gnewt.sourceforge.net/tutorial-4.html#ss4.1

### Spdylay - SPDY C Library

[`spdylay.sourceforge.net/`](http://spdylay.sourceforge.net/)

### libPhenom

http://facebook.github.io/libphenom/

libPhenom is an eventing framework for building high performance and high scalability systems in C

### curl

```

#include <stdio.h>
#include <curl/curl.h>

int main(void)
{
  CURL *curl;
  CURLcode res;

  curl = curl_easy_init();
  if(curl) {
    curl_easy_setopt(curl, CURLOPT_URL, "http://example.com");
    /* example.com is redirected, so we tell libcurl to follow redirection */
    curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);

    /* Perform the request, res will get the return code */
    res = curl_easy_perform(curl);
    /* Check for errors */
    if(res != CURLE_OK)
      fprintf(stderr, "curl_easy_perform() failed: %s\n",
              curl_easy_strerror(res));

    /* always cleanup */
    curl_easy_cleanup(curl);
  }
  return 0;
}

```

get content

```

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <curl/curl.h>

struct string {
  char *ptr;
  size_t len;
};

void init_string(struct string *s) {
  s->len = 0;
  s->ptr = malloc(s->len+1);
  if (s->ptr == NULL) {
    fprintf(stderr, "malloc() failed\n");
    exit(EXIT_FAILURE);
  }
  s->ptr[0] = '\0';
}

size_t writefunc(void *ptr, size_t size, size_t nmemb, struct string *s)
{
  size_t new_len = s->len + size*nmemb;
  s->ptr = realloc(s->ptr, new_len+1);
  if (s->ptr == NULL) {
    fprintf(stderr, "realloc() failed\n");
    exit(EXIT_FAILURE);
  }
  memcpy(s->ptr+s->len, ptr, size*nmemb);
  s->ptr[new_len] = '\0';
  s->len = new_len;

  return size*nmemb;
}

int main(void)
{
  CURL *curl;
  CURLcode res;

  curl = curl_easy_init();
  if(curl) {
    struct string s;
    init_string(&s);

    curl_easy_setopt(curl, CURLOPT_URL, "curl.haxx.se");
    curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, writefunc);
    curl_easy_setopt(curl, CURLOPT_WRITEDATA, &s);
    res = curl_easy_perform(curl);

    printf("%s\n", s.ptr);
    free(s.ptr);

    /* always cleanup */
    curl_easy_cleanup(curl);
  }
  return 0;
}

```

#### url encode / decode

encode

```
char *subject= curl_easy_escape(curl, "测试主题", 0);		

```

decode

```
char *subject= curl_easy_unescape(curl, "测试主题", 0);		

```

### libxml

#### example

创建 xml

```

#include <stdio.h>
#include <libxml/parser.h>
#include <libxml/tree.h>

int main(int argc, char **argv)
{
	xmlDocPtr doc = NULL;
	xmlNodePtr root_node = NULL, node = NULL, node1 = NULL;

	doc = xmlNewDoc(BAD_CAST "1.0"); // create a new xml document.
	root_node = xmlNewNode(NULL, BAD_CAST "root"); // create a root node.
	xmlDocSetRootElement(doc, root_node);

	xmlNewChild(root_node, NULL, BAD_CAST "node1", BAD_CAST "content of node1");
	//xmlNewChild(root_node, NULL, BAD_CAST "node2", NULL);

	node = xmlNewChild(root_node, NULL, BAD_CAST "node3", BAD_CAST "node3 has attributes");
	xmlNewProp(node, BAD_CAST "attribute", BAD_CAST "yes");

	node = xmlNewNode(NULL, BAD_CAST "node4");
	node1 = xmlNewText(BAD_CAST
	           "other way to create content (which is also a node)");
	xmlAddChild(node, node1);
	xmlAddChild(root_node, node);

	xmlSaveFormatFileEnc(argc > 1 ? argv[1] : "-", doc, "UTF-8", 1);

	xmlFreeDoc(doc);

	xmlCleanupParser();

	xmlMemoryDump();
	return(0);
}

```

#### Creating string with libxml2

```

	xmlChar *s;
	int size;
	xmlDocDumpMemory(doc, &s, &size);
	xmlFree(s);

	printf("xml: %s", (char *)s);

```

## 第 27 章 C++

## g++ - The GNU C++ compiler

$ sudo apt-get install g++

```
$ sudo apt-get install g++

```

## C++ library

### Boost C++ Libraries

www.boost.org

### google-perftools

#### Fast, multi-threaded malloc() and nifty performance analysis tools

http://code.google.com/p/google-perftools/

### TreeFrog Framework

#### High-speed C++ MVC Framework for Web Application

## 第 28 章 Objective-C

```
$ sudo apt-get install gobjc gobjc++

$ sudo apt-get install gnustep-make

```

例 28.1. Objective-C hello world

```

$ cat hello.m
#import <stdio.h>

int main( int argc, const char *argv[] ) {
    printf( "hello world\n" );
    return 0;
}

```

```
$ gcc hello.m

$ ./a.out
hello world

```

## 第 29 章 调试工具

## ftop - Tool to show progress of open files and file systems

ftop 是一个显示进程打开文件的工具

```
Tue Jan 20 16:25:50 2015                                                                                 ftop 1.0
Processes:  47 total, 0 unreadable                                                Press h for help, o for options
Open Files: 57 regular, 0 dir, 149 chr, 0 blk, 12 pipe, 59 sock, 15 misc

_  PID    #FD  USER      COMMAND                                                                                  
-- 413    10   root      /sbin/udevd -d
|  +- 3    -rw  --	935/935      /dev/.udev/queue.bin
-- 982    7    root      auditd
|  +- 5    --W  --     3.1M/3.1M     /var/log/audit/audit.log
-- 1002   5    root      /sbin/rsyslogd -i /var/run/syslogd.pid -c 5
|  +- out  --W  --     8059/8059     /var/log/messages
|  +- err  --W  --    16443/16443    /var/log/cron
|  +- 3    -r-  --        0/0        /proc/kmsg
|  +- 4    --W  --    14976/14976    /var/log/secure
-- 1106   17   www       nginx: worker process                   
|  +- err  --W  --   538920/538920   /var/log/nginx/error.log (fd 11 for PID 1106)

```

## strace - trace system calls and signals

```
-tt  在每行输出的前面,显示毫秒级别的时间
-T   显示每次系统调用所花费的时间
-v   对于某些相关调用,把完整的环境变量,文件 stat 结构等打出来.
-f   跟踪目标进程,以及目标进程创建的所有子进程.
-e  控制要跟踪的事件和跟踪行为,比如指定要跟踪的系统调用名称.
     trace=file     跟踪和文件访问相关的调用(参数中有文件名)
     trace=process  和进程管理相关的调用,比如 fork/exec/exit_group
      trace=network  和网络通信相关的调用,比如 socket/sendto/connect
      trace=signal    信号发送和处理相关,比如 kill/sigaction
      trace=desc  和文件描述符相关,比如 write/read/select/epoll 等    
      trace=ipc 进程见通信相关,比如 shmget 等

-o  把 strace 的输出单独写到指定的文件
-s  当系统调用的某个参数是字符串时,最多输出指定长度的内容，默认是 32 个字节.
-p  指定要跟踪的进程 pid,要同时跟踪多个 pid,重复多次-p 选项即可.		

```

```
strace -tt -T -v -f -e trace=file -o ~/strace.log.2 -s 1024 -p  25849		

```

strace -v ps -e 2

strace -v ls

```
neo@netkiller:~/workspace/Document$ strace -c ls
Docbook  makedoc  Tex
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
  -nan    0.000000           0        11           read
  -nan    0.000000           0         1           write
  -nan    0.000000           0        38        13 open
  -nan    0.000000           0        27           close
  -nan    0.000000           0        25           fstat
  -nan    0.000000           0        39           mmap
  -nan    0.000000           0        16           mprotect
  -nan    0.000000           0         4           munmap
  -nan    0.000000           0         3           brk
  -nan    0.000000           0         2           rt_sigaction
  -nan    0.000000           0         1           rt_sigprocmask
  -nan    0.000000           0         2           ioctl
  -nan    0.000000           0         9         9 access
  -nan    0.000000           0         1           execve
  -nan    0.000000           0         1           fcntl
  -nan    0.000000           0         2           getdents
  -nan    0.000000           0         1           getrlimit
  -nan    0.000000           0         1           statfs
  -nan    0.000000           0         1           arch_prctl
  -nan    0.000000           0         3         1 futex
  -nan    0.000000           0         1           set_tid_address
  -nan    0.000000           0         1           set_robust_list
------ ----------- ----------- --------- --------- ----------------
100.00    0.000000                   190        23 total

```

```
neo@netkiller:~/workspace/Document$ strace -f -e open ls >/dev/null
open("/etc/ld.so.cache", O_RDONLY)      = 3
open("/lib/librt.so.1", O_RDONLY)       = 3
open("/lib/libselinux.so.1", O_RDONLY)  = 3
open("/lib/libacl.so.1", O_RDONLY)      = 3
open("/lib/libc.so.6", O_RDONLY)        = 3
open("/lib/libpthread.so.0", O_RDONLY)  = 3
open("/lib/libdl.so.2", O_RDONLY)       = 3
open("/lib/libattr.so.1", O_RDONLY)     = 3
open("/proc/filesystems", O_RDONLY)     = 3
open("/usr/lib/locale/locale-archive", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/share/locale/locale.alias", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_IDENTIFICATION", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_IDENTIFICATION", O_RDONLY) = 3
open("/usr/lib/gconv/gconv-modules.cache", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_MEASUREMENT", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_MEASUREMENT", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_TELEPHONE", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_TELEPHONE", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_ADDRESS", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_ADDRESS", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_NAME", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_NAME", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_PAPER", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_PAPER", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_MESSAGES", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_MESSAGES", O_RDONLY) = 3
open("/usr/lib/locale/en_US.utf8/LC_MESSAGES/SYS_LC_MESSAGES", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_MONETARY", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_MONETARY", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_COLLATE", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_COLLATE", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_TIME", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_TIME", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_NUMERIC", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_NUMERIC", O_RDONLY) = 3
open("/usr/lib/locale/en_US.UTF-8/LC_CTYPE", O_RDONLY) = -1 ENOENT (No such file or directory)
open("/usr/lib/locale/en_US.utf8/LC_CTYPE", O_RDONLY) = 3
open(".", O_RDONLY|O_NONBLOCK|O_DIRECTORY|O_CLOEXEC) = 3

```

### -o file -- send trace output to FILE instead of stderr

```
$ strace -o strace.log php --version
$ grep php.ini strace.log

```

## ltrace - A library call tracer

ltrace ls

```

neo@netkiller:~/workspace/Document$ ltrace ls
__libc_start_main(0x407bb0, 1, 0x7fff827aea38, 0x413730, 0x413720 <unfinished ...>
strrchr("ls", '/')                                                                                            = NULL
setlocale(6, "")                                                                                              = "en_US.UTF-8"
bindtextdomain("coreutils", "/usr/share/locale")                                                              = "/usr/share/locale"
textdomain("coreutils")                                                                                       = "coreutils"
__cxa_atexit(0x40abb0, 0, 0, 0x736c6974756572, 1)                                                             = 0
isatty(1)                                                                                                     = 1
getenv("QUOTING_STYLE")                                                                                       = NULL
getenv("LS_BLOCK_SIZE")                                                                                       = NULL
getenv("BLOCK_SIZE")                                                                                          = NULL
getenv("BLOCKSIZE")                                                                                           = NULL
getenv("POSIXLY_CORRECT")                                                                                     = NULL
getenv("BLOCK_SIZE")                                                                                          = NULL
getenv("COLUMNS")                                                                                             = NULL
ioctl(1, 21523, 0x7fff827ae910)                                                                               = 0
getenv("TABSIZE")                                                                                             = NULL
getopt_long(1, 0x7fff827aea38, "abcdfghiklmnopqrstuvw:xABCDFGHI:"..., 0x00416a60, -1)                         = -1
__errno_location()                                                                                            = 0x7f89323f16a8
malloc(40)                                                                                                    = 0x02543870
memcpy(0x02543870, "", 40)                                                                                    = 0x02543870
__errno_location()                                                                                            = 0x7f89323f16a8
malloc(40)                                                                                                    = 0x025438a0
memcpy(0x025438a0, "", 40)                                                                                    = 0x025438a0
malloc(18400)                                                                                                 = 0x025438d0
malloc(32)                                                                                                    = 0x025434c0
strlen(".")                                                                                                   = 1
malloc(2)                                                                                                     = 0x025480c0
memcpy(0x025480c0, ".", 2)                                                                                    = 0x025480c0
__errno_location()                                                                                            = 0x7f89323f16a8
opendir(".")                                                                                                  = 0x025480e0
readdir(0x025480e0)                                                                                           = 0x02548108
readdir(0x025480e0)                                                                                           = 0x02548120
readdir(0x025480e0)                                                                                           = 0x02548138
readdir(0x025480e0)                                                                                           = 0x02548150
strlen("Tex")                                                                                                 = 3
malloc(4)                                                                                                     = 0x02550110
memcpy(0x02550110, "Tex", 4)                                                                                  = 0x02550110
readdir(0x025480e0)                                                                                           = 0x02548168
readdir(0x025480e0)                                                                                           = 0x02548188
strlen("makedoc")                                                                                             = 7
malloc(8)                                                                                                     = 0x02550130
memcpy(0x02550130, "makedoc", 8)                                                                              = 0x02550130
readdir(0x025480e0)                                                                                           = 0x025481a8
readdir(0x025480e0)                                                                                           = 0x025481c8
strlen("Docbook")                                                                                             = 7
malloc(8)                                                                                                     = 0x02550150
memcpy(0x02550150, "Docbook", 8)                                                                              = 0x02550150
readdir(0x025480e0)                                                                                           = NULL
closedir(0x025480e0)                                                                                          = 0
free(NULL)                                                                                                    = <void>
malloc(72)                                                                                                    = 0x025480e0
_setjmp(0x61c040, 0x25480e0, 0x2543af8, 3, 1)                                                                 = 0
__errno_location()                                                                                            = 0x7f89323f16a8
strcoll("makedoc", "Docbook")                                                                                 = 9
__errno_location()                                                                                            = 0x7f89323f16a8
strcoll("Tex", "Docbook")                                                                                     = 16
__errno_location()                                                                                            = 0x7f89323f16a8
strcoll("Tex", "makedoc")                                                                                     = 7
memcpy(0x025480f0, "\3208T\002", 8)                                                                           = 0x025480f0
realloc(NULL, 144)                                                                                            = 0x02548130
malloc(168)                                                                                                   = 0x025481d0
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550150, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550150, 0x7fff827ac0e0, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550130, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550130, 0x7fff827ac0e0, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550110, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac0e0, 8192, 0x2550110, 0x7fff827ac0e0, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550150, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550150, 0x7fff827ac110, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550150, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550150, 0x7fff827ac050, 0)                                    = 6
fwrite_unlocked("Docbook", 1, 7, 0x7f8931bab780)                                                              = 7
__overflow(0x7f8931bab780, 32, 0, 8, 0xffffffff)                                                              = 32
__overflow(0x7f8931bab780, 32, 1, 8, 0xffffffff)                                                              = 32
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550130, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550130, 0x7fff827ac110, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550130, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550130, 0x7fff827ac050, 0)                                    = 6
fwrite_unlocked("makedoc", 1, 7, 0x7f8931bab780)                                                              = 7
__overflow(0x7f8931bab780, 32, 1, 8, 7)                                                                       = 32
__overflow(0x7f8931bab780, 32, 2, 8, 7)                                                                       = 32
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550110, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac110, 8192, 0x2550110, 0x7fff827ac110, 0)                                    = 6
__errno_location()                                                                                            = 0x7f89323f16a8
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550110, -1, 0)                                                = 6
__ctype_get_mb_cur_max(0x7fff827ac050, 8192, 0x2550110, 0x7fff827ac050, 0)                                    = 6
fwrite_unlocked("Tex", 1, 3, 0x7f8931bab780)                                                                  = 3
__overflow(0x7f8931bab780, 10, 0, 120, 3Docbook  makedoc  Tex
)                                                                     = 10
free(0x025480c0)                                                                                              = <void>
free(NULL)                                                                                                    = <void>
free(0x025434c0)                                                                                              = <void>
exit(0 <unfinished ...>
__fpending(0x7f8931bab780, 0, 0x7f8931bac330, 0x7f8931bac330, 0x25434b0)                                      = 0
fclose(0x7f8931bab780)                                                                                        = 0
__fpending(0x7f8931bab860, 0, 0x7f8931bacdf0, 0, 0x7f89323f17a0)                                              = 0
fclose(0x7f8931bab860)                                                                                        = 0
+++ exited (status 0) +++

```

## ldd - print shared library dependencies

```
$ ldd /bin/ls
        linux-gate.so.1 =>  (0xffffe000)
        librt.so.1 => /lib/tls/i686/cmov/librt.so.1 (0xb7f13000)
        libacl.so.1 => /lib/libacl.so.1 (0xb7f0d000)
        libselinux.so.1 => /lib/libselinux.so.1 (0xb7ef9000)
        libc.so.6 => /lib/tls/i686/cmov/libc.so.6 (0xb7dc4000)
        libpthread.so.0 => /lib/tls/i686/cmov/libpthread.so.0 (0xb7db1000)
        /lib/ld-linux.so.2 (0xb7f22000)
        libattr.so.1 => /lib/libattr.so.1 (0xb7dad000)
        libdl.so.2 => /lib/tls/i686/cmov/libdl.so.2 (0xb7da9000)
        libsepol.so.1 => /lib/libsepol.so.1 (0xb7d6c000)
$

```

举例

```
# ./boinc
./boinc: error while loading shared libraries: libssl.so.1.0.0: cannot open shared object file: No such file or directory

# ldd ./boinc | grep libssl
./boinc: /lib64/libcurl.so.4: no version information available (required by ./boinc)
	libssl.so.1.0.0 => not found
	libssl3.so => /lib64/libssl3.so (0x00007f1f46998000)
	libssl.so.10 => /lib64/libssl.so.10 (0x00007f1f44ba1000)

```

## Valgrind

http://valgrind.org/

```
valgrind --tool=memcheck --leak-check=full ./test

```

## nm - list symbols from object files

```
$ nm libzeromq.so 
                 U asprintf@@GLIBC_2.2.5
00000000002020d0 B __bss_start
00000000002020d0 b completed.6992
0000000000000f25 T concat
                 w __cxa_finalize@@GLIBC_2.2.5
0000000000000e40 t deregister_tm_clones
0000000000000eb0 t __do_global_dtors_aux
0000000000201de8 t __do_global_dtors_aux_fini_array_entry
00000000002020c8 d __dso_handle
0000000000201df8 d _DYNAMIC
00000000002020d0 D _edata
00000000002020d8 B _end
0000000000001710 T _fini
0000000000000ef0 t frame_dummy
0000000000201de0 t __frame_dummy_init_array_entry
0000000000001ab8 r __FRAME_END__
                 U free@@GLIBC_2.2.5
0000000000202000 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
0000000000000ca8 T _init
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
0000000000201df0 d __JCR_END__
0000000000201df0 d __JCR_LIST__
                 w _Jv_RegisterClasses
                 U malloc@@GLIBC_2.2.5
                 U memcpy@@GLIBC_2.14
                 U memset@@GLIBC_2.2.5
0000000000000e70 t register_tm_clones
                 U __stack_chk_fail@@GLIBC_2.4
                 U strlen@@GLIBC_2.2.5
                 U strncpy@@GLIBC_2.2.5
00000000002020d0 d __TMC_END__
0000000000001399 T zmq_client
000000000000151f T zmq_client_deinit
0000000000001346 T zmq_client_init
                 U zmq_close
                 U zmq_connect
                 U zmq_ctx_destroy
                 U zmq_ctx_new
                 U zmq_msg_close
                 U zmq_msg_data
                 U zmq_msg_init
                 U zmq_msg_init_size
                 U zmq_msg_size
000000000000157d T zmq_publish
0000000000001703 T zmq_publish_deinit
000000000000152a T zmq_publish_init
0000000000000fae T zmq_read
0000000000001157 T zmq_read_deinit
0000000000000f5b T zmq_read_init
                 U zmq_recvmsg
                 U zmq_sendmsg
                 U zmq_socket
00000000000011b5 T zmq_write
000000000000133b T zmq_write_deinit
0000000000001162 T zmq_write_init

```

## objdump - display information from object files.

## readelf - Displays information about ELF files.

## 第 30 章 GNU Development Tools

## strip - Discard symbols from object files.

给 C 程序减肥

```
# cp nginx nginx.old
# strip nginx
# ll
total 4984
-rwxr-xr-x  1 root root  545080 Oct 18 10:48 nginx
-rwxr-xr-x. 1 root root 4554524 May  7 17:18 nginx.old

```