---
permalink: /:year-:month-:day/:title
layout: post
title:  "古诺（Cournot）竞争博弈模型 matlab仿真代码实现"
author: "Lei Lie"
categories: Study,Code
tag: notdouban
---

古诺竞争模型（也称古诺模型）是早期的寡头垄断模型。它是法国经济学家古诺于1838年提出的。古诺竞争模型通常被作为寡头理论分析的出发点。古诺模型是一个只有两个寡头厂商的简单模型，该模型也被称为“双头模型”。古诺模型的结论可以很容易地推广到在三个或三个以上的寡头垄断厂商的情况中去。

古诺模型的假定是：市场上有A、B两个厂商生产和销售相同的产品，它们的生产成本为零；它们共同面临的市场的需求曲线是线性的，A、B两个厂商都准确地了解市场的需求曲线；A、B两个厂商都是在已知对方产量的情况下，各自确定能够给自己带来最大利润的产量，即每一个厂商都是消极地以自己的产量去适应对方已确定的产量。

设市场的需求函数为

$$
D=D(p_1+p_2)=a-b(p_1+p_2)
$$

其中$p_1$和$p_2$分别为两个企业的产量。假设两企业的成本函数相同，都为$C=c_0p$（$p$为产量），则企业1在预测企业2的产量为$p_2$的情况下，寻求使自己利润最大化的最优产量$p_1$，即

$$
\max \quad p_1[a-b(p_1+p_2)]-cp_1
$$

上面优化模型中的最优解的$p_1$显然是$p_2$的函数$p_1=f(p_2)$；

同样企业2在预测企业1的产量为$p_1$的情况下，寻求使自己利润最大化的最优产量$p_2$，即

$$
\max \quad p_2[a-b(p_1+p_2)]-cp_2
$$

上面优化模型中的最优解的$p_2$显然是$p_1$的函数$p_2=g(p_1)$；

同时满足下面方程的$(p_1,p_2)$称为Cournot均衡：

$$
\left\{\begin{matrix}
p_1 = f(p_2)	\\
p_2 = g(p_1)
\end{matrix}\right.
$$

根据一阶最优性条件可以得到如下等式

$$
p^*_1=p^*_2=\frac{a-c}{3b}
\tag{1}
$$

这个结果是分别求解两个优化模型得到的，即

（1）$\max \quad p^{*}_1[a-b(p^\*_1+p_2)]-cp^\*_1$，得到$p^\*_1=\frac{a-bp_2-c}{2b}$；

（2）$\max \quad p^{*}_2[a-b(p_1+p^\*_2)]-cp^\*_2$，得到$p^\*_2=\frac{a-bp_1-c}{2b}$。

从而得到式$(1)$。

【例题】Cournot竞争模型应用实例。设

$$
\left\{\begin{matrix}
\begin{aligned}
a &= 61.2	\\
b &= 10		\\
c &= 1.2
\end{aligned}
\end{matrix}\right.
$$

即市场的需求函数为$D=61.2-10(p_1+p_2)$，两企业的成本函数都为$C_i=1.2p_i(i=1,2)$，求Cournot均衡时两个企业的产量。

【解】由优化模型得到

企业1的优化模型为

$$
\max \quad p^*_1[61.2-10(p^*_1+p_2)]-1.2p^*_1
$$

其最优产量为

$$
p^*_1 = \frac{60-10p_2}{20} =\frac{6-p_2}{2}
$$

企业2的优化模型为

$$
\max \quad p^*_2[61.2-10(p_1+p^*_2)]-1.2p^*_2
$$

其最优产量为

$$
p^*_2 = \frac{60-10p_1}{20} =\frac{6-p_1}{2}
$$

则Cournot均衡时两个企业的产量为

$$
p^{*}_1 = p^{*}_2 = \frac{a-c}{3b} = \frac{61.2-1.2}{3 \times 10} = 2
$$

【代码】matlab仿真代码在下面给出。

```python
clear;clc;close all;
x0 = [2*rand,2*rand];
global a b c
a = 61.2;
b = 10;
c = 1.2;
Aeq = [2*b b
        b 2*b];
beq = [a-c
       a-c];
options = optimoptions(@fmincon,'Display','iter-detailed','Algorithm','sqp');
[x, fval, exitFlag, output] = fmincon(@Cournot, x0, [], [], Aeq, beq, zeros(1,2), Inf*ones(1,2), [], options);
fprintf('均衡点为【%.4f】和【%.4f】\n',x(1),x(2));

function f = Cournot(x)
global a b c
p1 = x(1);
p2 = x(2);
f = -1 * (p1*(a-b*(p1+p2)) - c*p1);
end
```

`fmincon()`的使用说明见MathWork官网（[这里](https://ww2.mathworks.cn/help/optim/ug/fmincon.html?searchHighlight=fmincon&s_tid=srchtitle)）

`optimoptions`里面设置了展示迭代过程中的每一次取值，且优化算法选择序列二次规划算法（Sequence Quadratic Programing, SQP），即

```python
options = optimoptions(@fmincon,'Display','iter-detailed','Algorithm','sqp');
```

仿真结果为

```python
 Iter  Func-count            Fval   Feasibility   Step Length       Norm of   First-order  
                                                                       step    optimality
    0           3   -3.588266e+01     2.318e+01     1.000e+00     0.000e+00     2.318e+01  
    1           6   -4.000000e+01     7.105e-15     1.000e+00     1.037e+00     2.225e+01  
    2           9   -4.000000e+01     7.105e-15     1.000e+00     5.296e-16     3.553e-15  

Optimization completed: The relative first-order optimality measure, 1.776357e-16,
is less than options.OptimalityTolerance = 1.000000e-06, and the relative maximum constraint
violation, 3.064698e-16, is less than options.ConstraintTolerance = 1.000000e-06.

Optimization Metric                                            Options
relative first-order optimality =   1.78e-16       OptimalityTolerance =   1e-06 (default)
relative max(constraint violation) =   3.06e-16    ConstraintTolerance =   1e-06 (default)

均衡点为【2.0000】和【2.0000】
```

CSDN上还[有篇文章](https://blog.csdn.net/cclethe/article/details/80426760?ops_request_misc=&request_id=&biz_id=102&utm_term=Cournot%E6%A8%A1%E5%9E%8B%20%E4%BB%A3%E7%A0%81&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-.pc_search_result_control_group&spm=1018.2226.3001.4187)也是讲古诺模型的，他的代码我运行了一下，得到的均衡点是一样的。不过作者的逻辑我没有理解，为什么要加这行代码来判断呢？

```python
if abs(z-z_old)<0.0001 && abs(y-y_old)<0.0001
	break;
end
```

没有太明白。不知道为什么满足这个条件之后就可以输出了。但是结果是一样的，可能还是我没有理解透彻吧。