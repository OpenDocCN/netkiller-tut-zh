# 部分 VI. Graphics

## 第 34 章 Gnuplot

[`gnuplot.info/`](http://gnuplot.info/)

## 安装 Gnuplot

### CentOS 环境

```
# yum install gnuplot

```

### Ubuntu 环境

```
$ sudo apt-get install gnuplot

```

### 测试 Gnuplot 是否可用

```
$ cat test.log
8:00 506.877
8:30 501.068
9:00 493.254
9:30 469.184
10:00 460.161
11:00 426.065
12:00 429.734
14:00 409.255
15:00 423.512
16:00 390.676
17:00 390.676
18:00 390.676

$ cat test.gnuplot
set terminal png truecolor size 800,250
set output "test.png"
set autoscale
set xdata time
set timefmt "%H:%M"
set style data lines
set xlabel "time per day"
set ylabel "Mbps"
set title "Apache Traffic"
set grid
plot "test.log" using 1:2 title "Hit"

$ gnuplot test.gnuplot

```

## terminal

set terminal png | gif | jpg

```
set terminal png

set terminal png truecolor size 800,600
set output "asa5550.png"

```

## output

```
set output "apache.png"

```

## title/xlabel/ylabel

```
set title "My first graph"
set xlabel "Angle, \n in degrees"
set ylabel "sin(angle)"  plot sin(x)

```

## xrange/yrange

```
set xrange [-pi:pi]  replot  reset
set xrange [-pi:pi]

set xrange [-0.5:3.5]
set yrange [-1:1.4]

```

### 时间轴范围

```

hour=$(date '+%H')
gnuplot << EOF
set terminal png truecolor size 800,480
set output "/www/example.com/www.example.com/silver-hour.png"
set autoscale
set xdata time
set timefmt "%H:%M"
set xrange ["$hour:00":"$hour:60"]
set format x "%H:%M"
set style data lines
set xlabel "$datetime GMT+800"
set ylabel "Ounce/USD"
set title "SILVER - http://www.example.com"
set grid
plot "/var/tmp/silver.log" using 1:2 title "SILVER"
EOF

```

### 日期轴范围

```

gnuplot << EOF
set terminal png truecolor size 800,480
set output "/www/example.com/www.example.com/silver-hour.png"
set autoscale
set xdata time
set timefmt "%m/%d/%y"
set xrange ["03/21/95":"03/22/95"]
set format x "%H:%M"
set style data lines
set xlabel "$datetime GMT+800"
set ylabel "Ounce/USD"
set title "SILVER - http://www.example.com"
set grid
plot "/var/tmp/silver.log" using 1:2 title "SILVER"
EOF

```

## xdata

```
set xdata time

```

### Date/Time

```
set xdata time
set timefmt "%m/%d/%y"
set xrange ["03/21/95":"03/22/95"]
set format x "%m/%d"

```

## plot

### using

```
可以在 using 里对数据进行简单的计算，例如：
plot 'test.dat' using ($1):($1*$1)

```

## PHPlot

[`sourceforge.net/projects/phplot/`](http://sourceforge.net/projects/phplot/)

## FAQ

### Could not find/open font when opening font "arial", using internal non-scalable font

```
# yum install liberation-sans-fonts

```

### 变量传递

```

images=test
gnuplot << EOF

set terminal png truecolor size 800,480
set output "$images.png"
set autoscale
set xdata time
set timefmt "%H:%M"
set format x "%H:%M"
set style data lines
set xlabel "2013-5-2 12:09 GMT+800"
set ylabel "Ounce/USD"
set title "http://www.example.com"
set grid
plot "$images.log" using 1:2 title "GOLD"
quit
EOF

```

## 第 35 章 Graphviz - Graph Visualization Software

[`www.graphviz.org/`](http://www.graphviz.org/)

## Installation

### Apt-get

to see all available graphviz packages.

```
$ apt-cache search graphviz |grep ^g
graphviz - rich set of graph drawing tools
graphviz-dev - transitional package for graphviz-dev rename
graphviz-doc - additional documentation for graphviz

$ apt-cache search graphviz |grep Graphviz
dot2tex - Graphviz to LaTeX converter
libgraph-easy-perl - Perl module to convert or render graphs (as ASCII, HTML, SVG or via Graphviz)
python-pydot - Python interface to Graphviz's dot
python-pygraphviz - Python interface to the Graphviz graph layout and visualization package
python-yapgvb - Python bindings for Graphviz, using Boost.Python
xdot - interactive viewer for Graphviz dot files

```

```
$ sudo apt install graphviz

```

Test, A "Hello World" example made by giving the command:

```
echo "digraph G {Hello->World}" | dot -Tpng >hello.png

```

### Yum

```
# yum list 'graphviz*'
# yum install graphviz

```

## The DOT Language

### dot

#### 布局

##### -Kv - Set layout engine to 'v' (overrides default based on command name)

主要用于有向图

```
dot 默认布局方式
neato 基于 spring-model(又称 force-based)算法   基于斥力+张力的布局
twopi 径向布局
circo 圆环布局
osage

```

无向图布局

```
fdp 用于无向图
sfdp 用于无向图

```

演示

```

 dot test.gv  -Kdot -Tpng -o test.png
 dot test.gv  -Kcirco -Tpng -o test.png
 dot test.gv  -Kneato -Tpng -o test.png
 dot test.gv  -Ktwopi  -Tpng -o test.png
 dot test.gv  -Ksfdp  -Tpng -o test.png
 dot test.gv  -Kosage  -Tpng -o test.png

```

### twopi

### gprof

## Node, Edge and Graph Attributes

### Color Names

http://www.graphviz.org/doc/info/colors.html

线颜色

```
	subgraph cluster_api {
		#rank=same;
		node [style=filled];
		label = "api";
		#color=blue
		api [label = "api.netkiller.cn"];
		api->redis [color="red"];
		api->mongodb [color="green"];
		api->oracle [color="blue"];

	}

```

### Node Shapes

http://www.graphviz.org/doc/info/shapes.html

### 箭头

```
digraph G {  
	A -> B [arrowhead="vee"]
	AA -> BB [dir="back" arrowtail="vee"]
	AAA -> BBB [dir="both" arrowhead="vee" arrowtail="odiamond"]
}

```

## Example

### E-R

```

$ cat erd.gv
digraph g {
graph [
rankdir = "LR"
];
node [
fontsize = "16"
shape = "ellipse"
];
edge [
];

"user" [
        label = "User| <id> id|username|password|last|status"
        shape = "record"
];

"profile" [
        label = "Profile| <id> id | name | sex | age | address | icq | msn"
        shape = "record"
];

user:id->profile:id [label="1:1"];

"category" [
        label = "Category| <id> id | <pid> pid | name | status"
        shape = "record"
];

category:pid->category:id [label="1:n"];

"article" [
        label = "Article| <id> id| <user_id> user_id | <cid> category_id | title | content | datetime | status"
        shape = "record"
];

article:user_id->user:id [label="1:n"];
article:cid->category:id [label="1:n"];

"feedback" [
        label = "Feedback| <id> id| <user_id> user_id | <article_id> article_id | title | content | datetime | status"
        shape = "record"
];

feedback:user_id->user:id [label="1:n"];
feedback:article_id->article:id [label="1:n"];

}

```

```

$ dot -Tpng erd.gv > erd.png

```

### Network

```

neo@neo-OptiPlex-380:~/Test/Graphviz$ cat network.gv
digraph network {

ranksep=5;
ratio=auto;

graph [
rankdir = "LR"
];

node [color=lightblue, style=filled];
"idc";
subgraph firewall {
        rank = same;
        node[shape=box,color=green];
        "ASA5550-Master" [ label="ASA5550-A|SSM-4GE-INC",shape="record",style="filled",color="green" ];
        "ASA5550-Slave" [ label="ASA5550-B",shape="hexagon",style="filled",color="green" ];
        "ASA5550-Master"->"ASA5550-Slave" [label="Failover"];
        "ASA5550-Master"->idc
        "ASA5550-Slave"->idc
}

subgraph switch {
        rank = same;

        "SW4507RA" [label="Cisco Catalyst 4507R|WS-X4648-RJ45V+E|WS-X4606-X2-E|WS-X45-SUP7-E|WS-X4712-SFP+E" shape = "record"];
        "SW4507RB" [label="Cisco Catalyst 4507R" shape = "record"];
        "SW4507RA"->"SW4507RB" [label="HSRP"];
        "ASA5550-Master"->"SW4507RA" [label="1GB"];
        "ASA5550-Slave"->"SW4507RB" [label="1GB"];

        "SW4507RA"->O8
        "SW4507RB"->O8

        "O8"->O4
        "O8"->O7
        "O8"->O9

        "SW4507RA"->J9 [ label = "SFP+ 10G" ];
        "SW4507RA"->J10;
        "SW4507RA"->J11;
        "SW4507RA"->J12;
        "SW4507RA"->J13;
        "SW4507RA"->J14;
        "SW4507RA"->J15;
        "SW4507RA"->M12;

        "SW4507RB"->J9;
        "SW4507RB"->J10;
        "SW4507RB"->J11;
        "SW4507RB"->J12;
        "SW4507RB"->J13;
        "SW4507RB"->J14;
        "SW4507RB"->J15;
        "SW4507RB"->M12;
}

subgraph slb {
        rank = 2;
        slb1 [label="F5-Master",shape=circle];
        slb2 [label="F5-Backup",shape=circle];
        slb1->"SW4507RA";
        slb2->"SW4507RB";
        slb1->slb2 [label="VRRP"];
"10.10.0.3"    [label="cms.example.com preview.example.com publish.example.com"];
"10.10.0.4"    [label="media.example.com"];
"10.10.0.5"    [label="portal.example.com my.example.com login.example.com"];
"10.10.0.6"    [label="sso.example.com"];

slb1->"10.10.0.3"
slb1->"10.10.0.4"
slb1->"10.10.0.5"
slb1->"10.10.0.6"
slb1->"10.10.0.7"
slb1->"10.10.0.8"
slb1->"10.10.0.9"

}
subgraph service {
        nfs [label="NFSv4 NAS"];
        server->nfs;
}

subgraph server {
        rank = same;
        "10.10.10.2" [label="Monitor"];
        "10.10.10.3" [label="Backup"];
}

subgraph lvs {
        "10.10.10.6";

}

"O9"->"10.10.10.2" [label="Monitor"];
"O9"->"10.10.10.3" [label="Backup"];
"O9"->"10.10.10.5";
"O9"->"10.10.10.7";
"O9"->"10.10.10.14";
"O9"->"10.10.10.15";
"O9"->"10.10.10.11";
"O9"->"10.10.10.12";
"O9"->"10.10.10.27";
"O9"->"10.10.10.28";
"O9"->"10.10.10.71";
"O9"->"10.10.10.72";

"O8"->"10.10.10.20";
"O8"->"10.10.10.23";
"O8"->"10.10.10.19";
"O8"->"10.10.10.10";
"O8"->"10.10.10.74";
"O8"->"10.10.10.74";
"O8"->"10.10.10.75";
"O8"->"10.10.10.76";
"O8"->"10.10.10.216";

"O7"->"10.10.10.16";
"O7"->"10.10.10.46";
"O7"->"10.10.10.47";
"O7"->"10.10.10.48";

"O4"->"10.10.10.41";
"O4"->"10.10.10.42";
"O4"->"10.10.10.54";

"J9"->"10.10.0.21";
"J9"->"10.10.0.22";
"J9"->"10.10.0.23";
"J9"->"10.10.0.24";
"J9"->"10.10.0.25";
"J9"->"10.10.0.26";
"J9"->"10.10.0.27";
"J9"->"10.10.0.28";
"J9"->"10.10.0.29";
"J9"->"10.10.0.30";
"J9"->"10.10.0.31";
"J9"->"10.10.0.32";

"J10"->"10.10.0.41";
"J10"->"10.10.0.42";
"J10"->"10.10.0.43";
"J10"->"10.10.0.44";
"J10"->"10.10.0.45";
"J10"->"10.10.0.46";
"J10"->"10.10.0.47";
"J10"->"10.10.0.48";
"J10"->"10.10.0.49";
"J10"->"10.10.0.50";
"J10"->"10.10.0.51";
"J10"->"10.10.0.52";

"J11"->"10.10.0.61";
"J11"->"10.10.0.62";
"J11"->"10.10.0.63";
"J11"->"10.10.0.64";

"J12"->"10.10.0.254";
"J12"->"10.10.0.250";

"J13"->"10.10.0.81";
"J13"->"10.10.0.82";
"J13"->"10.10.0.83";
"J13"->"10.10.0.84";
"J13"->"10.10.0.85";
"J13"->"10.10.0.86";
"J13"->"10.10.0.87";
"J13"->"10.10.0.88";
"J13"->"10.10.0.89";
"J13"->"10.10.0.90";
"J13"->"10.10.0.91";
"J13"->"10.10.0.92";
"J13"->"10.10.0.93";

"J14"->"10.10.0.101";
"J14"->"10.10.0.102";
"J14"->"10.10.0.103";
"J14"->"10.10.0.104";
"J14"->"10.10.0.105";
"J14"->"10.10.0.106";
"J14"->"10.10.0.107";
"J14"->"10.10.0.108";
"J14"->"10.10.0.53";
"J14"->"10.10.0.54";

"J15"->"10.10.5.10";
"J15"->"10.10.5.11";
"J15"->"10.10.5.12";
"J15"->"10.10.5.13";
"J15"->"10.10.5.14";
"J15"->"10.10.5.15";
"J15"->"10.10.5.16";
"J15"->"10.10.5.17";
"J15"->"10.10.5.18";
"J15"->"10.10.5.19";

"M12"->"10.10.0.121";
"M12"->"10.10.0.122";
"M12"->"10.10.0.123";
"M12"->"10.10.0.124";
"M12"->"10.10.0.125";
"M12"->"10.10.0.126";
"M12"->"10.10.0.127";
"M12"->"10.10.0.128";
"M12"->"10.10.0.129";
"M12"->"10.10.0.130";
"M12"->"10.10.0.131";
"M12"->"10.10.0.132";
"M12"->"10.10.0.133";
}

```

```
$ twopi network.gv -Tpng > network.png

```

### workflow

```

/*
dot -Tpng workflow.gv -o workflow.png
*/
digraph workflow {
	graph
	[
	 ratio="auto"
	 label="User Login & Create Article Workflow"
	 labelloc=t
	 fontname="simyou.ttf"
	];
	node[shape=box,width=2];
	subgraph cluster_0 {
			style=filled;
			node [style=filled,color=white,fontcolor=blue];
			label="Login";
			color=lightgray;
			User -> Password -> "Sign in" [color=red];
	}
	subgraph cluster_1 {
			label="Article";
			color=black;
			Title -> Text -> Author -> Data -> Submit;
	}
	subgraph cluster_2 {
			style=filled;
			label="Auth";
			"Query db" [shape=parallelogram];
			"set cookie & session" [shape=parallelogram];
			"redirect" [shape=parallelogram];
			"Query db" -> "set cookie & session" -> "redirect";
	}
	Start -> Login;
	Login->User [label="N"];
	"Sign in"->"Query db";
	redirect->Title [style=dotted];
	Login->Title [label="Y"];

	User -> Author [style=dotted];

	Submit->End;

	Login [shape=diamond];
	Start [shape=circle,width=1];
	End	[shape=circle,width=1];
}

```

## 第 36 章 RRDTool

[`www.mrtg.org/rrdtool/`](http://www.mrtg.org/rrdtool/)

## install

```
$ apt-get install rrdtool

```

## rrdtool demo example

```

rrdtool create datafile.rrd \
	DS:packets:ABSOLUTE:900:0:10000000 \
	RRA:AVERAGE:0.5:1:9600 \
	RRA:AVERAGE:0.5:4:9600 \
	RRA:AVERAGE:0.5:24:6000

```

```

rrdtool update datafile.rrd N:100
rrdtool update datafile.rrd N:200
rrdtool update datafile.rrd N:300

```

or

```

for (( ; ; )) do
	rrdtool update datafile.rrd N:$( echo `< /dev/urandom tr -dc [:digit:] | head -c 3`)
	sleep 5
done &

```

```

rrdtool graph graph.png DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## title

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## start / end

```
--start -1d gives a graph of the last day;
--start -1w gives a graph of the last week;
--start -1m gives a graph of the last month;
--start -1y gives a graph of the last year;

#! /bin/sh

cd /var/log/rrd
rrds=`find . -type f -name '*.rrd' | cut -c3-`

for i in $rrds;
  do
  j=`echo $i | sed 's/.rrd//'`
  rrdtool graph /var/www/rrd/$j-day.png --start -1d DEF:pkt=$i:packet:AVERAGE LINE1:pkt#ff0000:Packets/sec
  rrdtool graph /var/www/rrd/$j-week.png --start -1w DEF:pkt=$i:packet:AVERAGE LINE1:pkt#ff0000:Packets/sec
  rrdtool graph /var/www/rrd/$j-month.png --start -1m DEF:pkt=$i:packet:AVERAGE LINE1:pkt#ff0000:Packets/sec
  rrdtool graph /var/www/rrd/$j-year.png --start -1y DEF:pkt=$i:packet:AVERAGE LINE1:pkt#ff0000:Packets/sec
done
cd -

```

```
--start "yesterday"
--start "-1month"
--start "-2weeks"
--start "-1year"
--start -86400 (24*60*60=86400)

```

end

```
rrdtool graph graph.png --title="Test Graph" --start=0 --end=start+86400 --width=800 DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## height / width

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## upper-limit / lower-limit

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 --lower-limit=0 --upper-limit=1000 DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## vertical-label

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 --vertical-label="Bits per second" DEF:pkt=datafile.rrd:packets:AVERAGE \
  LINE1:pkt#ff0000:Packets

```

## Data Source

Data Source Fields: DS:DS-Name:DST:HeartBeat:Min:Ma

## Round Robin Archives

Round Robin Archives: RRA:CF:XFF:Steps:Row

## AREA, LINE and STACK

### LINE

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 --vertical-label="Bits per second" \
	DEF:pkt=datafile.rrd:packets:AVERAGE \
	LINE1:pkt#ff0000:Packets

```

### AREA

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 --vertical-label="Bits per second" \
	DEF:pkt=datafile.rrd:packets:AVERAGE \
	AREA:pkt#ff0000:Packets

```

### STACK

```
rrdtool graph graph.png --title="Test Graph" --height=400 --width=800 --vertical-label="Bits per second" \
	DEF:pkt=datafile.rrd:packets:AVERAGE \
	LINE1:pkt#ff0000:Packets \
	STACK:pkt#0000ff:Packets

AREA:x1#FF0000:x1
STACK:x2#0000FF:x2

LINE2:x1#FF0000:x1
STACK:x2#0000FF:x2+x1

LINE2:x1#FF0000:x1
AREA:x2#0000FF:x2:STACK

```

### GPRINT

## Example

### Memory

```
rrdtool create memory.rrd \
        --start 1023654125 \
        --step 300 \
        DS:mem:GAUGE:600:0:671744 \
        RRA:AVERAGE:0.5:12:24 \
        RRA:AVERAGE:0.5:288:31

```

```

for (( ; ; )) do
	memory=$(snmpwalk -c public -v2c 172.16.1.10 hrSWRunPerfMem | awk 'BEGIN {total_mem=0} { if ($NF == "KBytes") {total_mem=total_mem+$(NF-1)}}  END {print total_mem}')
	rrdtool update memory.rrd N:${memory}
	sleep 300
done &

```

```
rrdtool graph memory.png \
	--title="Memory Usage" \
	--vertical-label="Memory Consumption (MB)" \
	--start=0 \
	--end=start+1day \
	--color=BACK#CCCCCC \
	--color=CANVAS#CCFFFF \
	--color=SHADEB#9999CC \
	--height=125 \
	--upper-limit=656 \
	--lower-limit=0 \
	--rigid \
	--base=1024 \
	DEF:tot_mem=memory.rrd:mem:AVERAGE \
	CDEF:tot_mem_cor=tot_mem,0,671744,LIMIT,UN,0,tot_mem,IF,1024,/ \
	CDEF:machine_mem=tot_mem,656,+,tot_mem,- \
	HRULE:656#000000:"Maximum Available Memory - 656 MB" \
	AREA:machine_mem#CCFFFF:"Memory Unused" \
	AREA:tot_mem_cor#6699CC:"Total memory consumed in MB"

```

### example 1

```
rrdtool create test.rrd             \
            --start 920804400          \
            DS:speed:COUNTER:600:U:U   \
            RRA:AVERAGE:0.5:1:24       \
            RRA:AVERAGE:0.5:6:10

rrdtool update test.rrd 920804700:12345 920805000:12357 920805300:12363
rrdtool update test.rrd 920805600:12363 920805900:12363 920806200:12373
rrdtool update test.rrd 920806500:12383 920806800:12393 920807100:12399
rrdtool update test.rrd 920807400:12405 920807700:12411 920808000:12415
rrdtool update test.rrd 920808300:12420 920808600:12422 920808900:12423

rrdtool fetch test.rrd AVERAGE --start 920804400 --end 920809200

rrdtool graph speed.png                                 \
         --start 920804400 --end 920808000               \
         DEF:myspeed=test.rrd:speed:AVERAGE              \
         LINE2:myspeed#FF0000

rrdtool graph speed2.png                           \
      --start 920804400 --end 920808000               \
      --vertical-label m/s                            \
      DEF:myspeed=test.rrd:speed:AVERAGE              \
      CDEF:realspeed=myspeed,1000,\*                  \
      LINE2:realspeed#FF0000

rrdtool graph speed3.png                             \
      --start 920804400 --end 920808000               \
      --vertical-label km/h                           \
      DEF:myspeed=test.rrd:speed:AVERAGE              \
      "CDEF:kmh=myspeed,3600,*"                       \
      CDEF:fast=kmh,100,GT,kmh,0,IF                   \
      CDEF:good=kmh,100,GT,0,kmh,IF                   \
      HRULE:100#0000FF:"Maximum allowed"              \
      AREA:good#00FF00:"Good speed"                   \
      AREA:fast#FF0000:"Too fast"

rrdtool graph speed4.png                           \
      --start 920804400 --end 920808000               \
      --vertical-label km/h                           \
      DEF:myspeed=test.rrd:speed:AVERAGE              \
      CDEF:nonans=myspeed,UN,0,myspeed,IF             \
      CDEF:kmh=nonans,3600,*                          \
      CDEF:fast=kmh,100,GT,100,0,IF                   \
      CDEF:over=kmh,100,GT,kmh,100,-,0,IF             \
      CDEF:good=kmh,100,GT,0,kmh,IF                   \
      HRULE:100#0000FF:"Maximum allowed"              \
      AREA:good#00FF00:"Good speed"                   \
      AREA:fast#550000:"Too fast"                     \
      STACK:over#FF0000:"Over speed"

   rrdtool create all.rrd --start 978300900 \
            DS:a:COUNTER:600:U:U \
            DS:b:GAUGE:600:U:U \
            DS:c:DERIVE:600:U:U \
            DS:d:ABSOLUTE:600:U:U \
            RRA:AVERAGE:0.5:1:10
   rrdtool update all.rrd \
            978301200:300:1:600:300    \
            978301500:600:3:1200:600   \
            978301800:900:5:1800:900   \
            978302100:1200:3:2400:1200 \
            978302400:1500:1:2400:1500 \
            978302700:1800:2:1800:1800 \
            978303000:2100:4:0:2100    \
            978303300:2400:6:600:2400  \
            978303600:2700:4:600:2700  \
            978303900:3000:2:1200:3000
   rrdtool graph all1.png -s 978300600 -e 978304200 -h 400 \
            DEF:linea=all.rrd:a:AVERAGE LINE3:linea#FF0000:"Line A" \
            DEF:lineb=all.rrd:b:AVERAGE LINE3:lineb#00FF00:"Line B" \
            DEF:linec=all.rrd:c:AVERAGE LINE3:linec#0000FF:"Line C" \
            DEF:lined=all.rrd:d:AVERAGE LINE3:lined#000000:"Line D"

```

### example 1

```
rrdtool create seconds1.rrd   \
  --start 920804700          \
  DS:seconds:COUNTER:600:U:U \
  RRA:AVERAGE:0.5:1:24

rrdtool update seconds1.rrd \
  920805000:000 920805300:300 920805600:600 920805900:900
rrdtool update seconds1.rrd \
  920806000:000 920806300:300 920806603:603 920806900:900

rrdtool graph seconds1.png                       \
  --start 920804700 --end 920806200             \
  --height 200                                  \
  --upper-limit 1.05 --lower-limit 0.95 --rigid \
  DEF:seconds=seconds1.rrd:seconds:AVERAGE      \
  CDEF:unknown=seconds,UN                       \
  LINE2:seconds#0000FF                          \
  AREA:unknown#FF0000

```

## 第 37 章 OpenBR

开源生物特征识别库 OpenBR (面部识别)

http://openbiometrics.org/

## 第 38 章 OCR - Optical Character Recognition

[`help.ubuntu.com/community/OCR`](https://help.ubuntu.com/community/OCR)

## Tesseract

查找 Tesseract 安装包

```
$ apt-cache search Tesseract
ocrodjvu - tool to perform OCR on DjVu documents
slimrat - GUI application for automated downloading from file hosters
slimrat-nox - CLI application for automated downloading from file hosters
tesseract-ocr - Command line OCR tool
tesseract-ocr-deu - tesseract-ocr language files for German text
tesseract-ocr-deu-f - tesseract-ocr language files for the German Fraktur script
tesseract-ocr-dev - Development files for the tesseract command line OCR tool
tesseract-ocr-eng - tesseract-ocr language files for English text
tesseract-ocr-fra - tesseract-ocr language files for French text
tesseract-ocr-ita - tesseract-ocr language files for Italian text
tesseract-ocr-nld - tesseract-ocr language files for Dutch text
tesseract-ocr-por - tesseract-ocr language files for Brasilian Portuguese text
tesseract-ocr-spa - tesseract-ocr language files for Spanish text
tesseract-ocr-vie - tesseract-ocr language files for Vietnamese text

```

```
$ sudo apt-get install tesseract-ocr

```

```
$ convert test.jpg test.tif
$ tesseract test.tif test
$ cat test.txt

```

## cuneiform - multi-language OCR system

## 第 39 章 Open-Source tool in Java to draw UML Diagram

http://plantuml.sourceforge.net/

## 第 40 章 Asymptote: The Vector Graphics Language

http://asymptote.sourceforge.net/index.html

```
$ sudo apt-get install asymptote

```

## UML

http://code.google.com/p/sml4asy/

```
wget http://sml4asy.googlecode.com/files/sml4asy-0.01.tar.gz
tar zxvf sml4asy-0.01.tar.gz
sudo scp sml4asy-0.01/asy/* /usr/share/asymptote/

```

test

```
asy sml4asy-0.01/examples/HelloSML.asy
$ convert HelloSML.eps HelloSML.png

```

## 第 41 章 MetaPost

## 第 42 章 OpenStreetMap

[`www.openstreetmap.org/`](https://www.openstreetmap.org/)

## OpenLayers

[`openlayers.org/`](http://openlayers.org/)

## Leaflet

[`leafletjs.com/`](http://leafletjs.com/)

```
map.setCenter(center,18);
前面的 center 好理解，后面的 18 是个 z 参数，是一个表示缩放级别的数字，该参数的值范围为 0 到 17。缩放级别 0 表示最低的缩放级别（显示整个地球），增加该数字可以进一步放大。

```

## 第 43 章 Baidu Map

## BMap.Circle

```
	var point = new BMap.Point(22.111, 114.111);
        var styleCircleF = {  
        strokeColor:"red",    	//边线颜色。  
        fillColor:"red",      	//填充颜色。当参数为空时，圆形将没有填充效果。  
        strokeWeight: 1,       	//边线的宽度，以像素为单位。  
        strokeOpacity: 1,    	//边线透明度，取值范围 0 - 1。  
        fillOpacity: 0.3,      	//填充的透明度，取值范围 0 - 1。  
        strokeStyle: 'dashed' 	//边线的样式，solid 或 dashed。  
    } 
    //画圆
    var circleF = new BMap.Circle(point,50000,styleCircleF);

```