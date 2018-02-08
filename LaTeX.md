[TOC]

## 什么是 TeX/LaTeX？

TeX 是一门标记式宏语言，用来排版科技文章，尤其擅长处理复杂的数学公式，同时，也是处理这一语言的排版软件。

LaTeX 是在 TeX 基础上按内容/格式分离和模块化等思想建立的一种 TeX 上的格式。

**注意：TeX 并不适用于文书处理。**





## 入门

### Hello, World!

~~~latex
%hello_world.tex
\documentclass{article}
\begin{document}
Hello, World!
\end{document}
~~~



### 语法和结构

#### 语法

LaTeX 源文件的语句可以分为三种：命令、数据和注释。

命令又分为普通命令和环境。

普通命令以\起始，大多只有一行。

环境包含一对起始声明和结尾声明，用于多行内容的场合。

命令和环境可以相互嵌套。

数据就是普通内容。

注释语句以%起始，在编译过程中被忽略。

#### 物理结构

LaTeX 文档的结构可以分为物理结构和逻辑结构。前者指的是源文件的组织形式，包括序言和正文两部分；后者则是最终输出文档的结构，包括标题、目录、章节等。

序言用来完成一些设置，比如指定文档类型，引入宏包，定义命令、环境等；文档的实际内容则放在正文部分。基本用法如下：

~~~latex
\documentclass[options]{class}	%文档类声明
\usepackage[options]{package}	%引入宏包
...
\begin{document}			   %正文
...
\end{document}
~~~

常用的文档类有三种：article、report、book，基本选项如下：

| 10pt，11pt，12pt      | 正文字号，缺省10pt。LaTeX 会根据正文字号选择标题、上下标等的字号    |
| ------------------- | ---------------------------------------- |
| letterpaper，a4paper | 纸张尺寸，缺省是 letterpaper                     |
|                     | 标题后是否另起新页。article 缺省 notitlepage，report 和 book 缺省有 titlepage |
| onecolumn，twocolumn | 栏数，缺省单栏                                  |
|                     | 单面双面。article 和 report 缺省用单面，book 缺省用双面   |
|                     | 横向打印，缺省是纵向                               |

#### 逻辑结构

一份文档的开头通常有标题、作者、摘要等信息，之后是章节等层次结构，内容则散布于层次结构之间。文档比较长时，还可以使用目录。

标题、作者、日期等命令用法如下：

~~~latex
\title{LaTeX Notes}
\author{Alpha Huang}
\date{\today}
\maketitle
~~~

article 和 report 可以有摘要，book 里没有。摘要环境用法如下：

~~~latex
\begin{abstract}
...
\end{abstract}
~~~

LaTeX 提供了七种层次结构命令，每个高级层次结构可以包含若干低级层次。article 中没有 chapter，而 report 和 book 则支持所有层次：

~~~latex
\part{...}				%level -1
\chapter{...}			%level 0
\section{...}			%level 1
\subsection{...}		%level 2
\subsubsection{...}		%level 3
\paragraph{...}			%level 4
\subparagraph{...}		%level 5
~~~

可以用\tableofcontents 命令来生成目录。系统会自动设定目录包含的章节层次，用户也可以显示指定目录层次深度：

~~~latex
\setcounter{tocdepth}{2}	%设定目录深度
\tableofcontents			%列出目录
~~~

如果不想让某些层次的标题出现在目录里，可以给命令加上星号：

~~~latex
\chapter*{...}
\section*{...}
\subsection*{...}
\subsubsection*{...}
~~~

类似地，可以用下面的命令生成插图和表格目录：

~~~latex
\listoffigures
\listoftables
~~~



### 文字

文档的内容可以分为文本模式和数学模式。前者是缺省工作方式；要输入数学内容则需要特殊命令或环境。

#### 字符输入

文档中可以输入的文字符号大致可以分为：普通字符、控制符、特殊符号、预定义字符串、注音符号等。

普通字符可以直接输入，而有些字符（例如#$%^&_{}~等）被用作特殊的控制符，输入时多数需要在前面加个\。而\本身则要用\textbackslash命令来输入，因为\\被用作换行指令：

~~~latex
\# \$ \^ \& \_ \{ \} \~ \textbackslash \%
~~~

#### 字体样式

LaTeX 的字体强调命令\emph 在不同字体环境中有不同的效果。如果周围字体是正体，它就是斜体；反之，它就是正体。

#### 换行、换页和断字

通常 LaTeX 会自动换行，也可以用\\或\newline 命令来强制换行；用\newpage 命令来强制换页。

LaTeX会自动断字，使得每一行的字间距分布均匀。有时，也需要显示指明断字位置，如下例指明 BASIC 这个词不能断开，而 blar-blar-blar可以在-处断开：

~~~latex
\hyphenation{BASIC blar-blar-blar}
~~~



### 对齐和间距

#### 段落对齐

LaTeX 中的段落缺省两端对齐，下面的三个环境可以让段落分别居左、居右或居中对齐：

~~~latex
\begin{flushleft}
居左\\段落
\end{flushleft}

\begin{flushright}
居右\\段落
\end{flushright}

\begin{center}
居中\\段落
\end{center}
~~~

#### 缩进和段间距

LaTeX 正文中第一个段落缺省不缩进首行，可以用 identfirst 宏包使得第一段也缩进首行。

段落首行缩进的距离可以用\parindent 变量来控制，段落之间的距离可以用\parskip 变量来控制。

~~~latex
\usepackage{identfirst}
...
\setlength{\parindent}{2em}
\addtolength{\parskip}{3pt}
~~~

#### 行间距

行间距是段落中相邻两行基线之间的距离，LaTeX缺省使用单倍行距。

可以用\linespread 命令来控制行距：

~~~latex
\linespread{1.3}	%一倍半行距
\linespread{1.6}	%双倍行距
~~~

\linespread 命令不仅会改变正文行距，同时也把目录、脚注、图表标题等的行距给改了。如果只想改正文行距，可以使用 setspace 宏包的行距命令：

~~~latex
\usepackage{setspace}
...
\singlespacing		%单倍行距
\onehalfspacing		%一倍半行距
\doublespacing		%双倍行距
\setstretch{1.25}	%任意行距
~~~

上述行距命令对全文的行距都会产生影响，setspace 宏包还提供了 singlespacing，onehalfspacing，doublespacing 等环境，可以用来设置局部文字的行距：

~~~latex
\begin{doublespacing}
double\\spacing
\end{doublespacing}

\begin{spacing}{1.25}
any\\spacing
\end{spacing}
~~~



### 特殊段落

#### 摘录

LaTeX 中有三中摘录环境：quote、quotation、verse。

quote 两端都缩进，quotation 在 quote 的基础上增加了首行缩进，verse 比 quote 多了第二行起的缩进：

~~~latex
\begin{quote}
引文两端\\都缩进。
\end{quote}

\begin{quotation}
引文两端缩进，首行增加缩进。
\end{quotation}

\begin{verse}
引文两端缩进，第二行起增加缩进。
\end{verse}
~~~

#### 脚注

脚注可以使用\footnote 命令；如要改变脚注编号形式，可以使用以下命令：

~~~latex
正文\footnote{脚注}
~~~

footnote 是一个计数器，计数器有五种显示格式，重定义\thefootnote 宏时可任选：

~~~latex
\renewcommand{\thefootnote}{\roman{footnote}} %i,ii,iii
~~~

以后还会遇到其他一些计数器，都可以用重定义\thecounter 的方法改变它们的显示格式。

| 阿拉伯数字 | \arabic{counter} | 1，2，3...    |
| ----- | ---------------- | ----------- |
|       | \alph{counter}   | a，b，b...    |
|       | \Alph{counter}   | A，B，C...    |
|       | \roman{counter}  | i，ii，iii... |
|       | \Roman{counter}  | Ⅰ，Ⅱ，Ⅲ...    |

#### 注释

前面提到可以用百分号来标明注释，但是对于大段文字的注释，百分号就显得比较繁琐。这时，可以使用 verbatim 宏包的 comment 环境：

~~~latex
\begin{comment}
...
\end{comment}
~~~



### 列表

#### 基本列表

LaTeX 有三种基本列表环境：无序列表、有序列表、描述列表。这些列表可以单独使用，也可以互相嵌套。

无序列表：

~~~latex
\begin{itemize}
	\item C++
	\item Java
	\item HTML
\end{itemize}
~~~

有序列表：

~~~latex
\begin{enumerate}
	\item C++
	\item Java
	\item HTML
\end{enumerate}
~~~

描述列表：

~~~latex
\begin{description}
	\item[C++] 编程语言
	\item[Java] 编程语言
	\item[HTML] 标记语言
\end{description}
~~~

#### 其他列表

上述列表的缺省行间距较大，如要节省空间，可以考虑 paralist 宏包，它提供了一系列压缩列表和行间列表环境。

压缩列表：

~~~latex
\begin{compactitem}
	\item C++
	\item Java
	\item HTML
\end{compactitem}

\begin{compactenum}
	\item C++
	\item Java
	\item HTML
\end{compactenum}

\begin{compactdesc}
	\item[C++] 编程语言
	\item[Java] 编程语言
	\item[HTML] 标记语言
\end{compactdesc}
~~~

行间列表：

~~~latex
\begin{inparaitem}
	\item C++
	\item Java
	\item HTML
\end{inparaitem}

\begin{inparaenum}
	\item C++
	\item Java
	\item HTML
\end{inparaenum}

\begin{inparadesc}
	\item[C++] 编程语言
	\item[Java] 编程语言
	\item[HTML] 标记语言
\end{inparadesc}
~~~



### 盒子

LaTeX 在排版时把每个对象（小到一个字母，大到一个段落）都视为一个矩形盒子。

#### 初级盒子

最简单的盒子命令是\mbox 和 \fbox。前者把一组对象组合起来，后者在此基础上加了个边框：

~~~latex
\mbox{010 6278 5001}
\fbox{010 6278 5001}
~~~

#### 中级盒子

稍复杂的\makebox 和 \framebox 命令提供了宽度和对齐方式控制的选项。其对齐方式有居中（缺省）、居左、居右和分散对齐，分别用c，l，r，s来表示：

~~~latex
%语法：[宽度][对齐方式]{内容}
\makefox[100pt][c][仪仗队]
\framefox[100pt][s][仪仗队]
~~~

#### 高级盒子

大一些的对象比如整个段落可以用\parbox 命令或 minipage 环境，两者语法类似，有宽度、高度、外部对齐、内部对齐等选项。

这里的外部对齐是指该盒子与周围对象的纵向关系，有三种方式：居顶、居中和居底对齐，分别用t，c，b来表示。

内部对齐是指该盒子内部内容的纵向排列方式，也是同样三种。

> 语法：\[外部对齐]\[高度][内部对齐]{宽度}{内容}

~~~latex
\fbox{%
	\parbox[c][36pt][t]{170pt}{
	锦瑟无端五十弦，一弦一柱思华年。庄生晓梦迷蝴蝶，望帝春心托杜鹃。
	}
}
\hfill
\fbox{%
	\begin{minipage}[c][36pt][b]{170pt}
	沧海月明珠有泪，蓝田日暖玉生烟。此情可待成追忆，只是当时已惘然。
	\end{minipage}%
}
~~~





## 数学

为了使用 AMS-LaTeX 提供的数学功能，需要在文档的序言部分加载 amsmath 宏包：

~~~latex
\usepackage{amsmath}
~~~

### 数学模式

LaTeX 的数学模式有两种形式：行间模式和独立模式。前者是指在正文中插入数学内容；后者独立排列，可以有或没有编号。

行间公式用\$...$，无编号独立公式用\\[...\\]。

\fbox 命令可以给文本内容加个方框，数学模式下也有个类似的命令\boxed。

~~~
Einstein's $E=mc^2$
\[E=mc^2\]
\[\boxed{E=mc^2}\]
\begin{equation}
	E=mc^2
\end{equation}
~~~



### 基本元素

#### 上下标和根号

指数或上标用^表示，下表用_表示，根号用\sqrt 表示。

上下标如果多于一个字母或符号，需要用一对{}括起来：

~~~latex
\[x_{ij}^2\quad \sqrt{x}\quad \sqrt[3]{x}\]
~~~

#### 分数

分数用\frac 命令表示，它会根据环境自动调整字号，比如在行间公式中小一点，在独立公式中则大一点。

可以人工设置分数字号，比如\dfrac 命令把分数的字号设置为独立公式中的大小，而\tfrac 命令则把字号设为行间公式中的大小：

~~~latex
$ \frac{1}{2} \dfrac{1}{2} $
\[\frac{1}{2} \tfrac{1}{2}\]
~~~

#### 运算符

和、积、极限、积分等大运算符用\sum \prod \lim \int 等命令表示，它们的上下标在行间公式中被压缩，以适应行高。也可以用\limits 和 \nolimits 命令显示指定是否压缩上下标：

~~~latex
$ \sum_{i=1}^n i\quad \prod_{i=1}^n\quad \lim_{x\to0}x^2\quad \int_a^b x^2 dx $\\
$ \sum\limits_{i=1}^n i\quad \prod\limits_{i=1}^n\quad 
\lim\limits_{x\to0}x^2\quad \int\limits_a^b x^2 dx $
\[\sum_{i=1}^n i\quad \prod_{i=1}^n\quad \lim_{x\to0}x^2\quad \int_a^b x^2 dx\]
\[\sum\nolimits_{i=1}^n i\quad \prod\nolimits_{i=1}^n\quad
\lim\nolimits_{x\to0}x^2\quad \int\nolimits_a^b x^2 dx\]
~~~

多重积分如果用多个\int 来输入的话，积分号之间的距离会过宽。正确的方法是用\iint，\iiint，\iiiint，\idotsint 等命令输入：

~~~latex
\[\iint\quad \iiint\quad \iiiint\quad \idotsint \]
~~~

#### 分隔符

各种括号用() [] \\{\\} \langle \rangle 等命令表示。

花括号通常用来输入命令和环境的参数，所以在数学公式中它们前面要加\。

#### 省略号

省略号用\dots \cdots \vdots \ddots 等命令表示。\dots 和 \cdots 的纵向位置不同，前者一般用于有下标的序列。

~~~latex
\[x_1,x_2,\dots,x_n\quad 1,2,\cdots,n\quad \]
~~~



### 矩阵

数学模式下可以用 array 环境来生成矩阵，它提供了外部对齐和列对齐的控制参数

外部对齐是指整个矩阵和周围对象的纵向关系，有三种方式：居顶、居中（缺省）、居底，分别用t，c，b来表示；列对齐也有三种方式：居左、居中、居右，分别用l，c，r表示。

\\\\ 和 &用来分隔行和列，语法如下：

~~~latex
\begin{array}[外部对齐]{列对齐}
	行列内容
\end{array}
~~~

~~~latex
\[\begin{array}{ccc}
x_1 & x_2 & \dots \\
x_3 & x_4 & \dots \\
\vdots & \vdots & \ddots
\end{array}\]
~~~

\smallmatrix 命令可以生成行间矩阵：

~~~latex
Marry has a little matrix $(\begin{smallmatrix} a&b\\c&d \end{smallmatrix})$.
~~~



### 多行公式

有时一个公式太长一行放不下，或几个公式需要写成一组，就要用到 amsmath 提供的一些多行公式环境。

#### 长公式

无需对齐的长公式可以使用 multline 环境，需要对齐的长公式可以使用 split 环境，它本身不能独立使用，必须包含在其他数学环境内，因此也称作次环境。用 \\\\ 和 & 来分行和设置对齐的位置。

无对齐长公式：

~~~latex
\begin{multline}
x = a+b+c+{} \\
	d+e+f+g
\end{multline}
~~~

对齐长公式：

~~~latex
\[\begin{split}
x ={} &a+b+c+{} \\
	  &d+e+f+g
\end{split}\]
~~~

#### 公式组

不需要对齐的公式组可以使用 gather 环境，需要对齐的公式组用 align 环境。

无对齐公式组：

~~~latex
\begin{gather}
a = b+c+d \\
x = y+z
\end{gather}
~~~

对齐公式组：

~~~latex
\begin{align}
a &= b+c+d \\
x &= y+z
\end{align}
~~~

multline，gather，align 等环境都有带 * 的版本，不生成公式编号。

#### 分支公式

分段函数通常用 cases 次环境写成分支公式：

~~~latex
\[y=\begin{cases}
	-x,\quad x\leq 0 \\
	x,\quad x>0
\end{cases}\]
~~~



### 定理和证明

\newtheorem 命令可以用来定义定理之类的环境，其语法如下：

> {环境名}[编号延续]{显示名}[编号层次]

下面的代码定制了四个环境：定义、定理、引理和推论，他们都在一个 section 内统一编号，而引理和推论会延续定理的编号：

~~~~latex
\newtheorem{definition}{定义}[section]
\newtheorem{theorem}{定理}[section]
\newtheorem{lemma}[theorem]{引理}
\newtheorem{corollary}[theorem]{推论}
~~~~

使用如下：

~~~latex
\begin{definition}
Java 是一种跨平台的编程语言。
\end{definition}
\begin{theorem}
咖啡因可以刺激人的中枢神经。
\end{theorem}
\begin{lemma}
茶和咖啡都会使人的大脑兴奋。
\end{lemma}
\begin{corollary}
晚上喝咖啡可能会导致失眠。
\end{corollary}
~~~

amsthm 宏包提供的 proof 环境可以用来输入证明，它会在证明结尾加一个 QED 符号：

~~~latex
\begin{proof}[命题物质无限可分的证明]
	一尺之棰，日取其半，万世不竭。
\end{proof}
~~~





## 插图

### 图形概览

#### 图形格式

LaTeX 支持点阵图形格式 JPEG 和 PNG，也支持矢量格式 EPS 和 PDF。对于示意图，我们应该首选矢量格式；包含大量自然色彩的图像（比如照片）应该选 JPEG；人工点阵图像应该选 PNG。

#### 图形优化

矢量图形的一个优点是可以无限缩放，而输出质量不变。图形尺寸对矢量图形而言意义不大。描述矢量图形所需数据较少，所以其文件体积一般也较小。

而点阵图形是以像素为单位描述、存储的，图形尺寸越大，文件体积就越大。当然影响文件体积的还有色彩深度、压缩算法等因素。

人们一般希望用较小的文件体积获取较好的输出效果，这就需要优化图形尺寸和色彩。

**图形尺寸**

点阵图形的像素是一种相对尺寸，其实际尺寸等于像素除以分辨率。最常用的分辨率单位是像素/英尺(PPI)。

比如有一幅100x150像素的点阵图形，其分辨率为100 PPI。在输出时，它的缺省尺寸就是1 inX1.5 in。如果强制输出为2 inX3 in，那么其实际分辨率就降为 50 PPI。



### 插入图形

#### 基本命令

基本用法如下：

~~~latex
\usepackage[dvipdfm]{graphicx}
\includegraphics[bb=0 0 300 200]{fig.png}
~~~

引用 graphicx 宏包时可加驱动选项。pdflatex 用 pdftex，但是它知道驱动就是自己，其实不用加该选项。

#### 图形操作

\includegraphics 命令有一些参数选项可以用于缩放、旋转、裁剪等图形操作，简要说明如下：

- 如果不设置任何尺寸参数，pdflatex 按缺省输出尺寸处理。如果图形文件缺少 PPI，会按72 PPI 计算输出尺寸。

| scale=s | 缩放比。绝对尺寸和缩放比用一种即可，同时使用两者，绝对尺寸起作用         |
| ------- | ---------------------------------------- |
|         | 保持图形比例。宽度和高度通常设置一个即可，否则图形比例会失调，除非再加上此选项，这样图形宽度和高度都不超过指定参数 |

~~~latex
\includegraphics[width=60pt]{homer.pdf}
\includegraphics[width=80pt,height=100pt,keepaspectratio]{homer.pdf}
~~~

#### 文件名和路径

若想省略文件后缀或路径名，可以使用如下命令：

~~~latex
\DeclareGraphicsExtensions{.eps,.mps,.pdf,.jpg,.png}
\graphicspath{{c:/secret-garden/}}
\graphicspath{{./img/}}
\graphicspath{{one-little/}{two-little/}{three-little-indians/}}
~~~

其中，第一行指定后缀列表让编译程序自行查找；后三行设置缺省搜索路径，分别使用了绝对路径、相对路径、多个路径。

**注意：**文件名和路径名都不能有空格；路径名分隔符最好用/，这样可以在多种操作系统上通用；路径名要用/