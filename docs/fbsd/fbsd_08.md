# 第 22 章 textproc

## docbook

```
cd /usr/ports/textproc/libxslt
make install

cd /usr/ports/textproc/docbook-xml
make install

cd /usr/ports/textproc/docbook-xsl
make install

```

通过 xsltproc 生成 html 文件

```
$ xsltproc /usr/local/share/xsl/docbook/xhtml/chunk.xsl book.xml

```