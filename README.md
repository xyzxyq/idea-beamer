# Idea 展示 Beamer 模板

面向论文撰写前的导师汇报。16:9、中文优先、现代卡片风格；框架图均为可编辑 TikZ。示例是通用方法结构，不代表已提出或已验证的科研贡献。

[查看 PDF 样稿](main.pdf) · [编辑示例内容](main.tex) · [主题与 TikZ 样式](idea-theme.sty)

## 开始使用

1. 修改 `main.tex` 顶部的标题、作者、课题组和日期。
2. 替换各页方括号内容；复制模块详情页或最后的框架组件页来添加模块。
3. 在此目录运行：

```sh
latexmk -xelatex -interaction=nonstopmode -halt-on-error main.tex
```

打开 `main.pdf`。需要 TeX Live / MacTeX 中的 Beamer、ctex、TikZ、Fandol 和 TeX Gyre Heros 字体；不依赖 macOS 专属字体，不需要 shell-escape。Overleaf 上传 `main.tex` 和 `idea-theme.sty`，编译器选择 XeLaTeX；该平台未在本次实测。

## 文件与页面

- `main.tex`：14 页示例，包括问题、假设、总框架、3 个模块、训练 / 推理、创新对照、实验设计、结果、消融、讨论及备用组件。
- `idea-theme.sty`：颜色、字体、标题页脚、提示条和 TikZ 样式。
- `main.pdf`：已编译样稿。

## 视觉规则

主色的原始定义：

```latex
\definecolor{IdeaPurple}{cmyk}{0.50,1.00,0.00,0.40}
```

浅灰紫色画布承托白色圆角卡片，深色短标题形成层级，指定深紫用于关键模块。总框架采用阶段分区，模块细节使用独立图面板；表格也置于白色面板内。方法图采用下表中的辅助色来区分职责，页面标题与导航仍使用指定紫色。CMYK 定义不等于经过 ICC 校色的印刷文件；不同屏幕、投影仪和印刷配置可能呈现不同紫色。

默认 11pt 正文、12pt 顶部标题、7pt 副标题、6.5pt 章节标记、9pt 图内文字。封面标题建议不超过两行；较长标题可以在 `\title` 中手动用 `\\` 换行。复杂框架优先拆页，避免用 `\resizebox` 把整张图缩得过小。节点的 `text width` 控制自动换行，坐标单位为 cm。实线表示前向信息流，虚线表示反馈。

```latex
\begin{frame}{模块标题}
  \framesubtitle{说明该页回答的问题}
  \centering
  \begin{tikzpicture}
    \node[idea io,text width=22mm] (x) at (0,0) {输入 $x$};
    \node[idea solid,text width=30mm] (m) at (4,0) {核心模块};
    \node[idea primary,text width=22mm] (y) at (8,0) {输出 $y$};
    \draw[idea arrow] (x)--(m);
    \draw[idea arrow] (m)--(y);
  \end{tikzpicture}
  \vfill
  \Takeaway{填写本页希望导师记住的一句话。}
\end{frame}
```

## 图优先版式

内容页标题与章节标记同处顶部一行，副标题为可选的小字说明。删除 `\framesubtitle{...}` 可进一步增加图面空间。封面保留标题、副标题、汇报人与日期，背景沿用内页的浅色画布、白色圆角面板和细边框，五色仅作小型色标，全部由 TikZ 绘制；课题组字段保留在源文件中，默认不显示。

第 4–8 页默认不放底部关键判断条，正文由框架图和必要说明组成。TikZ 使用 `y=1.4cm` 扩展纵向布局，节点文字不随坐标缩放；复杂图可以直接在腾出的空间内增加层级。第 14 页保留五色图例。

## 方法图语义配色

| 职责 | 颜色 | 色值 | TikZ 节点样式 |
| --- | --- | --- | --- |
| 数据与表征 | 蓝色 | `#3973B9` | `idea data` |
| 核心交互 | 指定紫色 | `C50 M100 Y0 K40` | `idea primary` / `idea solid` |
| 条件、先验与约束 | 琥珀色 | `#B78028` | `idea condition` |
| 任务输出 | 青绿色 | `#258579` | `idea result` |
| 验证、监督与反馈 | 珊瑚色 | `#BF665D` | `idea validation` |

节点使用浅色填充和深色文字，核心交互可使用深紫底白字。`idea io` 保留为无特定职责的中性节点。辅助色按 RGB 定义，指定紫色仍按 CMYK 定义。

颜色编码职责，而不是简单按 A/B/C 随意分配。同一模块内可包含多个职责：例如条件调制模块内，表征为蓝色、条件为琥珀色、交互为紫色、输出为青绿。箭头按所传信息选择颜色；默认紫色，反馈用虚线。节点名称、公式和线型应同时保留，确保含义不只依赖颜色。

```latex
\node[idea condition,text width=26mm] (c) at (4,1.5) {条件 / 先验};
\node[idea solid,text width=26mm] (m) at (4,0) {核心交互};
\draw[idea arrow,draw=IdeaAmber] (c)--(m);
% 反馈：\draw[idea feedback,draw=IdeaCoral] ...;
```

第 14 页提供五色复杂框架示例与完整图例，可复制为新的方法总览。每页按需使用辅助色，不必用满五种。

## 卡片与章节

- `\IdeaSection{02}{METHOD}{方法框架}`：同时设置章节与页眉标记。
- `\IdeaCard[30mm]{01 / TASK}{任务定义}{正文}`：等高卡片，30mm 为内部内容高度，上下各另留 4mm。
- `\Surface{内容}`：自适应高度的白色面板，内部表格使用 `\linewidth`。
- `\Takeaway{结论}`：底部关键判断；正式汇报替换为研究内容，也可删去以扩大图面。

## 结果与引用

结果表没有填入虚构数值。第 12 页曲线仅用于版式示意，页面上已标注；正式汇报请替换为真实数据绘图或导入矢量 PDF。指标、误差范围、单位、样本量和重复次数需明确。

尚无实验结果时，可删除结果与消融页，保留实验设计。使用文献支持论断时填写准确来源；模板未预置任何虚构参考文献。
