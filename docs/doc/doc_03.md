# 部分 II. Tex

## 第 17 章 Document Tools

## 1. texlive - TeX Live: A decent selection of the TeX Live packages

```
sudo apt-get install texlive

```

### 1.1. 安装 XeTeX

http://zh.wikipedia.org/wiki/XeTeX

xeTex 是与 tex 平级的，都是排版引擎，而 xeLatex 和 Latex 刚分别基于两者的宏包。不同的是 xeTex 支持 Unicode, 可以直接支持中文。

XeLaTeX 是使用 LaTeX 的排版引擎

#### 1.1.1. install usage apt-get

http://scripts.sil.org/xetex

```
$ sudo apt-get install texlive-xetex lmodern
$ sudo apt-get install texlive-latex-extra texlive-latex-recommended

```

### 1.2. XeLaTeX

安装 XeTeX 后现在我们尝试创建一个 PDF 文档

演示怎么创建文档

```
\documentclass[12pt,a4paper]{article}
\usepackage{fontspec}
\usepackage{xunicode}
\usepackage{xltxtra}
\setmainfont[Mapping=tex-text]{WenQuanYi Micro Hei}
\begin{document}
\XeTeX 可以使用系统自带的字体，而不需要再另外生成。
\end{document}

```

xelatex a.tex

### 1.3. 安装字体

查看已经安装的字体

```
$ fc-list :lang=zh
/usr/share/fonts/truetype/droid/DroidSansFallbackFull.ttf: Droid Sans Fallback:style=Regular

```

安装字体，将字体复制到/usr/share/fonts 目录中，然后运行下面的命令。

```
$ sudo mkdir /usr/share/fonts/windows
$ sudo cp /media/Windows/Fonts/{SIM,sim}* /usr/share/fonts/windows/

```

安装免费字体

```
$ sudo apt-get install xfonts-wqy ttf-wqy-microhei ttf-wqy-zenhei

```

更新字体

```
$ sudo mkfontscale
$ sudo mkfontdir
$ sudo fc-cache -fv		

```

查看字体

```
$ fc-list :lang=zh
/usr/share/fonts/windows/msyh.ttf: Microsoft YaHei,微软雅黑:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/windows/FZYTK.TTF: FZYaoTi,方正姚体:style=Regular
/usr/share/fonts/windows/STXIHEI.TTF: STXihei,华文细黑:style=Regular
/usr/share/fonts/windows/simhei.ttf: SimHei,黑体:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/truetype/wqy/wqy-microhei.ttc: WenQuanYi Micro Hei,文泉驛微米黑,文泉驿微米黑:style=Regular
/usr/share/fonts/windows/simsun.ttc: NSimSun,新宋体:style=Regular
/usr/share/fonts/windows/simsun.ttc: SimSun,宋体:style=Regular
/usr/share/fonts/X11/misc/wenquanyi_10ptb.pcf: WenQuanYi Bitmap Song:style=Bold
/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc: WenQuanYi Zen Hei,文泉驛正黑,文泉驿正黑:style=Regular
/usr/share/fonts/X11/misc/wenquanyi_12pt.pcf: WenQuanYi Bitmap Song:style=Regular
/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc: WenQuanYi Zen Hei Sharp,文泉驛點陣正黑,文泉驿点阵正黑:style=Regular
/usr/share/fonts/windows/STXINWEI.TTF: STXinwei,华文新魏:style=Regular
/usr/share/fonts/X11/misc/wenquanyi_10pt.pcf: WenQuanYi Bitmap Song:style=Regular
/usr/share/fonts/windows/simkai.ttf: KaiTi,楷体:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/X11/misc/wenquanyi_9pt.pcf: WenQuanYi Bitmap Song:style=Regular
/usr/share/fonts/truetype/droid/DroidSansFallbackFull.ttf: Droid Sans Fallback:style=Regular
/usr/share/fonts/X11/misc/wenquanyi_11pt.pcf: WenQuanYi Bitmap Song:style=Regular
/usr/share/fonts/windows/mingliu.ttc: PMingLiU,新細明體:style=Regular
/usr/share/fonts/windows/simfang.ttf: FangSong,仿宋:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/windows/STFANGSO.TTF: STFangsong,华文仿宋:style=Regular
/usr/share/fonts/windows/msjh.ttf: Microsoft JhengHei,微軟正黑體:style=Normal,Regular,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/X11/misc/wenquanyi_9ptb.pcf: WenQuanYi Bitmap Song:style=Bold
/usr/share/fonts/windows/STLITI.TTF: STLiti,华文隶书:style=Regular
/usr/share/fonts/windows/msjhbd.ttf: Microsoft JhengHei,微軟正黑體:style=Negreta,Bold,tučné,fed,Fett,Έντονα,Negrita,Lihavoitu,Gras,Félkövér,Grassetto,Vet,Halvfet,Pogrubiony,Negrito,Полужирный,Fet,Kalın,Krepko,Lodia
/usr/share/fonts/windows/STZHONGS.TTF: STZhongsong,华文中宋:style=Regular
/usr/share/fonts/windows/msyhbd.ttf: Microsoft YaHei,微软雅黑:style=Bold,Negreta,tučné,fed,Fett,Έντονα,Negrita,Lihavoitu,Gras,Félkövér,Grassetto,Vet,Halvfet,Pogrubiony,Negrito,Полужирный,Fet,Kalın,Krepko,Lodia
/usr/share/fonts/X11/misc/wenquanyi_11ptb.pcf: WenQuanYi Bitmap Song:style=Bold
/usr/share/fonts/windows/FZSTK.TTF: FZShuTi,方正舒体:style=Regular
/usr/share/fonts/windows/STSONG.TTF: STSong,华文宋体:style=Regular
/usr/share/fonts/windows/STXINGKA.TTF: STXingkai,华文行楷:style=Regular
/usr/share/fonts/windows/STHUPO.TTF: STHupo,华文琥珀:style=Regular
/usr/share/fonts/windows/SIMLI.TTF: LiSu,隶书:style=Regular
/usr/share/fonts/windows/kaiu.ttf: DFKai\-SB,標楷體:style=Regular
/usr/share/fonts/windows/ARIALUNI.TTF: Arial Unicode MS:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/truetype/wqy/wqy-zenhei.ttc: WenQuanYi Zen Hei Mono,文泉驛等寬正黑,文泉驿等宽正黑:style=Regular
/usr/share/fonts/windows/SIMYOU.TTF: YouYuan,幼圆:style=Regular
/usr/share/fonts/truetype/wqy/wqy-microhei.ttc: WenQuanYi Micro Hei Mono,文泉驛等寬微米黑,文泉驿等宽微米黑:style=Regular
/usr/share/fonts/windows/mingliu.ttc: MingLiU_HKSCS,細明體 _HKSCS:style=Regular
/usr/share/fonts/windows/STCAIYUN.TTF: STCaiyun,华文彩云:style=Regular
/usr/share/fonts/windows/mingliu.ttc: MingLiU,細明體:style=Regular
/usr/share/fonts/windows/STKAITI.TTF: STKaiti,华文楷体:style=Regular
/usr/share/fonts/X11/misc/wenquanyi_12ptb.pcf: WenQuanYi Bitmap Song:style=Bold		

```

测试 Unicode 文件 xetex.tex

```
%!Tex Program = xelatex
\documentclass[a4paper]{article}
\usepackage{xltxtra}
\setmainfont[Mapping=tex-text]{WenQuanYi Micro Hei}
\begin{document}\pagestyle{empty}
\section{Unicode support}

\subsection{English}
All human beings are born free and equal in dignity and rights.

\subsection{Íslenska}
Hver maður er borinn frjáls og jafn öðrum að virðingu og réttindum.

\subsection{Русский}  
Все люди рождаются свободными
и равными в своем достоинстве и 
правах.

\subsection{Tiếng Việt}
Tất cả mọi người sinh ra đều được tự do và bình đẳng về nhân phẩm và 
quyền lợi.

\subsection{简体中文}
每个人生来平等，享有相同的地位和权利。

\subsection{繁體中文}
每個人生來平等，享有相同的地位和權利。

\subsection{日本語}
すべての人間は自由であり、かつ、尊厳と権利とについて平等である。

\section{Legacy syntax}
When he goes---``Hello World!''\\
She replies—“Hello dear!”

\section{Ligatures}
\fontspec[Ligatures={Common, Historical}]{Linux Libertine O Italic}
\fontsize{12pt}{18pt}\selectfont Questo è strano assai!

\section{Numerals}
\fontspec[Numbers={OldStyle}]{Linux Libertine O}Old style: 1234567\\
\fontspec[Numbers={Lining}]{Linux Libertine O}Lining: 1234567

\end{document}		

```

测试字体

```

\documentclass[12pt,a4paper]{article}
\usepackage{fontspec,xunicode,xltxtra}
\usepackage{titlesec}
\usepackage[top=1in,bottom=1in,left=1.25in,right=1.25in]{geometry}

\titleformat{\section}{\Large\xbsong}{\thesection}{1em}{}

\XeTeXlinebreaklocale "zh"
\XeTeXlinebreakskip = 0pt plus 1pt minus 0.1pt

\newfontfamily\song{Simsun (Founder Extended)}
\newfontfamily\bwei{FZBeiWeiKaiShu-S19S}
\newfontfamily\zbhei{FZZhanBiHei-M22T}
\newfontfamily\xzt{FZXiaoZhuanTi-S13T}
\newfontfamily\xbsong{FZXiaoBiaoSong-B05}
\newfontfamily\dbsong{FZDaBiaoSong-B06}
\newfontfamily\gulif{FZGuLi-S12T}
\newfontfamily\gulij{FZGuLi-S12S}
\newfontfamily\kai{FZKai-Z03}
\newfontfamily\hei{FZHei-B01}
\newfontfamily\whei{WenQuanYi Zen Hei}
\newfontfamily\fsong{FZFangSong-Z02}
\newfontfamily\lanting{FZLanTingSong}
\newfontfamily\boya{FZBoYaSong}
\newfontfamily\lishu{FZLiShu-S01}
\newfontfamily\lishuII{FZLiShu II-S06}
\newfontfamily\yao{FZYaoTi-M06}
\newfontfamily\zyuan{FZZhunYuan-M02}
\newfontfamily\xhei{FZXiHei I-Z08}
\newfontfamily\xkai{FZXingKai-S04}
\newfontfamily\ssong{FZShuSong-Z01}
\newfontfamily\bsong{FZBaoSong-Z04}
\newfontfamily\nbsong{FZNew BaoSong-Z12}
\newfontfamily\caiyun{FZCaiYun-M09}
\newfontfamily\hanj{FZHanJian-R-GB}
\newfontfamily\songI{FZSongYi-Z13}
\newfontfamily\hcao{FZHuangCao-S09}
\newfontfamily\wbei{FZWeiBei-S03}
\newfontfamily\huali{FZHuaLi-M14}
\setmainfont{FZLanTingSong}

\renewcommand{\baselinestretch}{1.25}

\begin{document}

\title{\whei XeTeX 使用小结}
\author{\fsong 何勃亮}
\date{\kai2009 年 6 月 21 日}

\maketitle

\section{简介}
以前使用 CJK 进行中文的排版，需要自己生成字体库，近日，出现了 XeTeX，可以比较好的解决中文字体问题，不需要额外
生成 LaTeX 字体库，直接使用计算机系统里的字体。

\section{字体列表}
本文使用了大量本机自带的字体。

\begin{table}[htbp]
\caption{字体列表}

\centering
\begin{tabular}{|l|c|r|}
\hline
\hei 字体 & \hei 命令 & \hei 字体效果 \\
\hline
\kai 宋体 & \verb+\song+ & \song 宋体 \\
\kai 楷体 & \verb+\kai+ & \kai 楷体 \\
\kai 黑体 & \verb+\hei+ & \hei 黑体 \\
\kai 仿宋体 & \verb+\fsong+ & \fsong 仿宋体 \\
\kai 文泉驿黑体 & \verb+\whei+ & \whei 文泉驿黑体 \\
\kai 书宋体 & \verb+\ssong+ & \ssong 书宋体 \\
\kai 报宋体 & \verb+\bsong+ & \bsong 报宋体 \\
\kai 新报宋体 & \verb+\nbsong+ & \nbsong 新报宋体 \\
\kai 兰亭宋体 & \verb+\lanting+ & \lanting 兰亭宋体 \\
\kai 博雅宋体 & \verb+\boya+ & \boya 博雅宋体 \\
\kai 宋体一 & \verb+\songI+ & \songI 宋体一 \\
\kai 隶书 & \verb+\lishu+ & \lishu 隶书 \\
\kai 隶书二 & \verb+\lishuII+ & \lishuII 隶书二 \\
\kai 古隶简体 & \verb+\gulij+ & \gulij 古隶简体 \\
\kai 古隶繁体 & \verb+\gulif+ & \gulif 古隶繁体 \\
\kai 华隶书 & \verb+\huali+ & \huali 华隶书 \\
\kai 小标宋 & \verb+\xbsong+ & \xbsong 小标宋 \\
\kai 大标宋 & \verb+\dbsong+ & \dbsong 大标宋 \\
\kai 小篆体 & \verb+\xzt+ & \xzt 小篆体 \\
\kai 姚体 & \verb+\yao+ & \yao 姚体 \\
\kai 准圆 & \verb+\zyuan+ & \zyuan 准圆 \\
\kai 细黑一 & \verb+\xhei+ & \xhei 细黑一 \\
\kai 行楷书 & \verb+\xkai+ & \xkai 行楷书 \\
\kai 彩云体 & \verb+\caiyun+ & \caiyun 彩云体 \\
\kai 汉简书 & \verb+\hanj+ & \hanj 汉简书 \\
\kai 魏碑体 & \verb+\wbei+ & \wbei 魏碑体 \\
\hline
\end{tabular}
\end{table}

\end{document}

```

xelatex xetex.tex

### 1.4. LuaTeX

http://www.luatex.org/

```

sudo apt-get install texlive-luatex

```

### 1.5. FAQ

#### 1.5.1. ! LaTeX Error: File `fontspec.sty' not found.

```
$ texdoc fontsepc
Sorry, no documentation found for fontsepc.
If you are unsure about the name, try searching CTAN's TeX catalogue at
http://ctan.org/search.html#byDescription.

```

## 2. latex2html

install latex2html

```
sudo apt-get install latex2html

```

latex2html charset='utf-8'

<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=utf-8">

```
latex2html -html_version 4.0,gb18030,unicode index.latex

```

Icons

```
latex2html -local_icons index.latex

```

## 3. texi2html

```
$ sudo apt-get install texi2html

```

## 4. tex2page

install tex2page

```
sudo apt-get install mzscheme

```

reference http://docs.huihoo.com/homepage/shredderyin/tex2page/intro.html

## 5. CJK

CJK

```
$ sudo apt-get install texlive cjk-latex freetype1-tools
$ sudo apt-get install latex-cjk-all
$ sudo mkdir -p  /usr/share/texmf/fonts/tfm /usr/share/texmf/fonts/truetype

```

例 17.1. example.latex

```
\documentclass{article}
\usepackage{CJK}
\begin{document}
\begin{CJK*}{UTF8}{simsun}
中文
\end{CJK*}
\end{document}

```

生成 html 文件

```
latex2html -split 0 example.latex

```

Fonts

```
neo@master:~$ cd /usr/share/fonts/
neo@master:/usr/share/fonts$ cp -r /home/neo/Fonts .
neo@master:/usr/share/fonts$ sudo cp -r /home/neo/Fonts .

neo@master:/usr/share/fonts$ cd Fonts/
neo@master:/usr/share/fonts/Fonts$ sudo mkfontscale
neo@master:/usr/share/fonts/Fonts$ sudo mkfontdir
neo@master:/usr/share/fonts/Fonts$ sudo fc-cache

```

## 6. TeXstudio(LaTeX 编辑器)

http://texstudio.sourceforge.net/

## 第 18 章 编码转换

gb2312 转 utf-8

```

for f in `find .`; do [ -f $f ] && perl -MEncode -pi -e '$_=encode_utf8(decode(gb2312=>$_))' $f; done;

```

## 第 19 章 排版

选择 A4 纸

```
\documentclass[a4paper,12pt]{book}

```

设置页边距

Word 中默认的页边距为 上边距=2.54cm, 下边距=2.54cm, 左边距=3.17cm, 右边距=3.17cm

```
\usepackage[top=1in,bottom=1in,left=1.25in,right=1.25in]{geometry}

```

我的习惯 2cm

```
\usepackage[top=20mm,bottom=20mm,left=20mm,right=20mm]{geometry}

```

## 第 20 章 文章的组成元素

标题

```
\title{文章的题目}
\author{作者姓名}
\date{2005/09/23}
\maketitle

```

abstract

```
\begin{abstract}
	put your abstract here...
\end{abstract}

```

目录

```
\tableofcontents

```

章节

```
\chapter{章的名称}
\section{节的名称}
\subsection{小节的名称}
\subsubsection{子节的名称}

```

索引

```
\printindex

```

参考文献

```
\begin{thebibliography}
\bibitem{}参考文献 1
\bibitem{}参考文献 2
\end{thebibliography}

```

例 20.1. article.latex

```
% This is a comment line

\documentclass{article}

\begin{document}

% Note to self:
%    I must change this title later!
\title{Hello World}

\author{Your Name\\
	Department of Computer Science\\
	Courant Institute, NYU}
\maketitle

\begin{abstract}
	...put your abstract here...
\end{abstract}

\section{First Section}
	...text...
\subsection{First subsection}
	...text...
\subsection{Second subsection}
	...text...
\section{Second Section}
	...text...

...and so on...
\end{document}

```

例 20.2. book.latex

```
% This is a comment line

\documentclass{article}

\begin{document}

% Note to self:
%    I must change this title later!
\title{Hello World}

\author{Your Name\\
	Department of Computer Science\\
	Courant Institute, NYU}
\maketitle

\begin{abstract}
	...put your abstract here...
\end{abstract}

\chapter{First Chapter}
	...text...
\section{First Section}
	...text...
\subsection{First subsection}
	...text...
\subsection{Second subsection}
	...text...

\chapter{Second Chapter}
	...text...
\section{Second Section}
	...text...

...and so on...
\end{document}

```

## 第 21 章 Fonts

you can have bold font, italics font, etc as follows:

```
{\bf This is in bold} and
{\it this is in italics}
and this is in default roman
{\rm as is this}.

```

## 1. Sizes

```
\tiny
\scriptsize
\footnotesize
\small
\normalsize
\large
\Large
\LARGE
\huge
\Huge

```

## 2. Styles

Here is a simple example that will probably show you all you need to know about bold, italics, and underlining.

```
When something is \emph{really}, \textbf{really} important, you can \underline{underline it}, \emph{italicize it}, \textbf{bold it}. If you \underline{\textbf{\emph{must do all three}}}, then you can nest them.

```

## 第 22 章 项目符号和编号

项目符号

```
\begin{itemize}
\item 我的第一个项目
\item 我的第二个项目
\item 我的第三个项目
\end{itemize}

```

项目编号

```
\begin{enumerate}
\item 我的第一个项目
\item 我的第二个项目
\item 我的第三个项目
\end{enumerate}

```

修改 \enumerate 的编号格式

```
\usepackage{enumerate}

然后 enumerate 环境就可以使用一个可选参数，像这样：

\begin{enumerate}[(a)]
\item $(p\rightarrow(q\rightarrow p))$;
\item $((q\lor r)\rightarrow((\neg r)\rightarrow q))$;
\end{enumerate}

[]里面的 (a) 出来的结果是 (a)(b)(c)(d)这样子的。还可以用如下的：
1，得到 1,2,3,4....
i，得到 i,ii,iii.....
I，得到 I,II,III....
a，得到 a,b,c,d.....
A，得到 A,B,C,D....

还可以加各种修饰，比如
[Exer. i]，得到 Exer. i，Exer. ii，Exer. iii， Exer. iv，等等

你要的这种结果稍微麻烦一点，要把[]用花括号括起来
\begin{enumerate}[{[}1{]}]
\item $(p\rightarrow(q\rightarrow p))$;
\item $((q\lor r)\rightarrow((\neg r)\rightarrow q))$;
\end{enumerate}

```

无符号

```
\begin{description}
\item 我的第一个项目
\item 我的第二个项目
\item 我的第三个项目
\end{description}

\begin{description}
\item[(a)] First item...
\item[(b)] Second item...
\end{description}

```

## 第 23 章 表格

使用 tabular 环境可以生成表格，见下面这个例子：

```

\begin{tabular}{|c|c|c|c|c|c|}
\hline\\
编号 & 姓名 & 性别 & 年龄 & 地址 & 电话号码\\
\hline\\
1    & 张三 & 男 & 45 & 重庆工学院 &12345678\\
\hline\\
2 & 李四 & 女 & 29 & 重庆杨家坪 & 654321\\
\hline
\end{tabular}

```

这里要注意了，我们在第一行中，有几个 c 就表示有几列， c 表示你的列是居中对齐的，如果你想居左或居右，请用 l 或 r 。 如果你的某行中的某一列是空的，你也要列出来，放个空格在那里就行了，你甚至可以什么都不放，在要空的那里前后各放一个 & 符号就行。 在这里看到，对齐是用& 来实现的，我们前面说过。竖线是用 c 两边的那些竖杠实现的，横线是用命令\hline 来实现的。如果你不想要这些线，你可以把它们去掉。

## 第 24 章 图片

\documentclass[12pt]{article} 下面加入

```
\usepackage{graphicx}

```

插入图片

```
\includegraphics[width=0.8\textwidth]{fig1.eps}

```

```
\begin{figure}[htp]
    \begin{center}
        \includegraphics[width=450pt]{image/arch_02.png}
        \caption{描述文字}
        \label{fig:arch_02}
    \end{center}
\end{figure}

```

## 第 25 章 分割章节

main.tex

```
\documentclass{article}
\usepackage{amsmath}
\usepackage{CJK}
\begin{document}
\begin{CJK*}{GBK}{song}

\input{sec1.tex}
\include{sec2.tex}
\end{CJK*}

```

sec1.tex

```
\section{第一节}
恭承嘉惠兮，俟罪长沙。侧闻屈原兮，自沉汩罗。造托湘流兮，敬吊先生。

```

sec2.tex

```
\section{第一节}
贾生名谊，洛阳人也。年十八，以能诵诗属书闻于郡中。

```

## 第 26 章 参考文献

```

\begin{thebibliography}{}
\bibitem[Gelman et al., 2004]{Gelman} Gelman, A., Carlin, J. B., Stern, H. S. \& Rubin, D. B.
 (2004) Bayesian Data Analysis (Second Edition). \newblock Chapman \& Hall/CRC.
\end{thebibliography}
\clearpage
\end{document}

```