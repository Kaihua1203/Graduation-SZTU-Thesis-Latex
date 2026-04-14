# ARCHITECTURE

## 项目概览

本项目是一个深圳技术大学本科毕业论文 LaTeX 模板仓库，用来生成符合学校格式要求的论文 PDF。

核心流程是：在 `content/` 中填写论文内容与元信息，由 `sztuthesis_main.tex` 按固定顺序组装全文，`SZTUthesis.cls` 提供版式与命令定义，最后通过 `Makefile` 调用 `latexmk/xelatex + bibtex` 完成编译。

## 主要模块

- `sztuthesis_main.tex`
  论文主入口，负责组织封面、声明、目录、摘要、正文、参考文献和附录等阶段。
- `content/`
  论文内容层，存放封面信息、摘要、正文和附加内容，是日常写作的主要修改位置。
- `SZTUthesis.cls`
  模板样式层，负责页面布局、字体、标题、页眉页脚、摘要环境、参考文献接入等统一规则。
- `thesis-references.bib`
  参考文献数据源，供 BibTeX 生成参考文献列表与引用条目。
- `Makefile`
  构建入口，封装编译、持续监听、清理和字数统计命令。
- `images/` 与 `fonts/`
  静态资源层，分别提供插图资源和模板依赖字体。
- `official_documents/`
  外部规范参考，不参与编译链路，但用于对照学校要求。

## 模块关系

`Makefile` 驱动 `sztuthesis_main.tex` 编译。

`sztuthesis_main.tex` 通过 `\input` / `\include` 读取 `content/` 中的内容文件，并依赖 `SZTUthesis.cls` 提供的命令与环境完成排版。

`content/` 中的正文会引用 `images/` 资源，参考文献引用会落到 `thesis-references.bib`，最终由类文件中接入的国标参考文献样式输出。

## 架构约束

- 内容与样式分离：论文文本应优先写在 `content/`，不要把版式规则堆到正文文件里。
- 主文件负责装配顺序：新增或调整章节结构，优先改 `sztuthesis_main.tex`，不要把流程控制分散到类文件。
- 类文件负责统一规范：页边距、字体、标题、页眉页脚、环境定义等应集中放在 `SZTUthesis.cls`。
- 构建依赖固定编译链：默认假设使用 XeLaTeX，并结合 BibTeX 生成参考文献。
- 资源目录应保持被动依赖：`images/`、`fonts/` 提供素材，不应反向依赖内容或样式层。

## 阅读入口

- 想写论文内容：先看 `content/info.tex`、`content/abstract*.tex`、`content/content.tex`、`content/additional.tex`。
- 想调整文档组织顺序：先看 `sztuthesis_main.tex`。
- 想修改封面、标题、页眉页脚、摘要环境或参考文献样式：先看 `SZTUthesis.cls`。
- 想处理编译、清理或自动监听：先看 `Makefile`。
- 想核对学校规范来源：看 `official_documents/` 和 `README.md`。
