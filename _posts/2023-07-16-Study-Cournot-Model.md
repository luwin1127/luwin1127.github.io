---
permalink: /:year-:month-:day/:title
layout: post
title:  "库诺特（Cournot）模型：完全信息博弈和不完全信息博弈的求解及代码实现"
author: "Lei Lie"
categories: Essay
tag: notdouban
---

# 写在前面

库诺特（Cournot）模型中，有两个参与者，称为企业1 和企业2；每个企业的战略是选择产量；支付是利润，为两个企业产量的函数。

模型中， $q_i \in [0,\infty)$ 代表第 $i$ 个企业的产量，$C_i(q_i)$ 代表成本函数，$P=P(q_1+q_2)$ 代表逆需求函数（$P$ 是价格；$Q(P)$ 是原需求函数。）那么，第 $i$ 个企业的利润为：

$$
\pi_i(q_1,q_2) = q_i P(q_1+q_2) - C_i(q_i), \quad i=1,2,
$$

式中，$(q_1^\*,q_2^\*)$ 为纳什均衡（Nash equilibrium）下各企业产量，为

$$
(q_1^*,q_2) \le (q_1^*,q_2^*) \le (q_1,q_2^*)
$$

那么，如何求解纳什均衡呢？对每个企业的利润函数求一阶导数，并零其等于零，求解公式为

$$
\begin{aligned}
    \frac{\partial \pi_1}{\partial q_1} = P(q_1+q_2) + q_1 P'(q_1+q_2) - C_1'(q_1) = 0, \\
    \frac{\partial \pi_2}{\partial q_2} = P(q_1+q_2) + q_2 P'(q_1+q_2) - C_2'(q_2) = 0,    
\end{aligned}
$$

上述两个最优化一阶条件定义了两个**反应函数**（reaction function），为

$$
\begin{aligned}
    q_1^* = R_1(q_2),   \\
    q_2^* = R_2(q_1).    
\end{aligned}
$$

下面就两类博弈信息获取情况进行分别讨论，一类是完全信息博弈，一类是不完全信息博弈。

# 完全信息博弈

## 求解

更加具体地，考虑一类上述库诺特模型的简单情况。假定每个企业具有相同的不变单位成本，即：$C_1(q_1)=q_1 c$ 和 $C_2(q_2)=q_2 c$。需求函数为 $P=a-(q_1+q_2)$。那么，两个企业的利润函数可以写为

$$
\pi_1(q_1,q_2) = q_1 (a-q_1-q_2) - q_1 c, 
$$

$$
\pi_2(q_1,q_2) = q_2 (a-q_1-q_2) - q_2 c.
$$


可以得到最优化一阶条件，为

$$
    \frac{\partial \pi_1}{\partial q_1} = (a-q_1-q_2)-q_1-c = 0,
$$

$$
    \frac{\partial \pi_2}{\partial q_2} = (a-q_1-q_2)-q_2-c = 0.  
$$

下面是求解过程，求得反应函数 $R_1(q_2)$ 和  $R_2(q_1)$。对于上式，把 $q_1$ 保留，把其他变量挪向等号右边，为

$$
2 q_1^* = a-q_2-c \\
\Rightarrow q_1^* = 0.5(a-q_2-c).
$$

同样地，对于上式，把 $q_2$ 保留，把其他变量挪向等号右边，为

$$
2 q_2^* = a-q_1-c \\
\Rightarrow q_2^* = 0.5(a-q_1-c).
$$

将上两式联立，求解得到 $q_1^\*$ 和 $q_2^\*$，这里可以手算得到，但是我们可以借助工具来求解等式。下面给出 MATLAB 代码，为

```matlab
clc;clear;

syms q1 q2 a c

eq1 = 0.5*(a-q2-c)-q1 == 0;
eq2 = 0.5*(a-q1-c)-q2 == 0;

[sol_q1 sol_q2] = solve(eq1,eq2,q1,q2);

disp(['The value of q1 is: ', char(sol_q1)]);
disp(['The value of q2 is: ', char(sol_q2)]);
```

最后得到纳什均衡，为

$$
q_1^* = q_2^* = \frac{1}{3} (a-c).
$$

将纳什均衡的值代入利润函数中，为

$$
\begin{aligned}
    \pi_1(q_1^*,q_2^*) &= \frac{1}{3}(a-c)(a-\frac{2}{3}a+\frac{2}{3}) - \frac{1}{3}(a-c)c  \\
    &= \frac{1}{3} (a-c)(\frac{1}{3} a + \frac{2}{3} c) - \frac{1}{3} ac + \frac{1}{3} c^2  \\
    &= \frac{1}{9} a^2 + \frac{2}{9} ac - \frac{1}{9} ac - \frac{2}{9} c^2 - \frac{1}{3} ac + \frac{1}{3} c^2   \\
    &= \frac{1}{9} a^2 - \frac{2}{9} ac + \frac{1}{9} c^2   \\
    &= \frac{1}{9} (a-c)^2.
\end{aligned}
$$

与垄断情况作比较，给出垄断企业问题命名，为

$$
\max_Q \pi = Q(a-Q-c).
$$

求解一阶最优性条件，为

$$
\frac{\partial \pi}{\partial Q} = a - 2Q -c = 0,
$$

求解得到

$$
Q^*_{\text{monopoly}} = \frac{1}{2}(a-c).
$$

那么垄断利润为

$$
\begin{aligned}
    \pi^*_{\text{monopoly}} &= \frac{1}{2}(a-c)(a-\frac{1}{2}a+\frac{1}{2}c-c) \\
    &= \frac{1}{2}(a-c) \cdot \frac{1}{2}(a-c)  \\
    &= \frac{1}{4}(a-c)^2
\end{aligned}
$$

## 结论

就产量而言，

1. 垄断企业的最优产量为 $Q^*_{\text{monopoly}} = \frac{1}{2} (a-c)$；
2. 寡头竞争中企业的最优产量为 $Q^*_{\text{Cournot}} = \frac{2}{3} (a-c)$。

即：$Q^\*_{\text{monopoly}} < Q^\*_{\text{Cournot}}$。

就利润而言，

1. 垄断企业的利润为 $\pi^*_{\text{monopoly}} = \frac{1}{4}(a-c)^2$；
2. 寡头竞争中企业的利润为 $\pi^*_{\text{Cournot}} = \frac{2}{9}(a-c)^2$。

即：$\pi^\*_{\text{monopoly}} < \pi^\*_{\text{Cournot}}$。

可以发现，垄断企业的产量低于寡头竞争的总产量，而利润高于寡头竞争中的总利润。


# 不完全信息博弈

## 介绍

上一节讨论的模型，其基础建立在博弈中所有参与人都已获知其他人的成本，即成本是所有参与人的共同知识。但是，现实中许多情况不满足这样的假设，即有些情况下无法获知对方的所有信息。例如，和一位陌生人打交道时，不知道对方阵营九宫格，对方到底是守序善良呢，还是混乱邪恶呢，我们不知道。

考虑一类和人交往的例子。

对方是守序善良的人，那么我们自己自然可以在与对方的交往中获得许多快乐；反之，对方是混乱邪恶的人，那么我们在与之交往的过程中不免难过。这里的快乐和难过可以抽象为支付函数。

在传统的博弈论视角里，我们似乎无法处理这样的情况。因为一个人无法既守序善良又混乱邪恶。我们只能够猜测他是守序善良的可能性，或者混乱邪恶的可能性。那么，海萨尼（Harsanyi）提出一种方法，引入“自然”（nature）来帮我们选择。我们可以考虑，当自然选择对方是守序善良时，我们应该怎么做；又或者，当自然选择对方是混乱邪恶时，我们应该怎么做。

这样就解决了一个人无法既守序善良又混乱邪恶的难题。

上述过程称为海萨尼转换（Harsanyi transformation）。

通过海萨尼转换，最终得到的纳什均衡称为贝叶斯纳什均衡（Baysian Nash equilibrium）。

## 求解

继续考虑上一节的库诺特模型，但将条件改为不完全信息条件，即博弈过程中，有企业1 和企业2，企业2 有高成本和低成本两类。

那么，两个企业的利润函数为

$$
\pi_1(q_1,q_2) = q_1 (a-q_1-q_2) - q_1 c_1,
$$

$$
\pi_2^L(q_1,q_2^L) = q_2 (a-q_1-q_2^L) - q_2 c_2^L.
$$

$$
\pi_2^H(q_1,q_2) = q_2 (a-q_1-q_2^H) - q_2 c_2^H.
$$

式中，上标 $L$ 代表低成本企业2， $H$ 代表高成本企业2。

假定企业1 的成本 $c_1$ 为共同知识，企业2 的成本可能是 $c_2^L$，也可能是 $c_2^H$，$c_2^L < c_2^H$。企业2 知道自己的成本是 $c_2^L$ 还是$c_2^H$，但企业1 只知道 $c_2=c_2^L$ 的可能性为 $\mu$，$c_2=c_2^H$ 的可能性为 $(1-\mu)$，$\mu$ 为共同知识。

考虑一类简单情况，给定参数如下。

$$
a=2,c_1=1,c_2^L=\frac{3}{4},c_2^H=\frac{5}{4},\mu=\frac{1}{2}.
$$

那么，企业1 的利润函数为

$$
\pi_1 = q_1(1-q_1-q_2),
$$

企业2 的利润函数为

$$
\pi_2^L = q_2(\frac{5}{4}-q_1-q_2),
$$

或

$$
\pi_2^H = q_2(\frac{3}{4}-q_1-q_2),
$$

求企业2 的反应函数，为

$$
\frac{\partial \pi_2^L}{\partial q_2} = (\frac{5}{4}-q_1-q_2)-q_2=0,
$$

或

$$
\frac{\partial \pi_2^H}{\partial q_2} = (\frac{3}{4}-q_1-q_2)-q_2=0.
$$

对于上两式，把 $q_2$ 保留，把其他变量挪向等号右边，为

$$
q_2^{L*} = \frac{1}{2}(\frac{5}{4}-q_1),
$$

或

$$
q_2^{H*} = \frac{1}{2}(\frac{3}{4}-q_1),
$$

那么，企业1 选择产量 $q_1$，需要最大化下列利润期望函数，为

$$
E \pi_1 = \frac{1}{2} q_1 (1-q_1-q_2^{L*}) + \frac{1}{2} q_1 (1-q_1-q_2^{H*}).
$$

一阶最优化条件为

$$
\begin{aligned}
    \frac{\partial E \pi_1}{\partial q_1} &= \frac{1}{2} (1-q_1-q_2^{L*}) - \frac{1}{2} q_1 + \frac{1}{2} (1-q_1-q_2^{H*}) - \frac{1}{2} q_1 \\
    &= \frac{1}{2} -\frac{1}{2} q_1 - \frac{1}{2} q_2^{L*} - \frac{1}{2} q_1 + \frac{1}{2} - \frac{1}{2} q_1 - \frac{1}{2} q_2^{H*} - \frac{1}{2} q_1   \\
    &= 1 - 2 q_1 - \frac{1}{2} (q_2^{L*}+q_2^{H*}) = 0.
\end{aligned}
$$

企业1 的最优产量为

$$
q_1^* = \frac{1}{2}(1-\frac{1}{2}q_2^{L*}-\frac{1}{2}q_2^{H*}).
$$

联立 $q_1^*$ 、$q_2^{L*}$ 和 $q_2^{H*}$ 的表达式，可以得到贝叶斯均衡为

$$
q_1^*=\frac{1}{3},q_2^{L*} = \frac{11}{24},q_2^{H*}=\frac{5}{24}.
$$

上面考虑的情况是企业1 不知道企业2 成本的情况。

再考虑一类企业2 为低成本的情况。

企业1 的反应函数为

$$
q_{1NE}^{L*} = \frac{1}{2}(1-q_2^{L*}),
$$

企业2 的反应函数为

$$
q_{2NE}^{L*} = \frac{1}{2}(\frac{5}{4}-q_1),
$$

那么，纳什均衡为

$$
q_{1NE}^{L*}=\frac{1}{4},q_{2NE}^{L*} = \frac{1}{2}.
$$

最后考虑一类企业2 为高成本的情况。


企业1 的反应函数为

$$
q_{1NE}^{H*} = \frac{1}{2}(1-q_2^{L*}),
$$

企业2 的反应函数为

$$
q_{2NE}^{H*} = \frac{1}{2}(\frac{3}{4}-q_1),
$$

那么，纳什均衡为

$$
q_{1NE}^{H*}=\frac{5}{12},q_{2NE}^{H*} = \frac{1}{6}.
$$

上述求解过程可以通过 MATLAB 代码求解得到，代码如下。

```matlab
clc;clear;close all;

syms q1 q2_L q2_H
syms a c1 c2_L c2_H

cost = 'high-and-low';
% cost = 'low';
% cost = 'high';

if strcmp(cost,'high-and-low')
%% 求解方程组（高、低成本都算进去）
    eq1 = 0.5*(0.75-q1)-q2_H == 0;
    eq2 = 0.5*(1.25-q1)-q2_L == 0;
    eq3 = 0.5*(1-0.5*q2_L-0.5*q2_H)-q1 == 0;

    [sol_q1, sol_q2_L, sol_q2_H] = solve(eq1, eq2, eq3, q1, q2_L, q2_H);
    
    pi1 = 0.5*sol_q1*(1-sol_q1-sol_q2_L) +  0.5*sol_q1*(1-sol_q1-sol_q2_H);
    pi2_L = sol_q2_L*(1.25-sol_q1-sol_q2_L);
    pi2_H = sol_q2_H*(0.75-sol_q1-sol_q2_H);

    disp(['The value of q1 is: ',char(sol_q1)]);
    disp(['The value of q2_L is: ', char(sol_q2_L)]);
    disp(['The value of q2_H is: ', char(sol_q2_H)]);
    
    disp(['The payoff of q1 is: ',char(pi1)]);
    disp(['The payoff of q2_L is: ', char(pi2_L)]);
    disp(['The payoff of q2_H is: ', char(pi2_H)]);
elseif strcmp(cost,'low')
%% 求解方程组（仅算低成本）
    eq1 = 0.5*(1.25-q1)-q2_L == 0;
    eq2 = 0.5*(1-q2_L)-q1 == 0;

    [sol_q1, sol_q2_L] = solve(eq1, eq2, q1, q2_L);
    
    pi1 = sol_q1*(1-sol_q1-sol_q2_L);
    pi2_L = sol_q2_L*(1.25-sol_q1-sol_q2_L);

    disp(['The value of q1 is: ',char(sol_q1)]);
    disp(['The value of q2_L is: ', char(sol_q2_L)]);
    
    disp(['The payoff of q1 is: ',char(pi1)]);
    disp(['The payoff of q2_L is: ', char(pi2_L)]);
elseif strcmp(cost,'high')
%% 求解方程组（仅算高成本）
    eq1 = 0.5*(0.75-q1)-q2_H == 0;
    eq2 = 0.5*(1-q2_H)-q1 == 0;

    [sol_q1, sol_q2_H] = solve(eq1, eq2, q1, q2_H);
    
    pi1 = sol_q1*(1-sol_q1-sol_q2_H);
    pi2_H = sol_q2_H*(0.75-sol_q1-sol_q2_H);

    disp(['The value of q1 is: ',char(sol_q1)]);
    disp(['The value of q2_H is: ', char(sol_q2_H)]);
    
    disp(['The payoff of q1 is: ',char(pi1)]);
    disp(['The payoff of q2_H is: ', char(pi2_H)]);
else
    error('The selected COST is wrong!')
end
```

## 结论

对于本节讨论的三类情况，企业1 不知道企业2 的成本，企业1 知道企业2 为低成本和企业1 知道企业2 为高成本，得到的贝叶斯均衡/纳什均衡总结如下。

$$
\begin{aligned}
    &q_{1NE}^{L*} = \frac{1}{4} < q_1^* = \frac{1}{3}, \quad q_{1NE}^{H*} = \frac{5}{12} > q_1^* = \frac{1}{3}, \\
    &q_{2NE}^{L*} = \frac{1}{2} > q_2^{L*} = \frac{11}{24}, \quad q_{2NE}^{H*} = \frac{1}{6} < q_2^{H*} = \frac{5}{24}.
\end{aligned}
$$

各企业利润总结如下。

$$
\begin{aligned}
    &\pi_1^* = \frac{1}{9}, \quad \pi_2^{L*} = \frac{121}{576}, \quad \pi_2^{H*} = \frac{25}{576},   \\
    &\pi_{1NE}^{L*} = \frac{1}{16}, \quad \pi_{2NE}^{L*} = \frac{1}{4}, \\
    &\pi_{1NE}^{H*} = \frac{25}{144}, \quad \pi_{2NE}^{H*} = \frac{1}{36}.
\end{aligned}
$$

写得更加易读一些，为

$$
\begin{aligned}
    &\pi_1^* = 0.1111..., \quad \pi_2^{L*} = 0.2101, \quad \pi_2^{H*} = 0.0434,   \\
    &\pi_{1NE}^{L*} = 0.0625, \quad \pi_{2NE}^{L*} = 0.25, \\
    &\pi_{1NE}^{H*} = 0.1736, \quad \pi_{2NE}^{H*} = 0.0278.
\end{aligned}
$$

对于企业1 而言，在不完全信息条件下，
1. 企业2 为低成本时，企业1 的产量相较完全信息下企业1 的产量更高，利润更高；
>
>$$
>q_1^* = \frac{1}{3} > q_{1NE}^{L*} = \frac{1}{4}
>$$
>
>$$
>\pi_1^* = 0.1111... > \pi_{1NE}^{L*} = 0.0625
>$$
>
2. 企业2 为高成本时，企业1 的产量相较完全信息下企业1 的产量更低，利润更低。
>
>$$
>q_1^* = \frac{1}{3} < q_{1NE}^{H*} = \frac{5}{12}
>$$
>
>$$
>\pi_1^* = 0.1111... < \pi_{1NE}^{H*} = 0.1736
>$$
>

对于企业2 而言，在不完全信息条件下，
1. 企业2 为低成本时，其产量相较完全信息下企业2 的产量更低，利润更低；
>
>$$
>q_2^{L*} = \frac{11}{24} < q_{2NE}^{L*} = \frac{1}{2}
>$$
>
>$$
>\pi_2^{L*} = 0.2101 < \pi_{2NE}^{L*} = 0.25
>$$
>
2. 企业2 为高成本时，其产量相较完全信息下企业2 的产量更高，利润更高。
>
>$$
>q_2^{H*} = \frac{5}{24} > q_{2NE}^{H*} = \frac{1}{6}
>$$
>
>$$
>\pi_2^{H*} = 0.0278 > \pi_{2NE}^{H*} = 0.1736
>$$
>

# 最后

欢迎通过邮箱联系我：lordofdapanji@foxmail.com

来信请注明你的身份，否则恕不回信。

参考文献：

[1] 张维迎. 博弈论与信息经济学 [M]. 上海: 上海人民出版社, 2013: 43-45, 143-149.