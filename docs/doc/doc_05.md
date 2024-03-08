# 第 28 章 Jekyll

[`jekyllrb.com/`](http://jekyllrb.com/)

## 1. 安装 Jekyll

### 1.1. Ubuntu

```
sudo apt-get install ruby jekyll

```

## 2. 创建 Jekyll 项目

```
$ jekyll new my-web-site 
$ cd my-web-site 
$ jekyll serve 	

```

http://192.168.4.7:4000/

## 3. permalink

_config.yml：配置 permalink 格式

```
permalink: /:year/:month/:day/:title

```

## 4. _post

样式

```
layout: post		

```

标题

```
title:  "FreeBSD"		

```

文章分类

```
categories: ebook

```

## 5. category / categories

使用 Category 进行文章分类

```
使用 {{ category | first }} 输出分类的名称 
使用 {{ category | last | size }} 输出该分类下文章的数目 
遍历 category.last 输出所有文章的信息，构建到该文章的索引目录		

```

单一分类

```
---
layout: Template
title: Category Sample
category: computer
---

```

多个分类

```
---
layout: Template
title: Category Sample
categories: [linux, freebsd]
---

```

显示分类名称

```
{% for category in site.categories %}
	{{ category | first }}
{% endfor %}   		

```

```

{% for category in site.categories %}
	<h2>{{ category | first }}</h2> </span>{{ category | last | size }}</span> 
	<ul class="arc-list">
		{% for post in category.last %}
		<li>{{ post.date | date:"%d/%m/%Y"}}<a href="{{ post.url }}">{{ post.title }}</a></li>
		{% endfor %}
	</ul>
{% endfor %}

```