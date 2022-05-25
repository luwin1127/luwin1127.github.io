---
permalink: /:year-:month-:day/:title
layout: post
title:  "极小值原理及其应用"
author: "Lei Lie"
categories: Study
tag: notdouban
---

# 第3章 极小值原理及其应用

在实际物理系统中，控制向量总是受到一定的限制，容许控制值能在一定的控制域内取值，可以预料，用古典变分法难以处理上述问题。

注：本文总结自胡寿松教授的专著《最优控制理论与系统》第三章 极小值原理及其应用。文章中举的例题均用自己的方法写了一遍，没有依照书上的解法写。

全文的公式使用[LaTex公式编辑器](https://www.latexlive.com/)编辑而成。

## 3.1 连续系统的极小值原理

如果描述最优控制问题的一些函数，如 $f$ 和 $\varphi$ 等显含时间 $t$ 和 $t_f$ ，则称为**时变问题**。

### 3.1.1 定常系统

为了方便阐述，先研究**定常系统、末值型指标、末端自由、控制受约束**的极小值原理，然后将所得结果扩大至一般的最优控制问题中。再研究性能指标为**积分型**的控制问题极小值原理应用情况。

#### （1）定常系统、末值型性能指标、末端自由、控制受约束

【定理1】 对于如下定常系统、末值型性能指标、末端自由、控制受约束的最优控制问题：

$$
\min_{u(t) \in \Omega} J(u)=\varphi \big[ x(t_f)\big ]	\\
\mathrm{s. t. }\quad \dot x(t)=f(x,u),\quad x(t_0)=x_0,\quad t \in \big[ t_0,t_f \big]
\tag{1}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda,t)=\lambda^{\mathrm{T}}f(x,u,t)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)}
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H(x^*,u^*,\lambda)=\min_{u(t) \in \Omega} H(x^*,u,\lambda)=\mathrm{const}
$$

**④ *H* 变化率** （Hamilton函数沿最优轨线保持为常数）

当 $t_f$ 固定时

$$
H\big[x^*(t),u^*(t),\lambda(t)\big]=H\big[x^*(t_f),u^*(t_f),\lambda(t_f)\big]=\mathrm{const}
$$

当 $t_f$ 自由时

$$
H\big[x^*(t^*_f),u^*(t^*_f),\lambda^*(t^*_f)\big]=0
$$

#### （2）定常系统、积分型性能指标、末端自由、控制受约束

【定理2】对于如下定常系统、积分型性能指标、末端自由、控制受约束的最优控制问题：

$$
\min_{u(t) \in \Omega} J(x)=\int_{t_0}^{t_f}L[x(t),u(t)]\mathrm{d}t \\
\mathrm{s. t. }\quad \dot x(t)=f(x,u),\quad x(t_0)=x_0, \\
\quad t \in \big[ t_0,t_f \big],\quad t_f未定
\tag{2}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda)=L(x,u)+\lambda^{\mathrm{T}}f(x,u)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\lambda(t_f)=0
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H(x^*,u^*,\lambda)=\min_{u(t) \in \Omega} H(x^*,u,\lambda)=\mathrm{const}
$$

**④ *H* 变化率** （Hamilton函数沿最优轨线保持为常数）

当 $t_f$ 固定时

$$
H\big[x^*(t),u^*(t),\lambda(t)\big]=H\big[x^*(t_f),u^*(t_f),\lambda(t_f)\big]=\mathrm{const}
$$

当 $t_f$ 自由时

$$
H\big[x^*(t^*_f),u^*(t^*_f),\lambda^*(t^*_f)\big]=0
$$

#### 总结

当控制问题为定常系统时，性能指标为末值型和积分型的区别在于**边界条件**的不同，此外再无区别。

末值型性能指标的边界条件为

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)}
$$

积分型性能指标的边界条件为

$$
x(t_0) = x_0 \\
\lambda(t_f)=0
$$

所以，在解题时，按照 **① —>  ② —>  ③ —>  ④** 的步骤解题即可，与变分法无异。

#### 例题

---

【例题1】 设二阶系统的状态方程及初始条件为

$$
\left\{\begin{matrix}
\dot x_1(t) = -x_1(1)+u(t),\quad x_1(0)=1  \\
\dot x_2(t) = x_1(1),\quad x_2(0)=0
\end{matrix}\right.
$$

式中标量控制 $u(t)$ 的约束条件为

$$
|u(t)| \le 1
$$

若系统末端状态 $x(t_f)$ 是自由的，试求最优控制 $u(t)$ ，使性能指标

$$
J=x_2(1)
$$

取极小值。

---

【解】本例为定常系统、末值型性能指标、末端自由、 $t_f$ 固定和控制受约束的最优控制问题。

**① 构造Hamilton函数**

$$
H=\lambda_1 \big [-x_1(t)+u(t) \big]+\lambda_2 x_2(t)
$$

$x(t)$ 和 $\lambda (t)$ 满足下列正则方程

$$
\left\{\begin{matrix}
\dot x_1(t)=\frac{\partial H}{\partial \lambda_1}=-x_1(t)+u(t)\\
\dot x_2(t)=\frac{\partial H}{\partial \lambda_2}=x_1(t)
\end{matrix}\right.
$$

$$
\left\{\begin{matrix}
\dot \lambda_1=-\frac{\partial H}{\partial x_1(t)}=\lambda_1-\lambda_2\\
\dot \lambda_2=-\frac{\partial H}{\partial x_2(t)}=0
\end{matrix}\right. \\
\Rightarrow
\left\{\begin{matrix}
\lambda_1=C_1e^{t}+C_2\\
\lambda_2=C_2
\end{matrix}\right.
$$

**② 边界条件**

可知

$$
\varphi = x_2(1)
$$

那么边界条件为

$$
x_1(0)=1,\quad x_2(0)=0 \\
\lambda_1(1)=\frac{\partial \varphi}{\partial x_1(1)}=0,\quad \lambda_2(1)=\frac{\partial \varphi}{\partial x_2(1)}=1
$$

得到

$$
\lambda(t)=e^{1-t}-1
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

因为Hamilton函数可写为

$$
H=x_1(\lambda_2-\lambda_1)+\lambda_1u
$$

根据极小值原理，要使Hamilton函数无论在何种情况下都保持最小，其控制量 $u$ 的取值应为

$$
u=\left\{\begin{matrix}
0,\quad \lambda_1 \le 0 \\
-1,\quad \lambda_1 > 0 
\end{matrix}\right.
$$

因为

$$
\left\{\begin{matrix}
\dot \lambda_1(t)=-\lambda_1(t) \\
\lambda_1(1)=0
\end{matrix}\right.
\Rightarrow
\left\{\begin{matrix}
C_1=-e^{-1} \\
C_2=1
\end{matrix}\right.
$$

得到

$$
\lambda_1(t)=1-e^{1-t} \Rightarrow \left\{\begin{matrix}
\lambda_1>0,\quad 0 \le t<1 \\
\lambda_1=0,\quad t=1
\end{matrix}\right.
$$

最后得到最优控制 $u^*(t)$ 为

$$
u^*=\left\{\begin{matrix}
-1, \quad 0 \le t < 1 \\
0, \quad t = 1
\end{matrix}\right.
$$

---

【例题2】设一阶状态方程为

$$
\dot x(t) =x(t)-u(t),\quad x(0)=5
$$

式中控制约束

$$
0.5 \le u(t) \le 1
$$

试求使性能指标

$$
J=\int_{0}^{1}\big[x(t)+u(t) \big]\mathrm{d}t
$$

为极小值的最优控制 $u^*(t)$ 和相应的最优性能指标 $J^*$。

---

【解】本例为定常系统、积分型性能指标、$t_f$ 固定和末端自由的最优控制问题。

**① 构造Hamilton函数**

$$
H=x(t)+u(t)+\lambda \big [ x(t)-u(t) \big]
$$

$x(t)$ 和 $\lambda (t)$ 满足下列正则方程

$$
\dot x(t)=\frac{\partial H}{\partial \lambda}=x(t)-u(t)\\
\dot \lambda(t)=- \frac{\partial H}{\partial x}=-\big[1+\lambda(t)\big ]
$$

求出协态方程的表达式为

$$
\lambda(t)=Ce^{-t}-1
$$

**② 边界条件**

可知

$$
\varphi = x_2(1)
$$

那么边界条件为

$$
x(0)=5 \\
\lambda(1)=Ce^{-1}-1=0 \Rightarrow C=e
$$

得到

$$
\lambda(t)=e^{1-t}-1
$$

**③ 极值条件**

Hamilton函数可化为

$$
H=x(t)(1+\lambda)+u(t)(1-\lambda)
$$

根据极值条件，控制量 $u(t)$ 为

$$
u(t)=\left\{\begin{matrix}
0.5, \quad \lambda < 1 \\
1, \quad \lambda \ge 1
\end{matrix}\right.
$$

令 $\lambda = 1$ ，得到

$$
\lambda(t) = e^{1-t}-1=1 \\
\Rightarrow t=0.307
$$

那么，控制量 $u^*(t)$ 为

$$
u^*(t)=\left\{\begin{matrix}
0.5, \quad 0 \le t < 0.307 \\
1, \quad 0.307 \le t \le 1
\end{matrix}\right.
$$

将 $u^*(t)$ 代入状态方程，得

$$
\dot x(t)=\left\{\begin{matrix}
x(t)-1, \quad 0 \le t < 0.307 \\
x(t)-0.5, \quad 0.307 \le t \le 1
\end{matrix}\right.
$$

解得

$$
x(t)=\left\{\begin{matrix}
c_1x(t)+1, \quad 0 \le t < 0.307 \\
c_2x(t)+0.5, \quad 0.307 \le t \le 1
\end{matrix}\right.
$$

因为初始条件 $x(0)=5$ ，代入方程中，得到

$$
c_1x(0)+1=5 \\
\Rightarrow c_1=4
$$

当 $t=0.307$ 时，让左极限等于右极限，有

$$
4e^{0.307}+1=c_2e^{0.307}+0.5=6.44 \\
\Rightarrow c_2=4.37
$$

注意，这里的结果是在两位小数的精度下求解出来的。

所以，最优轨线 $x^*(t)$ 为

$$
x^*(t)=\left\{\begin{matrix}
4x(t)+1, \quad 0 \le t < 0.307 \\
4.37x(t)+0.5, \quad 0.307 \le t \le 1
\end{matrix}\right.
$$

最优性能为

$$
J = \int_{0}^{0.307}(4e^t+2)\mathrm{d}t+\int_{0.307}^{1}(4.37e^t+2)\mathrm{d}t=8.68
$$

代码为

```matlab
clear;clc;close all;
% 定义变量
syms H lambda x u x1
% Step 1：构造哈密顿方程
H = (x+u) + lambda*(x-u);
% 协态方程
Dlambda = -diff(H,'x');
% Step 2：边界条件
% 求出拉格朗日乘子 lambda1
lambda = dsolve('Dlambda = -lambda-1','lambda(1)=0');
% Step 3：极值条件
% 求切换时间 ts
ts = solve('exp(1-ts)-1=1');
x1 = dsolve('Dx1=x1-1','x1(0)=5')
xTemp = 4*exp(0.307)+1;
x2 = dsolve('Dx2=x2-0.5','x2(0.307)=6.4374')
```

### 3.1.2 时变系统

#### （1）时变系统、末值型性能指标、末端自由、控制受约束

【定理3】对于如下时变系统、末值型性能指标、末端自由、控制受约束的最优控制问题

$$
\min_{u(t) \in \Omega} J(x)=\varphi \big[ x(t_f),t_f \big ] \\
\mathrm{s. t. }\quad \dot x(t)=f(x,u),\quad x(t_0)=x_0, \\
\quad t \in \big[ t_0,t_f \big],\quad t_f未知
\tag{3}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda,t)=\lambda^{\mathrm{T}}f(x,u,t)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi[x(t_f),t_f]}{\partial x(t_f)}
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H(x^*(t),u^*(t),\lambda(t),t)=\min_{u(t) \in \Omega} H(x^*(t),u(t),\lambda(t),t)=\mathrm{const}
$$

**④ 最优轨线末端**Hamilton**函数应满足** 

$$
H\big[x^*(t_f^*),u^*(t_f^*),\lambda(t_f^*),t_f^*\big]=-\frac{\partial[x^*(t_f^*),t_f^*]}{\partial t_f}
$$

**⑤ *H* 变化率**（沿最优轨线Hamilton函数变化率）

$$
H\big[x^*(t),u^*(t),\lambda(t),t\big]=H\big[x^*(t_f),u^*(t_f),\lambda(t_f),t_f\big]-\int_{t}^{t_f} \frac{\partial H(x,u,\lambda,\tau)}{\partial \tau}\mathrm{d}\tau
$$

#### 总结

- 在解题时，按照 **① —>  ② —>  ③ —>  ④ —> ⑤** 的步骤解题即可，与变分法无异。

- 时变系统和定常系统的区别是，时变系统有**最优轨线末端Hamilton函数应满足的条件**，定常系统没有。但其实这个条件一般也用不上，两者都有***H* 变化率**此项。

### 3.1.3 末端约束问题的推广

#### （1）定常系统、末值型性能指标、末端受约束、控制受约束

【定理4】对于如下定常系统、末值型性能指标、末端受约束、控制受约束的最优控制问题

$$
\min_{u(t) \in \Omega} J(x)=\varphi \big[ x(t_f),t_f \big ] \\
\mathrm{s. t. }\quad ①\quad\dot x(t)=f(x,u),\quad x(t_0)=x_0, \quad t \in \big[ t_0,t_f \big],\quad t_f未定\\
②\quad \Psi[x(t_f)]=0
\tag{4}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda)=\lambda^{\mathrm{T}}f(x,u)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\Psi[x(x_f)] = 0\\
$$

**注意**：如果末端受约束，必定有此条件，这也是和末端自由最优控制问题的区别之处。

$$
\lambda(t_f)=\frac{\partial \varphi[x(t_f)]}{\partial x(t_f)}+\frac{\partial \Psi^{\mathrm{T}}[x(t_f)]}{\partial x(t_f)}\gamma
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H(x^*(t),u^*(t),\lambda(t))=\min_{u(t) \in \Omega} H(x^*(t_f),u(t_f),\lambda(t_f))
$$

**④ *H* 变化率** 

当 $t_f$ 固定时

$$
H\big[x^*(t),u^*(t),\lambda(t)\big]=H\big[x^*(t_f),u^*(t_f),\lambda(t_f)\big]=\mathrm{const}
$$

当 $t_f$ 自由时

$$
H\big[x^*(t^*_f),u^*(t^*_f),\lambda^*(t^*_f)\big]=0
$$

对于时变系统，末端约束一般表示为

$$
\Psi[x(t_f),t_f]=0
$$

#### （2）时变系统、末值型性能指标、末端受约束、控制受约束

【定理5】对于如下定常系统、末值型性能指标、末端受约束、控制受约束的最优控制问题

$$
\min_{u(t) \in \Omega} J(x)=\varphi \big[ x(t_f),t_f \big ] \\
\mathrm{s. t. }\quad ①\quad\dot x(t)=f(x,u),\quad x(t_0)=x_0, \quad t \in \big[ t_0,t_f \big],\quad t_f未定\\
②\quad \Psi[x(t_f)]=0
\tag{5}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda,t)=\lambda^{\mathrm{T}}(t)f(x,u,t)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\Psi[x(x_f),t_f] = 0\\
$$

**注意**：如果末端受约束，必定有此条件，这也是和末端自由最优控制问题的区别之处。

$$
\lambda(t_f)=\frac{\partial \varphi[x(t_f),t_f]}{\partial x(t_f)}+\frac{\partial \Psi^{\mathrm{T}}[x(t_f),t_f]}{\partial x(t_f)}\gamma
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H(x^*(t),u^*(t),\lambda(t),t)=\min_{u(t) \in \Omega} H(x^*(t),u(t),\lambda(t),t)
$$

**④ 在最优轨线末端Hamilton函数应满足**

$$
H\big[x^*(t_f^*),u^*(t_f^*),\lambda(t_f^*),t_f^*\big]=-\frac{\partial[x^*(t_f^*),t_f^*]}{\partial t_f}-\gamma^{\mathrm{T}}\frac{\partial \Psi[x^*(t_f^*),t_f^*]}{\partial t_f}
$$

**⑤ *H* 变化率**（沿最优轨线哈密顿函数变化率）

$$
H\big[x^*(t),u^*(t),\lambda(t),t\big]=H\big[x^*(t_f),u^*(t_f),\lambda(t_f),t_f\big]-\int_{t}^{t_f} \frac{\partial H(x,u,\lambda,\tau)}{\partial \tau}\mathrm{d}\tau
$$

### 3.1.4 复合型性能指标问题的推广

#### （1）时变系统、复合型性能指标、末端自由、控制受约束

【定理6】对于如下时变系统、复合型性能指标、末端自由、控制受约束的最优控制问题：

$$
\min_{u(t) \in \Omega} J(x)=\varphi \big[ x(t_f),t_f \big ]+\int_{t_0}^{t_f}L(x,u，t)\mathrm{d}t \\
\mathrm{s. t. }\quad ①\quad\dot x(t)=f(x,u),\quad x(t_0)=x_0, \quad t \in \big[ t_0,t_f \big],\quad t_f自由\\
\tag{6}
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列正则方程**

$$
\dot x(t)=\frac{\partial H}{\partial \lambda} \\
\dot \lambda(t)=- \frac{\partial H}{\partial x}
$$

式中

$$
H(x,u,\lambda,t)=L(x,u,t)+\lambda^{\mathrm{T}}f(x,u,t)
$$

**② 边界条件**

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi[x(t_f),t_f]}{\partial x(t_f)}
$$

**③ 极值条件**（即Hamilton函数相对最优控制为极小值）

$$
H[x^*(t),u^*(t),\lambda(t),t]=\min_{u(t) \in \Omega} H[x^*(t),u^*(t),\lambda(t),t]
$$

**④ *H* 变化率**

$$
H[x^*(t_f^*),u^*(t_f^*),\lambda(t_f^*),t_f^*]=-\frac{\partial \varphi[x^*(s_f^*),t_f^*]}{\partial t_f}
$$

### 3.1.5 定常系统和时变系统极小值原理

#### 例题

---

【例题3】设宇宙飞船登月舱的质量为 $m(t)$，高度为 $h(t)$，垂直速度为 $v(t)$，发动机推力为 $u(t)$，月球表面的重力加速度为常数 $g$，不带燃料的登月舱质量为 $M$，初始燃料的总质量为 $F$。已知登月舱的状态方程为

$$
\dot h(t)=v(t),\quad h(0)=h_0 \\
\dot v(t)=-g+\frac{1}{m(t)}u(t),\quad v(0)=v_0 \\
\dot m(t)=-ku(t),\quad m(0)=M+F
$$

要求登月舱在月球表面实现软着陆，即末端约束为

$$
\psi_1=h(t_f)=0\\
\psi_2=v(t_f)=0
$$

发动机推力 $u(t)$ 的约束为

$$
u(t) \in \Omega,\quad \Omega=\{u(t)|0 \le t \le a \},\quad \forall t \in[0,t_f]
$$

试确定最优控制 $u^*(t)$，使登月舱从已知初态转移到要求的目标集，并使登月舱燃料消耗

$$
J=-m(t)
$$

为最小值。


---

【解】本例为时变系统、末值型性能指标、 $t_f$ 自由和末端约束的最优控制问题。

**① 构造Hamilton函数**

$$
H=\lambda_h(t)v(t)+\lambda_v(t)[-g+\frac{1}{m(t)}u(t)]-\lambda_m(t)ku(t)
$$

协态方程为

$$
\left\{\begin{matrix}
\dot \lambda_h(t)=-\frac{\partial H}{\partial h(t)}=0	\\
\dot \lambda_v(t)=-\frac{\partial H}{\partial v(t)}=-\lambda_h(t)	\\
\dot \lambda_m(t)=-\frac{\partial H}{\partial m(t)}=\frac{1}{m^2(t)\lambda_v(t)}u(t)	\\
\end{matrix}\right.
$$

**② 边界条件**

可知

$$
\varphi =-m(t_f)
$$

由公式

$$
\left\{\begin{matrix}
\lambda_h(t_f)=\frac{\partial \varphi}{\partial h(t_f)}+\frac{\partial \psi_1}{\partial h(t_f)}\gamma_1=\gamma_1	\\
\lambda_v(t_f)=\frac{\partial \varphi}{\partial v(t_f)}+\frac{\partial \psi_2}{\partial v(t_f)}\gamma_2=\gamma_2	\\
\lambda_m(t_f)=\frac{\partial \varphi}{\partial m(t_f)}=0	\\
\end{matrix}\right.
$$

式中，$\gamma_1$ 和 $\gamma_2$ 是待定的拉格朗日乘子。

**③ 极值条件**

把Hamilton函数整理为

$$
H=\lambda_hv(t)-g\lambda_v+\bigg(\frac{\lambda_v}{m(t)}-k\lambda_m\bigg)u
$$

**④ 求得结果**

由极值条件，$H$ 相对 $u^*(t)$ 去绝对极小值。得到最优控制 $u^*(t)$ 为

$$
u^*(t)=\left\{\begin{matrix}
\alpha,\quad 当\frac{\lambda_v}{m(t)}-k\lambda_m<0	\\
0,\quad 当\frac{\lambda_v}{m(t)}-k\lambda_m>0
\end{matrix}\right.
$$

上式结果表明，只有当发动机推力在其最大值和零值之间进行开关控制，才有可能在实现软着陆的同时，实现登月舱的燃料消耗最少。

## 3.2 离散系统的极小值原理

随着数字计算机日益普及，计算机控制系统日益增多。因此，离散系统最优控制的研究显得十分重要，原因有二：（1）许多实际问题本身就是离散的；（2）实际系统本身是连续的，但为了对连续过程实行计算机控制，需要把时间整量化，从而得到一个离散化系统，使得连续最优控制中难以求解的两点边界值问题，可以化为易于用计算机求解的离散化两点边值问题。

### 3.2.1 离散系统的欧拉方程

设描述离散系统的状态差分方程为

$$
x(k+1)=f\big[x(k),u(k),k \big],\quad k=0,1,\dots,N-1
$$

式中，$x(k)$ 是离散时刻 $t_k$ 的 $n$ 维状态向量； $u(k)$ 是 $t_k$ 的 $n$ 维控制向量； $f(\cdot)$ 是 $n$ 维向量函数序列。对于等间隔采样， $k=kT$ ， $T$ 为采样周期； $N$ 为数据窗口长度。

离散最优控制问题中，性能指标取为如下标量函数的累加：

$$
\sum_{k=0}^{N-1} L\big[x(k),u(k),x(k+1),k \big]=\sum_{k=0}^{N-1}L_k
$$

式中 $L\big[x(k),u(k),x(k+1),k \big]$ 是第 $k$ 个采样周期内性能指标 $J$ 的增量。

那么， 离散泛函极值的必要条件如下。

离散欧拉方程（也称向量差分方程）为

$$
\left\{\begin{matrix}
\frac{\partial L_k}{\partial x(k)}+\frac{\partial L_{k-1}}{\partial x(k)}=0 \\
\frac{\partial L_k}{\partial u(k)}=0
\end{matrix}\right.
$$

以及，离散横截条件为

$$
\bigg[\frac{\partial L_k}{\partial x(k)} \bigg]_{k=N}^\mathrm{T} \delta x(N)-\bigg[\frac{\partial L_{k-1}}{\partial x(k)} \bigg]^\mathrm{T} \delta x(0)=0
$$

若始端固定，即 $x(0)=x_0$ ，末端自由，即 $\delta x(n)$ 任意，则边界条件为

$$
x(0)=x_0,\quad \frac{\partial L[x(N-1),u(N-1),x(N),N-1]}{\partial x(N)}=0
$$

#### 例题

---

【例题4】设一阶离散系统及其边界条件为

$$
x(k+1)=x(k)+u(k) \\\
x(0)=1,\quad x(5)=0
$$

性能指标为

$$
J=\frac{1}{2}\sum_{k=0}^{4}u^2(k)
$$

试求使性能指标为极小的最优控制序列 $u^*(k)$ 和相应的最优状态序列 $x^*(k)$

---

【解】使用离散欧拉法解题。

**① 构造广义离散泛函 $J_a$ 和 $L_k$、 $L_{k+1}$**

应用拉格朗日乘子函数 $\lambda (k)$ ，构造广义离散泛函

$$
J_a=\sum_{k=1}^{4}\bigg\{\frac{1}{2}u^2(k)+\lambda(k+1)\big[-x(k+1)+x(k)+u(k)\big] \bigg\}
$$

和

$$
L_k=\frac{1}{2}u^2(k)+\lambda(k+1)\big[-x(k+1)+x(k)+u(k)\big]\\
L_{k-1}=\frac{1}{2}u^2(k-1)+\lambda(k)\big[-x(k)+x(k-1)+u(k-1)\big]
$$

**② 离散泛函极值的必要条件**

离散欧拉方程为

$$
\frac{\partial L_k}{\partial x(k)}=\lambda(k+1),\quad \frac{\partial L_{k-1}}{\partial x(k)}=-\lambda(k) \\
\frac{\partial L_k}{\partial u(k)}=u(k)+\lambda(k+1)
$$

**③ 解出结果**

由上式可得，

$$
\lambda(k+1)=\lambda(k)=c \\
u(k)=-\lambda(k+1)=-c
$$

那么求得 $x(k+1)$ 为

$$
x(k+1)=x(k)+u(k)=x(k)-c
$$

用迭代法求上述差分方程，有

$$
x(1)=x(0)-c \\
x(2)=x(1)-c=x(0)-2c\\
\vdots	\\
x(k)=x(0)-kc
$$

代入边界条件：$x(0)=1$，$x(5)=0$，解得

$$
c=0.2
$$

因此，该离散系统的最优控制为

$$
u^*(k)=-0.2,\quad k=0,1,2,3,4
$$

最优轨线为

$$
x^*(k)=1-0.2k,\quad k=0,1,2,3,4
$$


### 3.2.2 离散系统的极小值原理

离散极小值原理可叙述如下。

【定理7】设离散系统状态方程为 

$$
x(k+1)=f\big[x(k),u(k),k \big],\quad x(0)=x_0 \\
k=0,1,2,\dots,N-1
$$

性能指标为

$$
J=\varphi \big[x(N),N \big]+\sum_{k=0}^{N-1}L\big[x(k),u(k),k \big] \\
\Psi \big[x(N),N \big]=0 \quad（末端受约束情况由此项、末端自由和末端固定没有）
$$

**① $x(t)$ 和 $\lambda (t)$ 满足下列差分方程**

$$
x(k+1)=\frac{\partial H(k)}{\partial \lambda(k+1)} \\
\lambda(k)=\frac{\partial H(k)}{\partial \lambda(k)}
$$

式中离散Hamilton函数为

$$
H(k)=L[x(k),u(k),k]+\lambda^{\mathrm{T}}(k+1)f[x(k),u(k),k]
$$

**② 边界条件**

当末端受**约束**时（即 $\Psi \big[x(N),N \big]=0$），有

$$
x(0)=x_0\\
\Psi[x(N),N]=0\\
\lambda(N)=\frac{\partial \varphi[x(N),N]}{\partial x(N)}+\frac{\partial \Psi^{\mathrm{T}}[x(N),N]}{\partial x(N)}\gamma
$$

当末端**自由**时，有

$$
x(0)=x_0\\

\lambda(N)=\frac{\partial \varphi[x(N),N]}{\partial x(N)}

$$

**③ 极值条件**（即离散Hamilton函数相对最优控制 $u^*(k)$ 为极小值）

$$
H[x^*(k),u^*(k),\lambda(k+1),k]=\min_{u(k) \in \Omega}H[x^*(k),u(k),\lambda(k+1),k]
$$

若控制变量不受约束，可得

$$
\frac{\partial H(k)}{\partial u(k)}=0
$$

上述离散极小值原理表明，离散系统最优化问题归结为求解一个离散两点边值问题。而对离散极小值原理的理解，与连续极小值原理一样，使离散性能泛函为极小与使离散Hamilton函数为极小是等价的。

#### 例题

---

【例题5】已知离散系统

$$
x(k+1)=x(k)+u(k),\quad x(0)=x_0
$$

性能指标

$$
J=\sum_{k=0}^{2}[x^2(k)+u^2(k)]
$$

求使性能指标达到最小的最优控制序列 $u^*(k)$。

---

【解】 本例 $N=0$ ，末态 $x(3)$ 自由，用第3.2.2节介绍的定理解题。

**① 构造Hamilton函数**

$$
H=x^2(k)+u^2(k)+\lambda(k+1)[x(k)+u(k)]
$$

协态方程为

$$
\lambda(k)=\frac{\partial H(k)}{\partial x(k)}=2x(k)+\lambda(k+1)
$$

**② 边界条件**

$$
x(0)=x_0,\quad \lambda(N)=\frac{\partial \varphi[x(N),N]}{\partial x(N)}=0
$$

**③ 极值条件**

$$
\frac{\partial H}{\partial u}=2u(k)+\lambda(k+1)=0
$$

由 ① 和 ③ 可知，

$$
\left\{\begin{matrix}
\lambda(k+1)=\lambda(k)-2x(k)	\\
u(k)=-\frac{1}{2}\lambda(k+1)
\end{matrix}\right.
$$

**④ 求得结果**

由上式得到

$$
\left\{\begin{matrix}
u(0)=-\frac{1}{2}\lambda(1)\\
u(1)=-\frac{1}{2}\lambda(2)\\
u(2)=-\frac{1}{2}\lambda(3)=0\\
\end{matrix}\right.
$$

和

$$
\left\{\begin{matrix}
\lambda(2)=2x(2) \\
\lambda(1)=2[x(1)+x(2)] \\
\lambda(0)=2[x(1)+x(2)+x(2)]
\end{matrix}\right.
$$

由状态方程 $x(k+1)=x(k)+u(k)$ 得到代入控制量的等式中，得到

$$
\left\{\begin{matrix}
u(0)=-x(1)-x(2)\\
u(1)=-x(2)\\
u(2)=0
\end{matrix}\right.
$$

求出

$$
\left\{\begin{matrix}
x(1)=\frac{1}{2}[x(0)-x(2)] \\
x(2)=\frac{1}{2}x(1)		\\
x(3)=x(2)
\end{matrix}\right.
$$

根据初始条件 $x(0)=x_0$，求得最优轨线为

$$
\left\{\begin{matrix}
x^*(1)=\frac{2}{5}x_0 \\
x^*(2)=\frac{1}{5}x_0 \\
x^*(3)=\frac{1}{5}x_0
\end{matrix}\right.
$$


最优控制为

$$
\left\{\begin{matrix}
u^*(0)=-\frac{3}{5}x_0 \\
u^*(1)=-\frac{1}{5}x_0 \\
u^*(2)=0
\end{matrix}\right.
$$

就这么多，今天学习了连续系统和离散系统的极小值原理，对于应用来说已经足够了。至于后面的时间最优控制和燃料最优控制，等需要使用的时候再学习吧。掌握了最优控制的思想即可。




