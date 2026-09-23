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

默认正文 9pt、顶部主标题 12pt、方法图 8pt。它们是可修改的起点，不是字号限制：页面可使用 `\small`、`\footnotesize` 或 `\fontsize{7.5}{9.5}\selectfont`；整图和单个节点可使用 `font=...`。封面保留独立标题与副标题，内容页页眉仅显示主标题。

节点默认按文字内容和内边距自适应，不指定固定宽高。短标签不必设置 `text width`；长段落可按需设置该值控制换行。优先在同一页组织完整方法，用分组、相对定位、局部字号及间距调整容纳复杂结构；没有强制拆页或最小字号规则。节点尺寸随内容变化，TikZ 不会自动重排整张图：需要自适应间距时使用 `right=... of ...` 等相对定位。`scale` 只改变坐标，只有加上 `transform shape` 才同时缩放节点和文字。

```latex
\begin{frame}{模块标题}
  \centering
  \begin{tikzpicture}[idea diagram,node distance=8mm]
    \node[idea data] (x) {输入 $x$};
    \node[idea solid,right=of x,font=\footnotesize] (m) {核心模块};
    \node[idea result,right=of m] (y) {输出 $y$};
    \draw[idea arrow] (x)--(m);
    \draw[idea arrow] (m)--(y);
  \end{tikzpicture}
  \vfill
  \Takeaway{填写本页希望导师记住的一句话。}
\end{frame}
```

## 图优先版式

内容页页眉仅保留主标题，不显示副标题与右侧章节标记。封面保留标题、副标题、汇报人与日期，背景沿用内页的浅色画布、白色圆角面板和细边框，五色仅作小型色标，全部由 TikZ 绘制；课题组字段保留在源文件中，默认不显示。

第 4–8 页默认不放底部关键判断条，正文由框架图和必要说明组成。TikZ 使用 `y=1.4cm` 扩展纵向布局，节点文字不随坐标缩放；复杂图可以直接在腾出的空间内增加层级。第 14 页将多源编码、检索增强、多任务决策和验证反馈完整放在同一页，提供九色图例与虚线分组。

## 方法图语义配色

| 职责 | 颜色 | 色值 | TikZ 节点样式 |
| --- | --- | --- | --- |
| 数据与表征 | 蓝色 | `#3973B9` | `idea data` |
| 核心交互 | 指定紫色 | `C50 M100 Y0 K40` | `idea primary` / `idea solid` |
| 条件、先验与约束 | 琥珀色 | `#B78028` | `idea condition` |
| 任务输出 | 青绿色 | `#258579` | `idea result` |
| 验证、监督与反馈 | 珊瑚色 | `#BF665D` | `idea validation` |
| 特征编码与变换 | 青蓝色 | `#248C9E` | `idea feature` |
| 记忆、知识库与检索 | 靛紫色 | `#6967AC` | `idea memory` |
| 候选筛选与采样 | 橄榄绿 | `#7C873C` | `idea selection` |
| 拟议创新点 | 玫红色 | `#B32672` | `idea innovation` |

节点使用浅色填充和深色文字，核心交互可使用深紫底白字。`idea io` 保留为无特定职责的中性节点。辅助色按 RGB 定义，指定紫色仍按 CMYK 定义。

颜色编码职责，而不是简单按 A/B/C 随意分配。同一模块内可包含多个职责：例如条件调制模块内，表征为蓝色、条件为琥珀色、交互为紫色、输出为青绿。箭头按所传信息选择颜色；默认紫色，反馈用虚线。节点名称、公式和线型应同时保留，确保含义不只依赖颜色。

```latex
\node[idea condition,text width=26mm] (c) at (4,1.5) {条件 / 先验};
\node[idea solid,text width=26mm] (m) at (4,0) {核心交互};
\draw[idea arrow,draw=IdeaAmber] (c)--(m);
% 反馈：\draw[idea feedback,draw=IdeaCoral] ...;
```

第 14 页提供九色复杂框架示例与完整图例，可复制为新的方法总览。每页按需使用颜色，不必用满全部颜色。

## 突出创新点

将拟议创新模块的节点样式改为 `idea innovation`，即可得到玫红浅底、加粗边框和右上角内嵌的 5.5pt“创新”标签：

```latex
\node[idea innovation,font=\small]
  (novel) at (4,0) {提出的模块\\关键机制};
```

“创新”是贡献属性，与模块职责不同：常规交互使用紫色，拟议创新才使用玫红。第 4、6、14 页已给出示例；样式自动增加节点上下内边距，为内部角标与正文留出间隔，不再占用框外空间。用标题或正文解释具体新在哪里，不能仅靠配色宣称新颖性；这里的标记是模板占位，需通过文献核查确认。

## 自适应节点与虚线分组

节点不设置固定宽高；公式、换行和字号决定自然尺寸。样式放在前面，局部覆盖放在后面：

```latex
\begin{tikzpicture}[idea diagram,node distance=8mm,font=\footnotesize]
  \node[idea feature] (a) {编码器};
  \node[idea feature,right=of a,font=\fontsize{7}{9}\selectfont]
    (b) {多尺度变换\\局部细节};
  \node[idea innovation,right=of b,font=\small]
    (c) {创新融合\\$h=g(z)$};
  \draw[idea arrow] (a)--(b);
  \draw[idea arrow] (b.east)--(c.west);
  \begin{scope}[on background layer]
    \node[idea dashed group,draw=IdeaCyan!65,fit=(a)(b)] (group) {};
    \node[idea group label] at (group.north west) {表征模块};
  \end{scope}
\end{tikzpicture}
```

`fit=(a)(b)` 自动包围指定节点；增删成员即可调整整体模块边界。使用 `inner sep` 调整分组留白，使用 `draw=...` 修改分组颜色。分组虚线不带箭头，表示结构归属；反馈虚线带箭头，表示信息流向。`idea box` 继承整图字体，`idea note`、`idea group label` 的默认小字也可通过后置 `font=...` 覆盖。创新角标保留小字号及无间隙圆角衔接，模块正文可独立调整。

## 卡片与章节

- `\IdeaSection{02}{METHOD}{方法框架}`：设置章节信息，章节名称显示在页脚。
- `\IdeaCard[30mm]{01 / TASK}{任务定义}{正文}`：等高卡片，30mm 为内部内容高度，上下各另留 4mm。
- `\Surface{内容}`：自适应高度的白色面板，内部表格使用 `\linewidth`。
- `\Takeaway{结论}`：底部关键判断；正式汇报替换为研究内容，也可删去以扩大图面。

## 结果与引用

结果表没有填入虚构数值。第 12 页曲线仅用于版式示意，页面上已标注；正式汇报请替换为真实数据绘图或导入矢量 PDF。指标、误差范围、单位、样本量和重复次数需明确。

尚无实验结果时，可删除结果与消融页，保留实验设计。使用文献支持论断时填写准确来源；模板未预置任何虚构参考文献。


## 复杂框架的连线布局

先按“输入 → 编码 → 交互 → 输出”排布列，再在列间保留走线通道。主流程朝右，条件从上 / 下接入，反馈沿图的外缘返回。分组框与通道之间保留空隙；多个输入分别落在模块边上的独立端口，避免全部挤在同一个点。节点变宽后检查通道，必要时移动整列。

提供以下原生 TikZ 组件，不需要外部布局程序：

| 组件 | 用途 |
| --- | --- |
| `\IdeaRouteX[样式]{源端口}{目标端口}{通道坐标}` | 水平出发，沿指定 x 通道转向，再水平进入；最多两个直角弯 |
| `\IdeaRouteY[样式]{源端口}{目标端口}{通道坐标}` | 垂直出发，沿指定 y 通道转向，再垂直进入；适合条件、残差和反馈 |
| `idea bus` / `idea junction` | 无箭头的共享干线 / 实心连接点；同一个信号分发时只画一次干线 |
| `idea edge label` | 带背景留白的连线标签，放在空白直线段旁 |
| `idea crossing` | 无法避免交叉时，局部用底色隔开上层线，表示跨越而非连接 |

```latex
% a、b 是已创建的节点；通道由两框边缘计算，随框尺寸移动。
\coordinate (lane) at ($(a.east)!.5!(b.west)$);
\IdeaRouteX[draw=IdeaBlue]{a.east}{b.west}{lane}

% 两个输入分别进入左边缘的 28% / 72% 位置。
\coordinate (in-top) at ($(b.north west)!.28!(b.south west)$);
\coordinate (in-bottom) at ($(b.north west)!.72!(b.south west)$);
% 用 in-top / in-bottom 替换目标端口即可。

% 反馈通道：放在所有模块和分组标题的下方。
\coordinate (return-lane) at (0,-3);
\IdeaRouteY[idea feedback,draw=IdeaCoral]{b.south}{a.south}{return-lane}
```

第 14 页给出 13 个节点的完整示例：两路编码使用独立端口，条件与检索从下方输入，输出通过共享干线分发，反馈走外围通道。箭头使用统一线宽、小箭头和轻微圆角，避免锐角及随意曲线。共享干线仅用于同一信号的分发；不同信号不可为美观合并。交叉处没有实心点表示不连接，存在连接时明确加点。

这些组件是**可控的直角布线工具，不是自动避障算法**。通道应位于空白区域，不能穿过其他模块；拥挤时先移动节点或通道，再局部减小字号。`idea crossing` 只应用于交叉点附近的一小段，放在最后绘制，背景不是浅灰画布时需覆盖 `preaction` 的颜色；不要给整条长连线使用遮罩。
