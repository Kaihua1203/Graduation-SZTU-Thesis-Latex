# SZTU Thesis LaTeX Template 结构说明

这份文档不是教你写 LaTeX 语法，而是教你理解这个模板的职责分层：

- 改内容，应该改哪里
- 改格式，应该改哪里
- 新增章节、图片、附录、参考文献，应该接到哪里
- 如果后续让 agent 自动改模板，应该先看哪些文件

## 1. 整体结构

这个模板是典型的“三层结构”：

1. `sztuthesis_main.tex`
   作用：总装配文件，决定整篇论文按什么顺序输出。
2. `content/*.tex`
   作用：承载你的论文内容。
3. `SZTUthesis.cls`
   作用：承载全局版式、字体、标题样式、页眉页脚、摘要环境、参考文献样式等。

可以把它理解成：

- `content/` 是“写什么”
- `SZTUthesis.cls` 是“长什么样”
- `sztuthesis_main.tex` 是“按什么顺序拼起来”

## 2. 各文件的职责

### 2.1 `sztuthesis_main.tex`

这是主入口文件。编译时通常就是编译它。

它负责：

- 载入文档类 `SZTUthesis`
- 加载少量额外宏包
- 定义一些辅助命令，如 `\pkg`、`\env`、`\app`
- `\input{content/info}` 读取封面元数据
- 按顺序输出封面、诚信声明、目录、摘要、正文、参考文献、附录
- 重新定义图表公式编号规则

你通常不需要频繁改它，但下面这些情况会改到它：

- 想把正文拆成多个文件并按顺序引入
- 想新增一个独立部分，比如“符号说明”或“术语表”
- 想额外加载宏包，而且更适合写在主文件而不是类文件
- 想调整整篇文档的总体顺序

### 2.2 `content/info.tex`

这是封面信息配置文件。

这里主要放：

- 论文中英文标题
- 专业、学院、学号、作者、导师
- 提交日期
- 中图分类号、学校代码、UDC
- 是否盲审

结论：

- 只改封面信息，就改这个文件。
- 不要去 `SZTUthesis.cls` 里直接写死个人信息。

### 2.3 `content/abstractcn.tex`

这里放中文摘要内容和中文关键词。

包括：

- `\keywordscn{...}`
- `\categorycn{...}`
- `\begin{abstractcn} ... \end{abstractcn}`

### 2.4 `content/abstracten.tex`

这里放英文摘要内容和英文关键词。

包括：

- `\keywordsen{...}`
- `\categoryen{...}`
- `\begin{abstracten} ... \end{abstracten}`

### 2.5 `content/content.tex`

这是正文主体。

默认从引言到结论都写在这里。这个模板当前没有强制把每章拆开，但你完全可以自己拆。

例如：

```tex
% 在 sztuthesis_main.tex 或 content/content.tex 中改成
\input{content/chapter1}
\input{content/chapter2}
\input{content/chapter3}
```

如果正文越来越长，建议拆章，不要把所有内容堆在一个文件里。

### 2.6 `content/additional.tex`

这是正文之后的补充内容区。

当前模板已经在这里放了致谢环境 `thankscontent`。通常这些内容也应放在这里：

- 致谢
- 附录
- 个人成果说明
- 其他放在正文后面的材料

### 2.7 `thesis-references.bib`

这是参考文献数据库。

所有新文献条目都应该加到这里，而不是直接手写到正文里。

正文引用时只写：

```tex
\cite{bibkey}
```

然后通过 BibTeX 自动排版。

### 2.8 `images/`

这是图片资源目录。模板已经在类文件中配置了图片搜索路径，所以正文插图时通常只写文件名即可。

建议：

- 所有论文图片都统一放这里
- 文件名尽量语义化，比如 `system-architecture.pdf`、`experiment-result-1.png`
- 不要把图片散落到仓库根目录

## 3. 修改内容时，应该去哪改

这是最重要的操作映射。

### 3.1 改题目、作者、导师、日期

去 `content/info.tex`。

### 3.2 改摘要、关键词

- 中文：`content/abstractcn.tex`
- 英文：`content/abstracten.tex`

### 3.3 改正文段落、章节标题、图表、公式

去 `content/content.tex`，或你后续拆分出来的 `content/chapter*.tex`。

### 3.4 改致谢、附录

去 `content/additional.tex`。

### 3.5 加新参考文献

去 `thesis-references.bib` 增加 bib 条目。

### 3.6 文中引用某条参考文献

回到正文文件，用：

```tex
\cite{你的bibkey}
```

### 3.7 加新图片

1. 先把文件放进 `images/`
2. 再在正文中插入：

```tex
\includegraphics[width=0.8\textwidth]{your-image.png}
```

## 4. 修改格式时，应该去哪改

原则很简单：

- 局部排版调整，先看正文 tex 文件
- 全局样式调整，优先看 `SZTUthesis.cls`
- 流程/顺序调整，改 `sztuthesis_main.tex`

## 5. `SZTUthesis.cls` 里各类格式控制的落点

如果你以后要改模板格式，这一节最关键。

### 5.1 改页面边距、行距、段距

看 `SZTUthesis.cls` 里这类设置：

- `\geometry{...}`：页边距
- `\renewcommand*{\baselinestretch}{...}`：全局行距倍率
- `\setlength{\baselineskip}{...}`：基准行距
- `\setlength{\parskip}{...}`：段间距
- `\setlength{\parindent}{2em}`：首行缩进

结论：

- 想整体变稀或变紧，优先改这里。
- 不要在正文里到处手工写 `\vspace` 解决全局排版问题。

### 5.2 改中英文字体

看 `SZTUthesis.cls` 的字体定义区：

- `\setCJKmainfont`：中文正文字体
- `\setCJKsansfont`：中文无衬线字体
- `\setCJKmonofont`：中文等宽字体
- `\setmainfont`：英文正文主字体
- `\setsansfont`：英文无衬线字体
- `\setmonofont`：英文等宽字体
- `\newCJKfontfamily`：自定义中文字体族，例如 `\heiti`、`\songti`、`\kaishu`

结论：

- 改默认中文/英文字体，去类文件。
- 如果只是正文某一小段临时改字体，可以在内容文件局部写字体命令。

### 5.3 改章节标题样式

看 `SZTUthesis.cls` 的：

- `\titleformat{\section}{...}`
- `\titleformat{\subsection}{...}`
- `\titleformat{\subsubsection}{...}`
- `\titlespacing{...}`

这些控制：

- 标题字号
- 是否居中
- 是否黑体/粗体
- 标题上下间距

结论：

- 想把一级标题改成左对齐，或者改字号，改这里。
- 不要在正文里某个 `\section` 前后手动塞空行去“模拟标题样式”。

### 5.4 改目录样式

看 `SZTUthesis.cls` 里的：

- `\titlecontents{section}`
- `\titlecontents{subsection}`
- `\titlecontents{subsubsection}`

这些控制：

- 目录缩进
- 目录字体
- 编号宽度
- 点线和页码样式

如果你只想改“目录标题”几个字，则主文件 `sztuthesis_main.tex` 里也有：

```tex
\renewcommand{\contentsname}{...}
```

### 5.5 改封面样式

看 `SZTUthesis.cls` 的 `\makecoverpage`。

封面上的这些内容都在这个宏里布局：

- 校名图片
- “本科毕业论文（设计）”字样
- 题目区域
- 姓名/学号/专业/学院/导师/日期表格

结论：

- 只改封面文字内容，用 `content/info.tex`
- 改封面的排版、字号、对齐、下划线长度，用 `\makecoverpage`

### 5.6 改诚信声明页

看 `SZTUthesis.cls` 的 `\announcement`。

如果学校以后改了声明文案或格式，直接改这个宏。

### 5.7 改页眉页脚和页码格式

看 `SZTUthesis.cls`：

- `\setheader`
- `\setabstractfooter`
- `\setfooter`

这些控制：

- 页眉显示什么文字
- 摘要部分用罗马页码还是阿拉伯页码
- 正文页脚的“第 X 页，共 Y 页”格式

### 5.8 改摘要样式

看 `SZTUthesis.cls` 里的：

- `abstractcn` 环境定义
- `abstracten` 环境定义
- `\keywordscn`、`\keywordsen`

如果要调整摘要标题字体、摘要正文样式、关键词前缀样式，都在这里改。

### 5.9 改图、表、算法标题样式

看 `SZTUthesis.cls`：

- `\captionsetup[subfigure]{...}`
- `\captionsetup[figure]{...}`
- `\captionsetup[table]{...}`
- `\captionsetup[algorithm]{...}`

这些控制：

- caption 字体
- caption 在上方还是下方
- 子图标题对齐方式
- 图表标题分隔符样式

### 5.10 改图片搜索路径

看 `SZTUthesis.cls` 的：

```tex
\graphicspath{{figures/}{figure/}{pictures/}{picture/}{pic/}{pics/}{image/}{images/}}
```

如果你新增了资源目录，比如 `assets/figures/`，应该在这里补路径。

### 5.11 改图表公式编号方式

编号逻辑分成两层：

1. `SZTUthesis.cls` 里通过 `\@addtoreset{figure}{section}` 等方式，让图表公式按节重置。
2. `sztuthesis_main.tex` 里通过：

```tex
\renewcommand{\thefigure}{\arabic{section}-\arabic{figure}}
\renewcommand{\theequation}{\arabic{section}-\arabic{equation}}
\renewcommand{\thealgorithm}{\arabic{section}-\arabic{algorithm}}
\renewcommand{\thetable}{\arabic{section}-\arabic{table}}
```

决定最终显示成什么样。

结论：

- 想改“显示格式”，优先改主文件里的 `\thefigure`、`\thetable` 等。
- 想改“什么时候重置计数器”，改类文件里的 reset 逻辑。

### 5.12 改参考文献样式

看 `SZTUthesis.cls` 的 `\setreference`。

这里控制：

- 参考文献标题加入目录
- 参考文献行距
- 参考文献字体
- 使用哪个 `.bst` 样式文件

当前模板使用：

```tex
\bibliographystyle{gbt7714-numerical}
\bibliography{thesis-references}
```

结论：

- 想换引用样式或参考文献标题样式，改这里。
- 想加/删某条具体文献，不在这里改，而是去 `thesis-references.bib`。

## 6. 新增内容时应该怎么放

### 6.1 新增一个正文章节

最简单的方法：直接在 `content/content.tex` 里继续写。

```tex
\section{新章节标题}
内容...
```

更推荐的方法：拆文件。

1. 新建 `content/chapter3.tex`
2. 在 `content/content.tex` 中接入：

```tex
\input{content/chapter3}
```

推荐拆分场景：

- 章节很多
- 每章都有大量图片和公式
- 你准备让 agent 自动维护章节内容

### 6.2 新增附录

因为主文件在正文后已经执行了 `\appendix`，所以附录内容通常直接写在 `content/additional.tex` 中即可。

可以写成无编号节，或按你的学校要求自行组织。

### 6.3 新增图片材料

放到 `images/`，然后正文里引用。

不要把图片直接嵌进 `SZTUthesis.cls`，除非那张图属于模板本身，比如封面校名图。

### 6.4 新增 bib reference

把新的 BibTeX 条目追加到 `thesis-references.bib`。

例如：

```bibtex
@article{your2026paper,
  author  = {Author, A. and Author, B.},
  title   = {Paper Title},
  journal = {Journal Name},
  year    = {2026}
}
```

然后正文：

```tex
\cite{your2026paper}
```

再重新完整编译一次。

## 7. 给 agent 的最小工作规则

如果后续让 agent 自动改这个模板，最稳的规则是：

1. 只改论文内容时，优先改 `content/` 和 `thesis-references.bib`。
2. 只改封面信息时，优先改 `content/info.tex`。
3. 需要改全局格式时，优先改 `SZTUthesis.cls`。
4. 需要改文档拼装顺序时，改 `sztuthesis_main.tex`。
5. 新图片默认放 `images/`。
6. 新文献默认放 `thesis-references.bib`。
7. 每次改完参考文献、目录、交叉引用后，都要多轮编译验证。

## 8. 建议的维护边界

为了避免把模板改乱，建议你自己和 agent 都遵守这条边界：

- `content/`：自由改，这是你的论文内容层。
- `thesis-references.bib`：自由改，这是你的文献层。
- `sztuthesis_main.tex`：谨慎改，这是文档装配层。
- `SZTUthesis.cls`：更谨慎改，这是全局样式层。

一个简单判断标准：

- 如果改动只影响某一段文字，那就别碰 `.cls`
- 如果改动影响整篇论文的视觉表现，才考虑碰 `.cls`

这样维护成本最低，也最不容易引入格式回归。
