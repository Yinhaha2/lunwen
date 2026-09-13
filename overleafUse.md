# Overleaf 工程使用说明（新手向）

本文面向第一次用 Overleaf 写 Springer / EMSE 论文的同学。读完应能：打开工程、看懂每个文件干什么、改一节文字、插一张图、加一条文献、成功编译出 PDF。

当前工程目录：

`C:\Users\Y2698\Desktop\研究生\毕设\Agentic_Performance_PR_Analysis__EMSE_`

论文题目（主文件里已写好）：**Beyond Merge Rate: Measuring Performance of Autonomous Coding Agents**

期刊模板：Springer `svjour3`（EMSE 常用扩展栏 `smallextended`）。

---

## 1. Overleaf 是什么，和本地文件夹是什么关系

Overleaf 是一个**在浏览器里编译 LaTeX 的网站**。你在网页里改 `.tex`，点 Recompile，右侧出 PDF。

本仓库是导师给的格式工程，可以：

1. **整夹上传到 Overleaf**（New Project → Upload Project，传 zip），之后主要在网页里改；或
2. **Overleaf 开 Git / Dropbox 同步**，本地 Cursor 改完再同步上去。

无论哪种，**主文件必须是** `ai-code-performance-EMSE.tex`。在 Overleaf 左上 Menu → Settings → Main document 里确认选的是它。没选对时，点编译会报 “找不到 `\documentclass`” 或编译到空文件。

本地用 TeX Live / MiKTeX 也可以编，命令一般是对主文件跑 pdfLaTeX。新手建议先只用 Overleaf，少装一套发行版。

---

## 2. 你每天实际怎么操作（最小闭环）

1. 打开 Overleaf 项目，确认 Main document = `ai-code-performance-EMSE.tex`。
2. **不要在主文件里写正文**。主文件只负责：标题、作者、按顺序 `\input` 各节。
3. 改哪一节，就打开哪一个同名 `.tex`（例如改引言就打开 `Introduction.tex`）。
4. 点 **Recompile**。第一次或改了参考文献后，再点一次（让交叉引用和文献编号稳定）。
5. 右侧 PDF 里搜你刚改的句子，确认进了稿，而不是改了没被 `\input` 的文件。
6. 有报错先看 **红色日志最后 20 行**，不要从第一行开始读。

保存习惯：Overleaf 自动保存。若同时在本地改，先约定**单一真相源**（只在 Overleaf 改，或只在本地改再上传），避免两头覆盖。

---

## 3. 工程结构（按“你会不会动它”分组）

```
Agentic_Performance_PR_Analysis__EMSE_/
├── ai-code-performance-EMSE.tex   # 主文件：标题、作者、节顺序
├── setup.tex                      # 宏、颜色、\fig \tab \eqn
├── svjour3.cls / svglov3.clo      # Springer 模板，不要改
├── natbib.bst                     # 参考文献样式，不要改
├── Abstract.tex
├── Introduction.tex
├── Background.tex
├── Approach.tex                   # 当前被主文件引用的「研究设置」
├── CaseStudyResult.tex            # RQ1–RQ4 结果
├── Discussion.tex
├── Threats.tex
├── RelatedWork.tex
├── Conclusions.tex
├── ACKNOWLEDGMENT.tex
├── Declarations.tex
├── ref.bib                        # 文献库
├── pics/                          # 图，现在基本是空的
├── CaseStudySetup.tex             # 旧论文残留，主文件里已注释
├── DeploymentInIndustry.tex       # 旧论文残留，主文件里已注释
├── overleafUse.md                 # 本说明
└── firstEdit.md                   # 各节初稿（尚未填进 .tex）
```

### 3.1 主文件在做什么

`ai-code-performance-EMSE.tex` 里真正决定**目录顺序**的是这一段：

```latex
\input{Abstract}
\keywords{Agentic Software\and Software Performance\and Performance Engineering \and Large Language Models}
\input{Introduction}
\input{Background}
\input{Approach}
% \input{CaseStudySetup}
\input{CaseStudyResult.tex}
% \input{DeploymentInIndustry}
\input{Discussion}
\input{Threats}
\input{RelatedWork}
\input{Conclusions}
\input{ACKNOWLEDGMENT}
\input{Declarations}
\bibliography{ref}
```

带 `%` 的行**不会编进 PDF**。所以：

- 现在读者看到的“方法/设置”来自 `Approach.tex`（节标题是 `Study setup`）。
- `CaseStudySetup.tex` 和 `DeploymentInIndustry.tex` 是上一篇黑盒性能回归论文的正文，只当格式化石，**不要往里面填我们的工作**。

若以后导师要求把 “Study setup” 改名或拆节，只改 `Approach.tex` 的 `\section{...}` 或主文件的 `\input` 顺序，不要复制一份新文件却忘了 `\input`。

### 3.2 各节文件现在的真实状态

| 文件 | 节标题 / label | 现在里面是什么 | 我们要做什么 |
|------|----------------|----------------|--------------|
| `Abstract.tex` | abstract | 半句 LLM 代码助手套话 | 按 `firstEdit.md` 换成我们的摘要 |
| `Introduction.tex` | Introduction, `sec:intro` | Copilot/CodeWhisperer 旧稿 + 已点出的 1,219 PR 线索 | 整节重写，保留贡献条目结构 |
| `Background.tex` | Background and example, `background` | 五个 agent 的空标题 + Copilot 生成函数的旧图 | 改成 agentic SE + 一条真实性能 PR 例子 |
| `Approach.tex` | Study setup, `approach` | HumanEval/MBPP 表 + 旧实验四步 + 别人的机器配置 | 改成语料构建、标注流水线、分析口径 |
| `CaseStudyResult.tex` | Case study results, `result` | RQ1–RQ4 标题和 Motivation 骨架已拟定 | 按 RQ 填 Motivation / Approach / Results |
| `Discussion.tex` | Discussion, `discussion` | 旧论文四个讨论点（语言、prompt、能耗、模型结构） | 换成我们的含义讨论 |
| `Threats.tex` | Threats, `threats` | 三个效度小标题，正文空 | 按本实证研究写具体威胁 |
| `RelatedWork.tex` | Related work, `related` | 两个空小节标题 | 填 agent / 性能工程 / PR 评审文献 |
| `Conclusions.tex` | Conclusion, `conclusion` | 半句 | 收束四个 RQ，不新增数字 |
| `ACKNOWLEDGMENT.tex` | （无节标题） | 空文件 | 有基金/致谢再写，没有就留空或删 `\input` |
| `Declarations.tex` | Declarations | Springer 固定声明模板 | 补作者贡献和数据链接 |
| `ref.bib` | — | 目前只有 2 条 | 每引用一篇就加一条 BibTeX |

### 3.3 不要改的文件

- `svjour3.cls`、`svglov3.clo`：期刊类文件。
- `natbib.bst`：文献样式。
- `setup.tex`：除非你要加宏。里面已经有 `\fig{}`、`\tab{}`、`\eqn{}`、`\sec{}`、`\todo{}`、`\jinfu{}`。

主文件里重复 `\usepackage` 很多（`setup.tex` 又装了一遍）。**先别整理宏包**，等正文稳定再清，避免为了“干净”把能编过的稿搞挂。

---

## 4. 编译、报错、常见坑

### 4.1 编译器

Menu → Settings → Compiler 选 **pdfLaTeX**。不要选 XeLaTeX / LuaLaTeX，除非以后要在正文里直接写中文（投稿 EMSE 用英文，不需要）。

### 4.2 改完文献必须再编一次

`\citep{foo2024}` 第一次常显示 `[?]`。原因是 LaTeX 要：编一回 → 跑 BibTeX → 再编一到两回。Overleaf 一般会自动做；若还是问号，点 Recompile 两次，或 Menu → Recompile from scratch。

### 4.3 新手最常碰到的错

| 现象 | 原因 | 怎么办 |
|------|------|--------|
| `File not found` 且指向某张图 | `\includegraphics` 路径和 `pics/` 对不上 | 路径用正斜杠 `pics/rq1/status.pdf`，先确认文件已上传 |
| `Undefined control sequence` | 用了没定义的命令 | 查是不是写在了未被 `\input` 的文件里，或宏包没装 |
| `Missing $ inserted` | 下划线、百分号没转义 | 正文里写 `56.9\%`、`file\_count`，或改用 `\code{file_count}` |
| `Label multiply defined` | 两个 `\label{RQ1}` | 全局唯一 |
| 引用一直是 `??` | 编的次数不够，或 label 写错 | 对一下 `\ref{RQ1}` 和 `\label{RQ1}` |
| 中文标点进了英文 PDF | 从微信/Word 粘贴了中文逗号、句号 | 正文只用英文标点 |
| 编译极慢 / 超时 | 图太大（几 MB 的 PNG） | 导出 PDF 矢量图，或压缩 PNG |

### 4.4 工程里已经埋的“旧论文地雷”

下面这些**看起来像正文，其实不是我们的工作**，填稿时要整段换掉，不要在其基础上润色：

- `Approach.tex` 的 HumanEval / AixBench / MBPP / EvalPerf 表
- `Approach.tex` 末尾的 i9-13900K / Ubuntu 实验机配置（那是旧动态性能测试，不是本挖掘研究）
- `Background.tex` 的 Copilot 生成函数图 `pics/background/update_example.pdf`
- `Discussion.tex` 的 prompt engineering、碳足迹、CodeLlama vs DeepSeek-Coder
- 整个 `CaseStudySetup.tex`、`DeploymentInIndustry.tex`（OpenMRS、Apache James、System X）

主文件标题、作者、单位已经是本篇的，可以保留。

---

## 5. 怎么写一节（对着文件填，而不是对着 PDF 空想）

以 RQ 结果为例，`CaseStudyResult.tex` 里每一问已经按导师模板拆成：

```latex
\subsection{RQ1: ...}
\label{RQ1}
\noindent\textbf{Motivation:}
...
\noindent\textbf{Approach:}
...
\noindent\textbf{Results:}
...
```

这是 EMSE 经验研究的标准块。填写规则：

1. **Motivation**：为什么要问，不问会误读什么（例如把 closed 当成“被审过并否决”）。
2. **Approach**：用了哪张表、哪个子集、分母是谁（全库 1219 / 终态 1180 / 被审 closed 子集）。
3. **Results**：先给主数字，再给表或图，再给一句读法。最后用 `tcolorbox` 收一条 Finding。
4. **不要在 Results 里写政策建议**。建议放到 RQ4 或 Discussion。

`firstEdit.md` 里每一节都有英文草稿 + 中文对照。往 Overleaf 填时：

- 填进 `.tex` 的是**英文**。
- 中文只留在 `firstEdit.md` 里给你和导师对齐，不要 `\input` 进主文件。

### 5.1 几个本工程已经定义好的命令

在任意节文件里可以直接用（定义在 `setup.tex` / 主文件）：

```latex
\fig{fig:overview}     % → Figure N
\tab{dataset}          % → Table N
\sec{RQ1}              % → Section N
\citep{key}            % (Author, year)
\citet{key}            % Author (year)
\code{fast_merge}      % 等宽代码字体
\todo{核对分母}         % 红色待办，投稿前删光
\jinfu{请确认这条口径}  % 给导师的紫色批注
```

草稿阶段多用 `\todo{}`，不要用 Word 式的“（待补）”散落在段落里。

---

## 6. 图怎么加

1. 在本地把图画好（优先 **PDF 矢量图**，其次 300 dpi PNG）。
2. 上传到 Overleaf 的 `pics/`，建议按节建子目录：

```
pics/
  background/motivating-example.pdf
  approach/pipeline.pdf
  rq1/status.pdf
  rq1/agent-merge.pdf
  rq1/closed-motivation.pdf
  rq2/lifespan.pdf
  rq3/failure-types.pdf
  rq4/boundary.pdf
```

3. 在对应 `.tex` 里写：

```latex
\begin{figure}[tb]
\centering
\includegraphics[width=0.95\columnwidth]{pics/rq1/status.pdf}
\caption{Outcome distribution of the 1,219 agent-authored performance PRs.}
\label{fig:rq1-status}
\end{figure}
```

通栏大图（流程、多子图）用 `figure*` 和 `width=\textwidth`。

正式仓 `analysis_viz/perf_pr_visualization.ipynb` 已规划 18 张分析图；**不要 18 张全进论文**。进哪些、每张证明什么，见 `firstEdit.md` 各节「配图意见」。

当前本地 `pics/` 是空的，旧模板引用的 `update_example.pdf`、`overview-new-new.drawio.pdf` 也不在工程里。编译 Background / Approach 时如果仍保留旧 `\includegraphics`，会直接报 File not found。填稿时删掉旧图命令，或先注释。

---

## 7. 表怎么加

短表用 `table` + `booktabs`；需要脚注用 `threeparttable`（主文件已装）。

```latex
\begin{table}[tb]
\centering
\caption{Outcome distribution of the study corpus.}
\label{tab:outcomes}
\begin{tabular}{lrrr}
\toprule
Status & Count & Share of 1,219 & Definition \\
\midrule
Merged & 669 & 54.9\% & \code{merged\_at} is not empty \\
Closed & 511 & 41.9\% & Terminal and not merged \\
Open   & 39  & 3.2\%  & Still open at snapshot \\
\bottomrule
\end{tabular}
\end{table}
```

口径一旦写进表，全文同一数字只能有一个来源。本项目主表和 LLM 分析表有 **2 条 PR 的状态差**（见 `firstEdit.md` 开篇「数字锁定」），投稿前必须锁死，禁止摘要 56.7%、结果 56.9% 混用。

---

## 8. 参考文献怎么加

1. 打开 `ref.bib`，按 BibTeX 追加一条，`key` 用稳定短名，例如 `li2025aidev`。
2. 正文用 `\citep{li2025aidev}` 或 `\citet{li2025aidev}`。
3. 不要在正文手写 “(Li et al., 2025)”——页码和作者列表交给 natbib。
4. Google Scholar → 引文 → BibTeX 可以粘，但要检查：书名大小写（用 `{AIDev}` 保护）、页码、年份、url。

现在 `ref.bib` 几乎是空的。引言里已经在引用的 `DBLP:conf/msr/NguyenN22`、`wang2025understandingcharacteristicscodegeneration` 可以留，但也要补 AIDev、性能工程、PR 评审、以及那两篇格式范文。

---

## 9. 建议的写作工作流（和 Cursor 怎么配合）

1. 先在 `firstEdit.md` 里和导师对齐：**叙事、口径、数字、图**。
2. 对齐一节，再把该节英文粘进对应 `.tex`，用 Overleaf 编译看版面。
3. 不要一次把 9 节全糊进去再编译——第一次几乎会因为旧图路径、未转义 `%`、未定义引用爆掉，难定位。
4. 每填完一节：Recompile → 扫 PDF 是否还有 HumanEval / System X / Copilot 函数示例。
5. 图单独一批：notebook 出图 → 放 `pics/` → 再插入。
6. 文献单独一批：每节引用列表中的论文补进 `ref.bib`。

本文件夹里的 `firstEdit.md`、`overleafUse.md` **不是论文的一部分**，不要 `\input` 它们。它们只给人和 Cursor 用。

---

## 10. 和正式仓、全量仓的关系（写论文时看哪份数）

| 位置 | 角色 | 写论文时 |
|------|------|----------|
| `datebase/影子仓` | 正式仓 | **数字、案例、标签以它为准** |
| `datebase/AI_Teammates_in_SE3/MyStudy` | 全量/开发仓 | 可看脚本和过程文档，数字冲突时丢掉它 |
| 本 Overleaf 工程 | 投稿稿 | 只放将发表的文字与图 |

正式仓里写稿最常用的几份：

- `finaldatabase/pr_master/perf_prs_expanded_final.csv`：1,219 条主表（GitHub 状态：merged 669 / closed 511 / open 39）
- `finaldatabase/README.md`、`summary/coverage_stats.json`：快照摘要
- `analysis_viz/RQ_Analysis.md`：四问已经写好的中文分析
- `analysis_viz/README.md`：图清单
- `full_analysis_distilled.csv`：LLM 结构化标签宽表（状态是 671 / 509 / 39，与主表差 2 条）
- `prompt.md` + `schema.json`：方法节要描述的标注协议

---

## 11. 投稿前再扫一遍

- [ ] 主文件里没有未注释的旧节
- [ ] PDF 全文搜不到 HumanEval、OpenMRS、System X、CodeLlama、EvalPerf
- [ ] 所有 `\todo` / `\jinfu` / 红色批注已删
- [ ] 图、表、公式、节交叉引用不是 `??`
- [ ] 摘要、引言、结论的三个头条数字一致
- [ ] `Declarations` 里的 replication 链接已换成真地址
- [ ] 作者、通讯作者、单位、邮箱与导师终稿一致

---

## 12. 你下一步只要做的三件事

1. 读 `firstEdit.md` 开篇「数字锁定」和「全文叙事」，先和导师确认口径。
2. 把 Overleaf Main document 设成 `ai-code-performance-EMSE.tex`，编译一次，看现在的骨架 PDF。
3. 从 `Abstract.tex` 和 `Introduction.tex` 开始填英文，不要从 Related Work 开始。
