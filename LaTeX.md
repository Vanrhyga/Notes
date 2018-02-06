[TOC]

### 什么是 TeX/LaTeX？

TeX 是一门标记式宏语言，用来排版科技文章，尤其擅长处理复杂的数学公式，同时，也是处理这一语言的排版软件。

LaTeX 是在 TeX 基础上按内容/格式分离和模块化等思想建立的一种 TeX 上的格式。

**注意：TeX 并不适用于文书处理。**



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

