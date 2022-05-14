---
permalink: /:year-:month-:day/:title
layout: post
title:  "一个简单的微分对策问题"
author: "Lei Lie"
categories: Study
tag: notdouban
---

# 理论知识

---
注：本文为《最优控制方法与Matlab实现》第八章 微分对策问题的实现。给出了文中例题的解法，以及matlab仿真代码，解题步骤和matlab均按照我习惯的方式给出。

全文的公式使用[LaTex公式编辑器](https://www.latexlive.com/)编辑而成。

---

首先对微分对策的一般形式进行介绍。

设 *P* 、*E* 是对策双方，动态系统状态方程为

$$
\dot {\boldsymbol x}(t)=\boldsymbol f[\boldsymbol x(t),\boldsymbol u(t),\boldsymbol v(t),t]
$$

给定初始状态为

$$
\boldsymbol x(t)=\boldsymbol x_0
$$

式中，$\boldsymbol x(t)$ 为 $n$ 维 $P$ 、$E$ 双方的状态变量；$\boldsymbol u(t)$ 为 $m_u$ 维 *P* 方控制变量，$\boldsymbol v(t)$ 为$m_v$ 维 $E$ 方控制变量，$\boldsymbol u(t)$ 和$\boldsymbol v(t)$的各分量 $u_i(t)$ 和 $v_i(t)$ 均为 $t$ 的分段连续函数， $[\boldsymbol u(t),\boldsymbol v(t)]$ 为容许控制策略或简称控制策略；$\boldsymbol f(\cdot)$为 $n$ 维连续可微的向量函数；$t \in [t_0,t_f]$ 。取性能指标为

$$
J[\boldsymbol u(t),\boldsymbol v(t)]=\theta[\boldsymbol x(t_f),t_f]+\int_{t_0}^{t_f}L[\boldsymbol x(t),\boldsymbol u(t),\boldsymbol v(t),t]\mathrm{d}t
$$

**【纳什-庞特里亚金最大最小原理】**

定义Hamilton函数如下：

$$
H[\boldsymbol x(t),\boldsymbol u(t),\boldsymbol v(t),\boldsymbol \lambda(t),t]=L[\boldsymbol x(t),\boldsymbol u(t),\boldsymbol v(t),t]+\boldsymbol \lambda^{\mathrm T} \boldsymbol f[\boldsymbol x(t),\boldsymbol u(t),\boldsymbol v(t),t]
$$

【定理】 若 $\boldsymbol u^*(t)$ 和 $\boldsymbol v^*(t)$ 是最优控制，则有：

（1）状态变量 $\boldsymbol x(t)$ 与协态变量 $\boldsymbol \lambda(t)$ 满足以下协态方程

$$
\dot {\boldsymbol x}(t)=\frac{\partial H}{\partial \lambda} \\
\dot {\boldsymbol \lambda}(t)=-\frac{\partial H}{\partial \boldsymbol x}
$$

（2）边界条件

$$
\boldsymbol x(t_0)=\boldsymbol x_0 \\
\boldsymbol \lambda(t_f)=\frac{\partial \theta[\boldsymbol x(t_f),t_f]}{\partial \boldsymbol x(t_f)}
$$

（3）对于任意 $t \in [t_0,t_f]$，Hamilton函数满足下述条件

$$
H[\boldsymbol x^*(t),\boldsymbol u^*(t),\boldsymbol v^*(t),\boldsymbol \lambda^*(t),t]=\min_{\boldsymbol u}\max_{\boldsymbol v}H[\boldsymbol x^*(t),\boldsymbol u(t),\boldsymbol v(t),\boldsymbol \lambda^*(t),t]\\
=\max_{\boldsymbol v}\min_{\boldsymbol u}H[\boldsymbol x^*(t),\boldsymbol u(t),\boldsymbol v(t),\boldsymbol \lambda^*(t),t]
$$

---

# 【例题1】

在追逃对局中，$y(t)$与$v(t)$分别表示追赶者与逃跑者的相对位移与速度，追赶者希望终端脱靶 $\|y(t_f)\|$最小，而逃跑者希望此值最大。该问题可描述为如下微分对策问题：

系统状态方程为和初值为

$$
\left\{\begin{matrix}
\dot y(t)=v(t) \\
\dot v(t)=a_1(t)-a_2(t)
\end{matrix}\right.,\quad
\left\{\begin{matrix}
y(t_0)=0 \\
v(t_0)=v_0
\end{matrix}\right.
$$

$t_f$ 给定，目标函数为

$$
J=\frac{1}{2}y^2(t_f)
$$

式中，追与逃的加速度 $a_1(t)$ 与 $a_2(t)$ 是该问题的两个控制变量，其范围为

$$
|a_1(t)| \le a_{1m},\quad |a_2(t)| \le a_{2m},\quad a_{1m}>a_{2m}\ge0
$$

---

【解】

**① 构造Hamilton函数**

$$
H=\lambda_1v+\lambda_2(a_1-a_2)
$$

则 $\lambda_1$ 和 $\lambda_2$ 满足如下协态方程

$$
\left\{\begin{matrix}
\dot \lambda_1(t)=-\frac{\partial H}{\partial y}=0	\\
\dot \lambda_2(t)=-\frac{\partial H}{\partial v}=-\lambda_2	
\end{matrix}\right.
\Rightarrow
\left\{\begin{matrix}
\lambda_1(t)=C_1	\\
\lambda_2(t)=-C_1t+C_2	
\end{matrix}\right.
\tag{1}
$$

**② 边界条件**

可知

$$
\varphi=\frac{1}{2}y^2(t_f)
$$

那么边界条件为

$$
\left\{\begin{matrix}
y(t_0)=0	\\
v(t_0)=v_0
\end{matrix}\right.,\quad
\left\{\begin{matrix}
\lambda_1(t_f)=\frac{\partial \varphi}{\partial y(t_f)}=y(t_f) \\
\lambda_2(t_f)=\frac{\partial \varphi}{\partial v(t_f)}=0
\end{matrix}\right.
\tag{2}
$$

将式 $(2)$ 与式 $(1)$ 作对比，得到

$$
C_1=y(t_f)
$$

$$
\lambda_2(t_f)=-y(t_f)\cdot t_f+C_2=0	\\
\Rightarrow C_2=y(t_f)\cdot t_f
$$

整理得到

$$
\left\{\begin{matrix}
\lambda_1(t)=y(t_f) \\
\lambda_2(t)=y(t_f)\cdot (t_f-t)
\end{matrix}\right.
\tag{3}
$$

**③ 极值条件**

根据纳什-庞特里亚金最大最小定理，可得到控制量$a_1(t)$ 与 $a_2(t)$ 的最优轨线如下

$$
\left\{\begin{matrix}
a_1^*(t)=-a_{1m}\mathrm{sgn}[\lambda_2(t)]	\\
a_2^*(t)=-a_{2m}\mathrm{sgn}[\lambda_2(t)]
\end{matrix}\right.
\tag{4}
$$

将式 $(3)$ 代入式 $(4)$，则有

$$
\left\{\begin{matrix}
a_1^*(t)=-a_{1m}\mathrm{sgn}[y(t_f)(t_f-t)]	\\
a_2^*(t)=-a_{2m}\mathrm{sgn}[y(t_f)(t_f-t)]
\end{matrix}\right.
$$

因为 $t_f-t>0$ 恒成立，上式可化简为

$$
\left\{\begin{matrix}
a_1^*(t)=-a_{1m}\mathrm{sgn}[y(t_f)]	\\
a_2^*(t)=-a_{2m}\mathrm{sgn}[y(t_f)]
\end{matrix}\right.
$$

更清晰地，将上式改写为

$$
\left\{\begin{matrix}
a_1^*(t)=-a_{1m},\quad y(t_f) > 0	\\
a_1^*(t)=a_{1m},\quad y(t_f) < 0
\end{matrix}\right.,\quad
\left\{\begin{matrix}
a_2^*(t)=-a_{2m},\quad y(t_f) > 0	\\
a_2^*(t)=a_{2m},\quad y(t_f) < 0
\end{matrix}\right.
\tag{5}
$$

**④ 求解结果**

因为原状态方程为

$$
\left\{\begin{matrix}
\dot y(t)=v(t) \\
\dot v(t)=(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)]
\end{matrix}\right.,\quad
\left\{\begin{matrix}
y(t_0)=0	\\
v(t_0)=v_0
\end{matrix}\right.
$$

注意，$(a_{1m}-a_{2m})\mathrm{sgn}[y(t_f)]$视作常数。对方程求积分，得到

$$
\left\{\begin{matrix}
y(t)=v_0(t-t_0)+\frac{1}{2}(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)](t-t_0)^2	\\
v(t)=(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)](t-t_0)
\end{matrix}\right.
\tag{6}
$$

*疑问：我不懂为什么 $y(t)$ 中还有 $v(t-t_0)$ 这一项，难道和初值条件有关系吗？*

所以，末端条件为

$$
y(t_f)=v_0(t_f-t_0)+\frac{1}{2}(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)](t_f-t_0)^2
$$

根据式 $(5)$ 的分段条件，有

$$
y(t_f)>0时,\\
y(t_f)=v_0(t_f-t_0)+\frac{1}{2}(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)](t_f-t_0)^2>0	\\
\Rightarrow
2v_0>-(a_{2m}-a_{1m})\cdot 1 \cdot (t_f-t_0) \\
\Rightarrow
2v_0>(a_{1m}-a_{2m})\cdot 1 \cdot (t_f-t_0) \\
\Rightarrow
\frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}>1\\
$$

$$
y(t_f)<0时,\\
y(t_f)=v_0(t_f-t_0)+\frac{1}{2}(a_{2m}-a_{1m})\mathrm{sgn}[y(t_f)](t_f-t_0)^2<0	\\
\Rightarrow
2v_0<-(a_{2m}-a_{1m})\cdot (-1) \cdot (t_f-t_0) \\
\Rightarrow
2v_0<(a_{1m}-a_{2m})\cdot (-1) \cdot (t_f-t_0) \\
\Rightarrow
\frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}<-1\\
$$

写在一起，有

$$
\left\{\begin{matrix}
y(t_f)>0\Rightarrow	\frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}>1\\
y(t_f)<0\Rightarrow	\frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}<-1
\end{matrix}\right.
\tag{7}
$$

最后，最优控制为

$$
\left\{\begin{matrix}
a_1^*(t)=-a_{1m},\quad \frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}>1	\\
a_1^*(t)=a_{1m},\quad \frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}<-1
\end{matrix}\right.\\
\left\{\begin{matrix}
a_2^*(t)=-a_{2m},\quad \frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}>1	\\
a_2^*(t)=a_{2m},\quad \frac{2v_0}{(a_{1m}-a_{2m})(t_f-t_0)}<-1
\end{matrix}\right.
\tag{8}
$$

# 思考

就本文最简单的微分对策问题而言，其解题步骤与最优控制类似，参考最优控制的解题方法解微分对策即可。

这里给出Matlab仿真代码：

【例题2】

现有追逃问题，题设【例题1】，其起始时刻 $t_0=0$，终端时刻 $t_f=1$，初始相对速度 $v_0=3m/s$，追赶者加速度 $\|a_1(t)\| \le a_{1m}=2$，逃跑者加速度 $\|a_2(t)\| \le a_{2m}=1.5$

```matlab
clear;clc;close all;
% 题目条件
t_0 = 0; t_f = 1;
v_0 = 3;
a_1m = 2; a_2m = 1.5;

%% 01 构建Hamilton函数
syms H lambda_1 lambda_2 y v a_1 a_2
H = lambda_1*v + lambda_2*(a_1-a_2);
% 求协态方程
Dlambda_1 = -diff(H,y);
Dlambda_2 = -diff(H,v);

%% 02 边界条件
% 性能指标中的常值型性能指标项
syms varphi
varphi = 0.5*y(1)^2;

lambda_1 = diff(varphi,y);
lambda_2 = diff(varphi,v);

%% 03 极值条件
% 省略没写

%% 04 求解结果
eq_1 = strcat('Dlambda_1=',char(Dlambda_1));
eq_2 = strcat('Dlambda_2=',char(Dlambda_2));
con_1 = strcat('lambda_1(1)=',char(y(1)));
con_2 = strcat('lambda_2(1)=',char(lambda_2));
[lambda_1,lambda_2] = dsolve(eq_1,eq_2,con_1,con_2);
[v,y] = dsolve('Dy=v,Dv=(a2m-a1m)*sgnyf','y(0)=0,v(0)=3');
% 求终端时刻相对位移
t = t_f;         % 这个位置有问题，代码错误
y = simplify(y);
y = expand(y);
yf = subs(y);
```