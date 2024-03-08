# 第 32 章 写作团队的运作

前提条件： subversion 服务器一台,或者使用 sf.net, github.com, code.google.com 等等提供的服务，团队人员需要懂得 docbook 以及配置 docbook 环境

## 1. Docbook 环境初始化

### 1.1. FreeBSD

```
# pkg_add -r vim
# pkg_add -r git
# pkg_add -r libxml2 libxslt
# pkg_add -r docbook-xsl

```

创建 book.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE subject SYSTEM "/usr/local/share/xml/docbook/5.0/dtd/docbook.dtd">
<book>
    <bookinfo>
        <title>An Example Book</title>

        <author>
            <firstname>Your first name</firstname>
            <surname>Your surname</surname>
            <affiliation>
                <address>
                    <email>foo@example.com</email>
                </address>
            </affiliation>
        </author>

        <copyright>
            <year>2000</year>
            <holder>Copyright string here</holder>
        </copyright>

        <abstract>
            <para>If your book has an abstract then it should go here.</para>
        </abstract>
    </bookinfo>

    <preface>
        <title>Preface</title>

        <para>Your book may have a preface, in which case it should be placed
            here.</para>
    </preface>

    <chapter>
        <title>My first chapter</title>

        <para>This is the first chapter in my book.</para>

        <section>
            <title>My first section</title>

            <para>This is the first section in my book.</para>
        </section>

    </chapter>
</book>

```

生成文档

```
$ xsltproc /usr/local/share/xsl/docbook/xhtml/docbook.xsl book.xml > book.html

```

### 1.2. Ubuntu/Debian

```
$ sudo apt-get install docbook-xsl
$ sudo apt-get install xsltproc xmlto
$ sudo apt-get install make
$ sudo apt-get install git

```

创建 book.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE subject SYSTEM "/usr/share/xml/docbook/schema/dtd/4.5/docbookx.dtd">
<book>
    <bookinfo>
        <title>An Example Book</title>

        <author>
            <firstname>Your first name</firstname>
            <surname>Your surname</surname>
            <affiliation>
                <address>
                    <email>foo@example.com</email>
                </address>
            </affiliation>
        </author>

        <copyright>
            <year>2000</year>
            <holder>Copyright string here</holder>
        </copyright>

        <abstract>
            <para>If your book has an abstract then it should go here.</para>
        </abstract>
    </bookinfo>

    <preface>
        <title>Preface</title>

        <para>Your book may have a preface, in which case it should be placed
            here.</para>
    </preface>

    <chapter>
        <title>My first chapter</title>

        <para>This is the first chapter in my book.</para>

        <section>
            <title>My first section</title>

            <para>This is the first section in my book.</para>
        </section>

    </chapter>
</book>

```

生成文档

```
$ xsltproc /usr/share/xml/docbook/stylesheet/docbook-xsl/xhtml/docbook.xsl book.xml > book.html

```

## 2. Subversion 版本控制

1.  subversion 初始化

    1.  trunk

    2.  branches

    3.  releases

    4.  tags

    ```
    svn co svn://127.0.0.1/document
    cd project
    mkdir trunk
    mkdir tags
    mkdir branches
    mkdir releases
    svn ci -m "Initialized empty subversion repository in your_project"

    ```

2.  创建 docbook 文档，安排章节

    将章节拆分成独立文件，并在主文档头部声明

    ```

    	<!ENTITY chapter.system SYSTEM "chapter.system.xml">
    	<!ENTITY chapter.system.harddisk SYSTEM "chapter.system.harddisk.xml">
    	<!ENTITY chapter.network SYSTEM "chapter.network.xml">

    ```

    完成后导入 subversion 的 trunk 中

3.  创建版本分支

    ```
    $ svn copy svn://netkiller.8800.org/document/trunk svn://netkiller.8800.org/document/branches/system
    $ svn copy svn://netkiller.8800.org/document/trunk svn://netkiller.8800.org/document/branches/network

    ```

4.  开始写作

    我们假设 jam 负责 system 章节

    1.  checkout

        ```
        $ svn checkout svn://netkiller.8800.org/document/branches/system

        ```

    2.  编辑文件

        ```
        vim chapter.system.xml

        ```

    3.  校验 XML

        ```
        $ export DSSSL=/usr/share/xml/docbook/stylesheet/nwalsh/xhtml/chunk.xsl
        $ xsltproc --stringparam html.stylesheet docbook.css ${DSSSL} book.xml

        ```

    4.  提交文件

        ```
        $ svn ci -m "I have finished this chapter."

        ```

    其他编辑人员操作类似 checkout 自己 branche 上的 network 章节等等

5.  tags 运作

    当 jam 完成了指派的任务的第一个阶段后，可以创建一个 tags

    ```
    svn copy svn://netkiller.8800.org/document/branches/system svn://netkiller.8800.org/document/tags/system_phase_I

    ```

    tags 一旦建立，以后不会在更改

    然后 jam 可以在/document/branches/system 继续写作

6.  合并 tags 到主干

    当 tags 完成后主编将其合并到 trunk

    ```
    svn merge svn://netkiller.8800.org/document/tags/system_phase_I

    ```

    然后发行 unstable 版本，你也可以每天产生一个快照。等待用户反馈。

    反馈结果由负责人在/document/branches/system 上修改，等待下一次发布在下一个阶段。

7.  发行文档

    当一切 OK 时,我就可以把 trunk 复制到 releases 中，随你怎命名。

    ```
    $ svn copy svn://netkiller.8800.org/document/trunk svn://netkiller.8800.org/document/release/document_v1.0

    ```

    这个版本/document/release/document_v1.0 就可以提供给读者了。

## 3. GIT

```
配置 GIT 环境

git config --global user.name "Neo Chan"
git config --global user.email netkiller@msn.com

克隆仓库
git clone https://github.com/freebook/PHP.git

...
编辑文件
...

提交文件
git add *
git commit -a

推送文件
git push origin master

```