---
permalink: /:year-:month-:day/:title
layout: post
title:  "单兵种和多兵种作战的微分对策模型"
author: "Lei Lie"
categories: Study
tag: notdouban
---
# 准备工作

这篇文章摘抄自沙昌基教授《数理战术学》第六章 单兵种和多兵种作战的微分对策模型。并附上了自己思考。

注：全文的公式使用[LaTex公式编辑器](https://www.latexlive.com/)编辑而成

# 预备知识

设甲、乙双方进行直瞄武器交战，对每一时刻 $t$ ，甲方仅有一类作战单位 $X$ ，$X$ 的数量为 $x=x(t)$ ，乙方共有两类作战单位，第一类作战单位的个数为 $y=y(t)$ ，第二类作战单位的个数为 $z=z(t)$ ，双方的初始作战单位数量为

$$
x(0)=x_0,y(0)=y_0,z(0)=z_0
\tag{1}
$$

其中

$$
x_0>0,y_0>0,z_0>0
$$

设甲方分配作战单位 $X$用于攻击乙方两类作战单位 $Y$， $Z$的比例分别为 $\phi_1=\phi_1(t)$与$\phi_2=\phi_2(t)$，乙方分配作战单位 $Y$， $Z$用于攻击甲方作战单位 $X$的比例分别为 $\psi_1=\psi_1(t)$ 与$\psi_2=\psi_2(t)$，则显然有

$$
\left\{\begin{matrix}
0 \le \phi_1(t) \le 1, \quad 0 \le \phi_2(t) \le 1	\\
0 \le \psi_1(t) \le 1, \quad 0 \le \psi_2(t) \le 1	\\
\phi_1(t) + \phi_2(t) \le 1
\end{matrix}\right.
\tag{2}
$$

此时交战双方的Lanchester方程为

$$
\left\{\begin{matrix}
\dot x(t) = -\psi_1cy-\psi_2dz	\\
\dot y(t) = -\phi_1ax	\\
\dot z(t) = -\phi_2bx	\\
\end{matrix}\right.
\tag{3}
$$

参数说明：

- a：甲方攻击乙I时，单位时间内消灭乙I作战单位个数，$a>0$；
- b：甲方攻击乙II时，单位时间内消灭乙II作战单位个数，$b>0$；
- c：乙I攻击甲方时，单位时间内消灭甲方作战单位个数，$c>0$；
- d：乙II攻击甲方时，单位时间内消灭甲方作战单位个数，$d>0$；

作战结束的条件为：甲方有足够兵力将乙方两类作战单位消灭。

对于**甲方**来说，目标是战斗结束时甲方剩余作战单位数 $\bar x$ 尽可能**大**；

对于**乙方**来说，目标是战斗结束时甲方剩余作战单位数 $\bar x$ 尽可能**小**。

在这里记状态向量为 $S=(x,y,z)^{\mathrm{T}}$，控制函数为$\Phi=(\phi_1,\phi_2)$，$\Psi=(\psi_1,\psi_2)$。当兵力分配矩阵 $\Phi$ 和 $\Psi$ 确定后，战斗过程就确定了，甲方剩余实力 $\bar x$ 就确定了。

因此可以将这个问题转化为微分对策问题，即微分动力系统$(3)$在初值$(1)$与控制函数约束$(2)$的条件下，甲、乙双方的最优策略 $\Phi$ 和 $\Psi$ 满足如下关系：

$$
\max_\Phi \min_\Psi(\Phi,\Psi)=\min_\Psi \max_\Phi(\Phi,\Psi)=g(\Phi^*,\Psi^*)
\tag{4}
$$

这个公式的前提是

$$
x(t) \ge 0,\quad y(t) \ge 0,\quad z(t) \ge 0
$$

# 微分对策问题的求解

假定作战过程中乙II在 $T$ 时刻被消灭，即

$$
z(T)=0
$$

随后，按照单对单兵种直瞄武器Lanchester方程中给出的原则，甲、乙双方的最优策略是将各自全部兵力投入战斗。在此种策略下，战斗结束时，甲方剩余战斗单位数平方为

$$
J=\bar x^2=x^2(T)-\frac{c}{a}y^2(T)
$$

那么式 $(4)$ 可改写为

$$
\max_\Phi \min_\Psi J=\min_\Psi \max_\Phi J=J \bigg |_{\Phi^*,\Psi^*}
$$

求解兵力分配矩阵 $\Phi$ 和 $\Psi$ 。

【解】

## **① 构建Halmiton函数**

$$
H=\lambda_1(-\psi_1cy-\psi_2dz)+\lambda_2(-\phi_1ax)+\lambda_3(-\phi_2bx)
$$

协态方程为

$$
\left\{\begin{matrix}
\dot \lambda_1=-\frac{\partial H}{\partial x}=\lambda_2\phi_1a+\lambda_3\phi_2b	\\
\dot \lambda_2=-\frac{\partial H}{\partial y}=\lambda_1\psi_1c	\\
\dot \lambda_3=-\frac{\partial H}{\partial z}=\lambda_1\psi_2d
\end{matrix}\right.
\tag{5}
$$

## **② 边界条件**

由性能指标 $J=x^2(T)-\frac{c}{a}y^2(T)$ 得

$$
\varphi=x^2(T)-\frac{c}{a}y^2(T)
$$

$$
\left\{\begin{matrix}
\lambda_1(T)=-\frac{\partial \varphi}{\partial x}=2x(T)	\\
\lambda_2(T)=-\frac{\partial \varphi}{\partial y}=-\frac{2c}{a}y(T)	\\
\end{matrix}\right.
$$

## **③ 终止条件**

由时间终止条件得

$$
H=0	\\
\Rightarrow 
\lambda_1(T)\bigg (-\psi_1(T)cy(T)-\psi_2(T)dz(T)\bigg )+\lambda_2(T)\bigg (-\phi_1(T)ax(T)\bigg )+\lambda_3(T)\bigg (-\phi_2(T)bx(T)\bigg )=0
$$

由既定假设条件可知 $z(T)=0$ ，代入上式，得到

$$
\lambda_1(T)\bigg (-\psi_1(T)cy(T)\bigg  )+\lambda_2(T)\bigg (-\phi_1(T)ax(T)\bigg )+\lambda_3(T)\bigg (-\phi_2(T)bx(T)\bigg )=0	\\
\Rightarrow 
\lambda_3(T)=-\frac{\lambda_1(T)\psi_1(T)cy(T)+\lambda_2(T)\phi_1(T)ax(T)}{\phi_2(T)bx(T)}\\
\Rightarrow 
\lambda_3(T)=-\frac{2x(T)\psi_1(T)cy(T)-\frac{2c}{a}y(T)\phi_1(T)ax(T)}{\phi_2(T)bx(T)}\\
\Rightarrow
\lambda_3(T)=-\frac{2c}{b} \cdot \frac{\psi_1(T)-\phi_1(T)}{\phi_2(T)} \cdot y(T)
$$

将辅助函数 $\lambda_1(T)$、$\lambda_2(T)$、$\lambda_3(T)$归纳在一起，写为

$$
\left\{\begin{matrix}
\lambda_1(T)=-\frac{\partial \varphi}{\partial x}=2x(T)	\\
\lambda_2(T)=-\frac{\partial \varphi}{\partial y}=-\frac{2c}{a}y(T)	\\
\lambda_3(T)=-\frac{2c}{b} \cdot \frac{\psi_1(T)-\phi_1(T)}{\phi_2(T)} \cdot y(T)
\end{matrix}\right.
\tag{6}
$$

题目的假设条件为在 $T$ 时刻乙II已经被消灭了，即 $z(T)=0$ ，所以 $\phi_2(T)>0$。

## **④ 求解结果**

根据微分对策原理，对于任意使 $\Phi^*$ 和 $\Psi^*$ 连续的时刻 $t$ ，均使Hamilton函数达到鞍点，即

$$
H\bigg (S^*(t),\lambda^*(t),\Phi^*(t),\Psi^*(t),t \bigg )	\\
=\max_\Phi \min_\Psi H\bigg (S^*(t),\lambda^*(t),\Phi(t),\Psi(t),t \bigg )	\\
=\min_\Psi \max_\Phi H\bigg (S^*(t),\lambda^*(t),\Phi(t),\Psi(t),t \bigg )
$$

其中， $\Phi$ 和 $\Psi$ 满足式 $(2)$。

下面通过分析，简化最优解 $S^*,\lambda^*,\Phi^*,\Psi^*$ 的求解过程。

由式 $(6)$ 可以对 $\lambda_1^*(T)$ 和 $\lambda_2^*(T)$ 进行定性分析，有

$$
\left\{\begin{matrix}
\lambda_1^*(T)=2x(T)>0	\\
\lambda_2^*(T)=-\frac{2c}{a}y(T)<0	\\
\end{matrix}\right.
$$

为了方便分析，再重新写一遍Hamilton函数放在这里。

$$
H=\lambda_1(-\psi_1cy-\psi_2dz)+\lambda_2(-\phi_1ax)+\lambda_3(-\phi_2bx)
$$

甲方的目标是使 $H$ 取极大值；

乙方的目标是使 $H$ 取极小值。

因为 $\lambda_1^*(T)>0$，应使 $\psi_1^*(t)$ 与 $\psi_2^*(t)$ 越大越好，取

$$
\left\{\begin{matrix}
\psi_1^*(t)=1	\\
\psi_2^*(t)=1
\end{matrix}\right.
\tag{7}
$$

有 $\lambda_2^*(T)<0$ ，则应使 $\phi_1^*(t)$ 越大越好，取

$$
\phi_1^*(t)=1-\phi_2^*(t)
\tag{8}
$$

那么 $\lambda_3^*(T)$ 取

$$
\lambda_3^*(T)=-\frac{2c}{b} \cdot \frac{\psi_1^*(T)-\phi_1^*(T)}{\phi_2^*(T)} \cdot y(T)	\\
\Rightarrow
\lambda_3^*(T)=-\frac{2c}{b} \cdot \frac{1-(1-\phi_2^*(t))}{\phi_2^*(T)} \cdot y(T)	\\
\Rightarrow
\lambda_3^*(T)=-\frac{2c}{b}y(T)<0
$$

---

**分析方法1：**

由式 $(6)$ ，若有 $\lambda_2(t)<0$ 、$\lambda_3(t)<0$，则得到 $\dot \lambda_1(t)<0$，说明 $\lambda_1$ 在 $t \in [0,T]$ 单调下降。又因为 $\lambda_1(T)>0$ ，那么可以看出 $\lambda_1$ 在 $t \in [0,T]$ 处，恒有 $\lambda_1(t)>0$。

然后，因为 $\lambda_1(t)>0$，则有 $\dot \lambda_2(t)>0$ 、$\dot \lambda_3(t)>0$，说明 $\lambda_1$ 、 $\lambda_2$ 在 $t \in [0,T]$ 单调上升，又因为 $\lambda_2(T)<0$、$\lambda_3(T)<0$，那么可以看出 $\lambda_2$、$\lambda_3$ 在 $t \in [0,T]$ 处，恒有 $\lambda_2(t)<0,\lambda_3(t)<0$。

**分析方法2：**

由微分对策的战术意义可知，甲方的目标是使 $H$ 取极大值，乙方的目标是使 $H$ 取极小值。那么下式恒成立。

$$
\left\{\begin{matrix}
\lambda_1(t) > 0	\\
\lambda_2(t) < 0	\\
\lambda_3(t) < 0
\end{matrix}\right.
$$

---

根据式 $(7)$ ，Hamilton函数可改写为

$$
H=\lambda_1cy-\lambda_1dz-(\lambda_2\phi_1a+\lambda_3\phi_2b)x
$$

根据式 $(8)$ 有，$\phi_1^{\*}(t)+\phi_2^{\*}(t)=1$。所以实际上就是 $-\lambda_2a$ 与 $-\lambda_3b$ 的加权平均。那么现在就得让 $(\lambda_2\phi_1a+\lambda_3\phi_2b)$ 的数值尽可能小，这样减去的数值才会小，$H$ 才会取极大值。所以要比较 $\lambda_2a$ 和 $\lambda_3b$ 中谁更小。取小值进行加权，即

$$
\left\{\begin{matrix}
\phi_1(t)=0,\phi_2(t)=1, \quad \lambda_2(t)a>\lambda_3(t)b	\\
\phi_1(t)=1,\phi_2(t)=0, \quad \lambda_2(t)a<\lambda_3(t)b
\end{matrix}\right.
$$

构造辅助函数

$$
\eta(T)=\lambda_2(T)a-\lambda_3(T)b		\\
=-\frac{2c}{a}y(T)a+\frac{2c}{b}y(T)b
=0
$$

其意义是，在 $T$ 时刻，$\lambda_2a$ 和 $\lambda_3b$ 两者数值一样大。

再讨论 $t\in[0,T)$ 的情况。

有

$$
\dot \eta(t) =\dot \lambda_2(t)a - \dot \lambda_3(t)b	\\
=\lambda_1(t)\psi_1ca-\lambda_1(t)\psi_2db	\\
=\lambda_1(t)\cdot1\cdot ca-\lambda_1(t)\cdot1\cdot bd	\\
=\lambda_1(t)(ac-bd)
$$

因 $\lambda_1(t)>0$ ，只需要看 $(ac-bd)$ 的符号即可。当 $ac<bd$ 时，有 $\dot \eta(t)<0，t \in [0,T]$，又因为 $\eta(T)=0$，所以

$$
\eta(t)>0,\quad t \in [0,T)
$$

也就是说明

$$
\eta(t)=\lambda_2(T)a-\lambda_3(T)b	\\
\Rightarrow \lambda_2(T)a>\lambda_3(T)b
$$

那么取

$$
\phi_1^*(t)=0,\quad \phi_2^*(t)=1
$$

## **⑤ 结论**

最优控制函数 $\Phi^{\*}(t)$ 和 $\Psi^{\*}(t)$ 的取值为

$$
\Phi^*(t)=\begin{bmatrix}
\phi_1^*(t)	\\
\phi_2^*(t)
\end{bmatrix}
=\begin{bmatrix}
0	\\
1
\end{bmatrix},\quad
\Psi^*(t)=\begin{bmatrix}
\psi_1^*(t)	\\
\psi_2^*(t)
\end{bmatrix}
=\begin{bmatrix}
1	\\
1
\end{bmatrix}
\tag{9}
$$

如果甲方决定集中力量先消灭乙I的话，那么最优控制函数 $\Phi^*(t)$ 和 $\Psi^*(t)$ 的取值变化如下。

$$
\Phi^*(t)=\begin{bmatrix}
\phi_1^*(t)	\\
\phi_2^*(t)
\end{bmatrix}
=\begin{bmatrix}
1	\\
0
\end{bmatrix},\quad
\Psi^*(t)=\begin{bmatrix}
\psi_1^*(t)	\\
\psi_2^*(t)
\end{bmatrix}
=\begin{bmatrix}
1	\\
1
\end{bmatrix}
$$

综上，体现了“集中兵力，攻击敌方”的作战原则。

# 扩展

考虑一般的单兵种对多兵种作战问题，给出1对 $n$ 的直瞄武器战斗的Lanchester方程为

$$
\left\{\begin{matrix}
\dot x = -\sum_{j=1}^{n} b_j \psi_j y_j	\\
\dot y = -a_j \phi_j x
\end{matrix}\right. , \quad j = 1,\cdots,n\\
$$

约束条件为

$$
\left\{\begin{matrix}
0 \le \psi_j(t) \le 1	\\
0 \le \phi_j(t)			\\
\sum_{j=1}^{n}\phi_j \le 1
\end{matrix}\right.,\quad j=1,\cdots,n
$$

与上述讨论的结果类似，采用微分对策原理，同样会得出相同结论：

- 乙方各类作战单位全部攻击甲方；
- 甲方集中兵力攻击乙方第 $i$ 类作战单位，将其全部歼灭后，再转向另一类作战单位……如此反复。

这种原则与一对二作战的情况及其相似。即甲方的最优火力分配策略仍然是集中火力攻击乙方的一类作战单位，攻击顺序按照交战强度的高低进行分配，最优兵力分配策略与双方当时的实力无关，并且不依赖于时间。