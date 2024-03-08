# 部分 II. Shell

## 第 22 章 Bash Shell

## 1. 快捷键

```

Ctrl+p shell 中上一个命令,或者 文本中移动到上一行
Ctrl+n shell 中下一个命令,或者 文本中移动到下一行
Ctrl+r 往后搜索历史命令
Ctrl+s 往前搜索历史命令
Ctrl+f 光标前移
Ctrl+b 光标后退
Ctrl+a 到行首
Ctrl+e 到行尾
Ctrl+d 删除一个字符,删除一个字符，相当于通常的 Delete 键
Ctrl+h 退格删除一个字符,相当于通常的 Backspace 键
Ctrl+u 删除到行首
Ctrl+k 删除到行尾
Ctrl+l 类似 clear 命令效果
Ctrl+y 粘贴		

```

## 2. bash - GNU Bourne-Again SHell

### 2.1. -n 检查脚本是否有语法错误

### 2.2. -x 显示详细运行过程

## 3. Introduction

### 3.1. chsh - change login shell

```
# chsh --list
/bin/sh
/bin/bash
/sbin/nologin
/bin/tcsh
/bin/csh
/bin/ksh

# chsh --list-shells
/bin/sh
/bin/bash
/sbin/nologin
/bin/dash
/bin/zsh

```

```
$ chsh -s /bin/zsh
or
$ usermod -s /bin/zsh

```

show me current shell

```
neo@netkiller:~$ echo $SHELL
/bin/zsh

neo@netkiller:~$ cat /etc/passwd|grep neo
neo:x:1000:1000:Neo Chen,,,:/home/neo:/bin/zsh

```

### 3.2. 切换身份

判断当前用户是否为 root

```
#!/bin/bash
if [[ $EUID -ne 0 ]]; then
   echo "This script must be run as root" 
   exit 1
fi		

```

使用 #!/bin/su 可以切换当前 shell 的所有者，全局切换

```
# cat test.sh

#!/bin/su www
ls

```

局部切换，运行$PROG 后将 pid（进程 ID）写入$PIDFILE 文件

```

su - $USER -c "$PROG & echo \$! > $PIDFILE"

```

### 3.3. I/O 重定向

```

cat <<End-of-message
   8 -------------------------------------
   9 This is line 1 of the message.
  10 This is line 2 of the message.
  11 This is line 3 of the message.
  12 This is line 4 of the message.
  13 This is the last line of the message.
  14 -------------------------------------
End-of-message

```

```

MYSQL=mysql
MYSQLOPTS="-h $zs_host -u $zs_user -p$zs_pass $zs_db"

$MYSQL $MYSQLOPTS <<SQL
SELECT
        category.cat_id AS  cat_id ,
        category.cat_name AS  cat_name ,
        category.cat_desc AS  cat_desc ,
        category.parent_id AS  parent_id ,
        category.sort_order AS  sort_order ,
        category.measure_unit AS  measure_unit ,
        category.style AS  style ,
        category.is_show AS is_show ,
        category.grade AS  grade
FROM  category
SQL

```

<<-LimitString 可以抑制输出时前边的 tab(不是空格). 这可以增加一个脚本的可读性.

```

cat <<-ENDOFMESSAGE
	This is line 1 of the message.
	This is line 2 of the message.
	This is line 3 of the message.
	This is line 4 of the message.
	This is the last line of the message.
ENDOFMESSAGE

```

关闭参数替换

```

NAME="John Doe"
RESPONDENT="the author of this fine script"

cat <<'Endofmessage'

Hello, there, $NAME.
Greetings to you, $NAME, from $RESPONDENT.

Endofmessage

```

```

NAME="John Doe"
RESPONDENT="the author of this fine script"

cat <<\Endofmessage

Hello, there, $NAME.
Greetings to you, $NAME, from $RESPONDENT.

Endofmessage

```

#### 3.3.1. stdout

```
$ ln -s /dev/stdout test
$ cat file > test

```

#### 3.3.2. error 重定向

```

your_shell 2>&1

```

错误输出演示

```

[root@localhost ~]# id ethereum
id: ethereum: no such user

# 这里可以看到错误输出 id: ethereum: no such user

[root@localhost ~]# id ethereum > test
id: ethereum: no such user

我们尝试将他重定向到文件 test，但是结果仍是输出 id: ethereum: no such user 

[root@localhost ~]# cat test 
[root@localhost ~]#

查看 test 文件，内容空。

```

继续做实验

```

[root@localhost ~]# id ethereum > test 2>&1 
[root@localhost ~]# cat test 
id: ethereum: no such user

```

测试实验结果成功了，将错误输出转到标准输出，然后写入文件。

#### 3.3.3. 使用块记录日志

```

{
	...
	...
} > $LOGFILE 2>&1

```

#### 3.3.4. tee - read from standard input and write to standard output and files

```
echo 1 > /proc/sys/net/ipv4/ip_forward
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward;

```

##### 3.3.4.1. 重定向到文件

```

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://du8c1in9.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker

```

##### 3.3.4.2. nettee - a network "tee" program

#### 3.3.5. 创建文件

```

cat << EOF > foo.sh
   printf "%s was here" "$name"
EOF

cat >> foo.sh <<EOF
   printf "%s was here" "$name"
EOF

```

#### 3.3.6. 快速清空一个文件的内容

```
$ > /www/access.log

```

### 3.4. pipes (FIFOs)

create a pipes

```
$ mkfifo /tmp/pipe
$ mkfifo -m 0644 /tmp/pipe

$ mknod /tmp/pipe p

```

let's see it

```
$ ls -l /tmp/piple
prw-r--r-- 1 neo neo 0 2009-03-13 14:40 /tmp/piple

```

remove a pipes

```
rm /tmp/pipe

```

using it

standing by pipe

```
$ cat /tmp/pipe

```

push string to pipe

```
$ echo hello world > /tmp/pipe

```

fetch string from /tmp/pipe

```
$ cat /tmp/piple
hello world

```

### 3.5. mktemp - create a temporary file or directory 临时目录与文件

```
# mktemp
/tmp/tmp.p8p0v5YzPf

# mktemp /tmp/test.XXX
/tmp/test.d8J

# mktemp /tmp/test.XXXXXX
/tmp/test.cFebDX

# mktemp /tmp/test.XXXXXXX
/tmp/test.CnyLr7C

```

创建临时目录

```
# mktemp -d
/tmp/tmp.xg5gFj0w8D

# mktemp -d --suffix=.tmp /tmp/test.XXXXX
/tmp/test.TDpz8.tmp

$ mktemp -d --suffix=.tmp -p /tmp deploy.XXXXXX
/tmp/deploy.FwebCc.tmp

```

### 3.6. History 命令历史记录

#### 3.6.1. .bash_history

从安全角度考虑禁止记录 history

```
ln -s /dev/null .bash_history

```

##### 3.6.1.1. 格式定义

定制.bash_history 格式

```
export HISTSIZE=1000
export HISTFILESIZE=2000
export HISTTIMEFORMAT="%Y-%m-%d-%H:%M:%S "
export HISTFILE="~/.bash_history"

```

看看实际效果

```
$ history | head
    1  2012-02-27-09:10:45 do-release-upgrade
    2  2012-02-27-09:10:45 vim /etc/network/interfaces
    3  2012-02-27-09:10:45 vi /etc/network/interfaces
    4  2012-02-27-09:10:45 ping www.163.com

```

### 提示

CentOS 可以添加到 /etc/bashrc 这样可以对所有用户起作用

```
echo 'export HISTTIMEFORMAT="%Y-%m-%d-%H:%M:%S "' >> /etc/bashrc

```

##### 3.6.1.2. 设置忽略命令

HISTIGNORE 可以设置那些命令不记入 history 列表。

```
HISTIGNORE="ls:ll:la:cd:exit:clear:logout"
HISTTIMEFORMAT="[%Y-%m-%d - %H:%M:%S] "
HISTFILE=~/.history
HISTSIZE=50000
SAVEHIST=50000

```

#### 3.6.2. .mysql_history

```
ln -s /dev/null .mysql_history

```

插入时间点，在~/.bashrc 中加入下面命令

```
$ tail ~/.bashrc
echo `date` >> ~/.mysql_history

```

```
$ tail ~/.mysql_history
EXPLAIN SELECT * FROM stuff where id=3 \G
EXPLAIN SELECT * FROM stuff where id='3' \G
EXPLAIN SELECT * FROM stuff where id='2' \G
Mon Feb 27 09:15:18 CST 2012
EXPLAIN SELECT * FROM stuff where id='2' and created = '2012-02-01' \G
EXPLAIN SELECT * FROM stuff where id='1' and created = '2012-02-01' \G
EXPLAIN SELECT * FROM stuff where id='3' and created = '2012-02-01' \G
EXPLAIN SELECT * FROM stuff where id='2' and created = '2012-02-01' \G
EXPLAIN SELECT * FROM stuff where id='2' or created = '2012-02-01' \G
EXPLAIN SELECT * FROM stuff where id='2' and created = '2012-02-01' \G
Mon Feb 27 11:48:37 CST 2012

```

### 3.7. hash - hash database access method

hase 命令：用来显示和清除哈希表，执行命令的时候，系统将先查询哈希表。

当你输入命令，首先在 hash 表中寻找，如果不存在，才会利用$PATH 环境变量指定的路径寻找命令，然后加以执行。同时也会将其放入到 hash table 中，当下一次执行同样的命令时就不会再通过$PATH 寻找。以此提高命令的执行效率。

显示哈希表中命令使用频率

```
$ hash
hits	command
   6	/usr/bin/svn
   1	/bin/chown
   3	/bin/bash
   4	/usr/bin/git
  12	/usr/bin/php
   1	/bin/rm
   1	/bin/chmod
   1	/usr/bin/nmap
   5	/bin/cat
  13	/usr/bin/vim
   3	/usr/bin/sudo
   4	/bin/sed
   2	/bin/ps
   2	/usr/bin/man
  23	/bin/ls

```

显示哈希表

```
$ hash -l
builtin hash -p /usr/bin/svn svn
builtin hash -p /bin/chown chown
builtin hash -p /bin/bash bash
builtin hash -p /usr/bin/git git
builtin hash -p /usr/bin/php php
builtin hash -p /bin/rm rm
builtin hash -p /bin/chmod chmod
builtin hash -p /usr/bin/nmap nmap
builtin hash -p /bin/cat cat
builtin hash -p /usr/bin/vim vim
builtin hash -p /usr/bin/sudo sudo
builtin hash -p /bin/sed sed
builtin hash -p /bin/ps ps
builtin hash -p /usr/bin/man man
builtin hash -p /bin/ls ls

```

显示命令的完整路径

```
$ hash -t git
/usr/bin/git

```

向哈希表中增加内容

```

$ hash -p /home/www/deployment/run run

$ run
Usage: /home/www/deployment/run [OPTION] <server-id> <directory/timepoint>

OPTION:
	development <domain> <host>
	testing <domain> <host>
	production <domain> <host>

	branch {development|testing|production} <domain> <host> <branchname>
	revert {development|testing|production} <domain> <host> <revision>
	backup <domain> <host> <directory>
	release <domain> <host> <tags> <message>

	list
	list <domain> <host>

	clean {development|testing|production} <domain> <host>
	log <project> <line>

	conf list
	cron show
	cron setup
	cron edit

```

命令等同于

```
PATH=$PATH:$HOME/www/deployment

export PATH

```

删除哈希表内容

```
$ hash -r

$ hash -l
hash: hash table empty

```

### 3.8. prompt

.bashrc

```
# Prompt definitions
if [ -f ~/.bash_prompt ]; then
        . ~/.bash_prompt
fi		

```

.bash_prompt

```
#!/bin/bash

function tonka2 {
local GRAY="\[\033[1;30m\]"
local LIGHT_GRAY="\[\033[0;37m\]"
local WHITE="\[\033[1;37m\]"

local LIGHT_BLUE="\[\033[1;34m\]"
local LIGHT_RED="\[\033[1;31m\]"
local YELLOW="\[\033[1;33m\]"

case $TERM in
    xterm*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="$TITLEBAR\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\u$LIGHT_BLUE@$YELLOW\h\
$LIGHT_BLUE)-(\
$YELLOW\$PWD\
$LIGHT_BLUE)-$YELLOW-\
$LIGHT_GRAY\n\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\$(date +%F)$LIGHT_BLUE:$YELLOW\$(date +%I:%M:%S)\
$LIGHT_BLUE:$WHITE\$$LIGHT_BLUE)-$YELLOW-$LIGHT_GRAY "

PS2="$LIGHT_BLUE-$YELLOW-$YELLOW-$LIGHT_GRAY "
}

function proml {
local BLUE="\[\033[0;34m\]"
local RED="\[\033[0;31m\]"
local LIGHT_RED="\[\033[1;31m\]"
local WHITE="\[\033[1;37m\]"
local NO_COLOUR="\[\033[0m\]"
case $TERM in
    xterm*|rxvt*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="${TITLEBAR}\
$BLUE[$RED\$(date +%H%M)$BLUE]\
$BLUE[$LIGHT_RED\u@\h:\w$BLUE]\
$WHITE\$$NO_COLOUR "
PS2='> '
PS4='+ '
}

function neo_prompt {
local GRAY="\[\033[1;30m\]"
local LIGHT_GRAY="\[\033[0;37m\]"
local WHITE="\[\033[1;37m\]"

local LIGHT_BLUE="\[\033[1;34m\]"
local LIGHT_RED="\[\033[1;31m\]"
local YELLOW="\[\033[1;33m\]"

case $TERM in
    xterm*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="$TITLEBAR\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\$(date +%F)$LIGHT_BLUE $YELLOW\$(date +%I:%M:%S)\
$LIGHT_BLUE)-(\
$YELLOW\$PWD\
$LIGHT_BLUE)-$YELLOW-\
$LIGHT_GRAY\n\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\u$LIGHT_BLUE@$YELLOW\h\
$LIGHT_BLUE:$WHITE\$$LIGHT_BLUE)-$YELLOW-$LIGHT_GRAY "

PS2="$LIGHT_BLUE-$YELLOW-$YELLOW-$LIGHT_GRAY "
}

# Created by KrON from windowmaker on IRC
# Changed by Spidey 08/06
function elite {
PS1="\[\033[31m\]\332\304\\033[34m\\[\033[31m\]-\\033[34m\\
\[\033[34m\]-:-\[\033[31m\]\$(date +%m)\[\033[34m\033[31m\]/\$(date +%d)\
\[\033[34m\])\[\033[31m\]\304-\[\033[34m]\\371\[\033[31m\]-\371\371\
\[\033[34m\]\372\n\[\033[31m\]\300\304\\033[34m\\
\[\033[31m\]\304\371\[\033[34m\]\372\[\033[00m\]"
PS2="> "
}

```

例 22.1. A "Power User" Prompt

.bash_prompt

```

#!/bin/bash
#----------------------------------------------------------------------
#       POWER USER PROMPT "pprom2"
#----------------------------------------------------------------------
#
#   Created August 98, Last Modified 9 November 98 by Giles
#
#   Problem: when load is going down, it says "1.35down-.08", get rid 
#   of the negative

function prompt_command
{
#   Create TotalMeg variable: sum of visible file sizes in current directory
local TotalBytes=0
for Bytes in $(ls -l | grep "^-" | awk '{print $5}')
do
    let TotalBytes=$TotalBytes+$Bytes
done
TotalMeg=$(echo -e "scale=3 \nx=$TotalBytes/1048576\n if (x<1) {print \"0\"} \n print x \nquit" | bc)

#      This is used to calculate the differential in load values
#      provided by the "uptime" command.  "uptime" gives load 
#      averages at 1, 5, and 15 minute marks.
#
local one=$(uptime | sed -e "s/.*load average: \(.*\...\), \(.*\...\), \(.*\...\)/\1/" -e "s/ //g")
local five=$(uptime | sed -e "s/.*load average: \(.*\...\), \(.*\...\), \(.*\...\).*/\2/" -e "s/ //g")
local diff1_5=$(echo -e "scale = scale ($one) \nx=$one - $five\n if (x>0) {print \"up\"} else {print \"down\"}\n print x \nquit \n" | bc)
loaddiff="$(echo -n "${one}${diff1_5}")"

#   Count visible files:
let files=$(ls -l | grep "^-" | wc -l | tr -d " ")
let hiddenfiles=$(ls -l -d .* | grep "^-" | wc -l | tr -d " ")
let executables=$(ls -l | grep ^-..x | wc -l | tr -d " ")
let directories=$(ls -l | grep "^d" | wc -l | tr -d " ")
let hiddendirectories=$(ls -l -d .* | grep "^d" | wc -l | tr -d " ")-2
let linktemp=$(ls -l | grep "^l" | wc -l | tr -d " ")
if [ "$linktemp" -eq "0" ]
then
    links=""
else
    links=" ${linktemp}l"
fi
unset linktemp
let devicetemp=$(ls -l | grep "^[bc]" | wc -l | tr -d " ")
if [ "$devicetemp" -eq "0" ]
then
    devices=""
else
    devices=" ${devicetemp}bc"
fi
unset devicetemp

}

PROMPT_COMMAND=prompt_command

function pprom2 {

local        BLUE="\[\033[0;34m\]"
local  LIGHT_GRAY="\[\033[0;37m\]"
local LIGHT_GREEN="\[\033[1;32m\]"
local  LIGHT_BLUE="\[\033[1;34m\]"
local  LIGHT_CYAN="\[\033[1;36m\]"
local      YELLOW="\[\033[1;33m\]"
local       WHITE="\[\033[1;37m\]"
local         RED="\[\033[0;31m\]"
local   NO_COLOUR="\[\033[0m\]"

case $TERM in
    xterm*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="$TITLEBAR\
$BLUE[$RED\$(date +%H%M)$BLUE]\
$BLUE[$RED\u@\h$BLUE]\
$BLUE[\
$LIGHT_GRAY\${files}.\${hiddenfiles}-\
$LIGHT_GREEN\${executables}x \
$LIGHT_GRAY(\${TotalMeg}Mb) \
$LIGHT_BLUE\${directories}.\
\${hiddendirectories}d\
$LIGHT_CYAN\${links}\
$YELLOW\${devices}\
$BLUE]\
$BLUE[${WHITE}\${loaddiff}$BLUE]\
$BLUE[\
$WHITE\$(ps ax | wc -l | sed -e \"s: ::g\")proc\
$BLUE]\
\n\
$BLUE[$RED\$PWD$BLUE]\
$WHITE\$\
\
$NO_COLOUR "
PS2='> '
PS4='+ '
}

```

例 22.2. A Prompt the Width of Your Term

```

#!/bin/bash
#   termwide prompt with tty number
#      by Giles - created 2 November 98, last tweaked 31 July 2001
#
#     This is a variant on "termwide" that incorporates the tty number.
#

hostnam=$(hostname -s)
usernam=$(whoami)
temp="$(tty)"
#   Chop off the first five chars of tty (ie /dev/):
cur_tty="${temp:5}"
unset temp

function prompt_command {

#   Find the width of the prompt:
TERMWIDTH=${COLUMNS}

#   Add all the accessories below ...
local temp="--(${usernam}@${hostnam}:${cur_tty})---(${PWD})--"

let fillsize=${TERMWIDTH}-${#temp}
if [ "$fillsize" -gt "0" ]
then
	fill="-------------------------------------------------------------------------------------------------------------------------------------------"
	#   It's theoretically possible someone could need more 
	#   dashes than above, but very unlikely!  HOWTO users, 
	#   the above should be ONE LINE, it may not cut and
	#   paste properly
	fill="${fill:0:${fillsize}}"
	newPWD="${PWD}"
fi

if [ "$fillsize" -lt "0" ]
then
	fill=""
	let cut=3-${fillsize}
	newPWD="...${PWD:${cut}}"
fi
}

PROMPT_COMMAND=prompt_command

function twtty {

local WHITE="\[\033[1;37m\]"
local NO_COLOUR="\[\033[0m\]"

local LIGHT_BLUE="\[\033[1;34m\]"
local YELLOW="\[\033[1;33m\]"

case $TERM in
    xterm*|rxvt*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="$TITLEBAR\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\$usernam$LIGHT_BLUE@$YELLOW\$hostnam$LIGHT_BLUE:$WHITE\$cur_tty\
${LIGHT_BLUE})-${YELLOW}-\${fill}${LIGHT_BLUE}-(\
$YELLOW\${newPWD}\
$LIGHT_BLUE)-$YELLOW-\
\n\
$YELLOW-$LIGHT_BLUE-(\
$YELLOW\$(date +%H%M)$LIGHT_BLUE:$YELLOW\$(date \"+%a,%d %b %y\")\
$LIGHT_BLUE:$WHITE\$$LIGHT_BLUE)-\
$YELLOW-\
$NO_COLOUR " 

PS2="$LIGHT_BLUE-$YELLOW-$YELLOW-$NO_COLOUR "

}

```

例 22.3. The Elegant Useless Clock Prompt

```

#!/bin/bash

#   This prompt requires a VGA font.  The prompt is anchored at the bottom
#   of the terminal, fills the width of the terminal, and draws a line up
#   the right side of the terminal to attach itself to a clock in the upper
#   right corner of the terminal.

function prompt_command {
#   Calculate the width of the prompt:
hostnam=$(echo -n $HOSTNAME | sed -e "s/[\.].*//")
#   "whoami" and "pwd" include a trailing newline
usernam=$(whoami)
newPWD="${PWD}"
#   Add all the accessories below ...
let promptsize=$(echo -n "--(${usernam}@${hostnam})---(${PWD})-----" \
                 | wc -c | tr -d " ")
#   Figure out how much to add between user@host and PWD (or how much to
#   remove from PWD)
let fillsize=${COLUMNS}-${promptsize}
fill=""
#   Make the filler if prompt isn't as wide as the terminal:
while [ "$fillsize" -gt "0" ] 
do 
   fill="${fill}Ä"
   # The A with the umlaut over it (it will appear as a long dash if
   # you're using a VGA font) is \304, but I cut and pasted it in
   # because Bash will only do one substitution - which in this case is
   # putting $fill in the prompt.
   let fillsize=${fillsize}-1
done
#   Right-truncate PWD if the prompt is going to be wider than the terminal:
if [ "$fillsize" -lt "0" ]
then
   let cutt=3-${fillsize}
   newPWD="...$(echo -n $PWD | sed -e "s/\(^.\{$cutt\}\)\(.*\)/\2/")"
fi
#
#   Create the clock and the bar that runs up the right side of the term
#
local LIGHT_BLUE="\033[1;34m"
local     YELLOW="\033[1;33m"
#   Position the cursor to print the clock:
echo -en "\033[2;$((${COLUMNS}-9))H"
echo -en "$LIGHT_BLUE($YELLOW$(date +%H%M)$LIGHT_BLUE)\304$YELLOW\304\304\277"
local i=${LINES}
echo -en "\033[2;${COLUMNS}H"
#   Print vertical dashes down the side of the terminal:
while [ $i -ge 4 ]
do
   echo -en "\033[$(($i-1));${COLUMNS}H\263"
   let i=$i-1
done

let prompt_line=${LINES}-1
#   This is needed because doing \${LINES} inside a Bash mathematical
#   expression (ie. $(())) doesn't seem to work.
}

PROMPT_COMMAND=prompt_command

function clock3 {
local LIGHT_BLUE="\[\033[1;34m\]"
local     YELLOW="\[\033[1;33m\]"
local      WHITE="\[\033[1;37m\]"
local LIGHT_GRAY="\[\033[0;37m\]"
local  NO_COLOUR="\[\033[0m\]"

case $TERM in
    xterm*)
        TITLEBAR='\[\033]0;\u@\h:\w\007\]'
        ;;
    *)
        TITLEBAR=""
        ;;
esac

PS1="$TITLEBAR\
\[\033[\${prompt_line};0H\]
$YELLOW\332$LIGHT_BLUE\304(\
$YELLOW\${usernam}$LIGHT_BLUE@$YELLOW\${hostnam}\
${LIGHT_BLUE})\304${YELLOW}\304\${fill}${LIGHT_BLUE}\304(\
$YELLOW\${newPWD}\
$LIGHT_BLUE)\304$YELLOW\304\304\304\331\
\n\
$YELLOW\300$LIGHT_BLUE\304(\
$YELLOW\$(date \"+%a,%d %b %y\")\
$LIGHT_BLUE:$WHITE\$$LIGHT_BLUE)\304\
$YELLOW\304\
$LIGHT_GRAY " 

PS2="$LIGHT_BLUE\304$YELLOW\304$YELLOW\304$NO_COLOUR "

}			

```

## 4. variable

### 4.1. 系统变量

系统变量,Shell 常用的系统变量并不多，但却十分有用，特别是在做一些参数检测的时候。下面是 Shell 常用的系统变量

```
表示方法	描述
$n	 $1 表示第一个参数，$2 表示第二个参数 ...
$#	 命令行参数的个数
$0	 当前程序的名称
$?	 前一个命令或函数的返回码
$*	 以"参数 1 参数 2 ... " 形式保存所有参数
$@	 以"参数 1" "参数 2" ... 形式保存所有参数
$$	 本程序的(进程 ID 号)PID
$!	 上一个命令的 PID

```

#### 4.1.1. 命令行参数传递

```
[root@cc tmp]# cat test.sh
echo $#
echo $@

[root@cc tmp]# ./test.sh  helloworld
1
helloworld

```

#### 4.1.2. $n $# $0 $?

其中使用得比较多得是 $n $# $0 $? ,看看下面的例子：

```
#!/bin/sh
if [ $# -ne 2 ] ; then
echo "Usage: $0 string file";
exit 1;
fi
grep $1 $2 ;
if [ $? -ne 0 ] ; then
echo "Not Found \"$1\" in $2";
exit 1;
fi
echo "Found \"$1\" in $2";
上面的例子中使用了$0 $1 $2 $# $? 等变量

下面运行的例子：

./chapter2.2.sh usage chapter2.2.sh
Not Found "usage" in chapter2.2.sh
-bash-2.05b$ ./chapter2.2.sh Usage chapter2.2.sh
echo "Usage: $0 string file";
Found "Usage" in chapter2.2.sh

```

#### 4.1.3. $? 程序运行返回值

0 表示正常结束运行， 1 表示异常退出

```

[root@iZ621r6pk9aZ nginx]# ping -W 2 -c 2 www.google.com
PING www.google.com (172.217.24.196) 56(84) bytes of data.
64 bytes from hkg12s13-in-f4.1e100.net (172.217.24.196): icmp_seq=1 ttl=57 time=1.51 ms
64 bytes from hkg12s13-in-f4.1e100.net (172.217.24.196): icmp_seq=2 ttl=57 time=1.44 ms

--- www.google.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 1.447/1.479/1.512/0.050 ms

[root@iZ621r6pk9aZ nginx]# echo $?
0

```

我们 ping 一个不存在的 IP 地址，然后 Ctrl+C 推出程序，返回值是 1.

```

[root@iZ621r6pk9aZ nginx]# ping -W 2 -c 2 172.16.1.100
PING 172.16.1.100 (172.16.1.100) 56(84) bytes of data.
^C
--- 172.16.1.100 ping statistics ---
2 packets transmitted, 0 received, 100% packet loss, time 999ms

[root@iZ621r6pk9aZ nginx]# echo $?
1			

```

如果 redis 用户不存，就创建一个名为 redis 的用户。

```

id redis
if [ $? -eq 1 ] 
then
	adduser -s /bin/false -d /var/lib/redis redis
fi		

```

#### 4.1.4. shift 移位

shift 移位传递过来的参数

```

$ cat test.sh 
echo $@
shift
echo $@

$ ./test.sh aaa bbb ccc ddd
aaa bbb ccc ddd
bbb ccc ddd

```

```

$ cat test.sh 
echo $@
shift
echo $@

shift 2
echo $@
$ ./test.sh aaa bbb ccc ddd eee
aaa bbb ccc ddd eee
bbb ccc ddd eee
ddd eee

```

### 4.2. 表达式

```
!!：再次执行上一条命令

!$：上一条命令的最后一个单词

{a..b}：按照从 a 到 b 顺序的一个数字列表

{a,b,c}：三个词 a,b,c. 可以这样使用 touch /tmp/{a,b,c}

{$1-$9}：执行 shell 脚本时的命令行参数

$0：正在执行的命令名称

$#：当前启动的命令中传入的参数个数

$?：上一条命令的执行返回值。

$$：该 shell 的进程号。

$*：从$1 开始，启动该 shell 脚本的所有参数。

```

```
$ mkdir -p {a..z}
$ ls
a  b  c  d  e  f  g  h  i  j  k  l  m  n  o  p  q  r  s  t  u  v  w  x  y  z

$ mkdir -p {a..z}{0..9}
$ ls
a0  b0  c0  d0  e0  f0  g0  h0  i0  j0  k0  l0  m0  n0  o0  p0  q0  r0  s0  t0  u0  v0  w0  x0  y0  z0
a1  b1  c1  d1  e1  f1  g1  h1  i1  j1  k1  l1  m1  n1  o1  p1  q1  r1  s1  t1  u1  v1  w1  x1  y1  z1
a2  b2  c2  d2  e2  f2  g2  h2  i2  j2  k2  l2  m2  n2  o2  p2  q2  r2  s2  t2  u2  v2  w2  x2  y2  z2
a3  b3  c3  d3  e3  f3  g3  h3  i3  j3  k3  l3  m3  n3  o3  p3  q3  r3  s3  t3  u3  v3  w3  x3  y3  z3
a4  b4  c4  d4  e4  f4  g4  h4  i4  j4  k4  l4  m4  n4  o4  p4  q4  r4  s4  t4  u4  v4  w4  x4  y4  z4
a5  b5  c5  d5  e5  f5  g5  h5  i5  j5  k5  l5  m5  n5  o5  p5  q5  r5  s5  t5  u5  v5  w5  x5  y5  z5
a6  b6  c6  d6  e6  f6  g6  h6  i6  j6  k6  l6  m6  n6  o6  p6  q6  r6  s6  t6  u6  v6  w6  x6  y6  z6
a7  b7  c7  d7  e7  f7  g7  h7  i7  j7  k7  l7  m7  n7  o7  p7  q7  r7  s7  t7  u7  v7  w7  x7  y7  z7
a8  b8  c8  d8  e8  f8  g8  h8  i8  j8  k8  l8  m8  n8  o8  p8  q8  r8  s8  t8  u8  v8  w8  x8  y8  z8
a9  b9  c9  d9  e9  f9  g9  h9  i9  j9  k9  l9  m9  n9  o9  p9  q9  r9  s9  t9  u9  v9  w9  x9  y9  z9

$ touch  {a..z}{0..9}/{a..z}{0..9}
$ ls
a0  b0  c0  d0  e0  f0  g0  h0  i0  j0  k0  l0  m0  n0  o0  p0  q0  r0  s0  t0  u0  v0  w0  x0  y0  z0
a1  b1  c1  d1  e1  f1  g1  h1  i1  j1  k1  l1  m1  n1  o1  p1  q1  r1  s1  t1  u1  v1  w1  x1  y1  z1
a2  b2  c2  d2  e2  f2  g2  h2  i2  j2  k2  l2  m2  n2  o2  p2  q2  r2  s2  t2  u2  v2  w2  x2  y2  z2
a3  b3  c3  d3  e3  f3  g3  h3  i3  j3  k3  l3  m3  n3  o3  p3  q3  r3  s3  t3  u3  v3  w3  x3  y3  z3
a4  b4  c4  d4  e4  f4  g4  h4  i4  j4  k4  l4  m4  n4  o4  p4  q4  r4  s4  t4  u4  v4  w4  x4  y4  z4
a5  b5  c5  d5  e5  f5  g5  h5  i5  j5  k5  l5  m5  n5  o5  p5  q5  r5  s5  t5  u5  v5  w5  x5  y5  z5
a6  b6  c6  d6  e6  f6  g6  h6  i6  j6  k6  l6  m6  n6  o6  p6  q6  r6  s6  t6  u6  v6  w6  x6  y6  z6
a7  b7  c7  d7  e7  f7  g7  h7  i7  j7  k7  l7  m7  n7  o7  p7  q7  r7  s7  t7  u7  v7  w7  x7  y7  z7
a8  b8  c8  d8  e8  f8  g8  h8  i8  j8  k8  l8  m8  n8  o8  p8  q8  r8  s8  t8  u8  v8  w8  x8  y8  z8
a9  b9  c9  d9  e9  f9  g9  h9  i9  j9  k9  l9  m9  n9  o9  p9  q9  r9  s9  t9  u9  v9  w9  x9  y9  z9
$ ls a0
a0  b0  c0  d0  e0  f0  g0  h0  i0  j0  k0  l0  m0  n0  o0  p0  q0  r0  s0  t0  u0  v0  w0  x0  y0  z0
a1  b1  c1  d1  e1  f1  g1  h1  i1  j1  k1  l1  m1  n1  o1  p1  q1  r1  s1  t1  u1  v1  w1  x1  y1  z1
a2  b2  c2  d2  e2  f2  g2  h2  i2  j2  k2  l2  m2  n2  o2  p2  q2  r2  s2  t2  u2  v2  w2  x2  y2  z2
a3  b3  c3  d3  e3  f3  g3  h3  i3  j3  k3  l3  m3  n3  o3  p3  q3  r3  s3  t3  u3  v3  w3  x3  y3  z3
a4  b4  c4  d4  e4  f4  g4  h4  i4  j4  k4  l4  m4  n4  o4  p4  q4  r4  s4  t4  u4  v4  w4  x4  y4  z4
a5  b5  c5  d5  e5  f5  g5  h5  i5  j5  k5  l5  m5  n5  o5  p5  q5  r5  s5  t5  u5  v5  w5  x5  y5  z5
a6  b6  c6  d6  e6  f6  g6  h6  i6  j6  k6  l6  m6  n6  o6  p6  q6  r6  s6  t6  u6  v6  w6  x6  y6  z6
a7  b7  c7  d7  e7  f7  g7  h7  i7  j7  k7  l7  m7  n7  o7  p7  q7  r7  s7  t7  u7  v7  w7  x7  y7  z7
a8  b8  c8  d8  e8  f8  g8  h8  i8  j8  k8  l8  m8  n8  o8  p8  q8  r8  s8  t8  u8  v8  w8  x8  y8  z8
a9  b9  c9  d9  e9  f9  g9  h9  i9  j9  k9  l9  m9  n9  o9  p9  q9  r9  s9  t9  u9  v9  w9  x9  y9  z9

```

### 4.3. Internal Environment Variables

[`tldp.org/LDP/abs/html/internalvariables.html`](http://tldp.org/LDP/abs/html/internalvariables.html)

#### 4.3.1. $RANDOM 随机数

```

neo@MacBook-Pro ~ % echo $RANDOM
15254			

```

$RANDOM 的范围是 0 ~ 32767

```

    for i in {1..10};
    do
        echo -e "$i \t $RANDOM"
    done			

```

#### 4.3.2. 与 history 有关的环境变量

HISTSIZE 将最后多少条历史记录保存到文件中

HISTFILESIZE 定义 ~/.bash_history 的行数

HISTTIMEFORMAT 历史记录格式

```

export HISTSIZE=10000
export HISTFILESIZE=10000
export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
export TIME_STYLE=long-iso			

```

格式如下

```

  903  2019-06-03 00:48:46 docker ps
  904  2019-06-03 00:48:49 docker images
  905  2019-06-03 00:48:53 docker rmi -f $(docker images -q)
  906  2019-06-03 00:48:56 docker stop $(docker ps -a -q)
  907  2019-06-03 00:48:57 docker rm -f $(docker ps -a -q)
  908  2019-06-03 00:48:57 docker rmi -f $(docker images -q)
  909  2019-06-03 00:48:57 docker volume rm $(docker volume ls -q)
  910  2019-06-03 00:49:00 docker

```

### 4.4. set 设置变量

```

$ set -- `echo aa bb cc`
$ echo $1
aa
$ echo $2
bb
$ echo $3
cc

$ set -- aa bb cc

```

### 4.5. unset 变量销毁

```
$ unset logfile

```

### 4.6. 设置变量默认值

如果 CHANNEL_NAME 没有被赋值，那么他的默认值是 "mychannel"

```

CHANNEL_NAME=$1
: ${CHANNEL_NAME:="mychannel"}
echo $CHANNEL_NAME   		

```

如果 logfile 值已经存在侧不会覆盖

```
$ logfile=/var/log/test.log

$ echo $logfile
/var/log/test.log

$ logfile=${logfile:-/tmp/test.log}

$ echo $logfile
/var/log/test.log

```

如果变量为空才能设置

```
$ unset logfile
$ logfile=${logfile:-/tmp/test.log}
$ echo $logfile
/tmp/test.log

```

### 4.7. export 设置全局变量

```
export CATALINA_OUT=/www/logs/tomcat/catalina.out

```

unset 销毁变量

```
unset CATALINA_OUT

```

### 4.8. declare

```
功能说明：声明 shell 变量。

语　　法：declare [+/-][rxi][变量名称＝设置值] 或 declare -f

补充说明：declare 为 shell 指令，在第一种语法中可用来声明变量并设置变量的属性([rix]即为变量的属性），在第二种语法中可用来显示 shell 函数。若不加上任何参数，则会显示全部的 shell 变量与函数(与执行 set 指令的效果相同)。

参　　数：
　+/- 　"-"可用来指定变量的属性，"+"则是取消变量所设的属性。
　-f 　仅显示函数。
　r 　将变量设置为只读。
　x 　指定的变量会成为环境变量，可供 shell 以外的程序来使用。
　i 　[设置值]可以是数值，字符串或运算式。

```

### 4.9. Numerical 数值运算

数值运算表达式

```
$((EXPR))		

```

```
neo@netkiller ~ % echo $((1+1))
neo@netkiller ~ % echo $((5*5))

neo@netkiller ~ % echo $(( (1 + 1) * 2 ))
4

```

```
num=$(awk "BEGIN {print $num1+$num2; exit}")
num=$(python -c "print $num1+$num2")
num=$(perl -e "print $num1+$num2")
num=$(echo $num1 + $num2 | bc) 		

```

巧用 linux 服务器下的/dev/shm, 实现斐波拉切数列

```

[neo@netkiller ~]# cat mblq.sh

TEMP_FILE=/dev/shm/mblq
echo 0 > $TEMP_FILE
echo 1 >> $TEMP_FILE
count=$1
for i in `seq $count`
do
    first=$(tail -2 $TEMP_FILE |head -1)
    two=$(tail -1 $TEMP_FILE)
    echo $((first+two)) >> $TEMP_FILE
done
cat $TEMP_FILE
[neo@netkiller ~]# bash mblq.sh 15
0
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987

```

### 4.10. Strings 字符串操作

```

[neo@netkiller ~]# cat abcde.sh
#!/bin/bash
str="abcde";
for ((m=1;m<=${#str};m++));do
    for ((n=0;n<${#str};n++));do
        [[ ${#str}-$n -lt $m ]] && continue || echo -n ${str:$n:$m}' '
    done
done
[neo@netkiller ~]# bash abcde.sh
a b c d e ab bc cd de abc bcd cde abcd bcde abcde 		

```

#### 4.10.1. ##/#

```

$ MYVAR=foodforthought.jpg
$ echo ${MYVAR##*fo}
rthought.jpg
$ echo ${MYVAR#*fo}
odforthought.jpg

```

一个简单的脚本例子

```

mytar.sh

#!/bin/bash

if [ "${1##*.}" = "tar" ]
then
    echo This appears to be a tarball.
else
    echo At first glance, this does not appear to be a tarball.
fi

$ ./mytar.sh thisfile.tar
This appears to be a tarball.
$ ./mytar.sh thatfile.gz
At first glance, this does not appear to be a tarball.

```

#### 4.10.2. %%/%

```

$ MYFOO="chickensoup.tar.gz"
$ echo ${MYFOO%%.*}
chickensoup
$ echo ${MYFOO%.*}
chickensoup.tar

MYFOOD="chickensoup"
$ echo ${MYFOOD%%soup}
chicken

```

```
$ test="aaa bbb ccc ddd"

$ echo ${test% *}
aaa bbb ccc

$ echo ${test%% *}
aaa

```

#### 4.10.3. :n1:n2

：${varible:n1:n2}:截取变量 varible 从 n1 到 n2 之间的字符串。

```

$ EXCLAIM=cowabunga

$ echo ${EXCLAIM:0:3}
cow

$ echo ${EXCLAIM:3:7}
abunga

file=netkiller.rpm
$echo ${file: -3}

```

#### 4.10.4. #

：${varible:n1:n2}:截取变量 varible 从 n1 到 n2 之间的字符串。

#### 4.10.5. example

```

$cat name.sh
#!/bin/bash
while read line ; do
	fistname=${line% *}
	lastname=${line#* }
	echo $fistname $lastname
done <<EOF
neo chen
jam zheng
EOF

$ bash name.sh
neo chen
jam zheng

```

#### 4.10.6. 计算字符串长度

计算字符串长度

```

echo ${#PATH}

```

```

$ VAR="This string is stored in a variable VAR"
$ echo ${#VAR}
39    		

```

#### 4.10.7. 字符串查找替换

```
# str="1 2 3 4";echo ${str// /}
1234

# str="1 2 3 4";echo ${str// /,}
1,2,3,4

# str="1 2 3 4";echo ${str// /+}
1+2+3+4

# str="neo netkiller";echo ${str//neo/hello}
hello netkiller

```

### 4.11. Array 数组

定义数组

```
arr=(Hello World)

arr[0]=Hello
arr[1]=World

```

访问数组

```
echo ${arr[0]} ${arr[1]}

${arr[*]}         # All of the items in the array
${!arr[*]}        # All of the indexes in the array
${#arr[*]}        # Number of items in the array
${#arr[0]}        # Length of item zero

```

追加操作

```
ARRAY=()
ARRAY+=('foo')
ARRAY+=('bar')

```

#### 4.11.1. for 与 array

```
#!/bin/bash

array=(one two three four [5]=five)

echo "Array size: ${#array[*]}"

echo "Array items:"
for item in ${array[*]}
do
    printf "   %s\n" $item
done

echo "Array indexes:"
for index in ${!array[*]}
do
    printf "   %d\n" $index
done

echo "Array items and indexes:"
for index in ${!array[*]}
do
    printf "%4d: %s\n" $index ${array[$index]}
done

```

```
#!/bin/bash

array=("first item" "second item" "third" "item")

echo "Number of items in original array: ${#array[*]}"
for ix in ${!array[*]}
do
    printf "   %s\n" "${array[$ix]}"
done
echo

arr=(${array[*]})
echo "After unquoted expansion: ${#arr[*]}"
for ix in ${!arr[*]}
do
    printf "   %s\n" "${arr[$ix]}"
done
echo

arr=("${array[*]}")
echo "After * quoted expansion: ${#arr[*]}"
for ix in ${!arr[*]}
do
    printf "   %s\n" "${arr[$ix]}"
done
echo

arr=("${array[@]}")
echo "After @ quoted expansion: ${#arr[*]}"
for ix in ${!arr[*]}
do
    printf "   %s\n" "${arr[$ix]}"
done

```

```
array=({23..32} {49,50} {81..92})

echo "Array size: ${#array[*]}"

echo "Array items:"
for item in ${array[*]}
do
    printf "   %s\n" $item
done

```

#### 4.11.2. while 与 array

while 与 array

```

declare -a array=('1:one' '2:two' '3:three');
len=${#array[@]}
i=0
while [ $i -lt $len ]; do
    echo "${array[$i]}"
    let i++
done

```

#### 4.11.3. array 与 read

array 与 read

```

declare -a array=('1:one' '2:two' '3:three');

while read -e item ; do
    echo "$item \n"
done <<< ${array[@]}	

mapfile CONFIG <<END
192.168.0.1 80
192.168.0.1 8080
192.168.0.2 8000
192.168.0.2 80
192.168.0.1 88
END

printf %s "${CONFIG[@]}"

for line in "${CONFIG[@]}"
do
	read ipaddr port <<< $(echo ${line})
	echo "$ipaddr : $port"
done

```

#### 4.11.4. 拆分字符串并转换为数组

##### Split string into an array in Bash

字符串

```
QUEUES="example|sss"			

```

类似列表的数据结构

```
for caption in $(echo $QUEUES | tr '|' ' '); do 
        echo $caption
done			

```

拆分为数组形式

```
captions=($(echo $QUEUES | tr '|' ' '))

for element in "${captions[@]}"
do
    echo "$element"
done

for key in ${!captions[@]}; do
    echo ${key} ${captions[${key}]}
done

```

#### 4.11.5. 数组转为字符串

```
ids=(1 2 3 4);echo ${ids[*]// /|}
ids=(1 2 3 4); lst=$( IFS='|'; echo "${ids[*]}" ); echo $lst

array=(1 2 3 4); echo ${array[*]// /|}
array=(1 2 3 4);string="${ids[@]}";echo ${string// /|}
array=(1 2 3 4);string="${ids[@]}";echo ${string// /,}

IFS='|';echo "${[*]// /|}";

```

### 4.12. read 赋值多个变量

```

[net@netkiller tmp]# cat test.sh 
read ipaddr port <<< $(echo www.netkiller.cn 80)

echo $ipaddr
echo $port

[net@netkiller tmp]# bash test.sh 
www.netkiller.cn
80

```

### 4.13. eval

```
$ i=5
$ a_5=250
$ eval echo $"a_$i"

```

```
# neo="Neo Chen"
# name=neo
# eval "echo \$$name"

Neo Chen

```

### 4.14. typeset

有两个选项 -l 代表小写 -u 代表大写。

```

typeset -u name
name='neo'
echo $name

typeset -l nickname
nickname='netkiller'
echo $nickname

typeset -l nickname
nickname='NETKILLER'
echo $nickname

```

操作演示

```

[root@localhost ~]# typeset -u name
[root@localhost ~]# name='neo'
[root@localhost ~]# echo $name
NEO
[root@localhost ~]# 
[root@localhost ~]# typeset -l nickname
[root@localhost ~]# nickname='netkiller'
[root@localhost ~]# echo $nickname
netkiller
[root@localhost ~]# 
[root@localhost ~]# typeset -l nickname
[root@localhost ~]# nickname='NETKILLER'
[root@localhost ~]# echo $nickname
netkiller		

```

## 5. conditions if and case

表 22.1. 文件目录表达式

| Primary | 意义 |
| --- | --- |
| [ -a FILE ] | 如果 FILE 存在则为真。 |
| [ -b FILE ] | 如果 FILE 存在且是一个块特殊文件则为真。 |
| [ -c FILE ] | 如果 FILE 存在且是一个字特殊文件则为真。 |
| [ -d FILE ] | 如果 FILE 存在且是一个目录则为真。 |
| [ -e FILE ] | 如果 FILE 存在则为真。 |
| [ -f FILE ] | 如果 FILE 存在且是一个普通文件则为真。 |
| [ -g FILE ] | 如果 FILE 存在且已经设置了 SGID 则为真。 |
| [ -h FILE ] | 如果 FILE 存在且是一个符号连接则为真。 |
| [ -k FILE ] | 如果 FILE 存在且已经设置了粘制位则为真。 |
| [ -p FILE ] | 如果 FILE 存在且是一个名字管道(F 如果 O)则为真。 |
| [ -r FILE ] | 如果 FILE 存在且是可读的则为真。 |
| [ -s FILE ] | 如果 FILE 存在且大小不为 0 则为真。 |
| [ -t FD ] | 如果文件描述符 FD 打开且指向一个终端则为真。 |
| [ -u FILE ] | 如果 FILE 存在且设置了 SUID (set user ID)则为真。 |
| [ -w FILE ] | 如果 FILE 如果 FILE 存在且是可写的则为真。 |
| [ -x FILE ] | 如果 FILE 存在且是可执行的则为真。 |
| [ -O FILE ] | 如果 FILE 存在且属有效用户 ID 则为真。 |
| [ -G FILE ] | 如果 FILE 存在且属有效用户组则为真。 |
| [ -L FILE ] | 如果 FILE 存在且是一个符号连接则为真。 |
| [ -N FILE ] | 如果 FILE 存在 and has been mod 如果 ied since it was last read 则为真。 |
| [ -S FILE ] | 如果 FILE 存在且是一个套 |
| [ FILE1 -nt FILE2 ] | 如果 FILE1 has been changed more recently than FILE2, or 如果 FILE1 exists and FILE2 does not 则为真。 |
| [ FILE1 -ot FILE2 ] | 如果 FILE1 比 FILE2 要老, 或者 FILE2 存在且 FILE1 不存在则为真。 |
| [ FILE1 -ef FILE2 ] | 如果 FILE1 和 FILE2 指向相同的设备和节点号则为真。 |

表 22.2. 字符串表达式

| Primary | 意义 |
| --- | --- |
| [ -o OPTIONNAME ] | 如果 shell 选项 “OPTIONNAME” 开启则为真。 |
| [ -z STRING ] | “STRING” 的长度为零则为真。 |
| [ -n STRING ] or [ STRING ] | “STRING” 的长度为非零 non-zero 则为真。 |
| [ STRING1 == STRING2 ] | 如果 2 个字符串相同则为真。 |
| [ STRING1 != STRING2 ] | 如果字符串不相等则为真。 |
| [ STRING1 < STRING2 ] | 如果 “STRING1” sorts before “STRING2” lexicographically in the current locale 则为真。 |
| [ STRING1 > STRING2 ] | 如果 “STRING1” sorts after “STRING2” lexicographically in the current locale 则为真。 |
| [ ARG1 OP ARG2 ] | “OP” 为 -eq, -ne, -lt, -le, -gt or -ge. |

Arithmetic relational operators

1.  -lt (<)

2.  -gt (>)

3.  -le (<=)

4.  -ge (>=)

5.  -eq (==)

6.  -ne (!=)

表 22.3. 组合表达式

| 操作 | 效果 |
| --- | --- |
| [ ! EXPR ] | 如果 EXPR 是 false 则为真。 |
| [ ( EXPR ) ] | 返回 EXPR 的值。这样可以用来忽略正常的操作符优先级。 |
| [ EXPR1 -a EXPR2 ] | 如果 EXPR1 and EXPR2 全真则为真。 |
| [ EXPR1 -o EXPR2 ] | 如果 EXPR1 或者 EXPR2 为真则为真。 |

### 5.1. if

```

if [ ! -d /directory/to/check ]; then
    mkdir -p /directory/toc/check
fi		

```

例 22.4. Basic conditional example if .. then

```

#!/bin/bash
if [ "foo" = "foo" ]; then
   echo expression evaluated as true
fi

```

例 22.5. Conditionals with variables

```

#!/bin/bash
T1="foo"
T2="bar"
if [ "$T1" = "$T2" ]; then
    echo expression evaluated as true
else
    echo expression evaluated as false
fi

```

```

(( $# != 1 )) && bool=0 || bool=${1}
[[ -f /tmp/test ]] && echo "True" || echo "False"

```

### 5.2. case

case 高级用法, 匹配 Yes,YES,YeS,yES,yEs ...

```

read -p "Do you want to continue [Y/n]?" BOOLEAN

case $BOOLEAN in
    [yY][eE][sS])
        echo 'Thanks' $BOOLEAN
        ;;
    [yY]|[nN])
        echo 'Thanks' $BOOLEAN
        ;;
    'T'|'F')
        echo 'Thanks' $BOOLEAN
        ;;
    [Tt]ure|[Ff]alse)
        echo 'Thanks' $BOOLEAN
        ;;
    *)
        exit 1
        ;;
esac

```

例 22.6. case

```
case "$1" in
        start)
            start
            ;;
        stop)
            stop
            ;;
        status)
            status
            ;;
        restart)
            stop
            start
            ;;
        condrestart)
            condrestart
            ;;

        *)
            echo $"Usage: $0 {start|stop|restart|condrestart|status}"
            exit 1
esac

```

## 6. Loops for, while and until

### 6.1. for

```

#!/bin/bash
for i in 1 2 3 4 5
do
   echo "Welcome $i times"
done

for i in $( ls ); do
    echo item: $i
done

for i in `seq 1 10`;
do
    echo $i
done

for i in {1..5}
do
   echo "Welcome $i times"
done

for (( c=1; c<=5; c++ ))
do
	echo "Welcome $c times..."
done

for ((i=1; $i<=9;i++)); do echo $i; done

```

```
for i in {0..10..2}
do
    echo "Welcome $i times"
done

for i in $(seq 1 2 20)
do
   echo "Welcome $i times"
done

```

单行实例

```
for ip in {1..10};do echo $ip; done

for i in `seq 1 10`;do echo $i;done

for ip in {81..92}; do ssh root@172.16.3.$ip date; done

for n in  {23..32} {49,50} {81..92}; do mkdir /tmp/$n; echo $n; done

```

```

for keyword in bash cmd ls
do
	echo $keyword
done 

string="aaa bbb ccc ddd" && for word in $string; do echo "$word"; done

files=( "/etc/passwd" "/etc/group" "/etc/hosts" )
for file in "${files[@]}"
do
    echo $file
done

```

infinite loops

```

#!/bin/bash
for (( ; ; ))
do
   echo "infinite loops [ hit CTRL+C to stop]"
done

```

find file

```
#!/bin/bash
for file in /etc/*
do
	if [ "${file}" == "/etc/resolv.conf" ]
	then
		countNameservers=$(grep -c nameserver /etc/resolv.conf)
		echo "Total  ${countNameservers} nameservers defined in ${file}"
		break
	fi
done

```

backup file

```
#!/bin/bash
FILES="$@"
for f in $FILES
do
        # if .bak backup file exists, read next file
	if [ -f ${f}.bak ]
	then
		echo "Skiping $f file..."
		continue  # read next file and skip cp command
	fi
        # we are hear means no backup file exists, just use cp command to copy file
	/bin/cp $f $f.bak
done

```

```
for n in  {23..32} {49,50} {81..92}; do mkdir /tmp/$n; echo $n; done

```

### 6.2. while

```

#!/bin/bash
COUNTER=0
while [  $COUNTER -lt 10 ]; do
    echo The counter is $COUNTER
    let COUNTER=COUNTER+1
done

```

```

while read name age
do
	echo $name $age
done << EOF
neo 30
jam 31
john 29
EOF

while read name age
do
	[[ $age -gt 30 ]] && echo $name
done << EOF
neo 30
jam 31
john 29
EOF

```

```

$ cat mount.sh
#!/bin/sh
while read LINE
do

	s=`echo $LINE|awk '{print $1}'`
	d=`echo $LINE|awk '{print $2}'`

	umount -f $d
	mount -t nfs4 $s $d

done < mount.conf

$ cat mount.conf
172.16.0.1:/	/www/logs/1
172.16.0.2:/	/www/logs/2
172.16.0.3:/	/www/logs/3
172.16.0.4:/	/www/logs/4
172.16.0.5:/	/www/logs/5

```

读一行

```

while IFS='' read -r line || [[ -n "$line" ]]; do
	echo "Text read from file: $line"
done < "$1"

```

### 6.3. until

```

#!/bin/bash
COUNTER=20
until [  $COUNTER -lt 10 ]; do
    echo COUNTER $COUNTER
    let COUNTER-=1
done

```

## 7. Functions

例 22.7. Functions with parameters sample

```
#!/bin/bash
function quit {
   exit
}
function e {
    echo $1
}
e Hello
e World
quit
echo foo

```

### 7.1. Local variables

```
 #!/bin/bash
	HELLO=Hello
	function hello {
	        local HELLO=World
	        echo $HELLO
	}
	echo $HELLO
	hello
	echo $HELLO

```

## 8. User interfaces

例 22.8. Using select to make simple menus

```
#!/bin/bash
OPTIONS="Hello Quit"
select opt in $OPTIONS; do
    if [ "$opt" = "Quit" ]; then
     echo done
     exit
    elif [ "$opt" = "Hello" ]; then
     echo Hello World
    else
     clear
     echo bad option
    fi
done

```

例 22.9. Using the command line

```
#!/bin/bash
if [ -z "$1" ]; then
    echo usage: $0 directory
    exit
fi
SRCD=$1
TGTD="/var/backups/"
OF=home-$(date +%Y%m%d).tgz
tar -cZf $TGTD$OF $SRCD

```

例 22.10. Reading user input with read

In many ocations you may want to prompt the user for some input, and there are several ways to achive this. This is one of those ways:

```
#!/bin/bash
echo Please, enter your name
read NAME
echo "Hi $NAME!"

```

As a variant, you can get multiple values with read, this example may clarify this.

```
#!/bin/bash
echo Please, enter your firstname and lastname
read FN LN
echo "Hi! $LN, $FN !"

```

### 8.1. input

例 22.11. read

限时 30 秒内，输入你的名字

```
$ read -p "Please input your name: " -t 30 named
Please input your name: neo

$ echo $named

```

```

READ_TIMEOUT=60
read -t "$READ_TIMEOUT" input

# if you do not want quotes, then escape it
input=$(sed "s/[;\`\"\$\' ]//g" <<< $input)

# For reading number, then you can escape other characters
input=$(sed 's/[⁰-9]*//g' <<< $input)

```

## 9. subshell

echo $$ $BASHPID ; ( echo $$ $BASHPID )

su 运行 shell 并获取 PID 并存入文件

```

su - $USER -c "$PROG & echo \$! > $PIDFILE"

```

## 10. Example

### 10.1. 有趣的 Shell

运行后会不停的 fork 新的进程，直到你的资源消耗尽。

```

:() { :|:& }; :

.() { .|.& }; .

```

### 10.2. backup

```
#!/bin/sh
umount /mnt/backup
mount /dev/sdb1 /mnt/backup

if [ `date +%d` = '01' ] #每月 1 号进行完全备份
then
	bakdir="/mnt/bak/daybak/month/"`date +%m%d`
	zl="" #进行完全备份
else
	backup_dir="/mnt/backup/"`date +%d`
	zl="-N "`date +'%Y-%m-01 00:00:01'`; #差异备份
	#zl="-N "`date -d '-1 day' +'%Y-%m-%d 00:00:01'` #日增量备份
fi

tar "${zl}" -czf ${backup_dir}/www.tgz /var/www
umount /mnt/backup

```

### 10.3. CPU 核心数

```
cat /proc/cpuinfo | grep processor | wc -l

```

### 10.4. Password

例 22.12. random password

```
cat /dev/urandom | head -1 | md5sum | head -c 8

od -N 4 -t x4 /dev/random | head -1 | awk '{print $2}'

```

### 10.5. processes

#### 10.5.1. pid

```
neo@debian:~/html/temp$ pidof lighttpd
2775

neo@debian:~/html/temp$ pgrep lighttpd
2775

neo@debian:~/html/temp$ pid=`pidof lighttpd`
neo@debian:~/html/temp$ echo $pid
2775

```

```
# user=`whoami`
# pgrep -u $user -f cassandra | xargs kill -9

```

#### 10.5.2. kill

kill 占用 7800 端口的进程

```
kill -9 `netstat -nlp | grep '192.168.0.5:7800' | awk -F ' ' '{print $7}' | awk -F '/' '{print $1}'`

```

#### 10.5.3. pgrep

```
#!/bin/bash
ntpdate 172.16.10.10

pid=$(pgrep rsync)

if [ -z "$pid" ]; then

rsync -auzP --delete -e ssh  --exclude=example/images --exclude=project/product --exclude=project/templates/caches  root@172.16.10.10:/www/project /www

fi

```

### 10.6. Shell 技巧

#### 10.6.1. 行转列，再批评

```
echo "abc def gfh ijk"| sed "s:\ :\n:g" |grep -w gfh

```

#### 10.6.2. for vs while

```
echo "aaa bbb ccc" > test.txt
echo "ddd eee fff" >> test.txt

```

```
for line in $(cat test.txt)
do
	echo $line
done

```

```
cat test.txt| while read line
do
	echo $line
done

```

#### 10.6.3. 遍历字符串

```
# find . -name "*.html" -o -name "*.php" -o -name '*.dwt' -printf "[%p] " -exec grep -c 'head' {} \; | grep -v "0$" |more

```

### 10.7. to convert utf-8 from gb2312 code

```

perl   -  MEncode   -  pi   -  e   '  $_=encode_utf8(decode(gb2312=>$_))  '   filename
for f in `find .`; do [ -f $f ] && perl -MEncode -pi -e '$_=encode_utf8(decode(gb2312=>$_))' $f; done;

```

### 10.8. 使用内存的百分比

```
$ free | sed -n 2p | awk '{print "used="$3/$2*100"%","free="$4/$2*100"%"}'
used=53.9682% free=46.0318%		

```

### 10.9. 合并 apache 被 cronlog 分割的 log 文件

```
$ find 2009 -type f -name access.log -exec cat {} >> access.log \;

```

### 10.10. Linux 交集 差集 并集

```

测试文件如下:

[root@test23 ~]# cat a.txt
1.1.1.1
2.2.2.2
3.3.3.3
1.2.3.4
[root@test23 ~]# cat b.txt
4.4.4.4
1.2.3.4
2.2.2.2
a.b.c.d
```
#### grep 
```
1) 差集
// 使用 grep -v 和 -f 参数方式 是最容易想到的

[root@test23 ~]# grep -v -f  a.txt  b.txt
4.4.4.4
a.b.c.d
[root@test23 ~]# grep -v -f   b.txt  a.txt
1.1.1.1
3.3.3.3
```
#### uniq 
```
1) 差集
// -u 表示的是输出出现次数为 1 的内容
[root@test23 ~]# sort a.txt  b.txt  | uniq -u
1.1.1.1
3.3.3.3
4.4.4.4
a.b.c.d

2) 并集
[root@test23 ~]# sort a.txt  b.txt  | uniq
1.1.1.1
1.2.3.4
2.2.2.2
3.3.3.3
4.4.4.4
a.b.c.d

3) 交集
// -d 表示的是输出出现次数大于 1 的内容
[root@test23 ~]# sort a.txt b.txt | uniq -d
1.2.3.4
2.2.2.2

```

## 第 23 章 Z Shell

[`www.zsh.org/`](http://www.zsh.org/)

## 1. installing Z shell

```
$ sudo apt install zsh

```

## 2. Oh My ZSH!

http://ohmyz.sh/

Oh My ZSH 是 z shell 命令主题

```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

```

## 3. Starting file

### 3.1. ~/.zshrc

```
neo@netkiller:~$ cat .zshrc
# Created by newuser for 4.3.9
PROMPT='%n@%M:%~$ '

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    eval "`dircolors -b`"
    alias ls='ls --color=auto'
    alias dir='dir --color=auto'
    alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -l'
alias la='ls -A'
alias l='ls -CF'

# Home/End/Del key
bindkey '\e[1~' beginning-of-line
bindkey '\e[4~' end-of-line
bindkey "\e[3~" delete-char

```

## 4. Prompting

```
$ PROMPT='%n@%M:%~$ '
neo@netkiller:~$

```

```
autoload colors; colors
export PS1="%B[%{$fg[red]%}%n%{$reset_color%}%b@%B%{$fg[cyan]%}%m%b%{$reset_color%}:%~%B]%b "

```

```
[neo@netkiller:~/.oh-my-zsh/themes] 		

```

## 5. Aliases

```
# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    eval "`dircolors -b`"
    alias ls='ls --color=auto'
    alias dir='dir --color=auto'
    alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# some more ls aliases
alias ll='ls -l'
alias la='ls -A'
alias l='ls -CF'

```

## 6. History

```
$ !$

```

```
$ history
   18  cd workspace/Document
   19  ls
   20  ls

$ !20
ls
Docbook  makedoc  Tex

```

## 7. FAQ

### 7.1. Home/End key

```
bindkey '\e[1~' beginning-of-line
bindkey '\e[4~' end-of-line

```

## 第 24 章 Berkeley UNIX C shell (csh)

$ sudo apt install csh

## 第 25 章 KornShell

$ sudo apt install ksh

## 第 26 章 Shell command

## 1. Help Commands

### 1.1. man - an interface to the on-line reference manuals

#### 1.1.1. manpath.config

```
cat /etc/manpath.config

```

#### 1.1.2. 查看 man 手册位置

```
$ man -aw ls
/usr/share/man/man1/ls.1.gz

```

#### 1.1.3. 指定手册位置

```
man -M /home/mysql/man mysql

```

## 2. getconf - Query system configuration variables

```

$ getconf LONG_BIT
32
$ getconf WORD_BIT
32

```

```

LINK_MAX                           65000
_POSIX_LINK_MAX                    65000
MAX_CANON                          255
_POSIX_MAX_CANON                   255
MAX_INPUT                          255
_POSIX_MAX_INPUT                   255
NAME_MAX                           255
_POSIX_NAME_MAX                    255
PATH_MAX                           4096
_POSIX_PATH_MAX                    4096
PIPE_BUF                           4096
_POSIX_PIPE_BUF                    4096
SOCK_MAXBUF                        
_POSIX_ASYNC_IO                    
_POSIX_CHOWN_RESTRICTED            1
_POSIX_NO_TRUNC                    1
_POSIX_PRIO_IO                     
_POSIX_SYNC_IO                     
_POSIX_VDISABLE                    0
ARG_MAX                            2097152
ATEXIT_MAX                         2147483647
CHAR_BIT                           8
CHAR_MAX                           127
CHAR_MIN                           -128
CHILD_MAX                          63918
CLK_TCK                            100
INT_MAX                            2147483647
INT_MIN                            -2147483648
IOV_MAX                            1024
LOGNAME_MAX                        256
LONG_BIT                           64
MB_LEN_MAX                         16
NGROUPS_MAX                        65536
NL_ARGMAX                          4096
NL_LANGMAX                         2048
NL_MSGMAX                          2147483647
NL_NMAX                            2147483647
NL_SETMAX                          2147483647
NL_TEXTMAX                         2147483647
NSS_BUFLEN_GROUP                   1024
NSS_BUFLEN_PASSWD                  1024
NZERO                              20
OPEN_MAX                           1024
PAGESIZE                           4096
PAGE_SIZE                          4096
PASS_MAX                           8192
PTHREAD_DESTRUCTOR_ITERATIONS      4
PTHREAD_KEYS_MAX                   1024
PTHREAD_STACK_MIN                  16384
PTHREAD_THREADS_MAX                
SCHAR_MAX                          127
SCHAR_MIN                          -128
SHRT_MAX                           32767
SHRT_MIN                           -32768
SSIZE_MAX                          32767
TTY_NAME_MAX                       32
TZNAME_MAX                         
UCHAR_MAX                          255
UINT_MAX                           4294967295
UIO_MAXIOV                         1024
ULONG_MAX                          18446744073709551615
USHRT_MAX                          65535
WORD_BIT                           32
_AVPHYS_PAGES                      972844
_NPROCESSORS_CONF                  8
_NPROCESSORS_ONLN                  8
_PHYS_PAGES                        4106156
_POSIX_ARG_MAX                     2097152
_POSIX_ASYNCHRONOUS_IO             200809
_POSIX_CHILD_MAX                   63918
_POSIX_FSYNC                       200809
_POSIX_JOB_CONTROL                 1
_POSIX_MAPPED_FILES                200809
_POSIX_MEMLOCK                     200809
_POSIX_MEMLOCK_RANGE               200809
_POSIX_MEMORY_PROTECTION           200809
_POSIX_MESSAGE_PASSING             200809
_POSIX_NGROUPS_MAX                 65536
_POSIX_OPEN_MAX                    1024
_POSIX_PII                         
_POSIX_PII_INTERNET                
_POSIX_PII_INTERNET_DGRAM          
_POSIX_PII_INTERNET_STREAM         
_POSIX_PII_OSI                     
_POSIX_PII_OSI_CLTS                
_POSIX_PII_OSI_COTS                
_POSIX_PII_OSI_M                   
_POSIX_PII_SOCKET                  
_POSIX_PII_XTI                     
_POSIX_POLL                        
_POSIX_PRIORITIZED_IO              200809
_POSIX_PRIORITY_SCHEDULING         200809
_POSIX_REALTIME_SIGNALS            200809
_POSIX_SAVED_IDS                   1
_POSIX_SELECT                      
_POSIX_SEMAPHORES                  200809
_POSIX_SHARED_MEMORY_OBJECTS       200809
_POSIX_SSIZE_MAX                   32767
_POSIX_STREAM_MAX                  16
_POSIX_SYNCHRONIZED_IO             200809
_POSIX_THREADS                     200809
_POSIX_THREAD_ATTR_STACKADDR       200809
_POSIX_THREAD_ATTR_STACKSIZE       200809
_POSIX_THREAD_PRIORITY_SCHEDULING  200809
_POSIX_THREAD_PRIO_INHERIT         200809
_POSIX_THREAD_PRIO_PROTECT         200809
_POSIX_THREAD_ROBUST_PRIO_INHERIT  
_POSIX_THREAD_ROBUST_PRIO_PROTECT  
_POSIX_THREAD_PROCESS_SHARED       200809
_POSIX_THREAD_SAFE_FUNCTIONS       200809
_POSIX_TIMERS                      200809
TIMER_MAX                          
_POSIX_TZNAME_MAX                  
_POSIX_VERSION                     200809
_T_IOV_MAX                         
_XOPEN_CRYPT                       
_XOPEN_ENH_I18N                    1
_XOPEN_LEGACY                      1
_XOPEN_REALTIME                    1
_XOPEN_REALTIME_THREADS            1
_XOPEN_SHM                         1
_XOPEN_UNIX                        1
_XOPEN_VERSION                     700
_XOPEN_XCU_VERSION                 4
_XOPEN_XPG2                        1
_XOPEN_XPG3                        1
_XOPEN_XPG4                        1
BC_BASE_MAX                        99
BC_DIM_MAX                         2048
BC_SCALE_MAX                       99
BC_STRING_MAX                      1000
CHARCLASS_NAME_MAX                 2048
COLL_WEIGHTS_MAX                   255
EQUIV_CLASS_MAX                    
EXPR_NEST_MAX                      32
LINE_MAX                           2048
POSIX2_BC_BASE_MAX                 99
POSIX2_BC_DIM_MAX                  2048
POSIX2_BC_SCALE_MAX                99
POSIX2_BC_STRING_MAX               1000
POSIX2_CHAR_TERM                   200809
POSIX2_COLL_WEIGHTS_MAX            255
POSIX2_C_BIND                      200809
POSIX2_C_DEV                       200809
POSIX2_C_VERSION                   200809
POSIX2_EXPR_NEST_MAX               32
POSIX2_FORT_DEV                    
POSIX2_FORT_RUN                    
_POSIX2_LINE_MAX                   2048
POSIX2_LINE_MAX                    2048
POSIX2_LOCALEDEF                   200809
POSIX2_RE_DUP_MAX                  32767
POSIX2_SW_DEV                      200809
POSIX2_UPE                         
POSIX2_VERSION                     200809
RE_DUP_MAX                         32767
PATH                               /bin:/usr/bin
CS_PATH                            /bin:/usr/bin
LFS_CFLAGS                         
LFS_LDFLAGS                        
LFS_LIBS                           
LFS_LINTFLAGS                      
LFS64_CFLAGS                       -D_LARGEFILE64_SOURCE
LFS64_LDFLAGS                      
LFS64_LIBS                         
LFS64_LINTFLAGS                    -D_LARGEFILE64_SOURCE
_XBS5_WIDTH_RESTRICTED_ENVS        XBS5_LP64_OFF64
XBS5_WIDTH_RESTRICTED_ENVS         XBS5_LP64_OFF64
_XBS5_ILP32_OFF32                  
XBS5_ILP32_OFF32_CFLAGS            
XBS5_ILP32_OFF32_LDFLAGS           
XBS5_ILP32_OFF32_LIBS              
XBS5_ILP32_OFF32_LINTFLAGS         
_XBS5_ILP32_OFFBIG                 
XBS5_ILP32_OFFBIG_CFLAGS           
XBS5_ILP32_OFFBIG_LDFLAGS          
XBS5_ILP32_OFFBIG_LIBS             
XBS5_ILP32_OFFBIG_LINTFLAGS        
_XBS5_LP64_OFF64                   1
XBS5_LP64_OFF64_CFLAGS             -m64
XBS5_LP64_OFF64_LDFLAGS            -m64
XBS5_LP64_OFF64_LIBS               
XBS5_LP64_OFF64_LINTFLAGS          
_XBS5_LPBIG_OFFBIG                 
XBS5_LPBIG_OFFBIG_CFLAGS           
XBS5_LPBIG_OFFBIG_LDFLAGS          
XBS5_LPBIG_OFFBIG_LIBS             
XBS5_LPBIG_OFFBIG_LINTFLAGS        
_POSIX_V6_ILP32_OFF32              
POSIX_V6_ILP32_OFF32_CFLAGS        
POSIX_V6_ILP32_OFF32_LDFLAGS       
POSIX_V6_ILP32_OFF32_LIBS          
POSIX_V6_ILP32_OFF32_LINTFLAGS     
_POSIX_V6_WIDTH_RESTRICTED_ENVS    POSIX_V6_LP64_OFF64
POSIX_V6_WIDTH_RESTRICTED_ENVS     POSIX_V6_LP64_OFF64
_POSIX_V6_ILP32_OFFBIG             
POSIX_V6_ILP32_OFFBIG_CFLAGS       
POSIX_V6_ILP32_OFFBIG_LDFLAGS      
POSIX_V6_ILP32_OFFBIG_LIBS         
POSIX_V6_ILP32_OFFBIG_LINTFLAGS    
_POSIX_V6_LP64_OFF64               1
POSIX_V6_LP64_OFF64_CFLAGS         -m64
POSIX_V6_LP64_OFF64_LDFLAGS        -m64
POSIX_V6_LP64_OFF64_LIBS           
POSIX_V6_LP64_OFF64_LINTFLAGS      
_POSIX_V6_LPBIG_OFFBIG             
POSIX_V6_LPBIG_OFFBIG_CFLAGS       
POSIX_V6_LPBIG_OFFBIG_LDFLAGS      
POSIX_V6_LPBIG_OFFBIG_LIBS         
POSIX_V6_LPBIG_OFFBIG_LINTFLAGS    
_POSIX_V7_ILP32_OFF32              
POSIX_V7_ILP32_OFF32_CFLAGS        
POSIX_V7_ILP32_OFF32_LDFLAGS       
POSIX_V7_ILP32_OFF32_LIBS          
POSIX_V7_ILP32_OFF32_LINTFLAGS     
_POSIX_V7_WIDTH_RESTRICTED_ENVS    POSIX_V7_LP64_OFF64
POSIX_V7_WIDTH_RESTRICTED_ENVS     POSIX_V7_LP64_OFF64
_POSIX_V7_ILP32_OFFBIG             
POSIX_V7_ILP32_OFFBIG_CFLAGS       
POSIX_V7_ILP32_OFFBIG_LDFLAGS      
POSIX_V7_ILP32_OFFBIG_LIBS         
POSIX_V7_ILP32_OFFBIG_LINTFLAGS    
_POSIX_V7_LP64_OFF64               1
POSIX_V7_LP64_OFF64_CFLAGS         -m64
POSIX_V7_LP64_OFF64_LDFLAGS        -m64
POSIX_V7_LP64_OFF64_LIBS           
POSIX_V7_LP64_OFF64_LINTFLAGS      
_POSIX_V7_LPBIG_OFFBIG             
POSIX_V7_LPBIG_OFFBIG_CFLAGS       
POSIX_V7_LPBIG_OFFBIG_LDFLAGS      
POSIX_V7_LPBIG_OFFBIG_LIBS         
POSIX_V7_LPBIG_OFFBIG_LINTFLAGS    
_POSIX_ADVISORY_INFO               200809
_POSIX_BARRIERS                    200809
_POSIX_BASE                        
_POSIX_C_LANG_SUPPORT              
_POSIX_C_LANG_SUPPORT_R            
_POSIX_CLOCK_SELECTION             200809
_POSIX_CPUTIME                     200809
_POSIX_THREAD_CPUTIME              200809
_POSIX_DEVICE_SPECIFIC             
_POSIX_DEVICE_SPECIFIC_R           
_POSIX_FD_MGMT                     
_POSIX_FIFO                        
_POSIX_PIPE                        
_POSIX_FILE_ATTRIBUTES             
_POSIX_FILE_LOCKING                
_POSIX_FILE_SYSTEM                 
_POSIX_MONOTONIC_CLOCK             200809
_POSIX_MULTI_PROCESS               
_POSIX_SINGLE_PROCESS              
_POSIX_NETWORKING                  
_POSIX_READER_WRITER_LOCKS         200809
_POSIX_SPIN_LOCKS                  200809
_POSIX_REGEXP                      1
_REGEX_VERSION                     
_POSIX_SHELL                       1
_POSIX_SIGNALS                     
_POSIX_SPAWN                       200809
_POSIX_SPORADIC_SERVER             
_POSIX_THREAD_SPORADIC_SERVER      
_POSIX_SYSTEM_DATABASE             
_POSIX_SYSTEM_DATABASE_R           
_POSIX_TIMEOUTS                    200809
_POSIX_TYPED_MEMORY_OBJECTS        
_POSIX_USER_GROUPS                 
_POSIX_USER_GROUPS_R               
POSIX2_PBS                         
POSIX2_PBS_ACCOUNTING              
POSIX2_PBS_LOCATE                  
POSIX2_PBS_TRACK                   
POSIX2_PBS_MESSAGE                 
SYMLOOP_MAX                        
STREAM_MAX                         16
AIO_LISTIO_MAX                     
AIO_MAX                            
AIO_PRIO_DELTA_MAX                 20
DELAYTIMER_MAX                     2147483647
HOST_NAME_MAX                      64
LOGIN_NAME_MAX                     256
MQ_OPEN_MAX                        
MQ_PRIO_MAX                        32768
_POSIX_DEVICE_IO                   
_POSIX_TRACE                       
_POSIX_TRACE_EVENT_FILTER          
_POSIX_TRACE_INHERIT               
_POSIX_TRACE_LOG                   
RTSIG_MAX                          32
SEM_NSEMS_MAX                      
SEM_VALUE_MAX                      2147483647
SIGQUEUE_MAX                       63918
FILESIZEBITS                       64
POSIX_ALLOC_SIZE_MIN               4096
POSIX_REC_INCR_XFER_SIZE           
POSIX_REC_MAX_XFER_SIZE            
POSIX_REC_MIN_XFER_SIZE            4096
POSIX_REC_XFER_ALIGN               4096
SYMLINK_MAX                        
GNU_LIBC_VERSION                   glibc 2.28
GNU_LIBPTHREAD_VERSION             NPTL 2.28
POSIX2_SYMLINKS                    1
LEVEL1_ICACHE_SIZE                 65536
LEVEL1_ICACHE_ASSOC                2
LEVEL1_ICACHE_LINESIZE             64
LEVEL1_DCACHE_SIZE                 65536
LEVEL1_DCACHE_ASSOC                2
LEVEL1_DCACHE_LINESIZE             64
LEVEL2_CACHE_SIZE                  524288
LEVEL2_CACHE_ASSOC                 16
LEVEL2_CACHE_LINESIZE              64
LEVEL3_CACHE_SIZE                  16777216
LEVEL3_CACHE_ASSOC                 16
LEVEL3_CACHE_LINESIZE              64
LEVEL4_CACHE_SIZE                  0
LEVEL4_CACHE_ASSOC                 0
LEVEL4_CACHE_LINESIZE              0
IPV6                               200809
RAW_SOCKETS                        200809
_POSIX_IPV6                        200809
_POSIX_RAW_SOCKETS                 200809	

```

## 3. test 命令

```

test -x $HAPROXY || exit 0
test -f "$CONFIG" || exit 0

```

### 3.1. 判断目录

```

test -d /path/to/directory || mkdir -p /path/to/directory		

```

## 4. Directory and File System Related

### 4.1. dirname

```
$ dirname /usr/bin/find
/usr/bin

```

### 4.2. filename

```
$ basename /usr/bin/find
find

```

#### 4.2.1. 排除扩展名

```
file=test.txt
b=${file%.*}
echo $b

```

```
$ for file in *.JPG;do mv $file ${file%.*}.jpg;done

```

#### 4.2.2. 取扩展名

```
file=test.txt
b=${file##*.}
echo $b

```

### 4.3. test - check file types and compare values

```

test -x /usr/bin/munin-cron && /usr/bin/munin-cron

```

### 4.4. file — determine file type

```
$ file mis.netkiller.cn-0.0.1.war 
mis.netkiller.cn-0.0.1.war: Zip archive data, at least v2.0 to extract

$ file dian_icon.png 
dian_icon.png: PNG image data, 8 x 24, 8-bit/color RGBA, non-interlaced

$ file sms-s3.jpg
sms-s3.jpg: JPEG image data, JFIF standard 1.01

$ file -i favicon.ico 
favicon.ico: image/x-icon; charset=binary

$ file netkiller.wmv 
netkiller.wmv: Microsoft ASF

$ file netkiller.flv
netkiller.flv: Macromedia Flash Video

$ file neo.swf
neo.swf: Macromedia Flash data (compressed), version 10

$ file cs800.css 
cs800.css: ISO-8859 text, with CRLF line terminators

```

查看 mime

```
$ file -i sms.jpg
sms.jpg: image/jpeg; charset=binary

$ file -i call.png
call.png: image/png; charset=binary

$ file -i cs800.css
cs800.css: text/plain; charset=iso-8859-1

$ file -i neo.swf
neo.swf: application/x-shockwave-flash; charset=binary

$ file -i neo.wmv
neo.wmv: video/x-ms-asf; charset=binary

$ file -i neo.flv
neo.flv: video/x-flv; charset=binary

```

### 4.5. stat

```
modification time（mtime，修改时间）：当该文件的“内容数据”更改时，会更新这个时间。内容数据指的是文件的内容，而不是文件的属性。
status time（ctime，状态时间）：当该文件的”状态（status）”改变时，就会更新这个时间，举例，更改了权限与属性，就会更新这个时间。
access time（atime，存取时间）：当“取用文件内容”时，就会更新这个读取时间。举例来使用 cat 去读取该文件，就会更新 atime 了。

```

```
[root@apache www]# stat index.html
  File: `index.html'
  Size: 145355          Blocks: 296        IO Block: 4096   regular file
Device: fd01h/64769d    Inode: 15861815    Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (  502/  upuser)   Gid: (  502/  upuser)
Access: 2010-10-28 11:09:52.000000000 +0800
Modify: 2010-10-28 10:23:13.000000000 +0800
Change: 2010-10-28 10:23:13.000000000 +0800

```

### 4.6. mkdir - make directories

```
mkdir -p /tmp/test/{aaa,bbb,ccc,ddd}

mkdir -p /tmp/test/{aaa,bbb,ccc,ddd}/{eee,fff}

mkdir -p /tmp/test/{2008,2009,2010,2011}/{01,02,03,04,05,06,07,08,09,10,11,12}/{1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30}

```

### 4.7. rename

批量更改扩展名

```
rename 's/\.png/\.PNG/' *.png

rename '/\.mp3/\.MP3/' *.mp3
rename .mp3 .MP3 *.mp3

rename GIF gif *.GIF

```

```
for file in *.GIF
do
        mv $file ${file%.*}.gif
done

```

```
$ mkdir chapter.command.xxx.xml
$ mkdir chapter.command.bbb.xml
$ mkdir chapter.command.ccc.xml
$ mkdir chapter.command.ddd.xml

$ rename 's/command/cmd/' *.command.*.xml

```

### 4.8. touch

创建空文件，修改文件日期时间

```
touch [-acdmt] 文件
参数：
-a : 仅修改 access time。
-c : 仅修改时间，而不建立文件。
-d : 后面可以接日期，也可以使用 --date="日期或时间"
-m : 仅修改 mtime。
-t : 后面可以接时间，格式为 [YYMMDDhhmm]

# touch filename
# touch -d 20050809 filename
# touch -t 0507150202 bashrc
# touch -d "2 days ago" bashrc
# touch --date "2011-06-03" filename

```

### 4.9. truncate

#### truncate - shrink or extend the size of a file to the specified size

创建指定大小的文件

```
truncate -s 1k /tmp/test.txt
truncate -s 100m /tmp/test100.txt	

```

### 4.10. ls - list directory contents

```
$ ls
$ ls ~
$ ls -l
$ ls -a
$ ls -1
$ ls -F
bg7nyt.txt*  Desktop/    Firefox_wallpaper.png  Music/     public_html@  Videos/
bg7nyt.wav*  Documents/  Mail/                  nat.txt*   script/       workspace/
BOINC/       Examples@   mbox                   Pictures/  Templates/

```

{}通配符

```
ls {*.py,*.php,*.{sh,shell}}

```

take a look at below

```
alias l='ls -CF'
alias la='ls -A'
alias ll='ls -l'
alias ls='ls --color=auto'

```

#### 4.10.1. full-time / time-style 定义日期时间格式

默认风格

```
[www@www.netkiller.cn ~]$ ls -l /var/log/message*
-rw------- 1 root root 302533 Jun 18 09:50 /var/log/messages
-rw------- 1 root root 392028 May 23 03:30 /var/log/messages-20160523
-rw------- 1 root root 334328 May 29 03:09 /var/log/messages-20160529
-rw------- 1 root root 395792 Jun  5 03:44 /var/log/messages-20160605
-rw------- 1 root root 308984 Jun 13 03:33 /var/log/messages-20160613

```

修改后

--full-time = --time-style=full-iso

```
[www@www.netkiller.cn ~]$ ls -l --full-time /var/log/messages*
-rw------- 1 root root 308659 2016-06-18 10:24:49.186979051 +0800 /var/log/messages
-rw------- 1 root root 392028 2016-05-23 03:30:01.869219181 +0800 /var/log/messages-20160523
-rw------- 1 root root 334328 2016-05-29 03:09:02.158442470 +0800 /var/log/messages-20160529
-rw------- 1 root root 395792 2016-06-05 03:44:02.424073354 +0800 /var/log/messages-20160605
-rw------- 1 root root 308984 2016-06-13 03:33:02.004785063 +0800 /var/log/messages-20160613

[www@www.netkiller.cn ~]$ ls -l --time-style=full-iso /var/log/messages*
-rw------- 1 root root 308659 2016-06-18 10:24:49.186979051 +0800 /var/log/messages
-rw------- 1 root root 392028 2016-05-23 03:30:01.869219181 +0800 /var/log/messages-20160523
-rw------- 1 root root 334328 2016-05-29 03:09:02.158442470 +0800 /var/log/messages-20160529
-rw------- 1 root root 395792 2016-06-05 03:44:02.424073354 +0800 /var/log/messages-20160605
-rw------- 1 root root 308984 2016-06-13 03:33:02.004785063 +0800 /var/log/messages-20160613

```

long-iso

```
[www@www.netkiller.cn ~]$ ls -lh --time-style long-iso /var/log/message*
-rw------- 1 root root 296K 2016-06-18 10:00 /var/log/messages
-rw------- 1 root root 383K 2016-05-23 03:30 /var/log/messages-20160523
-rw------- 1 root root 327K 2016-05-29 03:09 /var/log/messages-20160529
-rw------- 1 root root 387K 2016-06-05 03:44 /var/log/messages-20160605
-rw------- 1 root root 302K 2016-06-13 03:33 /var/log/messages-20160613			

```

通过配置 TIME_STYLE 环境变量，改变日期格式

```
[www@www.netkiller.cn ~]$ export TIME_STYLE=long-iso

[www@www.netkiller.cn ~]$ ls -l /var/log/message*
-rw------- 1 root root 302533 2016-06-18 09:50 /var/log/messages
-rw------- 1 root root 392028 2016-05-23 03:30 /var/log/messages-20160523
-rw------- 1 root root 334328 2016-05-29 03:09 /var/log/messages-20160529
-rw------- 1 root root 395792 2016-06-05 03:44 /var/log/messages-20160605
-rw------- 1 root root 308984 2016-06-13 03:33 /var/log/messages-20160613

[www@www.netkiller.cn ~]$ export TIME_STYLE=iso
[www@www.netkiller.cn ~]$ ls -l /var/log/message*
-rw------- 1 root root 302533 06-18 09:50 /var/log/messages
-rw------- 1 root root 392028 05-23 03:30 /var/log/messages-20160523
-rw------- 1 root root 334328 05-29 03:09 /var/log/messages-20160529
-rw------- 1 root root 395792 06-05 03:44 /var/log/messages-20160605
-rw------- 1 root root 308984 06-13 03:33 /var/log/messages-20160613

```

自定义格式

```
[www@www.netkiller.cn ~]$ ls -l --time-style="+%Y-%m-%d" /var/log/message*
-rw------- 1 root root 302533 2016-06-18 /var/log/messages
-rw------- 1 root root 392028 2016-05-23 /var/log/messages-20160523
-rw------- 1 root root 334328 2016-05-29 /var/log/messages-20160529
-rw------- 1 root root 395792 2016-06-05 /var/log/messages-20160605
-rw------- 1 root root 308984 2016-06-13 /var/log/messages-20160613

[root@www.netkiller.cn ~]# export TIME_STYLE='+%Y/%m/%d %H:%M:%S'
[root@www.netkiller.cn ~]# ls -l /var/log/messages*
-rw------- 1 root root 189352 2016/06/18 10:20:01 /var/log/messages
-rw------- 1 root root 322453 2016/05/22 03:48:02 /var/log/messages-20160522
-rw------- 1 root root 247398 2016/05/30 03:37:01 /var/log/messages-20160530
-rw------- 1 root root 174633 2016/06/05 03:14:02 /var/log/messages-20160605
-rw------- 1 root root 196728 2016/06/12 03:17:01 /var/log/messages-20160612

```

### 4.11. cp - copy files and directories

#### 4.11.1. copy directories recursively

```
cp -r /etc/* ~/myetc

```

#### 4.11.2. overwrite an existing file

```
# alias cp
alias cp='cp -i'

# unalias cp
# alias cp
-bash: alias: cp: not found

```

#### 4.11.3. -a, --archive same as -dR --preserve=all

```
# cp -a file file2

```

-a 参数可以保留原文件的日期与权限等等信息。

```
# ll
-rw-r--r--. 1 root root      2559 Aug 27 05:00 yum.sh

# cp -a yum.sh yum1.sh
# cp yum.sh yum2.sh

# ll yum*
-rw-r--r--. 1 root root 2559 Aug 27 05:00 yum1.sh
-rw-r--r--. 1 root root 2559 Aug 27 05:58 yum2.sh
-rw-r--r--. 1 root root 2559 Aug 27 05:00 yum.sh

```

现在可以看到 yum1.sh 与 yum.sh 日期是相同的，而没有实现-a 参数的 yum2.sh 日期为当前日期。

### 4.12. rm - remove files or directories

#### 4.12.1. -bash: /bin/rm: Argument list too long

```
ls -1 | xargs rm -f
find . -name 'spam-*' | xargs rm
find . -exec rm {} \;

ls | xargs -n 10 rm -fr # 10 个为一组

```

#### 4.12.2. zsh: sure you want to delete all the files in /tmp [yn]?

```
yes | rm -i file

```

### 4.13. df - report file system disk space usage

```
neo@netkiller:~$ df -lh
Filesystem            Size  Used Avail Use% Mounted on
/dev/sda1              19G  3.1G   15G  17% /
none                  996M  224K  996M   1% /dev
none                 1000M     0 1000M   0% /dev/shm
none                 1000M  520K 1000M   1% /var/run
none                 1000M     0 1000M   0% /var/lock
none                 1000M     0 1000M   0% /lib/init/rw
/dev/sda6              19G   13G  4.5G  75% /home
/dev/sda10            556M  178M  351M  34% /boot
/dev/sda7              46G  4.4G   40G  10% /var
/dev/sda8             367G   60G  289G  18% /opt
/dev/sda9             6.5G  143M  6.0G   3% /tmp

```

### 4.14. du - estimate file space usage

```
neo@netkiller:~$ sudo du -sh /usr/local
63M     /usr/local

```

### 4.15. tac - concatenate and print files in reverse

```
$ tac /etc/issue
Kernel \r on an \m
CentOS release 5.4 (Final)

```

### 4.16. split - split a file into pieces

#### 4.16.1. 按行分割文件

##### -l, --lines=NUMBER put NUMBER lines per output file

每 10000 行产生一个新文件

```
# split -l 10000 book.txt myfile		

```

#### 4.16.2. 按尺寸分割文件

##### -b, --bytes=SIZE put SIZE bytes per output file

下面的例子是每 10 兆分割为一个新文件

```
split -b 10m large.bin new_file_prefix			

```

### 4.17. find - search for files in a directory hierarchy

#### 4.17.1. name

Find every file under directory /usr ending in "stat".

```
$ find /usr -name *stat
/usr/src/linux-headers-2.6.24-22-generic/include/config/cpu/freq/stat
/usr/bin/lnstat
/usr/bin/sar.sysstat
/usr/bin/mpstat
/usr/bin/rtstat
/usr/bin/nstat
/usr/bin/lpstat
/usr/bin/ctstat
/usr/bin/stat
/usr/bin/kpsestat
/usr/bin/pidstat
/usr/bin/iostat
/usr/bin/vmstat
/usr/lib/sysstat
/usr/share/doc/sysstat
/usr/share/gnome/help/battstat
/usr/share/omf/battstat
/usr/share/zsh/help/stat
/usr/share/zsh/4.3.4/functions/Completion/Unix/_diffstat
/usr/share/zsh/4.3.4/functions/Completion/Zsh/_stat
/usr/share/zsh/4.3.4/functions/Zftp/zfstat

```

```
find  \( -iname '*.jpg' -o -iname '*.png' -o -iname '*.gif'  \)

find /www/images -type f \( -iname '*.js' -o -iname '*.css' -o -iname '*.html' \) | xargs tar -czf ~/images.tgz

```

使用通配符

```
find . -name "*.jsp" -delete
find . -name "*.xml" -delete		

```

#### 4.17.2. regex

```
find . -regex ".*\.\(jpg\|png\)"

```

下面 regex 与 name 作用相同

```
find . -regex ".*\.\(txt\|sh\)"
find . -name "*.sh" -o -name "*.txt"

```

-regex 参数，使用正则表达式来匹配. 查找当前目录以及子目录中以 ".sh",并改为以".shell"结尾.

```

[neo@netkiller test]# tree a
a
├── a.py
├── a.sh
└── b
    ├── b.py
    ├── b.sh
    ├── c
    │   └── c.sh
    └── d
        └── d.sh

[neo@netkiller test]# find ./a -type f  -regex ".*\.sh$" | sed -r -n 's#(.*\.)sh$#mv & \1shell#e'
[neo@netkiller test]# tree a
a
├── a.py
├── a.shell
└── b
    ├── b.py
    ├── b.shell
    ├── c
    │   └── c.shell
    └── d
        └── d.shell

 // 注意 sed s->e  使用方式,官方文档是这样解释的.
This command allows one to pipe input from a shell command into pattern space. If a substitution was made, the command that is found in pattern space is executed and pattern space is replaced with its output. A trailing newline is suppressed; results are undefined if the command to be executed contains a NUL character. This is a GNU sed extension.

```

#### 4.17.3. user

Find every file under /home and /var/www owned by the user neo.

```
$ find /home -user neo
$ find /var/www -user neo
$ find . -user nobody -iname '*.php'

```

#### 4.17.4. perm

```
find ./ -perm -7 -print | xargs chmod o-w
find . -perm -o=w

```

查找当前目录下权限为 777 的文件并显示到标准输出

```
find ./ -type f -perm 777 -print		

```

#### 4.17.5. type

##### 4.17.5.1. 分别设置文件与目录的权限

```
find /usr/www/phpmyadmin -type d -exec chmod 755 {} \;
find /usr/www/phpmyadmin -type f -exec chmod 644 {} \;

```

#### 4.17.6. -delete

```
# find /var/spool/clientmqueue/ -type f -delete

```

保留最近 7 天的问题，其他全部删除

```
find . -type f -mtime +7 -delete

```

#### 4.17.7. exec

替换文本

```
# find ./ -exec grep str1 ‘{}’ \; -exec sed -i.bak s/str1/str2/g ‘{}’ \;

```

```
find -exec ls -l {} \; | grep '2011-01-18'

```

查找*.html 文件中 aaa 替换为 bbb

```
find . -name "*.html" -type f -exec sed -i "s/aaa/bbb/" {} \;		

```

查找文件中含有 openWindow 字符串的文件

```
# find -type f -name "*.js" -exec grep -H -A2 openWindow {} \;

./javascript/commonjs.js:function openWindow(url){
./javascript/commonjs.js-	window.open(url + "?rand=" + getRandom(), 'gamebinary');
./javascript/commonjs.js-}

```

```
find -type f -regex ".*\.\(css\|js\)" -exec yuicompressor {} -o {} \;
find -type f -name "*.js" -exec yuicompressor --type js {} -o {} \;
find -type f -name "*.css" -exec yuicompressor --type css {} -o {} \;

```

#### 4.17.8. 排除目录

```
find /usr/local -path "/usr/local/share" -prune -o -print

find /usr/local \( -path /usr/local/bin -o -path /usr/local/sbin \) -prune -o -print

find /usr/local \(-path /usr/local/dir1 -o -path /usr/local/file1 \) -prune -o -name "temp" -print

```

查找当前目录下的 php 文件,排除子目录 templates_c, caches

```
find . \( -path ./templates_c -o -path ./caches \) -prune -o -name "*.php" -print

```

#### 4.17.9. -mmin n File's data was last modified n minutes ago.

```
# find . -mmin +5 -mmin -10

```

```
find /www -type f -mtime +60s

```

#### 4.17.10. -ctime

查找当前目录下超过 6 天且是空文件并删除

```
find ./ -type d -empty -ctime +6 -exec rm -f {} \;	

```

查找 7 天前的文件并删除

```
find /backup/svn/day -type f -ctime +7 -exec rm -f {} \; 
find /backup/svn/day -type f -ctime +7 -delete
find /backup/svn/day -type f -ctime +7 | xargs rm -f	

```

#### 4.17.11. -mtime / -mmin

查询最近 3 天前内修改的文件

```
find . -type f -mtime -3

```

3 天前

```
find . -type f -mtime +3

```

例 26.1. backup(find + tar)

```
find / -type f -mtime -7 | xargs tar -rf weekly_incremental.tar
gzip weekly_incremental.tar

```

保留 7 天，删除 7 天的日志文件

```
COPIES=7
find /var/log -type f -mtime +$COPIES -delete

```

#### 4.17.12. --newer

```
tar --newer="2011-07-04" -zcvf backup.tar.gz /var/www/
tar cvzf foo.tgz /bak -N "2004-03-03 16:49:17"

```

#### 4.17.13. -print / -printf

```
[root@scientific ~]# find / -maxdepth 1 -name '[!.]*' -printf 'Name: %16f Size: %6s\n'
Name:                / Size:   4096
Name:             misc Size:      0
Name:            media Size:   4096
Name:             home Size:   4096
Name:              dev Size:   3840
Name:              net Size:      0
Name:             proc Size:      0
Name:             sbin Size:  12288
Name:             root Size:   4096
Name:              lib Size:   4096
Name:           cgroup Size:   4096
Name:              srv Size:   4096
Name:              mnt Size:   4096
Name:              etc Size:  12288
Name:              usr Size:   4096
Name:            lib64 Size:  12288
Name:             boot Size:   1024
Name:              var Size:   4096
Name:          selinux Size:      0
Name:              opt Size:   4096
Name:              tmp Size:   4096
Name:       lost+found Size:  16384
Name:              sys Size:      0
Name:              bin Size:   4096

# find /etc/ -type f -printf "%CY-%Cm-%Cd %Cr %8s %f\n"

```

#### 4.17.14. -size

查找 0 字节文件

```
find /www -type f -size 0

```

查找根目录下大于 1G 的文件

```
find /  -type f -size +1000M		

```

#### 4.17.15. -path

搜索当前目录下除了 keys 目录下所以子目录中的文件

```
find ./ -path "./keys" -prune -o -type f -print 		

```

find 排除多个目录

```
find ./ \( -path ./conf -o -path ./logs \) -prune -o -print

find /data/ \( -path /data/data_backup -o -path /data/mysql \) -prune -o -name "core.*" -type f 
/data/mysql
/data/data_backup		

```

ps 要么都是绝对路径 要么都是相对路径 /data/ 必须有"/" path 后面的路径必须没有"/"

#### 4.17.16. 目录深度控制

```

neo@MacBook-Pro ~/workspace/Linux % find */images -type d -d 0 -exec echo {} \;
Cryptography/images
Monitoring/images
OpenLDAP/images
Project/images
Web/images

```

```

find */images -type d -d 0 -exec rsync -au {}/* $(PUBLIC_HTML)/linux/images \; 		

```

#### 4.17.17. -maxdepth

-maxdepth 和-mindepth，最大深度，最小深度搜索，搜索当前目录下最大深度为 1 的所以文件

```
find . -maxdepth 1 -type f 		

```

#### 4.17.18. xargs

```
find /etc -type f|xargs md5sum

```

sha1sum

```
find /etc -type f|xargs sha1sum

```

```
find ./ -name "*html" | xargs -n 1 sed -i -e 's/aaa/bbb/g'

```

```
find /tmp -name core -type f -print | xargs /bin/rm -f
find . -type f -exec file '{}' \;

```

find 后执行 xargs 提示 xargs: argument line too long 解决方法：

```
find . -type f -name "*.log" -print0 | xargs -0 rm -f		

```

## 5. package / compress and decompress

### 5.1. tar — The GNU version of the tar archiving utility

#### 5.1.1. tar examples

tar

```
tar -cvf foo.tar foo/
       tar contents of folder foo in foo.tar

tar -xvf foo.tar
       extract foo.tar

```

#### 5.1.2. gunzip

```
tar -zcvf foo.tar foo/
       tar contents of folder foo in foo.tar.gz

```

```
tar -xvzf foo.tar.gz
       extract gzipped foo.tar.gz

```

#### 5.1.3. b2zip

**b2zip**

```
tar -jcvf foo.tar.bz2 foo/
       tar contents of folder foo in foo.tar.bz2

tar -jxvf foo.tar.bz2
       extract b2zip foo.tar.bz2

```

#### 5.1.4. compress

**compress/uncompress**

```
tar -Zcvf foo.tar.Z foo/
       tar contents of folder foo in foo.tar.Z

tar -Zxvf foo.tar.Z
       extract compress foo.tar.Z

```

#### 5.1.5. .xz 文件

```

tar -Jxf file.pkg.tar.xz

```

#### 5.1.6. -t, --list

-t, --list list the contents of an archive

列出 tar 包中的文件

```
tar tvf neo.tar.gz

```

```
# mkdir -p /www/test.com/www.test.com/
# echo helloworld > /www/test.com/www.test.com/test.txt
# tar zcvf www.test.com.tar.gz /www/test.com/www.test.com/

# tar ztvf www.test.com.tar.gz
drwxr-xr-x root/root         0 2013-08-08 15:24 www/test.com/www.test.com/
-rw-r--r-- root/root        11 2013-08-08 15:24 www/test.com/www.test.com/test.txt

# tar zxvf www.test.com.tar.gz
www/test.com/www.test.com/
www/test.com/www.test.com/test.txt

# find www
www
www/test.com
www/test.com/www.test.com
www/test.com/www.test.com/test.txt

```

#### 5.1.7. tar: Removing leading `/’ from member names

-P, --absolute-names don't strip leading `/'s from file names

```
$ tar -czvPf neo.tar.gz /home/neo/
$ tar -xzvPf neo.tar.gz

```

```
tar zcvfP www.test.com.tar.gz /www/test.com/www.test.com/
tar zxvfP www.test.com.tar.gz

```

#### 5.1.8. -C, --directory=DIR

-C, --directory=DIR change to directory DIR

解压到目标目录

```
tar -xzvf neo.tar.gz -C /tmp

```

```
# tar zxvf www.test.com.tar.gz -C /tmp
www/test.com/www.test.com/
www/test.com/www.test.com/test.txt

# find /tmp/www/
/tmp/www/
/tmp/www/test.com
/tmp/www/test.com/www.test.com
/tmp/www/test.com/www.test.com/test.txt

# rm -rf /www/test.com/*

# tar zxvf www.test.com.tar.gz -C /
www/test.com/www.test.com/
www/test.com/www.test.com/test.txt

# find /www/test.com/
/www/test.com/
/www/test.com/www.test.com
/www/test.com/www.test.com/test.txt

```

#### 5.1.9. --exclude

排除 neo 目录

```
tar --exclude /home/neo -zcvf myfile.tar.gz /home/* /etc

tar zcvf rpmbuild/SOURCES/netkiller-1.0.tar.gz ~/workspace/public_html/* --exclude .git --exclude .svn

```

#### 5.1.10. -T

```
find . -name "*.jpg" -print >list
tar -T list -czvf picture.tar.gz

find /etc/ | tar czvf xxx1.tar.gz -T -

```

#### 5.1.11. 日期过滤

打包 2010/08/01 之后的文件和目录

```
tar -N '2010/08/01' -zcvf home.tar.gz /home

```

#### 5.1.12. 保留权限

```
tar -zxvpf /tmp/etc.tar.gz /etc

```

#### 5.1.13. -r, --append

追加最近 7 天更改过的文件

```
find / -type f -mtime -7 | xargs tar -rf weekly_incremental.tar

```

#### 5.1.14. 远程传输

**tar -jcpvf - file | ssh remote "tar -jxpvf -"**

```
tar -jcpvf - file.php | ssh root@172.16.3.1 "tar -jxpvf -"

```

#### 5.1.15. 分卷压缩

```
分卷压缩一个目录：如 doc

在 doc 目录的上次目录

#tar cvf doc | split -b 2m (已 2M 大小分卷压缩)
#cat x* > doc.tar (合成分卷压缩包)

```

或者

```
#tar czvf doc.tar.gz doc/
#tar czvfp - doc.tar.gz | split -b 5m
#cat x* > doc.tar.gz

查看压缩包里面的内容:

#tar -tf doc.tar
#tar -tzvf doc.tar.gz

```

### 5.2. cpio - copy files to and from archives

```
 find /opt -print | cpio -o > opt.cpio

find . -type f -name '*.sh' -print | cpio -o | gzip >sh.cpio.gz

cpio –i < opt.cpio 
```

 **### 5.3. gzip

**gzip/gunzip**

```
# ls access.2010-{10,11}-??.log
access.2010-10-01.log  access.2010-10-17.log  access.2010-11-02.log  access.2010-11-18.log
access.2010-10-02.log  access.2010-10-18.log  access.2010-11-03.log  access.2010-11-19.log
access.2010-10-03.log  access.2010-10-19.log  access.2010-11-04.log  access.2010-11-20.log
access.2010-10-04.log  access.2010-10-20.log  access.2010-11-05.log  access.2010-11-21.log
access.2010-10-05.log  access.2010-10-21.log  access.2010-11-06.log  access.2010-11-22.log
access.2010-10-06.log  access.2010-10-22.log  access.2010-11-07.log  access.2010-11-23.log
access.2010-10-07.log  access.2010-10-23.log  access.2010-11-08.log  access.2010-11-24.log
access.2010-10-08.log  access.2010-10-24.log  access.2010-11-09.log  access.2010-11-25.log
access.2010-10-09.log  access.2010-10-25.log  access.2010-11-10.log  access.2010-11-26.log
access.2010-10-10.log  access.2010-10-26.log  access.2010-11-11.log  access.2010-11-27.log
access.2010-10-11.log  access.2010-10-27.log  access.2010-11-12.log  access.2010-11-28.log
access.2010-10-12.log  access.2010-10-28.log  access.2010-11-13.log  access.2010-11-29.log
access.2010-10-13.log  access.2010-10-29.log  access.2010-11-14.log  access.2010-11-30.log
access.2010-10-14.log  access.2010-10-30.log  access.2010-11-15.log
access.2010-10-15.log  access.2010-10-31.log  access.2010-11-16.log
access.2010-10-16.log  access.2010-11-01.log  access.2010-11-17.log
# gzip access.2010-{10,11}-??.log

```

```

# ls access.2010-{0?,10,11}-??.log
access.2010-08-28.log  access.2010-10-02.log  access.2010-10-13.log  access.2010-10-27.log  access.2010-11-06.log  access.2010-11-17.log  access.2010-11-26.log
access.2010-08-31.log  access.2010-10-03.log  access.2010-10-14.log  access.2010-10-28.log  access.2010-11-08.log  access.2010-11-18.log  access.2010-11-27.log
access.2010-09-24.log  access.2010-10-04.log  access.2010-10-15.log  access.2010-10-29.log  access.2010-11-09.log  access.2010-11-19.log  access.2010-11-28.log
access.2010-09-25.log  access.2010-10-06.log  access.2010-10-17.log  access.2010-10-30.log  access.2010-11-10.log  access.2010-11-20.log  access.2010-11-29.log
access.2010-09-26.log  access.2010-10-07.log  access.2010-10-19.log  access.2010-10-31.log  access.2010-11-11.log  access.2010-11-21.log  access.2010-11-30.log
access.2010-09-27.log  access.2010-10-08.log  access.2010-10-20.log  access.2010-11-02.log  access.2010-11-12.log  access.2010-11-22.log
access.2010-09-29.log  access.2010-10-09.log  access.2010-10-22.log  access.2010-11-03.log  access.2010-11-14.log  access.2010-11-23.log
access.2010-09-30.log  access.2010-10-10.log  access.2010-10-23.log  access.2010-11-04.log  access.2010-11-15.log  access.2010-11-24.log
access.2010-10-01.log  access.2010-10-12.log  access.2010-10-25.log  access.2010-11-05.log  access.2010-11-16.log  access.2010-11-25.log
# gzip access.2010-{0?,10,11}-??.log &

```

### 5.4. zip, zipcloak, zipnote, zipsplit - package and compress (archive) files

*.zip

**zip/unzip file[.zip]**

### 5.5. bzip2, bunzip2 - a block-sorting file compressor

```

[root@localhost src]# yum install bzip2		

```

查看 RPM 包所含文件

```

[root@localhost src]# rpm -ql bzip2-1.0.6-13.el7
/usr/bin/bunzip2
/usr/bin/bzcat
/usr/bin/bzcmp
/usr/bin/bzdiff
/usr/bin/bzgrep
/usr/bin/bzip2
/usr/bin/bzip2recover
/usr/bin/bzless
/usr/bin/bzmore
/usr/share/doc/bzip2-1.0.6
/usr/share/doc/bzip2-1.0.6/CHANGES
/usr/share/doc/bzip2-1.0.6/LICENSE
/usr/share/doc/bzip2-1.0.6/README
/usr/share/man/man1/bunzip2.1.gz
/usr/share/man/man1/bzcat.1.gz
/usr/share/man/man1/bzcmp.1.gz
/usr/share/man/man1/bzdiff.1.gz
/usr/share/man/man1/bzgrep.1.gz
/usr/share/man/man1/bzip2.1.gz
/usr/share/man/man1/bzip2recover.1.gz
/usr/share/man/man1/bzless.1.gz
/usr/share/man/man1/bzmore.1.gz

```

### 5.6. RAR

```
sudo apt-get install rar unrar

```

### 5.7. 7-Zip

#### p7zip - 7z file archiver with high compression ratio

http://www.7-zip.org/

如果你仅仅是解压文件，只需安装下面的包即可

```
$ sudo apt-get install p7zip

```

如果你要创建 7zip 文件就需要安装 p7zip-full

```
$ sudo apt-get install p7zip-full

```

#### 5.7.1. 压缩

```
$ 7z a test.7z /etc/*			

```

#### 5.7.2. 浏览压缩包

```
$ 7z l test.7z 			

```

#### 5.7.3. 解压

```
$ 7z e test.7z			

```

#### 5.7.4. Creates self extracting archive.

创建自解压文件

```
7z a -sfx a.7z *.txt

```

解压

```
./a.7z			

```

### 5.8. RAR

```

$ unrar test.rar		

```

### 5.9. xz, unxz, xzcat, lzma, unlzma, lzcat - Compress or decompress .xz and .lzma files

```

[root@localhost ~]# echo "Hello" > test		
[root@localhost ~]# xz -z test
[root@localhost ~]# ll test.xz 
-rw------- 1 root root 1436 2019-01-16 06:13 test.xz
[root@localhost ~]# xz -d test.xz 
[root@localhost ~]# cat test
Hello

```

tar 用法

```

[root@localhost ~]# tar Jcvf test.tar.xz test
test
[root@localhost ~]# ll test.tar.xz 
-rw-r--r-- 1 root root 1528 2019-03-19 04:32 test.tar.xz

[root@localhost ~]# tar Jxvf test.tar.xz 
test		

```**  **## 6. date and time

### 6.1. 日期格式

自定义格式化显示日期

```
%n : 下一行
%t : 跳格
%H : 小时(00..23)
%I : 小时(01..12)
%k : 小时(0..23)
%l : 小时(1..12)
%M : 分钟(00..59)
%p : 显示本地 AM 或 PM
%r : 直接显示时间 (12 小时制，格式为 hh:mm:ss [AP]M)
%s : 从 1970 年 1 月 1 日 00:00:00 UTC 到目前为止的秒数
%S : 秒(00..61)
%T : 直接显示时间 (24 小时制)
%X : 相当于 %H:%M:%S
%Z : 显示时区 %a : 星期几 (Sun..Sat)
%A : 星期几 (Sunday..Saturday)
%b : 月份 (Jan..Dec)
%B : 月份 (January..December)
%c : 直接显示日期与时间
%d : 日 (01..31)
%D : 直接显示日期 (mm/dd/yy)
%h : 同 %b
%j : 一年中的第几天 (001..366)
%m : 月份 (01..12)
%U : 一年中的第几周 (00..53) (以 Sunday 为一周的第一天的情形)
%w : 一周中的第几天 (0..6)
%W : 一年中的第几周 (00..53) (以 Monday 为一周的第一天的情形)
%x : 直接显示日期 (mm/dd/yy)
%y : 年份的最后两位数字 (00.99)
%Y : 完整年份 (0000..9999)	

```

2010/06/18 17:57:38

```
$ date '+%Y/%m/%d %H:%M:%S'

```

2010-06-18 17:57:58

```
$ date '+%Y-%m-%d %H:%M:%S'

```

```
$ date +'%Y-%m-01 00:00:01'
2010-10-01 00:00:01

```

```
[root@netkiller ~]# date +%F
2015-07-30

[root@netkiller ~]# date +%Y-%m-%d-%H-%M
2015-07-30-13-49			

```

#### 6.1.1. weekday name

```
$ date +%a
Fri

$ date +%A
Friday

```

### 6.2. -d --date=

```
# date -d next-day +%Y%m%d
20060328
# date -d last-day +%Y%m%d
20060326
# date -d yesterday +%Y%m%d
20060326
# date -d tomorrow +%Y%m%d
20060328
# date -d last-month +%Y%m
200602
# date -d next-month +%Y%m
200604
# date -d next-year +%Y
2007

```

```
date 命令的另一个扩展是 -d 选项

1) 2 周后的日期 和一天前的日期
[root@netkiller ~]# date -d '2 weeks'
2015 年 08 月 13 日 星期四 13:53:06 HKT

[root@netkiller ~]# date -d '1 day ago'
2015 年 07 月 29 日 星期二 13:53:14 HKT
[root@netkiller ~]# date -d yesterday
2015 年 07 月 29 日 星期三 13:53:26 HKT

2)下周一的日期
[root@netkiller ~]# date -d 'next monday'
2015 年 08 月 03 日 星期一 00:00:00 HKT

3)使用负数以得到相反的日期
[root@netkiller ~]# date -d '-1 weeks'
2015 年 07 月 23 日 星期四 13:59:43 HKT
[root@netkiller ~]# date -d '1 weeks'
2015 年 08 月 06 日 星期四 13:59:50 HKT

上个月的一周前
[root@netkiller ~]# date -d 'last-month -1 week'
2015 年 06 月 23 日 星期二 14:00:59 HKT

相对于 6 月 30 号的前两周
[root@netkiller ~]# date -d 'jun 30 -2 weeks'
2015 年 06 月 16 日 星期二 00:00:00 HKT
[root@netkiller ~]# date -d 'jun 30 -2 weeks'  +%Y_%m_%d
2015_06_16		

```

#### 6.2.1. 日期偏移量

```
昨天 (前一天)
date --date='1 days ago' "+%Y-%m-%d"
date -d '1 days ago' "+%Y-%m-%d"
date -d yesterday "+%Y-%m-%d"

明天 (後一天)
date --date='1 days' "+%Y-%m-%d"
date -d '1 days' "+%Y-%m-%d"
date -d tomorrow "+%Y-%m-%d"

```

##### 6.2.1.1. day

```
$ date -d '-1 day' +'%Y-%m-%d 00:00:01'
2010-10-14 00:00:01

$ date -d '+5 day' +'%Y-%m-%d 00:00:01'
2010-10-20 00:00:01

```

##### 6.2.1.2. month

```
$ date -d '+2 month' +'%Y-%m-%d 00:00:01'
2010-12-15 00:00:01

$ date -d '-1 month' +'%Y-%m-%d 00:00:01'
2010-09-15 00:00:01

```

##### 6.2.1.3. year

```
$ date -d '-5 year' +'%Y-%m-%d'
2005-10-15
$ date -d '+1 year' +'%Y-%m-%d'
2011-10-15

```

#### 6.2.2. 时间偏移

```
1 小時前
date --date='1 hours ago' "+%Y-%m-%d %H:%M:%S"
1 小時後
date --date='1 hours' "+%Y-%m-%d %H:%M:%S"
1 分鐘前
date --date='1 minutes ago' "+%Y-%m-%d %H:%M:%S"
1 分鐘後
date --date='1 minutes' "+%Y-%m-%d %H:%M:%S"
1 秒前
date --date='1 seconds ago' "+%Y-%m-%d %H:%M:%S"
1 秒後
date --date='1 seconds' "+%Y-%m-%d %H:%M:%S"

```

### 6.3. 时间戳

```
1 计算当天的时间戳

[root@netkiller ~]#  date +%s
1440641485

2 计算指定日期的时间戳

[root@netkiller ~]# date -d "2015-08-05 09:45:44" +%s
1438739144

3 时间戳转换成时间

[root@netkiller ~]# date -d @1438739144
2015 年 08 月 05 日 星期三 09:45:44 HKT

格式输出
[root@mygitlab ~]# date -d @1440661395  +%Y-%m-%d-%H-%M
2015-08-27-00-43		

```

### 6.4. RFC 2822

RFC 2822 的日期与时间输出格式, RFC 2822 的格式像这样: 星期,日-月-年,小时:分钟:秒 时区

时区 +0800 等同于 GMT +8

```
[root@netkiller ~]# date -R
Thu, 30 Jul 2015 11:29:00 +0800

```

### 6.5. UTC

UTC time 以 UTC 形式显示日期和时间

```
$ datetime=$(date -u '+%Y%m%d %H:%M:%S')
$ echo $datetime
20091203 06:22:03

```

```
[root@netkiller ~]# date -u
2015 年 07 月 30 日 星期四 03:35:01 UTC		

```

## 7. Numeric

### 7.1. 数值运算

```

echo $((3+5))
expr 6 + 3
awk 'BEGIN{a=(3+2)*2;print a}'

```

### 7.2. seq - print a sequence of numbers

```

[neo@test ~]$ seq 10
1
2
3
4
5
6
7
8
9
10
[neo@test ~]$ seq 5 10
5
6
7
8
9
10

```

等差列, 步长设置

```

$ seq 1 1 10
1
2
3
4
5
6
7
8
9
10

$ seq 1 2 10
1
3
5
7
9

# seq 0 2 10
0
2
4
6
8
10

```

分隔符

```

# seq -s : -w 1 10
01:02:03:04:05:06:07:08:09:10

# seq -s '|' -w 1 10
01|02|03|04|05|06|07|08|09|10

```

等宽，前导字符用 0 填充

```

# seq -w 1 10
01
02
03
04
05
06
07
08
09
10

```

### 7.3. bc - An arbitrary precision calculator language

```

$ echo "4*5" | bc

```

```

# more calc.txt
3+2
4+5
8*2
10/4
# bc calc.txt
5
9
16
2

```

## 8. Text Processing

### 8.1. iconv - Convert encoding of given files from one encoding to another

#### 8.1.1. cconv - A iconv based simplified-traditional chinese conversion tool

cconv 是建立在 iconv 之上，可以 UTF8 编码直接转换，并增加了词转换。

```
sudo apt-get install cconv

```

使用 cconv 进行简繁转换的方法为：

```
cconv -f UTF8-CN -t UTF8-HK zh-cn.txt -o zh-hk.txt

```

#### 8.1.2. uconv - convert data from one encoding to another

安装

```
sudo apt-get install libicu-dev

```

例子

```
$ uconv -f cp1252 -t UTF-8 -o file_in_utf8.txt file_in_cp1252_encoding.txt

```

### 8.2. 字符串处理命令 expr

```

字符串处理命令 expr 用法简介:
名称：expr
用途:求表达式变量的值。
语法: expr Expression
实例如下:
例子 1:字串长度
shell>> expr length "this is a test content";
22
例子 2:求余数
shell>> expr 20 % 9
2
例子 3:从指定位置处截取字符串
shell>> expr substr "this is a test content" 3 5
is is
例子 4:指定字符串第一次出现的位置
shell>> expr index "testforthegame" s
3
例子 5:字符串真实重现
shell>> expr quote thisisatestformela
thisisatestformela

```

### 8.3. cat - concatenate files and print on the standard output

```
-b	不对空白行编号。
-e	使用 $ 字符显示行尾。
-n	从 1 开始对所有输出行编号。
-q	使用静默操作（禁止错误消息）。
-r	将所有多个空行替换为单行（“压缩”空白）。
-t	将制表符显示为 ^I。
-u	不对输出进行缓冲。
-v	可视地显示非打印控制字符。

```

#### 8.3.1. -s, --squeeze-blank suppress repeated empty output lines

-S 将多个空白行压缩到单行中（与 -r 相同）

```

$ cat >> /tmp/test <<EOF
Line1

Line2

Line3

Line4

Line5

EOF

$ cat -s /tmp/test
Line1

Line2

Line3

Line4

Line5

```

#### 8.3.2. -v, --show-nonprinting use ^ and M- notation, except for LFD and TAB

显示控制字符。例如 Tab 等，下面例子查看文件结尾换行符类型

```

[neo@netkiller ~]# cat -v file.txt
GRANT USAGE ON *.* TO 'esauser'@'localhost' IDENTIFIED BY xxxxxxx; ^M
^M
file^M
2059^M

```

### 8.4. nl - number lines of files

```
$ nl /etc/issue
     1  CentOS release 5.4 (Final)
     2  Kernel \r on an \m

```

### 8.5. tr - translate or delete characters

```

[:alnum:] ：所有字母字符与数字
[:alpha:] ：所有字母字符
[:blank:] ：所有水平空格
[:cntrl:] ：所有控制字符
[:digit:] ：所有数字
[:graph:] ：所有可打印的字符(不包含空格符)
[:lower:] ：所有小写字母
[:print:] ：所有可打印的字符(包含空格符)
[:punct:] ：所有标点字符
[:space:] ：所有水平与垂直空格符
[:upper:] ：所有大写字母
[:xdigit:] ：所有 16 进位制的数字		

```

#### 8.5.1. 替换字符

":"替换为"\n"

```

$ cat /etc/passwd |tr ":" "\n"

```

#### 8.5.2. 英文大小写转换

使用 tr '[:lower:]' '[:upper:]' 将小写字母替换成大写字母

```

[root@localhost ~]# echo "Helloworld" | tr '[:lower:]' '[:upper:]'
HELLOWORLD			

```

替换整段文字

```

[root@localhost ~]# cat /etc/redhat-release 
CentOS Linux release 7.5.1804 (Core) 

[root@localhost ~]# cat /etc/redhat-release | tr '[:lower:]' '[:upper:]'
CENTOS LINUX RELEASE 7.5.1804 (CORE) 

```

```

[root@localhost ~]# echo "Netkiller" |  tr ‘a-z’ ‘A-Z’
NETKILLER			

```

#### 8.5.3. [CHAR*] 和 [CHAR*REPEAT]

```

[root@localhost ~]# echo "1234567890" | tr ‘1-5′ ‘[A*]‘ 
AAAAA67890

[root@localhost ~]# echo "1234567890" | tr ‘1-9′ ‘[A*5]BCDE’
AAAAABCDE0			

```

#### 8.5.4. -s, --squeeze-repeats replace each input sequence of a repeated character that is listed in SET1 with a single occurrence of that character

删除重复的字符

```

[root@localhost ~]# echo "My      nickname  is      netkiller." | tr -s ' '
My nickname is netkiller.	

[root@localhost ~]# echo "aaaabbbbccccdddd." | tr -s 'a'
abbbbccccdddd.

[root@localhost ~]# echo "aaaabbbbccccdddd." | tr -s 'a-z'
abcd.		

```

#### 8.5.5. -d, --delete delete characters in SET1, do not translate

删除字符

```

[root@localhost ~]# echo "My nickname is netkiller" | tr -d ' '
Mynicknameisnetkiller

[root@localhost ~]# md5sum /etc/issue | tr -d [0-9] 
ffedfcfbdcaebdec  /etc/issue			

```

### 8.6. cut - remove sections from each line of files

列操作

```
$ last | grep  'neo' | cut -d ' ' -f1

```

```
$ cat /etc/passwd | cut -d ':' -f1
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy

$ cat /etc/passwd | cut -d ':' -f1,3,4

# cat /etc/passwd | cut -d ':' -f1,6
root:/root
bin:/bin
daemon:/sbin
adm:/var/adm
lp:/var/spool/lpd
sync:/sbin
shutdown:/sbin
halt:/sbin
mail:/var/spool/mail
uucp:/var/spool/uucp
operator:/root
games:/usr/games
gopher:/var/gopher
ftp:/var/ftp
nobody:/
vcsa:/dev
saslauth:/var/empty/saslauth
postfix:/var/spool/postfix
sshd:/var/empty/sshd
rpc:/var/cache/rpcbind
rpcuser:/var/lib/nfs
nfsnobody:/var/lib/nfs
ntp:/etc/ntp
nagios:/var/log/nagios

```

行操作

```
$ cat /etc/passwd | cut -c 1-4
root
daem
bin:
sys:
sync
game
man:

$ echo "No such file or directory"| cut -c4-7
such

$ echo "No such file or directory"| cut -c -8
No such

$ echo "No such file or directory"| cut -c-8
No such

```

### 8.7. printf - format and print data

```
printf "%d\n" 1234

```

```
$ printf "\033[1;33m TEST COLOR \n\033[m"

```

### 8.8. Free `recode' converts files between various character sets and surfaces.

Following will convert text files between DOS, Mac, and Unix line ending styles:

```

$ recode /cl../cr <dos.txt >mac.txt
$ recode /cr.. <mac.txt >unix.txt
$ recode ../cl <unix.txt >dos.txt

```

### 8.9. /dev/urandom 随机字符串

```

[neo@test .deploy]$ echo `< /dev/urandom tr -dc A-Z-a-z-0-9 | head -c 8`
GidAuuNN
[neo@test .deploy]$ echo `< /dev/urandom tr -dc A-Z-a-z-0-9 | head -c 8`
UyGaWSKr

```

我常常使用这样的随机字符初始化密码

```

[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:alnum:] | head -c 8`
xig8Meym
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:alnum:] | head -c 8`
23Ac1vZg
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:digit:] | head -c 8`
73652314
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:graph:] | head -c 8`
GO_o>OnJ
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:graph:] | head -c 10`
iGy0FS/aO5
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:graph:] | head -c 50`
;`E^{5(T4v~5$YovW.?%_?9la<`+qPcRh@7mD\!Whx;MJZVQ\K
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:print:] | head -c 50`
fy$[#:'(')jt'gp1/g-)d~p]8 :r9i;MO2d!8M<?Qs3t:QgK$O
[neo@test .deploy]$ echo `< /dev/urandom tr -dc [:graph:] | head -c 50`
6SivJ5y$/FTi8mf}rrqE&s0"WkA}r;uK-=MT!Wp0UlL_lF0|bL

```

批量生成

```

for i in {1..10}
do
echo `< /dev/urandom tr -dc A-Z-a-z-0-9 | head -c 8`
done

```

```

# cat /dev/urandom | tr -cd [:alnum:] | fold -w30 | head -n 20
AVqROzjF6ZATJGv2J6PzDHp3jLpKV4
ONt68UFNDwgXpSnLBV7oRDX3VLRYsX
EZTWCGvZc3mIEeuw9sxMtV8ZkzVRJv
BhUiv0a7utsjZFLYpKGZrY5aDXcZL4
5YfUl2hmDT1O9X61DRYg4wSp4lXoXX
ykyPJxH47PzxnNGlujIUF98ZtB01H0
QyP53mksQN8bCNNo1fSD3RtqhhEGfa
u2RkT1M9GUQF4a6O18tG5WD97OOXze
Whm5X7398Q8L9BONN8k2oLy9CL37JO
TmGQz7WB6WnkjhyB4wrBHBJ3HMIRyf
hww43yvddUDYUnbNOKjhv3sLhCA4YD
uY6zQtBC6miwLUl3jkCVVA0Xu8ASgj
jv58qu46VW7LvRIq4txNE8bG9NBlZl
pzaMkydAiCHCF5H2oQVqMn4DTTYgNL
yoN2A9LyrCwLfjP1ad9HMAwxExJL5i
J27iy2L90m9dpcPLJ8tl46GGb9xqmQ
6YwFCvuPHyyEwnctUTpqLFcvUafVZ2
Nuq9XgIgRQGynjlVqGLMOpO0MkGpsn
tChkRG7eoRuKVXgW7ccTGx45E54K3Y
qPv48XqdGlOrdULCOGZ45kwJ1v5kVX		

```

### 8.10. col - filter reverse line feeds from input

清除 ^M 字符

```
$ cat oldfile | col -b > newfile

```

### 8.11. apg - generates several random passwords

```
sudo apt-get install apg

$ apg

Please enter some random data (only first 16 are significant)
(eg. your old password):>
imlogNukcel5 (im-log-Nuk-cel-FIVE)
Drocdaf1 (Droc-daf-ONE)
fagJook0 (fag-Jook-ZERO)
heabugJer4 (heab-ug-Jer-FOUR)
5OsEsudy (FIVE-Os-Es-ud-y)
IrjOgneagOc9 (Irj-Og-neag-Oc-NINE)

$ apg -M SNCL -m 16
WoidWemFut6dryn,
byRowpEus-Flutt0
|QuogCagFaycsic0
ojHoadCyct4Freg_
Vir9blir`orhohoo
bapOip?Ibreawov2

```

### 8.12. head/tail

```
head -c 17 | tail -c 1

```

### 8.13. 反转字符串或文件内容

#### rev - reverse lines of a file or files

反转字符串

```
# echo hello | rev
olleh

# echo "hello world" | rev
dlrow olleh

```

反转文件内容

```
# rev /etc/passwd
hsab/nib/:toor/:toor:0:0:x:toor
nigolon/nibs/:nib/:nib:1:1:x:nib
nigolon/nibs/:nibs/:nomead:2:2:x:nomead
nigolon/nibs/:mda/rav/:mda:4:3:x:mda
nigolon/nibs/:dpl/loops/rav/:pl:7:4:x:pl
cnys/nib/:nibs/:cnys:0:5:x:cnys
nwodtuhs/nibs/:nibs/:nwodtuhs:0:6:x:nwodtuhs
tlah/nibs/:nibs/:tlah:0:7:x:tlah
nigolon/nibs/:liam/loops/rav/:liam:21:8:x:liam
nigolon/nibs/:pcuu/loops/rav/:pcuu:41:01:x:pcuu
nigolon/nibs/:toor/:rotarepo:0:11:x:rotarepo
nigolon/nibs/:semag/rsu/:semag:001:21:x:semag
nigolon/nibs/:rehpog/rav/:rehpog:03:31:x:rehpog
nigolon/nibs/:ptf/rav/:resU PTF:05:41:x:ptf
nigolon/nibs/:/:ydoboN:99:99:x:ydobon
nigolon/nibs/:ved/:renwo yromem elosnoc lautriv:96:96:x:ascv
nigolon/nibs/:ptn/cte/::83:83:x:ptn
nigolon/nibs/:htualsas/ytpme/rav/:"resu dhtualsaS":67:994:x:htualsas
nigolon/nibs/:xiftsop/loops/rav/::98:98:x:xiftsop
nigolon/nibs/:dhss/ytpme/rav/:HSS detarapes-egelivirP:47:47:x:dhss
hsab/nib/:lqsym/bil/rav/:revres LQSyM:994:894:x:lqsym
hsab/nib/:www/:noitacilppA beW:08:08:x:www
nigolon/nibs/:xnign/ehcac/rav/:resu xnign:894:794:x:xnign

```

### 8.14. TAB 符号与空格处理

#### 8.14.1. expand - convert tabs to spaces

##### 转换 TAB 字符为空格

```
root@netkiller /var/log % yum --showduplicates list httpd | expand
Repository epel is listed more than once in the configuration
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
Available Packages
httpd.x86_64                    2.4.6-67.el7.centos                      os     
httpd.x86_64                    2.4.6-67.el7.centos.2                    updates

```

#### 8.14.2. unexpand - convert spaces to tabs

##### 转换空格为 TAB 符

```
root@netkiller /var/log % cat /etc/fstab | unexpand -t 16
/dev/vda1	     /	          ext3	     noatime,acl,user_xattr 1 1
proc	     /proc	          proc	     defaults	           0 0
sysfs	     /sys	          sysfs	     noauto	           0 0
debugfs	     /sys/kernel/debug    debugfs    noauto	           0 0
devpts	     /dev/pts	          devpts     mode=0620,gid=5       0 0

```

将 16 个空格替换为一个 TAB 符

## 9. grep, egrep, fgrep, rgrep - print lines matching a pattern

### 9.1. 删除空行

```
$ cat file | grep '.'

```

### 9.2. -v, --invert-match

grep -v "grep"

```
[root@development ~]# ps ax | grep httpd
 6284 ?        Ss     0:10 /usr/local/httpd-2.2.14/bin/httpd -k start
 8372 ?        S      0:00 perl ./wrapper.pl -chdir -name httpd -class com.caucho.server.resin.Resin restart
19136 ?        S      0:00 /usr/local/httpd-2.2.14/bin/httpd -k start
19749 pts/1    R+     0:00 grep httpd
31530 ?        Sl     0:57 /usr/local/httpd-2.2.14/bin/httpd -k start
31560 ?        Sl     1:12 /usr/local/httpd-2.2.14/bin/httpd -k start
31623 ?        Sl     1:06 /usr/local/httpd-2.2.14/bin/httpd -k start
[root@development ~]# ps ax | grep httpd | grep -v grep
 6284 ?        Ss     0:10 /usr/local/httpd-2.2.14/bin/httpd -k start
 8372 ?        S      0:00 perl ./wrapper.pl -chdir -name httpd -class com.caucho.server.resin.Resin restart
19136 ?        S      0:00 /usr/local/httpd-2.2.14/bin/httpd -k start
31530 ?        Sl     0:57 /usr/local/httpd-2.2.14/bin/httpd -k start
31560 ?        Sl     1:12 /usr/local/httpd-2.2.14/bin/httpd -k start
31623 ?        Sl     1:06 /usr/local/httpd-2.2.14/bin/httpd -k start

```

### 9.3. Output control

#### 9.3.1. -o, --only-matching show only the part of a line matching PATTERN

```

$ curl -s http://www.example.com | egrep -o '<a href="(.*)">.*</a>' | sed -e 's/.*href="\([^"]*\)".*/\1/'

```

```
$ mysqlshow | egrep -o "|\w(.*)\w|"
Databases
information_schema
test

```

```

$ cat file.html | grep -o \
    -E '\b(([\w-]+://?|www[.])[^\s()<>]+(?:\([\w\d]+\)|([^[:punct:]\s]|/)))'

$ cat file.html | grep -o -E 'href="([^"#]+)"'

$ cat sss.html | grep -o -E 'thunder://([^<]+)'

```

```

neo@MacBook-Pro ~/project % cat WikiTest.java  | grep '@Api'
    @Api(method = GET, uri = "/project/:projectName/wikis/page")
    @Api(method = POST, uri = "/project/:projectName/wiki")
    @Api(method = POST, uri = "/project/:projectName/wiki")
    @Api(method = POST, uri = "/project/:projectName/wiki")

neo@MacBook-Pro ~/project % cat WikiTest.java  | egrep -o 'method\s=\s.+,\suri\s=\s.+"'
method = GET, uri = "/project/:projectName/wikis/page"
method = POST, uri = "/project/:projectName/wiki"
method = POST, uri = "/project/:projectName/wiki"
method = POST, uri = "/project/:projectName/wiki"

```

##### 9.3.1.1. IP 地址

```
# grep rhost /var/log/secure | grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"

```

##### 9.3.1.2. UUID

```

neo@MacBook-Pro ~ % curl -s -X POST --user 'api:secret' -d 'grant_type=password&username=netkiller@msn.com&password=123456' http://localhost:8080/oauth/token | grep -o -E '"access_token":"([0-9a-f-]+)"'
"access_token":"863ef5df-6448-40a6-8809-f6f4b680689b"

```

##### 9.3.1.3. 行列转换

```

$ grep -o . <<< "Helloworld"
H
e
l
l
o
w
o
r
l
d

```

#### 9.3.2. 递归操作

递归查询

```
$ sudo grep -r 'neo' /etc/*

```

递归替换

```

for file in $( grep -rl '8800.org' *  | grep -v .svn ); do
    echo item: $file
	[ -f $file ] && sed -e 's/8800\.org/sf\.net/g' -e 's/netkiller/neo/g' $file >$file.bak; cp $file.bak $file;
done

```

#### 9.3.3. -c, --count print only a count of matching lines per FILE

```
$ cat /etc/resolv.conf
nameserver localhost
nameserver 208.67.222.222
nameserver 208.67.220.220
nameserver 202.96.128.166
nameserver 202.96.134.133
$ grep -c nameserver /etc/resolv.conf
5

```

```
# grep -c GET /www/logs/access.log
188460

# grep -c POST /www/logs/access.log
421

```

### 9.4. Context control

#### 9.4.1. -A, --after-context=NUM print NUM lines of trailing context

返回匹配当前行至下面 N 行

```
# grep -A1 game /etc/passwd
games:x:12:100:games:/usr/games:/sbin/nologin
gopher:x:13:30:gopher:/var/gopher:/sbin/nologin

# grep -A2 game /etc/passwd
games:x:12:100:games:/usr/games:/sbin/nologin
gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin

```

#### 9.4.2. -B, --before-context=NUM print NUM lines of leading context

返回匹配当前行至上面 N 行

```
# grep -B1 game /etc/passwd
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin

# grep -B2 game /etc/passwd
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin

```

#### 9.4.3. -C, --context=NUM print NUM lines of output context

##### -NUM same as --context=NUM

```
neo@neo-OptiPlex-380:~$ grep -C 1 new /etc/passwd
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh

neo@neo-OptiPlex-380:~$ grep -C 5 new /etc/passwd
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh

# grep -3 game /etc/passwd
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
gopher:x:13:30:gopher:/var/gopher:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin

```

#### 9.4.4. --color

```
# grep --color root  /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

```

可以通过 alias 别名启用--color 选项

```
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'

```

加入.bashrc 中，每次用户登录将自动生效

```

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

```

### 9.5. Regexp selection and interpretation

n 开头

```
$ grep '^n' /etc/passwd
news:x:9:9:news:/var/spool/news:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
neo:x:1000:1000:neo chan,,,:/home/neo:/bin/bash
nagios:x:116:127::/var/run/nagios2:/bin/false

```

bash 结尾

```
$ grep 'bash$' /etc/passwd
root:x:0:0:root:/root:/bin/bash
neo:x:1000:1000:neo chan,,,:/home/neo:/bin/bash
postgres:x:114:124:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
cvsroot:x:1001:1001:cvsroot,,,,:/home/cvsroot:/bin/bash
svnroot:x:1002:1002:subversion,,,,:/home/svnroot:/bin/bash

```

中间包含 root

```
$ grep '.*root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
cvsroot:x:1001:1001:cvsroot,,,,:/home/cvsroot:/bin/bash
svnroot:x:1002:1002:subversion,,,,:/home/svnroot:/bin/bash

```

#### 9.5.1. .*

```

$ curl -s http://www.example.com | egrep -o '<a href=(.*)>.*</a>'

```

#### 9.5.2. 2010:(13|14|15|16)

regular 匹配一组

```
egrep "2010:(13|14|15|16)"  access.2010-11-18.log > apache.log

```

```
ps ax |grep -E "mysqld|httpd|resin"

```

#### 9.5.3. []与{}

源文件

```
# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Sat Sep 10 00:25:46 2011
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=091f295e-ea6d-4f57-9314-e2333f7ebff7 /                       ext4    defaults        1 1
UUID=b3661a0b-2c50-4e18-8030-be2d043cbfc4 /www                    ext4    defaults        1 2
UUID=4d3468de-a2ac-451c-b693-3bdca8832096 swap                    swap    defaults        0 0
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0

```

匹配每行包含 4 个连续字符的字符串的行。

```
# grep '[A-Z]\{4\}' /etc/fstab
UUID=091f295e-ea6d-4f57-9314-e2333f7ebff7 /                       ext4    defaults        1 1
UUID=b3661a0b-2c50-4e18-8030-be2d043cbfc4 /www                    ext4    defaults        1 2
UUID=4d3468de-a2ac-451c-b693-3bdca8832096 swap                    swap    defaults        0 0

```

#### 9.5.4. -P, --perl-regexp Perl 正则表达式

Interpret PATTERN as a Perl regular expression. This is highly experimental and grep -P may warn of unimplemented features.

```
[neo@netkiller nginx]$ grep -Po '\w+\.js' www.netkiller.cn.access.log
index.js
min.js
min.js
mCustomScrollbar.js
min.js
ajax_gd.js
ajax.js
validation.js
AC_RunActiveContent.js
WdatePicker.js
cookie.js
msg_modal.js
all.js
common.js
commonjs.js
swfobject.js
dateutil.js
form.js
live800.js
lang.js
cycle2.js
min.js
carousel.js
tabify.js
image.js
min.js
ctrl.js
packed.js
min.js
common.js

```

### 9.6. fgrep

^M 处理

```
fgrep -rl `echo -ne '\r'` .

find . -type f -exec grep $'\r' {} +

```

### 9.7. egrep

egrep = grep -E 在 egrep 中不许看使用转意字符，例如

```
# grep '\(oo\).*\1' /etc/passwd
root:x:0:0:root:/root:/bin/bash

# grep -E '(oo).*\1' /etc/passwd
root:x:0:0:root:/root:/bin/bash

# egrep '(oo).*\1' /etc/passwd
root:x:0:0:root:/root:/bin/bash

```

```
$ snmpwalk -v2c -c public 172.16.1.254 | egrep -i 'if(in|out)'

for pid in $(ps -axf |grep 'php-cgi' | egrep egrep "0:00.(6|7|8|9)"'{print $1}'); do kill -9 $pid; done

for pid in $(ps -axf |grep 'php-cgi' | egrep "0:(0|1|2|3|4|5)0.(6|7|8|9)" |awk '{print $1}'); do kill -9 $pid; done

```

## 10. sort - sort lines of text files

```
$ du -s * | sort -k1,1rn

```

```
$ rpm -q -a --qf '%10{SIZE}\t%{NAME}\n' | sort -k1,1n
$ dpkg-query -W -f='${Installed-Size;10}\t${Package}\n' | sort -k1,1n

```

### 10.1. 对列排序

sort -k 具体说来, 你可以使用 -k1,1 来对第一列排序, -k1 来对全行排序

```
# sort -t ':' -k 1 /etc/passwd

```

```
ort -n -t ‘ ‘ -k 2 file.txt

```

多列排序

```
$ sort -n -t ‘ ‘ -k 2 -k 3 file.txt

```

### 10.2. -s, --stable stabilize sort by disabling last-resort comparison

例如: 如果你要想对两例排序, 先是以第二列, 然后再以第一列, 那么你可以这样. sort -s 会很有用

```
 sort -k1,1 | sort -s -k2,2		

```

## 11. uniq

```
history | cut -c 8- |sort -r | uniq -u

```

```
# netstat -ant|fgrep ":"|cut -b 77-90|sort |uniq -c
      1 CLOSE_WAIT
      1 CLOSING
     88 ESTABLISHED
      7 FIN_WAIT1
      7 FIN_WAIT2
      3 LAST_ACK
      4 LISTEN
      1 SYN_RECV
      1 SYN_SENT
    177 TIME_WAIT

```

## 12. 表格操作/行列转换

### 12.1. column - columnate lists

列格式化

下面举一个例子 ，mount 执行结果

```
[www@netkiller www.netkiller.cn]$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
devtmpfs on /dev type devtmpfs (rw,nosuid,size=1931400k,nr_inodes=482850,mode=755)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,mode=755)
tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755)
cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,nosuid,nodev,noexec,relatime,perf_event)
cgroup on /sys/fs/cgroup/cpu,cpuacct type cgroup (rw,nosuid,nodev,noexec,relatime,cpuacct,cpu)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,nosuid,nodev,noexec,relatime,devices)
cgroup on /sys/fs/cgroup/net_cls type cgroup (rw,nosuid,nodev,noexec,relatime,net_cls)
cgroup on /sys/fs/cgroup/blkio type cgroup (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup on /sys/fs/cgroup/memory type cgroup (rw,nosuid,nodev,noexec,relatime,memory)
cgroup on /sys/fs/cgroup/cpuset type cgroup (rw,nosuid,nodev,noexec,relatime,cpuset)
configfs on /sys/kernel/config type configfs (rw,relatime)
/dev/xvda1 on / type ext4 (rw,relatime,nobarrier,data=ordered)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=33,pgrp=1,timeout=300,minproto=5,maxproto=5,direct)
mqueue on /dev/mqueue type mqueue (rw,relatime)
debugfs on /sys/kernel/debug type debugfs (rw,relatime)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime)
/dev/xvdb1 on /opt type btrfs (rw,relatime,ssd,space_cache)
binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,relatime)
none on /proc/xen type xenfs (rw,relatime)
tmpfs on /run/user/0 type tmpfs (rw,nosuid,nodev,relatime,size=361892k,mode=700)
/dev/xvdb1 on /var/ftp type btrfs (rw,relatime,ssd,space_cache)

```

使用 column 格式化后

```
[www@netkiller www.netkiller.cn]$ mount | column -t 
sysfs        on  /sys                        type  sysfs        (rw,nosuid,nodev,noexec,relatime)
proc         on  /proc                       type  proc         (rw,nosuid,nodev,noexec,relatime)
devtmpfs     on  /dev                        type  devtmpfs     (rw,nosuid,size=1931400k,nr_inodes=482850,mode=755)
securityfs   on  /sys/kernel/security        type  securityfs   (rw,nosuid,nodev,noexec,relatime)
tmpfs        on  /dev/shm                    type  tmpfs        (rw,nosuid,nodev)
devpts       on  /dev/pts                    type  devpts       (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs        on  /run                        type  tmpfs        (rw,nosuid,nodev,mode=755)
tmpfs        on  /sys/fs/cgroup              type  tmpfs        (ro,nosuid,nodev,noexec,mode=755)
cgroup       on  /sys/fs/cgroup/systemd      type  cgroup       (rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd)
pstore       on  /sys/fs/pstore              type  pstore       (rw,nosuid,nodev,noexec,relatime)
cgroup       on  /sys/fs/cgroup/perf_event   type  cgroup       (rw,nosuid,nodev,noexec,relatime,perf_event)
cgroup       on  /sys/fs/cgroup/cpu,cpuacct  type  cgroup       (rw,nosuid,nodev,noexec,relatime,cpuacct,cpu)
cgroup       on  /sys/fs/cgroup/devices      type  cgroup       (rw,nosuid,nodev,noexec,relatime,devices)
cgroup       on  /sys/fs/cgroup/net_cls      type  cgroup       (rw,nosuid,nodev,noexec,relatime,net_cls)
cgroup       on  /sys/fs/cgroup/blkio        type  cgroup       (rw,nosuid,nodev,noexec,relatime,blkio)
cgroup       on  /sys/fs/cgroup/hugetlb      type  cgroup       (rw,nosuid,nodev,noexec,relatime,hugetlb)
cgroup       on  /sys/fs/cgroup/freezer      type  cgroup       (rw,nosuid,nodev,noexec,relatime,freezer)
cgroup       on  /sys/fs/cgroup/memory       type  cgroup       (rw,nosuid,nodev,noexec,relatime,memory)
cgroup       on  /sys/fs/cgroup/cpuset       type  cgroup       (rw,nosuid,nodev,noexec,relatime,cpuset)
configfs     on  /sys/kernel/config          type  configfs     (rw,relatime)
/dev/xvda1   on  /                           type  ext4         (rw,relatime,nobarrier,data=ordered)
systemd-1    on  /proc/sys/fs/binfmt_misc    type  autofs       (rw,relatime,fd=33,pgrp=1,timeout=300,minproto=5,maxproto=5,direct)
mqueue       on  /dev/mqueue                 type  mqueue       (rw,relatime)
debugfs      on  /sys/kernel/debug           type  debugfs      (rw,relatime)
hugetlbfs    on  /dev/hugepages              type  hugetlbfs    (rw,relatime)
/dev/xvdb1   on  /opt                        type  btrfs        (rw,relatime,ssd,space_cache)
binfmt_misc  on  /proc/sys/fs/binfmt_misc    type  binfmt_misc  (rw,relatime)
none         on  /proc/xen                   type  xenfs        (rw,relatime)
tmpfs        on  /run/user/0                 type  tmpfs        (rw,nosuid,nodev,relatime,size=361892k,mode=700)
/dev/xvdb1   on  /var/ftp                    type  btrfs        (rw,relatime,ssd,space_cache)

```

```
$ (printf "PERM LINKS OWNER GROUP SIZE MONTH DAY HH:MM/YEAR NAME\n" ; ls -l | sed 1d) | column -t

$ cat /etc/passwd |tr ':' ' ' | column -t

$ cat /etc/passwd |tr ':' ' ' | column -t | colrm  20 20

```

### 12.2. paste - merge lines of files

```
# vim test
aaaaa   bbbbb   ccccc   ddddd
1111    2222    3333    444

# paste -s test
aaaaa   bbbbb   ccccc   ddddd   1111    2222    3333    444

```

### 12.3. join

join 命令就是一个根据关键字合并数据文件的命令(join lines of two files on a common field),类似于数据库中两张表关联查询.

```

内连接（inner join）                            格式：join <FILE1> <FILE2>
左连接（left join, 左外连接, left outer join）   格式：join -a1 <FILE1> <FILE2>
右连接（right join, 右外连接,right outer join）  格式：join -a2 <FILE1> <FILE2>
全连接（full join, 全外连接, full outer join）   格式：join -a1 -a2 <FILE1> <FILE2>

// 注意 使用 join 来合并两个文件的数据行时, 这两个文件必须要被正确排序.

1) 差集
[root@test23 ~]# cat a.txt | sort > file1; cat b.txt | sort > file2; join  -v 1   file1 file2
1.1.1.1
3.3.3.3
[root@test23 ~]# cat a.txt | sort > file1; cat b.txt | sort > file2; join  -v 2   file1 file2
4.4.4.4
a.b.c.d

2) 并集
[root@test23 ~]# cat a.txt | sort > file1; cat b.txt | sort > file2; join  -a1 -a2   file1 file2
1.1.1.1
1.2.3.4
2.2.2.2
3.3.3.3
4.4.4.4
a.b.c.d

3) 交集
// 不指定任何参数的情况下使用 join 命令,相当于数据库中的内连接,(关键字,默认用第一列[使用空格作为分割符]作为关键字)不匹配的行不会输出.
[root@test23 ~]# cat a.txt | sort > file1; cat b.txt | sort > file2; join     file1 file2
1.2.3.4
2.2.2.2

join 其他用法

-t <CHAR> 指定分隔符,比如: -t ':' 使用冒号作为分隔符,默认的分隔符是空白.

-o <FILENO.FIELDNO> ... 指定输出字段 其中 FILENO=1 表示第一个文件,FILENO=2 表示第二个文件,FIELDNO 表示字段序号,从 1 开始编号.默认会全部输出,但关键字列只输出一次.

```

## 13. standard input/output

### 13.1. xargs - build and execute command lines from standard input

xargs 命令是 给其他命令传递参数的一个过滤器,也是组合多个命令的一个工具 它擅长将标准输入数据转换成命令行参数,xargs 能够处理管道或者 stdin 并将其转换成特定命令的命令参数. xargs 也可以将单行或多行文本输入转换为其他格式,例如多行变单行,单行变多行. xargs 的默认命令是 echo,空格是默认定界符;这意味着通过管道传递给 xargs 的输入将会包含换行和空白,不过通过 xargs 的处理,换行和空白将被空格取代.

xargs 命令用法

#### 13.1.1. 格式化

xargs 用作替换工具，读取输入数据重新格式化后输出。

```

定义一个测试文件，内有多行文本数据：

cat >> test.txt <<EOF

a b c d e f g
h i j k l m n
o p q
r s t
u v w x y z

EOF

# cat test.txt 

a b c d e f g
h i j k l m n
o p q
r s t
u v w x y z

多行输入一行输出：

# cat test.txt | xargs
a b c d e f g h i j k l m n o p q r s t u v w x y z

等效
# cat test.txt | tr "\n" " "
a b c d e f g h i j k l m n o p q r s t u v w x y z

```

#### 13.1.2. standard input

```

# xargs < test.txt 
a b c d e f g h i j k l m n o p q r s t u v w x y z		

# cat /etc/passwd | cut -d : -f1 > users
# xargs -n1 < users echo "Your name is"
Your name is root
Your name is bin
Your name is daemon
Your name is adm
Your name is lp
Your name is sync
Your name is shutdown
Your name is halt
Your name is mail
Your name is operator
Your name is games
Your name is ftp
Your name is nobody
Your name is dbus
Your name is polkitd
Your name is avahi
Your name is avahi-autoipd
Your name is postfix
Your name is sshd
Your name is neo
Your name is ntp
Your name is opendkim
Your name is netkiller
Your name is tcpdump		

```

#### 13.1.3. -I 替换操作

-I R same as --replace=R

```
复制所有图片文件到 /data/images 目录下：

ls *.jpg | xargs -n1 -I cp {} /data/images

```

```

读取 stdin，将格式化后的参数传递给命令 xargs 的一个选项-I，使用-I 指定一个替换字符串{}，这个字符串在 xargs 扩展时会被替换掉，当-I 与 xargs 结合使用，每一个参数命令都会被执行一次：

# echo "name=Neo|age=30|sex=T|birthday=1980" | xargs -d"|" -n1 | xargs -I {} echo "select * from tab where {} "
select * from tab where name=Neo 
select * from tab where age=30 
select * from tab where sex=T 
select * from tab where birthday=1980 

# xargs -I user echo "Hello user" <users 
Hello root
Hello bin
Hello daemon
Hello adm
Hello lp
Hello sync
Hello shutdown
Hello halt
Hello mail
Hello operator
Hello games
Hello ftp
Hello nobody
Hello dbus
Hello polkitd
Hello avahi
Hello avahi-autoipd
Hello postfix
Hello sshd
Hello netkiller
Hello neo
Hello tss
Hello ntp
Hello opendkim
Hello noreply
Hello tcpdump

```

```
-I 使用-I 指定一个替换字符串,这个字符串在 xargs 扩展时会被替换掉,当-I 与 xargs 结合使用,每一个参数命令都会被执行一次.
mysql -u root -predhat -s -e "show databases" | egrep "^mt4_user_equity_" | xargs -I "@@" mysql -u root -predhat -e "DROP DATABASE \`@@\`;"  			

```

#### 13.1.4. -n, --max-args=MAX-ARGS use at most MAX-ARGS arguments per command line

-n 参数來指定每一次执行指令所使用的参数个数上限值.

```

-n 选项多行输出：

# cat test.txt | xargs -n3
a b c
d e f
g h i
j k l
m n o
p q r
s t u
v w x
y z
# cat test.txt | xargs -n4
a b c d
e f g h
i j k l
m n o p
q r s t
u v w x
y z
# cat test.txt | xargs -n5
a b c d e
f g h i j
k l m n o
p q r s t
u v w x y
z

[neo@netkiller test]# echo 'a b c d e 1 2 3 4 5' | xargs -n 5
a b c d e
1 2 3 4 5

```

#### 13.1.5. -t, --verbose print commands before executing them

-t 参数可以让 xargs 在执行指令之前先显示要执行的指令

```

[neo@netkiller test]# echo a b c d e f | xargs -t
/bin/echo a b c d e f
a b c d e f

```

#### 13.1.6. -d, --delimiter=CHARACTER items in input stream are separated by CHARACTER, not by whitespace; disables quote and backslash processing and logical EOF processing

-d 自定义一个定界符 默认是空格

```

[neo@netkiller test]# echo 'abc' | xargs -d b
a c

-d 选项可以自定义一个定界符：

# echo "name|age|sex|birthday" | xargs -d"|"
name age sex birthday

结合-n 选项使用：

# echo "name=Neo|age=30|sex=T|birthday=1980" | xargs -d"|" -n1
name=Neo
age=30
sex=T
birthday=1980	

```

#### 13.1.7. -0, --null items are separated by a null, not whitespace; disables quote and backslash processing and logical EOF processing

-0 是以 null 字符结尾的,而不是以白空格(whitespace)结尾的且引号和反斜杠,都不是特殊字符;

```

每个输入的字符,都视为普通字符禁止掉文件结束符,被视为别的参数.当输入项可能包含白空格,引号,反斜杠等情况时,才适合用此参数
[neo@netkiller test]# touch "Mr liu"
[neo@netkiller test]# ls M*
Mr liu
[neo@netkiller test]# find -type f -name "Mr*" | xargs rm -f
[neo@netkiller test]# ls M*
Mr liu
[neo@netkiller test]# find -type f -name "Mr*" | xargs -t rm  -f
rm -f ./Mr liu
// 这个时候我们可以将 find 指令加上 -print0 参数，另外将 xargs 指令加上 -0 参数，改成这样:
[neo@netkiller test]# find -type f -name "Mr*"  -print0| xargs -t -0 rm  -f
rm -f ./Mr liu
[neo@netkiller test]# ls M*
ls: 无法访问 M*: 没有那个文件或目录			

```

#### 13.1.8. -r, --no-run-if-empty if there are no arguments, then do not run COMMAND; if this option is not given, COMMAND will be

-r 如果标准输入不包含任何非空格，请不要运行该命令.

```

[neo@netkiller test]# echo a b c d e f | xargs -p -n 3
/bin/echo a b c ?...n
/bin/echo d e f ?...n
/bin/echo ?...n
//当我们使用 -p 参数时，如果所有的指令都输入 n 跳过不执行时候，最后还会出现一个沒有任何参数的 echo 指令,
如果想要避免以這种空字串作为参数来执行指令，可以加上 -r 参数
[neo@netkiller test]# echo a b c d e f | xargs -p -n 3 -r
/bin/echo a b c ?...n
/bin/echo d e f ?...n

```

#### 13.1.9. -p, --interactive prompt before running commands

-p 确认操作选项,具有可交互性:

```
-P 修改最大的进程数, 默认是 1.为 0 时候为 as many as it can.

```

## 14. flock - manage locks from shell scripts

```

### flock

    当多个进程可能会对同样的数据执行操作时,这些进程需要保证其它进程没有在操作,以免损坏数据.通常,这样的进程会使用一个“锁文件”,也就是建立一个文件来告诉别的进程自己在运行,如果检测到那个文件存在则认为有操作同样数据的进程在工作.
	这样的问题是,进程不小心意外死亡了,没有清理掉那个锁文件,那么只能由用户手动来清理了.

	flock 是对于整个文件的建议性锁;也就是说如果一个进程在一个文件(inode)上放了锁,那么其它进程是可以知道的,(建议性锁不强求进程遵守)最棒的一点是,它的第一个参数是文件描述符,在此文件描述符关闭时,锁会自动释放;而当进程终止时,所有的文件描述符均会被关闭.于是,很多时候就不用考虑解锁的事情.

flock 分为两种锁：
	一种是共享锁 使用-s 参数
	一种是独享锁 使用-x 参数

选项和参数：
-s  --shared：获取一个共享锁,在定向为某文件的 FD 上设置共享锁而未释放锁的时间内,其他进程试图在定向为此文件的 FD 上设置独占锁的请求失败,而其他进程试图在定向为此文件的 FD 上设置共享锁的请求会成功.
-x，-e，--exclusive：获取一个排它锁,或者称为写入锁,为默认项
-u，--unlock： 手动释放锁,一般情况不必须,当 FD 关闭时,系统会自动解锁,此参数用于脚本命令一部分需要异步执行,一部分可以同步执行的情况.
-n，--nb, --nonblock：非阻塞模式,当获取锁失败时,返回 1 而不是等待.
-w, --wait, --timeout seconds : 设置阻塞超时,当超过设置的秒数时,退出阻塞模式,返回 1,并继续执行后面的语句.
-o, --close : 表示当执行 command 前关闭设置锁的 FD,以使 command 的子进程不保持锁.
-c, --command command : 在 shell 中执行其后的语句.

<>打开${LOCK_FILE} (打开 LOCK_FILE 文件,与文件描述符 101 绑定),原因是定向文件描述符是先于命令执行的.因此假如在您要执行的语句段中需要读 LOCK_FILE 文件,例如想获得上一个脚本实例的 pid,并将此次的脚本实例的 pid 写入 LOCK_FILE ,此时直接用>打开 LOCK_FILE 会清空上次存入的内容,而用<打开 LOCK_FILE 当它不存在时会导致一个错误.

#### example
> ntp 

#!/bin/bash
#
#author junun
#description this script for start or stop check sever time from an ntp server every 1s
#please add in /etc/rc.local 
#
script_0=$0
script_name=${script_0##*/}
lockfile=/var/lock/subsys/$script_name
pidfile=/var/run/$script_name

start() {
    [ -f $lockfile ] && echo  -e "\033[31m$script_name is running...\033[0m" &&  exit 1
    while true ;do
        /usr/sbin/ntpdate clock.isc.org > /dev/null 2>&1
        echo $$ > $pidfile
        touch $lockfile
        sleep 1
    done
}

stop() {
    [ ! -f $lockfile ] && echo  -e "\033[31m$script_name is not running...\033[0m" &&  exit 1
    kill -TERM `cat $pidfile`
    rm -rf $lockfile
}

case "$1" in
    start)
        $1
        ;;
    stop)
        $1
        ;;
    *)
      echo $"Usage: $0 {start|stop}"
      exit 2
esac
exit $?

*/10 * * * * /usr/bin/flock -xn /var/run/check_time.lock -c '/usr/local/bin/monitor/check_time start &' > /dev/null 2>&1

>2 monitor

#!/bin/bash
#
#

SHELL_DIR=$(cd $(dirname $0);pwd)

LOCK_FILE=/dev/shm/`echo ${SHELL_DIR}|sed 's!/!.!g;s!.!!'`.monitor.lock

{
    flock -n 100 || { exit 2; }

    cd ${SHELL_DIR}
    function monitor() {
        while true;do
            ./run.sh monitor
            sleep 3
        done
    }

    monitor >> ../logs/monitor.log 2>&1 &

} 100<>${LOCK_FILE}

#!/bin/bash
#

ulimit -c unlimited
ulimit -u unlimited
ulimit -HSn 655350

SERVER_NAME='changed_order_deal'
SHELL_DIR=$(cd $(dirname $0);pwd)
BASE_DIR=$(cd $(dirname $0);cd ..;pwd)
SHELL_FILE="${SHELL_DIR}/run.sh"
SERVER_BIN=${SHELL_DIR}/${SERVER_NAME}
LOG_DIR=${BASE_DIR}/logs
PID_FILE=${LOG_DIR}/PID
CONF_FILE=${BASE_DIR}/conf/${SERVER_NAME}.conf
LOCK_FILE=/dev/shm/`echo ${SERVER_BIN}|sed 's!/!.!g;s!.!!'`.monitor.lock

start() {
    if [ ! -f "${SERVER_BIN}" ];then
        echo `date +"%F %T"` - ERROR - Can not find ${SERVER_BIN} ...
        exit 1
    fi

    PID=`/sbin/pidof ${SERVER_BIN}`
    if [ x"${PID}" == x"" ];then
        cd ${SHELL_DIR}
        mkdir -p ${LOG_DIR}
        nohup ${SERVER_BIN} -flagfile=${CONF_FILE} >> ${LOG_DIR}/${SERVER_NAME}.stdout.log 2>&1 &
        # place the following shell sentence right after the nohup statement
        /sbin/pidof ${SERVER_BIN} > ${PID_FILE}          #进程 pid 写入文件
        echo "`date +"%F %T"` - start ${SERVER_BIN} "
    else
        ps aux|grep pt_auth
        echo "`date +"%F %T"` - ERROR - PID:${PID} exist. ${SERVER_BIN} is already running."
    fi
}

stop() {
    PID=`cat ${PID_FILE}`
    if [ x"${PID}" == x"" ];then
        echo "`date +"%F %T"` - ERROR - ${SERVER_BIN} is not running..."
    else
        kill -15 $PID
        while true
        do
            if test $( ps aux | awk '{print $2}' | grep -w "$PID" | grep -v 'grep' | wc -l ) -eq 0;then
                echo "`date +"%F %T"` - SUCCESS - ${SERVER_BIN} has been stopped..."
                > ${PID_FILE}
                break
            else
                echo "`date +"%F %T"` - wait to stop..."
                sleep 1
            fi
        done
    fi
}

kill9() {
    PID=`cat ${PID_FILE}`
    if [ x"${PID}" == x"" ];then
        echo "`date +"%F %T"` - ERROR - ${SERVER_BIN} is not running..."
        exit 1
    else
        kill -9 $PID
    fi
}

restart() {
    stop
    start
}

monitor() {
    check_num=`ps ax -o pid,cmd|grep "$SERVER_BIN"|grep -v grep|wc -l`
    if [ $check_num -eq 0 ];then
        start
        echo `date +"%F %T"` - restart.
    fi
}

case "$1" in
    "start")
      start;
    ;;
    "stop")
      stop;
    ;;
    "restart")
      restart;
    ;;
    "kill9")
      kill9;
    ;;
    "monitor")
      monitor;
    ;;
  *)
    echo "Usage: $(basename "$0") start/stop/restart/kill9/monitor"
    exit 1
esac

* * * * * /srv/bin/monitor.sh  &> /dev/null

```

## 15. 进制转换 - 16 进制 - 8 进制 - 二进制

### 15.1. od - dump files in octal and other formats

#### 15.1.1. 16 进制

```

neo@netkiller ~ % echo "helloworld" | od -x
0000000      6568    6c6c    776f    726f    646c    000a                
0000013

neo@netkiller ~ % echo "helloworld" | od -x -An
             6568    6c6c    776f    726f    646c    000a  			

```

#### 15.1.2. 使用 od 随机生成密码

```

neo@netkiller ~ % od  -vN 32 -An -tx1 /dev/urandom | tr -d ' \n'
a6bf6dad8ed860a234046b66d550008f61c36e9cb2630c22d935dac5e20d7920			

```

### 15.2. hexdump, hd -- ASCII, decimal, hexadecimal, octal dump

#### 以十六进制方式显示二进制文件

```

neo@netkiller ~ % hexdump -n 256 -C ./coutput/HelloWorld.bin
00000000  36 30 36 30 36 30 34 30  35 32 33 34 31 35 36 31  |6060604052341561|
00000010  30 30 30 66 35 37 36 30  30 30 38 30 66 64 35 62  |000f57600080fd5b|
00000020  36 31 30 32 65 33 38 30  36 31 30 30 31 65 36 30  |6102e38061001e60|
00000030  30 30 33 39 36 30 30 30  66 33 30 30 36 30 36 30  |00396000f3006060|
00000040  36 30 34 30 35 32 36 30  30 34 33 36 31 30 36 31  |6040526004361061|
00000050  30 30 34 63 35 37 36 30  30 30 33 35 37 63 30 31  |004c576000357c01|
00000060  30 30 30 30 30 30 30 30  30 30 30 30 30 30 30 30  |0000000000000000|
*
00000090  30 30 30 30 30 30 30 30  39 30 30 34 36 33 66 66  |00000000900463ff|
000000a0  66 66 66 66 66 66 31 36  38 30 36 33 34 65 64 33  |ffffff1680634ed3|
000000b0  38 38 35 65 31 34 36 31  30 30 35 31 35 37 38 30  |885e146100515780|
000000c0  36 33 36 64 34 63 65 36  33 63 31 34 36 31 30 30  |636d4ce63c146100|
000000d0  61 65 35 37 35 62 36 30  30 30 38 30 66 64 35 62  |ae575b600080fd5b|
000000e0  33 34 31 35 36 31 30 30  35 63 35 37 36 30 30 30  |341561005c576000|
000000f0  38 30 66 64 35 62 36 31  30 30 61 63 36 30 30 34  |80fd5b6100ac6004|
00000100

```

### 15.3. xxd - make a hexdump or do the reverse.

```

neo@MacBook-Pro ~/workspace % xxd -b netkiller.dat 
00000000: 00000000 00000000 00000000 11111111 00000000 00000000  ......
00000006: 00000000 00000000 11111111 11111111 11111111 11111111  ......		

```

#### 15.3.1. 指定每行的列数

```

neo@MacBook-Pro ~ % xxd -c 2 -b netkiller.bin
00000000: 10010110 01001000  .H				

```

#### 15.3.2. 跳过字节

跳过两个字节，三列显示

```

neo@MacBook-Pro ~ % xxd -s 2 -c 3 -b netkiller.txt
00000002: 11101001 10011001 10001000  ...
00000005: 11100110 10011001 10101111  ...
00000008: 11100101 10110011 10110000  ...			

```

### 15.4. binutils

```

$ sudo apt-get install binutils

```

#### 15.4.1. strings - print the strings of printable characters in files.

```

tcpdump -i eth0 -s 0 -l -w - dst port 80 | strings

```

## 16. Logging

### 16.1. logger - a shell command interface to the syslog(3) system log module

```

# logger -p local0.notice -t HOSTIDM -f /dev/idmc
# tail /var/log/messages

# logger -p local0.notice -t passwd -f /etc/passwd
# tail /var/log/syslog

# logger -p user.notice -t neo -f /etc/passwd
# tail /var/log/syslog
# tail /var/log/messages

# logger -i -s -p local3.info -t passwd -f /etc/passwd
# tail /var/log/messages

```

## 17. Password

### 17.1. Shadow password suite configuration.

```
# cat /etc/login.defs
# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR      Maildir
MAIL_DIR        /var/spool/mail
#MAIL_FILE      .mail

# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_MIN_LEN    5
PASS_WARN_AGE   7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                   500
UID_MAX                 60000

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                   500
GID_MAX                 60000

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD    /usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is overridden with the -m flag on
# useradd command line.
#
CREATE_HOME     yes

# The permission mask is initialized to this value. If not specified,
# the permission mask will be initialized to 022.
UMASK           077

# This enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

# Use MD5 or DES to encrypt password? Red Hat use MD5 by default.
MD5_CRYPT_ENAB yes

ENCRYPT_METHOD MD5

```

### 17.2. newusers - update and create new users in batch

```
# cat userfile.txt
www00:x:520:520::/home/www00:/sbin/nologin
www01:x:521:521::/home/www01:/sbin/nologin
www02:x:522:522::/home/www02:/sbin/nologin
www03:x:523:523::/home/www03:/sbin/nologin
www04:x:524:524::/home/www04:/sbin/nologin
www05:x:525:525::/home/www05:/sbin/nologin
www06:x:526:526::/home/www06:/sbin/nologin
www07:x:527:527::/home/www07:/sbin/nologin
www08:x:528:528::/home/www08:/sbin/nologin
www09:x:529:529::/home/www09:/sbin/nologin

# newusers userfile.txt

```

### 17.3. chpasswd - update passwords in batch mode

**echo "user:password" | chpasswd**

```
[root@dev1 ~]# adduser test
[root@dev1 ~]# echo "test:123456" | chpasswd

```

```
# cat passwd.txt
neo:neopass
jam:jampass

# cat passwd.txt | chpasswd

```

```

#　chpasswd　-c　<　passwd.txt

```

passwd 命令实现相同功能

```
echo "mypasword" | passwd –stdin neo

```

### 17.4. sshpass - noninteractive ssh password provider

```

sudo apt install -y sshpass

root@ubuntu:~# sshpass -v
Usage: sshpass [-f|-d|-p|-e] [-hV] command parameters
   -f filename   Take password to use from file
   -d number     Use number as file descriptor for getting password
   -p password   Provide password as argument (security unwise)
   -e            Password is passed as env-var "SSHPASS"
   With no parameters - password will be taken from stdin

   -P prompt     Which string should sshpass search for to detect a password prompt
   -v            Be verbose about what you're doing
   -h            Show help (this screen)
   -V            Print version information
At most one of -f, -d, -p or -e should be used

```

```

sshpass -p Password scp target/*.jar root@dev.netkiller.cn:/root/		

```

```

sshpass -p Password ssh root@dev.netkiller.cn java -jar /root/java-0.0.1-SNAPSHOT.jar		

```

## 18. 信息摘要

### 18.1. cksum, sum -- display file checksums and block counts

```

neo@MacBook-Pro ~ % head -20 /dev/urandom | cksum | cut -f1 -d " "
1705222024		

```

### 18.2. md5sum - compute and check MD5 message digest

```

[root@localhost ~]# md5sum /etc/hosts
54fb6627dbaa37721048e4549db3224d  /etc/hosts

```

### 18.3. 

```

[root@localhost ~]# sha1sum /etc/hosts
7335999eb54c15c67566186bdfc46f64e0d5a1aa  /etc/hosts		

```

## 19. envsubst - substitutes environment variables in shell format strings

### 替代品在 shell 环境变量的格式字符串，类似模版替换操作

```

[root@localhost tmp]# echo "welcome $HOME ${USER:=a8m}" | envsubst
welcome /root root	

```

```

[root@localhost tmp]# cat config.template 
HOME=${HOME}
USER=${USER}

[root@localhost tmp]# envsubst < config.template > config.conf

[root@localhost tmp]# cat config.conf 
HOME=/root
USER=root

```

只替换 ${USER} 变量

```

[root@localhost tmp]# envsubst '${USER}' < config.template > config.conf
[root@localhost tmp]# cat config.conf 
HOME=${HOME}
USER=root	

```

模版变量

```

${var}				var 值( 与 $var 相同)
${var-$DEFAULT}		如果未设置 var，则将表达式计算为 $DEFAULT
${var:-$DEFAULT}	如果未设置 var 或者为空，则将表达式计算为 $DEFAULT
${var=$DEFAULT}		如果未设置 var，则将表达式计算为 $DEFAULT
${var:=$DEFAULT}	如果未设置 var 或者为空，则将表达式计算为 $DEFAULT
${var+$OTHER}		如果为 var，则将表达式计算为 $OTHER,，否则为空字符串
${var:+$OTHER}		如果为 var，则将表达式计算为 $OTHER,，否则为空字符串	

```

## 第 27 章 其他命令

## 第 28 章 Utility Programs

## 1. ed, red - text editor

行寻址

```

.	此选项对当前行寻址（缺省地址）。
number	此选项对第 number 行寻址。可以按逗号分隔的范围 (first,last) 对行寻址。0 代表缓冲区的开头（第一行之前）。
-number	此选项对当前行之前的第 number 行寻址。如果没有 number，则减号对紧跟在当前行之前的行寻址。
+number	此选项对当前行之后的第 number 行寻址。如果没有 number，则加号对紧跟在当前行之后的行寻址。
$	此选项对最后一行寻址。
,	此选项对第一至最后一行寻址，包括第一行和最后一行（与 1,$ 相同）。
;	此选项对当前行至最后一行寻址。
/pattern/	此选项对下一个包含与 pattern 匹配的文本的行寻址。
?pattern?	此选项对上一个包含与 pattern 匹配的文本的行寻址。	

```

命令描述

```

a	此命令在指定的地址之后追加文本。
c	此命令将指定的地址更改为给定的文本。
d	此命令删除指定地址处的行。
i	此命令在指定的地址之前插入文本。
q	此命令在将缓冲区保存到磁盘后终止程序并退出。
r file	此命令读取 filespec 的内容并将其插入指定的地址之后。
s/pattern/replacement/	此命令将匹配 pattern 的文本替换为指定地址中的 replacement 文本。
w file	此命令将指定的地址写到 file。如果没有 address，则此命令缺省使用整个缓冲区。

```

实例，删除 passwd 中的 neo 用户

```

ed -s passwd <<EOF
/neo/
d
wq
EOF

```

```

ed -s mfsmetalogger.cfg <<EOF
,s/^# //
wq
EOF		

```

删除尾随空格

```

$ cat -vet input.txt

This line has trailing blanks.    $
This line does not.$

$ (echo ',s/ *$//'; echo 'wq') | ed -s input.txt

$ cat -vet input.txt

This line has trailing blanks.$
This line does not.$

```

## 2. vim

### 2.1. 查找与替换

**s%/aaa/bbb/g**

```

Starting Nmap 5.21 ( http://nmap.org ) at 2012-02-02 17:03 CST
NSE: Script Scanning completed.
Nmap scan report for 10.10.1.1
Host is up (0.0072s latency).
The 1 scanned port on 10.10.1.1 is filtered

Nmap scan report for 10.10.1.2
Host is up (0.0064s latency).
The 1 scanned port on 10.10.1.2 is closed

Nmap scan report for 10.10.1.3
Host is up (0.0071s latency).
The 1 scanned port on 10.10.1.3 is closed

Nmap scan report for 10.10.1.4
Host is up (0.0072s latency).
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: Protocol: 10
| Version: 5.1.54-log
| Thread ID: 37337702
| Some Capabilities: Long Passwords, Connect with DB, Compress, ODBC, Transactions, Secure Connection
| Status: Autocommit
|_Salt: y0!QV;ekiN)"kx;\=Y+g

Nmap scan report for 10.10.1.5
Host is up (0.0081s latency).
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: Protocol: 10
| Version: 5.1.48-community-log
| Thread ID: 6655211
| Some Capabilities: Long Passwords, Connect with DB, Compress, ODBC, Transactions, Secure Connection
| Status: Autocommit
|_Salt: i3ap1?+UL^q>$5~=UqYJ

Nmap scan report for 10.10.1.6
Host is up (0.0073s latency).
The 1 scanned port on 10.10.1.6 is closed

Nmap scan report for www.example.com (10.10.1.7)
Host is up (0.0074s latency).
The 1 scanned port on www.example.com (10.10.1.7) is closed
		</screen>
		<para>删除 closed 上面 2 行</para>
		<screen>
:%s:.*\n.*\n.*closed$::g
:%s/\n\n\n//g			

```

### 2.2. 插入文件

当前光标处插入文件

```

:r /etc/passwd

```

第十行处插入文件

```

:10 r /etc/passwd

```

### 2.3. 批处理

test script

```

vim test.txt <<end > /dev/null 2>&1
:%s/neo/neo chen/g
:%s/hello/hello world/g
:wq
end

```

test.txt

```

begin
neo
test
hello
world
end

```

test result

```

$ ./test
$ cat test.txt
begin
neo chen
test
hello world
world
end
neo@netkiller:/tmp$

```

#### 2.3.1. vi 批处理

```

for i in file_list 
do 
vi $i <<-! 
:g/xxxx/s//XXXX/g 
:wq 
! 
done

```

### 2.4. line()

加入行号

```

:g/^/ s//\=line('.').' '/

```

### 2.5. set fileformat

加入行号

```

vim    set fileformat
执行 set fileformat  会返回当前文件的 format 类型 如:fileformat=dos
也可执行 set line	

```

## 3. awk

内置变量

```

ARGC               命令行参数个数
ARGV               命令行参数排列
ENVIRON            支持队列中系统环境变量的使用
FILENAME           awk 浏览的文件名
FNR                浏览文件的记录数
FS                 设置输入域分隔符，等价于命令行 -F 选项
NF                 浏览记录的域的个数
NR                 已读的记录数
OFS                输出域分隔符
ORS                输出记录分隔符
RS                 控制记录分隔符	

```

### 3.1. 处理列

```
# cat /etc/fstab | awk '{print $1}'

```

### 3.2. printf

```

%d 十进制有符号整数
%u 十进制无符号整数
%f 浮点数
%s 字符串
%c 单个字符
%p 指针的值
%e 指数形式的浮点数
%x, %X 无符号以十六进制表示的整数
%0 无符号以八进制表示的整数
%g 自动选择合适的表示法
\n 换行
\f 清屏并换页
\r 回车
\t Tab 符
\xhh 表示一个 ASCII 码用 16 进表示,其中 hh 是 1 到 2 个 16 进制数

说明:
(1). 可以在"%"和字母之间插进数字表示最大场宽。
例如: %3d 表示输出 3 位整型数, 不够 3 位右对齐。
%9.2f 表示输出场宽为 9 的浮点数, 其中小数位为 2, 整数位为 6,小数点占一位, 不够 9 位右对齐。
%8s 表示输出 8 个字符的字符串, 不够 8 个字符右对齐。
如果字符串的长度、或整型数位数超过说明的场宽, 将按其实际长度输出.但对浮点数, 若整数部分位数超过了说明的整数位宽度, 将按实际整数位输出;若小数部分位数超过了说明的小数位宽度, 则按说明的宽度以四舍五入输出.
另外, 若想在输出值前加一些 0, 就应在场宽项前加个 0。
例如: %04d 表示在输出一个小于 4 位的数值时, 将在前面补 0 使其总宽度为 4 位。
如果用浮点数表示字符或整型量的输出格式, 小数点后的数字代表最大宽度,小数点前的数字代表最小宽度。
例如: %6.9s 表示显示一个长度不小于 6 且不大于 9 的字符串。若大于 9, 则第 9 个字符以后的内容将被删除。

echo 1.7 > 2
awk '{printf ("%d\n",$1)} 2
1
awk '{printf ("%f\n",$1)}' 2
1.700000
awk '{printf ("%3.1f\n",$1)}' 2
1.7
awk '{printf ("%4.1f\n",$1)}' 2
1.7
awk '{printf ("%e\n",$1)}' 2

```

print 拼装 rm 命令实现，查找文件并删除

```
#!/bin/sh
LOCATE=/home/samba
find $LOCATE -name '*.eml'>log
find $LOCATE -name '*.nws'>>log
gawk '{print "rm -rf "$1}' log > rmfile
chmod 755 rmfile
./rmfile

```

### 3.3. Pattern(字符匹配)

```
输出包含（不包含）特定字符的行（sed 也可以完成该功能）：
:~$ awk '/[a-c]/ { print }' file.txt
daemon x 1 1 daemon /usr/sbin /bin/sh
bin x 2 2 bin /bin /bin/sh
sys x 3 3 sys /dev /bin/sh
sync x 4 65534 sync /bin /bin/sync
games x 5 60 games /usr/games /bin/sh
man x 6 12 man /var/cache/man /bin/sh
lp x 7 7 lp /var/spool/lpd /bin/sh
mail x 8 8 mail /var/mail /bin/sh
news x 9 9 news /var/spool/news /bin/sh
uucp x 10 10 uucp /var/spool/uucp /bin/sh
proxy x 13 13 proxy /bin /bin/sh
www-data x 33 33 www-data /var/www /bin/sh
backup x 34 34 backup /var/backups /bin/sh
list x 38 38 Mailing List Manager /var/list /bin/sh
irc x 39 39 ircd /var/run/ircd /bin/sh
gnats x 41 41 Gnats Bug-Reporting System (admin) /var/lib/gnats /bin/sh
nobody x 65534 65534 nobody /nonexistent /bin/sh
libuuid x 100 101  /var/lib/libuuid /bin/sh
syslog x 101 103  /home/syslog /bin/false
sshd x 102 65534  /var/run/sshd /usr/sbin/nologin
landscape x 103 108  /var/lib/landscape /bin/false
mysql x 104 112 MySQL Server,,, /var/lib/mysql /bin/false
ntpd x 105 114  /var/run/openntpd /bin/false
postfix x 106 115  /var/spool/postfix /bin/false
nagios x 107 117  /var/lib/nagios /bin/false
chun x 1003 1003 Li Fu Chun,,, /home/chun
munin x 108 118  /var/lib/munin /bin/false

$ awk '!/[a-c]/ { print }' file.txt
root x 0 0 root /root
neo x 1000 1000 neo,,, /home/neo

采用判断来输出特定的列数据：
neo@monitor:~$ sed -e 's/:/ /g' /etc/passwd | awk '$1 == "neo" { print $1 }'
neo

部分包含，不包含指定的字符：
$ awk '$1 ~ /[a-d]/ { print }' file.txt
$ awk '$1 !~ /[a-d]/ { print }' file.txt

```

#### 3.3.1. Pattern, Pattern

```
# awk '/www/,/Web/ {print}' /etc/passwd
www:x:80:80:Web User:/www:/bin/bash

# awk '/www/,/[Ww]eb/ {print}' /etc/passwd
www:x:80:80:Web User:/www:/bin/bash

```

```
cat /var/log/rinetd.log | awk -F' ' '$7 ~ /0/ {print $1"\t"$2"\t"$7"\t"$8"\t"$9}'

```

```
# cat /var/log/rinetd.log | awk -F' ' '$7 ~ /(210|209|210)/ {print $1"\t"$2"\t"$7"\t"$8"\t"$9}'

```

### 3.4. Built-in Variables (NR/NF)

```
例如 : awk 读入第一笔数据行
"aaa bbb ccc ddd" 之后, 程序中:
$0 之值将是 "aaa bbb ccc ddd"
$1 之值为 "aaa"
$2 之值为 "bbb"
$3 之值为 "ccc"
$4 之值为 "ddd"
$NF 之值为 4
$NR 之值为 1

```

#### 3.4.1. NR

NR=n 指定 n 行号

```
# awk -F':' 'NR==1 {print $(1)}' /etc/passwd
root

# awk -F':' 'NR==2 {print $(1)}' /etc/passwd
bin

```

取 1，3，4 行

```
# awk 'NR==1; NR==3; NR==4 {print $1}' /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin

```

awk ... '{if(NR=1){...}else{exit)}'

```
$ awk -F' ' '{if(NR==1) print $1}' /etc/issue
Ubuntu

```

#### 3.4.2. NF

```
# echo "aaa bbb ccc ddd" | awk  '{print $(NR)}'
aaa
# echo "aaa bbb ccc ddd" | awk  '{print $(NR+1)}'
bbb
# echo "aaa bbb ccc ddd" | awk  '{print $(NR+2)}'
ccc
# echo "aaa bbb ccc ddd" | awk  '{print $(NF)}'
ddd
# echo "aaa bbb ccc ddd" | awk  '{print $(NF-1)}'
ccc
# echo "aaa bbb ccc ddd" | awk  '{print $(NF-2)}'
bbb

uptime | awk '{print $(NF-2)}'

```

```
[root@netkiller ~]# netstat -na |awk '/^tcp/ {print NF}' | head -n 1
6

[root@netkiller ~]# netstat -ant |awk '/^tcp/ {print $NF}' | tail -n 5
TIME_WAIT
CLOSE_WAIT
CLOSE_WAIT
LISTEN
LISTEN

[root@netkiller ~]# netstat -ant |awk '/^tcp/ {print $(NF-5)}' | tail -n 5
tcp
tcp
tcp
tcp6
tcp6

```

#### 3.4.3. 练习

##### 3.4.3.1. 使用 ss 命令统计 TCP 状态

```
[root@netkiller ~]# ss -ant | awk '{++S[$1]} END {for(a in S) print a, S[a]}'
LISTEN 13
CLOSE-WAIT 42
ESTAB 95
State 1
FIN-WAIT-2 20
LAST-ACK 44
SYN-SENT 10
TIME-WAIT 403				

```

```
[root@netkiller ~]# ss -ant | awk 'BEGIN {stats["CLOSE-WAIT"]=0;stats["ESTAB"]=0;stats["FIN-WAIT-1"]=0;stats["FIN-WAIT-2"]=0;stats["LAST-ACK"]=0;stats["SYN-RECV"]=0;stats["SYN-SENT"]=0;stats["TIME-WAIT"]=0} {++stats[$1]} END {for(a in stats) print a, stats[a]}'
LISTEN 6
SYN-RECV 0
ESTAB 4
CLOSE-WAIT 0
State 1
FIN-WAIT-1 0
LAST-ACK 0
FIN-WAIT-2 0
TIME-WAIT 3
SYN-SENT 0

```

##### 3.4.3.2. TCP/IP Status

```
netstat -ant | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}'

TIME_WAIT 88
CLOSE_WAIT 6
FIN_WAIT1 9
FIN_WAIT2 9
ESTABLISHED 303
SYN_RECV 126
LAST_ACK 5

ss  | awk '$1 !~ /State/ {++state[$1]} END {for(key in state) print key,"\t",state[key]}'
LAST-ACK 	 1
ESTAB 	 5
FIN-WAIT-2 	 1
CLOSE-WAIT 	 13

```

##### 3.4.3.3. 用户 shell 统计

```
# cat /etc/passwd | awk -F':' '{++shell[$NF]} END {for(key in shell) print key,"\t",shell[key]}'
/sbin/shutdown 	 1
/bin/sh 	 1
/bin/bash 	 3
/sbin/nologin 	 20
/sbin/halt 	 1
/bin/sync 	 1

```

##### 3.4.3.4. access.log POST 与 GET 统计

```
# cat /www/logs/access.log | egrep -o 'GET|POST' | awk '{++method[$NF]} END {for(num in method) print num, method[num]}'
POST 422
GET 188571

# cat /www/logs/access.log | egrep -o 'GET|POST' | awk '{++method[$1]} END {for(num in method) print num, method[num]}'
POST 422
GET 188573

```

### 3.5. Built-in Functions

#### 3.5.1. length

```

# awk -F: 'length($1)<4 {print NR , $1}' /etc/passwd
2 bin
4 adm
5 lp
14 ftp
20 ntp
22 rpc
25 www

```

#### 3.5.2. toupper() 转为大写字母

```

[root@localhost ~]# awk '{print toupper($1)}' /etc/passwd
ROOT:X:0:0:ROOT:/ROOT:/BIN/BASH
BIN:X:1:1:BIN:/BIN:/SBIN/NOLOGIN
DAEMON:X:2:2:DAEMON:/SBIN:/SBIN/NOLOGIN
ADM:X:3:4:ADM:/VAR/ADM:/SBIN/NOLOGIN
LP:X:4:7:LP:/VAR/SPOOL/LPD:/SBIN/NOLOGIN
SYNC:X:5:0:SYNC:/SBIN:/BIN/SYNC
SHUTDOWN:X:6:0:SHUTDOWN:/SBIN:/SBIN/SHUTDOWN
HALT:X:7:0:HALT:/SBIN:/SBIN/HALT
MAIL:X:8:12:MAIL:/VAR/SPOOL/MAIL:/SBIN/NOLOGIN
OPERATOR:X:11:0:OPERATOR:/ROOT:/SBIN/NOLOGIN
GAMES:X:12:100:GAMES:/USR/GAMES:/SBIN/NOLOGIN
FTP:X:14:50:FTP
NOBODY:X:99:99:NOBODY:/:/SBIN/NOLOGIN
SYSTEMD-NETWORK:X:192:192:SYSTEMD
DBUS:X:81:81:SYSTEM
POLKITD:X:999:997:USER
POSTFIX:X:89:89::/VAR/SPOOL/POSTFIX:/SBIN/NOLOGIN
CHRONY:X:998:996::/VAR/LIB/CHRONY:/SBIN/NOLOGIN
SSHD:X:74:74:PRIVILEGE-SEPARATED
NTP:X:38:38::/ETC/NTP:/SBIN/NOLOGIN
DHCPD:X:177:177:DHCP
WWW:X:80:80:WEB
NGINX:X:997:995:NGINX
MYSQL:X:27:27:MYSQL
REDIS:X:1000:1000::/VAR/LIB/REDIS:/BIN/FALSE
ETHEREUM:X:1001:1001::/HOME/ETHEREUM:/BIN/BASH
MONGOD:X:996:991:MONGOD:/VAR/LIB/MONGO:/BIN/FALSE			

```

#### 3.5.3. tolower() 转为小写字母

```

[root@localhost ~]# awk -F '\n' '{print tolower($1)}' /etc/redhat-release 
centos linux release 7.5.1804 (core) 			

```

#### 3.5.4. rand() 随机数生成

```

neo@MacBook-Pro ~ % awk 'BEGIN{print rand()*1000000}' 
840188			
neo@MacBook-Pro ~ % awk 'BEGIN{srand(); print rand()}'
0.0334342
neo@MacBook-Pro ~ % awk 'BEGIN{srand(); print rand()*1000000}'
759412	

```

### 3.6. 过滤相同的行

```
grep 'Baiduspider' access.2011-02-22.log | awk '{print $1}' | awk '! a[$0]++'

```

```
awk '! a[$0]++' 1.txt >2.txt
这个是删除文件中所有列都重复的记录

awk '! a[$1]++' 1.txt >2.txt
删除文件中第一列重复的记录

awk '! a[$1,$2]++' 1.txt >2.txt
删除文件中第一，二列都重复的记录

```

### 3.7. 数组演示

```

[root@localhost ~]# awk -F ':' 'BEGIN {count=1;} {name[count] = $1;count++;}; END{for (i = 1; i < NR; i++) print i, name[i]}' /etc/passwd
1 root
2 bin
3 daemon
4 adm
5 lp
6 sync
7 shutdown
8 halt
9 mail
10 operator
11 games
12 ftp
13 nobody
14 systemd-network
15 dbus
16 polkitd
17 postfix
18 chrony
19 sshd
20 ntp
21 dhcpd
22 www
23 nginx
24 mysql
25 redis
26 ethereum		

```

## 4. sed

http://sed.sourceforge.net/

### 4.1. 查找与替换

#### find and replace

```

sed -n 's/root/admin/p' /etc/passwd
sed -n 's/root/admin/2p' /etc/passwd        				#在每行的第 2 个 root 作替换
sed -n 's/root/admin/gp' /etc/passwd
sed -n '1,10 s/root/admin/gp' /etc/passwd
sed -n 's/root/AAA&BBB/2p' /etc/passwd       				#将 root 替换成 AAArootBBB，&作反向引用，代替前面的匹配项
sed -ne 's/root/AAA&BBB/' -ne 's/bash/AAA&BBB/p' /etc/passwd #-e 将多个命令连接起来，将 root 或 bash 行作替换
sed -n 's/root/AAA&BBB/;s/bash/AAA&BBB/p' /etc/passwd   	#与上命令功能相同
sed -nr 's/(root)(.*)(bash)/\3\2\1/p' /etc/passwd     		#将 root 与 bash 位置替换，两标记替换 或 sed -n 's/root.∗bash/\3\2\1/p' /etc/passwd

```

```

ls -1 *.html| awk '{printf "sed \047s/ADDRESS/address/g\047 %s >%s.sed;mv %s.sed %s\n", $1, $1, $1, $1;}'|bash

for f in `ls -1 *.html`; do [ -f $f ] && sed 's/<\/BODY>/<script src="http:\/\/www.google-analytics.com\/urchin.js" type="text\/javascript"><\/script>\n<script type="text\/javascript">\n_uacct = "UA-2033740-1";\nurchinTracker();\n<\/script>\n<\/BODY>/g' $f >$f.sed;mv $f.sed $f ; done;

```

```

my=/root/dir
str="/root/dir/file1 /root/dir/file2 /root/dir/file3 /root/dir/file/file1"
echo $str | sed "s:$my::g"

```

#### 4.1.1. 正则

```
sed s/[[:space:]]//g  filename          删除空格

```

#### 4.1.2. aaa="bbb" 提取 bbb

```

$ echo "aaa=\"bbb\"" | sed 's/.*=\"\(.*\)\"/\1/g'
$ curl -s http://www.example.com | egrep -o '<a href="(.*)">.*</a>' | sed -e 's/.*href="\([^"]*\)".*/\1/'

```

Mac 地址转换

```
echo 192.168.2.1-a1f4.40c1.5756 | sed -r 's|(.*-)(..)(..).(..)(..).(..)(..)|\1\2:\3:\4:\5:\6:\7|g'

```

#### 4.1.3. 首字母大写

```

$ cat /etc/passwd | cut -d: -f1 | sed 's/\b[a-z]/\U&/g'
Root
Daemon
Bin
Sys
Sync
Games
Man
Lp
Mail
News
Uucp
Proxy
Www-Data
Backup
List
Irc
Gnats
Nobody
Libuuid
Syslog
Messagebus
Whoopsie
Landscape
Sshd
Neo
Ntop
Redis
Postgres
Colord
Mysql
Zookeeper

```

### 4.2. insert 插入字符

i 命令插入一行，并且在当前行前面有两个空格

在 root 行前插入一个 admin

```
sed '/root/i admin' /etc/passwd		

```

33 行处插入字符

```
sed -i "33 i \ \ authorization: enabled" /etc/mongod.conf

```

### 4.3. 追加字符

在 root 行后追加一个 admin 行

```
sed '/root/a admin' /etc/passwd

```

### 4.4. 修改字符

将 root 行替换为 admin

```
sed '/root/c admin' /etc/passwd

```

### 4.5. 删除字符

删除含有 root 的行

```
sed '/root/d' /etc/passwd 

```

#### 4.5.1. delete

删除空行

```
sed /^$/d         filename
sed '/./!d' filename

```

### 4.6. 行操作

模式空间中的内容全部打印出来

定位行：

```
sed -n '12,~3p' pass #从第 12 行开始，直到下一个 3 的倍数行（12-15 行）
sed -n '12,+4p' pass #从第 12 行开始，连续 4 行（12-16 行）
sed -n '12~3p' pass #从第 12 行开始，间隔 3 行输出一次（12，15，18，21...）
sed -n '10,$p' pass   #从第 10 行至结尾
sed -n '4!p' pass   #除去第 4 行

```

打印 3~6 行间的内容

```
$ sed -n '3,6p' /etc/passwd
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin

```

打印 35 行至行尾

```
$ sed -n '35,$p' /etc/passwd
sshd:x:116:65534::/var/run/sshd:/usr/sbin/nologin
mysql:x:117:126:MySQL Server,,,:/nonexistent:/bin/false
uuidd:x:100:101::/run/uuidd:/bin/false
libvirt-qemu:x:118:128:Libvirt Qemu,,,:/var/lib/libvirt:/bin/false
libvirt-dnsmasq:x:119:129:Libvirt Dnsmasq,,,:/var/lib/libvirt/dnsmasq:/bin/false
redis:x:120:130::/var/lib/redis:/bin/false		

```

### 4.7. 编辑文件

```
-i[SUFFIX], --in-place[=SUFFIX]
                 edit files in place (makes backup if extension supplied)

```

下面例子是替换 t.php 中的 java 字符串为 php

```

$ cat t.php
<?java

$ sed -i 's/java/php/g' t.php

$ cat t.php
<?php

```

```

find -name "*.php" -exec sed -i '/<?.*eval(gzinflate(base64.*?>/ d' '{}' \; -print

```

指定查找替换的行号

```

sed -i "7,7 s/#server.host: \"localhost\"/server.host: \"0.0.0.0\"/" /etc/kibana/kibana.yml		

```

### 4.8. 正则表达式

正则：'/正则式/'

```

sed -n '/root/p' /etc/passwd
sed -n '/^root/p' /etc/passwd
sed -n '/bash$/p' /etc/passwd
sed -n '/ro.t/p' /etc/passwd
sed -n '/ro*/p' /etc/passwd
sed -n '/[ABC]/p' /etc/passwd
sed -n '/[A-Z]/p' /etc/passwd
sed -n '/[^ABC]/p' /etc/passwd
sed -n '/^[^ABC]/p' /etc/passwd
sed -n '/\<root/p' /etc/passwd
sed -n '/root\>/p' /etc/passwd

```

扩展正则：

```

sed -n '/root\|yerik/p' /etc/passwd #拓展正则需要转义
sed -nr '/root|yerik/p' /etc/passwd #加-r 参数支持拓展正则
sed -nr '/ro(ot|ye)rik/p' /etc/passwd #匹配 rootrik 和 royerik 单词
sed -nr '/ro?t/p' /etc/passwd   #?匹配 0-1 次前导字符
sed -nr '/ro+t/p' /etc/passwd   #匹配 1-n 次前导字符
sed -nr '/ro{2}t/p' /etc/passwd   #匹配 2 次前导字符
sed -nr '/ro{2,}t/p' /etc/passwd   #匹配多于 2 次前导字符
sed -nr '/ro{2，4}t/p' /etc/passwd #匹配 2-4 次前导字符
sed -nr '/(root)*/p' /etc/passwd   #匹配 0-n 次前导单词	

```

### 4.9. 管道操作

```

cat <<! | sed '/aaa=\(bbb\|ccc\|ddd\)/!s/\(aaa=\).*/\1xxx/'
> aaa=bbb
> aaa=ccc
> aaa=ddd
> aaa=[something else]
!
aaa=bbb
aaa=ccc
aaa=ddd
aaa=xxx		

```

### 4.10. 字母大小写转换

```

[root@localhost ~]# echo "netkiller" | sed 's/[a-z]/\u&/g'
NETKILLER

[root@localhost ~]# echo "NETKILLER" | sed 's/[A-Z]/\l&/g'
netkiller

```

### 4.11. perl

```
sed -i -e 's/aaa/bbb/g' *
perl -p -i -e 's/aaa/bbb/g' *

```

## 5. CURL - transfer a URL

### 5.1. 基本用法

```

curl http://www.google.com/		

```

### 5.2. data

post 表单数据

```

curl -d "user=neo&password=chen" http://www.example.com/login.php
curl --data "user=neo&password=chen" http://www.example.com/login.php

```

### 5.3. 上传文件

```

curl  -F "upload=@card.txt;type=text/plain"  "http://www.example.com/upload.php"		

```

使用 CURL 上传 Oauth2 + Jwt 认证的 Restful 接口

```

curl -s  -H "Authorization: Bearer ${TOKEN}" -X POST -F "file=@/etc/hosts" http://localhost:8080/upload/single		

```

### 5.4. connect-timeout

```

curl -o /dev/null --connect-timeout 30 -m 30 -s -w %{http_code} http://www.google.com/		

```

### 5.5. max-time

-m, --max-time SECONDS Maximum time allowed for the transfer

```
			curl -o /dev/null --max-time 10 http://www.netkiller.cn/

```

### 5.6. compressed

#### --compressed Request compressed response (using deflate or gzip)

```
			curl --compressed http://www.example.com

```

### 5.7. vhosts

有时候你需要设觉察/etc/hosts 文件才能访问 vhost,下面例子可以不设置/etc/hosts

```
			curl -x 127.0.0.1:80 your.exmaple.com/index.php

```

socks5 服务器

```

$ curl -v -x socks5://username:password@IP:1080 http://www.google.com/		

```

### 5.8. -w, --write-out <format> 输出格式定义

```
			计时器 描述
			time_connect 建立到服务器的 TCP 连接所用的时间
			time_starttransfer 在发出请求之后,Web 服务器返回数据的第一个字节所用的时间
			time_total 完成请求所用的时间
			time_namelookup DNS 解析时间,从请求开始到 DNS 解析完毕所用时间(记得关掉 Linux 的 nscd 的服务测试)
			speed_download 下载速度，单位-字节每秒。

```

```
			curl -o /dev/null -s -w %{time_connect}:%{time_starttransfer}:%{time_total} http://www.example.net
			curl -o /dev/null -s -w "Connect: %{time_connect}\nTransfer: %{time_starttransfer}\nTotal: %{time_total}\n" https://www.netkiller.cn/index.html

			curl -o /dev/null -s -w "Connect: %{time_connect} \nTransfer: %{time_starttransfer}\nTotal: %{time_total}\nNamelookup: %{time_namelookup}\nDownload: %{speed_download}\n" https://www.netkiller.cn/index.html
			Connect: 0.024241
			Transfer: 0.117727
			Total: 0.117842
			Namelookup: 0.004367
			Download: 129877.000

```

测试页面所花费的时间

```
			date ; curl -s -w 'Connect: %{time_connect} TTFB: %{time_starttransfer} Total time: %{time_total} \n' -H "Host: www.example.com" http://172.16.0.1/webapp/test.jsp ; date ;

```

```
			curl -o /dev/null -s -w %{time_connect}, %{time_starttransfer}, %{time_total}, %{time_namelookup}, %{speed_download} http://www.netkiller.cn

```

返回 HTTP 状态码

```
			curl -s -I http://netkiller.sourceforge.net/ | grep HTTP | awk '{print $2" "$3}'
			curl -o /dev/null -s -w %{http_code} http://netkiller.sourceforge.net/

			curl --connect-timeout 5 --max-time 60 --output /dev/null -s -w %{response_code} http://www.netkiller.cn/

```

```
			# curl -w '\nLookup time:\t%{time_namelookup}\nConnect time:\t%{time_connect}\nPreXfer time:\t%{time_pretransfer}\nStartXfer time:\t%{time_starttransfer}\n\nTotal time:\t%{time_total}\n' -o /dev/null -s http://www.netkiller.cn

			Lookup time: 0.125
			Connect time: 0.125
			PreXfer time: 0.125
			StartXfer time: 0.125

			Total time: 0.126

```

### 5.9. -A/--user-agent <agent string>

设置用户代理，这样 web 服务器会认为是其他浏览器访问

```
			curl -A "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" http://www.example.com

```

### 5.10. referer

```

curl -v -o /dev/null -e "http://www.example.com" http://www.your.com/
* About to connect() to www.your.com port 80
*   Trying 172.16.1.10... connected
* Connected to www.your.com (172.16.1.10) port 80
> GET / HTTP/1.1
> User-Agent: curl/7.15.5 (x86_64-redhat-linux-gnu) libcurl/7.15.5 OpenSSL/0.9.8b zlib/1.2.3 libidn/0.6.5
> Host: www.your.com
> Accept: */*
> Referer: http://www.example.com
>
< HTTP/1.1 200 OK
< Date: Thu, 30 Sep 2010 07:59:47 GMT
< Server: Apache/2.2.16 (Unix) mod_ssl/2.2.16 OpenSSL/0.9.8e-fips-rhel5 PHP/5.2.14
< Accept-Ranges: bytes
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  172k    0  172k    0     0  10.2M      0 --:--:-- --:--:-- --:--:-- 11.9M* Connection #0 to host www.your.com left intact

* Closing connection #0

```

### 5.11. -v

### 5.12. -o, --output FILE Write output to <file> instead of stdout

```
			curl -o /dev/null http://www.example.com
			curl -o index.html http://www.example.com

```

### 5.13. -L, --location

```

curl -L --retry 5 --retry-delay 3 https://github.com/hyperledger/fabric/releases/download/v2.0.1/hyperledger-fabric-linux-amd64-2.0.1.tar.gz | tar xz		

```

### 5.14. -H/--header <line> Custom header to pass to server (H)

#### 5.14.1. Last-Modified / If-Modified-Since

If-Modified-Since

```
				neo@neo-OptiPlex-780:/tmp$ curl -I http://images.example.com/test/test.html
				HTTP/1.0 200 OK
				Cache-Control: s-maxage=7200, max-age=900
				Expires: Mon, 16 May 2011 08:10:37 GMT
				Content-Type: text/html
				Accept-Ranges: bytes
				ETag: "1205579110"
				Last-Modified: Mon, 16 May 2011 06:57:39 GMT
				Content-Length: 11
				Date: Mon, 16 May 2011 07:55:37 GMT
				Server: lighttpd/1.4.26
				Age: 604
				X-Via: 1.0 ls71:80 (Cdn Cache Server V2.0), 1.0 lydx136:8105 (Cdn Cache Server V2.0)
				Connection: keep-alive

				neo@neo-OptiPlex-780:/tmp$ curl -H
				"If-Modified-Since: Fri, 12 May 2011 18:53:33 GMT" -I http://images.example.com/test/test.html
				HTTP/1.0 304 Not Modified
				Date: Mon, 16 May 2011 07:56:19 GMT
				Content-Type: text/html
				Expires: Mon, 16 May 2011 08:11:19 GMT
				Last-Modified: Mon, 16 May 2011 06:57:39 GMT
				ETag: "1205579110"
				Cache-Control: s-maxage=7200, max-age=900
				Age: 790
				X-Via: 1.0 wzdx168:8080 (Cdn Cache Server V2.0)
				Connection: keep-alive

```

#### 5.14.2. ETag / If-None-Match

```
				neo@neo-OptiPlex-780:/tmp$ curl -I http://images.example.com/test/test.html
				HTTP/1.1 200 OK
				Cache-Control: s-maxage=7200, max-age=900
				Expires: Mon, 16 May 2011 09:48:45 GMT
				Content-Type: text/html
				Accept-Ranges: bytes
				ETag: "1984705864"
				Last-Modified: Mon, 16 May 2011 09:01:07 GMT
				Content-Length: 22
				Date: Mon, 16 May 2011 09:33:45 GMT
				Server: lighttpd/1.4.26

```

```
				neo@neo-OptiPlex-780:/tmp$ curl -H 'If-None-Match: "1984705864"' -I http://images.example.com/test/test.html
				HTTP/1.1 304 Not Modified
				Cache-Control: s-maxage=7200, max-age=900
				Expires: Mon, 16 May 2011 09:48:32 GMT
				Content-Type: text/html
				Accept-Ranges: bytes
				ETag: "1984705864"
				Last-Modified: Mon, 16 May 2011 09:01:07 GMT
				Date: Mon, 16 May 2011 09:33:32 GMT
				Server: lighttpd/1.4.26

```

#### 5.14.3. Accept-Encoding:gzip,defalte

```
				$ curl -H Accept-Encoding:gzip,defalte -I http://www.example.com/product/374218.html
				HTTP/1.1 200 OK
				Date: Mon, 16 May 2011 09:13:18 GMT
				Server: Apache
				Accept-Ranges: bytes
				Content-Encoding: gzip
				Content-Length: 16660
				Content-Type: text/html; charset=UTF-8
				X-Pad: avoid browser bug
				Age: 97
				X-Via: 1.1 dg44:8888 (Cdn Cache Server V2.0)
				Connection: keep-alive

```

```
				$ curl -H Accept-Encoding:gzip,defalte http://www.example.com/product/374218.html | gunzip

```

#### 5.14.4. HOST

```
				curl -H HOST:www.example.com -I http://172.16.1.10/product/374218.html

```

#### 5.14.5. HTTP 认证

未认证返回 401

```

# curl --compressed http://webservice.example.com/members
<html>
<head><title>401 Authorization Required</title></head>
<body bgcolor="white">
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx</center>
</body>
</html>

```

-u/--user <user[:password]> Set server user and password

使用 -u 或者 --user 指定用户与密码

```
				# curl --compressed -uneo:chen http://webservice.example.com/members

```

#### 5.14.6. Accept

```

-H "Accept: application/json"		

```

#### 5.14.7. Content-Type

```

-H "Content-Type: application/json"			

```

### 5.15. curl-config

```
			curl-config --features

```

### 5.16. 指定网络接口或者地址

--interface INTERFACE Use network INTERFACE (or address)

```
			curl --interface 127.0.0.1 http://www.netkiller.cn

```

### 5.17.  Cookie 处理

cookie 可以从 http header 设置

```
			curl -LO -H "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm

```

curl 还提供两个参数用于处理 cookie

```
			-b, --cookie STRING/FILE Read cookies from STRING/FILE (H) 读取 cookie 文件
			-c, --cookie-jar FILE Write cookies to FILE after operation (H) 将 cookie 写入文件

```

```

curl -c cookie.txt -d "user=neo&password=123456" http://www.netkiller.cn/login
curl -b cookie.txt http://www.netkiller.cn/user/profile

```

### 5.18. RestFul 应用 JSON 数据处理

下面提供一些使用 curl 操作 restful 的实例

GET 操作

```
			curl http://api.netkiller.cn/v1/withdraw/get/15.json

```

用户认证的情况

```
			curl http://test:123456@api.netkiller.cn/v1/withdraw/get/id/815.json

```

POST 操作

```

curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X POST -d '
{
"id":"B040200000000000000","name":"Neo","amount":12,"password":"12345","createdate":"2016-09-12 13:10:10"

}' https://test:123456@api.netkiller.cn/v1/withdraw/create.json

```

#### 5.18.1. Curl Oauth2

```

URL=https://api.netkiller.cn
token=$(curl -k --cacert -s -X POST --user 'api:secret' -d 'grant_type=password&username=netkiller@msn.com&password=123456' ${URL}/oauth/token | grep -o -E '"access_token":"([0-9a-f-]+)"' | cut -d \" -f 4 )
curl -k -H "Accept: application/json" -H "Content-Type: application/json" -H "Authorization: Bearer ${token}" -X GET ${URL}/search/article/list/22/0/5.json			

```

#### 5.18.2. Curl + Oauth2 + Jwt

获取 access_token 字符串

方法一

```

curl -s  -X POST --user 'api:secret' -d 'grant_type=password&username=netkiller@msn.com&password=123456' http://localhost:8080/oauth/token | sed 's/.*"access_token":"\([^"]*\)".*/\1/g'

```

方法二

```

curl -s  -X POST --user 'api:secret' -d 'grant_type=password&username=netkiller@msn.com&password=123456' http://localhost:8080/oauth/token | grep -o -E '"access_token":"(.+)"' | cut -d \" -f 4			

```

### 5.19. HTTP2

curl 已经支持 HTTP2，使用方法如下

```

neo@MacBook-Pro ~/workspace % curl -I --http2 https://www.google.com  
HTTP/2 200 
date: Tue, 26 Feb 2019 06:36:03 GMT
expires: -1
cache-control: private, max-age=0
content-type: text/html; charset=ISO-8859-1
p3p: CP="This is not a P3P policy! See g.co/p3phelp for more info."
server: gws
x-xss-protection: 1; mode=block
x-frame-options: SAMEORIGIN
set-cookie: 1P_JAR=2019-02-26-06; expires=Thu, 28-Mar-2019 06:36:03 GMT; path=/; domain=.google.com
set-cookie: NID=160=aQySEvsSa9gVU8qivD3t5qsgi_PRUtD5Ao3nRb48jMyLAzlYA1ViWuF1BaZHChVzY6EuknQ0OUz3Z2vhWwrclzO4WV6BmWgPhz6jowqFF3XCStsyYVwLQp2-_c0aGioBryAP1bTT0c-PX9iJzk5Zcbm2cFs6kH0Qb2a_3ML7Ioc; expires=Wed, 28-Aug-2019 06:36:03 GMT; path=/; domain=.google.com; HttpOnly
alt-svc: quic=":443"; ma=2592000; v="44,43,39"
accept-ranges: none
vary: Accept-Encoding		

```

HTTP/2 200 表示当前采用 HTTP2 连接

### 5.20. FAQ

Error in TLS handshake, trying SSLv3...

解决方案

```

curl -v --cipher rsa_rc4_128_sha https://www.mpaymall.com/MPay/MerchantPay.do		

```

## 6. expect

```
$ sudo apt-get install expect

```

命令含义

```
#!/usr/bin/expect
set timeout 30
spawn ssh root@192.168.1.1
expect "password:"
send "mypassword\r"
interact

```

set 设置变量

spawn 执行命令

expect 检测点

send 发送指令

### 6.1. 模拟登录 telnet 获取 Cisco 配置

例 28.1. example for expect

```
cat tech-support.exp
#!/usr/bin/expect
set timeout 30
spawn telnet 172.16.1.24
expect "Password: "
send "chen\r"
expect "*>"
send "enable\r"
expect "Password: "
send "chen\r"
expect "*#"
send "sh tech-support\r"
send "!\r"
expect "*-Switch#"
send "exit\r"
expect eof
exit

```

3 层设备

```
cisco.exp
#!/usr/bin/expect
set ip [lindex $argv 0]
set username [lindex $argv 1]
set password [lindex $argv 2]
log_file $ip.log
spawn telnet $ip
expect "Username:"
send "$username\r"
expect "Password:"
send "$password\r"
expect "#"
send "show running-config\r"
send "exit\r"
expect eof

```

2 层设备

```

$ cat config.exp
#!/usr/bin/expect
set timeout 30
set host [lindex $argv 0]
set password [lindex $argv 1]
set done 0

log_file $host.log
spawn telnet $host
expect "Password:"
send "$password\r"
expect "*>"
send "enable\r"
expect "Password: "
send "$password\r"
expect "*#"
send "show running-config\r"

while {$done == 0} {
expect {
" --More--" { send -- " " }
"*#" { set done 1 }
eof { set done 1 }
}
}

send "\r"
expect "*#"
send "exit\r"
expect eof
exit

$ cat loop.sh
#! /bin/sh
while read sw
do
	./config.exp $sw
done <<EOF
172.16.0.240 chen
172.16.0.241 chen
172.16.0.242 chen
172.16.0.243 chen
172.16.0.245 chen
172.16.0.246 chen
EOF

$ chmod +x config.exp loop.sh
$ ./loop.sh

```

### 6.2. 模拟登录 ssh

例 28.2. example for expect

```
#!/usr/bin/expect
set timeout 30
spawn ssh root@192.168.1.1
expect "password:"
send "mypassword\r"
interact

```

例 28.3. example 1

```
#!/usr/bin/expect
　　set password 1234  #密码
　　#download
　　spawn scp /www/* root@172.16.1.2:/www/
　　set timeout 300
　　expect "172.16.1.2's password:"
　　set timeout 3000
　　#exec sleep 1
　　send "$password\r"
　　set timeout 300
　　send "exit\r"
　　#expect eof
　　interact
　　spawn scp /www/* root@172.16.1.3:/www/
　　set timeout 300
　　expect "root@172.16.1.3's password:"
　　set timeout 3000
　　#exec sleep 1
　　send "$password\r"
　　set timeout 300
　　send "exit\r"
　　interact

```

例 28.4. *.exp

```
$ expect autossh.exp neo@192.168.3.10 chen "ls /"

```

autossh.exp

```
#!/usr/bin/expect -fset ipaddress [lindex $argv 0]
set ipaddress [lindex $argv 0]
set passwd [lindex $argv 1]
set command [lindex $argv 2]
set timeout 30
spawn ssh $ipaddress
expect {
    "yes/no" { send "yes\r";exp_continue }
    "password:" { send "$passwd\r" }
}
expect ""

send "$command \r"

send "exit\r"

expect eof
exit

```

批量执行

```
password.lst
192.168.0.1 passwd
192.168.0.2 passwd
192.168.0.3 passwd

```

```
#!/bin/bash

cat password.lst | while read line
do
	host=$(echo $line|awk '{print $1}')
	passwd=$(echo $line|awk '{print $2}')
	expect autossh.exp $host $passwd
	sleep 2
done

```

### 6.3. SCP

```
#! /usr/bin/expect -f
spawn scp 1 neo@192.168.0.1:
expect "*password:"
send "your password\r"

expect eof

```

```

#!/bin/expect

spawn scp x.x.x.x

for {} {1} {} {
	expect {
		"password:" {
	        send "YourPassWord"
	    }
	}
}

```

```

spawn scp 1 neo@172.16.0.1:
for { set i 1 } {$i<5} {incr i} {
	expect {
		"*password:" {send "koven\r"}
		"*(yes/no)*" {send "yes\r"}
	}
}

```

## 7. expect-lite - quick and easy command line automation tool

## 8. sshpass - noninteractive ssh password provider

**sshpass -p 'ssh_password' ssh www.example.org**

```
# ssh neo@192.168.6.1
The authenticity of host '192.168.6.1 (192.168.6.1)' can't be established.
RSA key fingerprint is c9:97:95:2a:5c:6a:2f:ac:e8:ac:94:24:b0:5c:45:8a.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.6.1' (RSA) to the list of known hosts.
neo@192.168.6.1's password: 

[root@centos6]~# sshpass -p 'chen' ssh neo@192.168.6.1
Last login: Wed Nov 13 15:24:50 2013
[neo@NEO ~]$ 

```

## 9. Klish - Kommand Line Interface Shell (the fork of clish project)

http://code.google.com/p/klish/

Klish 是一个命令行补全工具，可以实现类似于 CISCO 路由器的命令行帮助界面。它是 Clish 的后续版本，Klish 有一个特殊的功能，可以让用户仅使用指定目录中的命令。

### 9.1. 安装 Klish

```
# cd /usr/local/src/
# wget http://klish.googlecode.com/files/klish-1.6.4.tar.bz2
# tar jxvf klish-1.6.4.tar.bz2
# cd klish-1.6.4/
# ./configure --prefix=/srv/klish-1.6.4
# make
# make install

# cp -r xml-examples /srv/klish-1.6.4/
# export CLISH_PATH=/srv/klish-1.6.4/xml-examples/clish

```

启动 clish

```

# /srv/klish-1.6.4/bin/clish

********************************************
*         CLISH (see-lish)                 *
*                                          *
*      WARNING: Authorised Access Only     *
********************************************

Welcome root it is Mon Feb 18 09:59:06 CST 2013
>

```

### 9.2. 为用户指定 clish 作为默认 Shell

```
# vim /etc/passwd
neo:x:1000:1000:neo,,,:/home/neo:/bin/bash

```

改为

```
neo:x:1000:1000:neo,,,:/home/neo:/srv/klish-1.6.4/bin/clish

```

### 9.3. FAQ

#### 9.3.1. clish/shell/shell_expat.c:36:19: fatal error: expat.h: No such file or directory

```
clish/shell/shell_expat.c:36:19: fatal error: expat.h: No such file or directory
compilation terminated.
make[1]: *** [clish/shell/libclish_la-shell_expat.lo] Error 1
make[1]: Leaving directory `/usr/local/src/klish-1.6.4'
make: *** [all] Error 2

```

解决方案，安装 expat 开发包

```
# apt-get install libexpat1-dev

```

## 10. Limited command Shell (lshell)

[`github.com/ghantoos/lshell`](https://github.com/ghantoos/lshell)

主要功能就是能够限制那些命令用户可以运行

## 11. Wget - The non-interactive network downloader.

wget 各种选项分类列表

```
* 启动
-V, –version 显示 wget 的版本后退出
-h, –help 打印语法帮助
-b, –background 启动后转入后台执行
-e, –execute=COMMAND 执行`.wgetrc’格式的命令，wgetrc 格式参见/etc/wgetrc 或~/.wgetrc
* 记录和输入文件
-o, –output-file=FILE 把记录写到 FILE 文件中
-a, –append-output=FILE 把记录追加到 FILE 文件中
-d, –debug 打印调试输出
-q, –quiet 安静模式(没有输出)
-v, –verbose 冗长模式(这是缺省设置)
-nv, –non-verbose 关掉冗长模式，但不是安静模式
-i, –input-file=FILE 下载在 FILE 文件中出现的 URLs
-F, –force-html 把输入文件当作 HTML 格式文件对待
-B, –base=URL 将 URL 作为在-F -i 参数指定的文件中出现的相对链接的前缀
–sslcertfile=FILE 可选客户端证书
–sslcertkey=KEYFILE 可选客户端证书的 KEYFILE
–egd-file=FILE 指定 EGD socket 的文件名
* 下载
–bind-address=ADDRESS 指定本地使用地址(主机名或 IP，当本地有多个 IP 或名字时使用)
-t, –tries=NUMBER 设定最大尝试链接次数(0 表示无限制).
-O –output-document=FILE 把文档写到 FILE 文件中
-nc, –no-clobber 不要覆盖存在的文件或使用.#前缀
-c, –continue 接着下载没下载完的文件
–progress=TYPE 设定进程条标记
-N, –timestamping 不要重新下载文件除非比本地文件新
-S, –server-response 打印服务器的回应
–spider 不下载任何东西
-T, –timeout=SECONDS 设定响应超时的秒数
-w, –wait=SECONDS 两次尝试之间间隔 SECONDS 秒
–waitretry=SECONDS 在重新链接之间等待 1…SECONDS 秒
–random-wait 在下载之间等待 0…2*WAIT 秒
-Y, –proxy=on/off 打开或关闭代理
-Q, –quota=NUMBER 设置下载的容量限制
–limit-rate=RATE 限定下载输率
* 目录
-nd –no-directories 不创建目录
-x, –force-directories 强制创建目录
-nH, –no-host-directories 不创建主机目录
-P, –directory-prefix=PREFIX 将文件保存到目录 PREFIX/…
–cut-dirs=NUMBER 忽略 NUMBER 层远程目录
* HTTP 选项
–http-user=USER 设定 HTTP 用户名为 USER.
–http-passwd=PASS 设定 http 密码为 PASS.
-C, –cache=on/off 允许/不允许服务器端的数据缓存 (一般情况下允许).
-E, –html-extension 将所有 text/html 文档以.html 扩展名保存
–ignore-length 忽略 `Content-Length’头域
–header=STRING 在 headers 中插入字符串 STRING
–proxy-user=USER 设定代理的用户名为 USER
–proxy-passwd=PASS 设定代理的密码为 PASS
–referer=URL 在 HTTP 请求中包含 `Referer: URL’头
-s, –save-headers 保存 HTTP 头到文件
-U, –user-agent=AGENT 设定代理的名称为 AGENT 而不是 Wget/VERSION.
–no-http-keep-alive 关闭 HTTP 活动链接 (永远链接).
–cookies=off 不使用 cookies.
–load-cookies=FILE 在开始会话前从文件 FILE 中加载 cookie
–save-cookies=FILE 在会话结束后将 cookies 保存到 FILE 文件中
* FTP 选项
-nr, –dont-remove-listing 不移走 `.listing’文件
-g, –glob=on/off 打开或关闭文件名的 globbing 机制
–passive-ftp 使用被动传输模式 (缺省值).
–active-ftp 使用主动传输模式
–retr-symlinks 在递归的时候，将链接指向文件(而不是目录)
* 递归下载
-r, –recursive 递归下载－－慎用!
-l, –level=NUMBER 最大递归深度 (inf 或 0 代表无穷).
–delete-after 在现在完毕后局部删除文件
-k, –convert-links 转换非相对链接为相对链接
-K, –backup-converted 在转换文件 X 之前，将之备份为 X.orig
-m, –mirror 等价于 -r -N -l inf -nr.
-p, –page-requisites 下载显示 HTML 文件的所有图片
* 递归下载中的包含和不包含(accept/reject)
-A, –accept=LIST 分号分隔的被接受扩展名的列表
-R, –reject=LIST 分号分隔的不被接受的扩展名的列表
-D, –domains=LIST 分号分隔的被接受域的列表
–exclude-domains=LIST 分号分隔的不被接受的域的列表
–follow-ftp 跟踪 HTML 文档中的 FTP 链接
–follow-tags=LIST 分号分隔的被跟踪的 HTML 标签的列表
-G, –ignore-tags=LIST 分号分隔的被忽略的 HTML 标签的列表
-H, –span-hosts 当递归时转到外部主机
-L, –relative 仅仅跟踪相对链接
-I, –include-directories=LIST 允许目录的列表
-X, –exclude-directories=LIST 不被包含目录的列表
-np, –no-parent 不要追溯到父目录

```

### 11.1. Logging and input file

#### 11.1.1. -i, --input-file=FILE download URLs found in local or external FILE.

准备输入文件，将要下载的链接放入文件中，例如：

```
$ vim file.lst

http://www.example.com/file1.txt
http://www.example.com/file2.txt
...
http://www.example.com/file10.txt

```

开始下载

```
$ wget -i file.lst

```

### 11.2. 下载相关参数

#### 11.2.1. -O, --output-document=FILE write documents to FILE 保存到文件

```
wget -q https://raw.githubusercontent.com/oscm/shell/master/web/tomcat/systemd/tomcat.service -O /usr/lib/systemd/system/tomcat.service

```

### 11.3. HTTP options (HTTP 选项)

#### 11.3.1. --post-data=STRING use the POST method; send STRING as the data.

```

wget -O - -q --post-data="user=neo&password=pasw0rd&title=test&message=helloworld" http://localhost/index.php

```

#### 11.3.2. header HTTP 头定义

-–header=STRING 在 headers 中插入字符串 STRING

```
wget --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/server-jre-8u131-linux-x64.tar.gz

```

### 11.4. Recursive download

#### 11.4.1. -r, --recursive specify recursive download.

使用-r 是应该注意，很多网页有外站链接，-r 会将外站一同下载(旧版本)

```

wget -r http://netkiller.github.com

```

#### 11.4.2. -m, --mirror shortcut for -N -r -l inf --no-remove-listing.

我们通常使用-m 可以下载整个网站例如我的网站上有很多电子书，你想一次下载下来离线阅读

```
wget -m http://netkiller.github.com/index.html			

```

### 11.5. --no-passive-ftp disable the "passive" transfer mode.

```
$ wget ftp://ftp:59bde6@42.120.45.123/test.zip
--2012-04-05 15:48:47--  ftp://ftp:*password*@42.120.45.123/test.zip
           => `test.zip'
Connecting to 42.120.45.123:21... connected.
Logging in as ftp ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD not needed.
==> SIZE 20120404.zip ... 42023258
==> PASV ...		

```

程序一直停留在 PASV 处

```

$ wget --no-passive-ftp ftp://ftp:26d9a0dd@42.120.45.123/test.zip
--2012-04-05 15:50:15--  ftp://ftp:*password*@42.120.45.123/test.zip
           => `test.zip'
Connecting to 42.120.45.123:21... connected.
Logging in as ftp ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD not needed.
==> SIZE test.zip ... 42023258
==> PORT ... done.    ==> RETR test.zip ... done.
Length: 42023258 (40M) (unauthoritative)

100%[=====================================================================================================================>] 42,023,258   691K/s   in 62s     

2012-04-05 15:51:18 (657 KB/s) - `test.zip' saved [42023258]

```

### 11.6. 下载一组连续的文件名

地址如下

```

http://news.netkiller.cn/2018/1/index.html
http://news.netkiller.cn/2018/2/index.html
...
...
http://news.netkiller.cn/2018/12/index.html		

```

下载方法

```

wget -c http://news.netkiller.cn/2018/{1..12}/index.html

```

## 12. TUI

### 12.1. screen - screen manager with VT100/ANSI terminal emulation

screen 类似 jobs, 前者是对 terminal, 后者针对进程。你可以随时再次链接 screen 会话，而不用担心中途因网络不稳定造成的中断。

```
sudo apt-get install screen

```

进入

```
screen

```

查看任务

```
screen -ls

```

重新连接会话

```
screen -r 16582

```

退出 screen 使用组合键 C-a K 或者

```
screen -wipe

```

### 12.2. tmux — terminal multiplexer

http://tmux.sourceforge.net/

查看当前 session **$tmux ls**

```
$ tmux ls
0: 1 windows (created Mon Aug 19 10:12:15 2013) [270x56] (attached)

$ tmux list-sessions
0: 1 windows (created Mon Aug 19 10:12:15 2013) [270x56] (attached)

```

创建 session

```
tmux new -s session-name

```

结束 session

```
$tmux kill-session -t 0

#结束所有 session
$tmux kill-server

```

使当前会话进入后台

```
tmux detach

```

恢复 session, detach 的反向操作

```
tmux attach -t session-id

```

### 12.3. byobu - wrapper script for seeding a user's byobu configuration and launching a text based window manager (either screen or tmux)

### 12.4. htop - interactive process viewer

### 12.5. elinks

### 12.6. chat

finch,irssi

## 13. jq - Command-line JSON processor

[`stedolan.github.io/jq/`](https://stedolan.github.io/jq/)

```

[root@localhost ~]# curl -s https://api.github.com/repos/netkiller/netkiller.github.io/commits?per_page=5 | jq '[.[] | {message: .commit.message, name: .commit.committer.name, parents: [.parents[].html_url]}]'
[
  {
    "message": "ethereum",
    "name": "netkiller",
    "parents": [
      "https://github.com/netkiller/netkiller.github.io/commit/4aa0409b9049c4ff77d047e17514964617d23d26"
    ]
  },
  {
    "message": "ethereum",
    "name": "netkiller",
    "parents": [
      "https://github.com/netkiller/netkiller.github.io/commit/939a62d6a8a0058025fca4a0226ded30c07f9178"
    ]
  },
  {
    "message": "ethereum",
    "name": "netkiller",
    "parents": [
      "https://github.com/netkiller/netkiller.github.io/commit/111a7d09089d7a1950d9879239370ca198f0870a"
    ]
  },
  {
    "message": "hyperledger",
    "name": "netkiller",
    "parents": [
      "https://github.com/netkiller/netkiller.github.io/commit/201b88ec4ad328268856ce6e894b860fa4bdd3a7"
    ]
  },
  {
    "message": "ethereum",
    "name": "netkiller",
    "parents": [
      "https://github.com/netkiller/netkiller.github.io/commit/92a052d152ef1333565646c79f12ada2f701003f"
    ]
  }
]

```

## 14. parallel - build and execute shell command lines from standard input in parallel

### 并行执行 shell 命令

```
$ sudo apt-get install parallel		

```

例 28.5. parallel - build and execute shell command lines from standard input in parallel

```
$ cat *.csv | parallel  --pipe grep '13113'			

```

设置块大小

```
$ cat *.csv | parallel --block 10M --pipe grep '131136688'			

```

## 15. multitail

```

dnf -y install epel-release
dnf -y update

dnf install -y gcc gcc-c++ make automake autoconf patch
dnf install -y git
dnf install -y python36
dnf install -y ncurses-devel

cd /usr/local/src/
git clone git://github.com/martine/ninja.git
cd ninja/
python3 bootstrap.py
cp ninja /usr/local/bin/

cd /usr/local/src/
git clone https://github.com/flok99/multitail.git
cd multitail/
make install

```

安装出错

```

[root@localhost multitail]# make install
cmake --build ../.build-multitail-Debug --target install
ninja: error: loading 'build.ninja': No such file or directory
make: *** [GNUmakefile:65: install] Error 1		

```

## 第 29 章 Shell Terminal

### *dialog, whiptail, gdialog, kdialog and nautilus*

## 1. terminal

### 1.1. resize - set TERMCAP and terminal settings to current xterm window size

显示终端屏幕的尺寸

```
$ resize
COLUMNS=151;
LINES=46;
export COLUMNS LINES;		

```

设置终端屏幕的尺寸

```
eval `resize`

```

### 1.2. tset, reset - terminal initialization

```
tset -e ^? 设置 Backspace 删除前面一个字符
tset -k ^C 设置删除一行

```

建议使用 stty 替代 tset

### 1.3. stty - change and print terminal line settings

```
$ stty
speed 38400 baud; line = 0;
eol = M-^?; eol2 = M-^?; swtch = M-^?;
ixany iutf8

$ stty -a
speed 115200 baud; rows 46; columns 151; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = M-^?; eol2 = M-^?; swtch = M-^?; start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R; werase = ^W;
lnext = ^V; flush = ^O; min = 1; time = 0;
-parenb -parodd cs8 hupcl -cstopb cread -clocal -crtscts
-ignbrk brkint -ignpar -parmrk -inpck -istrip -inlcr -igncr icrnl ixon -ixoff -iuclc ixany imaxbel iutf8
opost -olcuc -ocrnl onlcr -onocr -onlret -ofill -ofdel nl0 cr0 tab0 bs0 vt0 ff0
isig icanon iexten echo echoe echok -echonl -noflsh -xcase -tostop -echoprt echoctl echoke

```

```
OLDCONFIG=`stty -g`      # save configuration
stty -echo               # do not display password
echo "Enter password: \c"
read PASSWD              # get the password
stty $OLDCONFIG          # restore configuration			

```

## 2. tput

为输出着色

```
tput Color Capabilities:

tput setab [1-7] – Set a background color using ANSI escape
tput setb [1-7] – Set a background color
tput setaf [1-7] – Set a foreground color using ANSI escape
tput setf [1-7] – Set a foreground color

tput Text Mode Capabilities:

tput bold – Set bold mode
tput dim – turn on half-bright mode
tput smul – begin underline mode
tput rmul – exit underline mode
tput rev – Turn on reverse mode
tput smso – Enter standout mode (bold on rxvt)
tput rmso – Exit standout mode
tput sgr0 – Turn off all attributes

Color Code for tput:

0 – Black
1 – Red
2 – Green
3 – Yellow
4 – Blue
5 – Magenta
6 – Cyan	
7 – White

```

```
NORMAL=$(tput sgr0)  
RED=$(tput setaf 1)  
GREEN=$(tput setaf 2; tput bold)  
YELLOW=$(tput setaf 3)  
BLUE=$(tput setaf 4) 
MAGENTA=$(tput setaf 5) 
CYAN=$(tput setaf 6) 
WHITE=$(tput setaf 7) 

function exception(){
	echo -e "$WHITE$*$NORMAL"
}

function critical() {  
    echo -e "$RED$*$NORMAL"  
}  

function info() {  
    echo -e "$GREEN$*$NORMAL"  
}  

function warning() {  
    echo -e "$YELLOW$*$NORMAL"  
}  

function error(){
	echo -e "$MAGENTA$*$NORMAL"
} 

function debug(){
	echo -e "$CYAN$*$NORMAL"
} 

# To print critical  
critical "kernel error"  

# To print exception 
exception "file system exception"

# To print error  
error "The configuration file does not exist"  

# To print warning  
warning "You have to use higher version."

# To print info  
info "Task has been completed."	

# To print debug 
debug "This is a debug message."

```

### 2.1. Change the prompt color using tput

```
$ export PS1="\[$(tput bold)$(tput setb 4)$(tput setaf 7)\]\u@\h:\w $ \[$(tput sgr0)\]"			

```

## 3. dialog

```
$ sudo apt-get install dialog		

```

### 3.1. --inputbox

```
result=$(dialog --output-fd 1 --inputbox "Enter some text" 10 30)
echo Result=$result		

```

## 4. whiptail - display dialog boxes from shell scripts

### 4.1. --msgbox

```
whiptail --title "Example Dialog" --msgbox "This is an example of a message box. You must hit OK to continue." 8 78

```

```

 ┌─────────────────────────────┤ Example Dialog ├─────────────────────────────┐ 
 │                                                                            │ 
 │ This is an example of a message box. You must hit OK to continue.          │ 
 │                                                                            │ 
 │                                                                            │ 
 │                                   <Ok>                                     │ 
 │                                                                            │ 
 └────────────────────────────────────────────────────────────────────────────┘ 

```

### 4.2. --infobox

```
whiptail --title "Example Dialog" --infobox "This is an example of a message box. You must hit OK to continue." 8 78

```

### 4.3. --yesno

例 29.1. whiptail - yesno

```
#! /bin/bash
# http://archives.seul.org/seul/project/Feb-1998/msg00069.html
if (whiptail --title "PPP Configuration" --backtitle "Welcome to SEUL" --yesno "
Do you want to configure your PPP connection?"  10 40 )
then 
        echo -e "\nWell, you better get busy!\n"
elif    (whiptail --title "PPP Configuration" --backtitle "Welcome to
SEUL" --yesno "           Are you sure?" 7 40)
        then
                echo -e "\nGood, because I can't do that yet!\n"
        else
                echo -e "\nToo bad, I can't do that yet\n"
fi

```

```
whiptail --title "Example Dialog" --yesno "This is an example of a yes/no box." 8 78

exitstatus=$?
if [ $exitstatus = 0 ]; then
    echo "User selected Yes."
else
    echo "User selected No."
fi

echo "(Exit status was $exitstatus)"

```

设置--yes-button，--no-button，--ok-button 按钮的文本

```
whiptail --title "Example Dialog" --yesno "This is an example of a message box. You must hit OK to continue." 8 78 --no-button 取消 --yes-button 确认

```

### 4.4. --inputbox

例 29.2. whiptail - inputbox

```
result=$(tempfile) ; chmod go-rw $result
whiptail --inputbox "Enter some text" 10 30 2>$result
echo Result=$(cat $result)
rm $result

```

```

                         ┌────────────────────────────┐                         
                         │ Enter some text            │                         
                         │                            │                         
                         │ __________________________ │                         
                         │                            │                         
                         │                            │                         
                         │                            │                         
                         │    <Ok>        <Cancel>    │                         
                         │                            │                         
                         └────────────────────────────┘                         

```

```

COLOR=$(whiptail --inputbox "What is your favorite Color?" 8 78 --title "Example Dialog" 3>&1 1>&2 2>&3)

exitstatus=$?
if [ $exitstatus = 0 ]; then
    echo "User selected Ok and entered " $COLOR
else
    echo "User selected Cancel."
fi

echo "(Exit status was $exitstatus)"				

```

### 4.5. --passwordbox

例 29.3. whiptail - passwordbox

```

whiptail --title "Example Dialog" --passwordbox "This is an example of a password box. You must hit OK to continue." 8 78			

```

### 4.6. --textbox

例 29.4. whiptail - passwordbox

```

whiptail --title "Example Dialog" --textbox /etc/passwd 20 60

```

为文本取添加滚动条功能

```
whiptail --title "Example Dialog" --textbox /etc/passwd 20 60 --scrolltext

```

### 4.7. --checklist

例 29.5. whiptail - example 1

```
whiptail --title "Check list example" --checklist \
"Choose user's permissions" 20 78 16 \
"NET_OUTBOUND" "Allow connections to other hosts" ON \
"NET_INBOUND" "Allow connections from other hosts" OFF \
"LOCAL_MOUNT" "Allow mounting of local devices" OFF \
"REMOTE_MOUNT" "Allow mounting of remote devices" OFF

```

### 4.8. --radiolist

例 29.6. whiptail - radiolist

```
whiptail --title "Check list example" --radiolist \
"Choose user's permissions" 20 78 16 \
"NET_OUTBOUND" "Allow connections to other hosts" ON \
"NET_INBOUND" "Allow connections from other hosts" OFF \
"LOCAL_MOUNT" "Allow mounting of local devices" OFF \
"REMOTE_MOUNT" "Allow mounting of remote devices" OFF

```

### 4.9. --menu

```

whiptail --title "Menu example" --menu "Choose an option" 22 78 16 \
"<-- Back" "Return to the main menu." \
"Add User" "Add a user to the system." \
"Modify User" "Modify an existing user." \
"List Users" "List all users on the system." \
"Add Group" "Add a user group to the system." \
"Modify Group" "Modify a group and its list of members." \
"List Groups" "List all groups on the system."			

```

```

 ┌──────────────────────────────┤ Menu example ├──────────────────────────────┐ 
 │            <-- Back     Return to the main menu.                           │ 
 │            Add User     Add a user to the system.                          │ 
 │            Modify User  Modify an existing user.                           │ 
 │            List Users   List all users on the system.                      │ 
 │            Add Group    Add a user group to the system.                    │ 
 │            Modify Group Modify a group and its list of members.            │ 
 │            List Groups  List all groups on the system.                     │ 
 │                                                                            │ 
 │                                                                            │ 
 │                    <Ok>                        <Cancel>                    │ 
 │                                                                            │ 
 └────────────────────────────────────────────────────────────────────────────┘ 			

```

### 4.10. --gauge

```

#!/bin/bash
{
    for ((i = 0 ; i <= 100 ; i+=30)); do
        sleep 1
        echo $i
    done
} | whiptail --gauge "Please wait" 5 50 0	

```**