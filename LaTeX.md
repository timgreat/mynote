## LaTeX

原链接：https://liam.page/2014/09/08/latex-introduction/

### Hello World

```latex
\documentclass{article}
%this is first line of latex
\begin{document}
Hello,world!!!
\end{document}
```

此处的`\documentclass{article}`中包含了一个控制序列，不被输出但会影响文档的输出效果。这里的控制序列是`documentclass`，`{}`中的参数代表调用名为`article`的文档类。

*部分控制序列还有被方括号 `[]` 包括的可选参数。*

接着是控制序列`begin`，总是与`end`成对出现。这两个控制序列及中间的内容被称为『环境』；它们之后的参数总是**一致的**，被称为『环境名』。

**只有在`document`环境中的内容才会被正常输出到文档中或作为控制序列对文档产生影响。**

从`\documentclass{article}`开始到`\begin{document}`之前的部分被称为导言区。导言区是对整篇文档进行设置的区域——在导言区出现的控制序列往往会影响整篇文档的格式。

### 中英文混排

宏包就是一系列控制序列的合集。这些序列非常常用，于是就被打包放在同一个文件中成为宏包。`\usepackages{}`被用于调用宏包。

```latex
\documentclass{article}
\usepackage{xeCJK} %调用 xeCJK 宏包
\setCJKmainfont{SimSun} %设置 CJK 主字体为 SimSun （宋体）
\begin{document}
你好，world!
\end{document}
```

```latex
%使用[UTF-8]的文档类选项
\documentclass[UTF8]{ctexart}
\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
你好，world!
\end{document}
```

### 组织文章 ###

```latex
\documentclass[UTF8]{ctexart}
\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
你好，world!
\end{document}
```

在导言区，定义了文档的标题、作者、日期。

在document环境中多加了一个控制序列`\maketitle`，这个控制序列能将导言区定义的标题、作者、日期按照预定的格式展现出来。

### 章节和段落

```latex
\documentclass[UTF8]{ctexart}
\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
\section{你好中国}
中国在East Asia.
\subsection{Hello Beijing}
北京是capital of China.
\subsubsection{Hello Dongcheng District}
\paragraph{Tian'anmen Square}
is in the center of Beijing
\subparagraph{Chairman Mao}
is in the center of 天安门广场。
\subsection{Hello 山东}
\paragraph{山东大学} is one of the best university in 山东。
\end{document}
```

在文档类中，定义了五个控制序列来调整行文组织结构，分别是:

- `\section{}`

- `\subsection{}`

- `\subsubsection{}`

- `paragraph{}`

- `\subparagraph{}`

`section`代表章节名，`subsection`代表子章节名。。。`paragraph`代表段开头，会被加粗。。。

### 插入目录

在`\maketitle`下面插入控制序列`\tableofcontents`，即可生成目录。

**在一个章节中，两段间要用两个换行分开。**

### 数学公式

为了使用latex的数学功能，需要在导言区加载`amsmath`宏包：

```latex
\usepackage{amsmath}
```

#### 数学模式

latex的数学模式有两种：行内模式与行间模式。前者在正文的行文中，插入数学公式；后者独立排列单独成行并自动居中。

在行文中，、使用`$...$`可以插入行内公式，使用`\[...\]`插入行间公式，如果需要对行间公式进行编号，可以使用`equation`环境：

```latex
\begin{equation}
...
\end{equation}
```

#### 上下标

```latex
\documentclass{article}
\usepackage{amsmath}
\begin{document}
Einstein 's $E=mc^2$.

\[ E=mc^2. \]

\begin{equation}
E=mc^2.
\end{equation}
\end{document}
```

**行内公式和行间公式对标点的要求是不同的：行内公式的标点，应该放在数学模式的限定符之外，而行间公式则应该放在数学模式限定符之内。**

在数学模式中使用`^`表示上标，使用`_`表示下标。它默认只作用之后的一个字符，如果相对连续的几个字符起作用，可以用`{}`括起来。

```latex
\[ z = r\cdot e^{2\pi i}. \]
```

#### 根式与分式

根式使用`\sqrt{}`来表示，分式用`\frac{分子}{分母}`表示。

```latex
\documentclass{article}
\usepackage{amsmath}
\begin{document}
$\sqrt{x}$, $\frac{1}{2}$.

\[ \sqrt{x}, \]

\[ \frac{1}{2}. \]
\end{document}
```

在行间公式和行内公式中，分式的输出效果是有差异的。如果要强制行内模式的分式显示为行间模式的大小，可以使用 `\dfrac`, 反之可以使用 `\tfrac`。

> 在行内写分式，你可能会喜欢 `xfrac` 宏包提供的 `\sfrac` 命令的效果。

> 排版繁分式，你应该使用 `\cfrac` 命令。

#### 运算符

一些小的运算符，可以在数学模式下直接输入，另一些需要用控制序列生成，如：

```latex
\[ \pm\; \times \; \div\; \cdot\; \cap\; \cup\;\geq\; \leq\; \neq\; \approx \; \equiv \]
```

![image-20230830094255397](/home/timi/.config/Typora/typora-user-images/image-20230830094255397.png)

连加、连乘、极限、积分等大型运算符分别用 `\sum`, `\prod`, `\lim`, `\int` 生成。他们的上下标在行内公式中被压缩，以适应行高。我们可以用 `\limits` 和 `\nolimits` 来强制显式地指定是否压缩这些上下标。如：

```latex
$ \sum_{i=1}^n i\quad \prod_{i=1}^n $
$ \sum\limits _{i=1}^n i\quad \prod\limits _{i=1}^n $
\[ \lim_{x\to0}x^2 \quad \int_a^b x^2 dx \]
\[ \lim\nolimits _{x\to0}x^2\quad \int\nolimits_a^b x^2 dx \]
```

![image-20230830094218494](/home/timi/.config/Typora/typora-user-images/image-20230830094218494.png)

多重积分可以使用 `\iint`, `\iiint`, `\iiiint`, `\idotsint` 等命令输入。

![image-20230830093913589](/home/timi/.config/Typora/typora-user-images/image-20230830093913589.png)

#### 定界符

各种括号用 `()`, `[]`, `\{\}`, `\langle\rangle` 等命令表示；注意花括号通常用来输入命令和环境的参数，所以在数学公式中它们前面要加 `\`。对于`||与|| ||`，amsmath 宏包推荐用 `\lvert\rvert` 和 `\lVert\rVert` 取而代之。

![image-20230830094840481](/home/timi/.config/Typora/typora-user-images/image-20230830094840481.png)

为了调整这些定界符的大小，amsmath 宏包推荐使用 `\big`, `\Big`, `\bigg`, `\Bigg` 等一系列命令放在上述括号前面调整大小。

```latex
\[ \Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr) \]
\[ \Biggl[\biggl[\Bigl[\bigl[[x]\bigr]\Bigr]\biggr]\Biggr] \]
\[ \Biggl \{\biggl \{\Bigl \{\bigl \{\{x\}\bigr \}\Bigr \}\biggr \}\Biggr\} \]
\[ \Biggl\langle\biggl\langle\Bigl\langle\bigl\langle\langle x
\rangle\bigr\rangle\Bigr\rangle\biggr\rangle\Biggr\rangle \]
\[ \Biggl\lvert\biggl\lvert\Bigl\lvert\bigl\lvert\lvert x
\rvert\bigr\rvert\Bigr\rvert\biggr\rvert\Biggr\rvert \]
\[ \Biggl\lVert\biggl\lVert\Bigl\lVert\bigl\lVert\lVert x
\rVert\bigr\rVert\Bigr\rVert\biggr\rVert\Biggr\rVert \]
```

![image-20230830095142150](/home/timi/.config/Typora/typora-user-images/image-20230830095142150.png)

#### 省略号

省略号用 `\dots`, `\cdots`, `\vdots`, `\ddots` 等命令表示。`\dots` 和 `\cdots` 的纵向位置不同，前者一般用于有下标的序列。

```latex
\[ x_1,x_2,\dots ,x_n\quad 1,2,\cdots ,n\quad
\vdots\quad \ddots \]
```

![image-20230830095716152](/home/timi/.config/Typora/typora-user-images/image-20230830095716152.png)

#### 矩阵

`amsmath` 的 `pmatrix`, `bmatrix`, `Bmatrix`, `vmatrix`, `Vmatrix` 等环境可以在矩阵两边加上各种分隔符。

```latex
\[ \begin{pmatrix} a&b\\c&d \end{pmatrix} \quad
\begin{bmatrix} a&b\\c&d \end{bmatrix} \quad
\begin{Bmatrix} a&b\\c&d \end{Bmatrix} \quad
\begin{vmatrix} a&b\\c&d \end{vmatrix} \quad
\begin{Vmatrix} a&b\\c&d \end{Vmatrix} \]
```

![image-20230830100337640](/home/timi/.config/Typora/typora-user-images/image-20230830100337640.png)

使用 `smallmatrix` 环境，可以生成行内公式的小矩阵。

```latex
Marry has a little matrix $ ( \begin{smallmatrix} a&b\\c&d \end{smallmatrix} )
```

![image-20230830100612884](/home/timi/.config/Typora/typora-user-images/image-20230830100612884.png)

#### 多行公式

有的公式特别长，我们需要手动为他们换行；有几个公式是一组，我们需要将他们放在一起；还有些类似分段函数，我们需要给它加上一个左边的花括号。

#### 长公式

##### 不对齐

无需对齐可以使用`mutline`环境。

```latex
\begin{multline}
x = a+b+c+{} \\
d+e+f+g
\end{multline}
```

![image-20230830101027601](/home/timi/.config/Typora/typora-user-images/image-20230830101027601.png)

##### 对齐

需要对齐的公式，可以使用 `aligned` *次环境*来实现，它必须包含在数学环境之内。

```latex
\[\begin{aligned}
x ={}& a+b+c+{} \\
&d+e+f+g
\end{aligned}\]
```

![image-20230830101213246](/home/timi/.config/Typora/typora-user-images/image-20230830101213246.png)

#### 公式组

无需对齐的公式组可以使用 `gather` 环境，需要对齐的公式组可以使用 `align` 环境。他们都带有编号，如果不需要编号可以使用带星号的版本。

```latex
\begin{gather}
a = b+c+d \\
x = y+z
\end{gather}
\begin{align}
a &= b+c+d \\
x &= y+z
\end{align}
```

#### 分段函数

分段函数可以用`cases`次环境来实现，它必须包含在数学环境之内。

```latex
\[ y= \begin{cases}
-x,\quad x\leq 0 \\
x,\quad x>0
\end{cases} \]
```

![image-20230830102345202](/home/timi/.config/Typora/typora-user-images/image-20230830102345202.png)

### 插入图片与表格

#### 图片

在latex中插入图片可以使用graphicx宏包提供的`\includegraphics`命令。

```latex
\documentclass{article}
\usepackage{graphicx}
\begin{document}
%\includegraphics[width = .8\textwidth]{a.jpg}宽度缩放到原来的80%
\includegraphics{a.jpg}
\end{document}
```

#### 表格

`tabular` 环境提供了最简单的表格功能。它用 `\hline` 命令表示横线，在列格式中用 `|` 表示竖线；用 `&` 来分列，用 `\\` 来换行；每列可以采用居左、居中、居右等横向对齐方式，分别用 `l`、`c`、`r` 来表示。

```latex
\begin{tabular}{|l|c|r|}
 \hline
操作系统& 发行版& 编辑器\\
 \hline
Windows & MikTeX & TexMakerX \\
 \hline
Unix/Linux & teTeX & Kile \\
 \hline
Mac OS & MacTeX & TeXShop \\
 \hline
通用& TeX Live & TeXworks \\
 \hline
\end{tabular}
```

xxxxxxxxxx str_view("(1st) other (2nd)", r"{\(.+?\)}")## [1] │ <(1st)> other <(2nd)>R

#### 浮动体

插图和表格通常需要占据大块空间，所以在文字处理软件中我们经常需要调整他们的位置。`figure` 和 `table` 环境可以自动完成这样的任务；这种自动调整位置的环境称作浮动体(float)。我们以 `figure` 为例。

```latex
\begin{figure}[htbp]
\centering
\includegraphics{a.jpg}
\caption{有图有真相}
\label{fig:myphoto}
\end{figure}
```

### 版面设置

#### 页边距

设置页边距，可以使用geometry宏包。

比如将纸张的长度设置为 20cm、宽度设置为 15cm、左边距 1cm、右边距 2cm、上边距 3cm、下边距 4cm，可以在导言区加上这样几行：

```latex
\usepackage{geometry}
\geometry{papersize={20cm,15cm}}
\geometry{left=1cm,right=2cm,top=3cm,bottom=4cm}
```

#### 页眉页脚

设置页眉页脚，可以使用fancyhdr宏包。

比如在页眉左边写上我的名字，中间写上今天的日期，右边写上我的电话；页脚的正中写上页码；页眉和正文之间有一道宽为 0.4pt 的横线分割，可以在导言区加上如下几行：

```latex
\usepackage{fancyhdr}
\pagestyle{fancy}
\lhead{\author}
\chead{\date}
\rhead{152xxxxxxxx}
\lfoot{}
\cfoot{\thepage}
\rfoot{}
\renewcommand{\headrulewidth}{0.4pt}
\renewcommand{\headwidth}{\textwidth}
\renewcommand{\footrulewidth}{0pt}
```

#### 首行缩进

CTeX 宏集已经处理好了首行缩进的问题（自然段前空两格汉字宽度）。因此，使用 CTeX 宏集进行中西文混合排版时，我们不需要关注首行缩进的问题。

#### 行间距

通过 `setspace` 宏包提供的命令来调整行间距。比如在导言区添加如下内容，可以将行距设置为字号的 1.5 倍：

```latex
\usepackage{setspace}
\onehalfspacing
```

#### 段间距

可以通过修改长度 `\parskip` 的值来调整段间距。例如在导言区添加以下内容：

```latex
\addtolength{\parskip}{.4em}
```

则可以在原有的基础上，增加段间距 0.4em。如果需要减小段间距，只需将该数值改为负值即可。