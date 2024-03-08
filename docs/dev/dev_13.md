# 第 47 章 Miscellaneous

## php function check

```

#!/bin/bash
LOGFILE=/tmp/my.log
echo > $LOGFILE
for helper in `ls -1 class/helper/`
do
    echo ========================== $helper ============================ >> $LOGFILE
    class=`grep '^class' class/helper/$helper | awk -F ' ' '{print $2}'`
    for fun in `grep 'public function [a-zA-Z]' class/helper/$helper | awk -F ' ' '{print $3}' | awk -F '(' '{print $1}'`
    do
        count=`grep -r "$class->$fun(" *|wc -w`
        if [ $count == 0 ]; then
               echo "[ unused ] $class->$fun" >> $LOGFILE
        else

               echo "[  used  ] $class->$fun" >> $LOGFILE
        fi
        echo "[`date`] [$helper] $class->$fun (checked: $count)"
    done
done

```

## whois 域名查询

```

<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
<title>whois</title>
</head>
<body>
<fieldset>
<legend>whois</legend>
<form name="form1" method="post" action="<? $PHP_SELF ?>">
<input type="text" name="domainname">
.cn
<input type="submit" name="Submit" value="查询">
</form>
</fieldset>

查询域名：
<?echo $domainname;?>
.cn
<?php
$fp = fsockopen ("whois.cnnic.cn", 43 , $errno, $errstr, 30);
if (!$fp) {
echo "$errstr ($errno)<br>\n";
} else {
fputs ($fp, "$domainname".".cn"."\r\n");
echo "<pre>";
while (!feof($fp)) {

$data = fgets ($fp,1024);
$data = str_replace("no matching record", "该域名没有被注册\n<a href='http://www.cnwwwcn.com'>我想注册该域名</a>", $data);
/*
$data = fgetc ($fp);

if($data == "\n"){
echo "<br>";
}
*/
echo $data;
//no matching record
}
echo "</pre>";
fclose ($fp);
}
?>
</body>
</html>

```

## 身份证校验

```

<?php 

function check_id_number($no){
	if (strlen($no) != 18){
		return false;
	}
	$sigma = 0;
	$wi = array(7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2); 
	$ai = array('1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'); 
	for ($i = 0;$i < 17;$i++) { 
	    $sigma += ((int) $no{$i}) * $wi[$i]; 
	}

	if (substr($no,17) == $ai[($sigma % 11)]){	
		return true;
	}else{
		return false; 
	}
}

echo check_id_number('330702198003090915');		

```

## PHP PDF 处理库

TCPDF is a FLOSS PHP class for generating PDF documents.

http://www.tcpdf.org/

## Kint - a modern and powerful PHP debugging helper

http://raveren.github.io/kint/

## snoopy 模拟浏览器操作

http://snoopy.sourceforge.net/

## PHP Nightrain

Using PHP Nightrain you will be able to deploy and run HTML, CSS, JavaScript and PHP web applications as a native desktop application on Windows, Mac and the Linux operating systems. Popular PHP Frameworks (e.g. CakePHP, Laravel, Drupal, etc…) are well supported!

http://www.naetech.com/php-nightrain