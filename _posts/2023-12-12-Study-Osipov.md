---
permalink: /:year-:month-:day/:title
layout: post
title:  "奥西波夫：俄国的“兰切斯特”方程"
author: "Lei Lie"
categories: Essay
tag: notdouban
---

> # 写在前面
>
> 该文章为论文*Osipov: The 'Russian Lanchester'* Helmbold (1991) 的翻译版本。目的在于让人了解在历史深厚的尘埃之下，同样有另外一位学者提出了与兰切斯特类似的理论。
>
> 仅以此文纪念俄国科学家奥西波夫。

注：除Lanchester翻译为兰切斯特，Osipov翻译为奥西波夫之外，所引用文献的论文作者名字均保留其英文原文。

---

[TOC]

# 奥西波夫：俄国的“兰切斯特”

## 摘要

1915年，几乎与兰切斯特同一时间，奥西波夫在沙皇俄国的杂志*Military Collection*（《军事收藏》）上发表了一系列瞩目的文章。它包含了一系列分析军事战斗损耗的理论，该理论在西方被称为“兰切斯特平方律”。奥西波夫的著作译本新近完成，这将使西方军事分析家更容易欣赏其优点。

**关键词**：历史；军事；时间；经验统计；微分方程；兰切斯特方程；奥西波夫

## 1. 引言

Helmbold和Rehm（1991）最近完成了奥西波夫（1915）五部分系列文章的英文翻译，该系列文章于1915年发表在俄国期刊《军事收藏》，题目为《敌对双方的数值兵力对其伤亡的影响》。这些文章刊登在《军事收藏》的以下几期：

- 第一部分：1915年6月，第6期，第58-74页；
- 第二部分：1915年7月，第7期，第25-36页；
- 第三部分：1915年8月，第8期，第31-40页；
- 第四部分：1915年9月，第9期，第25-37页；
- 第五部分（增编）：1915年10月，第10期，第93-96页。

该著作共计55页，包括9章内容。此外还有引言和增编内容。它包括19个公式，6个不包含战例的表格，4个案例，和10个问题。之所以进行这项翻译工作的原因，是因为奥西波夫的作品在历史上和方法论上都非常重要，应该用英语进行翻译。

## 2. 优先权的问题

Yusupov (1988)声称奥西波夫在发现通常被称为兰彻斯特方程的微分方程和被称为兰彻斯特的$N$-平方定律的关系方面具有优先权。我们希望下面的粗浅评论能对两人所提出理论的优先权问题有所启发。

首先，我们注意到，在19世纪和20世纪初，对科学的战争理论的兴趣非常“在空中”。Clausewitz、Jomini、Bloch、Fuller、Mahan等作家试图理解支配战争和作战力量相互作用的基本原理或科学规律。这种关注足够广泛，以至于在文学作品中得到了反映。例如，Tolstoy (1865) 写道:“军队的精神是一个因素，它乘以数量就会产生力量。定义和表达这种未知因素——军队精神——的意义，是科学的难题。十个人、营或师与十五个人、营或师作战，征服——也就是说，杀死或俘虏——所有其他人，而自己损失四个....因此，4人等于15人，因此$4x = 15y$。因此，$x/y = 15/4$。这个方程并没有给我们未知因子的值，而是给了我们两个未知数之间的比率。通过将各种选定的历史单位（战役、战役、战争时期）纳入这样的方程，可以得到一系列数字，其中应该存在某些规律，并且可能被发现。”此外，其他研究者也致力于编纂统计数据来支持这种探索（Livermore、Berndt、Bodart等人)。Kipp (1987) 提到了早期俄国人对军事统计和军事应用数学的兴趣，并在这方面特别注意到Miliutin和Obruchev (1871) 的工作（他们分别是俄国亚历山大二世时期的战争部长，亚历山大二世、亚历山大三世和尼古拉斯二世时期的总参谋长），以及Volotsky  (1903) 关于概率论和步兵和炮兵规范的著作。

但是为了关注更直接相关的工作，我们首先观察到Fiske (1905) 的获奖工作，正如Weiss (1962)所报道的那样，包含了兰彻斯特平方定律方程的离散版本的基本思想。Hugles (1988a) 最近发现了Chase (1902) 结果的奇怪故事，这些结果可能是该领域最早的，但直到1972年才解密。美国海军研究生学院的 Wayne Hughes 教授提请注意Fiske (1916)的一个脚注，其中提到了这些结果，并指出这些结果是由海军少将Jehu Valentine Chase使用“微积分的应用”获得的。Chase (1902)的一篇论文出现在Hughes (1988b)的附录C中。

F. W. Lanchester (1916)的名著于1916年1月在伦敦出版。然而。早前的部分（特别是第五章和第六章）在1914年9月至12月间刊登在英国《工程学》杂志上。著名的“$N$-平方”定律出现在Lanchester (1914)。兰彻斯特这本书的初稿是在1913年至1914年的冬天准备的。

相比之下，奥西波夫的文章发表于1915年6月至10月。在我们看来，作品本身的结构、形式和惊人的独创性证明了他长时间的沉思。此外，根据内部证据，我们认为奥西波夫对兰彻斯特或Fiske的工作都没有直接或具体的了解，他的成就是独立于他们的。为了支持这一观点，让我们指出，奥西波夫在其序言的最后一句（不那么明确地在其系列文章的其他地方）中明确提出了优先权的主张，而且没有引用前人的著作以及他整个陈述的基调都使这一观点可信。

## 3. 奥西波夫与兰彻斯特著作比较

即使事实证明奥西波夫知道Fiske或兰彻斯特的工作（例如，通过兰彻斯特在《工程学》杂志上发表的文章），奥西波夫的独特贡献也可以说比兰彻斯特的贡献更加实质性和重要。奥西波夫对这个问题的研究更深入，他的方法和观点更健全、更科学，这使他站在了那些对战斗理论感兴趣的学者前列。

一般来说，兰彻斯特的表达比奥西波夫的更具视觉性和说教性。两者都注重力量（forces）的质量和集中。奥西波夫的书中充斥着更多的东西，如表格和公式。而兰彻斯特强调图形显示。奥西波夫实际上写下了平方定律损耗方程的一般数学解。虽然作者确信兰彻斯特知道这个答案，但他并没有在他的著作中提出，而是满足于用图形的例子。总体印象是，奥西波夫采取了科学的方法，而兰彻斯特采取了政治的、辩论的或倡导的方法。

在我看来，这方面最显著的例子是，奥西普夫特别注意通过将理论的预测与历史战役的结果进行比较，来检验或验证理论。通过这种方式，他发现了一个重要的事实，即战斗力与数量实力的平方成不成正比。他显然是历史上第一个陈述这一事实的人，我称之为“奥西波夫定律”。

## 4. 奥西波夫理论的突出特点

在他的文章第一段中，奥西波夫提出了一个基本的挑战：“在战斗中取得胜利的所有主要原因或环境是什么?”无疑，这是所有军事行动分析中最根本的问题。面对这一挑战，奥西波夫提出了一个假设，即损失与优势成反比。这个定性假设可以翻译成现代数学符号，大致为

$$
\frac{\rm A's \ losses}{\rm B's losses} = K \left ( \frac{\rm B's \ strength}{\rm A's \ strength} \right )^p
$$

其中，$p$为正整数。

然后，奥西波夫用著名的《俄罗斯军事和海军科学百科全书》中的数据，列出了38场战役的优势和损失，并对这些数字是否倾向于支持他的假设进行了定性的介绍，以激励他随后对这个问题的定量处理。

在那之后。奥西波夫给出了一些接触兵力相互损耗的数值例子来介绍他对平方定律损耗方程的数学发展。为了给奥西波夫的论文增添趣味，我们举例说明了奥西波夫对他的公式的推导。令$A$和$B$表示战斗人员队伍中步枪兵的初始兵力，$A$表示一名步枪兵在所有单位时间内造成的命中，$t$表示到期时必须找到交战双方步枪的剩余强度$A'$和$B'$。奥西波夫的目的是证明这些与初始强度有一个非常简单的关系，即

$$
A'^2-B'^2 = A^2-B^2
\tag{1}
$$

虽然奥西波夫承认公式(1)可以通过微积分得到，但他提出了以下替代推导。设$\Delta t$是一个无限小的时间间隔。在双方条件相同的情况下，订约双方在$\Delta t$期满时的剩余强度为$\Delta t$、$2\Delta t$、$3\Delta t$、$\dots$、$n\Delta t$，如表1所示

表1

| 时间间隔     | $A$方剩余兵力                                                | $B$方剩余兵力                                                |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| $\Delta t$   | $A'_1=A-(\alpha \Delta t)B$                                  | $B'_1=B-(\alpha \Delta t)A$                                  |
| $2 \Delta t$ | $A'_2=A-2(\alpha \Delta t)B+(\alpha \Delta t)^2 A$           | $B'_2=B-2(\alpha \Delta t)A+(\alpha \Delta t)^2 B$           |
| $3 \Delta t$ | $A'_3=A-3(\alpha \Delta t)B+3(\alpha \Delta t)^2 A-(\alpha \Delta t)^3 B$ | $B'_3=B-3(\alpha \Delta t)A+3(\alpha \Delta t)^2 B-(\alpha \Delta t)^3 A$ |

以这种方式，奥西波夫组成了类似于牛顿二项式公式的项，为

$$
n \Delta t \left\{\begin{matrix}
 A'_n = A - n(\alpha \Delta t)B + \frac{n(n-1)}{1 \cdot 2}(\alpha \Delta t)^2 A - \frac{n(n-1)(n-2)}{1 \cdot 2 \cdot 3}(\alpha \Delta t)^3 B + \cdots \\
 B'_n = B - n(\alpha \Delta t)A + \frac{n(n-1)}{1 \cdot 2}(\alpha \Delta t)^2 B - \frac{n(n-1)(n-2)}{1 \cdot 2 \cdot 3}(\alpha \Delta t)^3 A + \cdots
\end{matrix}\right.
$$

将最后一个表达式的项乘以$(n/n)^0, (n/n)^1, (n/n)^2, \dots$，观察 $n \Delta t = t$，以及分数 $1/n, 2/n, 3/n, \dots$，当 $n$ 趋于无穷大时归零。就不难推导出下公式：

$$
A' - B(\alpha t) + \frac{A}{1 \cdot 2}(\alpha t)^2 - \frac{B}{1 \cdot 2 \cdot 3}(\alpha t)^3 + \frac{A}{1 \cdot 2 \cdot 3 \cdot 4}(\alpha t)^4 - \cdots,
\tag{2a}
$$

$$
B' - B(\alpha t) + \frac{B}{1 \cdot 2}(\alpha t)^2 - \frac{A}{1 \cdot 2 \cdot 3}(\alpha t)^3 + \frac{B}{1 \cdot 2 \cdot 3 \cdot 4}(\alpha t)^4 - \cdots.
\tag{2b}
$$

这些公式很容易化简，如果我们观察到
$$
1 + (\alpha t) + \frac{1}{1 \cdot 2}(\alpha t)^2 + \frac{1}{1 \cdot 2 \cdot 3}(\alpha t)^3 + \frac{1}{1 \cdot 2 \cdot 3 \cdot 4}(\alpha t)^4 + \cdots,
\tag{2'a}
$$

$$
1 - (\alpha t) + \frac{1}{1 \cdot 2}(\alpha t)^2 - \frac{1}{1 \cdot 2 \cdot 3}(\alpha t)^3 + \frac{1}{1 \cdot 2 \cdot 3 \cdot 4}(\alpha t)^4 - \cdots,
\tag{2'b}
$$

表示自然对数 $\rm e=2.718281828\dots$的底数，取幂  $+ \alpha t$ 和 $- \alpha t$。如果在(2)中，我们将包含 $A$ 的项和包含 $B$ 的项分开组合，那么得到如下公式

$$
A' = A \frac{e^{\alpha t}+e^{-\alpha t}}{2}-B \frac{e^{\alpha t}+e^{-\alpha t}}{2}, \quad B' = B \frac{e^{\alpha t}+e^{-\alpha t}}{2}-A \frac{e^{\alpha t}+e^{-\alpha t}}{2}.
\tag{3}
$$

在最后一个公式(3)中，变量 $(\exp(\alpha t)+\exp (-\alpha t))/2$ 和 $(\exp(\alpha t)-\exp (-\alpha t))/2$ 在数学中被称为变量 $\alpha t$ 和公式(3)的双曲余弦和双曲正弦，可以写成

$$
A' = A \cosh(\alpha t)-B \sinh (\alpha t), \quad B' = B \cosh(\alpha t)-A\sinh(\alpha t).
\tag{4}
$$

奥西波夫随后注意到，公式(4)不便于与军事史数据进行比较，因为它们包含 $\alpha$ 和 $t$，而这两个通常是未知的。为了避免这个困难，他对(3)进行加减，得到

$$
A'+B'=(A+B)e^{-\alpha t}, \quad A'-B'=(A-B)e^{+\alpha t},
\tag{4'}
$$

然后将这些表达式相乘来获得(1)。

然后，奥西波夫一步一步地将这些结果推广到更复杂的情况。首先，他引入了“武器装备的技能或质量不同以及由此导致的交战双方单支步枪造成的命中不同”的可能性，例如，考虑 $A$ 方和 $B$ 方，$A$ 方在一个单位时间内命中等于 $\alpha$，$B$ 方在一个单位时间内命中等于 $\beta$。这个公式的推导没有给出，因为它的大致轮廓与前面引用的公式相似。以下是公式，为

$$
\sqrt \alpha A' = \sqrt \alpha A \cosh(t \sqrt{\alpha \beta}) - \sqrt \beta B \sinh(t \sqrt{\alpha \beta}),
\tag{5a}
$$

$$
\sqrt \beta B' = \sqrt \alpha B \cosh(t \sqrt{\alpha \beta}) - \sqrt \alpha A \sinh(t \sqrt{\alpha \beta}).
\tag{5b}
$$

公式(5)可以简化为公式(1)的形式：为此，有必要进行变换，注意到公式(5)是公式(4)的形式，将 $\alpha t$代入 $t \sqrt{\alpha \beta}$， $A$ 代入$A \sqrt \alpha$，$B$ 代入 $B \sqrt \beta$， $A'$ 代入 $A'\sqrt \alpha$，$ B'$ 代入 $B'\sqrt \beta$ 即可得到；因此，后一种更复杂的表达式转化为公式(4)，如果毁伤系数是 $\sqrt{\alpha \beta}$ （代替公式(4)中的 $\alpha$），它将给出剩余兵力；因此，将公式(1)中的初始兵力和剩余兵力 $A,B,A',B'$ 替换为新的表达式，得到

$$
\alpha (A^2-A'^2)=\beta(B^2-B'2), \quad or \ A^2-A'2=\frac{\beta}{\alpha}(B^2-B'^2).
\tag{6}
$$

为了考虑火炮，奥西波夫解决了以下问题：分为两方，一方有 $A$ 支步枪和 $M$ 门大炮，另一方有 $B$ 支步枪和 $N$ 门大炮。假定单位时间内每支步枪的毁伤系数为 $\alpha$，每门大炮的毁伤系数为 $\beta$。此外，假设伤亡只发生在步枪身上，大炮不造成伤亡，其数量保持不变。问题是：在进入战斗的 $t$ 个单位时间结束后，双方还剩下多少步枪？这方面的公式推导，虽然复杂，但基本上会和公式(2)到公式(4)的推导类似，和公式(2)一样，首先要推导出无穷小乘以 $\text{d}r,2 \text{d}r,3 \text{d}r,\dots,$以此类推。因此，这里不做进一步说明，而是给出了确定双方在 $t$ 单位时间后的剩余兵力 $A'$ 和 $B’$ 的公式，为

$$
\left ( A' + \frac{\beta}{\alpha} M \right ) = \left ( A + \frac{\beta}{\alpha} M \right ) \cosh (\alpha t) - \left ( B + \frac{\beta}{\alpha} N \right ) \sinh(\alpha t),
\tag{7a}
$$

$$
\left ( B' + \frac{\beta}{\alpha} N \right ) = \left ( B + \frac{\beta}{\alpha} N \right ) \cosh (\alpha t) - \left ( A + \frac{\beta}{\alpha} M \right ) \sinh(\alpha t).
\tag{7b}
$$

将式(7a)和(7b)与式(4)进行比较，我们发现它们是相同的，除了数量 $A$，$A'$ 和 $B$，$B'$ 增加了 $M\beta / \alpha$ 和 $N \beta / \alpha$ 的数量。后一项代表了可以取代 $M$ 和 $N$ 大炮作用的步枪的假设数量。因此，如果我们想要考虑火炮的效果，那么我们可以使用前面的公式，只是步枪的数量必须增加火炮当量的数量。

如果除了火炮（$M$ 和 $N$ 门大炮，命中率为 $\beta$），战斗人员还拥有 $P$ 和 $Q$ 门机枪，命中率为 $\gamma$，那么双方兵力计算公式计算为

$$
A+M \beta / \alpha + P \gamma / \alpha \ 和 \ B + N \beta / \alpha + Q \gamma / \alpha,
$$

其中，$\gamma / \alpha$ 为机枪换算成步枪的等价系数。

从式(4)推导式(1)完全类似，可以从式(7)推导出如下公式，为

$$
\left ( A + \frac{\beta}{\alpha}M \right )^2 - \left ( A' + \frac{\beta}{\alpha}M \right )^2 = \left ( B + \frac{\beta}{\alpha}N \right )^2 - \left ( B' + \frac{\beta}{\alpha}M \right )^2.
\tag{8}
$$

有了这个公式，我们可以根据 $A$、$B$、$M$、$N$、$B’$ 和 $\beta / \alpha$ 的知识来确定 $A’$；或者根据对 $A$、$B$、$M$、$N$ 的了解，以及双方 $a$ 和 $B$ 的伤亡情况，确定数值系数 $\beta / \alpha$。为了后一种目的，有必要展开(8)式并消去公共项，将 $A' = A -a$ 和 $B'=B-b$，由此我们将得到

$$
(A^2-A'^2)=(B^2-B'^2)-2\frac{\beta}{\alpha}(aM-bN).
\tag{9}
$$

利用这个公式，再加上波罗底诺战役、吕岑战役和滑铁卢战役中大炮数量的数据，奥西波夫然后估计出“将大炮转换成大炮的系数火枪手等价物”。然后，奥西波夫给出了公式(7)和式(8)的另一个有趣的应用，在这种情况下，假设较强的一方在正面有 $A$ 名步枪兵，在转弯的侧面有$C$ 名步枪兵，而较弱的一方在正面只有 $B$ 名步枪兵。由于与包围机动作战是困难的，奥西波夫假设侧翼的 $C$ 单元没有暴露在 $B$ 的火力下，而是它自己——就像火炮的情况一样——仅仅造成 $\beta = ma$ 的命中，即正面火力的 $m$ 倍。然后，从侧翼翻转的那一刻起，两侧幸存的 $A’$ 和 $B’$ 的兵力由以下公式决定：

$$
A'+mC = (A+mC) \cosh(\alpha t) - B \sinh(\alpha t),
\tag{10a}
$$

$$
B' = B \cosh(\alpha t) - (A+mC) \sinh(\alpha t),
\tag{10b}
$$

$$
(A+mC)^2-(A'+mC)^2=B^2-B'^2.
\tag{10c}
$$

通过这些公式，奥西波夫表明，在 $m$ 大于 $1$ 的情况下，即侧翼被翻转时（论文原文为：i.e., when the flank is turned），与 $A + C$ 步枪兵与 $B$ 步枪兵正面攻击的战斗相比，在 $m$ 等于 $1$ 的情况下，幸存者 $A’$ 和 $B’$ 的数量会增加，$B’$ 的数量会减少。

最后，奥西波夫假设两方有如下情况：1）$A$ 名步枪手命中率为 $\alpha$，$M$ 门大炮命中率为 $\gamma$，$P$ 挺机枪命中 率为$\varepsilon$（单位时间），2）$B$ 名步枪手命中率为 $\beta$，$N$ 门大炮命中 率为$\alpha$，$Q$ 挺机枪命中率为 $\zeta$。然后，对于 $t$ 单位时间进入战斗后剩下的数字 $A'$ 和 $B'$，奥西波夫找到了以下公式，为

$$
\sqrt{\alpha}(\alpha A' + \frac{\gamma}{\alpha}M + \frac{\varepsilon}{\alpha} P) = \sqrt{\alpha} (A+\frac{\gamma}{\alpha}M+\frac{\varepsilon}{\alpha}P)\cosh(t\sqrt{\alpha \beta})-\sqrt{\beta} \left ( B+\frac{\delta}{\beta}N+\frac{\zeta}{\beta}Q \right ) \sinh{(t\sqrt{\alpha \beta})},
\tag{11a}
$$

$$
\sqrt{\beta}(B' + \frac{\delta}{\beta}N + \frac{\zeta}{\beta} Q) = \sqrt{\beta} (B+\frac{\delta}{\beta}N+\frac{\zeta}{\beta}Q)\cosh(t\sqrt{\alpha \beta})-\sqrt{\alpha} \left ( A+\frac{\gamma}{\alpha}M+\frac{\varepsilon}{\alpha}P \right ) \sinh{(t\sqrt{\alpha \beta})},
\tag{11a}
$$

$$
\left ( A'+\frac{\gamma}{\alpha}M+\frac{\varepsilon}{\alpha}P \right )^2=\left ( A+\frac{\gamma}{\alpha}M+\frac{\varepsilon}{\alpha}P \right )^2 - \frac{\beta}{\alpha} \left [ \left( B+\frac{\delta}{\beta}N+\frac{\zeta}{\beta}Q \right )^2-\left( B'+\frac{\delta}{\beta}N+\frac{\zeta}{\beta}Q \right )^2 \right ].
\tag{11c}
$$

奥西波夫对这组公式作了如下的总结:

> “所有的推导公式都是明确假设的必然结果。其中包括一个假设，即在无限短的时间内，双方的伤亡将与他们的数值兵力成反比，即 $\text{d} A \times A =\text{d}B \times b$。如果我们要将伤亡理论建立在无限短的时间内伤亡与数值兵力的平方根成反比的假设上，即 $\text{d} A \sqrt{A} = \text{d} B \sqrt{B}$，那么代替公式(1)，我们将取而代之，有
>
> $$
> A^{3/2} - A'^{3/2} = B^{3/2} - B'^{3/2}.
> \tag{12}
> $$
>
> 有了其他的假设，我们就会得到其他的公式。

我对奥西波夫测试或验证这些理论考虑的方法非常着迷。他非常明确地说明了他使用的是哪种方法，并花了一些时间来解释它们。他明确指出，这种方法首先要为大量战役编制关于数量优势、损失、炮兵优势、战术等方面的准确数据。然后，为了调查，例如战术对损失的影响，其程序是将战斗分成两列，一列用于使用战术，另一列用于不使用战术。一旦这一步完成，计算每一列的总损失列。奥西波夫观察到，这些总数将主要取决于使用或不使用所讨论的战术。

奥西波夫强调，他的理论是关于平均数的，而不是关于每一次偏离平均数的理论。他说：

> 通过个别战斗的例子来测试理论推论是愚蠢的，因为损失取决于 ......
>
> 在许多情况下……（因此）不可能要求这一理论适用于每一场战役……（另一方面）系统趋势虽然在个别战斗中被随机波动所掩盖，但在总体上却非常突出，因为它们随着战斗数量的增加而积累，并且不像随机波动那样相互抵消....对我们来说，研究系统趋势比研究随机波动更重要。

多么合理的方法啊！当然，我们需要知道平均数或趋势是什么，然后才能认识到(更不用说衡量偏离它的程度了)。在我们能够很好地解释是什么原因导致个别情况偏离平均或趋势之前，我们还需要了解主要平均或趋势是如何产生的。

奥西波夫认为，如果兵力损耗的 $N$-平方损耗定律是正确的，那么 $A$ 方的损失可以写成

$$
a = A - \sqrt{A^2-B^2+B'^2}.
$$

因此，计算出来 $v_i=a_{\text{computed}}-a_{\text{observed}}$，即根据该公式计算出的 $A$ 方的损失与历史记录的损失之间的差异，衡量了该理论与第一次战斗的历史记录的偏离程度。奥西波夫通过检查 $v$ 的平均值和标准差来检验该理论与历史的吻合程度。如果它们的平均值相对于它们的标准偏差离零太远，那么这表明一些“系统趋势”导致了这种偏离理论。奥西波夫用这种方式检验了一些理论。其中一种理论使用了方程中的指数 $2$。

$$
A^n-A'^n=B^n-B'^n,
$$

而另一种理论用指数 $3/2$ 代替 $2$ 。作为这些假设检验的结果，奥西波夫发现了一个重要的事实，即取值为 $n=3/2$ 时，“无比地符合现实”。即战斗力与战斗员人数的平方成正比。我称之为“奥西波夫定律”（Osipov’s Law）。

由于奥西波夫研究的是真实的历史数据，他还意识到，真正的战斗几乎不会持续到任何一方被歼灭的地步。为了解释这一点，他引入了一个断点假设，甚至从历史数据中估计了它的价值！后来的研究表明，所有这样的断点假设都是站不住脚的。但就当时的技术水平而言，奥西波夫的贡献是杰出的。

奥西波夫认识到，事实上也强调，出现在消耗方程中的数字指的是实际参战的部队数量。然而，他也意识到，通常由历史记录提供的数字是列在名册上的人员总数，而这些数字一般不等于实际参战的人数。因此，在方程式中使用历史上的花名册数字会产生一定程度的误差。奥西波夫对这个问题很敏感，并以两种方式处理这个问题。首先，他估计了使用花名册编号而不是那些积极参与的人所涉及的错误的大小，并表明它没有大到足以扰乱他的主要结论。其次，他认为他的数据来自激烈的战斗，每个人都必须参与其中，“无论他们是否愿意”，因此，使用花名册号码的任何错误都应该可以忽略不计。

奥西波夫通过10个数值例子说明了他的方程的应用。其中，

- 问题1是在特定条件下对双方损失数量的直接估计，并表明当a方对B方施加更大的力时，情况会更好。
- 问题2是B方的断点为20%，我们想要找到$A$方的损失。
- 问题3是一方只有效地利用了其力量的一部分。第四个问题是是根据普列夫纳战役的历史数据估算损耗系数。
- 问题4将这个损耗系数的比值作为已知值，并问在普列夫纳战役中需要多少俄国人才能达到收支平衡上的损失。
- 问题5解决的是在A方可能投入2500人或3000人，B方可能投入2000人或2500人的情况下，四种备选方案中哪一种是最合适的。
- 问题7比较了B方分批或一次性全部投入战斗的结果。
- 问题8分析了一场战役的结果，在这场战役中，a方使用其大部队连续三次与规模较小的B方分队交战，假设a方部队在三次交战中每一次都遭受一定的消耗，并使用其幸存的部队进行下一次交战。
- 问题9处理的是兵力的最优分配问题。
- 问题10说明，当双方数值相等时，总损失要比双方数值不相等时高得多。

奥西波夫将他的结论总结为三个主要论点，即：

1. 人数最多的一方承受的损失绝对较小，而不是战斗力较弱的一方。
2. 如果我们在力量上占优势，那么通过增加现役部队的数量，我们就可以赢得时间，减少损失。如果力量优势在对手方，那么通过增加现役部队的数量，我们就增加了敌人的损失，减轻了自己军队的道德压力，而不会增加我们的损失。
3. 我们所说的现役部队人数是指积极使用军用步枪、火炮、机枪、军刀等武器的人数，而不是名册上的人数，也不包括未参战的预备役人数。为了进行有效的比较，其他武器应换算成步枪的等价物。

奥西波夫随后指出了一个定性原则，即“关于聚集或集中力量的优点的推论不仅可以从平方定律得到，而且也可以从任何理论中得到，只要简单地假设数字上最强的一方的损失将小于较弱的一方，并且随着较强一方优势的增加而进一步减少”。奥西波夫重要而新颖的观点是，他的三个论点并不依赖于损耗方程的确切性质。相反，任何损耗与数值强度成反比的食堂吃定律都会导致定性相同的结果。这是对兰彻斯特的论点和观点的一个相当大的概括，也说明了奥西波夫在这些问题上思想的深度。

## 5. 奥西波夫理论的优点和缺点

在这里，我将尝试总结一下我认为奥西波夫论著的主要优点和缺点。首先，我们首次在已发表的著作中找到兰彻斯特微分方程解的显式公式。奥西波夫用一种独特的方法得到了这个解，据我们所知，这种方法之前或之后都没有在其他地方出现过。此外，我们还找到了一种独特的由该解推导兰彻斯特平方定律的方法，以及首先从兰彻斯特微分方程中消去时间参数的常用推导方法。奥西波夫还明确地将微分方程与差分方程联系起来，并证明差分方程的解在时间步长趋于零时接近前者的解。这一结果后来被 Engel (1963) 独立地重新发现。

奥西波夫在当时的另一个独特之处是对非同构部队的处理。奥西波夫从解决由单一类型部队（即仅装备步枪的步兵）组成部队的幸存者问题开始，陆续引入了其他类型的武器，特别是火炮和机枪。在介绍这些武器时，奥西波夫定义了火炮和机枪与步兵数量之间的 “换算系数”，并根据历史资料估计，一门大炮相当于约 100 名步兵。他的方法在概念上与许多当代（西方和苏联）集合兵力模型中使用的方法相同——见 Taylor (1983) 和 Yusupov (1988)——但有一个重要的例外，即奥西波夫努力从历史数据中获得其转换系数的数值估计。

奥西波夫也知道，真正的战斗很少会持续到一方被歼灭，并明确假设，当一方的伤亡人数达到一定比例时，一方将被迫放弃战斗——根据历史证据，他估计这一比例约为20%。这个概念在今天也经常被使用，尽管它现在被认为是不可接受的——参见Clark（1954）和Helmbold

（此页缺失，p. 286）

也许奥西波夫先生是他的亲戚，我们不知道）。Kipp (1987) 还指出，虽然Golovin (1938) 有很长的一节是关于奥托·本特的，但他没有提到奥西波夫。Kipp博士告诉我们，他花了很长时间、很辛苦地寻找关于这些杰出文章的作者奥西波夫的更多信息，但没有成功。

在他的文章中，奥西波夫自己说他从未参加过战争，但随后显示出对各种俄罗斯野战条例和“规划因素”的熟悉，比如一个单位的部队将被投入突击梯队的百分比，大炮与步兵的比例，以及突击队伍中部队的理论间隔。同样，虽然否认自己在军事史上有任何专业知识，但奥西波维通常会引用相关的历史例子来说明自己的观点，并表现出对军事史的普遍熟悉。奥西波夫参考了工程师手册中的双曲函数的表格值，并显示出非常扎实的数学和统计分析能力，这在他的时代是非常先进的技术教育。他的文笔也非常优雅，词汇量也很大，这可能表明他有学术背景。奥西波夫抱怨没有时间发展这一课题。或许，他是一个年轻的学者出身的军官，一边训练自己的部队，准备随军上前线，一边匆忙地为后代记录自己的工作——或者，这只是一个荒谬的浪漫想法？还有什么能解释他对“新闻报道”的持续抱怨呢？我将欢迎更多关于奥西波夫先生的信息，这位“俄罗斯兰彻斯特”的作品具有巨大的历史意义，他对方法论和认识论问题的观点在首次出现在文献中75年后的今天仍然具有极其重要的意义。

## 参考文献

Chase, J.V. (1902), "A mathematical investigation of the effect of superiority of force in combats upon the sea", Naval War College  Archives, RG 8, Box 109, XTAV, 1902, reprinted in Hughes (1988b). 

Clark, D.K. (1954), "Casualties as a measure of the loss of combat effectiveness of an infantry battalion", Technical Paper,  ORO-T-289, Operations Research Office, Bethesda, MD. 

Engel, J.H. (1954), "A verification of Lanchester's Law", Operations Research 2, 163-171.

Engel, J.H. (1963), "Comments on a paper by H.K. Weiss", Operations Research 11/1, 147-150.

Fiske, B.A. (1905), "American Naval Policy", US Naval Institute 113,1-80.

Fiske, B.A. (1916), "Hae Navy as a Fig~'iting Machine, see Hughes (1988b),.

Golovin, A.A. (1938), Nauka o Voine (Science of War), Signal, Paris. 

Helmbold, R.L (1961a), "Lanchcester parameters for some battles of the last two hundred years", Staff Paper, CORG-SP-122,  Combat Operations Research Group, US Army Continental Army Command, Fortress Monroe, VA.

Helmbold, R.L. (1961b), "Historical data and Lanchester's theory of combat", Staff Paper, CORG-SP-128, US Army Continental  Army Command, Fortress Monroe, VA. 

Helmbold, R.L. (1961c), "Lanchester's equations, historical battles and war games", in: Proceedings of the Eighth Military  Operations Research Symposium, Pt. Mugu, CA. 

Helmbold, R.L (1964a), "Historical data and Lanchester theory of combat: Part lI', Staff paper, CORG-SP-190, HQ, US Army  Combat Developments Command, Ft. Belvoir, VA.

Helmbold, R.L. (1964b), "Some observations on the use of Lanchester's theory for prediction", Operations Research 12, 778-781. 

Helmbold, R.L. (1965), "A modification of Lanchester's equation", Operations Research 13, 857-859.

Helmhold, R.L. (1967), "Some observations on validating combat models", Presented to the NATO Conference on Recent  Developments in Lanchester Theory. Munich, Germany. 

Helmbold, R.L. (1969), "Probability of victory in land combat as related to force ratio", Technical Paper, RAND P-4199, The  RAND Corporation, Santa Monica, CA. 

Helmbold, R.L. (1971a), "Air battles and ground battles - A common pattern?", Technical Paper, RAND P-4548, The RAND  Corporation, Santa Monica, CA.

Helmbold, R.L. (1971b), "Decision in battle: Breakpoint hypotheses and engagement termination data", Technical Report, RAND  R-772-PR, The RAND Corporation, Santa Monica, CA.

Helmbold, R.L. (1986), "Combat History Analysis Study Effort (CHASE): Progress Report for the period August 1984-June  1985", Technical Paper, CAA-TP-86-2, US Army Concepts Analysis Agency, Bethesda, MD, AD-F 860 122. 

Helmbold, R.L. (1987), "Do battles and wars have a common relationship between casualties and victory?", Technical Paper,  CAA-TP-87-16, US Army Concepts Analysis Agency, Bethesda, MD, AD-A 196 126.

Helmbold, R.L. (1990), "Rates of advance in historical land combat operations", Research Paper', CAA-RP-90-1, US Army  Concepts Analysis Agency, Bethesda, MD.

Helmbold, R.L (1991). "The constant fallacy", submitted to European Journal of Operational Research.

Helmboid, R.L,. and Rehm, A.S, (1991), "The influence of the numerical strength of engaged forces on their casualties", Research  Paper, CA_A-RP-91-2, US Army Concepts Analysis Agency, Bethesda, MD 20814-2797. A translation of Osipov (1915); to be  published in Naval Research Logistics Journal. Also available from the US Defense Documentation Center (DDC) as  AD-A241-534. 

Hughes. W.P. (1988a), personal communication.

Hughes, W.P. (ed.) (1988b), republication of Fiske, B.A. (1916), The Navy as a Fighting Machine, US Naval Institute Press.

Kipp, J. (1987), Personal communication of 2 June 1987 from Dr. Jakob W. Kipp, Senior Analyst, Soviet Army Studies Office, US  Army Combined Arms Center, Fort Leavenworth, Kansas. 

Lanchester, F.W. (1914), "The principle of concentration", Engineering October 2.

Lanchester, F.W. (1916), Aircraft in Warfare: The Dawn of the Fourth Arm, Constable & Co., London, The portion dealing with  'Lanchester's Square Law' was reprinted in: J.R. Newman (ed.) The World of Mathematics, Vol. 4, Simon & Schuster, New  York, 1956, 2138-2157.

Miliutin, D.S., and Obruchev, N.N. (1871), Voenniya Statistika - St. Petersburg, Izdaterstvo Generarnogo Shtaba.

Osipov, M. (1915). "The influence of the numerical strength of engaged sides on their losses", in Russian, Voenniy Sbornik, Nos 6  through 10 (Jun through Oct). 

Taylor, J.G. (1983). Lanchester Models of Warfare, Vols. I, II, Military Applications Section, Operations Research Society of  America, Arlington, VA.

Tolostoy, L. (1865), War and Peace, published between 1895 and 1869.

Volotsky, N. (1903). Voenniy Sbornik.

Weiss. H,K. (1962), "The Fiske model of warfare", Operations Research 10/4, 569-571. 

Yusupov, R.M., and Ivanov, V.P. (1988), "Matyematychyeskoye Modyelyrovanyye V Voyennom Dyelye (Mathematical modelling of Military engagements)", Voyenno-Istorichyeskyye Zhurnal (Military-Historical Journal) 9, 79-83.

# 附：Osipov: The ‘Russian Lanchester’原文

![Osipov_The Russian Lanchester_页面_01](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_01.jpg)

![Osipov_The Russian Lanchester_页面_02](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_02.jpg)

![Osipov_The Russian Lanchester_页面_03](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_03.jpg)

![Osipov_The Russian Lanchester_页面_04](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_04.jpg)

![Osipov_The Russian Lanchester_页面_05](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_05.jpg)

![Osipov_The Russian Lanchester_页面_06](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_06.jpg)

![Osipov_The Russian Lanchester_页面_07](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_07.jpg)

![Osipov_The Russian Lanchester_页面_08](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_08.jpg)

![Osipov_The Russian Lanchester_页面_09](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_09.jpg)

![Osipov_The Russian Lanchester_页面_10](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_10.jpg)

![Osipov_The Russian Lanchester_页面_11](./../images/img-2023-12-12/Osipov_The Russian Lanchester_页面_11.jpg)