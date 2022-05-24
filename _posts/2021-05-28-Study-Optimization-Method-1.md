---
permalink: /:year-:month-:day/:title
layout: post
title:  "最优化方法——乘子法和外点罚函数法"
author: "Lei Lie"
categories: Study
tag: notdouban
---

此题摘自文件 最优化方法 第三章（乘子法） 13/24

【题目】分别用**外点法**和**乘子法**求等式约束问题

$$
\mathrm{min} \quad \frac{1}{2}x^2_1+\frac{1}{6}x^2_2	\\
\mathrm{s.t.} \quad x_1+x_2=1
$$

【解】（外点罚函数法）① 构造罚函数

$$
F(x,M_k)=\frac{1}{2}x^2_1+\frac{1}{6}x^2_2+M_k(x_1+x_2-1)^2
$$

② 求偏导

$$
\frac{\partial F}{\partial x_1}=x_1+2M_k(x_1+x_2-1)=0
\tag{1}
$$

$$
\frac{\partial F}{\partial x_1}=\frac{1}{3}x_2+2M_k(x_1+x_2-1)=0
\tag{2}
$$

③ 联立两个偏导式，求驻点，并得到$x_1$和$x_2$的表达式

联立$(1)$和$(2)$，得到

$$
x_2=3x_1
\tag{3}
$$

将$(3)$代回$(1)$，得到

$$
\begin{aligned}
x_1+2M_k(4x_1-1)&=0	\\
\Rightarrow (1+8M_k)x_1&=2M_k	\\
\Rightarrow x_1&=\frac{2M_k}{1+8M_k}
\end{aligned}
$$

根据$(3)$，得到

$$
x_2=3x_1=\frac{6M_k}{1+8M_k}
$$

将$x_1$和$x_2$的表达式改写为

$$
x^*=\begin{bmatrix}
\frac{2}{\frac{2}{M_k}+8}&\frac{6}{\frac{2}{M_k}+8}
\end{bmatrix}^\mathrm{T}
\tag{4}
$$

④ 令$M_k \to \infty$，得到结果

$$
x^*=\begin{bmatrix}
\frac{1}{4}&\frac{3}{4}
\end{bmatrix}^\mathrm{T}
$$

【总结】

解题步骤如下：

① 构造罚函数；

② 求出对$x_1$和$x_2$的罚函数偏导；

③ 联立两个偏导式，求出驻点，并代回偏导式，得到$x^k_1$和$x^k_2$的表达式；

④ 令$M_k$趋近无穷，得到$x^*$。

---

【解】（乘子法）

① 写出增广Lagrange函数；

$$
\varphi(x,v^k)=\frac{1}{2}x^2_1+\frac{1}{6}x^2_2+v^k(x_1+x_2-1)+\frac{c_k}{2}(x_1+x_2-1)^2
$$

② 用解析法求驻点；

$$
\frac{\partial \varphi}{\partial x_1} = x_1+v^k+c(x_1+x_2-1)=0
\tag{5}
$$

$$
\frac{\partial \varphi}{\partial x_2} = \frac{1}{3}x_1+v^k+c(x_1+x_2-1)=0
\tag{6}
$$

联立$(5)$和$(6)$，得到

$$
x_2=3x_1
\tag{7}
$$

将$(7)$代入$(5)$中，得到

$$
\begin{aligned}
x_1+v^k+c(x_1+3x_1-1) &= 0	\\
\Rightarrow (4c+1)x_1+(v^k-c) &= 0	\\
\Rightarrow x_1 &= \frac{c-v^k}{4c+1}
\end{aligned}
$$

那么，$x_2$为

$$
x_1 = \frac{3(c-v^k)}{4c+1}
$$

③ 根据乘子迭代公式求下一步的$v^{k+1}$

根据乘子迭代公式，有

$$
\begin{aligned}
v^{k+1} &= v^k+c(x_1+x_2-1)	\\
& = v^k + c(\frac{4c-4v^k}{4c+1}+\frac{4c+1}{4c+1})	\\
& = v^k + c(\frac{1+4v^k}{4c+1})
\end{aligned}
$$

④ $c$取任意值，解出$v^k$的值；

令$c=1$，且$v^k \to \infty$，得到

$$
\begin{aligned}
v^* &= v^* - \frac{1+4v^*}{5}	\\
\Rightarrow v^* &= -0.25
\end{aligned}
$$

⑤ 将$c$和$v^k$代入点$x^k$中，得出结果

$$
x^k_1 = \frac{1+\frac{1}{4}}{5}=\frac{1}{4},\quad x^k_2 = 3x^k_1 = \frac{3}{4}
$$

即

$$
x^*=\begin{bmatrix}
\frac{1}{4} & \frac{3}{4}
\end{bmatrix}^\mathrm{T}
$$

【总结】

解题步骤如下：

① 写出增广Lagrange函数；

② 用解析法求驻点；

③ 根据乘子迭代公式求下一步的$v^{k+1}$

④ $c$ 取任意值，解出 $v^k$ 的值；

⑤ 将 $c$ 和 $v^k$ 代入点 $x^k$ 中，得出结果。