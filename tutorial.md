# SZTU Thesis LaTeX Template 快速上手

这份文档的目标是让你尽快把这个模板跑起来，并知道日常写论文时最常用的命令放在哪里。

## 1. 最短路径

这个模板的主入口是 `sztuthesis_main.tex`，正常情况下你只需要改 `content/` 目录下的文件。

最常改的文件：

- `content/info.tex`：封面信息
- `content/abstractcn.tex`：中文摘要
- `content/abstracten.tex`：英文摘要
- `content/content.tex`：正文
- `content/additional.tex`：致谢、附录等
- `thesis-references.bib`：参考文献数据库

## 2. 编译方式

仓库已经带了 `Makefile`。

```bash
make
```

持续监听并自动编译：

```bash
make pvc
```

清理辅助文件：

```bash
make clean
make cleanall
```

如果你不用 `make`，至少要保证编译链是：

```bash
xelatex -> bibtex -> xelatex -> xelatex
```

原因：参考文献、目录、交叉引用都需要多轮编译。

## 3. 你应该先改哪里

### 3.1 封面信息

编辑 `content/info.tex`：

```tex
\titlecn{你的中文题目\\需要时可手动换行}
\titleen{Your English Title}
\priormajor{你的专业}
\author{你的姓名}
\supervisor{导师姓名}
\supervisortitle{导师职称}
\department{学院名称}
\studentid{学号}
\thesisdate{year=2026,month=4,day=9}
```

注意：

- 中文标题过长时，用 `\\` 手动断行。
- 盲审开关也在这个文件里：

```tex
\blindreviewtrue
% 或
\blindreviewfalse
```

### 3.2 摘要

中文摘要改 `content/abstractcn.tex`：

```tex
\keywordscn{关键词1；关键词2；关键词3}
\categorycn{TP391}
\begin{abstractcn}
这里写中文摘要。
\end{abstractcn}
```

英文摘要改 `content/abstracten.tex`：

```tex
\keywordsen{keyword1;~keyword2;~keyword3}
\categoryen{TP391}
\begin{abstracten}
Here is the English abstract.
\end{abstracten}
```

### 3.3 正文

正文默认写在 `content/content.tex`。

章节结构示例：

```tex
\section{引言}
\subsection{研究背景}
正文内容。

\section{方法}
\subsection{模型设计}
正文内容。
```

### 3.4 致谢和附录

改 `content/additional.tex`。

模板里已经有致谢环境：

```tex
\begin{thankscontent}
这里写致谢。
\end{thankscontent}
```

如果你要加附录，通常也放在这个文件里。

## 4. 日常最常用的 LaTeX 命令

这些是你写毕业论文时会高频用到的。

### 4.1 章节与结构

```tex
\section{一级标题}
\subsection{二级标题}
\subsubsection{三级标题}
```

分页：

```tex
\newpage
```

模板示例里很多章节之间手动用了 `\newpage`，删掉或保留都可以，按你的排版需求决定。

### 4.2 强调、引用、交叉引用

加粗强调：

```tex
\emph{重点内容}
```

注意：这个模板把 `\emph` 重定义成了加粗，不是默认斜体。

标签与引用：

```tex
\label{sec:intro}
\ref{sec:intro}
\eqref{eq:model}
```

### 4.3 插图

图片建议放 `images/` 目录。

```tex
\begin{figure}[hbt]
    \centering
    \includegraphics[width=0.6\textwidth]{your-image.png}
    \caption{图片标题}
    \label{fig:example}
\end{figure}
```

说明：

- 模板已经配置了图片搜索路径，放在 `images/` 后，正文里通常直接写文件名即可。
- `\caption` 最好和 `\label` 配套。
- 引用图片：`图~\ref{fig:example}`。

### 4.4 表格

```tex
\begin{table}[htb]
    \centering
    \caption{表格标题}
    \label{tab:example}
    \begin{tabular}{lll}
        \hline
        A & B & C \\
        \hline
        1 & 2 & 3 \\
        \hline
    \end{tabular}
\end{table}
```

### 4.5 公式

```tex
\begin{equation}
E = mc^2
\label{eq:example}
\end{equation}
```

引用公式：

```tex
公式~\eqref{eq:example}
```

### 4.6 列表

```tex
\begin{itemize}
    \item 第一项
    \item 第二项
\end{itemize}

\begin{enumerate}
    \item 第一项
    \item 第二项
\end{enumerate}
```

### 4.7 参考文献

正文引用：

```tex
\cite{lamport1994latex}
```

多个引用：

```tex
\cite{lamport1994latex,pritchard1969statistical}
```

## 5. 这个模板自带且值得记住的自定义命令

这些命令定义在 `sztuthesis_main.tex` 和 `SZTUthesis.cls` 中。

### 5.1 写封面信息时用到的命令

- `\titlecn{...}`：中文题目
- `\titleen{...}`：英文题目
- `\priormajor{...}`：专业
- `\author{...}`：作者
- `\supervisor{...}`：导师
- `\supervisortitle{...}`：导师职称
- `\department{...}`：学院
- `\studentid{...}`：学号
- `\thesisdate{year=...,month=...,day=...}`：日期
- `\clcnumber{...}`：中图分类号
- `\schoolcode{...}`：学校代码
- `\udc{...}`：UDC
- `\academiccategory{...}`：学术类别

### 5.2 写摘要时用到的命令/环境

- `\keywordscn{...}`：中文关键词
- `\categorycn{...}`：中文分类号
- `\begin{abstractcn} ... \end{abstractcn}`：中文摘要环境
- `\keywordsen{...}`：英文关键词
- `\categoryen{...}`：英文分类号
- `\begin{abstracten} ... \end{abstracten}`：英文摘要环境

### 5.3 正文里辅助排版的命令

这些主要是模板作者为了示例文本定义的：

- `\app{...}`：无衬线字体，常用于软件名
- `\oper{...}`：等宽字体，常用于命令名
- `\pkg{...}`：标记宏包名
- `\cls{...}`：标记类文件名
- `\bib{...}`：标记 bib 文件名
- `\format{...}`：标记格式名
- `\env{...}`：标记环境名

例如：

```tex
使用 \cls{SZTUthesis.cls} 控制格式。
通过 \cite{lamport1994latex} 插入参考文献。
图片来自 \oper{images/} 目录。
```

### 5.4 模板流程命令

这些一般不需要你频繁手写，但你需要知道它们存在：

- `\makecoverpage`：生成封面
- `\announcement`：生成诚信声明页
- `\setheader`：启用正文页眉
- `\setabstractfooter`：摘要页码样式
- `\setfooter`：正文页码样式
- `\setreference`：输出参考文献

## 6. 建议你的实际写作习惯

推荐按这个节奏使用模板：

1. 先改 `content/info.tex`，保证封面正确。
2. 再写 `abstractcn.tex` 和 `abstracten.tex`。
3. 正文只专注 `content/content.tex`。
4. 文中需要引用时马上加 `\label` 和 `\cite`，不要拖到最后统一补。
5. 新文献立即写入 `thesis-references.bib`。
6. 每完成一个章节就编译一次，别攒到最后一起修。

## 7. 常见问题

### 7.1 为什么图片文件明明在 `images/`，正文里却不用写 `images/xxx.png`？

因为类文件里已经设置了：

- `figures/`
- `figure/`
- `pictures/`
- `picture/`
- `pic/`
- `pics/`
- `image/`
- `images/`

所以一般直接写文件名就能找到。

### 7.2 为什么改了参考文献却没更新？

因为 BibTeX 需要多轮编译。重新跑一次完整流程：

```bash
make clean
make
```

或者手动执行：

```bash
xelatex sztuthesis_main.tex
bibtex sztuthesis_main
xelatex sztuthesis_main.tex
xelatex sztuthesis_main.tex
```

### 7.3 我只想写内容，不想碰格式，应该改哪些文件？

只改这些：

- `content/info.tex`
- `content/abstractcn.tex`
- `content/abstracten.tex`
- `content/content.tex`
- `content/additional.tex`
- `thesis-references.bib`

其余文件先不要动。
