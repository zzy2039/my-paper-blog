---
title: "通过机器学习求解混合整数规划：综述"
description: "原论文中文译读校订版：补齐 5 张原图，保留公式、算法、表格和参考文献；结合来源页码与校注辨析关键概念。"
pubDate: 2026-10-25
updatedDate: 2026-10-25
---

# 通过机器学习求解混合整数规划：综述

> 原文：*A Survey for Solving Mixed Integer Programming via Machine Learning*  
> 作者：Jiayi Zhang¹、Chang Liu¹、Junchi Yan¹（通讯作者）、Xijun Li²、Hui-Ling Zhen²、Mingxuan Yuan²  
> 原文版本：arXiv:2203.02878v1，2022 年 3 月 6 日  
> **图文校订版（2026-10-25）：** 按原论文结构译读，保留公式、算法、两张方法汇总表与作者—年份引文，补齐全部 5 张原图。图片从用户提供的 PDF 按原图区域导出为 SVG，原 PDF 中已有的位图元素仍保留，不能保证每张原图都是纯矢量；图内英文保持原样，配有中文图注与术语说明。标为“校注”的内容是编辑补充，不属于原论文结论；原文有争议的说法与算法笔误会在相邻位置说明。下文页码按 8 页 PDF 的文件页序计。

**作者单位：**

1. 上海交通大学计算机科学与工程系、人工智能教育部重点实验室
2. 华为有限公司诺亚方舟实验室

**联系邮箱：**  
{zhangjiayirr, only-changer, yanjunchi}@sjtu.edu.cn  
{xijun.li, zhenhuiling2, yuan.mingxuan}@huawei.com

## 摘要

*原文定位：PDF 第 1 页。*

本文综述了利用机器学习求解混合整数规划（mixed integer programming，MIP）问题的发展趋势。从理论上看，MIP 是一个 NP 难问题，而且大多数组合优化（combinatorial optimization，CO）问题都可以表述为 MIP。与其他组合优化问题一样，人工设计的 MIP 启发式算法依赖良好的初始解，并且会消耗大量计算资源。因此，我们考虑应用机器学习方法求解 MIP，因为机器学习增强的方法能够根据历史数据中的典型模式给出解。

本文首先介绍 MIP 的数学表述和预备知识，以及几种求解 MIP 的传统算法。随后，我们讨论机器学习与 MIP 的多种融合途径，并介绍相关学习方法；这些方法可以分为精确算法和启发式算法。最后，我们展望基于学习的 MIP 求解器，讨论如何走向 MIP 之外更多的组合优化问题，以及传统求解器与机器学习组件如何相互融合。

## 1 引言

*原文定位：PDF 第 1–2 页。*

混合整数规划问题（MIP）是组合优化（CO）问题的重要组成部分。得益于学术理论和商业软件的发展，MIP 已经成为服务于众多应用的重要求解工具，其应用包括规划 [Pochet and Wolsey, 2010; Wu et al., 2013]、调度 [Sawik, 2011; Keha et al., 2009]、路径规划 [Malandraki and Daskin, 1992; Schouwenaars et al., 2001] 和装箱 [Gajda et al., 2022]。此外，[Paulus et al., 2021] 成功地将整数规划求解器集成到神经网络架构中，把它作为一个可微分层，使其能够同时学习成本项和约束。由此得到的端到端可训练架构，既能从原始数据中提取特征，又能学习一组合适的约束来描述任意组合问题。这种架构可以学习拟合解决任务所需的恰当 NP 难问题，这也展示了 MIP 在组合优化领域的重要性。

在处理上述工业应用中的数学建模问题时，决策变量，尤其是整数变量，有时不可避免，这些问题会被建模为 MIP 模型。例如：$x$ 辆汽车、$y$ 个包裹。这里的 $x$、$y$ 都是整数变量，小数没有实际意义。一个 MIP 问题可以同时包含整数变量和连续变量。如果问题的目标函数不含二次项，则称为混合整数线性规划（mixed integer linear programming，MILP）。其形式为：

$$
\begin{aligned}
\min \quad & c^T x \\
\text{s.t.} \quad & Ax \geq b,\quad x\in\mathbb{R}^n,\\
& x_j\in\mathbb{Z},\quad \forall j\in I .
\end{aligned}
\tag{1}
$$

当目标函数中至少有一个二次项时，问题称为混合整数二次规划（mixed integer quadratic program，MIQP）。如果模型的任意约束含有二次项，那么无论目标函数如何，该问题都称为混合整数二次约束规划（mixed integer quadratically constrained program，MIQCP）。本文只关注基础的 MILP；下文中的“MIP”均指 MILP。

> **校注｜线性规划的定义（第 1 页）。** 原文用“目标函数不含二次项”来介绍 MILP，条件并不充分：MILP 要求目标函数与所有约束都是线性的，且部分变量受整数条件约束。没有二次项并不能排除其他非线性项。式（1）才给出了本文实际讨论的线性模型。

求解 MILP 对应的线性规划（LP）松弛，可以得到 MILP 最优值的一个下界。省略整数要求即可得到 LP 松弛：

$$
\begin{aligned}
\min \quad & c^T x \\
\text{s.t.} \quad & Ax \geq b,\quad x\in\mathbb{R}^n .
\end{aligned}
\tag{2}
$$

精确求解 MILP 是 NP 难的，原文将其表述为“可在指数时间内求解”。不过，相比找到可证明的最优解，实践者通常更关心能否尽快得到高质量的解。因此，研究如何在有限时间内获得高质量解，具有重要的实际意义。

> **校注｜复杂性与松弛界（第 1 页）。** NP 难不是“指数时间”的定义，也不意味着每个实例都难。式（1）是最小化模型，所以 LP 松弛给出下界；最大化模型的 LP 松弛则给出上界。

本综述关注基于机器学习的 MIP 求解算法技术。这些基础且已得到广泛研究的技术可以分为两大类：用于精确求解的、基于分支定界的算法，以及用于寻找近似解的启发式算法。我们将在后续章节逐一介绍，并讨论其他有趣的研究方向。在现代 MIP 求解器中，大多数都以分支定界法为基础，而且经常把割平面技术集成到分支定界法中，以提高分支定界算法的效率。这两项技术共同构成了分支切割（branch-and-cut）框架。常见的 MIP 求解器包括 SCIP [Bestuzheva et al., 2021]、CPLEX [Cplex, 2009] 和 GUROBI [Gurobi Optimization, 2020]。由于篇幅限制，本文不介绍商业求解器中求解 MIP 的经典方法。

目前出现了一些新兴开源库，用于促进机器学习与混合整数规划交叉领域的研究。MIPLearn[^1] 提供了一个定制库，用于基于学习的求解器配置，同时支持 GUROBI 和 CPLEX。它可以被看作马尔可夫决策过程（MDP）框架的一种边界情形（可理解为退化情形），而 MDP 正是另一个新兴库 Ecole [Prouvost et al., 2020] 关注的框架。Ecole 旨在与先进的开源 MIP 求解器 SCIP 协同工作，作为对现有求解器的增强。此外，ORGym [Hubbs et al., 2020] 和 OpenGraphGym [Zheng et al., 2020] 是类似 Gym 的库，用于针对包括 MIP 在内的一系列组合优化问题学习启发式策略。

**与已有综述的区别。** 在机器学习求解组合优化 [Bengio et al., 2021] 以及利用学习辅助精确求解混合整数规划 [Kleinert et al., 2021; Lodi and Zarpellon, 2017; Huang et al., 2021a] 方面，已经有一些优秀综述。由于五年前相关研究的发展有限，此前的综述 [Lodi and Zarpellon, 2017] 只涵盖学习辅助的节点选择技术；[Kleinert et al., 2021] 则聚焦双层定制方法，即利用 MIP 技术求解双层优化问题。本文旨在对机器学习求解 MIP 领域进行全面且与时俱进的回顾，尤其是考虑到该领域近年来的快速发展。近期论文 [Huang et al., 2021a] 主要关注数十年来分支定界算法而非整个 MIP 的发展，也没有强调在 MIP 中采用机器学习的重要性。

[^1]: https://anl-ceeesa.github.io/MIPLearn

## 2 分支定界

*原文定位：PDF 第 2–5 页。*

一般而言，混合整数线性规划问题可以用基于线性规划的分支定界（branch-and-bound，B&B）算法求解 [Land and Doig, 2010]。B&B 于约半个世纪前提出 [Land and Doig, 1960]，是一种精确算法，已被证明最终能够达到最优解。本节首先概述一般的 B&B 算法，然后把现有工作分为分支变量选择、节点选择和割平面选择，并详细讨论这些方法与机器学习方法的结合。

### 算法 1：分支定界

以下保留原算法的 1–9 行结构。输入问题为 $(X,f)$。

1. 令 $L=\{X\}$。
2. 当 $L\neq\varnothing$ 时，执行下面的循环体：
3. 　从 $L$ 中选择一个待探索的子问题 $S$。
4. 　如果能找到 $\hat{x}'\in\{x\in S\mid f(x)<f(\hat{x})\}$，则：
5. 　　更新 $\hat{x}=\hat{x}'$。
6. 　如果 $S$ 不能被剪枝，则：
7. 　　将 $S$ 划分为 $S_1,S_2,\ldots,S_r$。
8. 　　将 $S_1,S_2,\ldots,S_r$ 插入 $L$。
9. 　从 $L$ 中移除 $S$。

**输出：** $\hat{x}$。

分支定界是一种经典的树搜索方法。它通过分治法，根据变量的取值进行分支，从而划分搜索空间，并利用松弛问题提供的上下界，剪去搜索树中没有希望的区域。定义优化问题为 $\mathcal{P}=(X,f)$，其中 $X$（称为搜索空间）是所有有效解构成的集合，$f:X\rightarrow\mathbb{R}$ 是目标函数。目标是找到最优解 $x^*\in\arg\min_{x\in X}f(x)$。为了求解 $\mathcal{P}$，需要构建一棵由搜索空间子集组成的搜索树 $T$。此外，算法在全局保存一个可行解 $\hat{x}\in X$，称为**当前最好解**（incumbent solution）。

每次迭代时，算法从未探索子集的列表 $L$ 中选择一个新的子集 $S\subseteq X$ 进行探索。如果能在 $S$ 中找到一个目标值优于 $\hat{x}$ 的解 $\hat{x}'\in S$（称为候选改进解），即 $f(\hat{x}')<f(\hat{x})$，就更新当前最好解。另一方面，如果能够证明 $S$ 中不存在比 $\hat{x}$ 更好的解，即 $\forall x\in S,\ f(x)\geq f(\hat{x})$，则对该子集进行剪枝（fathom，即判定该节点无需继续展开），这个子集随即成为终止节点。否则，将 $S$ 划分为一组穷尽的子集 $S_1,S_2,\ldots,S_r$，这些子集不一定互斥，再把它们插入 $T$。一旦不存在尚未探索的子集，就返回当前最好可行解；因为只有当一个子集不含优于 $\hat{x}$ 的解时才会被排除，所以最终必然有 $\hat{x}\in\arg\min_{x\in X}f(x)$。通用 B&B 过程的伪代码见算法 1，用 B&B 求解 MIP 的示例 [He et al., 2014] 见图 1。

> **校注｜算法 1 的适用前提（第 2 页）。** 这是概念性伪代码，不能直接作为实现。它省略了当前最好解的初始化、松弛求解等细节；尚无可行解时，最小化问题通常把 incumbent 目标值设为正无穷。最优性结论以有效的界、正确的剪枝和搜索最终完成为前提。

![原论文图 1：分支定界搜索树](/figures/paper-guides/survey-figure-1.svg)

**图 1：使用分支定界求解整数线性规划最小化问题。**（图片来源：[He et al., 2014]）

*来源：所提供综述 PDF 第 2 页，原图 1；保留原始图形与数值。可点击图片查看大图。*

图中的示例模型为：

$$
\begin{aligned}
\min\quad & -2x-y\\
\text{s.t.}\quad &3x-5y\leq0,\\
&3x+5y\leq15,\\
&x\geq0,\ y\geq0,\\
&x,y\in\mathbb{Z}.
\end{aligned}
$$

图中术语：训练样例（training examples）、剪枝（prune）、节点展开顺序（node expansion order）、全局下界与上界（global lower and upper bound）、最优节点（optimal node）、已排除节点（fathomed node）。图中灰色框的 $ub$、$lb$ 按原图图例表示搜索过程中的全局上界、下界；节点内部也标出了该子问题的界。$y\leq1/y\geq2$、$x\leq1/x\geq2$、$y\leq2/y\geq3$ 等表示分支时增加的变量取值条件。

原文认为，B&B 难以进行形式化分析的一个重要原因，是其搜索规模可能呈指数增长：求解一个 MILP 可能需要数百万次分支变量选择（branching variable selection，BVS）决策，而一次糟糕的选择就可能让搜索树规模翻倍，却无法推动搜索取得进展。如此复杂且数据丰富的情境，再加上人们通常缺乏对特定问题结构的形式化理解，使 B&B 成为应用机器学习技术的理想场景；机器学习近来也在离散优化领域蓬勃发展。设计有效的分支规则对 MIP 求解器至关重要。

大多数工作聚焦分支定界算法中的决策 [Balcan et al., 2018; Khalil et al., 2016]。其中两项重要决策是分支变量选择和节点选择。近期研究探索了“学习分支”，包括学习分支变量选择 [Khalil et al., 2016; Song et al., 2020b; Liberto et al., 2016; Gasse et al., 2019; Sun et al., 2020; Zarpellon et al., 2020; Etheve et al., 2020]、节点选择 [He et al., 2014; Song et al., 2017; 2020b; Yilmaz et al., 2020]，以及割平面选择 [Huang et al., 2021b; Tang et al., 2020]。特别是，分支变量选择是这些文章的主流方向，因为下一个变量的选择对 B&B 非常重要，会影响 B&B 的总成本。

**表 1：机器学习与 B&B 相结合的方法汇总。**“选择”表示哪个环节涉及学习。

| 方法 | 选择环节 | 学习方式 | 网络 | 表示 | 备注 |
|---|---|---|---|---|---|
| [Gasse et al., 2019] | 变量 | 强化学习（按原表） | GCN | 二分图 | 模仿强分支 |
| [Gupta et al., 2020] | 变量 | 监督学习 | GCN | 二分图 | 通过动态嵌入加速 |
| [Sun et al., 2020] | 变量 | 强化学习 | 原始—对偶策略网络 | 子问题集合 | 使用进化策略训练 |
| [He et al., 2014] | 节点 | 强化学习（按原表） | 独立模型 | 独立特征 | 模仿最优决策参考（oracle） |
| [Yilmaz et al., 2020] | 节点 | 监督学习 | MLP | 人工构造 | 剪除叶节点 |
| [Khalil et al., 2016] | 变量 | 监督学习 | SVM | 人工构造 | 学习排序 |
| [Shen et al., 2021] | 变量 | 监督学习 | GCN | 二分图 | 与深度优先搜索结合 |
| [Huang et al., 2021b] | 割平面 | 监督学习 | MLP | 人工构造 | 大规模 |
| [Zarpellon et al., 2020] | 变量 | 强化学习（按原表） | MLP | 人工构造 | 模仿强分支 |
| [Tang et al., 2020] | 割平面 | 强化学习 | 注意力机制与 LSTM | 人工构造 | 使用进化策略训练 |
| [Nair et al., 2021] | 变量 | 强化学习（按原表） | GCN | 二分图 | 模仿强分支 |
| [Ding et al., 2019] | 变量 | 监督学习 | GCN | 三分图 | 提取连接信息 |
| [Alvarez et al., 2014] | 变量 | 监督学习 | ExtraTrees | 人工构造 | 模仿强分支 |

### 2.1 分支变量选择（BVS）

*原文定位：PDF 第 3–4 页。*

分支变量选择决定选择哪个分数变量（也称候选变量），把当前节点分支为两个子节点。为了表示候选变量的质量，需要为该变量计算一个分数，用它衡量变量的有效性，并选择得分最高的候选变量进行分支。算法 2 给出了 BVS 的伪代码，说明变量选择的基本思想。

### 算法 2：分支变量选择

**输入：** 当前节点的子问题 $S$，及其最优 LP 解 $\hat{x}\notin X_{MILP}$。

1. 定义分支候选集合 $C=\{i\in I\mid\hat{x}_i\notin\mathbb{Z}\}$；
2. 对每个候选 $i\in C$ 执行：
   
   计算其分数 $s_i\in\mathbb{R}$。

**输出：** 一个在当前 LP 解中取非整数值的整数变量的下标；原文写为 $i=\arg\min_{i\in C}s_i$。

> **校注｜评分方向（第 3 页，算法 2）。** 正文说选择得分最高者，伪代码却写成最小化，二者不一致。这里保留原式供核对；实际实现要按评分定义确定使用最大值还是最小值。

BVS 中有多种用于衡量变量的分支规则，包括强分支（strong branching，SB）规则 [Applegate et al., 1995]、伪成本（pseudo-cost）[Bénichou et al., 1971] 和混合分支（hybrid branching）[Achterberg and Berthold, 2009]。于是，采用模仿学习 [Ho and Ermon, 2016] 来学习分支规则的快速近似，这一想法应运而生。模仿学习旨在利用专家经验训练神经网络，使所得网络达到与最初专家相同、甚至更好的性能。[Alvarez et al., 2014; 2017; Khalil et al., 2016; Gasse et al., 2019; Gupta et al., 2020] 通过模仿强分支规则学习分支策略。这类方法采用一种质量高但计算昂贵的启发式方案，是机器学习在该方向最早、也是研究最广泛的方法。

具体而言，[Alvarez et al., 2014] 首次使用监督学习来学习强分支模型。通过观察专家，神经网络可以学习变量分数的近似函数，并进一步利用它在 B&B 中选择合适的变量。[Khalil et al., 2016] 扩展了 [Alvarez et al., 2014] 的工作，设计了一种能够即时求解 MIP 的新流程：在前 500 次分支中，使用传统 SB 做决策，同时记录问题特征以训练神经网络；从第 501 次分支开始，由神经网络接管决策。这两种方法都成功模仿了 SB，但由于每个节点都要计算大量特征，耗时仍然过高。

为了克服复杂特征计算的问题，[Gasse et al., 2019] 将 MIP 编码到图卷积网络（graph convolutional network，GCN）[Kipf and Welling, 2017] 中，通过消息传递高效提取图结构特征。[Nair et al., 2021] 把 MIP 编码为二分图并输入图卷积网络，先计算初始可行解（Neural Diving，神经潜入），再训练 GCN 模仿基于 ADMM 的分支策略（Neural Branching，神经分支）。该方法还使用强化学习学习一种每次取消一个变量赋值的策略，并且每隔 $\eta$ 步求解一次子 MIP，以计算新解。作者把 MIP 自然表述为代表变量—约束关系的二分图，如图 2 所示。每个 MIP 有两组节点，一组表示变量，另一组表示约束；变量 $i$ 与约束 $j$ 之间存在一条边，表示变量 $i$ 出现在约束 $j$ 中。许多后续工作都参考了 [Gasse et al., 2019] 提出的二分图模型。

> **校注｜不要混淆不同求解环节（第 4 页）。** 这段原文把初始解生成、分支决策和取消变量赋值后重新求解子 MIP 连在一起描述。它们分别服务于不同环节；不能把后者直接当作 Neural Branching 的定义。这里的 Neural Diving 指神经网络辅助的潜入式原始启发式，“潜入”不是字面上的潜水。

![原论文图 2：MIP 的变量—约束二分图](/figures/paper-guides/survey-figure-2.svg)

**图 2：将 MIP 实例转化为二分图。** 变量（绿色）对应一侧，约束（红色）对应另一侧。（图片来源：[Nair et al., 2021]）

*来源：所提供综述 PDF 第 3 页，原图 2；保留原始图形与数值。可点击图片查看大图。*

受 GCN 模型应用的启发，[Gupta et al., 2020] 提出一种混合架构：只在 B&B 树的根节点使用 GNN 模型，在其余节点使用较弱但更快的预测器，例如简单的多层感知机（multi-layer perceptron，MLP）。该模型将 [Gasse et al., 2019] 强大的表示框架与 [Khalil et al., 2016] 计算成本更低的框架结合起来，在分支中实现时间—准确率权衡。此外，[Ding et al., 2019] 根据 MIP 表述生成三分图，如图 3 所示。他们利用收集到的特征、标签和三分图训练 GCN，以预测变量的解值。在 [Zarpellon et al., 2020] 中，作者指出，全局分支定界树状态所包含的信息是变量选择的重要因素，并采用了一种新颖的神经网络设计，直接纳入分支定界树的状态。此外，他们的方法是少数能够处理异构实例的技术之一。

![原论文图 3：MIP 的目标—变量—约束三分图](/figures/paper-guides/survey-figure-3.svg)

**图 3：从 MIP 到三分图。** 三类顶点分别表示目标函数、变量和约束。（图片来源：[Ding et al., 2019]）

*来源：所提供综述 PDF 第 4 页，原图 3；保留原始图形与数值。可点击图片查看大图。*

不同于学习 B&B 中某种高计算成本启发式算法的输出，[Etheve et al., 2020] 从零开始将强化学习（reinforcement learning，RL）用于 BVS，不依赖任何启发式算法。强化学习的核心思想是从智能体与环境的交互中学习；在这里，MIP 实例就是环境，智能体尝试求解它。经验表明，强化学习方法有时比前文提到的模仿学习效果更好。在 [Sun et al., 2020] 中，作者认为强分支并不是一个好的模仿专家，于是把变量选择过程建模为马尔可夫决策过程（MDP），进而使用强化学习。他们还基于简化 LP 松弛上的原始—对偶迭代设计策略网络，并利用进化策略更新神经网络。

### 2.2 节点选择

*原文定位：PDF 第 4 页。*

如前所述，分支定界算法递归地把问题的可行集合划分为互不相交的子集，并组织成树结构，其中每个节点表示一个子问题，只搜索该节点对应的子集。节点选择算法的主要步骤见算法 3。

### 算法 3：节点选择

**输入：** 节点列表 $L=\{P\}$ 和上界 $\bar{J}$。

1. 为每个活跃节点赋予分数；
2. 从 $L$ 中弹出节点 $P^i$；
3. 求解 LP 松弛；
4. 得到解 $(x^i,y^i)$ 和下界 $J^i$；
5. 如果 $J^i\geq\bar{J}$，则：
6. 剪除节点 $P^i$；
7. 否则，如果 $y^i\in\mathbb{Z}^p$，则：
8. 改进上界，令 $\bar{J}=J^i$；
9. 否则：
10. 对节点 $P^i$ 进行分支，并将子节点加入列表 $L$。

如果对一个子问题计算得到的界不能排除其子集包含最优解的可能性，就可以根据需要进一步划分（即“分支”）这个子集。否则，如果一个节点可能解的下界大于已知上界，就可以剪除该节点。B&B 的一个关键问题，是如何确定优先考虑哪些节点。有效的节点优先决策策略能把树搜索引向有希望的区域，提高迅速找到良好当前最好解的机会；这个当前最好解又可以用来判断应舍弃还是展开其他节点。因此，学习合适的 B&B 树节点选择策略值得研究。更准确地说，策略是一个从状态到动作的函数，在这里，动作就是选择下一个节点。

节点选择方面的研究少于 BVS。[He et al., 2014] 使用模仿学习训练节点选择策略和节点剪枝策略，以加速 B&B 过程中的树搜索。在上述工作之后，[Yilmaz et al., 2020] 通过模仿学习获得节点选择器。不同之处在于，[Yilmaz et al., 2020] 只学习在当前节点的子节点中应该选择哪一个，而不是学习类似广度优先搜索（BFS）的方法；这有助于更快找到解。

### 2.3 割平面

*原文定位：PDF 第 4–5 页。*

众所周知，去掉整数约束后，每个 MIP 都可以松弛为线性规划（LP），而 LP 有许多传统的高效算法，例如单纯形法 [Dantzig, 1990]、内点法（interior point method，IPM）[Roos et al., 2005] 等。这意味着，我们可以把 MIP 松弛成 LP，并以可接受的成本求解相应的 LP，由此引出割平面算法。如图 4 所示，割平面方法迭代地向松弛 LP 中添加割。割是线性约束，它可以在保留 LP 最优解的同时去掉一部分可行域（原文如此表述，准确含义见下方校注），从而收紧 LP 松弛。

> **校注｜割保留什么（第 5 页）。** 原文的“preserving the LP optimal solution”不准确。有效割应保留原问题的所有整数可行解，同时可以排除当前 LP 松弛的分数最优解，从而收紧松弛。图 4 的绿色区域表示该示意图中整数可行点的凸包；绿色区域中的每一点并不都是整数解。

![原论文图 4：割平面与可行区域](/figures/paper-guides/survey-figure-4.svg)

**图 4：MIP 的割平面。** 图例：LP 可行区域、IP 可行区域、被割除区域（Cut-off Region）。

*来源：所提供综述 PDF 第 5 页，原图 4；保留原始图形与数值。可点击图片查看大图。*

假设把割集合

$$
C'=\{\alpha_i^T x\geq\beta_i\}_{i=1}^{|C'|}
$$

加入式（1）的原始表述，那么 MIP 的优化形式变为：

$$
\begin{aligned}
\min\quad &c^Tx\\
\text{s.t.}\quad &Ax\geq b,\quad x\in\mathbb{R}^n,\\
&\alpha^Tx\geq\beta,\\
&x_j\in\mathbb{Z},\quad \forall j\in I .
\end{aligned}
\tag{3}
$$

如新的表述所示，割的作用是缩小 LP 的解空间，这可能使分支切割算法中的搜索树更小，从而显著减少需要搜索的节点数量。如前所述，割平面可以与分支定界算法结合，构成分支切割框架。分支切割是现代求解器中最常用的算法之一。

由于割平面在 MIP 求解中的重要性，许多研究者尝试利用机器学习技术改进传统割平面算法。与 B&B 不同，割平面算法中相对缺少设计完善的启发式规则，这意味着模仿学习或监督学习无法直接应用。因此，研究者开始考虑强化学习。

> **校注｜适用范围（第 5 页）。** “无法直接应用”是原文对所述研究背景的概括，不是监督学习或模仿学习在割选择中不可用的理论结论。下一段就介绍了基于神经网络的割排序方法。[Tang et al., 2020] 针对顺序选择 MIP 割平面的问题提出 MDP 表述，并使用进化策略训练强化学习智能体。这项工作展示了强化学习改进割平面算法的能力，也可能开辟一个新的研究主题。后续工作 [Huang et al., 2021b] 在多实例学习设定下提出了一种用于割选择的割排序方法，使用神经网络为下一步的不同候选割打分。

## 3 启发式算法

*原文定位：PDF 第 5–6 页。*

虽然 B&B 可以精确求解 MIP，但由于 MIP 的 NP 难特性，这一过程会消耗大量时间和资源。在许多情况下，尤其是大规模问题实例中，B&B 会变得难以处理。因此，人们转而考虑基于启发式的方法。启发式算法通过结合贪心方法与搜索，对 MIP 进行近似求解。常见的 MIP 启发式算法有大邻域搜索和可行性泵。此外，一些工作旨在通过机器学习更好地利用现有 MIP 求解器。我们将详细讨论这些方法。

> **校注｜汇总表的读法。** 两张表均保留原论文分类与备注。“优于 SCIP”等措辞仅转述相应研究中的比较，不表示对任意实例都成立；表 1 的部分学习范式标签与正文存在不一致，见文末说明。

**表 2：机器学习与启发式算法相结合的方法简要汇总。**“关注点”一列表示所结合的启发式算法。LNS 表示大邻域搜索，FP 表示可行性泵。

| 方法 | 关注点 | 学习方式 | 网络 | 表示 | 备注 |
|---|---|---|---|---|---|
| [Song et al., 2020a] | LNS | 强化学习 | MLP | 人工构造 | 与 GUROBI 结合 |
| [Sonnerat et al., 2021] | LNS | 强化学习 | GCN | 二分图 | 使用模仿学习 |
| [Wu et al., 2021] | LNS | 强化学习 | GCN | 二分图 | 优于 SCIP |
| [Liu et al., 2021] | LNS | 监督学习与强化学习 | GNN | 二分图 | 自适应邻域大小 |
| [Qi et al., 2021b] | FP | 强化学习 | MLP 与 CNN | 参数矩阵 | 将 $[A,b]$ 组合为矩阵 |
| [Ding et al., 2020] | 选择 | 监督学习 | GCN | 三分图 | 预测变量的解值 |
| [Grover et al., 2018] | 选择 | 强化学习 | 独立模型 | 人工构造 | 在 CPLEX 中选择启发式算法 |
| [Xavier et al., 2021] | 选择 | 监督学习 | KNN | 人工构造 | 用作热启动 |

### 3.1 大邻域搜索（LNS）

*原文定位：PDF 第 5–6 页。*

大邻域搜索（large neighborhood search，LNS）是一种强大的 MIP 启发式算法。给定问题实例和初始可行解，LNS 在每次迭代中，于当前解预先定义的邻域内搜索更好的候选解。迭代持续进行，直到耗尽搜索预算（例如计算时间）。由于 LNS 的特性，必须避免搜索陷入较差的局部最优。一般来说，邻域规模会随输入问题规模增大而呈指数增长。因此，有必要利用学习技术优化 LNS 算法，以提高效率。

决定 LNS 效果的两个关键选择是：1）初始解；2）每次迭代的搜索邻域。[Song et al., 2020a] 使用模仿学习和强化学习学习邻域选择策略，并用随机邻域选择策略生成模仿学习的训练数据。沿用第 2.1 节所述 [Nair et al., 2021] 的神经潜入思想，[Sonnerat et al., 2021] 对神经潜入进行改造，用它获得初始解，并在每个 LNS 步骤中选择搜索邻域。一些研究者 [Liu et al., 2021] 对 LNS 进行分析，发现邻域大小很重要，而且最合适的邻域大小会随迭代变化。因此，他们提出使用机器学习自动寻找合适的邻域大小。[Wu et al., 2021] 将强化学习与 LNS 结合，其中强化学习的动作是选择需要重新赋值的变量。他们还提出了一种新的 MIP 变量和约束特征提取器。

LNS 旨在持续改进一个解，这也是求解组合优化问题的常见思想。因此，下面讨论组合优化领域中一些与 LNS 思想相近的方法，希望它们能启发我们把相应方法用于求解 MIP。[Chen and Tian, 2019] 提出一种称为局部重写的框架，通过选择解的一部分并加以修改来改进给定解。该研究展示了局部重写框架在车辆路径问题和计算资源分配问题上的有效性。ECO-DQN [Barrett et al., 2020] 框架重新设计了强化学习智能体的动作空间，使智能体可以撤销之前的动作；换言之，在 ECO-DQN 框架中，智能体的动作是可撤销的。组合优化领域还有许多其他工作 [Lu et al., 2020; Fu et al., 2021] 沿用了局部重写和 ECO-DQN 的思想。

### 3.2 可行性泵

*原文定位：PDF 第 6 页。*

可行性泵（feasibility pump，FP）是一种启发式算法，其运行步骤如下：1）求出 MIP 连续松弛（此时退化为 LP）的最优解，并将其取整；2）在松弛可行域中搜索距离最近的解；3）对每一步找到的新解进行扰动和取整，直到解可行。如果达到最大步数限制，算法停止并输出当前可行解。FP 算法的基本步骤见算法 4。

> **校注｜停止不等于成功（第 6 页）。** 原文声称达到步数上限后返回“当前可行解”，但启发式方法可能尚未找到可行解。只有解满足原模型全部约束及整数条件时才能称为可行解；否则应报告未找到可行解。对混合变量取整时，应只对要求为整数的变量施加整数化操作。

### 算法 4：可行性泵

**输入：** $x^0\leftarrow\arg\min c^Tx$；$\bar{x}^0\leftarrow[x^0]$；$i=0$。其中 $[\cdot]$ 表示取整。

1. 当 $\bar{x}^i$ 不可行时，循环执行：
2. $x^{i+1}\leftarrow\arg\min\|x-\bar{x}^i\|$；
3. $\bar{x}^{i+1}\leftarrow[x^{i+1}]$；
4. 如果 $\bar{x}^{i+1}==\bar{x}^i$，则：
5. 对所有 $j\in I$，随机扰动 $\bar{x}_j^i$；
6. 否则：
7. 按原文为 $k\leftarrow k+1$。

**输出：** $\bar{x}^i$。

> **校注｜算法 4 的省略与笔误（第 6 页）。** 两个最小化步骤都应在 LP 松弛可行域上进行；原伪代码省略了该约束。循环索引使用 $i$，递增语句却使用 $k$，并且扰动与索引更新的安排也不完整。因此上面保留的是原文伪代码译写，不应直接运行；不能只替换一个字母就认为得到完整算法。

尽管 FP 是一种相对强大的启发式算法，其效率和成本效益仍不理想。因此，[Qi et al., 2021a; 2021b] 使用强化学习改进 FP。强化学习的切入点是在步骤 2 中寻找下一个非整数解。作者不再选择距离最近的解，而是让强化学习智能体把选择下一个解作为动作。此外，针对状态空间的表示，作者设计了 MLP 和 CNN 两种方法；其中 CNN 把 MIP 的参数 $[A,b]$ 当作图像来处理。原文称图 4 展示了 [Qi et al., 2021a] 提出的 FP 算法与智能 FP，实际对应图 5。

![原论文图 5：传统可行性泵与智能可行性泵](/figures/paper-guides/survey-figure-5.svg)

**图 5：可行性泵。** 图例：取整（Rounding）、投影（Projection）、智能 FP（Smart FP）。（图片来源：[Qi et al., 2021a]）

*来源：所提供综述 PDF 第 6 页，原图 5；保留原始图形与数值。可点击图片查看大图。*

### 3.3 预测与选择

*原文定位：PDF 第 6 页。*

一些工作并不直接求解 MIP，而是致力于在现有 MIP 求解器或方法中进行预测与选择。基于已有工作求解 MIP 是合理的，因为现有 MIP 求解器已经经历了数十年的改进。一些研究者认为，更好地利用现有 MIP 方法是一个有价值的研究主题。

[Ding et al., 2020] 根据历史数据预测每个变量的解值，并决定使用启发式算法还是精确分支方法来求解。作者试图让精确分支方法聚焦困难情况，把容易情况交给启发式算法。[Grover et al., 2018] 通过强化学习，根据给定实例的特征选择 CPLEX 中预定义的启发式算法，这是一个在线学习框架。[Xavier et al., 2021] 关注以数据驱动方式求解 MIP 实例。结合现有 MIP 求解器，他们训练 KNN 来预测冗余约束、良好的初始可行解，以及最优解可能位于其中的仿射子空间，从而显著缩小 MIP 的问题规模。他们在电网机组组合问题上进行了实验，该问题是 MIP 的一种应用。

此外，提前停止技术被广泛应用于超参数优化 [Makarova et al., 2021]，它利用机器学习预测何时停止搜索过程而不造成太大的质量损失，从而大幅提高效率并节省计算资源。提前停止技术也可能被集成到 MIP 求解器中。具体而言，可以使用机器学习技术预测何时提前停止 B&B 过程，同时保证当前的原始可行解仍然可以接受。

## 4 结论与展望

*原文定位：PDF 第 6 页。*

本综述介绍了用于求解 MIP 的先进机器学习方法，旨在总结现有工作，并作为这些领域未来研究的起点。我们发现，机器学习技术与传统运筹学算法的融合是研究领域一个正在兴起的主题，其中包括与精确 B&B 算法以及启发式算法的结合。

由于 MIP 是 NP 难问题，获得精确解非常困难。在实际应用中，利用机器学习技术在有限计算资源内获得可接受的解，是受欢迎且合理的。我们可以设计模型，让学习模型模仿启发式算法，在 B&B 算法中作出某些决策，或者调整初始解。可以看到，随着机器学习，尤其是深度学习和强化学习的发展，越来越多的模型被用于 MIP 求解，不断提高求解效率和解的质量。

## 参考文献

*原文定位：PDF 第 7–8 页。*

> 以下保留作者、英文题名和出版信息，并附中文题名，方便按原文检索。

- **[Achterberg and Berthold, 2009]** T. Achterberg and T. Berthold. *Hybrid branching*（混合分支）. CPAIOR, 2009.
- **[Alvarez et al., 2014]** A. M. Alvarez, Q. Louveaux, and L. Wehenkel. *A supervised machine learning approach to variable branching in branch-and-bound*（一种用于分支定界中变量分支的监督机器学习方法）. ECML, 2014.
- **[Alvarez et al., 2017]** A. M. Alvarez, Q. Louveaux, and L. Wehenkel. *A machine learning-based approximation of strong branching*（基于机器学习的强分支近似）. JOC, 2017.
- **[Applegate et al., 1995]** D. Applegate, R. Bixby, V. Chvátal, and B. Cook. *Finding cuts in the TSP*（寻找旅行商问题中的割）. Technical report, 1995.
- **[Balcan et al., 2018]** M.-F. Balcan, T. Dick, T. Sandholm, and E. Vitercik. *Learning to branch*（学习分支）. ICML, 2018.
- **[Barrett et al., 2020]** T. Barrett, W. Clements, J. Foerster, and A. Lvovsky. *Exploratory combinatorial optimization with reinforcement learning*（使用强化学习进行探索式组合优化）. AAAI, 2020.
- **[Bengio et al., 2021]** Y. Bengio, A. Lodi, and A. Prouvost. *Machine learning for combinatorial optimization: a methodological tour d’horizon*（面向组合优化的机器学习：方法论全景综述）. EJOR, 2021.
- **[Bénichou et al., 1971]** M. Bénichou, J.-M. Gauthier, P. Girodet, G. Hentges, G. Ribière, and O. Vincent. *Experiments in mixed-integer linear programming*（混合整数线性规划实验）. Mathematical Programming, 1971.
- **[Bestuzheva et al., 2021]** K. Bestuzheva et al. *The SCIP Optimization Suite 8.0*（SCIP 优化套件 8.0）. ZIB-Report 21-41, Zuse Institute Berlin, December 2021.
- **[Chen and Tian, 2019]** X. Chen and Y. Tian. *Learning to perform local rewriting for combinatorial optimization*（学习执行面向组合优化的局部重写）. NeurIPS, 2019.
- **[Cplex, 2009]** I. I. Cplex. *V12.1: User’s manual for CPLEX*（V12.1：CPLEX 用户手册）. International Business Machines Corporation, 46(53):157, 2009.
- **[Dantzig, 1990]** G. B. Dantzig. *Origins of the simplex method*（单纯形法的起源）. In *A History of Scientific Computing*, pages 141–151, 1990.
- **[Ding et al., 2019]** J. Ding, C. Zhang, L. Shen, S. Li, B. Wang, Y. Xu, and L. Song. *Optimal solution predictions for mixed integer programs*（混合整数规划的最优解预测）. arXiv:1906.09575, 2019.
- **[Ding et al., 2020]** J.-Y. Ding, C. Zhang, L. Shen, S. Li, B. Wang, Y. Xu, and L. Song. *Accelerating primal solution findings for mixed integer programs based on solution prediction*（基于解预测加速混合整数规划原始解的寻找）. AAAI, 2020.
- **[Etheve et al., 2020]** M. Etheve, Z. Alès, C. Bissuel, O. Juan, and S. Kedad-Sidhoum. *Reinforcement learning for variable selection in a branch and bound algorithm*（用于分支定界算法中变量选择的强化学习）. arXiv:2005.10026, 2020.
- **[Fu et al., 2021]** Z.-H. Fu, K.-B. Qiu, and H. Zha. *Generalize a small pre-trained model to arbitrarily large TSP instances*（将小型预训练模型泛化到任意规模的 TSP 实例）. AAAI, 2021.
- **[Gajda et al., 2022]** M. Gajda, A. Trivella, R. Mansini, and D. Pisinger. *An optimization approach for a complex real-life container loading problem*（一种求解复杂现实集装箱装载问题的优化方法）. Omega, 2022.
- **[Gasse et al., 2019]** M. Gasse, D. Chételat, N. Ferroni, L. Charlin, and A. Lodi. *Exact combinatorial optimization with graph convolutional neural networks*（使用图卷积神经网络进行精确组合优化）. arXiv:1906.01629, 2019.
- **[Grover et al., 2018]** A. Grover et al. *Best arm identification in multi-armed bandits with delayed feedback*（延迟反馈多臂老虎机中的最佳臂识别）. AISTATS, 2018.
- **[Gupta et al., 2020]** P. Gupta, M. Gasse, E. B. Khalil, M. P. Kumar, A. Lodi, and Y. Bengio. *Hybrid models for learning to branch*（用于学习分支的混合模型）. arXiv:2006.15212, 2020.
- **[Gurobi Optimization, 2020]** Gurobi Optimization. *Gurobi Optimizer Reference Manual*（Gurobi 优化器参考手册）. 2020.
- **[He et al., 2014]** H. He, H. Daumé III, and J. M. Eisner. *Learning to search in branch and bound algorithms*（学习在分支定界算法中搜索）. NeurIPS, 2014.
- **[Ho and Ermon, 2016]** J. Ho and S. Ermon. *Generative adversarial imitation learning*（生成对抗模仿学习）. NeurIPS, 2016.
- **[Huang et al., 2021a]** L. Huang, X. Chen, W. Huo, J. Wang, F. Zhang, B. Bai, and L. Shi. *Branch and bound in mixed integer linear programming problems: A survey of techniques and trends*（混合整数线性规划问题中的分支定界：技术与趋势综述）. arXiv:2111.06257, 2021.
- **[Huang et al., 2021b]** Z. Huang, K. Wang, F. Liu, H.-L. Zhen, W. Zhang, M. Yuan, J. Hao, Y. Yu, and J. Wang. *Learning to select cuts for efficient mixed-integer programming*（学习选择割以实现高效混合整数规划）. Pattern Recognition, 2021.
- **[Hubbs et al., 2020]** C. D. Hubbs, H. D. Perez, O. Sarwar, N. V. Sahinidis, I. E. Grossmann, and J. M. Wassick. *OR-Gym: A reinforcement learning library for operations research problems*（OR-Gym：面向运筹学问题的强化学习库）. arXiv:2008.06319, 2020.
- **[Keha et al., 2009]** A. B. Keha, K. Khowala, and J. W. Fowler. *Mixed integer programming formulations for single machine scheduling problems*（单机调度问题的混合整数规划表述）. Computers & Industrial Engineering, 2009.
- **[Khalil et al., 2016]** E. B. Khalil, P. Le Bodic, L. Song, G. Nemhauser, and B. Dilkina. *Learning to branch in mixed integer programming*（混合整数规划中的学习分支）. AAAI, 2016.
- **[Kipf and Welling, 2017]** T. Kipf and M. Welling. *Semi-supervised classification with graph convolutional networks*（使用图卷积网络进行半监督分类）. arXiv:1609.02907, 2017.
- **[Kleinert et al., 2021]** T. Kleinert, M. Labbé, I. Ljubić, and M. Schmidt. *A survey on mixed-integer programming techniques in bilevel optimization*（双层优化中混合整数规划技术综述）. EURO Journal on Computational Optimization, 2021.
- **[Land and Doig, 1960]** A. H. Land and A. G. Doig. *An automatic method of solving discrete programming problems*（一种求解离散规划问题的自动方法）. Econometrica, 28:497, 1960.
- **[Land and Doig, 2010]** A. H. Land and A. G. Doig. *An automatic method for solving discrete programming problems*（一种求解离散规划问题的自动方法）. In *50 Years of Integer Programming 1958–2008: From the Early Years to the State-of-the-Art*, 2010.
- **[Liberto et al., 2016]** G. Di Liberto, S. Kadioglu, K. Leo, and Y. Malitsky. *DASH: Dynamic approach for switching heuristics*（DASH：动态切换启发式算法的方法）. EJOR, 2016.
- **[Liu et al., 2021]** D. Liu, M. Fischetti, and A. Lodi. *Learning to search in local branching*（学习在局部分支中搜索）. arXiv, 2021.
- **[Lodi and Zarpellon, 2017]** A. Lodi and G. Zarpellon. *On learning and branching: a survey*（学习与分支综述）. TOP, 2017.
- **[Lu et al., 2020]** H. Lu, X. Zhang, and S. Yang. *A learning-based iterative method for solving vehicle routing problems*（一种基于学习的车辆路径问题迭代求解方法）. ICLR, 2020.
- **[Makarova et al., 2021]** A. Makarova, H. Shen, V. Perrone, A. Klein, J. B. Faddoul, A. Krause, M. Seeger, and C. Archambeau. *Overfitting in Bayesian optimization: an empirical study and early-stopping solution*（贝叶斯优化中的过拟合：实证研究与提前停止方案）. arXiv:2104.08166, 2021.
- **[Malandraki and Daskin, 1992]** C. Malandraki and M. S. Daskin. *Time dependent vehicle routing problems: Formulations, properties and heuristic algorithms*（时间依赖的车辆路径问题：表述、性质与启发式算法）. Transportation Science, 1992.
- **[Nair et al., 2021]** V. Nair et al. *Solving mixed integer programs using neural networks*（使用神经网络求解混合整数规划）. arXiv:2012.13349, 2021.
- **[Paulus et al., 2021]** A. Paulus, M. Rolínek, V. Musil, B. Amos, and G. Martius. *CombOptNet: Fit the right NP-hard problem by learning integer programming constraints*（CombOptNet：通过学习整数规划约束拟合正确的 NP 难问题）. ICML, 2021.
- **[Pochet and Wolsey, 2010]** Y. Pochet and L. A. Wolsey. *Production Planning by Mixed Integer Programming*（基于混合整数规划的生产计划）. Springer, 2010.
- **[Prouvost et al., 2020]** A. Prouvost, J. Dumouchelle, L. Scavuzzo, M. Gasse, D. Chételat, and A. Lodi. *Ecole: A gym-like library for machine learning in combinatorial optimization solvers*（Ecole：面向组合优化求解器机器学习的类 Gym 库）. arXiv:2011.06069, 2020.
- **[Qi et al., 2021a]** M. Qi, M. Wang, and Z.-J. Shen. *Smart feasibility pump: Reinforcement learning for (mixed) integer programming*（智能可行性泵：面向（混合）整数规划的强化学习）. arXiv:2102.09663, 2021.
- **[Qi et al., 2021b]** M. Qi, M. Wang, Zuo-jun, and M. Shen. *Reinforcement learning for (mixed) integer programming: Smart feasibility pump*（面向（混合）整数规划的强化学习：智能可行性泵）. RL4RealLife Workshop of ICML, 2021.
- **[Roos et al., 2005]** C. Roos, T. Terlaky, and J.-P. Vial. *Interior point methods for linear optimization*（线性优化的内点法）. 2005.
- **[Sawik, 2011]** T. Sawik. *Scheduling in supply chains using mixed integer programming*（使用混合整数规划进行供应链调度）. John Wiley & Sons, 2011.
- **[Schouwenaars et al., 2001]** T. Schouwenaars, B. De Moor, E. Feron, and J. How. *Mixed integer programming for multi-vehicle path planning*（用于多载具路径规划的混合整数规划）. ECC, 2001.
- **[Shen et al., 2021]** Y. Shen, Y. Sun, A. Eberhard, and X. Li. *Learning primal heuristics for mixed integer programs*（学习混合整数规划的原始启发式算法）. IJCNN, 2021.
- **[Song et al., 2017]** J. Song, R. Lanka, A. Zhao, Y. Yue, and M. Ono. *Learning to search via self-imitation with application to risk-aware planning*（通过自我模仿学习搜索及其在风险感知规划中的应用）. NeurIPS Workshop, 2017.
- **[Song et al., 2020a]** J. Song, R. Lanka, Y. Yue, and B. Dilkina. *A general large neighborhood search framework for solving integer linear programs*（一种求解整数线性规划的通用大邻域搜索框架）. arXiv:2004.00422, 2020.
- **[Song et al., 2020b]** J. Song, R. Lanka, Y. Yue, and M. Ono. *Co-training for policy learning*（用于策略学习的协同训练）. Uncertainty in Artificial Intelligence, pages 1191–1201, PMLR, 2020.
- **[Sonnerat et al., 2021]** N. Sonnerat, P. Wang, I. Ktena, S. Bartunov, and V. Nair. *Learning a large neighborhood search algorithm for mixed integer programs*（学习面向混合整数规划的大邻域搜索算法）. arXiv:2107.10201, 2021.
- **[Sun et al., 2020]** H. Sun, W. Chen, H. Li, and L. Song. *Improving learning to branch via reinforcement learning*（通过强化学习改进学习分支）. LMCA, 2020.
- **[Tang et al., 2020]** Y. Tang, S. Agrawal, and Y. Faenza. *Reinforcement learning for integer programming: Learning to cut*（面向整数规划的强化学习：学习切割）. ICML, 2020.
- **[Wu et al., 2013]** T. Wu, K. Akartunalı, J. Song, and L. Shi. *Mixed integer programming in production planning with backlogging and setup carryover: Modeling and algorithms*（含缺货积压与设置延续的生产计划混合整数规划：建模与算法）. DEDS, 2013.
- **[Wu et al., 2021]** Y. Wu, W. Song, Z. Cao, and J. Zhang. *Learning large neighborhood search policy for integer programming*（学习整数规划的大邻域搜索策略）. NeurIPS, 2021.
- **[Xavier et al., 2021]** Á. S. Xavier, F. Qiu, and S. Ahmed. *Learning to solve large-scale security-constrained unit commitment problems*（学习求解大规模安全约束机组组合问题）. INFORMS Journal on Computing, 33(2):739–756, 2021.
- **[Yilmaz et al., 2020]** K. Yilmaz, N. Yorke-Smith, and xxx. *Learning efficient search approximation in mixed integer branch and bound*（学习混合整数分支定界中的高效搜索近似）. arXiv:2007.03948, 2020.
- **[Zarpellon et al., 2020]** G. Zarpellon, J. Jo, A. Lodi, and Y. Bengio. *Parameterizing branch-and-bound search trees to learn branching policies*（参数化分支定界搜索树以学习分支策略）. arXiv:2002.05120, 2020.
- **[Zheng et al., 2020]** W. Zheng, D. Wang, and F. Song. *OpenGraphGym: A parallel reinforcement learning framework for graph optimization problems*（OpenGraphGym：面向图优化问题的并行强化学习框架）. International Conference on Computational Science, 2020.

## 校订说明与原文疑点

本次对照所提供 PDF 的正文与图表，补齐 5 张原图、各节页码和相邻校注，调整直译生硬的术语，恢复算法 1 的行号结构。正文原式（1）—（3）、原表分类与作者—年份引文保留；本版不是对每篇被引论文的独立复核，也不把原文的经验表述提升为普遍保证。

1. 引言把“NP 难”直接解释为“可在指数时间内求解”，这不是严格定义。NP 难表示至少与 NP 中最难的问题一样难，并不等同于“所有 NP 难问题都已被证明只能在指数时间内求解”。
2. 第 2.1 节正文说“选择得分最高的候选变量”，算法 2 的输出却写成 $\arg\min_{i\in C}s_i$。二者方向相反，应以具体分数定义为准。
3. 表 1 将若干正文明确描述为模仿学习的方法标为“Reinforcement”。译文保留原表，并用“按原表”提示，不应仅凭该表判断论文的学习范式。
4. 算法 4 的循环变量是 $i$，第 7 步却写成 $k\leftarrow k+1$，疑似应为 $i\leftarrow i+1$。
5. 第 3.2 节正文称“图 4”展示可行性泵，实际图 4 是割平面，图 5 才是可行性泵。
6. 参考文献 [Yilmaz et al., 2020] 的第三作者在原 PDF 中印为“xxx”；[Qi et al., 2021b] 的作者信息也疑似不完整。译文未擅自补全。

<!-- folio:notes v1
Notes on this document, written with Folio (a Markdown reader for VS Code). Renderers hide this comment.
One JSON object per line. "quote" is the exact text a note is about ("prefix", "suffix" and "line" help find it); "text" is the note.
To answer a note, add {"author": "<your name>", "text": "<answer>"} to its "replies". To close it, set "status": "resolved".
Keep each note on one line, leave "id", "quote", "prefix" and "suffix" as they are, and never write "--" followed by ">" in this block.
{"id":"mvn89aes-gfxzt","author":"crane","quote":"此外，[Paulus et al., 2021] 成功地将整数规划求解器集成到神经网络架构中，把它作为一个可微分层，使其能够同时学习成本项和约束。由此得到的端到端可训练架构，既能从原始数据中提取特征，又能学习一组合适的约束来描述任意组合问题。","text":"Paul的工作是使用神经网络从原始数据中生成MIP问题，以便求解器进行求解。以此引入本文论述的MIP重要性。","line":25,"prefix":" 2001] 和装箱 [Gajda et al., 2022]。","suffix":"这种架构可以学习拟合解决任务所需的恰当 NP 难问题，这也展示了","created":"2026-10-25T02:47:34.922Z","updated":"2026-10-25T02:47:34.922Z"}
{"id":"mvn8b4id-8urcx","author":"crane","quote":"求解 MILP 对应的线性规划（LP）松弛，可以得到 MILP 最优值的一个下界。","text":"此处是求解最小解，故LP松弛之后得到的是下界；如果是求最大解，松弛之后得到的就是上界。\n假设我们在搜索过程中，发现某个分支的 LP 松弛最优值最多只有 2.5，而我们已经找到一个值为 3 的整数可行解。\n\n那么那个分支根本不可能产生更好的整数解，可以直接舍弃！\n\n这就叫做 剪枝（Pruning），也是分支定界法的基础。","line":40,"prefix":"","suffix":"省略整数要求即可得到 LP 松弛：","created":"2026-10-25T02:49:00.567Z","updated":"2026-10-25T11:59:12.055Z"}
{"id":"mvn8gbzx-izix0","author":"crane","quote":"常见的 MIP 求解器包括 SCIP [Bestuzheva et al., 2021]、CPLEX [Cplex, 2009] 和 GUROBI [Gurobi Optimization, 2020]。","text":"SCIP：开源、适合学术研究，内部机制较开放。\n与综述提到的 Ecole 配合，做机器学习求解器研究时很重要\n\nCPLEX：IBM 的商业求解器。\n知道它是主流商业软件即可\n\nGUROBI / Gurobi：广泛使用的商业求解器，通常提供学术许可。\n调用方便、性能强，很多论文用它做实验基线","line":52,"prefix":"两项技术共同构成了分支切割（branch-and-cut）框架。","suffix":"由于篇幅限制，本文不介绍商业求解器中求解 MIP 的经典方法。","created":"2026-10-25T02:53:03.551Z","updated":"2026-10-25T02:53:29.284Z"}
{"id":"mvn8pen2-e5lg5","author":"crane","quote":"MIPLearn","text":"用历史数据帮助配置 Gurobi、CPLEX，例如提供初始解或求解参数。\n\n研究“学习型求解器配置、热启动”再去学","line":54,"prefix":"了一些新兴开源库，用于促进机器学习与混合整数规划交叉领域的研究。","suffix":"[1] 提供了一个定制库，用于基于学习的求解器配置，同时支持 G","created":"2026-10-25T03:00:06.880Z","updated":"2026-10-25T03:00:06.880Z"}
{"id":"mvn8qrii-bjtfp","author":"crane","quote":"Ecole","text":"把 SCIP 的求解过程包装成机器学习可以交互的训练环境\n\n复现学习分支、节点选择等论文时最值得学","line":54,"prefix":"过程（MDP）框架的一个边界情形，而 MDP 正是另一个新兴库 ","suffix":" [Prouvost et al., 2020] 关注的框架。E","created":"2026-10-25T03:01:10.219Z","updated":"2026-10-25T03:01:10.219Z"}
{"id":"mvn8rcxw-6kdbg","author":"crane","quote":"ORGym","text":"面向多种运筹优化问题的强化学习实验环境\n\n研究范围扩展到一般组合优化时","line":54,"prefix":"IP 求解器 SCIP 协同工作，作为对现有求解器的增强。此外，","suffix":" [Hubbs et al., 2020] 和 OpenGrap","created":"2026-10-25T03:01:37.990Z","updated":"2026-10-25T03:01:37.990Z"}
{"id":"mvn8rw13-g8be8","author":"crane","quote":"OpenGraphGym","text":"面向图优化问题的强化学习实验框架\n\n研究图上的组合优化时","line":54,"prefix":"此外，ORGym [Hubbs et al., 2020] 和 ","suffix":" [Zheng et al., 2020] 是类似 Gym 的库","created":"2026-10-25T03:02:02.729Z","updated":"2026-10-25T03:02:02.729Z"}
{"id":"mvn8yx5g-i99ms","author":"crane","quote":"马尔可夫决策过程（MDP）框架","text":"在当前状态信息足够完整的前提下，下一步只取决于当前状态和当前动作，不需要知道过去全部历史。","line":54,"prefix":"求解器配置，同时支持 GUROBI 和 CPLEX。它可以被看作","suffix":"的一个边界情形，而 MDP 正是另一个新兴库 Ecole [Pr","created":"2026-10-25T03:07:30.774Z","updated":"2026-10-25T03:07:30.774Z"}
{"id":"mvnapal7-me6xx","author":"crane","quote":"图 1：使用分支定界求解整数线性规划最小化问题","text":"先进行LP松弛，只考虑两个有关x，y的不等式，得到下界解x=5/2 y=3/2，然后在此基础上进行分支定界。","line":85,"prefix":"","suffix":"。（图片来源：[He et al., 2014]）","created":"2026-10-25T03:56:00.862Z","updated":"2026-10-25T03:56:00.862Z"}
{"id":"mvngdwgm-3jw6z","author":"crane","quote":"算法 2：分支变量选择","text":"这里只得到下一步分支需要挑选哪个变量进行分支，但下两个节点如何定界见算法3","line":127,"prefix":"","suffix":"","created":"2026-10-25T06:35:07.032Z","updated":"2026-10-25T06:35:54.495Z"}
{"id":"mvngfmf4-17uxv","author":"crane","quote":"强分支（strong branching，SB）规则 [Applegate et al., 1995]、伪成本（pseudo-cost）[Bénichou et al., 1971] 和混合分支（hybrid branching）[Achterberg and Berthold, 2009]","text":"算法2中计算分数的一些方法","line":138,"prefix":"BVS 中有多种用于衡量变量的分支规则，包括","suffix":"。于是，采用模仿学习 [Ho and Ermon, 2016] ","created":"2026-10-25T06:36:27.330Z","updated":"2026-10-25T06:36:27.330Z"}
{"id":"mvnib0f9-la1nb","author":"crane","quote":"二分图","text":"节点分成两类，边只连接不同类型的节点，同类节点之间不直接连边。\n\n这里两类节点就是变量和约束","line":142,"prefix":"特征。[Nair et al., 2021] 把 MIP 编码为","suffix":"并输入图卷积网络，先计算初始可行解（Neural Diving，","created":"2026-10-25T07:28:51.430Z","updated":"2026-10-25T07:28:51.430Z"}
{"id":"mvniifoc-796ir","author":"crane","quote":"ADMM","text":"这里ADMM作为GCN的分支老师","line":142,"prefix":"eural Diving，神经潜水），再训练 GCN 模仿基于 ","suffix":" 的分支策略（Neural Branching，神经分支）。该方","created":"2026-10-25T07:34:37.789Z","updated":"2026-10-25T07:34:37.789Z"}
{"id":"mvnio67t-8m8b0","author":"crane","quote":"该方法还使用强化学习学习一种每次取消一个变量赋值的策略，并且每隔 η 步求解一次子 MIP，以计算新解。","text":"用于改进现有解的大邻域搜索，与此处分支变量选择关系不大","line":142,"prefix":"MM 的分支策略（Neural Branching，神经分支）。","suffix":"作者把 MIP 自然表述为代表变量—约束关系的二分图，如图 2 ","created":"2026-10-25T07:39:05.467Z","updated":"2026-10-25T07:55:56.929Z"}
{"id":"mvnjtpua-nuhav","author":"crane","quote":"初始可行解（Neural Diving，神经潜水）","text":"先用神经网络猜一个尽可能好的答案，猜出来如果是可行解就至少确定了一个上界，这会帮助之后BB进行剪枝","line":142,"prefix":", 2021] 把 MIP 编码为二分图并输入图卷积网络，先计算","suffix":"，再训练 GCN 模仿基于 ADMM 的分支策略（Neural ","created":"2026-10-25T08:11:23.795Z","updated":"2026-10-25T08:13:03.559Z"}
{"id":"mvnk00rm-33blx","author":"crane","quote":"Neural Branching，神经分支）","text":"帮助BB更快地决定分支变量，加速BB搜索到全局最优解","line":142,"prefix":"g，神经潜水），再训练 GCN 模仿基于 ADMM 的分支策略（","suffix":"。该方法还使用强化学习学习一种每次取消一个变量赋值的策略，并且每","created":"2026-10-25T08:16:17.892Z","updated":"2026-10-25T08:16:46.195Z"}
-->
