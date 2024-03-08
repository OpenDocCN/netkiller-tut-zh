# 部分 XIV. 软件版本控制

## 第 180 章 Git - Fast Version Control System

### *distributed revision control system*

homepage: http://git.or.cz/index.html

过程 180.1. Git

1.  install

    ```
    sudo apt-get install git-core

    ```

2.  config

    ```

    $ git-config --global user.name neo
    $ git-config --global user.email openunix@163.com

    ```

3.  Initializ

    ```
    $ mkdir repository
    $ cd repository/

    /repository$ git-init-db
    Initialized empty Git repository in .git/

    ```

    to check .gitconfig file

    ```
    $ cat ~/.gitconfig
    [user]
            name = chen
            email = openunix@163.com

    ```

## 1. Repositories 仓库管理

### 1.1. initial setup

```
Tell git who you are:

$ git config user.name "FirstName LastName"
$ git config user.email "user@example.com"

If you have many git repositories under your current user, you can set this for all of them

$ git config --global user.name "FirstName LastName"
$ git config --global user.email "user@example.com"

If you want pretty colors, you can setup the following for branch, status, and diff commands:

$ git config --global color.branch "auto"
$ git config --global color.status "auto"
$ git config --global color.diff "auto"

Or, to turn all color options on (with git 1.5.5+), use:

$ git config --global color.ui "auto"
To enable aut-detection for number of threads to use (good for multi-CPU or multi-core computers) for packing repositories, use:

$ git config --global pack.threads "0"
To disable the rename detection limit (which is set "pretty low" according to Linus, "just to not cause problems for people who have less memory in their machines than kernel developers tend to have"), use:

$ git config --global   diff.renamelimit "0"

```

### 1.2. checkout

将 nqp-cc/src/QASTCompilerMAST.nqp 文件 重置到 211ab0b19f25b8c81685a97540f4b1491eb17504 版本

```
git checkout 211ab0b19f25b8c81685a97540f4b1491eb17504 -- nqp-cc/src/QASTCompilerMAST.nqp

```

### 1.3. Creating and Commiting

```
$ cd (project-directory)
$ git init
$ (add some files)
$ git add .
$ git commit -m 'Initial commit'

```

### 1.4. Manager remote

remote add

```

git remote add origin git@localhost:example.git

```

remote show

```

git remote show
origin

```

remote rm

```

git remote rm origin

```

添加多个远程仓库

```
git remote add origin git@localhost:example.git
git remote add another https://gitcafe.com/netkiller/netkiller.gitcafe.com.git
git push origin master
git push another master

```

### 1.5. Status

```

$ git clone git://10.10.0.5/example.git
Cloning into example...
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 1), reused 0 (delta 0)
Receiving objects: 100% (5/5), done.
Resolving deltas: 100% (1/1), done.

neo@neo-OptiPlex-380:~/tmp$ cd example/

neo@neo-OptiPlex-380:~/tmp/example$ git status
# On branch master
nothing to commit (working directory clean)

neo@neo-OptiPlex-380:~/tmp/example$ ls
test1  test2  test3  test4

neo@neo-OptiPlex-380:~/tmp/example$ echo hello > test1

neo@neo-OptiPlex-380:~/tmp/example$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   test1
#
no changes added to commit (use "git add" and/or "git commit -a")

```

### 1.6. Diff

```

neo@neo-OptiPlex-380:~/tmp/example$ git diff
diff --git a/test1 b/test1
index e69de29..ce01362 100644
--- a/test1
+++ b/test1
@@ -0,0 +1 @@
+hello

```

比较 nqp-cc/src/QASTCompilerMAST.nqp 文件 当前版本与 211ab0b19f25b8c81685a97540f4b1491eb17504 版本的区别

```
git diff 211ab0b19f25b8c81685a97540f4b1491eb17504 -- nqp-cc/src/QASTCompilerMAST.nqp

```

#### 1.6.1. --name-only 仅显示文件名

```
git diff --name-only

```

### 1.7. Cloning

```

$ git clone git://github.com/git/hello-world.git
$ cd hello-world
$ (edit files)
$ git add (files)
$ git commit -m 'Explain what I changed'

```

### 1.8. Push

```

$ git clone git://10.10.0.5/example.git
$ cd example
$ (edit files)
$ git add (files)
$ git commit -m 'Explain what I changed'

$ git push origin master
Counting objects: 5, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 278 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git://10.10.0.5/example.git
   27f8417..b088cc3  master -> master

```

### 1.9. Pull

```
$ git pull
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From git://10.10.0.5/example
   27f8417..b088cc3  master     -> origin/master
Updating 27f8417..b088cc3
Fast-forward
 test1 |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

### 1.10. fetch

```
$ git fetch
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 2 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (2/2), done.
From git://10.10.0.5/example
   b088cc3..7e8c17d  master     -> origin/master

```

### 1.11. Creating a Patch

```

$ git clone git://github.com/git/hello-world.git
$ cd hello-world
$ (edit files)
$ git add (files)
$ git commit -m 'Explain what I changed'
$ git format-patch origin/master

```

### 1.12. reset

重置到上一个版本

```
git log
git reset --hard HEAD^
git log
git push -f

```

## 2. Manipulating branches

git-branch - List, create, or delete branches

### 2.1. list branches

```
$ git-branch
* master

```

### 2.2. create branches

```
$ git-branch mybranch
$ git-branch
* master
  mybranch

```

### 2.3. delete branches

```
$ git-branch -d mybranch
Deleted branch mybranch.

$ git-branch
* master

```

### 2.4. switch branch

```
$ git-branch
* master
  mybranch

$ git-checkout mybranch
Switched to branch "mybranch"

$ git-branch
  master
* mybranch

```

### 2.5. git-show-branch - Show branches and their commits

```
$ git-show-branch
! [master] add a new file
 * [mybranch] add a new file
--
+* [master] add a new file

```

## 3. Sharing Repositories with others

### 3.1. Setting up a git server

First we need to setup a user with a home folder. We will store all the repositories in this users home folder.

```

sudo adduser git

```

Rather than giving out the password to the git user account use ssh keys to login so that you can have multiple developers connect securely and easily.

Next we will make a repository. For this example we will work with a repository called example. Login as the user git and add the repository.

login to remote server

```

ssh git@REMOTE_SERVER

```

once logged in

```

sudo mkdir example.git
cd example.git
sudo git --bare init
Initialized empty Git repository in /home/git/example.git/

```

That’s all there is to creating a repository. Notice we named our folder with a .git extension.

Also notice the ‘bare’ option. By default the git repository assumes that you’ll be using it as your working directory, so git stores the actual bare repository files in a .git directory alongside all the project files. Since we are setting up a remote server we don’t need copies of the files on the filesystem. Instead, all we need are the deltas and binary objects of the repository. By setting ‘bare’ we tell git not to store the current files of the repository only the diffs. This is optional as you may have need to be able to browse the files on your remote server.

Finally all you need to do is add your files to the remote repository. We will assume you don’t have any files yet.

```

mkdir example
cd example
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin git@REMOTE_SERVER:example.git
git push origin master

```

replace REMOTE_SERVER with your server name or IP

### 3.2. 修改 origin

```

git remote rename origin old-origin

```

### 3.3. 删除 origin

```

git remote remove origin		

```

## 4. Submodule 子模块

### 4.1. 添加模块

```

neo@MacBook-Pro ~ % cd workspace/Linux

neo@MacBook-Pro ~/workspace/Linux % git submodule add https://github.com/netkiller/common.git common 
Cloning into '/Users/neo/workspace/Linux/common'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 185 (delta 2), reused 6 (delta 1), pack-reused 176
Receiving objects: 100% (185/185), 56.49 KiB | 163.00 KiB/s, done.
Resolving deltas: 100% (105/105), done.		

```

模块信息存储在 .gitmodules 文件中

```

neo@MacBook-Pro ~/workspace/Linux % cat .gitmodules  
[submodule "common"]
	path = common
	url = https://github.com/netkiller/common.git		

```

同时也添加到 .git/config 文件中

```

neo@MacBook-Pro ~/workspace/Linux % cat .git/config | tail -n 3
[submodule "common"]
	url = https://github.com/netkiller/common.git
	active = true		

```

### 4.2. checkout 子模块

clone 项目，然后进入目录

```

neo@MacBook-Pro /tmp/test % git clone https://github.com/netkiller/Linux.git
neo@MacBook-Pro /tmp/test % cd Linux

```

初始化子模块

```

neo@MacBook-Pro /tmp/test/Linux % git submodule init
Submodule 'common' (https://github.com/netkiller/common.git) registered for path 'common'

```

更新模块

```

neo@MacBook-Pro /tmp/test/Linux % git submodule update
Cloning into '/private/tmp/test/Linux/common'...
Submodule path 'common': checked out 'cdf61a1de34590bcc80b895fdf0e90b62cfd729f'			

```

### 4.3. 删除子模块

```

git rm --cached <module>

```

## 5. Git Large File Storage

https://git-lfs.github.com/

Git Large File Storage | Git Large File Storage (LFS) replaces large files such as audio samples, videos, datasets, and graphics with text pointers inside Git, while storing the file contents on a remote server like GitHub.com or GitHub Enterprise.

```

/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew install git-lfs	

```

### 5.1. 安装 LFS 支持

```

git lfs install
git lfs track "*.psd"
git add .gitattributes

git add file.psd
git commit -m "Add design file"
git push origin master	

```

### 5.2. LFS lock

文件锁的用途是用户可以对一个文件进行加锁，阻止其他用户同一时间对该文件进行修改操作。因为在 GIT 仓库中同时编辑一个文件，会发生冲突，然而解决二进制大文件的冲突，合并操作极其困难。

```

neo@MacBook-Pro ~/workspace/java-project % git lfs lock test.psd  
Locked test.psd

neo@MacBook-Pro ~/workspace/java-project % git lfs locks
test.psd	bg7nyt	ID:55777		

```

如果 Push 被锁的文件，提示 Remote "origin" does not support the LFS locking API

```

neo@MacBook-Pro /tmp/java % git commit -a -m 'aaa'
[master b832eb3] aaa
 1 file changed, 2 insertions(+), 2 deletions(-)
neo@MacBook-Pro /tmp/java % git push
Remote "origin" does not support the LFS locking API. Consider disabling it with:
  $ git config 'lfs.https://github.com/bg7nyt/java.git/info/lfs.locksverify' false
Post https://github.com/bg7nyt/java.git/info/lfs/locks/verify: EOF
error: failed to push some refs to 'https://github.com/bg7nyt/java.git'		

```

解锁后 Push 成功

```

neo@MacBook-Pro ~/workspace/java-project % git lfs unlock test.psd    
Unlocked test.psd

neo@MacBook-Pro /tmp/java % git push
Git LFS: (1 of 1 files) 9 B / 9 B                                                                                                                                                                        
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 352 bytes | 352.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/bg7nyt/java.git
   b29f474..b832eb3  master -> master		

```

## 6. command

### 6.1. hash-object

使用 git 命令计算文件的 sha-1 值

```

neo@MacBook-Pro ~ % echo 'test content' | git hash-object --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4		

```

### 6.2. git-add - Add file contents to the index

```

$ echo 'hello world!!!'> newfile
$ git-add newfile

```

### 6.3. git-status - Show the working tree status

```

$ git-status newfile
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#       new file: newfile
#		

```

### 6.4. git-commit - Record changes to the repository

```
$ git-commit -m 'add a new file' newfile
Created initial commit f6fda79: add a new file
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 newfile

```

### 6.5. git-show - Show various types of objects

```

$ git-show
commit f6fda79f2f550ea3b2c1b483371ed5d12499ac35
Author: chen <openunix@163.com>
Date:   Sat Nov 1 08:50:45 2008 -0400

    add a new file

diff --git a/newfile b/newfile
new file mode 100644
index 0000000..b659464
--- /dev/null
+++ b/newfile
@@ -0,0 +1 @@
+hello world!!!

```

### 6.6. git-checkout - Checkout and switch to a branch

#### 6.6.1. checkout master

```
$ git-checkout master
Switched to branch "master"

```

#### 6.6.2. checkout branch

```
$ git-branch
* master
  mybranch

$ git-checkout mybranch
Switched to branch "mybranch"

$ git-branch
  master
* mybranch

```

### 6.7. git config

```
$ git config --file config http.receivepack true

```

### 6.8. git log

```

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ai) %C(bold blue)<%an>%Creset' --abbrev-commit

```

## 7. git-daemon 服务器

### 7.1. git-daemon - A really simple server for git repositories

在/home/gitroot/ 上运行 git 守护进程

```
$ cd /home/gitroot
$ mkdir test.git
$ cd test.git
$ git --bare init --shared
Initialized empty shared Git repository in /home/gitroot/test.git/

```

```

git daemon --verbose --export-all --base-path=/home/gitroot --enable=receive-pack --reuseaddr

```

允许 push,否则该仓库只能 clone/pull

```
sudo git daemon --verbose --export-all --base-path=/home/gitroot --enable=upload-pack --enable=upload-archive --enable=receive-pack

```

或者增加配置项

```
$ git config daemon.receivepack true
$ git config --file config receive.denyCurrentBranch ignore

```

### 7.2. git-daemon-sysvinit

for a read-only repo:

```

$ sudo apt-get install git-daemon-sysvinit

$ dpkg -l git-daemon-sysvinit
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                                   Version                  Architecture             Description
+++-======================================-========================-========================-==================================================================================
ii  git-daemon-sysvinit                    1:1.7.10.4-1ubuntu1      all                      fast, scalable, distributed revision control system (git-daemon service)

$ dpkg -L git-daemon-sysvinit
/.
/usr
/usr/share
/usr/share/git-core
/usr/share/git-core/sysvinit
/usr/share/git-core/sysvinit/sentinel
/usr/share/doc
/usr/share/doc/git-daemon-sysvinit
/usr/share/doc/git-daemon-sysvinit/copyright
/usr/share/doc/git-daemon-sysvinit/README.Debian
/etc
/etc/default
/etc/default/git-daemon
/etc/init.d
/etc/init.d/git-daemon
/usr/share/doc/git-daemon-sysvinit/changelog.Debian.gz

```

配置 /etc/default/git-daemon 文件

### 7.3. inet.conf / xinetd 方式启动

过程 180.2. git-daemon

1.  /etc/shells

    /etc/shells 最后一行添加 '/usr/bin/git-shell'

    ```
    $ grep git /etc/shells
    /usr/bin/git-shell

    ```

2.  add new user 'git' and 'gitroot' for git

    you need to assign shell with /usr/bin/git-shell

    ```
    $ sudo adduser git --shell /usr/bin/git-shell
    $ sudo adduser gitroot --ingroup git --shell /bin/bash

    ```

    /etc/passwd

    ```
    $ grep git /etc/passwd
    git:x:1001:1002:,,,:/home/git:/usr/bin/git-shell
    gitroot:x:1002:1002:,,,:/home/gitroot:/bin/bash

    ```

3.  /etc/services

    ```
    $ grep 9418 /etc/services
    git             9418/tcp                        # Git Version Control System

    ```

4.  /etc/inet.conf

    ```
    $ grep git /etc/inet.conf
    git     stream  tcp     nowait  nobody \
      /usr/bin/git-daemon git-daemon --inetd --syslog --export-all /home/gitroot

    ```

    reload inetd

    ```
    $ sudo pkill -HUP inetd

    ```

5.  xinetd

    目前的 Linux 逐渐使用 xinetd.d 替代 inet.conf，如 Redhat 系列已经不再使用 inet.conf, Ubuntu 系列发行版已经不预装 inet 与 xinetd

    ```
    $ apt-cache search xinetd
    globus-gfork-progs - Globus Toolkit - GFork Programs
    rlinetd - gruesomely over-featured inetd replacement
    update-inetd - inetd configuration file updater
    xinetd - replacement for inetd with many enhancements

    $ sudo apt-get install xinetd

    ```

    /etc/xinetd.d/

    ```
    $ cat /etc/xinetd.d/git
    # default: off
    # description: The git server offers access to git repositories
    service git
    {
            disable 		= no
            type            = UNLISTED
            port            = 9418
            socket_type     = stream
            protocol 		= tcp
            wait            = no
            user            = gitroot
            server          = /usr/bin/git
            server_args     = daemon --inetd --export-all --enable=receive-pack --reuseaddr --base-path=/home/gitroot
            log_on_failure  += USERID
    }

    ```

    reload xinitd

    ```
    $ sudo /etc/init.d/xinetd reload
     * Reloading internet superserver configuration xinetd                                       [ OK ]

    ```

### 7.4. git-daemon-run

```
$ sudo apt-get install git-daemon-run

```

安装后会创建下面两个用户

```
$ cat /etc/passwd | grep git
gitlog:x:117:65534::/nonexistent:/bin/false
gitdaemon:x:118:65534::/nonexistent:/bin/false

```

git-daemon-run 包携带的文件

```
$ dpkg -L git-daemon-run
/.
/etc
/etc/sv
/etc/sv/git-daemon
/etc/sv/git-daemon/run
/etc/sv/git-daemon/log
/etc/sv/git-daemon/log/run
/usr
/usr/share
/usr/share/doc
/usr/share/doc/git-daemon-run
/usr/share/doc/git-daemon-run/changelog.gz
/usr/share/doc/git-daemon-run/changelog.Debian.gz
/usr/share/doc/git-daemon-run/README.Debian
/usr/share/doc/git-daemon-run/copyright

```

同时创建下面配置文件

```
$ find /etc/sv/git-daemon/
/etc/sv/git-daemon/
/etc/sv/git-daemon/run
/etc/sv/git-daemon/supervise
/etc/sv/git-daemon/log
/etc/sv/git-daemon/log/run
/etc/sv/git-daemon/log/supervise

```

编辑/etc/sv/git-daemon/run 配置

```

$ sudo vim /etc/sv/git-daemon/run

#!/bin/sh
exec 2>&1
echo 'git-daemon starting.'
exec chpst -ugitdaemon \
  "$(git --exec-path)"/git-daemon --verbose --reuseaddr \
    --base-path=/var/cache /var/cache/git

```

```
git-daemon --verbose --reuseaddr \
    --base-path=/var/cache /var/cache/git

改为

git-daemon --verbose --reuseaddr \
	--enable=receive-pack --export-all --base-path=/opt/git

```

### 提示

* 我加上了一个--export-all 使用该选项后，在 git 仓库中就不必创建 git-daemon-export-ok 文件。

其他选项--enable=upload-pack --enable=upload-archive --enable=receive-pack

/etc/services 文件中加入

```
# Local services
git             9418/tcp                        # Git Version Control System

```

确认已经加入

```
$ grep 9418 /etc/services

```

启动 git-daemon

```
$ sudo sv stop git-daemon

or

$ sudo runsv git-daemon
runsv git-daemon: fatal: unable to change to directory: file does not exist

```

扫描 git 端口，确认 git-daemon 已经启动

```
$ nmap localhost

Starting Nmap 5.00 ( http://nmap.org ) at 2012-01-31 10:45 CST
Warning: Hostname localhost resolves to 2 IPs. Using 127.0.0.1.
Interesting ports on localhost (127.0.0.1):
Not shown: 989 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1723/tcp open  pptp
3128/tcp open  squid-http
3306/tcp open  mysql
9418/tcp open  git

```

### 7.5. Testing

```

$ sudo mkdir -p /opt/git/example.git
$ cd /opt/git/example.git
$ git init
$ sudo vim example.git/.git/config
[receive]
denyCurrentBranch = ignore

$ sudo chown gitdaemon -R /opt/git/*
$ touch git-daemon-export-ok

```

.git/config 文件应该是下面这样

```
$ cat example.git/.git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true

[receive]
denyCurrentBranch = ignore

```

git-clone git://localhost/example.git

```

neo@deployment:/tmp$ git clone git://localhost/example.git example.git
Cloning into example.git...
warning: You appear to have cloned an empty repository.
neo@deployment:/tmp$ cd example.git/
neo@deployment:/tmp/example.git$ echo helloworld > hello.txt
neo@deployment:/tmp/example.git$ git add hello.txt
neo@deployment:/tmp/example.git$ git commit -m 'Initial commit'
[master (root-commit) 65a0f83] Initial commit
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 hello.txt

```

我们添加了一些文件 push 到服务器

```
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 214 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git://localhost/example.git
 * [new branch]      master -> master

```

然后再 git clone，可以看到文件数目

```

$ git-clone git://localhost/example.git
Cloning into example...
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

```

## 8. git config

### 8.1. core.sshCommand

git 默认使用 id_rsa，指定私钥方法是：

```

git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"
git pull
git push

```

```

GIT_SSH_COMMAND="ssh -i ~/.ssh/id_rsa_example -F /dev/null" git clone git@github.com:netkiller/netkiller.github.io.git		

```

### 8.2. fatal: The remote end hung up unexpectedly

```
error: RPC failed; result=22, HTTP code = 413 | 18.24 MiB/s
fatal: The remote end hung up unexpectedly

```

```
git config http.postBuffer 524288000

```

### 8.3. 忽略 SSL 检查

使用自颁发 ssl 证书时需要开启，否则无法 clone 和 push

```

export GIT_SSL_NO_VERIFY=true			

```

```

git config http.sslVerify "false"  			

```

## 9. git-svn - Bidirectional operation between a single Subversion branch and git

```

sudo apt-get install git-svn

```

clone

```
git-svn clone -s svn://netkiller.8800.org/neo
cd neo
git gc

git commit -a
git-svn dcommit

```

从 svn 仓库更新

```
git-svn rebase

```

git-svn init svn://netkiller.8800.org/neo/public_html

## 10. .gitignore

```
find ./ -type d -empty | grep -v \.git | xargs -i touch {}/.gitignore

```

## 11. .gitattributes

### 11.1. SVN Keywords

```

Example:

$ echo '*.txt ident' >> .gitattributes
$ echo '$Id$' > test.txt
$ git commit -a -m "test"

$ rm test.txt
$ git checkout -- test.txt
$ cat test.txt			

```

## 12. gitolite - SSH-based gatekeeper for git repositories

```
$ apt-cache search gitolite
gitolite - SSH-based gatekeeper for git repositories

```

```
sudo apt-get install gitolite

```

No adminkey given - not setting up gitolite.

### 12.1. gitolite-admin

```
git@192.168.2.1:gitolite-admin.git

```

#### 12.1.1. gitolite.conf

gitolite-admin/conf/gitolite.conf

##### 12.1.1.1. staff

```
@admin 		= neo
@developer 	= bottle nada dick blank phabricator
@designer 	= blank
@deployer 	= phoenix
@tester 	= jimmy

```

##### 12.1.1.2. repo

```
repo gitolite-admin
    RW+     = @admin
    R       = @deployer

repo mydomain.com/www.mydomain.com
    RW+     = @admin
    RW		= @developer @designer
    R		= @deployer

repo mydomain.com/images.mydomain.com
    RW+     = @admin
    RW		= @developer @designer
    R		= @deployer

repo mydomain.com/passport.mydomain.com
    RW+     = @admin
    RW		= @developer
    R		= @deployer @designer

repo    example.com/www.example.com
  RW+       = @all  

repo    @all
  RW        = @developer @designer
  R         = @agentbot @deployment @test	

```

## 13. Web Tools

### 13.1. viewgit

http://viewgit.sourceforge.net/

## 14. FAQ

### 14.1. 导出最后一次修改过的文件

有时我们希望把刚刚修改的文件复制出来，同时维持原有的目录结构，这样可能交给运维直接覆盖服务器上的代码。我们可以使用下面的命令完成这样的操作，而不用一个一个文件的复制。

```
git archive -o update.zip HEAD $(git diff --name-only HEAD^)

```

### 14.2. 导出指定版本区间修改过的文件

首先使用 git log 查看日志，找到指定的 commit ID 号。

```

$ git log
commit ee808bb4b3ed6b7c0e7b24eeec39d299b6054dd0
Author: 168 <lineagelx@126.com>
Date:   Thu Oct 22 13:12:11 2015 +0800

    统计代码

commit 3e68ddef50eec39acea1b0e20fe68ff2217cf62b
Author: netkiller <netkiller@msn.com>
Date:   Fri Oct 16 14:39:10 2015 +0800

    页面修改

commit b111c253321fb4b9c5858302a0707ba0adc3cd07
Author: netkiller <netkiller@msn.com>
Date:   Wed Oct 14 17:51:55 2015 +0800

    数据库连接

commit 4a21667a576b2f18a7db8bdcddbd3ba305554ccb
Author: netkiller <netkiller@msn.com>
Date:   Wed Oct 14 17:27:15 2015 +0800

    init repo

```

导入 b111c253321fb4b9c5858302a0707ba0adc3cd07 至 ee808bb4b3ed6b7c0e7b24eeec39d299b6054dd0 间修改过的文件。

```
$ git archive -o update2.zip HEAD $(git diff --name-only b111c253321fb4b9c5858302a0707ba0adc3cd07)

```

### 14.3. 回撤提交

首先 reset 到指定的版本，根据实际情况选择 --mixed 还是 --hard

```

git reset --mixed 096392721f105686fc3cdafcb4159439a66b0e5b --
or
git reset --hard 33ba6503b4fa8eed35182262770e4eab646396cd --

```

```

git push origin --force --all
or
git push --force --progress "origin" master:master

```

### 14.4. 每个项目一个证书

方法一

```

[root@localhost ~]# cat .ssh/config 
host git.netkiller.cn
    user git
    hostname git.netkiller.cn
    port 22
    identityfile ~/.ssh/netkiller

host github.com
 HostName github.com
 IdentityFile ~/.ssh/id_rsa_github
 User git    

```

方法二

```

$ ssh-agent sh -c 'ssh-add ~/.ssh/id_rsa; git fetch user@host'			

```

## 第 181 章 Subversion

## 1. Invoking the Server

配置开发环境版本控制 Subversion

Squid + Subversion 请参考 Squid 一节

### 1.1. Installing

#### 1.1.1. Ubuntu

过程 181.1. subversion

1.  installation

    **$ sudo apt-get install subversion**

    ```
    $ sudo apt-get install subversion

    ```

2.  create svn group and svnroot user

    ```
    $ sudo groupadd svn
    $ sudo adduser svnroot --ingroup svn

    ```

3.  create repository

    ```
    $ svnadmin create /home/svnroot/test

    ```

4.  testing

    ```
    svnroot@netkiller:~$ svnserve -d --foreground -r /home/svnroot/

    ```

    check out

    neo@netkiller:/tmp$ svn list svn://localhost/test

    you may see some file and directory

    ```
    neo@netkiller:/tmp$ ls test/.svn/
    entries  format  prop-base  props  text-base  tmp

    ```

5.  configure

    $ vim repositories/conf/svnserve.conf

    ```
    [general]
    anon-access = read
    auth-access = write
    password-db = passwd
    # authz-db = authz
    # realm = My First Repository

    ```

    $ vim repositories/conf/passwd

    ```
    [users]
    # harry = harryssecret
    # sally = sallyssecret
    neo = chen

    ```

    如果不允许匿名用户 checkout 代码，配置文件这样写 anon-access = none

    ```
    [general]
    anon-access = none
    auth-access = write

    ```

6.  firewall

    ```
    $ sudo ufw allow svn

    ```

#### 1.1.2. CentOS 5

```

[root@development ~]# yum -y install subversion

```

##### 1.1.2.1. classic Unix-like xinetd daemon

```

[root@development ~]# vim /etc/xinetd.d/subversion
service subversion
{
    disable = no
    port = 3690
    socket_type = stream
    protocol = tcp
    wait = no
    user = svnroot
    server = /usr/bin/svnserve
    server_args = -i -r /home/svnroot
}

```

firewall

```

iptables -A INPUT -p tcp -m tcp --sport 3690 -j ACCEPT
iptables -A OUTPUT -p tcp -m tcp --dport 3690 -j ACCEPT

```

##### 1.1.2.2. WebDav

install webdav module

```

[root@development ~]# yum install mod_dav_svn

```

create directory

```

mkdir /var/www/repository
svnadmin create /var/www/repository

```

subversion.conf

```

[root@development ~]# vim /etc/httpd/conf.d/subversion.conf
LoadModule dav_module modules/mod_dav.so
LoadModule dav_svn_module modules/mod_dav_svn.so
LoadModule authz_svn_module modules/mod_authz_svn.so

```

vhost.conf

```

<Location />
DAV svn
SVNPath /var/www/repository
AuthType Basic
AuthName "Subversion Repository"
AuthUserFile /etc/subversion/svn-auth-file
Require valid-user
</Location>

```

auth file

```

[root@development ~]# htpasswd -c /etc/subversion/svn-auth-file my_user_name

```

##### 1.1.2.3. 项目目录结构

–trunk #存放主线

–branches #存放分支，可修改

–tags #存放标记，不可修改

#### 1.1.3. CentOS 6

CentOS 6 默认没有安装 xinetd

```
# yum install xinetd
# yum install subversion

# mkdir -p /opt/svnroot

```

xinetd 配置

```
# vim /etc/xinetd.d/svn
service svn
{
    disable = no
    port = 3690
    socket_type = stream
    protocol = tcp
    wait = no
    user = svnroot
    server = /usr/bin/svnserve
    server_args = -i -r /opt/svnroot
}

# /etc/init.d/xinetd restart
Stopping xinetd:                                           [FAILED]
Starting xinetd:                                           [  OK  ]

# tail /var/log/messages | grep xinetd
May  5 18:57:20 SZVM42-C1-02 yum: Installed: 2:xinetd-2.3.14-16.el5.i386
May  5 18:59:22 SZVM42-C1-02 xinetd[4558]: Unknown user: svnroot [file=/etc/xinetd.d/svn] [line=8]
May  5 18:59:22 SZVM42-C1-02 xinetd[4558]: Error parsing attribute user - DISABLING SERVICE

[file=/etc/xinetd.d/svn] [line=8]
May  5 18:59:22 SZVM42-C1-02 xinetd[4558]: xinetd Version 2.3.14 started with libwrap loadavg labeled-networking

options compiled in.
May  5 18:59:22 SZVM42-C1-02 xinetd[4558]: Started working: 0 available services

```

service 名字必须与 /etc/services 中定义的名字相同，否则将不能启动，同时在/var/log/message 中会提示如下

```
May  4 14:33:08 www xinetd[5656]: service/protocol combination not in /etc/services: subversion/tcp
May  4 14:33:08 www xinetd[5656]: xinetd Version 2.3.14 started with libwrap loadavg labeled-networking options compiled in.
May  4 14:33:08 www xinetd[5656]: Started working: 0 available services
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:33 www pulseaudio[21913]: sink-input.c: Failed to create sink input: too many inputs per sink.
May  4 14:33:41 www xinetd[5656]: Exiting...
May  4 14:33:41 www xinetd[5676]: xinetd Version 2.3.14 started with libwrap loadavg labeled-networking options compiled in.
May  4 14:33:41 www xinetd[5676]: Started working: 1 available service

```

### 1.2. standalone “daemon” process

svn daemon

```
$ svnserve --daemon --root /home/svnroot

```

#### 1.2.1. starting subversion for debian/ubuntu

/etc/init.d/subversion for debian/ubuntu

```

debian:/etc/init.d# cat subversion
#!/bin/sh
### BEGIN INIT INFO
# Provides:          subversion
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Should-Start:      fam
# Should-Stop:       fam
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start the subversion subversion server.
### END INIT INFO

#########################
# Author: Neo <openunix@163.com>
#########################

PATH=/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/usr/bin/svnserve
NAME=subversion
DESC="subversion server"
PIDFILE=/var/run/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME
SVNROOT=/srv/svnroot
DAEMON_OPTS="-d -T -r $SVNROOT --pid-file $PIDFILE"

test -x $DAEMON || exit 0

set -e

. /lib/lsb/init-functions

case "$1" in
    start)
        log_daemon_msg "Starting $DESC" $NAME
        echo
        $DAEMON $DAEMON_OPTS
        echo `pgrep -o $NAME` > $PIDFILE > /dev/null 2> /dev/null
        ;;
    stop)
        log_daemon_msg "Stopping $DESC" $NAME
        echo
        killall `basename $DAEMON` > /dev/null 2> /dev/null
        rm -rf $PIDFILE
        ;;
    restart)
        $0 stop
        $0 start
        ;;
    status)
        ps ax | grep $NAME
        ;;
    *)
        echo "Usage: $SCRIPTNAME {start|stop|restart|status}" >&2
        exit 1
        ;;
esac

exit 0

```

#### 1.2.2. starting subversion daemon script for CentOS/Radhat

```

#!/bin/bash
#
# /etc/rc.d/init.d/subversion
#
# Starts the Subversion Daemon
#
# chkconfig: 345 90 10
#
# description: Subversion Daemon

# processname: svnserve

source /etc/rc.d/init.d/functions

[ -x /usr/bin/svnserve ] || exit 1

### Default variables
SYSCONFIG="/etc/sysconfig/subversion"

### Read configuration
[ -r "$SYSCONFIG" ] && source "$SYSCONFIG"

RETVAL=0
USER="svnroot"
prog="svnserve"
desc="Subversion Daemon"

start() {
   echo -n $"Starting $desc ($prog): "
   daemon --user $USER $prog -d $OPTIONS
   RETVAL=$?
   [ $RETVAL -eq 0 ] && touch /var/lock/subsys/$prog
   echo
}

stop() {
   echo -n $"Shutting down $desc ($prog): "
   killproc $prog
   RETVAL=$?
   [ $RETVAL -eq 0 ] && success || failure
   echo
   [ $RETVAL -eq 0 ] && rm -f /var/lock/subsys/$prog
   return $RETVAL
}

case "$1" in
  start)
   start
   ;;
  stop)
   stop
   ;;
  restart)
   stop
   start
   RETVAL=$?
   ;;
  condrestart)
        [ -e /var/lock/subsys/$prog ] && restart
   RETVAL=$?
   ;;
  *)
   echo $"Usage: $0 {start|stop|restart|condrestart}"
   RETVAL=1
esac

exit $RETVAL

```

/etc/sysconfig/subversion

```
# Configuration file for the Subversion service

#
# To pass additional options (for instace, -r root of directory to server) to
# the svnserve binary at startup, set OPTIONS here.
#
#OPTIONS=
OPTIONS="--threads --root /srv/svnroot"

```

### 1.3. classic Unix-like inetd daemon

/etc/inetd.conf

```
svn stream tcp nowait svn /usr/bin/svnserve svnserve -i -r /home/svnroot/repositories

```

xinetd.d

/etc/xinetd.d/subversion

```
$ sudo apt-get install xinetd
$ sudo vim /etc/xinetd.d/subversion

service subversion
{
    disable = no
    port = 3690
    socket_type = stream
    protocol = tcp
    wait = no
    user = svnroot
    server = /usr/bin/svnserve
    server_args = -i -r /home/svnroot
}

```

restart xinetd

```
$ sudo /etc/init.d/xinetd restart

```

### 1.4. hooks

```
$ sudo apt-get install subversion-tools

```

#### 1.4.1. post-commit

install SVN::Notify

```
perl -MCPAN -e 'install SVN::Notify'

```

```

$ sudo cp post-commit.tmpl post-commit
$ sudo chown svnroot:svn post-commit
$ sudo vim post-commit

REPOS="$1"
REV="$2"

#/usr/share/subversion/hook-scripts/commit-email.pl "$REPOS" "$REV" openunix@163.com
/usr/share/subversion/hook-scripts/commit-email.pl "$1" "$2" --from neo@netkiller.8800.org -h localhost  -s "[SVN]" --diff y openunix@163.com openx@163.com

```

另一种方法

```

#!/bin/sh

REPOS="$1"
REV="$2"

/usr/local/bin/svnnotify                    \
    --repos-path    "$REPOS"                \
    --revision      "$REV"                  \
    --subject-cx                            \
    --with-diff                             \
    --handler       HTML::ColorDiff         \
    --to            <your e-mail address>   \
    --from          <from e-mail address>

```

```
/usr/bin/svnnotify --repos-path "$REPOS" --revision "$REV" \
--from neo@netkiller.8800.org --to openunix@163.com --smtp localhost \
--handler "HTML::ColorDiff"  --with-diff --charset zh_CN:GB2312  -g zh_CN --svnlook /usr/bin/svnlook --subject-prefix '[SVN]'

```

如果你没有安装邮件服务器，你可以使用服务商的 SMTP 如 163.com

```
/usr/bin/svnnotify --repos-path "$REPOS" --revision "$REV" \
--from openx@163.com --to openunix@163.com --smtp smtp.163.com  --smtp-user openunix --smtp-pass ****** \
--handler "HTML::ColorDiff"  --with-diff --charset UTF-8 --language zh_CN --svnlook /usr/bin/svnlook --subject-prefix '[SVN]'

```

Charset

```
REPOS="$1"
REV="$2"

svnnotify --repos-path "$REPOS" --revision "$REV" \
        --subject-cx \
        --from neo.chen@example.com \
        --to group@example.com,manager@example.com \
        --with-diff \
        --svnlook /usr/bin/svnlook \
        --subject-prefix '[SVN]' \
        --charset UTF-8  --language zh_CN

```

### 1.5. WebDav

Apache SVN

**$ sudo apt-get install libapache2-svn**

```
netkiller@neo:/etc/apache2$ sudo apt-get install libapache2-svn

```

vhost

```

<VirtualHost *>
        ServerName svn.netkiller.8800.org
        DocumentRoot /var/svn

      <Location />
                DAV svn
                SVNPath /var/svn
                AuthType Basic
                AuthName "Subversion Repository"
                AuthUserFile /etc/apache2/svn.passwd
                <LimitExcept GET PROPFIND OPTIONS REPORT>
                        Require valid-user
                </LimitExcept>
        </Location>
</VirtualHost>

```

建立密码文件

建立第一个用户需要加-c 参数

```
netkiller@neo:/etc/apache2$ sudo htpasswd2 -c /etc/apache2/svn.passwd svn
New password:
Re-type new password:
Adding password for user svn

```

输入两次密码

建立其他用户

```
sudo htpasswd2 /etc/apache2/svn.passwd otheruser

```

#### 1.5.1. davfs2 - mount a WebDAV resource as a regular file system

install

```
$ sudo apt-get install davfs2

```

mount a webdav to directory

```
$ sudo mount.davfs https://opensvn.csie.org/netkiller /mnt/davfs/
Please enter the username to authenticate with server
https://opensvn.csie.org/netkiller or hit enter for none.
Username: svn
Please enter the password to authenticate user svn with server
https://opensvn.csie.org/netkiller or hit enter for none.
Password:
mount.davfs: the server certificate is not trusted
  issuer:      CSIE.org, CSIE.org, Taipei, Taiwan, TW
  subject:     CSIE.org, CSIE.org, Taipei, TW
  identity:    *.csie.org
  fingerprint: e6:05:eb:fb:69:5d:25:4e:11:3c:83:e8:7c:44:ee:bf:a9:85:a3:64
You only should accept this certificate, if you can
verify the fingerprint! The server might be faked
or there might be a man-in-the-middle-attack.
Accept certificate for this session? [y,N] y

```

test

```
$ ls davfs/
branches  lost+found  tags  trunk

```

## 2. repository 管理

### 2.1. create repository

```
$ su - svnroot
$ svnadmin create /home/svnroot/neo

```

### 2.2. user admin

```

#!/bin/bash
###############################
# Author: Neo<openunix@163.com
# Home: http://netkiller.sf.net
###############################
SVNROOT=/srv/svnroot/project
adduser(){
    echo $1  $2
    if [ -z $1 ]; then
        usage
    else
        local user=$1
    fi
    if [ -z $2 ]; then
        usage
    else
        local passwd=$2
    fi
    echo "$1 = $2" >> $SVNROOT/conf/passwd
}
deluser(){
    local user=$1
    if [ -z $user ]; then
        usage
    else
        ed -s $SVNROOT/conf/passwd <<EOF
/$user/
d
wq
EOF
    fi
}
list(){
    cat $SVNROOT/conf/passwd
}
usage(){
    echo $"Usage: $0 {list|add|del} username"
}
case "$1" in
    list)
        list
        ;;
    add)
        adduser $2 $3
        ;;
    del)
        deluser $2
        ;;
    restart)
        stop
        start
        ;;
    condrestart)
        condrestart
        ;;

    *)
        usage
        exit 1
esac

```

用法

```
./svnuser list
./svnuser add user passwd
./svnuser del user

```

### 2.3. authz

$ svnadmin create /home/svnroot/project

$ svnserve --daemon --root /home/svnroot/project

```
[groups]
member = neo
blog = neo,netkiller
wiki = bg7nyt,chen,jingfeng

[/]
* =

[/member]
@member = rw
* = r

[/app/blog]
@blog = rw
* =

[/app/wiki]
@blog = rw
* =

# [repository:/baz/fuz]
# @harry_and_sally = rw
# * = r

```

$ svnadmin create /home/svnroot/project1

$ svnadmin create /home/svnroot/project2

$ svnserve --daemon --root /home/svnroot

```
[groups]
member = neo
blog = neo,netkiller
wiki = bg7nyt,chen,jingfeng

[project1:/]
* =
[project2:/]
* = r

[project1:/member]
@member = rw
* = r

[project2:/app/blog]
@blog = rw
* =

[project2:/app/wiki]
@blog = rw
* = r

```

例 181.1. authz

```
[aliases]
# joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average

### This file is an example authorization file for svnserve.
### Its format is identical to that of mod_authz_svn authorization
### files.
### As shown below each section defines authorizations for the path and
### (optional) repository specified by the section name.
### The authorizations follow. An authorization line can refer to:
###  - a single user,
###  - a group of users defined in a special [groups] section,
###  - an alias defined in a special [aliases] section,
###  - all authenticated users, using the '$authenticated' token,
###  - only anonymous users, using the '$anonymous' token,
###  - anyone, using the '*' wildcard.
###
### A match can be inverted by prefixing the rule with '~'. Rules can
### grant read ('r') access, read-write ('rw') access, or no access
### ('').

[aliases]
# joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average

[groups]

manager = neo
developer = jam,john,zen
tester = eva
designer = allan
deployer = ken

[/]
@manager = rw
@developer = r
@designer = r
@deployer = r
@tester = r
* =

#############################
# Trunk
# ##########################
[/www.mydomain.com/trunk]
@manager = rw
@designer = rw
@developer = rw
@deployer = r

[/images.mydomain.com/trunk]
@designer = rw

[/myid.mydomain.com/trunk]
@designer = r

[/info.mydomain.com/trunk]
@developer = r
@designer = r

#############################
#\Branches
#############################
[/admin.mydomain.com/branches]
@developer = rw
@designer = rw

[/myid.mydomain.com/branches]
@developer = rw
@designer = rw

[/info.mydomain.com/branches]
@developer = rw
@designer = rw

[/www.mydomain.com/branches]
@developer = rw
@designer = rw

[/images.mydomain.com/branches]
@developer = rw
@designer = rw

[/log.mydomain.com/branches]
@developer = rw

[/report.mydomain.com/branches]
@developer = rw

###############################
# TAGS
# #############################
[/myid.mydomain.com/tags]
@deployer = rw
[/admin.mydomain.com/tags]
@deployer = rw
[/info.mydomain.com/tags]
@deployer = rw

```

### 2.4. dump

```
svnadmin dump /svnroot/project | gzip > svn.gz

```

## 3. 使用 Subversion

### 3.1. Initialized empty subversion repository for project

```
svn co svn://127.0.0.1/project
cd project
mkdir trunk
mkdir tags
mkdir branches
svn ci -m "Initialized empty subversion repository in your_project"

```

### 3.2. ignore

**svn propset svn:ignore [filename] [folder]**

```
$ svn propset svn:ignore 'images' .
$ svn ci -m 'Ignoring a directory called "images".'

```

```
$ svn propset svn:ignore '*' images
$ svn ci -m 'Ignoring a directory called "images".'

```

```
$ svn export spool spool-tmp
$ svn rm spool
$ svn ci -m 'Removing inadvertently added directory "spool".'
$ mv spool-tmp spool
$ svn propset svn:ignore 'spool' .
$ svn ci -m 'Ignoring a directory called "spool".'

```

.ignore

```
svn propset svn:ignore -F .cvsignore .
svn propset -R svn:ignore -F .svnignore .

```

### 3.3. 关键字替换

```
Date
	 这个关键字保存了文件最后一次在版本库修改的日期，看起来类似于$Date: 2012-08-06 17:43:09 +0800 (Mon, 06 Aug 2012) $，它也可以用 LastChangedDate 来指定。
Revision
	 这个关键字描述了这个文件最后一次修改的修订版本，看起来像$Revision: 446 $，也可以通过 LastChangedRevision 或者 Rev 引用。
Author
	 这个关键字描述了最后一个修改这个文件的用户，看起来类似$Author: netkiller $，也可以用 LastChangedBy 来指定。
HeadURL
	 这个关键字描述了这个文件在版本库最新版本的完全 URL，看起来类似$HeadURL: https://svn.code.sf.net/p/netkiller/svn/trunk/Docbook/Version/section.version.svn.xml $，可以缩写为 URL。
Id
	 这个关键字是其他关键字一个压缩组合，它看起来就像$Id: section.version.svn.xml 446 2012-08-06 09:43:09Z netkiller $，可以解释为文件 calc.c 上一次修改的修订版本号是 148，时间是 2006 年 7 月 28 日，作者是 sally。

```

```
$ cat weather.txt
$Id: section.version.svn.xml 446 2012-08-06 09:43:09Z netkiller $

$ svn propset svn:keywords "Id" weather.txt
property 'svn:keywords' set on 'weather.txt'

$ cat weather.txt
$Id: section.version.svn.xml 446 2012-08-06 09:43:09Z netkiller $

```

设置多个关键字

```
$ svn propset svn:keywords "Author HeadURL Id Revision" -R *.php

```

```
svn -R propset svn:keywords -F .keywords *

```

### 3.4. lock 加锁/ unlock 解锁

$ svn lock -m "LockMessage" [–force] PATH

```
$ svn lock -m “lock test file“ test.php
$ svn unlock PATH

```

### 3.5. import

```
svn import [PATH] URL
svn export URL [PATH]

```

### 3.6. export 指定版本

```
svn log file
svn export -r rxxxxx file
or
svn export -r rxxxxx file newfile
svn ci -m "restore rxxxxxx"

```

### 3.7. 修订版本关键字

```
HEAD

    版本库中最新的（或者是“最年轻的”）版本。
BASE

    工作拷贝中一个条目的修订版本号，如果这个版本在本地修改了，则“BASE 版本”就是这个条目在本地未修改的版本。
COMMITTED

    项目最近修改的修订版本，与 BASE 相同或更早。
PREV

    一个项目最后修改版本之前的那个版本，技术上可以认为是 COMMITTED -1。

```

```
$ svn cat -r PREV filename > filename
$ svn diff -r PREV filename

```

### 3.8. 恢复旧版本

svn 没有恢复旧版本的直接功能，不过可以使用 svn merge 命令恢复。比如说当前 HEAD 为 2，而我要恢复成 1 版本，怎么做？

用 svn merge：

```

svn update
svn merge --revision 2:1 svn://localhost/lynn
svn commit -m "restore to revision 1"

```

svn merge --r HEAD:1 svn://localhost/lynn

## 4. branch

### 4.1. create

create a new branch using copy

```
svn cp http://www.domain.com/truck/project http://www.domain.com/branches/project_branch_1

```

### 4.2. remove

remove

```
svn rm http://www.domain.com/branches/project_branch_1

```

### 4.3. switch

```
svn switch http://www.domain.com/branches/project_branch_2 .

```

### 4.4. merge

```
svn -r 148:149 merge svn://server/trunk branches/module

```

### 4.5. relocate

switch --relocate FROM TO [PATH...]

```
svn switch --relocate svn://192.168.3.9/neo svn://192.168.3.5/neo .

```

## 5. FAQ

### 5.1. 递归添加文件

```
$ svn add `svn st | grep '?' | awk '{print $2}'`

```

### 5.2. 清除项目里的所有.svn 目录

```
find . -type d -iname ".svn" -exec rm -rf {} \;

```

### 5.3. color diff

http://colordiff.sourceforge.net/

```
$ sudo apt-get install colordiff

```

add the following to your ~/.bashrc

```
alias svndiff=’svn diff --diff-cmd=colordiff’

```

### 5.4. cvs2svn

http://cvs2svn.tigris.org/

```
[root@development ~]# cvs2svn --encoding=gb2312 --fallback-encoding=utf_8 --existing-svnrepos --svnrepos /home/svnroot /home/cvsroot
[root@development ~]# cvs2svn --encoding=gb2312 --fallback-encoding=utf_8 --svnrepos /home/svnroot /home/cvsroot

```

### 5.5. Macromedia Dreamweaver MX 2004 + WebDAV +Subversion

首先进入站点管理

![](img/image001.jpg)

单击新建(New…)按钮选择站点(Site)

![](img/image002.jpg)

显示站点设置面版 Local Info 中设置

![](img/image003.jpg)

Remote Info 中设置

![](img/image004.jpg)

单击设置按钮（settings）

![](img/image005.jpg)

单击确定

![](img/image006.jpg)

单击 Done 完成

连接 WebDAV 服务器

![](img/image007.jpg)

单击

![](img/image008.jpg)

连接

![](img/image009.jpg)

### 5.6. 指定用户名与密码

```
svn co svn://www.example.com/repos  --username neo --password chen;

```

## 第 182 章 cvs - Concurrent Versions System

## 1. installation

过程 182.1. install cvs

1.  install

    ```
    $ sudo apt-get install xinetd
    $ sudo apt-get install cvs

    ```

    show the cvs version

    ```
    $ cvs -v

    Concurrent Versions System (CVS) 1.12.13 (client/server)

    ```

2.  create cvs group and cvsroot user

    ```
    $ sudo groupadd cvs
    $ sudo adduser cvsroot --ingroup cvs

    ```

    change user become cvsroot

    ```
    $ su - cvsroot

    ```

3.  initialization 'CVSROOT'

    ```
    $ cvs -d /home/cvsroot init

    ```

    if you have successed, you can see CVSROOT directory in the '/home/cvsroot'

    ```
    $ ls /home/cvsroot/
    CVSROOT

    ```

4.  authentication

    default SystemAuth=yes, you can use system user to login cvs.

    but usually, we don't used system user because it isn't security.

    SystemAuth = no

    edit '/home/cvsroot/CVSROOT/config' make sure SystemAuth = no

    ```
    $ vim /home/cvsroot/CVSROOT/config
    SystemAuth = no

    ```

    create passwd file

    the format is user:password:cvsroot

    you need to using htpasswd command, if you don't have, please install it as the following

    ```
    $ sudo apt-get install apache2-utils

    ```

    or

    ```
    $ perl -e 'print("userPassword: ".crypt("secret","salt")."\n");'

    ```

    or

    ```
    $ cat passwd
    #!/usr/bin/perl
    srand (time());
    my $randletter = "(int (rand (26)) + (int (rand (1) + .5) % 2 ? 65 : 97))";
    my $salt = sprintf ("%c%c", eval $randletter, eval $randletter);
    my $plaintext = shift; my $crypttext = crypt ($plaintext, $salt);
    print "${crypttext}\n";

    $ ./passwd "mypasswd"
    atfodI2Y/dcdc

    ```

    let's using htpasswd to create a passwd

    ```
    $ htpasswd -n neo
    New password:
    Re-type new password:
    neo:yA50LI1BkXysY

    ```

    copy 'neo:yA50LI1BkXysY' and add ':cvsroot' to the end

    ```
    $ vim /home/cvsroot/CVSROOT/passwd
    neo:yA50LI1BkXysY:cvsroot
    nchen:GXaAkSKaQ/Hpk:cvsroot

    ```

5.  Go into directory '/etc/xinetd.d/', and then create a cvspserver file as the following.

    ```
    $ sudo vim /etc/xinetd.d/cvspserver

    service cvspserver
    {
       disable = no
       flags = REUSE
       socket_type = stream
       wait = no
       user = cvsroot
       server = /usr/bin/cvs
       server_args = -f --allow-root=/home/cvsroot pserver
       log_on_failure += USERID
    }

    ```

6.  check cvspserver in the '/etc/services'

    ```
    $ grep cvspserver /etc/services
    cvspserver      2401/tcp                        # CVS client/server operations
    cvspserver      2401/udp

    ```

7.  restart xinetd

    ```
    $ /etc/init.d/xinetd
    Usage: /etc/init.d/xinetd {start|stop|reload|force-reload|restart}

    ```

8.  port

    ```
    $ nmap localhost -p cvspserver

    Starting Nmap 4.53 ( http://insecure.org ) at 2008-11-14 16:21 HKT
    Interesting ports on localhost (127.0.0.1):
    PORT     STATE SERVICE
    2401/tcp open  cvspserver

    Nmap done: 1 IP address (1 host up) scanned in 0.080 seconds

    ```

9.  firewall

    ```
    $ sudo ufw allow cvspserver

    ```

environment variable

CVSROOT=:pserver:username@ip:/home/cvsroot

```
vim .bashrc

export CVS_RSH=ssh
export CVSROOT=:pserver:neo@localhost:/home/cvsroot

```

test

```
$ cvs login
Logging in to :pserver:neo@localhost:2401/home/cvsroot
CVS password:
neo@netkiller:/tmp/test$ cvs co test
cvs checkout: Updating test
U test/.project
U test/NewFile.xml
U test/newfile.php
neo@netkiller:/tmp/test$

```

### 1.1. chroot

```
$ sudo apt-get install cvsd

```

environment variable

```
neo@netkiller:~/workspace/cvs$ export CVSROOT=:pserver:neo@localhost:/home/cvsroot

```

ssh

```
export CVS_RSH=ssh
export CVSROOT=:ext:$USER@localhost:/home/cvsroot

```

## 2. cvs login | logout

```
neo@netkiller:~/workspace/cvs$ cvs login
Logging in to :pserver:neo@localhost:2401/home/cvsroot
CVS password:

```

logout

```
$ cvs logout
Logging out of :pserver:neo@localhost:2401/home/cvsroot

```

## 3. cvs import

cvs import -m "write some comments here" project_name vendor_tag release_tag

```
$ cvs import -m "write some comments here" project_name vendor_tag release_tag

```

## 4. cvs checkout

```
$ cvs checkout project_name
cvs checkout: Updating project_name

```

checkout before

**cvs checkout -r release_1_0 project_name**

```
$ cvs checkout -r release_1_0 project_name
cvs checkout: Updating project_name
U project_name/file
cvs checkout: Updating project_name/dir1
U project_name/dir1/file1
cvs checkout: Updating project_name/dir2
U project_name/dir2/file1
U project_name/dir2/file2

```

## 5. cvs update

about update

```
$ cvs update
$ cvs update -r HEAD
$ cvs update -r 1.5
$ cvs update -D now
$ cvs update -D now file

```

## 6. cvs add

```
$ cd project_name/
$ touch new_file
$ cvs add new_file
cvs add: scheduling file `new_file' for addition
cvs add: use `cvs commit' to add this file permanently

```

if the file is binary

cvs add -kb new_file.gif

add a directory

```
$ mkdir dir1
$ mkdir dir2
$ touch dir1/file1
$ touch dir2/file1
$ touch dir2/file2
$ cvs add dir1
? dir1/file1
Directory /home/cvsroot/project_name/dir1 added to the repository
$ cvs add dir2
? dir2/file1
? dir2/file2
Directory /home/cvsroot/project_name/dir2 added to the repository

```

add mulit files

```
$ cvs add dir1/file1
$ cvs add dir2/file?

```

## 7. cvs status

```
$ cvs status dir1/file1
cvs status: use `cvs add' to create an entry for `dir1/file1'
===================================================================
File: file1             Status: Unknown

   Working revision:  No entry for file1
   Repository revision: No revision control file

```

## 8. cvs commit

```

$ cvs commit -m "add a new file"
cvs commit: Examining .
/home/cvsroot/project_name/new_file,v  <--  new_file
initial revision: 1.1

```

commit multi files

```

$ cvs commit -m "add a new file" dir1/* dir2/*
/home/cvsroot/project_name/dir1/file1,v  <--  dir1/file1
initial revision: 1.1
/home/cvsroot/project_name/dir2/file1,v  <--  dir2/file1
initial revision: 1.1
/home/cvsroot/project_name/dir2/file2,v  <--  dir2/file2
initial revision: 1.1

```

## 9. cvs remove

```

$ rm -rf new_file
$ cvs remove new_file
cvs remove: scheduling `new_file' for removal
cvs remove: use `cvs commit' to remove this file permanently
$ cvs commit -m "delete file" new_file
/home/cvsroot/project_name/new_file,v  <--  new_file
new revision: delete; previous revision: 1.1

```

## 10. cvs log

let me create a file, and then modify the file to make several version

```

$ touch file
$ echo helloworld > file
$ cvs add file
cvs add: scheduling file `file' for addition
cvs add: use `cvs commit' to add this file permanently
$ cvs commit -m 'add file to cvs' file
/home/cvsroot/project_name/file,v  <--  file
initial revision: 1.1
$ echo I am Neo > file
$ cvs commit -m 'add file to cvs' file
/home/cvsroot/project_name/file,v  <--  file
new revision: 1.2; previous revision: 1.1
$ echo my nickname is netkiller > file
$ cvs commit -m 'modified file' file
/home/cvsroot/project_name/file,v  <--  file
new revision: 1.3; previous revision: 1.2
$ echo I am 28 years old > file
$ cvs commit -m 'modified file' file
/home/cvsroot/project_name/file,v  <--  file
new revision: 1.4; previous revision: 1.3

```

show log message

```

$ cvs log file

RCS file: /home/cvsroot/project_name/file,v
Working file: file
head: 1.4
branch:
locks: strict
access list:
symbolic names:
keyword substitution: kv
total revisions: 4;   selected revisions: 4
description:
----------------------------
revision 1.4
date: 2008-11-24 15:42:49 +0800;  author: neo;  state: Exp;  lines: +1 -1;  commitid: V0iuptfP43iETPrt;
modified file
----------------------------
revision 1.3
date: 2008-11-24 15:42:20 +0800;  author: neo;  state: Exp;  lines: +1 -1;  commitid: YWfYHFSV10duTPrt;
modified file
----------------------------
revision 1.2
date: 2008-11-24 15:41:47 +0800;  author: neo;  state: Exp;  lines: +1 -1;  commitid: 4iRs5fm1g9diTPrt;
add file to cvs
----------------------------
revision 1.1
date: 2008-11-24 15:41:28 +0800;  author: neo;  state: Exp;  commitid: zCWkxnWxLZHbTPrt;
add file to cvs
=============================================================================

```

cvs log -r1.2 file

```
$ cvs log -r1.2 file

RCS file: /home/cvsroot/project_name/file,v
Working file: file
head: 2.1
branch:
locks: strict
access list:
symbolic names:
        release_1_0_patch: 1.4.0.2
        release_1_0: 1.4
keyword substitution: kv
total revisions: 5;     selected revisions: 1
description:
----------------------------
revision 1.2
date: 2008-11-24 15:41:47 +0800;  author: neo;  state: Exp;  lines: +1 -1;  commitid: 4iRs5fm1g9diTPrt;
add file to cvs
=============================================================================

```

## 11. cvs annotate

```
$ cvs annotate file

Annotations for file
***************
2.2          (nchen    26-Nov-08): I am Neo
2.2          (nchen    26-Nov-08): My nickname netkiller
2.3          (nchen    26-Nov-08): I'm from shenzhen
1.4          (neo      24-Nov-08): I am 28 years old

```

## 12. cvs diff

```

neo@netkiller:~/workspace/cvs/project_name$ cvs diff -r1.3 -r1.4 file
Index: file
===================================================================
RCS file: /home/cvsroot/project_name/file,v
retrieving revision 1.3
retrieving revision 1.4
diff -r1.3 -r1.4
1c1
< my nickname is netkiller
---
> I am 28 years old
neo@netkiller:~/workspace/cvs/project_name$ cvs diff -r1.2 -r1.4 file
Index: file
===================================================================
RCS file: /home/cvsroot/project_name/file,v
retrieving revision 1.2
retrieving revision 1.4
diff -r1.2 -r1.4
1c1
< I am Neo
---
> I am 28 years old

```

--side-by-side

```
neo@netkiller:/tmp/cvs/test/project_name$ cvs diff --side-by-side -r1.2 -r1.4 file
Index: file
===================================================================
RCS file: /home/cvsroot/project_name/file,v
retrieving revision 1.2
retrieving revision 1.4
diff --side-by-side -r1.2 -r1.4
I am Neo                                                      | I am 28 years old

```

## 13. rename file

mv file_name new_file_name && cvs remove file_name
cvs add new_file_name

```

neo@netkiller:/tmp/cvs/project_name$ mv file file.txt
neo@netkiller:/tmp/cvs/project_name$ cvs remove file
cvs remove: scheduling `file' for removal
cvs remove: use `cvs commit' to remove this file permanently
neo@netkiller:/tmp/cvs/project_name$ cvs add file.txt
cvs add: scheduling file `file.txt' for addition
cvs add: use `cvs commit' to add this file permanently
neo@netkiller:/tmp/cvs/project_name$ cvs commit -m 'rename file to file.txt'
cvs commit: Examining .
cvs commit: Examining dir1
cvs commit: Examining dir2
/home/cvsroot/project_name/file,v  <--  file
new revision: delete; previous revision: 2.3
/home/cvsroot/project_name/file.txt,v  <--  file.txt
initial revision: 1.1

```

## 14. revision

```
neo@netkiller:~/workspace/cvs/project_name$ cvs update -r 1.2 file
U file
neo@netkiller:~/workspace/cvs/project_name$ cvs st file
===================================================================
File: file              Status: Up-to-date

   Working revision:  1.2
   Repository revision: 1.2     /home/cvsroot/project_name/file,v
   Commit Identifier: 4iRs5fm1g9diTPrt
   Sticky Tag:        1.2
   Sticky Date:       (none)
   Sticky Options:    (none)

```

last version

```
neo@netkiller:~/workspace/cvs/project_name$ cvs update -r HEAD file
U file
neo@netkiller:~/workspace/cvs/project_name$ cvs st file
===================================================================
File: file              Status: Up-to-date

   Working revision:  1.4
   Repository revision: 1.4     /home/cvsroot/project_name/file,v
   Commit Identifier: V0iuptfP43iETPrt
   Sticky Tag:        HEAD (revision: 1.4)
   Sticky Date:       (none)
   Sticky Options:    (none)

```

## 15. cvs export

**cvs export -r release_1_0 project_name**

```
$ cvs export -r release_1_0 project_name
cvs export: Updating project_name
U project_name/file
cvs export: Updating project_name/dir1
U project_name/dir1/file1
cvs export: Updating project_name/dir2
U project_name/dir2/file1
U project_name/dir2/file2

```

cvs export -D 20081126 project_name

```
$ cvs export -D 20081126 project_name
cvs export: Updating project_name
U project_name/file
cvs export: Updating project_name/dir1
U project_name/dir1/file1
cvs export: Updating project_name/dir2
U project_name/dir2/file1
U project_name/dir2/file2

```

**cvs export -D now -d nightly project_name**

```
$ cvs export -D now -d nightly project_name
cvs export: Updating nightly
U nightly/file
cvs export: Updating nightly/dir1
U nightly/dir1/file1
cvs export: Updating nightly/dir2
U nightly/dir2/file1
U nightly/dir2/file2
neo@netkiller:/tmp/cvs$

```

## 16. cvs release

```
$ ls
project_name

$ cvs release -d project_name
You have [0] altered files in this repository.
Are you sure you want to release (and delete) directory `project_name': y

$ ls

```

## 17. branch

### 17.1. milestone

set up a release number

```
$ cvs tag release_1_0
cvs tag: Tagging .
T file
cvs tag: Tagging dir1
T dir1/file1
cvs tag: Tagging dir2
T dir2/file1
T dir2/file2

```

beginning next one milestone

```

$ cvs commit -r 2

Log message unchanged or not specified
a)bort, c)ontinue, e)dit, !)reuse this message unchanged for remaining dirs
Action: (continue) c

CVS: ----------------------------------------------------------------------
CVS: Enter Log.  Lines beginning with `CVS:' are removed automatically
CVS:
CVS: Committing in .
CVS:
CVS: Modified Files:
CVS:  Tag: 2
CVS:    file dir1/file1 dir2/file1 dir2/file2
CVS: ----------------------------------------------------------------------

/home/cvsroot/project_name/file,v  <--  file
new revision: 2.1; previous revision: 1.4
/home/cvsroot/project_name/dir1/file1,v  <--  dir1/file1
new revision: 2.1; previous revision: 1.1
/home/cvsroot/project_name/dir2/file1,v  <--  dir2/file1
new revision: 2.1; previous revision: 1.1
/home/cvsroot/project_name/dir2/file2,v  <--  dir2/file2
new revision: 2.1; previous revision: 1.1

```

other user

```
$ cvs up
cvs update: Updating .
P file
cvs update: Updating dir1
U dir1/file1
cvs update: Updating dir2
U dir2/file1
U dir2/file2

$ cvs st file
===================================================================
File: file              Status: Up-to-date

   Working revision:    2.1
   Repository revision: 2.1     /home/cvsroot/project_name/file,v
   Commit Identifier:   SuZpTC1gCRrH2Qrt
   Sticky Tag:          (none)
   Sticky Date:         (none)
   Sticky Options:      (none)

```

### 17.2. patch branch

create a branch release_1_0_patch from release_1_0 by cvs admin

```
$ cvs rtag -b -r release_1_0 release_1_0_patch project_name
cvs rtag: Tagging project_name
cvs rtag: Tagging project_name/dir1
cvs rtag: Tagging project_name/dir2

```

checkout release_1_0_patch by other user

```
$ cvs checkout -r release_1_0_patch project_name
cvs checkout: Updating project_name
U project_name/file
cvs checkout: Updating project_name/dir1
U project_name/dir1/file1
cvs checkout: Updating project_name/dir2
U project_name/dir2/file1
U project_name/dir2/file2

```

show the status, and you can see 'Sticky Tag' is 'release_1_0_patch'

```
$ cvs st file
===================================================================
File: file              Status: Up-to-date

   Working revision:    1.4
   Repository revision: 1.4     /home/cvsroot/project_name/file,v
   Commit Identifier:   V0iuptfP43iETPrt
   Sticky Tag:          release_1_0_patch (branch: 1.4.2)
   Sticky Date:         (none)
   Sticky Options:      (none)

```

## 18. keywords

$Author: netkiller $
$Date: 2012-02-03 17:18:44 +0800 (Fri, 03 Feb 2012) $
$Name$
$Id: section.version.cvs.xml 340 2012-02-03 09:18:44Z netkiller $
$Header$
$Log$
$Revision: 340 $

add above keywords into a file, and then commit it.

```
$ cat file.txt
$Author: netkiller $
$Date: 2012-02-03 17:18:44 +0800 (Fri, 03 Feb 2012) $
$Name:  $
$Id: section.version.cvs.xml 340 2012-02-03 09:18:44Z netkiller $
$Header: /home/cvsroot/project_name/file.txt,v 1.2 2008-11-27 01:33:29 nchen Exp $
$Log: file.txt,v $
Revision 1.2  2008-11-27 01:33:29  nchen
added some of keywords

$Revision: 340 $

```

## 第 183 章 其他命令