# 第 30 章 pandoc - general markup converter

http://johnmacfarlane.net/pandoc/

## 1. Docbook to Markdown

```
pandoc -f docbook -t markdown -s howto.xml -o example.text

```

## 2. Markdown to HTML

```
pandoc -f markdown -t html5 -s example.md -o example.html

```

例 30.1. Makefile

```

PANDOC = pandoc
PANDOCOPTS = -f markdown -t html5
PREFIX=..
HTML := $(patsubst %.md,%.html,$(wildcard *.md))

all: $(HTML)

%.html: %.md 
	$(PANDOC) $(PANDOCOPTS) -s $< -o $(PREFIX)/$@

clean:
	@rm -rf $(PREFIX)/*.html

rebuild : clean all					

```