---
permalink: /:year-:month-:day/:title
layout: post
title:  "关于不完全信息博弈均衡求解的记录"
author: "Lei Lie"
categories: Study
tag: notdouban 
---

参考文献：《运筹学》教材编写组，运筹学（第四版），北京：清华大学出版社，2012，450--466.

# 矩阵对策的基本定理

> **定理4** 设$x^\* \in S_1^\*$，$y^\* \in S_2^*$，则$(x^\*,y^\*)$为$G$的解的充要条件是：存在数$v$，使得$x^\*$和$y^\*$分别是不等式组（I）和（II）的解，且$v = V_G$。
> 
> $$
> (\text{I}) \ \left \{ \begin{array}{l}
> 	\mathop{\sum}\limits_i a_{ij} x_i \ge v, \quad j = 1,\cdots,n	\\
> 	\mathop{\sum}\limits_i x_i = 1	\\
> 	x_i \ge 0, \quad i = 1,\cdots,m
> \end{array}\right.
> $$
>
> $$
> (\text{II}) \ \left \{ \begin{array}{l}
> 	\mathop{\sum}\limits_j a_{ij} y_j \le v, \quad i = 1,\cdots,m	\\
> 	\mathop{\sum}\limits_j y_j = 1	\\
> 	y_j \ge 0, \quad j = 1,\cdots,n
> \end{array}\right.
> $$

> **定理5** 对任一矩阵对策$G=\{S_1,S_2;\boldsymbol{A}\}$，一定存在混合策略意义下的解。

> **定理7** 设有两个矩阵对策
> 
> $$
> G_1 = \{ S_1, S_2; \boldsymbol{A}_1 \}\\
> G_2 = \{ S_1, S_2; \boldsymbol{A}_2 \}
> $$
> 
> 其中$\boldsymbol{A}_1 = (a_{ij})$，$\boldsymbol{A}_2 = (a_{ij}+L)$，$L$为任一常数，则有
>
> （1）$V_{G_2}=V_{G_1} + L$
>
> （2）$T(G_1) = T(G_2)$

> **定理11** 设矩阵对策$G=\{S_1,S_2;\boldsymbol{A}\}$的值为$V_G$，则
> 
> $$
> V_G = \max_{x \in S_1^*} \min_{1 \le j \le n} E(x,j) = \min_{y \in S_2^*} \max_{1 \le i \le m} E(i,j)
> $$

**上述定理证明略，详见《运筹学》第451-452、463页。**

# 线性规划方法求解混合策略

对于博弈值

$$
v = \max_{x \in S_1^*} \min_{y \in S_2^*} E(x,y) = \min_{y \in S_2^*} \max_{x \in S_1^*} E(x,y)
$$

而言，其最终值$V_G = v$。

> 自己的思考：从博弈值的式子来看，是X方要令博弈值最大，Y方要令博弈值最小。谨记问题（I）——>问题（P），即令博弈值最大；问题（II）——>问题（D），即令博弈值最小。
>
> **对应关系不要搞错了！！！**

又由**定理5**已知，对任一矩阵对策$G=\{S_1,S_2;\boldsymbol{A}\}$的求解均等价于一对互为对偶的线性规划问题，而**定理4**表明，对策$G$的解$x^*$和$y^*$等价于下面两个不等式组的解。

$$
(\text{I}) \ \left \{ \begin{array}{l}
	\mathop{\sum}\limits_i a_{ij}x_i \ge v, \quad j = 1,\cdots,n	\\
	\mathop{\sum}\limits_i x_i = 1	\\
	x_i \ge 0, \quad i = 1,\cdots,m
\end{array}\right.
$$

$$
(\text{II}) \ \left \{ \begin{array}{l}
	\mathop{\sum}\limits_j a_{ij}y_j \le v, \quad i = 1,\cdots,m	\\
	\mathop{\sum}\limits_j y_j = 1	\\
	y_j \ge 0, \quad j = 1,\cdots,n
\end{array}\right.
$$

---

下面给出求解矩阵对策的线性规划方法。

根据**定理7**，设$v > 0$，作变换

$$
x_i' = x_i / v, \quad i = 1,\cdots,m
$$

则不等式组（I）变为

$$
(\text{I}) \ \left \{ \begin{array}{l}
	\mathop{\sum}\limits_i a_{ij} x_i' \ge 1, \quad j = 1,\cdots,n	\\
	\mathop{\sum}\limits_i x_i' = 1/v	\\
	x_i \ge 0, \quad i = 1,\cdots,m
\end{array}\right.
$$

根据**定理11**，有$v = \mathop{\max}\limits_{x \in S_1^*} \mathop{\min}\limits_{1 \le j \le n} \mathop{\sum}_i a_{ij} x_i$。

那么，不等式组（I）等价于线性规划问题

$$
(\text{P}) \ \left \{ \begin{array}{l}
	\min z = \mathop{\sum}\limits_i x_i'\\
	\mathop{\sum}\limits_i a_{ij} x_i' \ge 1, \quad j = 1,\cdots,n	\\
	x_i' \ge 0, \quad i = 1,\cdots,m
\end{array}\right.
$$

同理，作变换

$$
y_j' = y_j / v,\quad j = 1,\cdots,n
$$

根据**定理11**，有$v = \mathop{\min}\limits_{y \in S_2^*} \mathop{\max}\limits_{1 \le i \le m} \mathop{\sum}\limits_j a_{ij} y_j$。xm

那么，不等式组（II）等价于线性规划问题

$$
(\text{D}) \ \left \{ \begin{array}{l}
	\max w = \mathop{\sum}\limits_j y_j'\\
	\mathop{\sum}\limits_i a_{ij} y_j' \le 1, \quad i = 1,\cdots,m	\\
	y_j' \ge 0, \quad j = 1,\cdots,n
\end{array}\right.
$$

可以看出，问题（P）和问题（D）是互为对偶的线性规划问题，故可利用单纯形或者对偶单纯形方法求解。在求解时，一般先求问题（D）的解，易于在迭代的第一步找到第一个基本可行解，而问题（P）的解从问题（D）的最后一个单纯形表上即可得到。当求得问题（P）和问题（D）的解后，再利用变换式$x_i' = x_i / v, \quad i = 1,\cdots,m$和$y_j' = y_j / v,\quad j = 1,\cdots,n$，即可求出原博弈问题的解及博弈的值。

> 自己的思考：
>
> 使用Matlab的`linprog()`函数进行求解，设置线性规划求解方法为对偶单纯形，即
>
> ```matlab
> options = optimoptions('linprog', 'Algorithm', 'dual-simplex', 'Display', 'off');
> ```

# 例题

**例18** 利用线性规划方法，求解赢得矩阵为$\boldsymbol{A}$的矩阵对策。

$$
\boldsymbol{A} = \begin{bmatrix}
7 & 2 & 9 \\
2 & 9 & 0 \\
9 & 0 & 11
\end{bmatrix}
$$

**解** 依据上述理论，将该问题化成两个互为对偶的线性规划问题，为

$$
\min(x_1+x_2+x_3)\\
(\text{P}) \ \left \{ \begin{array}{l}
	7 x_1 + 2 x_2 + 9 x_3 \ge 1 \\
	2 x_1 + 9 x_2  \ge 1 \\
	9 x_1 + 11 x_3 \ge 1 \\
	x_1, x_2, x_3 \ge 0
\end{array}\right.
$$

$$
\max(y_1+y_2+y_3) \\
(\text{P}) \ \left \{ \begin{array}{l}
    7 y_1 + 2 y_2 + 9 y_3 \ge 1 \\
    2 y_1 + 9 y_2  \ge 1 \\
    9 y_1 + 11 y_3 \ge 1 \\
    y_1, y_2, y_3 \ge 0
\end{array}\right.
$$

根据题意，写出Matlab代码，为

```matlab
clc;clear;

% 赢得矩阵
U = [7 2 9
     2 9 0
     9 0 11];

%% 求X方
f_x = ones(3,1);    % min(x1+x2+x3)
A_x = -U;           % eg.-( 7 x1 + 2 x2 + 9 x3 ) <= -1
b_x = -ones(3,1);   % 记得加负号
Aeq_x = [];
beq_x = [];
lb_x = zeros(1,3);
ub_x = ones(1,3);
% 求解线性规划
options = optimoptions('linprog', 'Algorithm', 'dual-simplex', 'Display', 'off');
[res, fval, exitFlag_x] = linprog(f_x, A_x, b_x, Aeq_x, beq_x, lb_x, ub_x, options);
F_star_D = res/fval;
disp(F_star_D);

%% 求Y方
f_y = -ones(3,1);   % max(y1+y2+y3)
A_y = U;            % 别加负号
b_y = ones(3,1);    % 别加负号
Aeq_y = [];
beq_y = [];
lb_y = zeros(1,3);
ub_y = ones(1,3);
% 求解线性规划
options = optimoptions('linprog', 'Display', 'off');
[res, fval, exitFlag_y] = linprog(f_y, A_y, b_y, Aeq_y, beq_y, lb_y, ub_y, options);
F_star_A = -(res/fval);
disp(F_star_A);
```

得到的结果，为

$$
(x_1,x_2,x_3) = (0.25,0.5,0.25)\\
(y_1,y_2,y_3) = (0.25,0.5,0.25)
$$

与例题结果一致。

> 自己的思考：这道例题里，$\boldsymbol{A}$是一个对称矩阵，所以怎么乘都可以。一般来说，我的习惯是每行代表防御者的策略，每列代表攻击者的策略。所以使用`linprog()`时，要注意到我们要优化的变量`x1,x2,x3,x4,...`是一个`Nx1`的向量，求解防御者的策略时，记得把博弈收益矩阵转置；求解攻击者的策略时，不用转置。