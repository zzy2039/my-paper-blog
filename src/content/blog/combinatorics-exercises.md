---
title: "组合数学习题课：从逻辑推理到计数与证明"
description: "45 页清晰重排资料的网页版：真值表、鸽巢原理、Nim、排列组合、线性扩张与样卷证明。保留原有解答、来源页码和资料缺口，支持原页对照与下载。"
pubDate: 2026-10-25
---

> 根据提供的《组合数学习题课·清晰重排版完整包》整理。这里呈现资料中已有的题目与解答，不补造缺失题干，也不把未完成的推导写成完整证明。

<div class="study-resource-bar"><a href="/downloads/combinatorics-notes.pdf" target="_blank">阅读 45 页 PDF ↗</a><a href="/downloads/combinatorics-complete.zip" download>下载完整资料包 ↓</a><span>网页正文 · 公式排版 · 原页对照</span></div>

## 阅读方式与资料范围

按左侧目录选择题目；可以先阅读“题目”，再核对“原有解答”。每节末尾可展开对应的高清原页。包含示意图的原页单独注明，保留原资料的棋盘、连线和标注。

原包说明：前 33 页为习题讲义，后 12 页为后半段批注课件。页内标注的“讲义提取版页码”“课件页码”“视频时间”属于原始来源；本页的“合订页”对应下载 PDF 的页码，两者不要混用。

<strong>已知缺口</strong>：第 29 题缺少五段完整论证题干；样卷第 6 题解法二缺部分过渡及结尾；非攻击型车问题缺原棋盘；后半段“作业一”仅有章节标题。所有说明按原资料保留。



# 一、习题讲义：逻辑、计数与样卷


## 01 · 第 2 次作业 · 第 13 题

<strong>组合数学 · 习题课</strong>  
来源：讲义提取版第 3–4 页；视频 01:15、01:29 · 合订 PDF 第 1 页

#### 题目

用真值表证明下列蕴含式均为重言式（T 表示真，F 表示假）。

#### 原有解答

（i）$[\neg p\land(p\lor q)]\to q$。

<div class="center">

| $p$ | $q$ | $\neg p$ | $p\lor q$ | $\neg p\land(p\lor q)$ | 原式 |
|:-----:|:-----:|:----------:|:-----------:|:------------------------:|:----:|
|   T   |   T   |     F      |      T      |            F             |  T   |
|   T   |   F   |     F      |      T      |            F             |  T   |
|   F   |   T   |     T      |      T      |            T             |  T   |
|   F   |   F   |     T      |      F      |            F             |  T   |

</div>

（ii）$[(p\to q)\land(q\to r)]\to(p\to r)$。

<div class="center">

| $p$ | $q$ | $r$ | $p\to q$ | $q\to r$ | $(p\to q)\land(q\to r)$ | $p\to r$ | 原式 |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| T | T | T | T | T | T | T | T |
| T | T | F | T | F | F | F | T |
| T | F | T | F | T | F | T | T |
| T | F | F | F | T | F | F | T |
| F | T | T | T | T | T | T | T |
| F | T | F | T | F | F | T | T |
| F | F | T | T | T | T | T | T |
| F | F | F | T | T | T | T | T |

</div>


<details class="source-page"><summary>查看第 1 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=1" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-01.webp" width="1400" height="1029" alt="组合数学习题课合订第 1 页原稿" loading="lazy" decoding="async" />

</details>


## 02 · 真值表与重言式

<strong>第 2 次作业 · 第 13 题（续）</strong>  
来源：讲义提取版第 3–4 页；视频 01:15、01:29 · 合订 PDF 第 2 页

（iii）$[p\land(p\to q)]\to q$。

<div class="center">

| $p$ | $q$ | $p\to q$ | $p\land(p\to q)$ | 原式 |
|:-----:|:-----:|:----------:|:------------------:|:----:|
|   T   |   T   |     T      |         T          |  T   |
|   T   |   F   |     F      |         F          |  T   |
|   F   |   T   |     T      |         F          |  T   |
|   F   |   F   |     T      |         F          |  T   |

</div>

（iv）$[(p\lor q)\land(p\to r)\land(q\to r)]\to r$。

<div class="center">

| $p$ | $q$ | $r$ | $p\lor q$ | $p\to r$ | $q\to r$ | $H$ | 原式 |
|:-----:|:-----:|:-----:|:-----------:|:----------:|:----------:|:-----:|:----:|
|   T   |   T   |   T   |      T      |     T      |     T      |   T   |  T   |
|   T   |   T   |   F   |      T      |     F      |     F      |   F   |  T   |
|   T   |   F   |   T   |      T      |     T      |     T      |   T   |  T   |
|   T   |   F   |   F   |      T      |     F      |     T      |   F   |  T   |
|   F   |   T   |   T   |      T      |     T      |     T      |   T   |  T   |
|   F   |   T   |   F   |      T      |     T      |     F      |   F   |  T   |
|   F   |   F   |   T   |      F      |     T      |     T      |   F   |  T   |
|   F   |   F   |   F   |      F      |     T      |     T      |   F   |  T   |

</div>

其中 $H=(p\lor q)\land(p\to r)\land(q\to r)$。四个原式列均为 T，故均为重言式。


<details class="source-page"><summary>查看第 2 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=2" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-02.webp" width="1400" height="1037" alt="组合数学习题课合订第 2 页原稿" loading="lazy" decoding="async" />

</details>


## 03 · 第 23 题 · 量化命题的英文表达

<strong>第 2 次作业</strong>  
来源：讲义提取版第 5 页；视频 01:33 · 合订 PDF 第 3 页

#### 题目

论域为所有学生，$P(x)$ 表示“学生 $x$ 在每个工作日的上课时间都超过 5 小时”。将下列量化命题表述为英文。

#### 原有解答

<div class="center">

| 小问 | 命题 | 英文表述 |
|:--:|:---|:---|
| \(i\) | $\exists xP(x)$ | At least one student spends more than five hours in class every weekday. |
| \(ii\) | $\forall xP(x)$ | Every student spends more than five hours in class every weekday. |
| \(iii\) | $\exists x\neg P(x)$ | There is a student who spends at most five hours in class on at least one weekday. |
| \(iv\) | $\forall x\neg P(x)$ | Every student has at least one weekday on which they spend at most five hours in class. |

</div>

> 资料说明：保留原讲义英文答案及其量词范围；没有将 “at most” 改为 “less than”。


<details class="source-page"><summary>查看第 3 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=3" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-03.webp" width="1400" height="818" alt="组合数学习题课合订第 3 页原稿" loading="lazy" decoding="async" />

</details>


## 04 · 第 29 题 · 推理规则

<strong>第 2 次作业</strong>  
来源：讲义提取版第 5–6 页；视频 01:33、01:35 · 合订 PDF 第 4 页

#### 题目

指出各论证所用的推理规则。

#### 原有解答

<div class="center">

|  小问   | 规则名称   | 英文名称               |
|:-------:|:-----------|:-----------------------|
|  \(i\)  | 附加律     | Addition               |
| \(ii\)  | 化简律     | Simplification         |
| \(iii\) | 肯定前件式 | Modus ponens           |
| \(iv\)  | 否定后件式 | Modus tollens          |
|  \(v\)  | 假言三段论 | Hypothetical syllogism |

</div>

> 来源缺口／核对说明：视频中的讲义仅给出各小问对应的规则名称，未显示原题的五段论证。本页保留可见内容，未补写论证题干。


<details class="source-page"><summary>查看第 4 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=4" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-04.webp" width="1400" height="893" alt="组合数学习题课合订第 4 页原稿" loading="lazy" decoding="async" />

</details>


## 05 · 第 61 题 · 骨牌覆盖与断层线

<strong>第 4 次作业</strong>  
来源：讲义提取版第 7–8 页；视频 01:53、05:00 · 合订 PDF 第 5 页

#### 题目

用 18 块 $1\times2$ 骨牌完全覆盖 $6\times6$ 棋盘。证明：必有一条水平或垂直的内部格线，可以把棋盘切成非空的两部分而不切断任何骨牌（即存在断层线）。

#### 原有解答

一条格线若穿过骨牌内部，就会把它切断；若全程沿骨牌边界通过，就是所求的断层线。

<div class="center">

</div>

> 资料说明：细线为方格线，粗线为骨牌边界。示意图按原画面重绘；两图采用同一种铺法。一般证明见下一页。


<details class="source-page" open><summary>查看图示与第 5 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=5" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-05.webp" width="1400" height="868" alt="组合数学习题课合订第 5 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 06 · 跨线骨牌的奇偶性

<strong>第 4 次作业 · 第 61 题（续）</strong>  
来源：讲义提取版第 8 页；视频 05:00 · 合订 PDF 第 6 页

#### 原有解答

<strong>先看一条格线：跨线骨牌数为什么是偶数？</strong> 取一条水平内部格线，设其上方有 $k$ 行，共 $6k$ 个方格。上方的方格只有两种覆盖方式：完全位于上方的骨牌，每块贡献 2 个方格；跨过这条线的骨牌，每块在上方贡献 1 个方格。 若这两类骨牌分别有 $a$ 块、$b$ 块，按上方的方格数计数，有

$$
6k=2a+b,\qquad b=6k-2a.
$$

因此 $b$ 必为偶数。若这条线不是断层线，就有 $b>0$，于是至少跨过 2 块骨牌。对垂直格线，改为数左侧的方格，同理成立。

<strong>再看全部格线：用骨牌总数得到矛盾。</strong> 棋盘共有 5 条水平、5 条垂直内部格线。假设它们都不是断层线，每条至少跨过 2 块骨牌，总共至少有

$$
(5+5)\times2=20
$$

次“格线穿过骨牌”的情况。但是，一块 $1\times2$ 骨牌只跨过连接它的两个方格中间的那条格线，其余格线至多沿骨牌边界通过。故每块骨牌只贡献 1 次，总共只有 18 次，与至少 20 次矛盾。


$$
\text{至少有一条内部格线不穿过任何骨牌，即必有断层线。}
$$



<details class="source-page"><summary>查看第 6 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=6" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-06.webp" width="1400" height="851" alt="组合数学习题课合订第 6 页原稿" loading="lazy" decoding="async" />

</details>


## 07 · 第 66 题 · Nim 的平衡局面

<strong>第 4 次作业</strong>  
来源：讲义提取版第 9–10 页；视频 05:30、08:30 · 合订 PDF 第 7 页

#### 题目

五堆 Nim 的硬币数为 10、20、30、40、50。判断是否平衡，并给出先手的第一步。

#### 原有解答

普通 Nim 由两人轮流操作，每次只从一堆取走至少 1 枚，允许取完一堆；取走最后一枚者获胜。把各堆写成二进制，若每一位上 1 的个数均为偶数，就称为平衡。 平衡局面走一步必变为不平衡；不平衡局面总有一步能变为平衡。因此，制胜策略是每次把平衡局面留给对方，全空也是平衡局面。

<div class="center">

| 堆大小／位权 |  32 |  16 |   8 |   4 |   2 |   1 |
|:------------:|----:|----:|----:|----:|----:|----:|
|      10      |   0 |   0 |   1 |   0 |   1 |   0 |
|      20      |   0 |   1 |   0 |   1 |   0 |   0 |
|      30      |   0 |   1 |   1 |   1 |   1 |   0 |
|      40      |   1 |   0 |   1 |   0 |   0 |   0 |
|      50      |   1 |   1 |   0 |   0 |   1 |   0 |
|   1 的个数   |   2 |   3 |   3 |   2 |   3 |   0 |

</div>

位权为 16、8、2 的三位上，1 的个数为奇数，所以当前局面不平衡。在 50 的二进制表示中翻转这三位，$110010$ 变为 $101000$，即 40。


$$
\text{先手从 50 枚的一堆取走 10 枚。}
$$


新局面为 $10,20,30,40,40$，各位上 1 的个数为 $2,2,4,2,2,0$，均为偶数，局面平衡。


<details class="source-page"><summary>查看第 7 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=7" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-07.webp" width="1400" height="1048" alt="组合数学习题课合订第 7 页原稿" loading="lazy" decoding="async" />

</details>


## 08 · 第 76 题 · 连续若干天的棋局数

<strong>第 4 次作业</strong>  
来源：讲义提取版第 9–10 页；视频 05:30、08:30 · 合订 PDF 第 8 页

#### 题目

棋手连续准备 11 周，每天至少下 1 局，每个日历周至多下 12 局。证明：对每个 $k=1,2,\ldots,21$，都存在连续若干天恰好下 $k$ 局。对于 22 局，能否保证同样的结论？

#### 原有解答

鸽巢原理：把 $m+1$ 个物体放入 $m$ 个盒子，必有两个物体落入同一盒。本题让“数的个数”多于“可能的取值数”。 设 $s_i$ 为前 $i$ 天累计局数，令 $s_0=0$。第 $i+1$ 天至第 $j$ 天的局数为 $s_j-s_i$，要找的关系是 $s_j=s_i+k$。 每天至少 1 局，11 周共 77 天，总局数至多 $11\times12=132$，故

$$
0=s_0<s_1<\cdots<s_{77}\le132.
$$

对任意 $1\le k\le22$，把两组数放在一起：

$$
s_0,s_1,\ldots,s_{77};\qquad s_0+k,s_1+k,\ldots,s_{77}+k.
$$

它们共有 $78+78=156$ 个，却只能取 $0$ 到 $132+k$ 之间的整数，至多有 $133+k\le155$ 个取值，故必有两个数相等。 每组内部严格递增，重复只能发生在两组之间，故有 $s_j=s_i+k$，且 $j>i$。对应连续若干天恰好下 $k$ 局，故题设结论成立，<span style="color: teal">22 局也能保证</span>。

> 资料说明：保留 $s_0=0$，既能表示从第 1 天开始的区间，也让两组各有 78 个数；漏掉它，$k=22$ 时上述计数不足以保证重复。


<details class="source-page"><summary>查看第 8 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=8" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-08.webp" width="1400" height="872" alt="组合数学习题课合订第 8 页原稿" loading="lazy" decoding="async" />

</details>


## 09 · 第 126 题 · 桥牌发牌

<strong>第 6 次作业</strong>  
来源：讲义提取版第 11 页；视频 12:30 · 合订 PDF 第 9 页

#### 题目

将一副 52 张的扑克牌发给 4 名桥牌玩家，每人 13 张。有多少种不同的发牌结果？

#### 原有解答

每人只关心拿到哪些牌，手牌内部不计顺序，因此用组合数选牌；依次确定四人的手牌，再用乘法原理相乘。 第一人从 52 张中选 13 张，第二人从剩余 39 张中选 13 张，第三人从剩余 26 张中选 13 张，最后 13 张全部给第四人。因此共有


$$
\binom{52}{13}\binom{39}{13}\binom{26}{13}\binom{13}{13}=\frac{52!}{(13!)^4}.
$$


四名玩家可区分，交换两人的手牌会得到不同的发牌结果，所以无需再除以 $4!$。组合数已经忽略各人手牌内部的顺序。


<details class="source-page"><summary>查看第 9 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=9" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-09.webp" width="1400" height="690" alt="组合数学习题课合订第 9 页原稿" loading="lazy" decoding="async" />

</details>


## 10 · 第 131 题 · 格点路线与被淹路段

<strong>第 6 次作业</strong>  
来源：讲义提取版第 11–12 页；视频 12:30、14:30 · 合订 PDF 第 10 页

#### 题目

工作地点在家以东 9 个街区、以北 8 个街区，每天走 17 个街区上班。（i）有多少条路线？（ii）若从家以东 4、以北 3 个街区处向东的一段道路被淹，有多少条可行路线？

#### 原有解答

（i）至少需向东 9 步、向北 8 步，共 17 步；限定恰好 17 步，所以不能向西或向南走。每条路线对应一个含 9 个“东”和 8 个“北”的排列，路线数为 $\binom{17}{8}=24310$。

（ii）被淹的是 $A=(4,3)$ 到 $B=(5,3)$ 的一条边。经过它的路线唯一分为三段：

<div class="center">

| 路段                 | 所需步数                | 路线数       |
|:---------------------|:------------------------|:-------------|
| $S=(0,0)$ 到 $A$ | 4 步东、3 步北，共 7 步 | $\binom73$ |
| $A$ 到 $B$       | 固定走被淹的一条边      | 1            |
| $B$ 到 $T=(9,8)$ | 4 步东、5 步北，共 9 步 | $\binom94$ |

</div>

前段任选一路线，均可与后段任意路线连接；反之，每条经过被淹边的路线也只对应这种分解。因此经过它的路线有 $\binom73\times1\times\binom94=35\times126=4410$ 条。


$$
\text{可行路线数}=24310-4410=19900.
$$



<details class="source-page"><summary>查看第 10 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=10" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-10.webp" width="1400" height="940" alt="组合数学习题课合订第 10 页原稿" loading="lazy" decoding="async" />

</details>


## 11 · 坐标与被淹道路

<strong>第 6 次作业 · 第 131 题（图示）</strong>  
来源：讲义提取版第 11–12 页；视频 12:30、14:30 · 合订 PDF 第 11 页

<div class="center">

</div>

> 资料说明：按原讲义坐标网格重绘。保留 A 到 B 的具体方向及位置；路线计数见前页。


<details class="source-page" open><summary>查看图示与第 11 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=11" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-11.webp" width="1400" height="812" alt="组合数学习题课合订第 11 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 12 · 第 141 题 · 有下界的整数解

<strong>第 6 次作业</strong>  
来源：讲义提取版第 13–14 页；视频 15:30、16:30 · 合订 PDF 第 12 页

#### 题目

求方程 $x_1+x_2+x_3+x_4=30$ 的整数解个数，其中 $x_1\ge2$、$x_2\ge0$、$x_3\ge-5$、$x_4\ge8$。

#### 原有解答

先换元消去下界，再用隔板法。令

$$
y_1=x_1-2,\quad y_2=x_2,\quad y_3=x_3+5,\quad y_4=x_4-8.
$$

$x_3$ 的下界是 $-5$，所以应减去 $-5$。四个新变量均为非负整数，且

$$
y_1+y_2+y_3+y_4=30-2+5-8=25.
$$

反过来，由 $x_1=y_1+2$、$x_2=y_2$、$x_3=y_3-5$、$x_4=y_4+8$ 能唯一还原原解，因此两者解数相同。 将 25 个相同对象排成一行，用 3 块隔板分成 4 组。允许变量为 0，所以隔板可以相邻，也可位于两端。例如四组数量为 $2,0,5,18$，第二组为空。 对象与隔板合计 $25+3=28$ 个位置，选出 3 个位置放隔板，其余位置全部放对象，四组数量就唯一确定。


$$
\binom{25+4-1}{4-1}=\binom{28}{3}=3276.
$$



<details class="source-page"><summary>查看第 12 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=12" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-12.webp" width="1400" height="857" alt="组合数学习题课合订第 12 页原稿" loading="lazy" decoding="async" />

</details>


## 13 · 第 208 题 · 子集偏序的线性扩张

<strong>第 8 次作业</strong>  
来源：讲义提取版第 15–16 页；视频 17:00、18:30 · 合订 PDF 第 13 页

#### 题目

集合 $\{a,b,c\}$ 的全部 8 个子集按包含关系构成偏序。这个偏序有多少个线性扩张？

#### 原有解答

线性扩张就是把这些子集排成一列，同时保留原来的先后限制：若 $A$ 是 $B$ 的真子集，$A$ 就必须排在 $B$ 前面；原本不可比较的两个集合，不必按大小先后排列。

<div class="center">

</div>

空集必在首位，全集必在末位，只需排列中间 6 个集合。一个二元素集合必须等自己的两个单元素子集都出现后，才能放入序列。

> 资料说明：图按原讲义重绘：连线下方的集合必须先出现；没有圆点的交叉处不是节点。分类计数见下一页。


<details class="source-page" open><summary>查看图示与第 13 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=13" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-13.webp" width="1400" height="903" alt="组合数学习题课合订第 13 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 14 · 两类情况合计

<strong>第 8 次作业 · 第 208 题（续）</strong>  
来源：讲义提取版第 15–16 页；视频 17:00、18:30 · 合订 PDF 第 14 页

#### 原有解答

<strong>情况一：三个单元素集合都先出现。</strong> 三个单元素集合任意排序，有 $3!$ 种。它们全部出现后，三个二元素集合的限制都已满足，也可任意排序，有 $3!$ 种。因此本类有

$$
3!\times3!=36
$$

种线性扩张。

<strong>情况二：某个二元素集合先于第三个单元素集合出现。</strong> 前面必须先放两个不同单元素集合，按顺序选它们有 $3\times2$ 种。假设先放的是 $\{a\}$、$\{b\}$，接下来能放的二元素集合只有 $\{a,b\}$。 此后必须放 $\{c\}$，因为另外两个二元素集合 $\{a,c\}$、$\{b,c\}$ 都要等 $\{c\}$ 出现。最后这两个集合任意排序，有 $2!$ 种。例如中间六项可以是

$$
\{a\},\quad\{b\},\quad\{a,b\},\quad\{c\},\quad\{a,c\},\quad\{b,c\}.
$$

本类有 $3\times2\times2!=12$ 种。两类互不重叠，且覆盖所有可能情况，故


$$
\text{线性扩张总数}=36+12=48.
$$



<details class="source-page"><summary>查看第 14 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=14" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-14.webp" width="1400" height="815" alt="组合数学习题课合订第 14 页原稿" loading="lazy" decoding="async" />

</details>


## 15 · 第 216 题 · Pascal 三角

<strong>第 8 次作业</strong>  
来源：讲义提取版第 17–18 页；视频 19:00、20:00 · 合订 PDF 第 15 页

#### 题目

填写 Pascal 三角中对应 $n=9$ 和 $n=10$ 的两行。

#### 原有解答

第 $n$ 行依次是 $\binom n0,\binom n1,\ldots,\binom nn$。两端都是 1，中间每项等于上一行相邻两项之和，即

$$
\binom nk=\binom{n-1}{k-1}+\binom{n-1}{k}.
$$


<div class="center">

| $n\backslash k$ |   0 |   1 |   2 |   3 |   4 |   5 |   6 |   7 |   8 |   9 |  10 |
|:-----------------:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|----:|
|         9         |   1 |   9 |  36 |  84 | 126 | 126 |  84 |  36 |   9 |   1 |     |
|        10         |   1 |  10 |  45 | 120 | 210 | 252 | 210 | 120 |  45 |  10 |   1 |

</div>

例如 $n=10$ 的中间项为


$$
\binom{10}{5}=126+126=252.
$$


> 资料说明：保留原讲义的两行数据；空白格对应 $k>n$，没有填为 0。


<details class="source-page"><summary>查看第 15 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=15" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-15.webp" width="1400" height="955" alt="组合数学习题课合订第 15 页原稿" loading="lazy" decoding="async" />

</details>


## 16 · 第 229 题 · 带权交错组合和

<strong>第 8 次作业</strong>  
来源：讲义提取版第 17–18 页；视频 19:00、20:00 · 合订 PDF 第 16 页

#### 题目

证明：对每个整数 $n>1$，都有

$$
\binom n1-2\binom n2+3\binom n3+\cdots+(-1)^{n-1}n\binom nn=0.
$$


#### 原有解答

原式带有额外系数 $k$，先把 $k$ 并入组合数，再用二项式定理。先回顾恒等式

$$
k\binom nk=n\binom{n-1}{k-1}.
$$

两边都在计算“从 $n$ 人中选 $k$ 人，并指定一名队长”的方法数：左边先选队伍再选队长；右边先定队长，再从其余 $n-1$ 人中选 $k-1$ 人。 设原式为 $S$，令 $j=k-1$，则

$$
\begin{aligned}
S&=\sum_{k=1}^n(-1)^{k-1}k\binom nk
=n\sum_{k=1}^n(-1)^{k-1}\binom{n-1}{k-1}\\
&=n\sum_{j=0}^{n-1}(-1)^j\binom{n-1}{j}.
\end{aligned}
$$

由 $(1+x)^{n-1}=\sum_{j=0}^{n-1}\binom{n-1}j x^j$，令 $x=-1$，得 $S=n(1-1)^{n-1}=0$。这里 $n>1$ 保证 $n-1\ge1$，原式得证。


<details class="source-page"><summary>查看第 16 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=16" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-16.webp" width="1400" height="945" alt="组合数学习题课合订第 16 页原稿" loading="lazy" decoding="async" />

</details>


## 17 · 第 1.1 题 · 生日推理

<strong>期中样卷</strong>  
来源：讲义提取版第 19–20 页；视频 20:30、22:30 · 合订 PDF 第 17 页

#### 题目

A、B 询问 C 的生日。C 给出以下 10 个可能日期，只告诉 A 月份，只告诉 B 日子：

<div class="center">

| 月份 | 可能日期      | 月份 | 可能日期      |
|:-----|:--------------|:-----|:--------------|
| 5 月 | 15、16、19 日 | 7 月 | 14、16 日     |
| 6 月 | 17、18 日     | 8 月 | 14、15、17 日 |

</div>

A 说：“我不知道，但我知道 B 也不知道。”B 说：“一开始我不知道，但现在知道了。”A 又说：“现在我也知道了。”求 C 的生日。 选项：（a）5 月 16 日；（b）6 月 17 日；（c）7 月 16 日；（d）8 月 15 日。

#### 原有解答

<strong>第一轮：</strong>最初只有 18 日、19 日各出现一次；B 听到其中一个日子便能立即确定。若 A 得知 5 月或 6 月，就不能保证 B 不知道。因此排除整个 5 月、6 月，只剩

$$
7/14,\quad7/16,\quad8/14,\quad8/15,\quad8/17.
$$

<strong>第二轮：</strong>14 日仍有两个候选，B 听到 14 日便仍无法确定。排除 14 日，剩下 $7/16,8/15,8/17$。 <strong>第三轮：</strong>7 月只剩一个候选，8 月仍有两个。A 能确定，说明月份是 7 月，所以生日是<span style="color: teal">7 月 16 日，选（c）</span>。


<details class="source-page"><summary>查看第 17 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=17" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-17.webp" width="1400" height="883" alt="组合数学习题课合订第 17 页原稿" loading="lazy" decoding="async" />

</details>


## 18 · 第 1.5 题 · 足球分配

<strong>期中样卷</strong>  
来源：讲义提取版第 19–20 页；视频 20:30、22:30 · 合订 PDF 第 18 页

#### 题目

将 15 个相同的足球分给 4 个人，每人至少 3 个，有多少种分法？ 选项：（a）10；（b）20；（c）28；（d）36。

#### 原有解答

先给每人 3 个，共分掉 12 个，还剩 3 个任意分配。若第 $i$ 人最终得到 $x_i$ 个，令 $y_i=x_i-3$，则

$$
y_1+y_2+y_3+y_4=3,\qquad y_i\ge0.
$$

用 3 块隔板把 3 个相同对象分成四组，允许空组。对象与隔板共有 6 个位置，选出 3 个位置放隔板即可：


$$
\binom{3+4-1}{4-1}=\binom63=20.
$$


因此选（b）。四个人可区分，交换两人的所得数量一般是不同分法。


<details class="source-page"><summary>查看第 18 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=18" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-18.webp" width="1400" height="739" alt="组合数学习题课合订第 18 页原稿" loading="lazy" decoding="async" />

</details>


## 19 · 第 1.6 题 · 城市访问的容斥计数

<strong>期中样卷</strong>  
来源：讲义提取版第 21–22 页；视频 23:00、24:30 · 合订 PDF 第 19 页

#### 题目

访问 3 个城市，每个城市恰好访问 2 次，且不连续访问同一城市，有多少种方案？选项：（a）30；（b）36；（c）72；（d）90。

#### 原有解答

记三个城市为 A、B、C，路线为 A,A,B,B,C,C 的排列。先不加相邻限制，安排数为 $6!/(2!)^3=90$。除以三个 $2!$，因为同一城市的两次访问不可区分。 若两次 A 相邻，把 AA 捆成一个整体。类似地，可同时捆绑两个或三个城市：

<div class="center">

| 指定相邻的城市         | 捆绑后的对象（示例） | 每种指定的方案数 |
|:-----------------------|:---------------------|:-----------------|
| 指定 1 个，有 3 种选法 | AA、B、B、C、C       | $5!/(2!)^2=30$ |
| 指定 2 个，有 3 种选法 | AA、BB、C、C         | $4!/2!=12$     |
| 指定 3 个，只有 1 种   | AA、BB、CC           | $3!=6$         |

</div>

先减去三个“某城市相邻”的坏情况之和；同时有两种相邻的路线被多减一次，要加回；三个城市都相邻的路线再减一次。这就是容斥原理。


$$
90-3\times30+3\times12-6=30.
$$


故选（a）。


<details class="source-page"><summary>查看第 19 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=19" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-19.webp" width="1400" height="1011" alt="组合数学习题课合订第 19 页原稿" loading="lazy" decoding="async" />

</details>


## 20 · 第 2 题 · 三堆 Nim

<strong>期中样卷</strong>  
来源：讲义提取版第 21–22 页；视频 23:00、24:30 · 合订 PDF 第 20 页

#### 题目

普通 Nim 游戏有三堆硬币，分别为 8、12、15 枚。两人轮流从一堆中取至少 1 枚，取走最后一枚者获胜。谁有必胜策略？给出第一步。

#### 原有解答

将各堆写成二进制，每一位上 1 的个数均为偶数，就称为平衡。平衡局面走一步必变成不平衡；不平衡局面总能走到平衡。全空也是平衡局面，因此从不平衡局面出发，每次把平衡留给对方，便能保证最后取完。

<div class="center">

| 堆大小／位权 |   8 |   4 |   2 |   1 |
|:------------:|----:|----:|----:|----:|
|      8       |   1 |   0 |   0 |   0 |
|      12      |   1 |   1 |   0 |   0 |
|      15      |   1 |   1 |   1 |   1 |
|   1 的个数   |   3 |   2 |   1 |   1 |

</div>

位权 8、2、1 上的个数为奇数，故当前不平衡，先手可保证获胜。把 15 的二进制 $1111$ 中这三位翻转，得到 $0100$，即 4。


$$
\text{第一步从 15 枚的一堆取走 11 枚，留下 }8,12,4.
$$


此时各位上 1 的个数为 $2,2,0,0$，均为偶数；以后继续把平衡局面留给对方。


<details class="source-page"><summary>查看第 20 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=20" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-20.webp" width="1400" height="1048" alt="组合数学习题课合订第 20 页原稿" loading="lazy" decoding="async" />

</details>


## 21 · 第 3 题 · 因子偏序的线性扩张

<strong>期中样卷</strong>  
来源：讲义提取版第 23–24 页；视频 25:00、25:30 · 合订 PDF 第 21 页

#### 题目

在 $\{1,2,3,4,5,6\}$ 上，用“是……的因子”定义偏序，求这个偏序的所有线性扩张。

#### 原有解答

若一个数是另一个不同数的因子，便必须排在它前面。1 是其余所有数的因子，必须排第一。5 与 2、3、4、6 都不可比较，只需排在 1 后，可留到最后插入。 剩下 2、3、4、6 的限制为：2 在 4、6 前，3 在 6 前。先出现的只能是 2 或 3：先放 2，后面只有 346、364、436 三种；先放 3，下一项必须是 2，最后 4、6 任意排序。 核心顺序恰为 2346、2364、2436、3246、3264 五种。每个核心前加 1，再把 5 插入 1 后面的五个空隙。例如 $1,\square,2,\square,3,\square,4,\square,6,\square$。

<div class="center">

| 核心 | 插第 1 处 | 插第 2 处 | 插第 3 处 | 插第 4 处 | 插第 5 处 |
|:----:|:---------:|:---------:|:---------:|:---------:|:---------:|
| 2346 |  152346   |  125346   |  123546   |  123456   |  123465   |
| 2364 |  152364   |  125364   |  123564   |  123654   |  123645   |
| 2436 |  152436   |  125436   |  124536   |  124356   |  124365   |
| 3246 |  153246   |  135246   |  132546   |  132456   |  132465   |
| 3264 |  153264   |  135264   |  132564   |  132654   |  132645   |

</div>

每个结果中 5 的位置唯一；删掉 1 和 5，又唯一得到核心顺序，故上表不重不漏，共 $5\times5=25$ 个。

> 资料说明：六位串表示从左到右的顺序，例如 123456 表示 1、2、3、4、5、6 依次出现。原图覆盖关系已用正文逐条保留。


<details class="source-page"><summary>查看第 21 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=21" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-21.webp" width="1400" height="963" alt="组合数学习题课合订第 21 页原稿" loading="lazy" decoding="async" />

</details>


## 22 · 因子偏序的 Hasse 图

<strong>期中样卷 · 第 3 题（图示）</strong>  
来源：讲义提取版第 23–24 页；视频 25:00、25:30 · 合订 PDF 第 22 页

<div class="center">

</div>

只画覆盖关系：$1\prec2$、$1\prec3$、$1\prec5$、$2\prec4$、$2\prec6$、$3\prec6$。连线下端必须先于上端。

> 资料说明：按原图重绘，教学时的圈画不改变节点与边。所有线性扩张已在前页的 25 项表中列出。


<details class="source-page" open><summary>查看图示与第 22 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=22" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-22.webp" width="1400" height="729" alt="组合数学习题课合订第 22 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 23 · 无序正整数解 · 分类依据

<strong>期中样卷 · 第 6 题</strong>  
来源：讲义提取版第 25 页；视频 26:30 · 合订 PDF 第 23 页

#### 题目

正整数 $x_1,x_2,x_3$ 满足 $x_1+x_2+x_3=1701$，且 $1701$ 整除 $x_1x_2x_3$。求解集合 $\{x_1,x_2,x_3\}$ 的个数。

#### 原有解答

<strong>解法一。</strong>三个数不计顺序，但允许相等。先利用整除条件分类、换元，再数各类正整数解；计数时要避免交换三个数造成重复。 分解 $1701=3^5\times7=243\times7$。三个数的和被 3 整除，因此其中两数被 3 整除时，第三数也被 3 整除。又因乘积被 3 整除，至少有一数被 3 整除。分成三类：

<div class="center">

| 类别   | 条件                                     |
|:-------|:-----------------------------------------|
| 第一类 | 三个数不全被 3 整除：恰有一数被 3 整除。 |
| 第二类 | 三个数全被 3 整除，但不全被 9 整除。     |
| 第三类 | 三个数全被 9 整除。                      |

</div>

三类互不重叠，且包含所有可能情况。以下分别用 $N_1,N_2,N_3$ 表示三类解数。

> 资料说明：本题较长，按原讲义的分类分段排版。下页起保留原有计数、公式与结果。


<details class="source-page"><summary>查看第 23 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=23" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-23.webp" width="1400" height="893" alt="组合数学习题课合订第 23 页原稿" loading="lazy" decoding="async" />

</details>


## 24 · 第一类：不全被 3 整除

<strong>期中样卷 · 第 6 题 · 解法一</strong>  
来源：讲义提取版第 25 页；视频 26:30 · 合订 PDF 第 24 页

#### 原有解答

其余两数不能提供因子 3，所以唯一被 3 整除的数必须被 $3^5=243$ 整除，写为 $243a$。由另外两数为正数，有 $1\le a\le6$，所以 $243a$ 不被 7 整除。 乘积还需因子 7，故其余两数至少一数被 7 整除；又不能两数都被 7 整除，否则由和被 7 整除，$243a$ 也会被 7 整除。因此可唯一写成

$$
\{243a,7b,c\},\qquad243a+7b+c=1701,
$$

其中 $a,b,c$ 为正整数，且 $b$ 不是 3 的倍数。这三项分别是“唯一的 3 倍数”“唯一的 7 倍数”和剩余一项，无需再乘或除排列数。 令 $k=7-a$，则 $1\le k\le6$，且 $7b+c=243k$。由 $c\ge1$，$b$ 的上界为 $\lfloor(243k-1)/7\rfloor$；这里 $243k$ 不被 7 整除，故也等于 $\lfloor243k/7\rfloor$。 扣掉 $b$ 中 3 的倍数，固定 $k$ 的解数为

$$
F(k)=\left\lfloor\frac{243k}{7}\right\rfloor-\left\lfloor\frac{243k}{21}\right\rfloor
=\left\lfloor\frac{243k}{7}\right\rfloor-\left\lfloor\frac{81k}{7}\right\rfloor.
$$

每选定一个 $b$，$c$ 就唯一确定，并且也不是 3 的倍数。代入 $k=1,2,\ldots,6$，得


$$
N_1=23+46+70+92+116+139=486.
$$



<details class="source-page"><summary>查看第 24 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=24" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-24.webp" width="1400" height="828" alt="组合数学习题课合订第 24 页原稿" loading="lazy" decoding="async" />

</details>


## 25 · 第二类：全被 3 整除，不全被 9 整除

<strong>期中样卷 · 第 6 题 · 解法一</strong>  
来源：讲义提取版第 27–28 页；视频 31:00、35:00 · 合订 PDF 第 25 页

#### 原有解答

先令 $x_i=3y_i$，则 $y_1+y_2+y_3=567$，且 $63\mid y_1y_2y_3$。 三个 $y_i$ 不全被 3 整除，其中恰有一个被 3 整除；乘积所需因子 9 必须全由它提供。因此原来三个数中，唯一一数被 27 整除，其余两数被 3 整除但不被 9 整除。 特殊项记为 $27a$，其余为 $3b,3c$，得到

$$
9a+b+c=567,\qquad1\le a\le62,
$$

其中 $b,c$ 均不是 3 的倍数。按 $a$ 是否被 7 整除分支。 <strong>（1）$a$ 不是 7 的倍数。</strong>其余两项恰有一项为 7 的倍数，唯一写成 $\{27a,21b,3c\}$，满足

$$
9a+7b+c=567,\qquad3\nmid b.
$$

令 $k=63-a$，则 $1\le k\le62$，且 $7\nmid k$。由 $7b+c=9k$、$c\ge1$，得 $1\le b\le\lfloor9k/7\rfloor$。扣掉 $b$ 中的 3 倍数，得

$$
G(k)=\left\lfloor\frac{9k}{7}\right\rfloor-\left\lfloor\frac{3k}{7}\right\rfloor.
$$


> 资料说明：端点可写为 $\lfloor9k/7\rfloor$，因为 $9k$ 不被 7 整除。配对求和与另一个分支见下一页。


<details class="source-page"><summary>查看第 25 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=25" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-25.webp" width="1400" height="815" alt="组合数学习题课合订第 25 页原稿" loading="lazy" decoding="async" />

</details>


## 26 · 第二类计数：配对与去重

<strong>期中样卷 · 第 6 题 · 解法一</strong>  
来源：讲义提取版第 27–28 页；视频 31:00、35:00 · 合订 PDF 第 26 页

#### 原有解答

共有 $62-8=54$ 个允许的 $k$。把 $k$ 与 $63-k$ 配对，两数都在允许范围内且互不相同。对应分数的和分别为 81 与 27，且都非整数，因此

$$
\left\lfloor\frac{9k}{7}\right\rfloor+\left\lfloor\frac{9(63-k)}7\right\rfloor=80,\quad
\left\lfloor\frac{3k}{7}\right\rfloor+\left\lfloor\frac{3(63-k)}7\right\rfloor=26.
$$

每对贡献 $G(k)+G(63-k)=80-26=54$，共 27 对，本分支有 $27\times54=1458$ 组；三项仍有唯一身份，不存在交换重复。 <strong>（2）$a$ 是 7 的倍数。</strong>令 $a=7t$，则 $1\le t\le8$。因子 7 已满足，剩下 $b+c=63(9-t)$，$b,c$ 均不是 3 的倍数。 令 $k=9-t$，则 $1\le k\le8$。为避免交换重复，规定 $b\le c$，即 $1\le b\le\lfloor63k/2\rfloor$。扣掉其中 3 的倍数，解数为

$$
\left\lfloor\frac{63k}{2}\right\rfloor-\left\lfloor\frac{63k}{6}\right\rfloor
=\left\lfloor\frac{63k}{2}\right\rfloor-\left\lfloor\frac{21k}{2}\right\rfloor=21k.
$$

只要 $b$ 不是 3 的倍数，由 $b+c=63k$ 可知 $c$ 也不是。本分支有 $21(1+\cdots+8)=756$ 组。


$$
N_2=1458+756=2214.
$$



<details class="source-page"><summary>查看第 26 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=26" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-26.webp" width="1400" height="811" alt="组合数学习题课合订第 26 页原稿" loading="lazy" decoding="async" />

</details>


## 27 · 第三类：三个数全被 9 整除

<strong>期中样卷 · 第 6 题 · 解法一</strong>  
来源：讲义提取版第 29–30 页；视频 35:30、39:45 · 合订 PDF 第 27 页

#### 原有解答

令 $x_i=9y_i$，则 $y_1+y_2+y_3=189$。此时 $x_1x_2x_3=9^3y_1y_2y_3$ 已含因子 $3^6$，满足 $3^5$ 的要求，只需 $7\mid y_1y_2y_3$。 三个 $y_i$ 的和被 7 整除，若两数被 7 整除，第三数也被 7 整除。因此有两种情况。 <strong>（1）恰有一个 $y_i$ 被 7 整除。</strong>唯一记为 $7a$，另两数记为 $b,c$，其中 $7\nmid b,c$。由 $7a+b+c=189$，令 $k=27-a$，得

$$
b+c=7k,\qquad1\le k\le26.
$$

规定 $b\le c$，则 $1\le b\le\lfloor7k/2\rfloor$。扣掉其中 7 的倍数，固定 $k$ 的解数为

$$
\left\lfloor\frac{7k}{2}\right\rfloor-\left\lfloor\frac{k}{2}\right\rfloor=3k.
$$

由于和为 7 的倍数，$b$ 不是 7 倍数时，$c$ 也不是。本分支有


$$
3(1+\cdots+26)=3\times\frac{26\times27}{2}=1053.
$$


特殊项 $7a$ 的身份唯一，另两项已按大小排序，不会重复。


<details class="source-page"><summary>查看第 27 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=27" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-27.webp" width="1400" height="861" alt="组合数学习题课合订第 27 页原稿" loading="lazy" decoding="async" />

</details>


## 28 · 第三类的无序计数与总结果

<strong>期中样卷 · 第 6 题 · 解法一</strong>  
来源：讲义提取版第 29–30 页；视频 35:30、39:45 · 合订 PDF 第 28 页

#### 原有解答

<strong>（2）三个 $y_i$ 全被 7 整除。</strong>再提取公因子 7，只需数

$$
u+v+w=27,\qquad u,v,w\ge1
$$

的无序解。隔板法给出有序解数 $\binom{26}{2}=325$。转换成无序解时，必须区分三个数是否相等：

<div class="center">

| 相等情况 | 无序解的计数 | 每组排列数 |
|:---|:---|:--:|
| 三个相同 | 只有 $\{9,9,9\}$，共 1 组。 | 1 |
| 恰有两个相同 | $\{t,t,27-2t\}$；$t=1,\ldots,13$，去掉 $t=9$，共 12 组。 | 3 |
| 三个互异 | 扣掉前两类的有序解，再除以 6。 | 6 |

</div>

本分支的无序解数为

$$
\frac{325-3\times12-1}{6}+12+1=48+12+1=61.
$$

不能直接用 $325/6$，因为有重复数的解不足 6 种排列。故第三类合计 $N_3=1053+61=1114$。


$$
N_1+N_2+N_3=486+2214+1114=3814.
$$


> 资料说明：此处结果、分类和计算均按原讲义录入；没有另行求解或改换原题计数口径。


<details class="source-page"><summary>查看第 28 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=28" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-28.webp" width="1400" height="1030" alt="组合数学习题课合订第 28 页原稿" loading="lazy" decoding="async" />

</details>


# 二、样卷第 6 题：解法二可见原文


## 29 · 共同质因子分类：可见原文

<strong>期中样卷 · 第 6 题 · 解法二（样卷原文）</strong>  
来源：讲义提取版第 31–32 页；视频 40:00、40:29 · 合订 PDF 第 29 页

［解］由题，将 1701 分解成质数的乘积，即

$$
1701=3^5\cdot7.\tag{6.1}
$$

考虑 $x_1x_2x_3$ 能被 $3^5\cdot7$ 整除的条件，以 3 和 7 作为共同质数因子对正整数解集进行分类。 <strong>i. 无共同因子，此时可能的解为</strong>

$$
A_0:\quad3^5y_1+7y_2+y_3=3^5\cdot7,\quad3\nmid y_2.\tag{6.2}
$$

求得

$$
\begin{aligned}
|A_0|&=\sum_{y_1=1}^{7-1}\left(\left\lfloor\frac{3^5y_1}{7}\right\rfloor-\left\lfloor\frac{3^5y_1}{7\cdot3}\right\rfloor\right)\\
&=\sum_{y_1=1}^{6}\left(34y_1+\left\lfloor\frac{5y_1}{7}\right\rfloor-11y_1-\left\lfloor\frac{4y_1}{7}\right\rfloor\right)\\
&=\sum_{y_1=1}^{6}\left(23y_1+\left\lfloor\frac{5y_1}{7}\right\rfloor-\left\lfloor\frac{4y_1}{7}\right\rfloor\right)\\
&=23\cdot\frac{6\cdot7}{2}+(0+1+2+2+3+4)-(0+1+1+2+2+3)=486.\tag{6.3}
\end{aligned}
$$


> 资料说明：按画面保留原符号和求和变量；本解法的后续仅部分显示，以下不补写画面外的推导。


<details class="source-page"><summary>查看第 29 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=29" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-29.webp" width="1400" height="1000" alt="组合数学习题课合订第 29 页原稿" loading="lazy" decoding="async" />

</details>


## 30 · 3 是、7 不是共同因子

<strong>期中样卷 · 第 6 题 · 解法二（续）</strong>  
来源：讲义提取版第 31 页；视频 40:00 · 合订 PDF 第 30 页

<strong>ii. 3 是、7 不是共同因子，可能的解为</strong>

$$
\begin{aligned}
A_3:\;&3(3^2y_1+7y_2+y_3)=3^5\cdot7,&&7\nmid y_1,\ 3\nmid y_2;\\
&3(3^2\cdot7y_1+y_2+y_3)=3^5\cdot7,&&7\nmid y_2,\ 3\nmid y_2;\\[2pt]
A_{3^2}:\;&3^2(7y_1+y_2+y_3)=3^5\cdot7,&&3\nmid y_1,\ 7\nmid y_2;\\
&3^2(7\cdot3y_1+y_2+y_3)=3^5\cdot7,&&7\nmid y_2,\ 3\nmid y_2;\\[2pt]
A_{3^3}:\;&3^3(7y_1+y_2+y_3)=3^5\cdot7,&&3\nmid y_1,\ 7\nmid y_2;\\
&3^3(7\cdot3y_1+y_2+y_3)=3^5\cdot7,&&7\nmid y_2,\ 3\nmid y_2;\\[2pt]
A_{3^4}:\;&3^4(7y_1+y_2+y_3)=3^5\cdot7,&&7\nmid y_2.\tag{6.4}
\end{aligned}
$$


> 资料说明：分类名及条件按原画面逐项录入。该页存在多行排列紧密的限制条件；需进一步校勘时可按来源时间回看原图。


<details class="source-page"><summary>查看第 30 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=30" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-30.webp" width="1400" height="742" alt="组合数学习题课合订第 30 页原稿" loading="lazy" decoding="async" />

</details>


## 31 · 原文中的 |A₃| 求和式

<strong>期中样卷 · 第 6 题 · 解法二（续）</strong>  
来源：讲义提取版第 31、33 页；视频 40:00、40:31 · 合订 PDF 第 31 页


$$
\begin{aligned}
|A_3|={}&\sum_{\substack{y_1=1\\7\nmid y_1}}^{3^2\cdot7-1}
\left(\left\lfloor\frac{3^2y_1}{7}\right\rfloor-\left\lfloor\frac{3^2y_1}{7\cdot3}\right\rfloor\right)\\
&+\sum_{y_1=1}^{3^2-1}\left(\left\lfloor\frac{3^2\cdot7y_1}{2}\right\rfloor-\left\lfloor\frac{3^2\cdot7y_1}{2\cdot7}\right\rfloor
-\left\lfloor\frac{3^2\cdot7y_1}{2\cdot3}\right\rfloor+\left\lfloor\frac{3^2\cdot7y_1}{2\cdot7\cdot3}\right\rfloor\right)\\
={}&\sum_{k=0}^{3^2-1}\sum_{j=1}^{7-1}\left(\left\lfloor\frac{3^2(7k+j)}7\right\rfloor-\left\lfloor\frac{3^2(7k+j)}{7\cdot3}\right\rfloor\right)+\sum_{y_1=1}^{8}18y_1.
\end{aligned}
$$


> 来源缺口／核对说明：上式与下面的末段计算之间，所附提取版没有完整连续画面。缺失的过渡步骤不予补写。


$$
\begin{aligned}
&\cdots+9\big[(0+0+0+1+1+1)-(0+0+1+1+2+2)\big]+648\\
&=1296+189-27+648=1458+648=2106.
\end{aligned}
$$


> 资料说明：这里保留原样卷的分类记号；不能直接把此分类项与解法一的 $N_2$ 对号比较。


<details class="source-page"><summary>查看第 31 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=31" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-31.webp" width="1400" height="893" alt="组合数学习题课合订第 31 页原稿" loading="lazy" decoding="async" />

</details>


## 32 · 原文中的 |A₃²| 求和式

<strong>期中样卷 · 第 6 题 · 解法二（续）</strong>  
来源：讲义提取版第 33 页；视频 40:31 · 合订 PDF 第 32 页


$$
\begin{aligned}
|A_{3^2}|={}&\sum_{\substack{y_1=1\\3\nmid y_1}}^{3^3-1}
\left(\left\lfloor\frac{7y_1}{2}\right\rfloor-\left\lfloor\frac{7y_1}{2\cdot7}\right\rfloor\right)\\
&+\sum_{y_1=1}^{3^2-1}\left(\left\lfloor\frac{7\cdot3y_1}{2}\right\rfloor-\left\lfloor\frac{7\cdot3y_1}{2\cdot7}\right\rfloor
-\left\lfloor\frac{7\cdot3y_1}{2\cdot3}\right\rfloor+\left\lfloor\frac{7\cdot3y_1}{2\cdot7\cdot3}\right\rfloor\right)\\
={}&\sum_{k=0}^{3^2-1}\sum_{j=1}^{2}\left(\left\lfloor\frac{7(3k+j)}2\right\rfloor-\left\lfloor\frac{3k+j}{2}\right\rfloor\right)+\sum_{y_1=1}^{8}6y_1\\
={}&\sum_{k=0}^{8}\sum_{j=1}^{2}(9k+3j)+6\cdot9\cdot4\\
={}&2\cdot9\cdot9\cdot4+9\cdot3\cdot3+216=945.
\end{aligned}
$$


> 资料说明：保留原画面可辨认的求和上下限、分母和结果；没有添加新的演算步骤。


<details class="source-page"><summary>查看第 32 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=32" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-32.webp" width="1400" height="818" alt="组合数学习题课合订第 32 页原稿" loading="lazy" decoding="async" />

</details>


## 33 · 原文中的 |A₃³| 求和式

<strong>期中样卷 · 第 6 题 · 解法二（续）</strong>  
来源：讲义提取版第 33 页；视频 40:31 · 合订 PDF 第 33 页


$$
\begin{aligned}
|A_{3^3}|={}&\sum_{\substack{y_1=1\\3\nmid y_1}}^{3^2-1}
\left(\left\lfloor\frac{7y_1}{2}\right\rfloor-\left\lfloor\frac{7y_1}{2\cdot7}\right\rfloor\right)\\
&+\sum_{y_1=1}^{3-1}\left(\left\lfloor\frac{7\cdot3y_1}{2}\right\rfloor-\left\lfloor\frac{7\cdot3y_1}{2\cdot7}\right\rfloor
-\left\lfloor\frac{7\cdot3y_1}{2\cdot3}\right\rfloor+\left\lfloor\frac{7\cdot3y_1}{2\cdot7\cdot3}\right\rfloor\right)\\
={}&\sum_{k=0}^{3-1}\sum_{j=1}^{3-1}\left(\left\lfloor\frac{7(3k+j)}2\right\rfloor-\left\lfloor\frac{3k+j}{2}\right\rfloor\right)+\sum_{y_1=1}^{2}6y_1\\
={}&\sum_{k=0}^{2}\sum_{j=1}^{2}(9k+3j)+6\cdot3\\
={}&2\cdot9\cdot3+3\cdot3\cdot3+18=99.
\end{aligned}
$$


> 来源缺口／核对说明：所附画面到此结束，$|A_{3^4}|$、其余分类及解法二最终合计未完整显示。完整的可见解法一已在前文保留；这里不自行续写解法二。


<details class="source-page"><summary>查看第 33 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=33" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-33.webp" width="1400" height="833" alt="组合数学习题课合订第 33 页原稿" loading="lazy" decoding="async" />

</details>


# 三、后半段课件：批注与补充


## 34 · 课件章节标记

<strong>后半段课件 · 作业一</strong>  
来源：批注课件提取版第 2 页；课件标注第 3/55 页 · 合订 PDF 第 34 页

原画面为章节页：

<div class="center">

</div>

> 资料说明：所附视频提取版中，该章节页之后直接跳转至第 40 题，未包含作业一各题的连续题干。这里记录原有章节文字，不补造缺失题目。


<details class="source-page"><summary>查看第 34 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=34" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-34.webp" width="1400" height="587" alt="组合数学习题课合订第 34 页原稿" loading="lazy" decoding="async" />

</details>


## 35 · 第 40 题 · 3 × 4 棋盘骨牌覆盖

<strong>后半段课件 · 作业三</strong>  
来源：批注课件提取版第 3–5 页；课件标注第 18、20、21/55 页 · 合订 PDF 第 35 页

#### 题目

（Brualdi, 2012, 1.5）求 $3\times4$ 棋盘的多米诺骨牌完美覆盖方式数。用 $1\times2$ 骨牌覆盖整个棋盘，要求无重叠、无空缺。按骨牌占据的格子区分铺法；旋转或镜像得到的铺法不另行合并。

<div class="center">

</div>

#### 原有解答

设跨线骨牌数为 $c$，完全落在左半棋盘的骨牌数为 $a$。左半边有 6 格，内部骨牌每块占 2 格，跨线骨牌每块占 1 格，故

$$
6=2a+c,\qquad0<c\le3\quad\Longrightarrow\quad c=2.
$$

$c$ 为偶数；中线最多有 3 行可以横跨，本类要求 $c>0$。两块跨线骨牌选第 1、2 行或第 2、3 行都可以，其余骨牌被唯一确定；选第 1、3 行不可以，左上角与左下角会争用第 2 行第 1 列同一格。

> 资料说明：题目来源：课件注明“作业讲义，第 9 页”。无跨线情形的中间页未完整包含在所附提取版，原最终合计在下一页保留。


<details class="source-page" open><summary>查看图示与第 35 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=35" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-35.webp" width="1400" height="811" alt="组合数学习题课合订第 35 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 36 · 跨线铺法、总数与板书

<strong>后半段课件 · 第 40 题（续）</strong>  
来源：批注课件提取版第 5 页；课件标注第 21/55 页 · 合订 PDF 第 36 页

两块跨线骨牌位于相邻两行时，各有一种完整铺法。

<div class="center">

</div>

两类互不重叠，并且覆盖所有情形，因此


$$
9+2=11.
$$


<strong>课堂点拨：</strong>先按是否跨线分类，再用奇偶性；只计 $3\times3$ 会漏两种铺法。

<span class="source-annotation"><strong>板书（可辨认部分）</strong></span>：记 $3\times n$ 的铺法数为 $f(n)$，画面写有

$$
\color{#b45309}f(n)=f(2)f(n-2)+f(n-2)-f(n-4)=4f(n-2)-f(n-4).
$$


> 资料说明：保留可读递推式；板书中用于解释递推的随手棋盘草图未作为独立题目。上面两种跨线铺法按原课件重绘。


<details class="source-page" open><summary>查看图示与第 36 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=36" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-36.webp" width="1400" height="976" alt="组合数学习题课合订第 36 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 37 · 第 83 题 · 有理数与循环小数

<strong>后半段课件 · 作业五</strong>  
来源：批注课件提取版第 6–7 页；课件标注第 23、25/55 页 · 合订 PDF 第 37 页

#### 题目

（Brualdi, 2004, 2.8）Use the pigeonhole principle to prove that the decimal expansion of a rational number $m/n$ eventually is repeating. For example,

$$
\frac{34{,}478}{99{,}900}=.34512512512512512\ldots
$$

<span class="source-annotation"><strong>板书：递推关系。</strong></span>小数写作 $0.a_1a_2\cdots$，画面列出

$$
\begin{aligned}
a_1&=\left\lfloor\frac{10m}{n}\right\rfloor,& r_1&=(10m)\bmod n,\\
a_2&=\left\lfloor\frac{10r_1}{n}\right\rfloor,&r_2&=(10r_1)\bmod n,\\
a_3&=\left\lfloor\frac{10r_2}{n}\right\rfloor,&r_k&=(10r_{k-1})\bmod n,\\
a_k&=\left\lfloor\frac{10r_{k-1}}n\right\rfloor.&&
\end{aligned}
$$


> 资料说明：题目来源：课件注明“作业讲义，第 15 页”。板书按小数部分进行除法，起始记号沿用画面；整数部分的单独处理在这张图中未完整写出。重复余数对应重复数字块的结论见下一页。


<details class="source-page"><summary>查看第 37 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=37" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-37.webp" width="1400" height="832" alt="组合数学习题课合订第 37 页原稿" loading="lazy" decoding="async" />

</details>


## 38 · 鸽巢原理保证最终循环

<strong>后半段课件 · 第 83 题（续）</strong>  
来源：批注课件提取版第 7 页；课件标注第 25/55 页 · 合订 PDF 第 38 页

#### 原有解答

若余数始终不为 0，则这 $n$ 个余数

$$
r_0,r_1,\ldots,r_{n-1}
$$

都属于只有 $n-1$ 个元素的集合 $\{1,2,\ldots,n-1\}$。 由鸽巢原理，存在 $0\le i<j\le n-1$，使得 $r_i=r_j$。由商和余数唯一，得

$$
a_{i+1}=a_{j+1},\qquad r_{i+1}=r_{j+1}.
$$

继续推下去，有 $a_{i+t}=a_{j+t}$ 对所有 $t\ge1$ 成立。


$$
\text{从第 }i+1\text{ 位起，长度为 }j-i\text{ 的数字块不断重复。}
$$


因此小数展开最终循环。

> 资料说明：保留原课件“余数始终不为 0”这一分支。有限小数分支在所附图中未展开，没有新增证明步骤。


<details class="source-page"><summary>查看第 38 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=38" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-38.webp" width="1400" height="724" alt="组合数学习题课合订第 38 页原稿" loading="lazy" decoding="async" />

</details>


## 39 · 第 95 题 · 三色三角形的上界

<strong>后半段课件 · 作业五</strong>  
来源：批注课件提取版第 8 页；课件标注第 29/55 页 · 合订 PDF 第 39 页

#### 原有解答

给 $K_{17}$ 的每条边任意染红、蓝、绿三色之一，任取顶点 $v$。 从 $v$ 出发的 16 条边至少有 $\lceil16/3\rceil=6$ 条同色。不妨选 6 条红边，邻点集记为 $S$。

<div class="center">

</div>

1\. $S$ 内有红边：其两个端点与 $v$ 构成红三角形。

2\. $S$ 内没有红边：$S$ 上的 $K_6$ 仅有蓝、绿两色。由二色结论，必有蓝或绿三角形。 所以


$$
r(3,3,3)\le17.
$$


> 资料说明：原课件点拨：17 来自“先保证某点有 6 个同色邻点”，本题只要求上界。红色板书记为“鸽巢原理加强版”；手绘六条边在上图重绘。题目来源标注：作业讲义第 16 页。


<details class="source-page" open><summary>查看图示与第 39 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=39" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-39.webp" width="1400" height="900" alt="组合数学习题课合订第 39 页原稿，含原资料图示" loading="lazy" decoding="async" />

</details>


## 40 · 第 201 题 · 列出全部线性扩张

<strong>后半段课件 · 作业七</strong>  
来源：批注课件提取版第 9 页；课件标注第 43/55 页 · 合订 PDF 第 40 页

#### 原有解答

$a$ 必须第一，$d$ 必须最后。中间四个元素只需保持 $b<c$ 和 $e<f$。从 4 个位置中选 2 个依次放 $b,c$，其余位置依次放 $e,f$，共有 $\binom42=6$ 种。

<div class="center">

| 序号 | 线性扩张（从小到大） |
|:----:|:---------------------|
|  1   | $a<b<c<e<f<d$      |
|  2   | $a<b<e<c<f<d$      |
|  3   | $a<b<e<f<c<d$      |
|  4   | $a<e<b<c<f<d$      |
|  5   | $a<e<b<f<c<d$      |
|  6   | $a<e<f<b<c<d$      |

</div>

<strong>课堂点拨：</strong>线性扩张保留原有的全部先后要求，同时给不可比元素安排顺序。要求列出全部扩张，不能只报数目 6。

> 资料说明：题目来源：课件注明“作业讲义，第 31 页”。页码采用页脚 43/55；左下阅读器浮标短暂显示 42/55，不据浮标改写页脚。


<details class="source-page"><summary>查看第 40 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=40" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-40.webp" width="1400" height="889" alt="组合数学习题课合订第 40 页原稿" loading="lazy" decoding="async" />

</details>


## 41 · 第 1.2 题 · 逆序数列与排列

<strong>后半段课件 · 样卷补充</strong>  
来源：批注课件提取版第 10、17 页；课件标注第 45/55 页 · 合订 PDF 第 41 页

#### 题目

对应于逆序数列为 $6,6,5,1,2,1,0,0$，构造 $\{1,2,\ldots,8\}$ 的排列，结果为

<div class="center">

| 选项  | 排列     |
|:-----:|:---------|
| \(a\) | 35168274 |
| \(b\) | 73658412 |
| \(c\) | 74658312 |
| \(d\) | 83176215 |

</div>

#### 原有解答

原卷参考答案为


$$
\text{（c）}\quad74658312.
$$


> 资料说明：题目与答案来源：课件注明“样卷，第 1 页”。两次出现的是同一页，已合并；未新增求解过程。


<details class="source-page"><summary>查看第 41 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=41" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-41.webp" width="1400" height="880" alt="组合数学习题课合订第 41 页原稿" loading="lazy" decoding="async" />

</details>


## 42 · 第 4 题 · 非攻击型车的容斥式

<strong>后半段课件 · 样卷补充</strong>  
来源：批注课件提取版第 11–12 页；课件标注第 49–50/55 页 · 合订 PDF 第 42 页

> 来源缺口／核对说明：所附画面仅包含参考答案，原题的棋盘与禁止位置未显示。本页按可见答案重排，不据此补造题干。

#### 原有解答

［解］由容斥原理，放置 $n$ 个非攻击型车的方式数为

$$
\begin{aligned}
p={}&n!-r_1(n-1)!+r_2(n-2)!-\cdots\\
&+(-1)^kr_k(n-k)!+\cdots+(-1)^nr_n.\tag{4.1}
\end{aligned}
$$

其中 $r_k$ 为放置 $k$ 个非攻击型车到禁止位置上的方式数。对于本题：

$$
\begin{aligned}
r_1&=4+4+3=11,\qquad\text{(4.2)}\\
r_2&=(2+2+1)+(4\cdot4+4\cdot3+4\cdot3)=45,\qquad\text{(4.3)}\\
r_3&=4\cdot2\cdot2+(4\cdot1+2\cdot3)\cdot2+4\cdot4\cdot3=84,\qquad\text{(4.4)}\\
r_4&=(2\cdot1\cdot2+2\cdot2)+(4\cdot4\cdot1+4\cdot3\cdot2\cdot2)=72,\qquad\text{(4.5)}\\
r_5&=3\cdot2\cdot2+4\cdot2\cdot1\cdot2=28,\qquad\text{(4.6)}\\
r_6&=2\cdot2\cdot1=4.\qquad\text{(4.7)}
\end{aligned}
$$


> 资料说明：原卷参考答案来源标注：样卷第 4–5 页。红色手写大括号为各项分组的提示，已通过保留原算式各组乘积体现；未额外计算画面未给出的最终 $p$。


<details class="source-page"><summary>查看第 42 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=42" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-42.webp" width="1400" height="876" alt="组合数学习题课合订第 42 页原稿" loading="lazy" decoding="async" />

</details>


## 43 · 第 5 题 · 三个整数集合

<strong>后半段课件 · 样卷补充</strong>  
来源：批注课件提取版第 13–14 页；课件标注第 52–53/55 页 · 合订 PDF 第 43 页

#### 题目

设 $S_1,S_2,S_3$ 是三个由整数组成的非空集。已知对 123 的任意排列 $ijk$，若 $x\in S_i$、$y\in S_j$，则 $y-x\in S_k$。证明：$S_1,S_2,S_3$ 中必有两个集合相等。

<span class="source-annotation"><strong>板书提示：</strong></span>画面圈出“整数”“非空集”，并以 $x=y\Rightarrow0\in S_k$ 提示两个集合相等时第三个集合含 0 的关系。

#### 原有解答

［解］首先检查任一集合元素的一般属性。若 $x\in S_i$、$y\in S_j$，则 $y-x\in S_k$。由题，那么

$$
(y-x)-y=-x\in S_i.\tag{5.1}
$$

即若 $x\in S_i$，那么 $-x\in S_i$。

> 资料说明：题目来源标注：样卷第 5 页；式（5.1）亦来自样卷第 5 页。原板书中的提示以文字保留，未将提示当作新的待证题。


<details class="source-page"><summary>查看第 43 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=43" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-43.webp" width="1400" height="615" alt="组合数学习题课合订第 43 页原稿" loading="lazy" decoding="async" />

</details>


## 44 · 出现零时，两个集合相等

<strong>后半段课件 · 样卷第 5 题（续）</strong>  
来源：批注课件提取版第 15–16 页；课件标注第 54–55/55 页 · 合订 PDF 第 44 页

#### 原有解答

要使两个集合相等，第三个集合定含有 0。不妨设 $0\in S_1$，那么对任意

$$
x\in S_2,\qquad x-0\in S_3,\tag{5.2}
$$

即 $S_2\subseteq S_3$。同理，对任意

$$
y\in S_3,\qquad y-0\in S_2,\tag{5.3}
$$

即 $S_3\subseteq S_2$，因此 $S_2=S_3$。

> 资料说明：本页保留原卷叙述及式（5.2）–（5.3）；下一页为“至少一个集合含 0”的反证。


<details class="source-page"><summary>查看第 44 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=44" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-44.webp" width="1400" height="587" alt="组合数学习题课合订第 44 页原稿" loading="lazy" decoding="async" />

</details>


## 45 · 最小正整数的反证

<strong>后半段课件 · 样卷第 5 题（续）</strong>  
来源：批注课件提取版第 16 页；课件标注第 55/55 页 · 合订 PDF 第 45 页

#### 原有解答

若三个集合均不含 0，因为集合非空，且前面已知任一集合含符号相反的整数，那么一定存在最小正整数

$$
a\in S_1\cup S_2\cup S_3.
$$

不妨设 $a\in S_1$，再取最小正整数 $b\in S_2\cup S_3$，不妨设 $b\in S_2$，那么 $b>a$（否则 $b-a=0\in S_3$，与前面假设矛盾），但

$$
b>b-a>0,\qquad b-a\in S_3.\tag{5.4}
$$

这与 $b$ 是 $S_2\cup S_3$ 的最小正整数矛盾。反证法给出“三个集合均不含 0”的条件不成立，得证。

> 资料说明：原卷参考答案来源标注：样卷第 6 页，式（5.4）。下划线强调的“非空”“最小正整数”已用正文与公式保留。


<details class="source-page"><summary>查看第 45 页高清原稿</summary>

<a href="/downloads/combinatorics-notes.pdf#page=45" target="_blank">在 PDF 中查看本页 ↗</a>

<img src="/figures/combinatorics/page-45.webp" width="1400" height="602" alt="组合数学习题课合订第 45 页原稿" loading="lazy" decoding="async" />

</details>
