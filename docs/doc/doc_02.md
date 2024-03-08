# 部分 I. Docbook

[`www.docbook.org/`](http://www.docbook.org/)

http://docbook.org/tdg51/en/html/docbook.html

## 第 1 章 Open Document Licenses

Creative Commons Attribution 3.0

GNU Free Documentation License

Open Publication License

The FreeBSD Documentation License

## 第 2 章 Document Tools

Docbook SGML,XML 转换程序

## 1. SGML

```
				sudo apt-get install docbook-utils

```

CentOS

```
				# yum install docbook*

```

## 2. XML 转换

docbook-xsl-1.73.2.tar.gz

docbook-dsssl-1.79.tar.gz

rxp - A validating XML parser

rxp -s file.xml

To validate file.xml, use the command:

$ rxp -s -V file.xml

### 2.1. xsltproc - XSLT command line processor

xsltproc --stringparam html.stylesheet docbook.css ../../docbook-xsl-1.73.2/xhtml/chunk.xsl ../book.xml

```
					$ sudo apt-get install docbook-xsl
					$ export DSSSL=/usr/share/xml/docbook/stylesheet/nwalsh/xhtml/chunk.xsl
					$ /usr/bin/xsltproc --stringparam html.stylesheet docbook.css ${DSSSL} ../book.xml

```

### 2.2. docbook-ebnf - EBNF module for the XML version of the DocBook DTD

docbook-ebnf - EBNF module for the XML version of the DocBook DTD

```
					$ sudo apt-get install docbook-ebnf

```

### 2.3. docbook-xsl-saxon

```
					$ sudo apt-get install docbook-xsl-saxon

```

创建一个 test.xml 的测试文件

```

SRCS = test.xml

DESTDIR = .

all: html

html: $(SRCS:.xml=.noext.html) $(SRCS:.xml=.html)

%.png : %.png.uu
	[ -d ${DESTDIR} ] || mkdir -p ${DESTDIR}
	uudecode -o /dev/stdout < $< > ${DESTDIR}/$@

%.html : %.xml
	[ -d ${DESTDIR} ] || mkdir -p ${DESTDIR}
	java -cp "/usr/share/java/saxon.jar:/usr/share/java/xslthl.jar:/usr/share/java/docbook-xsl-saxon.jar" \
	  -Dhighlight.xslthl.config="/usr/share/xml/docbook/stylesheet/docbook-xsl/highlighting/xslthl-config.xml" \
	  com.icl.saxon.StyleSheet \
	  -u -o ${DESTDIR}/$@ $< db2html.xsl \
	  highlight.source=1

%.noext.html : %.xml
	[ -d ${DESTDIR} ] || mkdir -p ${DESTDIR}
	xsltproc --xinclude --nonet -o ${DESTDIR}/$@ \
			--stringparam highlight.source 1 \
			--stringparam xslthl.config /usr/share/xml/docbook/stylesheet/docbook-xsl/highlighting/xslthl-config.xml \
			--param use.extensions 0 \
			--stringparam  paper.type A4 \
			db2html.xsl $<

validate: check

check:
	xmllint --xinclude --nonet --noout --postvalid $(SRCS)

clean:
	rm -f ${DESTDIR}/*.html ${DESTDIR}/*.png

.PHONY: all check clean html validate

```

生成 html

```
					cp /usr/share/doc/docbook-xsl-saxon/examples/db2html.xsl 。

					make html

```

## 3. XML 校验

```
				$ xmllint book.xml

```

## 4. PDF

```
				sudo apt-get install docbook-xml docbook-xsl xsltproc fop

```

安装字体

```
				sudo mkdir /usr/share/fonts/microsoft

```

将 C:\Windows\Fonts 目录中的字体复制到 /usr/share/fonts/microsoft

```
				$ java -cp /usr/share/java/fop.jar org.apache.fop.fonts.apps.TTFReader /usr/share/fonts/microsoft/simhei.ttf simhei.xml
				$ java -cp /usr/share/java/fop.jar org.apache.fop.fonts.apps.TTFReader -ttcname "SimSun" /usr/share/fonts/microsoft/simsun.ttc simsun.xml

```

```

sudo vim /usr/share/xml/docbook/stylesheet/docbook-xsl/fo/param.xsl
<xsl:param name="callout.unicode.font">simsun</xsl:param>
<xsl:param name="symbol.font.family">simsun</xsl:param>
<xsl:param name="callout.unicode.font">simsun</xsl:param>

```

```

$ vim fop.conf

<?xml version="1.0"?>
<fop version="1.0">
<base>.</base>

<renderers>
	<renderer mime="application/pdf">
     <filterList>
         <value>flate</value>
     </filterList>
      <fonts>
        <font metrics-url="simhei.xml" kerning="yes" embed-url="/usr/share/fonts/microsoft/simhei.ttf">
          <font-triplet name="simhei" style="normal" weight="normal"/>
          <font-triplet name="simhei" style="normal" weight="bold"/>
          <font-triplet name="simhei" style="italic" weight="normal"/>
          <font-triplet name="simhei" style="italic" weight="bold"/>
          </font>
          <font metrics-url="simsun.xml" kerning="yes" embed-url="/usr/share/fonts/microsoft/simsun.ttc">
          <font-triplet name="simsun" style="normal" weight="normal"/>
          <font-triplet name="simsun" style="normal" weight="bold"/>
          <font-triplet name="simsun" style="italic" weight="normal"/>
          <font-triplet name="simsun" style="italic" weight="bold"/>
          </font>
        </fonts>
    </renderer>
</renderers>
</fop>

```

```
				xsltproc -o helloworld.fo /usr/share/xml/docbook/stylesheet/docbook-xsl/fo/docbook.xsl helloworld.xml
				fop -c fop.conf helloworld.fo -pdf helloworld.pdf

```

## 5. CHM

下载 [HTML Help Wordshop](http://download.microsoft.com/download/0/a/9/0a939ef6-e31c-430f-a3df-dfae7960d564/htmlhelp.exe) 工具，并安装

```
				copy “c:\Program Files\HTML Help Workshop\hhc.exe” d:\docbook\example
				xsltproc –xinclude docbook_chm.xsl docbook.xml
				hhc.exe htmlhelp.hhp

```

```

set PATH=C:\Program Files (x86)\HTML Help Workshop;%PATH%
hhc htmlhelp.hhp		

```

## 6. docbook2odf - Convert docbook document to Oasis Open Document

```
				sudo apt-get install docbook2odf

```

```
				docbook2odf book.xml

```

## 7. docbook-website - XML Website DTD and XSL Stylesheets

```
sudo apt-get install docbook-website

```

```
#!/bin/bash
export DSSSL=/usr/share/xml/docbook/stylesheet/nwalsh/xhtml/onechunk.xsl
mkdir tmp
cd tmp
xsltproc --stringparam html.stylesheet docbook.css ${DSSSL} ../book.xml

```

## 8. EPUB

### 8.1. 创建.epub 文件

```

<?xml version="1.0" encoding="utf-8"?>
<book>
  <bookinfo>
    <title>My EPUB book</title>
    <author><firstname>Liza</firstname>
            <surname>Daly</surname></author>
    <volumenum>1234</volumenum>
  </bookinfo>
  <preface id="preface">
    <title>Title page</title>
    <figure id="cover-image">
      <title>Our EPUB cover image icon</title>
      <graphic fileref="cover.png"/>
    </figure>
  </preface>
  <chapter id="chapter1">
    <title>This is a pretty simple DocBook example</title>
    <para>
      Not much to see here.
    </para>
  </chapter>
  <chapter id="end-notes">
    <title>End notes</title>
    <para>
      This space intentionally left blank.
    </para>
  </chapter>
</book>

```

```
$ xsltproc /usr/share/xml/docbook/stylesheet/docbook-xsl/epub/docbook.xsl docbook.xml
$ echo "application/epub+zip" > mimetype
$ zip -0Xq  my-book.epub mimetype
$ zip -Xr9D my-book.epub *

```

### 8.2. Reader

http://calibre-ebook.com/

## 9. Apple Mac

安装 dtd 文件

```
				neo@MacBook-Pro ~ % brew install docbook

```

安装目录 /usr/local/Cellar/docbook/5.0/docbook/xml/5.0

安装 xsl 文件

```
				neo@MacBook-Pro ~ % brew install docbook-xsl

```

安装目录/usr/local/Cellar/docbook-xsl/1.79.1/docbook-xsl/

```

neo@MacBook-Pro /tmp/doc % cat sample.xml 
<?xml version="1.0" encoding="utf-8"?>
<book>
  <bookinfo>
    <title>My EPUB book</title>
    <author><firstname>Liza</firstname>
            <surname>Daly</surname></author>
    <volumenum>1234</volumenum>
  </bookinfo>
  <preface id="preface">
    <title>Title page</title>
    <figure id="cover-image">
      <title>Our EPUB cover image icon</title>
      <graphic fileref="cover.png"/>
    </figure>
  </preface>
  <chapter id="chapter1">
    <title>This is a pretty simple DocBook example</title>
    <para>
      Not much to see here.
    </para>
  </chapter>
  <chapter id="end-notes">
    <title>End notes</title>
    <para>
      This space intentionally left blank.
    </para>
  </chapter>
</book>		

```

```

neo@MacBook-Pro /tmp/doc % xsltproc /usr/local/Cellar/docbook-xsl/1.79.1/docbook-xsl/html/chunk.xsl sample.xml
Writing pr01.html for preface(preface)
Writing ch01.html for chapter(chapter1)
Writing ch02.html for chapter(end-notes)
Writing index.html for book

neo@MacBook-Pro /tmp/doc % ll
total 40
-rw-r--r--  1 neo  wheel   1.6K Sep 28 16:51 ch01.html
-rw-r--r--  1 neo  wheel   1.5K Sep 28 16:51 ch02.html
-rw-r--r--  1 neo  wheel   1.8K Sep 28 16:51 index.html
-rw-r--r--  1 neo  wheel   1.8K Sep 28 16:51 pr01.html
-rw-r--r--  1 neo  wheel   710B Sep 28 16:51 sample.xml		

```

## 第 3 章 docbook-xsl

## 1. example

```

<?xml version='1.0' encoding="utf-8"?>
<xsl:stylesheet  version="1.0">
   	<xsl:import href="/usr/share/xml/docbook/stylesheet/nwalsh/xhtml/chunk.xsl"/>
   	<xsl:param name="admon.graphics" 		select="1"/>
   	<xsl:param name="admon.graphics.path">/graphics/</xsl:param>
   	<xsl:param name="admon.graphics.extension">.png</xsl:param>
   	<xsl:param name="html.stylesheet" 	select="'/docbook.css'"/>
   	<xsl:param name="css.decoration" 	select="1"/>

   	<xsl:param name="toc.max.depth">5</xsl:param>
   	<xsl:param name="toc.section.depth">4</xsl:param>
   	<xsl:param name="use.id.as.filename" select="1"/>

	<xsl:param name="preface.autolabel" select="1"/>
	<xsl:param name="chapter.autolabel" select="1" />
	<xsl:param name="section.autolabel" select="1" />
	<xsl:param name="appendix.autolabel" select="1" />

	<xsl:param name="section.autolabel.max.depth">8</xsl:param>
	<xsl:param name="section.label.includes.component.label" select="1" />
	<xsl:param name="generate.meta.abstract" select="1"></xsl:param>

	<xsl:param name="highlight.source" select="1"/>
	<xsl:param name="highlight.xslthl.config">/usr/share/xml/docbook/stylesheet/docbook-xsl/highlighting/xslthl-config.xml</xsl:param>
<!--
	<xsl:param name="eclipse.autolabel" select="1"/>

	<xsl:param name="use.extensions" select="1"/>
	<xsl:param name="textinsert.extension" select="1"/>

	<xsl:param name="xslthl.config" select="/usr/share/xml/docbook/stylesheet/docbook-xsl/highlighting/xslthl-config.xml"/>
	<xsl:param name="highlight.xslthl.config">/usr/share/xml/docbook/stylesheet/docbook-xsl/highlighting/xslthl-config.xml</xsl:param>
-->

	<xsl:template name="user.preroot">
	</xsl:template>

   <xsl:template name="user.head.content">
   		<!-- 
		<xsl:copy-of select="document('myscriptfile.js',/)"/>
		 -->
   </xsl:template>

	<xsl:template name="user.header.navigation">

        <a href="//www.netkiller.cn/">Home</a> |
		<a href="//netkiller.github.io/">简体中文</a> |
	    <a href="http://netkiller.sourceforge.net/">繁体中文</a> |
	    <a href="/journal/index.html">杂文</a> |
	    <a href="//www.netkiller.cn/home/donations.html">打赏(Donations)</a> |
	    <a href="http://netkiller-github-com.iteye.com/">ITEYE 博客</a> |
	    <a href="http://my.oschina.net/neochen/">OSChina 博客</a> |
	    <a href="https://www.facebook.com/bg7nyt">Facebook</a> |
	    <a href="http://cn.linkedin.com/in/netkiller/">Linkedin</a> |
	    <a href="https://zhuanlan.zhihu.com/netkiller">知乎专栏</a> |
	    <a href="/search.html">Search</a> |
		<a href="mailto:netkiller@msn.com">Email</a>
<!-- 	
<table width="100%" border="0">
  <tr>
    <td align="left" >
    </td>
    <td  align="right">
 -->
<!-- Google CSE Search Box Begins -->
<!-- 
  <form id="searchbox_008589143145807374698:f5uprauilyy" action="/search.html">

    <input type="hidden" name="cx" value="008589143145807374698:f5uprauilyy" />
    <input type="hidden" name="cof" value="FORID:11" />
    <input name="q" type="text" size="25" style="border-top-width: 1px; border-right-width: 1px; border-bottom-width: 1px; border-left-width: 1px; border-top-style: solid; border-right-style: solid; border-bottom-style: solid; border-left-style: solid; border-top-color: rgb(126, 157, 185); border-right-color: rgb(126, 157, 185); border-bottom-color: rgb(126, 157, 185); border-left-color: rgb(126, 157, 185); padding-top: 2px; padding-right: 2px; padding-bottom: 2px; padding-left: 2px; background-image: url(http://www.google.com/cse/intl/en/images/google_custom_search_watermark.gif); background-attachment: initial; background-origin: initial; background-clip: initial; background-color: rgb(255, 255, 255); background-position: 0% 50%; background-repeat: no-repeat no-repeat; " />
    <input type="submit" name="sa" value="Search" />
    <input name="siteurl" type="hidden" value="http://netkiller.sourceforge.net/" />
  </form>
  <script type="text/javascript" src="http://www.google.com/coop/cse/brand?form=searchbox_008589143145807374698%3Af5uprauilyy"></script>
 -->
<!-- Google CSE Search Box Ends -->
<!-- 
    </td>
  </tr>
</table>	
 -->
	</xsl:template>

   <xsl:template name="user.header.content">

<table><tr><td>
<iframe src="//ghbtns.com/github-btn.html?user=netkiller&amp;repo=netkiller.github.io&amp;type=watch&amp;count=true&amp;size=large" height="30" width="170" frameborder="0" scrolling="0" style="width:170px; height: 30px;" allowTransparency="true"></iframe>
</td><td>
<iframe src="//ghbtns.com/github-btn.html?user=netkiller&amp;repo=netkiller.github.io&amp;type=fork&amp;count=true&amp;size=large" height="30" width="170" frameborder="0" scrolling="0" style="width:170px; height: 30px;" allowTransparency="true"></iframe>
</td><td>
<iframe src="//ghbtns.com/github-btn.html?user=netkiller&amp;type=follow&amp;count=true&amp;size=large" height="30" width="240" frameborder="0" scrolling="0" style="width:240px; height: 30px;" allowTransparency="true"></iframe>
</td></tr></table>

   </xsl:template>

   <xsl:template name="user.footer.content">

<div id="disqus_thread"></div>
<script>

var disqus_config = function () {
this.page.url = "http://www.netkiller.cn";  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = 'netkiller'; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = '//netkiller.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

		<br />

		<script type="text/javascript" id="clustrmaps" src="//cdn.clustrmaps.com/map_v2.js?u=r5HG&amp;d=9mi5r_kkDC8uxG8HuY3p4-2qgeeVypAK9vMD-2P6BYM"></script>

   </xsl:template>

   <xsl:template name="user.footer.navigation">

<script >
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-11694057-1', 'auto');
  ga('send', 'pageview');

</script>

<script async="async">
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?93967759a51cda79e49bf4e34d0b0f2c";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

<!-- 搜索自动收录代码 -->
<script async="async">
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';        
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>

<script type="text/javascript" src="/js/q.js" async="async"></script>

   </xsl:template>

</xsl:stylesheet>

```

## 2. Docbook 参数

/usr/share/xml/docbook/stylesheet/nwalsh/xhtml/param.xsl

```

默认 HTML 格式输出文件中的章内小节是没有自动编号功能的，如果要实现小节自动编号需要设置 HTML 格式的两个转换参数。

section.autolabel 参数为 1 代表章内小节可自动编号，为 0 表示不会自动编号；

section.label.includes.component.label 参数为 1 表示章内小节编号包含章节编号,为 0 表示不包含章节编号。

设置这两个参数可通过命令行方式，也可通过修改 XSL 转换文件方法。下面分别介绍这两种方法：

通过命令行方式：

$ xsltproc --stringparam  section.autolabel 1 \
           --stringparam  section.label.includes.component.label 1 \
           docbook.xsl  myfile.xml>myfile.htm
修改 XSL 转换文件方式：

DocBook 文档的 HTML 格式转换样式文件一般位于/usr/share/sgml/docbook/docbook-xsl-1.65.1/html/目录下。通过查看 docbook.xsl 文件可知，在 docbook.xsl 中引用 param.xsl 作为参数设置文件，所有的参数都在这里设置。我们打开 param.xsl 文件，找到以下两行，再把参数置成 1 就可以了。

<xsl:param name="section.autolabel" select="1"/>
<xsl:param name="section.label.includes.component.label" select="1"/>

```

另一种方法

```

cat my_docbook.xsl
<?xml version='1.0'?>
<xsl:stylesheet 
                version='1.0'>
<xsl:include href="/usr/share/xml/docbook/stylesheet/nwalsh/html/docbook.xsl"/>
<xsl:output method="html"
            encoding="UTF-8"
            indent="no"/>
</xsl:stylesheet>

xsltproc -o docbook.html my_docbook.xsl docbook.xml

```

## 3. 生成目录深度

```

总深度
<xsl:param name="toc.max.depth">8</xsl:param>

章节深度
<xsl:param name="toc.section.depth">2</xsl:param>

```

## 4. 字符集

```
				--stringparam chunker.output.encoding UTF-8

```

## 5. Filename prefix

```

base.dir parameter value	Description	Example 					chunk filename
base.dir="htmlout/"			Output directory only.					htmlout/chap1.html
base.dir="refbook-"			Filename prefix only.					refbook-chap1.html
base.dir="htmlout/refbook-"	Output directory and filename prefix.	htmlout/refbook-chap1.html

```

base.dir parameter

```
				xsltproc --stringparam base.dir /usr/apache/htdocs/ chunk.xsl myfile.xml

```

## 第 4 章 DOCTYPE

## 1. SYSTEM

在线引用 DOCTYPE 头，这样在创建文档的时候速度会比较慢。

```

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE book PUBLIC "-//OASIS//DTD DocBook V5.0//EN"
"http://www.oasis-open.org/docbook/xml/5.0/docbook.dtd">

```

我们也可使使用本地的 DTD 文件

```

<!DOCTYPE subject SYSTEM "/usr/share/xml/docbook/schema/dtd/4.5/docbookx.dtd" [
	<!ENTITY book.info.author.xml			SYSTEM "../common/book.info.author.xml">
	<!ENTITY book.info.legalnotice.xml		SYSTEM "../common/book.info.legalnotice.xml">
	<!ENTITY book.info.abstract.xml			SYSTEM "../common/book.info.abstract.xml">
    <!ENTITY book.preface.xml           	SYSTEM "../common/book.preface.xml">
	<!ENTITY preface.about.me.xml       	SYSTEM "../common/about.me.xml">

	<!ENTITY chapter.docbook 			 	SYSTEM "chapter.docbook.xml">
	<!ENTITY chapter.docbook.doctype.xml 	SYSTEM "chapter.docbook.doctype.xml">
	<!ENTITY chapter.docbook.list.xml	 	SYSTEM "chapter.docbook.list.xml">
	<!ENTITY chapter.docbook.paragraphs.xml	SYSTEM "chapter.docbook.paragraphs.xml">
	<!ENTITY chapter.docbook.admonition.xml	SYSTEM "chapter.docbook.admonition.xml">
	<!ENTITY chapter.docbook-website.xml	SYSTEM "chapter.docbook-website.xml">
	<!ENTITY chapter.docbook.epub.xml	 	SYSTEM "chapter.docbook.epub.xml">
]>

```

```

<!DOCTYPE article PUBLIC "-//FreeBSD//DTD DocBook XML V4.5-Based Extension//EN" "../../../share/xml/freebsd45.dtd">

```

## 2. DTD 引用

### 2.1. 引用本地 DTD

```

<!DOCTYPE subject SYSTEM "/usr/share/xml/docbook/schema/dtd/4.5/docbookx.dtd" [
...
]>

```

### 2.2. oasis-open.org

```

<!DOCTYPE book PUBLIC "-//OASIS//DTD DocBook XML V5.0//EN" "http://www.oasis-open.org/docbook/xml/5.0b5/dtd/docbook.dtd" [
...
]>

```

## 3. article

例 4.1. article

```

<!DOCTYPE article PUBLIC "-//OASIS//DTD DocBook V4.1//EN">

<article>
  <articleinfo>
    <title>An example article</title>

    <author>
      <firstname>Your first name</firstname>
      <surname>Your surname</surname>
      <affiliation>
        <address><email>foo@example.com</email></address>
      </affiliation>
    </author>

    <copyright>
      <year>2000</year>
      <holder>Copyright string here</holder>
    </copyright>

    <abstract>
      <para>If your article has an abstract then it should go here.</para>
    </abstract>
  </articleinfo>

  <sect1>
    <title>My first section</title>

    <para>This is the first section in my article.</para>

    <sect2>
      <title>My first sub-section</title>

      <para>This is the first sub-section in my article.</para>
    </sect2>
  </sect1>
</article>

```

## 第 5 章 book / article

## 1. info

### bookinfo / articleinfo

### 1.1. affiliation

```

  <affiliation>
    <shortaffil>IBM</shortaffil>
    <jobtitle>Senior LAMP Engineer</jobtitle>
    <orgname>ArborText, Inc.</orgname>
    <orgdiv>Application Developement</orgdiv>
  </affiliation>

```

### 1.2. address

```

<address>
  <street>162 Guelph Street</street>
  <city>Georgetown</city>
  <state>ON</state>
  <country>Canada</country>
  <postcode>L7G 5X7</postcode>
  <phone>1-888-628-2028</phone>
  <fax>(905) 702-7851</fax>
  <email>info@cogent.ca</email>
  <otheraddr>www.cogent.ca</otheraddr>
</address>

```

### 1.3. reference

```

<reference>
    <title>Reference</title>
    <refentry>
(The content here is shown in the Reference Entries appendix.)
    </refentry>
</reference>

```

### 1.4. author

```

<authorgroup>
	<author>
		<FirstName>Bert</FirstName><Surname>Hubert</Surname>
		<affiliation>
	  		<orgname>Netherlabs BV</orgname>
	  		<address><email>bert.hubert@netherlabs.nl</email></address>
		</affiliation>
	</author>

    <collab>
		<collabname>Thomas Graf (Section Author)</collabname>
        <affiliation>
        	<address><email>tgraf%suug.ch</email></address>
		</affiliation>
	</collab>
</authorgroup>

```

#### 1.4.1. contrib

```

<authorgroup>
    <author>
	<firstname>Jordan</firstname>
	<surname>Hubbard</surname>
	<contrib>原著</contrib>
    </author>
</authorgroup>

```

#### 1.4.2. affiliation

```

<!DOCTYPE author PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<author>
  <honorific>Mr</honorific>
  <firstname>Norman</firstname>
  <surname>Walsh</surname>
  <othername role='mi'>D</othername>
  <affiliation>
    <shortaffil>ATI</shortaffil>
    <jobtitle>Senior Application Analyst</jobtitle>
    <orgname>ArborText, Inc.</orgname>
    <orgdiv>Application Developement</orgdiv>
  </affiliation>
</author>

```

#### 1.4.3. editor

```

<!DOCTYPE authorgroup PUBLIC "-//OASIS//DTD DocBook V3.1//EN">
<authorgroup>
  <author>
    <honorific>Dr.</honorific>
    <firstname>Lois</firstname>
    <surname>Common-Demoninator</surname>
    <affiliation>
      <shortaffil>Director, M. Behn School of Coop. Eng.</shortaffil>
      <jobtitle>Director of Cooperative Efforts</jobtitle>
      <orgname>The Marguerite Behn International School of
               Cooperative Engineering</orgname>
    </affiliation>
  </author>

  <editor>
    <firstname>Peter</firstname>
    <surname>Parker</surname>
    <lineage>Sr.</lineage>
    <othername>Spiderman</othername>
    <authorblurb>
      <para>
      Peter's a super hero in his spare time.
      </para>
    </authorblurb>
  </editor>
</authorgroup>

```

#### 1.4.4. othercredit

```

<article xmlns='http://docbook.org/ns/docbook'>
<info>
  <title>Example othercredit</title>
  <author>
    <personname>
      <firstname>Norman</firstname>
      <surname>Walsh</surname>
    </personname>
  </author>
  <othercredit>
    <personname>
      <firstname>John</firstname>
      <surname>Doe</surname>
    </personname>
    <contrib>Extensive review and rough drafts of Section 1.3, 1.4, and 1.5
    </contrib>
  </othercredit>
  <biblioid>5</biblioid>
</info>

<para>…</para>

</article>

```

```

<!DOCTYPE articleinfo PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<articleinfo>
  <title>Something Snappy</title>
  <author>
    <firstname>Norman</firstname>
    <surname>Walsh</surname>
  </author>
  <othercredit>
    <firstname>John</firstname>
    <surname>Doe</surname>
    <contrib>Extensive review and rough drafts of Section 1.3, 1.4, and 1.5
      </contrib>
  </othercredit>
  <pubsnumber>5</pubsnumber>
</articleinfo>

```

### 1.5. 出版信息

#### 1.5.1. publisher

```

<publisher>                      <!-- 出版者 -->
    <publishername>XXXXXX 有限公司</publishername>  <!-- 出版機構名稱 -->
    <address>
        <street>xxx 道 xxx 巷 12 号 204</street>
        <postcode>518000</postcode>
        <city>深圳</city>
        <state>广东</state>
        <country>中国</country>
        <phone>+86 755 xxxxxxxx</phone>
        <fax>+86 755 xxxxxxxx</fax>
        <email>your@domain.com</email>
      </address>
  </publisher>

```

#### 1.5.2. printhistory

```

<printhistory>
<para>
   June 2000: First Printed Edition.
</para>
</printhistory>

```

#### 1.5.3. 出版紀錄

```

	<edition>User's Guide version 1.0 for DocBook V3.0</edition>
	<pubdate>1997</pubdate>
	<releaseinfo></releaseinfo>
    <isbn>ISBN xx-xxxxxx-x-x</isbn>
	<revhistory>                     	<!-- 出版紀錄，可以有很多版本。 -->
		<revision>                     	<!-- 某一版本 -->
			<revnumber>1.0.0</revnumber> <!-- 版本編號 -->
			<date>2006-11-14</date>  	<!-- 發佈出版日期 -->
			<authorinitials></authorinitials>  <!-- 作者簡稱識別 -->
			<revremark></revremark>  	<!-- 版本發佈簡述 -->
		</revision>
	</revhistory>

```

### 1.6. copyright

```

	<copyright>
		<year>2008</year>
		<year>2009</year>
		<year>2010</year>
		<year>2011</year>
		<holder>Netkiller(Neo Chan). All rights reserved.</holder>
	</copyright>

```

### 1.7. keyword

```

	<keywordset>
	    <keyword>Linux</keyword>
	    <keyword>Beginners</keyword>
	    <keyword>linux</keyword>
	    <keyword>start</keyword>
	    <keyword>Getting started</keyword>
	    <keyword>guide</keyword>
	    <keyword>Guide</keyword>
	    <keyword>Exercises</keyword>
	    <keyword>exercises</keyword>
	</keywordset>

```

### 1.8. dedication

```

<book>
	<info>
		<dedication><title>鸣谢</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</dedication>
	</info>
</book>

```

### 1.9. abstract

```

<book>
	<info>
		<abstract><title>Abstract</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</abstract>
	</info>
</book>

```

### 1.10. legalnotice

```

<book>
	<info>
		<legalnotice><title>Legalnotice</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</legalnotice>
	</info>
</book>

```

### 1.11. cover 书籍封面图

```

<book xmlns='http://docbook.org/ns/docbook'>
<info>
  <title>DocBook</title>
  <subtitle>The Definitive Guide</subtitle>
  <biblioid class="isbn">978-0-596-8050-2-9</biblioid>
  <authorgroup>
    <author>
      <personname>
	<firstname>Norman</firstname>
	<surname>Walsh</surname>
      </personname>
    </author>
  </authorgroup>
  <publisher>
    <publishername>O'Reilly Media, Inc.</publishername>
    <address><city>Beijing</city></address>
    <address><city>Cambridge</city></address>
    <address><city>Farnham</city></address>
    <address><city>Köln</city></address>
    <address><city>Paris</city></address>
    <address><city>Sebastopol</city></address>
    <address><city>Taipei</city></address>
    <address><city>Tokyo</city></address>
  </publisher>
  <copyright>
    <year>2010</year>
    <holder>Norman Walsh, All rights reserved.</holder>
  </copyright>
  <releaseinfo>Published by O'Reilly Media, Inc.,
101 Morris Street, Sebastopol, CA 95472.</releaseinfo>
  <editor>
    <personname>
      <firstname>Richard L.</firstname>
      <surname>Hamilton</surname>
    </personname>
  </editor>
  <revhistory>
    <revision>
      <date>March 2010</date>
      <revremark>Second Edition.</revremark>
    </revision>
    <revision>
      <date>October 1999</date>
      <revremark>First Edition.</revremark>
    </revision>
  </revhistory>
  <legalnotice>
    <para>Nutshell Handbook, the Nutshell Handbook logo, and the…</para>
    <para>Many of the designations used by manufacturers…</para>
    <para>While every precaution has been taken in the preparation…</para>
  </legalnotice>
  <cover>
    <para role="tagline">The Official Documentation for DocBook</para>
    <mediaobject>
      <imageobject>
	<imagedata fileref="graphics/duck-cover.png">
	  <info>
	    <othercredit>
	      <orgname>O'Reilly Media</orgname>
	    </othercredit>
	    <othercredit>
	      <orgname>Dover Archives</orgname>
	    </othercredit>
	  </info>
	</imagedata>
      </imageobject>
    </mediaobject>
  </cover>
  <cover>
    <mediaobject>
      <imageobject>
	<imagedata fileref="graphics/duck-backcover.png"/>
      </imageobject>
    </mediaobject>
    <para>DocBook is a system for writing structured documents using…</para>
    <para><citetitle>DocBook: The Definitive Guide</citetitle> is the…</para>
    <itemizedlist>
      <listitem>
	<para>A brief introduction to  …</para>
      </listitem>
      <listitem>
	<para>A guide to creating documents with the DocBook schema…</para>
      </listitem>
      <listitem>
	<para>…</para>
      </listitem>
    </itemizedlist>
    <para>In an era of collaborative creation of technology, …</para>
    <formalpara>
      <title>Norman Walsh</title>
      <para>is a …</para>
    </formalpara>
  </cover>
</info>

<preface>
<title>Preface</title>

<para>DocBook provides a system …</para>
</preface>

<!-- … -->

</book>			

```

## 5. include

```

<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE book PUBLIC "-//OASIS//DTD DocBook XML V5.0//EN"
	"http://www.oasis-open.org/docbook/xml/5.0b5/dtd/docbook.dtd" [

	<!ENTITY chapter.database SYSTEM "chapter.database.xml">
]>
<book xml:base="http://netkiller.mefound.com/book/publish/"
	 xml:lang="zh-cn">
	<info>
		<title>Netkiller Document 手札</title>
		<author>
			<firstname>netkiller</firstname>
			<surname>Neo Chan</surname>
			<affiliation>
				<address>
					<email>openunix(at)163(dot)com</email>
				</address>
			</affiliation>
		</author>
		<copyright>
			<year>2008</year>
			<holder>Copyright netkiller</holder>
		</copyright>
		<releaseinfo>
			<para>修订版本 2008-5-24</para>
		</releaseinfo>

		<abstract>
			<title></title>
			<para></para>
		</abstract>
	</info>

	<preface>
		<title></title>
		<para></para>
	</preface>

	&chapter.database;

</book>

```

chapter.database.xml

```

<chapter>
	<title>Chapter 1</title>
</chapter>

```

## 6. Docbook HTML 输出选项

### HTML output options

### 6.1. 指定多页输出时 html 文档名

```

我们把 docbook 文档转换成 html 文档时，可以转换成一个大的 html 文档，也可以转换成多页的 html 文档。当转换成多页是，默认的文件是以 ch01.html、ch02.html 方式命名的，较不直观。我们可在 docbook 文档中添加一个属性，使它输出时按我们指定的文件名输出。

方法一：
<chapter><?dbhtml filename="first.html" ?>
<title>Introduction</title>
...

```

```

方法二：
<chapter id="first">
<title>Introduction</title>
...

xsltproc --stringparam  use.id.as.filename 1 chunk.xsl myfile.xml

```

### 6.2. 指定输出路径

```

默认转换后的文档是保存在当前目录的，我们可在 xsltproc 命令行用-o 选项指定转换后文档的输出路径，另外一种是在 docbook 中指定。

<book><?dbhtml dir="neo" ?>
<title>Docbook Guide</title>
...
<chapter id="first"><?dbhtml dir="technic" ?>
...
<chapter id="second">
...
<appendix id="three"><?dbhtml dir="read" ?>
...

输出后的路径：
jims/index.html
jims/technic/first.html
jims/second.html
jims/read/three.html

```

### 6.3. 包含 HTML 文件

```

...
</para>
<?dbhtml-include href="mycode.html"?>
<?dbhtml-include href="../additonal.html"?>
<para>
...

```

### 6.4. 包含 HTML 节点

```

<xsl:template name="user.header.content">
   <xsl:variable name="codefile" select="document('mycode.html',/)"/>
   <xsl:copy-of select="$codefile/htmlcode/node()"/>
</xsl:template>

```

### 6.5. 图标

#### Docbook icon graphics

#### 6.5.1. Admonition graphics

admon.graphics 1

```
xsltproc  --stringparam admon.graphics 1 docbook.xsl myfile.xml

```

#### 6.5.2. 导航图标

##### Navigational icons

```
xsltproc --stringparam  navig.graphics 1  chunk.xsl  myfile.xml

```

#### 6.5.3. Callout icons

```
If you use callout graphics, then there are three parameters that give you more control over the generated img tag.

callout.graphics.extension
Use this parameter to change the icon file extension from .png to something else. Of course, you must have the graphics that match that extension.

callout.graphics.path
Use this parameter to change the generated directory name from the default images/callouts/. Be sure to include the trailing slash.

callout.graphics.number.limit
Use this parameter to set the highest number for which you have a callout graphic. The stylesheets are distributed with callout graphics files with numbers up to 15, but you could create graphics with additional numbers if you need them. If you have more numbers but you do not reset this parameter, then any numbers over 15 will still format like (16).

```

### 6.6. dbhtml-include

包含 html 文件到当前页面

```

<?dbhtml-include href="file:///www/freebsd/faq.html"?>
<?dbhtml-include href="http://www.example.org/freebsd/faq.html"?>

```

注意：需要开启下面两个选项才能生效

```

	<xsl:param name="use.extensions" select="1"/>
	<xsl:param name="textinsert.extension" select="1"/>

```

### 6.7. 显示当前时间

```

<para>This document was generated <?dbtimestamp format="Y-m-d H:M:S"?>. </para>

```

```

<?dbtimestamp format="Y-m-d"  padding="0" ?>		

```

## 第 5 章 book / article

## 1. info

### bookinfo / articleinfo

### 1.1. affiliation

```

  <affiliation>
    <shortaffil>IBM</shortaffil>
    <jobtitle>Senior LAMP Engineer</jobtitle>
    <orgname>ArborText, Inc.</orgname>
    <orgdiv>Application Developement</orgdiv>
  </affiliation>

```

### 1.2. address

```

<address>
  <street>162 Guelph Street</street>
  <city>Georgetown</city>
  <state>ON</state>
  <country>Canada</country>
  <postcode>L7G 5X7</postcode>
  <phone>1-888-628-2028</phone>
  <fax>(905) 702-7851</fax>
  <email>info@cogent.ca</email>
  <otheraddr>www.cogent.ca</otheraddr>
</address>

```

### 1.3. reference

```

<reference>
    <title>Reference</title>
    <refentry>
(The content here is shown in the Reference Entries appendix.)
    </refentry>
</reference>

```

### 1.4. author

```

<authorgroup>
	<author>
		<FirstName>Bert</FirstName><Surname>Hubert</Surname>
		<affiliation>
	  		<orgname>Netherlabs BV</orgname>
	  		<address><email>bert.hubert@netherlabs.nl</email></address>
		</affiliation>
	</author>

    <collab>
		<collabname>Thomas Graf (Section Author)</collabname>
        <affiliation>
        	<address><email>tgraf%suug.ch</email></address>
		</affiliation>
	</collab>
</authorgroup>

```

#### 1.4.1. contrib

```

<authorgroup>
    <author>
	<firstname>Jordan</firstname>
	<surname>Hubbard</surname>
	<contrib>原著</contrib>
    </author>
</authorgroup>

```

#### 1.4.2. affiliation

```

<!DOCTYPE author PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<author>
  <honorific>Mr</honorific>
  <firstname>Norman</firstname>
  <surname>Walsh</surname>
  <othername role='mi'>D</othername>
  <affiliation>
    <shortaffil>ATI</shortaffil>
    <jobtitle>Senior Application Analyst</jobtitle>
    <orgname>ArborText, Inc.</orgname>
    <orgdiv>Application Developement</orgdiv>
  </affiliation>
</author>

```

#### 1.4.3. editor

```

<!DOCTYPE authorgroup PUBLIC "-//OASIS//DTD DocBook V3.1//EN">
<authorgroup>
  <author>
    <honorific>Dr.</honorific>
    <firstname>Lois</firstname>
    <surname>Common-Demoninator</surname>
    <affiliation>
      <shortaffil>Director, M. Behn School of Coop. Eng.</shortaffil>
      <jobtitle>Director of Cooperative Efforts</jobtitle>
      <orgname>The Marguerite Behn International School of
               Cooperative Engineering</orgname>
    </affiliation>
  </author>

  <editor>
    <firstname>Peter</firstname>
    <surname>Parker</surname>
    <lineage>Sr.</lineage>
    <othername>Spiderman</othername>
    <authorblurb>
      <para>
      Peter's a super hero in his spare time.
      </para>
    </authorblurb>
  </editor>
</authorgroup>

```

#### 1.4.4. othercredit

```

<article xmlns='http://docbook.org/ns/docbook'>
<info>
  <title>Example othercredit</title>
  <author>
    <personname>
      <firstname>Norman</firstname>
      <surname>Walsh</surname>
    </personname>
  </author>
  <othercredit>
    <personname>
      <firstname>John</firstname>
      <surname>Doe</surname>
    </personname>
    <contrib>Extensive review and rough drafts of Section 1.3, 1.4, and 1.5
    </contrib>
  </othercredit>
  <biblioid>5</biblioid>
</info>

<para>…</para>

</article>

```

```

<!DOCTYPE articleinfo PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<articleinfo>
  <title>Something Snappy</title>
  <author>
    <firstname>Norman</firstname>
    <surname>Walsh</surname>
  </author>
  <othercredit>
    <firstname>John</firstname>
    <surname>Doe</surname>
    <contrib>Extensive review and rough drafts of Section 1.3, 1.4, and 1.5
      </contrib>
  </othercredit>
  <pubsnumber>5</pubsnumber>
</articleinfo>

```

### 1.5. 出版信息

#### 1.5.1. publisher

```

<publisher>                      <!-- 出版者 -->
    <publishername>XXXXXX 有限公司</publishername>  <!-- 出版機構名稱 -->
    <address>
        <street>xxx 道 xxx 巷 12 号 204</street>
        <postcode>518000</postcode>
        <city>深圳</city>
        <state>广东</state>
        <country>中国</country>
        <phone>+86 755 xxxxxxxx</phone>
        <fax>+86 755 xxxxxxxx</fax>
        <email>your@domain.com</email>
      </address>
  </publisher>

```

#### 1.5.2. printhistory

```

<printhistory>
<para>
   June 2000: First Printed Edition.
</para>
</printhistory>

```

#### 1.5.3. 出版紀錄

```

	<edition>User's Guide version 1.0 for DocBook V3.0</edition>
	<pubdate>1997</pubdate>
	<releaseinfo></releaseinfo>
    <isbn>ISBN xx-xxxxxx-x-x</isbn>
	<revhistory>                     	<!-- 出版紀錄，可以有很多版本。 -->
		<revision>                     	<!-- 某一版本 -->
			<revnumber>1.0.0</revnumber> <!-- 版本編號 -->
			<date>2006-11-14</date>  	<!-- 發佈出版日期 -->
			<authorinitials></authorinitials>  <!-- 作者簡稱識別 -->
			<revremark></revremark>  	<!-- 版本發佈簡述 -->
		</revision>
	</revhistory>

```

### 1.6. copyright

```

	<copyright>
		<year>2008</year>
		<year>2009</year>
		<year>2010</year>
		<year>2011</year>
		<holder>Netkiller(Neo Chan). All rights reserved.</holder>
	</copyright>

```

### 1.7. keyword

```

	<keywordset>
	    <keyword>Linux</keyword>
	    <keyword>Beginners</keyword>
	    <keyword>linux</keyword>
	    <keyword>start</keyword>
	    <keyword>Getting started</keyword>
	    <keyword>guide</keyword>
	    <keyword>Guide</keyword>
	    <keyword>Exercises</keyword>
	    <keyword>exercises</keyword>
	</keywordset>

```

### 1.8. dedication

```

<book>
	<info>
		<dedication><title>鸣谢</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</dedication>
	</info>
</book>

```

### 1.9. abstract

```

<book>
	<info>
		<abstract><title>Abstract</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</abstract>
	</info>
</book>

```

### 1.10. legalnotice

```

<book>
	<info>
		<legalnotice><title>Legalnotice</title>
			<para></para>
			<para></para>
			<para></para>
			<para></para>
		</legalnotice>
	</info>
</book>

```

### 1.11. cover 书籍封面图

```

<book xmlns='http://docbook.org/ns/docbook'>
<info>
  <title>DocBook</title>
  <subtitle>The Definitive Guide</subtitle>
  <biblioid class="isbn">978-0-596-8050-2-9</biblioid>
  <authorgroup>
    <author>
      <personname>
	<firstname>Norman</firstname>
	<surname>Walsh</surname>
      </personname>
    </author>
  </authorgroup>
  <publisher>
    <publishername>O'Reilly Media, Inc.</publishername>
    <address><city>Beijing</city></address>
    <address><city>Cambridge</city></address>
    <address><city>Farnham</city></address>
    <address><city>Köln</city></address>
    <address><city>Paris</city></address>
    <address><city>Sebastopol</city></address>
    <address><city>Taipei</city></address>
    <address><city>Tokyo</city></address>
  </publisher>
  <copyright>
    <year>2010</year>
    <holder>Norman Walsh, All rights reserved.</holder>
  </copyright>
  <releaseinfo>Published by O'Reilly Media, Inc.,
101 Morris Street, Sebastopol, CA 95472.</releaseinfo>
  <editor>
    <personname>
      <firstname>Richard L.</firstname>
      <surname>Hamilton</surname>
    </personname>
  </editor>
  <revhistory>
    <revision>
      <date>March 2010</date>
      <revremark>Second Edition.</revremark>
    </revision>
    <revision>
      <date>October 1999</date>
      <revremark>First Edition.</revremark>
    </revision>
  </revhistory>
  <legalnotice>
    <para>Nutshell Handbook, the Nutshell Handbook logo, and the…</para>
    <para>Many of the designations used by manufacturers…</para>
    <para>While every precaution has been taken in the preparation…</para>
  </legalnotice>
  <cover>
    <para role="tagline">The Official Documentation for DocBook</para>
    <mediaobject>
      <imageobject>
	<imagedata fileref="graphics/duck-cover.png">
	  <info>
	    <othercredit>
	      <orgname>O'Reilly Media</orgname>
	    </othercredit>
	    <othercredit>
	      <orgname>Dover Archives</orgname>
	    </othercredit>
	  </info>
	</imagedata>
      </imageobject>
    </mediaobject>
  </cover>
  <cover>
    <mediaobject>
      <imageobject>
	<imagedata fileref="graphics/duck-backcover.png"/>
      </imageobject>
    </mediaobject>
    <para>DocBook is a system for writing structured documents using…</para>
    <para><citetitle>DocBook: The Definitive Guide</citetitle> is the…</para>
    <itemizedlist>
      <listitem>
	<para>A brief introduction to  …</para>
      </listitem>
      <listitem>
	<para>A guide to creating documents with the DocBook schema…</para>
      </listitem>
      <listitem>
	<para>…</para>
      </listitem>
    </itemizedlist>
    <para>In an era of collaborative creation of technology, …</para>
    <formalpara>
      <title>Norman Walsh</title>
      <para>is a …</para>
    </formalpara>
  </cover>
</info>

<preface>
<title>Preface</title>

<para>DocBook provides a system …</para>
</preface>

<!-- … -->

</book>			

```

## 2. toc

```

<book xmlns='http://docbook.org/ns/docbook'>
<title>DocBook: The Definitive Guide</title>
<subtitle>TOC Markup Example</subtitle>

<toc>
  <title>Table of Contents</title>
  <tocdiv>
    <title>Preface</title>
    <tocentry>Why Read This Book?</tocentry>
    <tocentry>This Book's Audience</tocentry>
    <!-- ... -->
  </tocdiv>
  <tocdiv>
    <title>Part I. Introduction</title>
    <tocdiv>
      <title>Chapter 1\. Getting Started with DocBook</title>
      <tocdiv>
        <title>A Short DocBook History</title>
        <tocentry>The HaL and O'Reilly era</tocentry>
        <tocentry>The Davenport era</tocentry>
        <tocentry>The OASIS era</tocentry>
      </tocdiv>
      <tocdiv>
        <title>DocBook V5.0</title>
        <tocdiv>
          <title>What's New in DocBook V5.0?</title>
          <tocentry>Renamed and removed elements</tocentry>
          <!-- ... -->
        </tocdiv>
      </tocdiv>
    </tocdiv>
    <tocdiv>
      <title>Chapter 2\. Creating DocBook Documents</title>
      <tocdiv>
        <title>Making an XML Document</title>
        <tocentry>An XML Declaration</tocentry>
        <!-- ... -->
      </tocdiv>
    </tocdiv>
  </tocdiv>
</toc>

<preface>
  <title>Preface</title>
  <para>DocBook provides a system for writing structured documents using
  <acronym>XML</acronym>. …</para>

  <!-- ... -->

  <section>
    <title>Why Read This Book?</title>

    <para>This book is designed to be the clear, concise, normative reference to
    the DocBook schema. This book is the official documentation for DocBook.
    </para>

    <!-- ... -->
  </section>
</preface>

<!-- ... -->

</book>	

```

## 3. topic

```

<topic xmlns='http://docbook.org/ns/docbook' version="5.1">
<info>
  <title>Troubleshooting</title>
</info>

<para>If your <citetitle>Joo Janta 200 Super-Chromatic Peril Sensitive
Sunglasses</citetitle> become opaque, do not be alarmed. That's more
or less the point.</para>

</topic>	

```

## 4. preface 前言

```

<preface id="svn.foreword">
  <prefaceinfo>
    <author>
      <firstname>Neo</firstname>
      <surname>Chan</surname>
    </author>
    <pubdate>Chicago, March 14, 2004.</pubdate>
  </prefaceinfo>

  <title>Preface</title>
  <para>...</para>
</preface>

```

## 第 6 章 Part / Chapter / Section

## 1. part 部

```

<part>
	<title>Part</title>
	<partintro>
        <para></para>
	</partintro>
</part>

```

```

<?xml version="1.0" encoding="UTF-8"?>
<book version="5.0" 

      >
  <info>
    <title>The Gallio Book</title>
    <authorgroup>
      <author><personname>The Gallio Documentation Team</personname></author>
    </authorgroup>
    <copyright>
      <year>2009</year>
      <holder>Gallio Project</holder>
    </copyright>
    <address>http://www.gallio.org/</address>
    <xi:include href="ReleaseInfo.docbook"/>
  </info>

  <xi:include href="Preface.docbook" />
  <part label="I" xml:id="part_Using">
    <title>Using MbUnit and Gallio</title>
    <xi:include href="Installation/Installation.docbook" />
  </part>

  <part label="II" xml:id="part_Developing">
    <title>Developing MbUnit and Gallio</title>
    <xi:include href="GallioDevelopmentBasics/GallioDevelopmentBasics.docbook" />
  </part>

  <part label="III" xml:id="appendices">
    <title>Appendices</title>
    <xi:include href="Appendices/MigrationGuide.docbook" />
  </part>

</book>

```

partintro

```

<!DOCTYPE part PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<part label="II">
<title>Programming with the Java API</title>
<partintro>
<para>
The sections in Part II present real-world examples of 
programming with Java.  You can study and learn from the
examples, and you can adapt them for use in your own programs.
</para>

<para>
The example code in these chapters is available for downloading.  
See <systemitem role="url">http://www.ora.com/catalog/books/javanut</systemitem>.
</para>

<literallayout>
<xref linkend="jnut-ch-04"/>
<xref linkend="jnut-ch-05"/>
<xref linkend="jnut-ch-06"/>
<xref linkend="jnut-ch-07"/>
<xref linkend="jnut-ch-08"/>
<xref linkend="jnut-ch-09"/>
</literallayout>
</partintro>
<chapter id="jnut-ch-04"><title/><para>...</para></chapter>
<chapter id="jnut-ch-05"><title/><para>...</para></chapter>
<chapter id="jnut-ch-06"><title/><para>...</para></chapter>
<chapter id="jnut-ch-07"><title/><para>...</para></chapter>
<chapter id="jnut-ch-08"><title/><para>...</para></chapter>
<chapter id="jnut-ch-09"><title/><para>...</para></chapter>		

```

## 2. chapter 章

```

	<part>
		<title>Part</title>
		<chapter>
			<title>Chapter</title>
		</chapter>
		<chapter>
			<title>Chapter</title>
		</chapter>
	</part>

```

### 2.1. chapterinfo

```

	<chapterinfo>
		<abstract>
		  <para>Kickstart 无人值守安装</para>
		</abstract>
		<keywordset>
			<keyword>ISO</keyword>
			<keyword>kickstart</keyword>
			<keyword>ks</keyword>
		</keywordset>
	</chapterinfo>

<chapterinfo>
    <authorgroup>
      <author>
    	<firstname>Tom</firstname>
	    <surname>Rhodes</surname>
    	<contrib>Written by </contrib>
      </author>
    </authorgroup>
</chapterinfo>

<chapterinfo>
<keywordset>
  <keyword>images</keyword>
  <keyword>illustrations</keyword>
</keywordset>
<itermset>
  <indexterm zone="figures"><primary>Figures</primary></indexterm>
  <indexterm zone="figures"><primary>Pictures</primary></indexterm>
  <indexterm zone="notreal">
    <primary>Sections</primary><secondary>Not Real</secondary>
  </indexterm>
</itermset>
</chapterinfo>

```

### 2.2. epigraph

```

<epigraph>

<attribution>
   <ulink url="http://www.margepiercy.com/">Marge Piercy</ulink>
</attribution>

<para>
   Life is the first gift, love is the second, and understanding is the third.
</para>

</epigraph>

```

## 3. section 节

```

<part>
	<title>Part I</title>
	<chapter>
		<title>Chapter 1</title>
		<section>
			<title>Section 1</title>
		</section>
	</chapter>
</part>

```

## 4. title 标题

```

<part>
	<title>Part I</title>
	<chapter>
		<title>Chapter 1</title>
		<section>
			<title>Section 1</title>
		</section>
	</chapter>
</part>

```

### 4.1. subtitle

```

<part>
	<title>Part I</title>
	<subtitle>Part I subtitle</subtitle>
	<chapter>
		<title>Chapter 1</title>
		<subtitle>Chapter 1 subtitle</subtitle>
		<section>
			<title>Section 1</title>
			<subtitle>Section 1 subtitle</subtitle>
		</section>
	</chapter>
</part>

```

### 4.2. titleabbrev

```

<part>
	<title>Part I</title>
	<subtitle>Part I subtitle</subtitle>
	<titleabbrev>Part I titleabbrev</titleabbrev>
	<chapter>
		<title>Chapter 1</title>
		<subtitle>Chapter 1 subtitle</subtitle>
		<titleabbrev>Chapter I titleabbrev</titleabbrev>
		<section>
			<title>Section 1</title>
			<subtitle>Section 1 subtitle</subtitle>
			<titleabbrev>Section I titleabbrev</titleabbrev>
		</section>
	</chapter>
</part>

```

## 第 7 章 appendix 附录

```

<appendix>
    <title>Appendix One</title>
    <para>Appendix content.</para>
</appendix>

```

## 第 8 章 Paragraphs 段落

## 1. para

```

<para>helloworld</para>

```

## 2. simpara

```

<simpara>
	Just the text, ma'am.
</simpara>

```

Just the text, ma'am.

## 3. formalpara

```

<formalpara>
  <title>A Formal Paragraph</title>
  <para>has a title that appears in bold, and a body paragraph.
  The title paragraph is incorporated into the body paragraph in HTML,
  and is separate in PDF.  The body paragraph is a regular &lt;para&gt;
  paragraph, and there is only one body paragraph allowed per formal
  paragraph.</para>
</formalpara>

```

A Formal Paragraph.  has a title that appears in bold, and a body paragraph. The title paragraph is incorporated into the body paragraph in HTML, and is separate in PDF. The body paragraph is a regular <para> paragraph, and there is only one body paragraph allowed per formal paragraph.

## 4. bridgehead

```

<bridgehead renderas="sect3">Topic Heading</bridgehead>
<para>You can use the Bridgehead element to create a heading
between paragraphs, without creating a new section or
sub-section.</para>

```

#### Topic Heading

You can use the Bridgehead element to create a heading between paragraphs, without creating a new section or sub-section.

## 5. blockquote

```

 <blockquote>
    <para>
      This text should be indented.
    </para>
</blockquote>

```

> This text should be indented.

## 6. sidebar

```

<sidebar>
	<title>A Sidebar</title>
	<para>
	Sidebar content.
	</para>
</sidebar>

```

A Sidebar

Sidebar content.

## 7. TM 商标

```

<trademark>Microsoft</trademark>

```

Microsoft™

## 8. epigraph 题词

```

<epigraph>
	<attribution>William Safire</attribution>
	<para>
		Knowing how things work is the basis for appreciation, and is
		thus a source of civilized delight.
	</para>
</epigraph>

```

Knowing how things work is the basis for appreciation, and is thus a source of civilized delight.

—William Safire

## 9. Font Formatting Codes

### 9.1. strong

```

<para><emphasis role="strong">Note</emphasis></para>

```

**Note**

### 9.2. bold

```

<emphasis role="bold">text</emphasis>

```

**text**

### 9.3. italic

```

<emphasis role="italic">text</emphasis>

```

text

### 9.4. literal

```

<para><literal>literal</literal></para>

```

`literal`

Code

```

<literal role="code">< ! [ CDATA [ code ] ] ></literal>

```

`code`

### 9.5. remark

```

<remark>Some details are still a bit shaky</remark>

```

## 10. citetitle

```

<!DOCTYPE para PUBLIC "-//OASIS//DTD DocBook XML V4.1.2//EN"
          "http://www.oasis-open.org/docbook/xml/4.1.2/docbookx.dtd">
<para>
The <emphasis>most</emphasis> important example of this
phenomenon occurs in A. Nonymous's book
<citetitle>Power Snacking</citetitle>.
</para>		

```

The *most* important example of this phenomenon occurs in A. Nonymous's book *Power Snacking* .

## 第 9 章 Lists 项目符号与编号

calloutlist, glosslist, itemizedlist, listitem, orderedlist, segmentedlist, simplelist, variablelist

## 1. itemizedlist

itemizedlist

```

<itemizedlist>
	<listitem><para>item 1</para></listitem>
	<listitem><para>item 2</para></listitem>
	<listitem><para>item 3</para></listitem>
</itemizedlist>

```

*   item 1

*   item 2

*   item 3

```

<itemizedlist mark='opencircle'>
<listitem>
<para>
Item 1
</para>
</listitem>
<listitem override='bullet'>
<para>
Item 2
</para>
</listitem>
<listitem>
<para>
Item 3
</para>
</listitem>
</itemizedlist>

```

*   Item 1

*   Item 2

*   Item 3

## 2. orderedlist

orderedlist

```

<orderedlist>
	<listitem><para>列表内容 1</para></listitem>
	<listitem><para>列表内容 2</para></listitem>
	<listitem><para>列表内容 3</para></listitem>
</orderedlist>

```

1.  列表内容 1

2.  列表内容 2

3.  列表内容 3

## 3. simplelist

例 9.1. SimpleList

```

<para>Here is a <tag>SimpleList</tag>, rendered inline:
<simplelist type='inline'>
<member>A</member>
<member>B</member>
<member>C</member>
<member>D</member>
<member>E</member>
<member>F</member>
<member>G</member>
</simplelist>
</para>

```

Here is a `SimpleList` , rendered inline: A, B, C, D, E, F, G

```

<para>Here is the same <tag>SimpleList</tag> rendered horizontally with
three columns:
<simplelist type='horiz' columns='3'>
<member>A</member>
<member>B</member>
<member>C</member>
<member>D</member>
<member>E</member>
<member>F</member>
<member>G</member>
</simplelist>
</para>

```

Here is the same `SimpleList` rendered horizontally with three columns:

| A | B | C |
| D | E | F |
| G |   |   |

```

<para>Finally, here is the list rendered vertically:
<simplelist type='vert' columns='3'>
<member>A</member>
<member>B</member>
<member>C</member>
<member>D</member>
<member>E</member>
<member>F</member>
<member>G</member>
</simplelist>
</para>

```

Finally, here is the list rendered vertically:

| A | D | G |
| B | E |   |
| C | F |   |

## 4. variablelist

```

<variablelist>
  <title>A variable list</title>
  <varlistentry>
    <term>First term</term>
    <listitem>
      <para>Definition or explanation.</para>
    </listitem>
  </varlistentry>
  <varlistentry>
    <term>Second term</term>
    <listitem>
      <para>Definition or explanation.</para>
    </listitem>
  </varlistentry>
</variablelist>

```

A variable list

First term

Definition or explanation.

Second term

Definition or explanation.

```

<variablelist>
	<title>Font Filename Extensions</title>
	<varlistentry>
		<term>
			<filename>TTF</filename>
		</term>
		<listitem>
			<para>TrueType fonts.</para>
		</listitem>
	</varlistentry>
	<varlistentry>
		<term>
			<filename>PFA</filename>
		</term>
		<term>
			<filename>PFB</filename>
		</term>
		<listitem>
			<para>
				PostScript fonts.
				<filename>PFA</filename>
				files are common on
				<acronym>UNIX</acronym>
				systems,
				<filename>PFB</filename>
				files
				are more common on Windows systems.
			</para>
		</listitem>
	</varlistentry>
</variablelist>		

```

Font Filename Extensions

`TTF`

TrueType fonts.

`PFA` , `PFB`

PostScript fonts. `PFA` files are common on UNIX systems, `PFB` files are more common on Windows systems.

## 5. segmentedlist

```

<segmentedlist>
	<segtitle>Name</segtitle>
	<segtitle>Occupation</segtitle>
	<segtitle>Favorite Food</segtitle>
	<seglistitem>
		<seg>Tux</seg>
		<seg>Linux mascot</seg>
		<seg>Herring</seg>
	</seglistitem>
	<seglistitem>
		<seg>Konqui</seg>
		<seg>The KDE Dragon</seg>
		<seg>Gnomes</seg>
	</seglistitem>
</segmentedlist>

```

**Name:** Tux**Occupation:** Linux mascot**Favorite Food:** Herring**Name:** Konqui**Occupation:** The KDE Dragon**Favorite Food:** Gnomes

## 6. procedure 过程

procedure

```

<procedure>
	<step>
		<para>Do this.</para>
	</step>

	<step>
		<para>Then do this.</para>
	</step>

	<step>
		<para>And now do this.</para>
	</step>
</procedure>

```

```

<procedure>
  <title>A procedure</title>
  <step>
    <para>Step 1.</para>
  </step>
  <step>
    <para>Step 2.</para>
    <substeps>
      <step>
        <para>Substep a.</para>
      </step>
      <step>
        <para>Substep b.</para>
      </step>
    </substeps>
  </step>
  <step>
    <para>Step 3.</para>
  </step>
</procedure>

```

1.  Do this.

2.  Then do this.

3.  And now do this.

过程 9.1. A procedure

1.  Step 1.

2.  Step 2.

    1.  Substep a.

    2.  Substep b.

3.  Step 3.

## 7. question/answer 问答

```

<qandaset>
	<qandaentry>
		<question>
			<para>What are little boys made of?</para>
		</question>
		<answer>
			<para>Snips and snails and puppy dog tails.</para>
		</answer>
	</qandaentry>
	<qandaentry>
		<question>
			<para>What are little girls made of?</para>
		</question>
		<answer>
			<para>Sugar and spice and everything nice.</para>
		</answer>
	</qandaentry>
</qandaset>

```

| **7.1.** | What are little boys made of? |
|  | Snips and snails and puppy dog tails. |
| **7.2.** | What are little girls made of? |
|  | Sugar and spice and everything nice. |

```

<article xmlns='http://docbook.org/ns/docbook'>
    <title>Example qandaset</title>

    <qandaset defaultlabel='qanda'>
      <qandaentry>
        <question>
          <para>To be, or not to be?</para>
        </question>
        <answer>
   		    <para>That is the question.</para>
        </answer>
   	  </qandaentry>
    </qandaset>

</article>		

```

| **问：** | To be, or not to be? |
| **答：** | That is the question. |

## 8. glosslist 注解

```

<glosslist>
	<glossentry>
		<glossterm>C</glossterm>
		<glossdef>
			<para>
				A procedural programming language invented by K&amp;R.
			</para>
		</glossdef>
	</glossentry>
	<glossentry>
		<glossterm>Pascal</glossterm>
		<glossdef>
			<para>
				A procedural programming language invented by Niklaus Wirth.
			</para>
		</glossdef>
	</glossentry>
</glosslist>

```

C

A procedural programming language invented by K&R.

Pascal

A procedural programming language invented by Niklaus Wirth.

## 9. glossary 术语表

```

<glossary><title>词汇表的例子</title>
	<glossdiv><title>E</title>

		<glossentry id="xml"><glossterm>Extensible Markup Language</glossterm>
			<acronym>XML</acronym>
			<glossdef>
			<para>关于 XML 的定义...</para>
			<glossseealso otherterm="sgml">
			</glossdef>
		</glossentry>

	</glossdiv>
</glossary>

```

引用

```

<glossterm linkend="xml">Extensible Markup Language</glossterm> is a new standard…

```

## 第 10 章 Table 表格

```

<table>
   <title>表格标题</title>
   <tgroup cols="2">
     <thead>
       <row>
          <entry>列标题 1</entry>
          <entry>列标题 2</entry>
       </row>
     </thead>
    <tbody>
       <row>
          <entry>列内容 1</entry>
          <entry>列内容 2</entry>
       </row>
       ...
    </tbody>
   </tgroup>
</table>

```

表 10.1. 表格标题

| 列标题 1 | 列标题 2 |
| --- | --- |
| 列内容 1 | 列内容 2 |

## 1. informaltable

<article xmlns='http://docbook.org/ns/docbook'>
<title>Example footnoteref</title>
<informaltable>
<tgroup cols='2'>
<tbody>
<row>
<entry>foo<footnote xml:id='fnrex1a'><para>A meaningless word</para></footnote></entry>
<entry>3<footnote xml:id='fnrex1b'><para>A meaningless number</para></footnote></entry>
</row>
     <row>
  <entry>bar<footnoteref linkend='fnrex1a'/></entry>
<entry>5<footnoteref linkend='fnrex1b'/></entry>
  </row>
</tbody>
</tgroup>
</informaltable>
</article>

| foo [^([a])](#ftn.fnrex1a) | 3 [^([b])](#ftn.fnrex1b) |
| bar [^([a])](table.xhtml#ftn.fnrex1a) | 5 [^([b])](table.xhtml#ftn.fnrex1b) |
|  
[^([a])](#fnrex1a) A meaningless word

[^([b])](#fnrex1b) A meaningless number

 |

## 第 11 章 图片，视频，音乐

## 1. figure

```

<figure><title>Notre Dame Cathedral</title>
	<graphic srccredit="Norman Walsh, 1998" fileref="figures/notredame.png"/>
</figure>		

<figure>
 <title>My Image</title>
 <screenshot>
  <screeninfo>Sample GNOME Display</screeninfo>
  <graphic  format="png" fileref="myfile" srccredit="me">
  </graphic>
 </screenshot>
</figure>

```

## 第 11 章 图片，视频，音乐

## 1. figure

```

<figure><title>Notre Dame Cathedral</title>
	<graphic srccredit="Norman Walsh, 1998" fileref="figures/notredame.png"/>
</figure>		

<figure>
 <title>My Image</title>
 <screenshot>
  <screeninfo>Sample GNOME Display</screeninfo>
  <graphic  format="png" fileref="myfile" srccredit="me">
  </graphic>
 </screenshot>
</figure>

```

## 3. imageobject

```

<article xmlns='http://docbook.org/ns/docbook'>
<title>Example imageobject</title>

<mediaobject>
  <imageobject condition="web">
    <imagedata fileref="figs/web/db5d_ref09.png" format="PNG" scale="70"/>
  </imageobject>
  <imageobject condition="print">
    <imagedata fileref="figs/print/db5d_ref09.pdf" format="PDF"/>
  </imageobject>
  <textobject>
    <phrase>The Eiffel Tower</phrase>
  </textobject>
  <caption>
    <para>Designed by Gustave Eiffel in 1889, The Eiffel Tower is one
          of the most widely recognized buildings in the world.
    </para>
  </caption>
</mediaobject>

</article>		

```

## 4. 视频

```

<article xmlns='http://docbook.org/ns/docbook'>
<title>Example videoobject</title>

<mediaobject>
  <videoobject>
    <videodata fileref='movie.avi' autoplay="true" classid="com.example.player">
      <multimediaparam name="crossfade" value="false"/>
    </videodata>
  </videoobject>
  <imageobject>
    <imagedata fileref='movie-frame.gif'/>
  </imageobject>
  <textobject>
    <para>This video illustrates the proper way to assemble an
    inverting time distortion device.
    </para>
    <warning>
      <para>It is imperative that the primary and secondary temporal
      couplings not be mounted in the wrong order. Temporal
      catastrophe is the likely result. The future you destroy
      may be your own.
      </para>
    </warning>
  </textobject>
</mediaobject>

</article>		

```

```

<article xmlns='http://docbook.org/ns/docbook'>
<title>Example videoobject</title>

<mediaobject>
  <videoobject>
    <videodata fileref='movie.ogg' autoplay="true" classid="com.example.player">
      <multimediaparam name="type" value="video/ogg"/>
    </videodata>
    <videodata fileref='movie.mp4' autoplay="true" classid="com.example.player">
      <multimediaparam name="type" value="vidoe/mp4"/>
    </videodata>
    <videodata fileref='movie.webm' autoplay="true" classid="com.example.player">
      <multimediaparam name="type" value="video/webm"/>
    </videodata>
  </videoobject>
  <imageobject>
    <imagedata fileref='movie-frame.gif'/>
  </imageobject>
  <textobject>
    <para>This video illustrates the proper way to assemble an
    inverting time distortion device.
    </para>
    <warning>
      <para>It is imperative that the primary and secondary temporal
      couplings not be mounted in the wrong order. Temporal
      catastrophe is the likely result. The future you destroy
      may be your own.
      </para>
    </warning>
  </textobject>
</mediaobject>

</article>		

```

## 5. 插入音乐

```

<article xmlns='http://docbook.org/ns/docbook'>
<title>Example audioobject</title>

<mediaobject>
  <audioobject>
    <audiodata fileref="phaser.wav"/>
  </audioobject>
  <textobject>
    <phrase>A <trademark>Star Trek</trademark> phaser sound effect</phrase>
  </textobject>
</mediaobject>

</article>		

```

## 6. screenshot

```

<figure id="app-FIG-shot1">
 <title>Screenshot</title>
 <screenshot>
  <mediaobject>
   <imageobject>
    <imagedata fileref="figures/example_screenshot.png" format="PNG"/>
   </imageobject>
   <textobject>
     <phrase>Shows the application screenshot with several buttons.</phrase>
   </textobject>
   <caption>
    <para>Screenshot of a program</para>
   </caption>
  </mediaobject>
 </screenshot>
</figure>

```

## 第 12 章 Admonition 警告与提示

caution, important, note, tip, warning

## 1. caution

```

<para><caution>Text goes here.</caution></para>

```

### 小心

Text goes here.

## 2. important

```

<para><important>Text goes here.</important></para>

```

### 重要

Text goes here.

## 3. note

```

<para><note>Text goes here.</note></para>

```

### 注意

Text goes here.

## 4. tip

```

<para><tip>Text goes here.</tip></para>

```

### 提示

Text goes here.

## 5. warning

```

<para><warning>Text goes here.</warning></para>

```

### 警告

Text goes here.

## 6. footnote 脚注

```

<para>During the installation of the product<footnote><para>In versions 2.3 and 2.4.</para> </footnote> you may see messages such as these. </para>		

<para>
			An annual percentage rate (
			<abbrev>APR</abbrev>
			) of 13.9%
			<footnote>
				<para>
					The prime rate, as published in the
					<citetitle>Wall Street
						Journal
					</citetitle>
					on the first business day of the month,
					plus 7.0%.
				</para>
			</footnote>
			will be charged on all balances carried forward.
		</para>

```

During the installation of the product [^([1])](#ftn.idp982) you may see messages such as these.

An annual percentage rate ( APR ) of 13.9% [^([2])](#ftn.idp986) will be charged on all balances carried forward.

* * *

[^([1])](#idp982) In versions 2.3 and 2.4.

[^([2])](#idp986) The prime rate, as published in the *Wall Street Journal* on the first business day of the month, plus 7.0%.

## 第 13 章 Computer terminal

## 1. literallayout

```

<literallayout>
1
2
3
</literallayout>

```

## 2. programlisting

```

<programlisting role="c">
#include &lt;stdio.h&gt;

int
main(void)
{
    printf("hello, world\n");
}
</programlisting>

<programlisting language="java">
</programlisting>

<programlisting linenumbering="numbered" startinglinenumber="12">

</programlisting>

<programlisting linenumbering="numbered" >
<?dbhtml linenumbering.everyNth="2" linenumbering.separator=" &gt;" linenumbering.width="2" ?>
<?dbfo linenumbering.everyNth="2" linenumbering.separator=" &gt;" linenumbering.width="2" ?>
<textobject><textdata  fileref="mycode.c" /></textobject>
</programlisting>

<example><title>My program listing</title>
  <programlisting><textobject><textdata
     fileref="mycode.c" /></textobject></programlisting>
</example>

Using XInclude for text inclusions
<example><title>My program listing</title>
  <programlisting><xi:include  href="mycode.c"  parse="text"
      /></programlisting>
</example>

```

```

<programlisting linenumbering="numbered" startinglinenumber="12">
<![CDATA[
#include <stdio.h>

int main(void)
{
   printf("Hello, world!\n");
   return 0;
}
]] >
</programlisting>

<programlisting linenumbering="numbered" >
	<?dbhtml linenumbering.everyNth="2" linenumbering.separator=" &gt;" linenumbering.width="2"?>
	<?dbfo linenumbering.everyNth="2"   linenumbering.separator=" &gt;" linenumbering.width="2"?>
	<textobject><textdata  fileref="mycode.c" /></textobject>
</programlisting>

```

```

<?php

class foo
{
	private $bar;

	public function __construct($bar)
	{
		$this->bar = $bar;
	}

	/**
	 * getFoo
	 *
	 * Returns bar if $this->bar is foo else foo .. Oo ;D
	 *
	 * @return string
	 */
	public function getFoo()
	{
		if($this->bar == 'foo')
		{
			return 'bar';
		}
		else
		{
			return 'foo';
		}
	}
}

```

### 2.1. Callouts

```

<screen>
bash@host:~/cvs/newbiedoc$ ls -l
total 48
<co id="perm">drwxr-sr-x    2 jesse    jesse        4096 May  4 16:26 CVS<co id="cvs">
drwxr-sr-x    3 jesse    jesse        4096 Mar 29 03:29 dev
drwxr-sr-x    3 jesse    jesse        4096 Apr  8 19:31 general
drwxr-sr-x    3 jesse    jesse        4096 Apr  9 00:15 images
</screen>

<calloutlist>
   <callout arearefs="cvs">
      <para>
      This is the CVS directory.  CVS files are stored here.
      </para>
   </callout>

   <callout arearefs="perm">
      <para>
      These are the permissions for the CVS directory.
      </para>
   </callout>
</calloutlist>

```

## 3. userinput

ls -l $KDEDIR

```

<userinput><command>ls</command>
<option>-l</option>
<parameter>$<envar>KDEDIR<envar></parameter>
</userinput>

```

export $KDEDIR=/usr/local/kde

```

<userinput>
<command>export</command>
<parameter>$<envar>KDEDIR</envar>=<filename>
/usr/local/kde</filename></parameter></userinput>

```

## 4. Keyboard Input

```

<keycombo>
 <keycap>Ctrl</keycap>
 <keycap>Alt</keycap>
 <keycap>F1</keycap>
</keycombo>

```

```

<menuchoice>
 <shortcut>
  <keycombo><keycap>Ctrl</keycap><keycap>q</keycap></keycombo>
 </shortcut>
 <guimenuitem> Quit</guimenuitem>
</menuchoice>

```

## 第 14 章 Cross references

## 1. link

```

<link linkend="table">Table</link>
<link linkend="article-My-Article-sec"><quote>sec</quote></link>

<link linkend="font"><quote>font</quote></link>

```

## 2. ulink

```

<ulink url="http://netkiller.hikz.com">homepage</ulink>

```

## 3. xreflabel / xref

```

<para  id="ChooseSCSIid"  xreflabel="choosing a SCSI id">The methods
for choosing a <acronym>SCSI</acronym> id are ...
</para>
...
<para>
See the paragraph on <xref  linkend="ChooseSCSIid"/>.
</para>

```

```

<para  id="ChooseSCSIid"  >The methods
for <phrase  id="SCSIxref">choosing a <acronym>SCSI</acronym> id</phrase>
are ...
</para>
...
<para>
See the paragraph on <xref  linkend="ChooseSCSIid"  endterm="SCSIxref"/>.
</para>

```

## 第 15 章 Cross references

```

	<lot>
		<title>List of Figures</title>
		<lotentry pagenum='5'>The Letters inside their boxes</lotentry>
		<lotentry pagenum='15'>Example figure produced by both TeX and troff</lotentry>
		<!-- ... -->
	</lot>

```

## 第 16 章 The DocBook HTML Forms Document Type

```
$ sudo apt-get install docbook-html-forms

```

To use this module, specify the public and system identifiers of this module in your document type declaration. For example, to use this module to write a book, use the following document type declaration:

```

<!DOCTYPE book PUBLIC "-//OASIS//DTD DocBook HTML Forms Module V1.2CR1//EN"
               "http://www.oasis-open.org/docbook/xml/htmlforms/1.2CR1/dbforms.dtd">

```

Naturally, you can include an internal subset if you wish.

To incorporate this module into a higher-level customization layer, use the public and system identifiers of this module in your customization layer. For example:

```

<!DOCTYPE % docbookforms PUBLIC "-//OASIS//DTD DocBook HTML Forms Module V1.2CR1//EN"
                        "http://www.oasis-open.org/docbook/xml/htmlforms/1.2CR1/dbforms.dtd">
%docbookforms;

```