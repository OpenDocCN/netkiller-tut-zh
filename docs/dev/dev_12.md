# 第 46 章 Regular expression (正则表达式)

## Network 网络地址处理

```

$ wget -q -O - checkip.dyndns.org|sed -e 's/.*Current IP Address: //' -e 's/<.*$//'
202.130.101.34

$ curl -q -s http://checkip.dyndns.org | egrep -o '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+'
202.130.101.34

$ curl -q -s http://checkip.dyndns.org | egrep -o '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}'
202.130.101.34

```

## HTML 处理

url 抓取

```

$ curl -sq http://www.163.com | grep -o -E 'http://([^"#]+)'

```