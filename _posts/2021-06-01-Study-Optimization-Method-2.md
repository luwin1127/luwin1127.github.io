---
permalink: /:year-:month-:day/:title
layout: post
title:  "一般性约束问题的最优性条件例题"
author: "Lei Lie"
categories: Study
tag: notdouban
---

本题摘自《最优化方法及其Matlab程序设计 第一版》 123(130/271)

【例22】考虑优化问题

$$
\mathrm{min} f(x) = -2x^2_1-x^2_1 \ ,	\\
\mathrm{s.t.} \quad x^2_1+x^2_2-2=0	\ ,\\
-x_1+x_2 \ge 0 \ ,	\\
x_1 \ge 0 \ , x_2 \ge 0.
$$

试验证$x^*=(1,1)^\mathrm{T}$为$KT$点，并求出问题的$KT$对。

【解】记

$$
\begin{aligned}
f(x) &= -2x^2_1-x^2_1,\\
h(x) &= x^2_1+x^2_2-2,\\
g_1(x) &= -x_1+x_2,	 \\
g_2(x) &= x_1,		 \\
g_3(x) &= x_2.
\end{aligned}
$$

求梯度，得到

$$
\begin{aligned}
\bigtriangledown f(x) &= \begin{bmatrix}
-4x_1	\\
-2x_2
\end{bmatrix}, \quad \bigtriangledown h(x) = \begin{bmatrix}
2x_1	\\
2x_2
\end{bmatrix},	\\
\bigtriangledown g_1(x) &= \begin{bmatrix}
-1	\\
1
\end{bmatrix}, \quad \bigtriangledown g_2(x) = \begin{bmatrix}
1	\\
0
\end{bmatrix}, \quad \bigtriangledown g_3(x) = \begin{bmatrix}
0	\\
1
\end{bmatrix}.
\end{aligned}
$$

把$x^*=(1,1)^\mathrm{T}$代入上面5个式子，由$KT$条件有，

**注意**：这里一定注意要加负号！！！！

$$
\left\{\begin{matrix}
-4-2\mu+\omega_1-\omega_2 = 0	\\
-2-2\mu-\omega_1+\omega_3 = 0
\end{matrix}\right.
\tag{1}
$$

因为

$$
\left\{\begin{matrix}
\omega_2\bigtriangledown g_2(\bar{x}) = 0	\\
\omega_3\bigtriangledown g_3(\bar{x}) = 0
\end{matrix}\right.
\Rightarrow
\left\{\begin{matrix}
\omega^*_2 = 0	\\
\omega^*_3 = 0
\end{matrix}\right.
$$

所以$(1)$变为

$$
\left\{\begin{matrix}
-4-2\mu+\omega_1 = 0	\\
-2-2\mu-\omega_1 = 0
\end{matrix}\right.
\tag{2}
$$

求解$(2)$式，得到

$$
\left\{\begin{matrix}
\begin{aligned}
\mu^* &= -1.5	\\
\omega^*_1 &= 1
\end{aligned}
\end{matrix}\right.
$$

这表明 $x^\{*}$ 是 $KT$ 点，$(x^\*,(\mu^\*,\omega^\*))$是 $KT$ 对，其中，$\mu^\*=-1.5$，$\omega^\*=(1,0,0)^\mathrm{T}$。