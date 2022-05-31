---
permalink: /:year-:month-:day/:title
layout: post
title:  "数理战术学中微分方程的解法（思考题解法）"
author: "Lei Lie"
categories: Study
tag: notdouban
---
这篇文章摘抄自沙昌基教授《数理战术学》第七章 多兵种对多兵种作战的微分对策模型，提供了求解微分方程的代码，防止自己忘记。

在《数理战术学》第79页提到：

> 最优火力分配策略随时间变化的根本原因在于相对作战指数向量与实际交战过程中各类作战单位的作战效果很不一致，改变例1中乙方作战指数向量，则最优火力分配策略会发生相应变化，请读者自行证明。

此文为《数理战术学》该思考题的证明。

【例】最优火力分配矩阵随时间变化的例子

设一对二作战中双方的实力向量为

$$
x=[x_1],\quad y=\begin{bmatrix}
y_1 \\
y_2
\end{bmatrix}
$$

其初值为

$$
x_{10}=100,\quad y_{10}=30,\quad y_{20}=30
$$

双方毁伤系数矩阵为

$$
A=\begin{bmatrix}
1\\
1
\end{bmatrix},\quad
B = \begin{bmatrix}
9 & 1
\end{bmatrix}
$$

火力分配矩阵为

$$
\Phi=\begin{bmatrix}
\phi_1	\\
\phi_2
\end{bmatrix},\quad 
\Psi=\begin{bmatrix}
\psi_1 & \psi_2
\end{bmatrix}
$$

其中

$$
0 \le \psi_1 \le 1,\quad 0 \le \psi_2 \le 1, \\
0 \le \phi_1, \quad 0 \le \phi_2, \quad \phi_1+\phi_2 \le 1
$$

于是，双方交战的Lanchester方程变为

$$
\left\{\begin{matrix}
\begin{aligned}
 \dot x_1 &= -9\psi_1 y_1 - \psi_2 y_2, \\
 \dot y_1 &= -\phi_1 x_1, \\
 \dot y_2 &= -\phi_2 x_1.
\end{aligned}
\end{matrix}\right.
\tag{1}
$$

**设双方的作战指数向量为**

$$
u=(9),\quad v=\begin{bmatrix}
9	\\
1
\end{bmatrix}
$$

战斗时间为

$$
T=\frac{1}{6}\mathrm{ln}10+\mathrm{ln}\frac{10}{9}
$$

**则支付函数为**

$$
\mathit \Theta=\mathit \Theta(x_1(T),y_1(T),y_2(T))=9x_1(T)-9y_1(T)-y_2(T)
\tag{2}
$$

---

【解】

（1）构建Hamilton函数，为

$$
H=\lambda(-9\psi_1y_1-\psi_2y_2)+\mu_1(-\phi_1x_1)+\mu_2(-\phi_2x_1)
\tag{3}
$$

（2）根据Hamilton函数，求得协态方程为

$$
\left\{\begin{matrix}
\begin{aligned}
\dot \lambda &= -\frac{\partial H}{\partial x}=\mu_1\phi_1+\mu_2\phi_2,	\\
\dot \mu_1 &= -\frac{\partial H}{\partial y_1}=9\lambda\psi_1,	\\
\dot \mu_2 &= -\frac{\partial H}{\partial y_2}=\lambda\psi_2
\end{aligned}
\end{matrix}\right.
\tag{4}
$$

**（3）横截条件为**

$$
\left\{\begin{matrix}
\begin{aligned}
\lambda(T) &= \frac{\partial \mathit \Theta}{\partial x_1} \bigg |_T = 9,	\\
\mu_1(T) &= \frac{\partial \mathit \Theta}{\partial y_1} \bigg |_T = -9,		\\
\mu_2(T) &= \frac{\partial \mathit \Theta}{\partial y_2} \bigg |_T = -1	
\end{aligned}
\end{matrix}\right.
\tag{5}
$$

参考[单兵种对多兵种作战的微分对策模型](https://blog.csdn.net/Ruins_LEE/article/details/115872583)对于协态变量的分析可知，恒有

$$
\lambda(t)>0,\quad \mu_1(t)<0,\quad \mu_2(t)<0,\quad 0 \le t \le T.
$$

由鞍点条件可知，对于最优火力分配矩阵有

$$
\left\{\begin{matrix}
\psi_1(t)=\psi_2(t)=1,\quad 0 \le t \le T,	\\
\phi_1(t)+\phi_1(t)=1,\quad 0 \le t \le T
\end{matrix}\right.
\tag{6}
$$

且当$\mu_1(t)<\mu_2(t)$时（即变量$\mu_1(t)$让$H$负得更少），有

$$
\phi_1(t)=1,\quad \phi_2(t)=0.
\tag{7}
$$

当$\mu_1(t)>\mu_2(t)$时（即变量$\mu_2(t)$让$H$负得更少），有

$$
\phi_1(t)=0,\quad \phi_2(t)=1.
\tag{8}
$$

今因$\mu_1(T)<\mu_2(T)$，故在临近时刻 $T$ 的一个时间区间内上式均成立，即存在$\Delta \in [0,T)$，使得

$$
\mu_1(t) < \mu_2(t), \quad \Delta < t \le T
\tag{9}
$$

现在来考察$\Delta$的最小值，并将这最小值仍记作$\Delta$。如果该$\Delta>0$，则意味着$\Delta$再不能减小，从而必有

$$
\mu_1(\Delta)=\mu_2(\Delta)
\tag{9}
$$

由式$(7)$有

$$
\phi_1(t)=1, \quad \phi_2(t)=0, \quad \Delta < t \le T
$$

那么，联立式$(4)$、式$(6)$和式$(8)$，则式$(4)$变为

$$
\left\{\begin{matrix}
\begin{aligned}
\dot \lambda &= \mu_1,	\\
\dot \mu_1 &= 9\lambda,	\\
\dot \mu_2 &= \lambda,
\end{aligned}
\end{matrix}\right.
\quad \Delta < t \le T
$$

考虑终值条件式$(5)$进行求解。

此处编写MATLAB代码求解上述微分方程，仿真代码为

```matlab
%% 《数理战术学》第7.3节 式（7.23）、式（7.29）
% 【delta <= t <= T】 区间
syms lambda(t) mu1(t) mu2(t) T
eq1 = diff(lambda,t)==mu2;con1 = lambda(T)==9;
eq2 = diff(mu1,t)==9*lambda;con2 = mu1(T)==-1;
eq3 = diff(mu2,t)==lambda;con3 = mu2(T)==-9;
sol1 = dsolve(eq1,eq2,eq3,con1,con2,con3);
```

因在$t=T$处有$\mu_1(t)<\mu_2(t)$，此处需要考察$\mu_1(t)$和$\mu_2(t)$的初值。

如果有$\mu_1(0)<\mu_2(0)$，则在$t \in [0,T]$内恒有$\mu_1(t)<\mu_2(t)$。

如果有$\mu_1(0)>\mu_2(0)$，则在$t \in [0,\Delta]$内恒有$\mu_1(t)<\mu_2(t)$，在$t \in (\Delta,T]$内恒有$\mu_1(t)>\mu_2(t)$。

即如下图（A）、（B）所示情况。

![]({{site.page}}/images/img-2021-10-20/fig8.webp)

上述MATLAB代码已经求得$\mu_1(t)$和$\mu_2(t)$的表达式，因此只需再在MATLAB中编写代码求解$\mu_1(0)$和$\mu_2(0)$即可。

MATLAB代码为

```matlab
%% 《数理战术学》第7.3节 式（7.30）
syms delta
res.lambda = subs(sol1.lambda, t, 0);
res.mu1 = subs(sol1.mu1, t, 0);
res.mu2 = subs(sol1.mu2, t, 0);
if eval(res.mu1) < eval(res.mu2)
   disp('mu1 恒小于 mu2'); 
else
    disp('mu1 不恒小于 mu2'); 
end
```

结果为

```matlab
eval(res.mu1)=-52.101934085986272
eval(res.mu2)=-14.677992676220697
```

说明$\mu_1(0)<\mu_2(0)$，那么有$t \in [0,T]$内$\mu_1(t)<\mu_2(t)$。

即如下图所示情况。

![fig9]({{site.page}}/images/img-2021-10-20/fig9.webp)

也就是说，在$t \in [0,T]$内，甲方的火力分配策略不会改变，持续攻击乙方第一类作战单位。该结果与[上一篇文章](https://blog.csdn.net/Ruins_LEE/article/details/120733615)的结果有所区别。

证明了书中结论：**作战指数改变后，会导致最优火力分配策略改变**。

此处可以看出，甲方与乙方第一类作战单位的交战强度比甲方与乙方第二类作战单位的交战强度更加“激烈”，并且考虑乙方第一类作战单位的作战指数大于乙方第二类作战单位的作战指数，说明乙方第一类作战单位的重要程度强于第二类作战单位的重要程度。

按照数理战术学理论，双方倾向于先完成交战强度“激烈”的交战格局。因此甲方先与乙方第一类作战单位是合理的。在交战过程中，先消灭交战强度“激烈”的敌方作战单位，减少对己方的威胁，保存己方实力，再消灭对己方威胁程度较低的敌方作战单位，符合常规作战认知。