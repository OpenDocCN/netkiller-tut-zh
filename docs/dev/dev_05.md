# 部分 III. Node.js

## 第 22 章 Node.js 安装

[`nodejs.org/`](http://nodejs.org/)

## Ubuntu

```
$ sudo apt-get install nodejs
$ sudo apt-get install npm

```

## CentOS

```
[root@netkiller www]# yum install -y nodejs
[root@netkiller www]# node --version
v6.9.1

```

## npm -- node package manager

npm registry

```
$ npm install mysql

```

### link

```
# npm link gulp
/root/node_modules/gulp -> /srv/node-v7.10.0-linux-x64/lib/node_modules/gulp

```

## pm2

### Production process manager for Node.js apps with a built-in load balancer http://pm2.io

```
npm install -g pm2		

```

### logs

```
$ pm2 logs project --lines 100			

```

## Loop

### forEach

```

arr.forEach(function (item) {
  someFn(item);
})

elements.forEach(function(element){

});

```

## 第 23 章 Meteor

## 第 24 章 express