---
permalink: /:year-:month-:day/:title
layout: post
title:  "最优控制中的变分法"
author: "Lei Lie"
categories: Study
tag: notdouban
---

# 第2章 最优控制中的变分法

最优控制问题是在一定的约束条件下，寻求使性能达到极值时的控制函数。

## 2.1 欧拉方程

### 2.1.1 无约束泛函极值的必要条件

无约束泛函极值问题为

$$
\min_xJ(x)=\int_{t_0}^{t_f}  L(x,\dot{x},t)\mathrm{d}t
\tag{1}
$$

**【定理】** 对于式 $(1)$，使其取极值的必要条件，是轨线*x(t)*满足下列欧拉方程

$$
\frac{\partial L}{\partial x}-\frac{\mathrm{d}}{\mathrm{d}t}
\frac{\partial L}{\partial \dot{x}}=0
$$

### 2.1.2 有约束泛函极值的必要条件

有等式约束的泛函极值问题为

$$
\begin{matrix}
\min_xJ(x)=\int_{t_0}^{t_f}  L(x,\dot{x},t)\mathrm{d}t\\
\mathrm{s. t. }\quad f(x,\dot{x},t )=0
\end{matrix}
\tag{2}
$$

**【定理】** 对于式 $(2)$，使其取极值的必要条件，是轨线 *x(t)* 满足下列欧拉方程

$$
\frac{\partial L}{\partial x}-\frac{\mathrm{d}}{\mathrm{d}t}
\frac{\partial L}{\partial \dot{x}}=0
$$

式中

$$
L(x,\dot{x},\lambda,t)=g(x,\dot{x},\lambda)+\lambda^\mathrm{T}(t)f(x,\dot{x},\lambda)
$$

$\lambda \in R^n$ 为待定拉格朗日乘子向量。

### 2.1.3 无约束泛函极值的充分条件

使式 $(1)$ 成立的充分条件是：除了欧拉方程应成立之外，下列等价勒让德条件之一应成立：

$$
\begin{bmatrix}
  \frac{\partial^2 L}{\partial x^2} &  \frac{\partial^2 L}{\partial x \partial \dot{x} } \\
  (\frac{\partial^2 L}{\partial x \partial \dot{x} })^\mathrm{T} & \frac{\partial^2 L}{\partial \dot{x}^2} 
\end{bmatrix}
\tag{3}
$$

$$
\frac{\partial^2 L}{\partial x^2}-
\frac{\mathrm{d}}{\mathrm{d}t} \frac{\partial^2 L}{\partial x \partial \dot{x} }\geq 0, 
\quad \frac{\partial^2 L}{\partial \dot{x}^2}>0
\tag{4}
$$

$$
\frac{\partial^2 L}{\partial x^2}-
\frac{\mathrm{d}}{\mathrm{d}t} \frac{\partial^2 L}{\partial x \partial \dot{x} } > 0, 
\quad \frac{\partial^2 L}{\partial \dot{x}^2} \geq 0
\tag{5}
$$

### 2.1.4 有约束泛函极值的充分条件

使式 $(2)$ 成立的充分条件是：除了欧拉方程应成立和约束条件满足之外，下列等价勒让德条件之一应成立：

$$
\begin{bmatrix}
  \frac{\partial^2 L}{\partial x^2} &  \frac{\partial^2 L}{\partial x \partial \dot{x} } \\
  (\frac{\partial^2 L}{\partial x \partial \dot{x} })^\mathrm{T} & \frac{\partial^2 L}{\partial \dot{x}^2} 
\end{bmatrix}
\tag{6}
$$

$$
\frac{\partial^2 L}{\partial x^2}-
\frac{\mathrm{d}}{\mathrm{d}t} \frac{\partial^2 L}{\partial x \partial \dot{x} }\geq 0, 
\quad \frac{\partial^2 L}{\partial \dot{x}^2}>0
\tag{7}
$$

$$
\frac{\partial^2 L}{\partial x^2}-
\frac{\mathrm{d}}{\mathrm{d}t} \frac{\partial^2 L}{\partial x \partial \dot{x} } > 0, 
\quad \frac{\partial^2 L}{\partial \dot{x}^2} \geq 0
\tag{8}
$$

式中

$$
L(x,\dot{x},\lambda,t)=g(x,\dot{x},\lambda)+\lambda^\mathrm{T}(t)f(x,\dot{x},\lambda)
$$

$\lambda \in R^n$ 为待定拉格朗日乘子向量。

## 2.2 横截条件

求解欧拉方程，需要由横截条件提供两点边界值[^注1]。

在实际工程中，会出现很多种情况：

- 初始时刻 $t_0$ 和末端时刻 $t_f$ 自由；
- 初始时刻 $t_0$ 和末端时刻 $t_f$ 固定；
- 初始时刻 $t_0$ 固定、末端时刻 $t_f$ 自由；
- 初始时刻 $t_0$ 自由、末端时刻 $t_f$ 固定。

### 2.2.1 末端时刻固定时的横截条件

当末端时刻固定时，由泛函极值的必要条件可知，横截条件的一般表达式为

$$
(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\delta x\bigg |^{t_f}_{t_0}
=(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg |_{t=t_f}\delta x(t_f)
-(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg |_{t=t_0}\delta x(t_0)
=0
\tag{9}
$$

只要不是两端固定问题，宗量变分 $\delta x(t_f)$ 和 $\delta x(t_0)$ 不能同时为零，使泛函一次变分 $\delta J=0$ 缺少条件，就应当由极值的基本必要条件来补足：或是 $(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg \|_{t_f}=0$，或是 $(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg \|_{t_0}=0$.

如果把初始状态 $x(t_0)$ 称为起点，把末端状态  $x(t_f)$  称为终点，有如下几个性质：

- 起点固定时，$x(t_0)=x_0,\quad \delta x(t_0)=0$ ；
- 起点自由时，$x(t_0) \ne 0$ ；
- 终点固定时，$x(t_f)=x_f,\quad \delta x(t_f)=0$ ；
- 终点自由时，$x(t_f) \ne x_f$ 。

则得到末端时刻 $t_f$ 固定时的各种横截条件表格。

| 序号 |        名称        |                           横截条件                           |
| :--: | :----------------: | :----------------------------------------------------------: |
|  1   | 固定起点、固定终点 |                $x(t_0)=x_0,\quad x(t_f)=x_f$                 |
|  2   | 自由起点、自由终点 | $\frac{\partial L}{\partial \dot{x}}\bigg \|_{t_0}=0,\quad \frac{\partial L}{\partial \dot{x}}\bigg \|_{t_f}=0$ |
|  3   | 自由起点、固定终点 | $\frac{\partial L}{\partial \dot{x}}\bigg \|_{t_0}=0,\quad x(t_f)=x_f$ |
|  4   | 固定起点、自由终点 | $x(t_0)=x_0,\quad \frac{\partial L}{\partial \dot{x}}\bigg \|_{t_f}=0$ |

注：这里jekyll有bug，没法正常显示代码中的“丨”，一旦有2个“丨”就会识别成markdown的表格符号，大家可以通过latex编辑器来看一下没有显示出来的公式。

### 2.2.2 末端时刻自由时的横截条件

末端时刻自由问题的实质是，末端时间 $t_f$ 不固定，末端状态或自由、或受约束，属于变动端点问题。

经过一系列运算后，得到末端变动、$t_f$ 自由时泛函极值的必要条件

$$
\frac{\partial L}{\partial x}-\frac{\mathrm{d}}{\mathrm{d}t}
\frac{\partial L}{\partial \dot{x}}=0
$$

以及

$$
(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg |_{t_f}\delta x(t_f)
-(\frac{\partial L}{\partial \dot{x}})^\mathrm{T}\bigg |_{t_0}\delta x(t_0)+L(x^*,\dot{x}^*,t)\bigg
|_{t_f}\delta t_f
=0
\tag{10}
$$

式 $(10)$ 称为横截条件，除提供求解欧拉方程所需的两点边界值外，还提供了一个确定最优末端时间 $t_f^*$ 所需边界条件。

**（1）起点固定、末端自由**

有 $x(t_0)=x_0,\quad \delta x(t_0)=0$。

整理得公式为

$$
\left(L-\dot{x}^\mathrm{T}(t)\frac{\partial L}{\partial \dot{x}}\right)\bigg |_{t_f}\delta t_f+
\left(\frac{\partial L}{\partial \dot{x}}\right)^\mathrm{T}\bigg |_{t_f}\delta x_f
=\mathbf{0}
\tag{11}
$$

因 $\delta t_f$ 和$\delta x_f$ 任意，即$\delta t_f \ne 0$、$\delta x_f \ne 0$ ，故横截条件为

$$
\left\{\begin{matrix}
 \left(L-\dot{x}^\mathrm{T}(t)\frac{\partial L}{\partial \dot{x}}\right)\bigg |_{t_f}\delta t_f =0\\
 \left(\frac{\partial L}{\partial \dot{x}}\right) = 0\\
x(t_0)=x_0
\end{matrix}\right.
$$

**（2）起点固定、末端受约束**

设末端约束方程为

$$
x(t_f)=c(t_f)
$$

因为末端受约束，则 $\delta x_f$ 不能任意。那么横截条件在式 $(11)$ 的基础上进一步演化为

$$
\left(L-\dot{x}^\mathrm{T}(t)\frac{\partial L}{\partial \dot{x}}\right)\bigg |_{t_f}\delta t_f+
\left(\frac{\partial L}{\partial \dot{x}}\right)^\mathrm{T}\bigg |_{t_f}\dot{c}(t_f)\delta t_f
=\left[L+(\dot{c}-\dot{x}^\mathrm{T})\right]\bigg |_{t_f}\delta t_f=0
$$

因 $\delta t_f$ 任意，即$\delta t_f \ne 0$，故横截条件为

$$
\left\{\begin{matrix}
 \left[L+(\dot{c}-\dot{x}^\mathrm{T})\right]\bigg |_{t_f} =0\\
x(t_f)=c(t_f)\\
x(t_0)=x_0
\end{matrix}\right.
$$

### 2.2.3 初始时刻自由时的横截条件

初始时刻自由问题的实质时：末端固定 $x(t_f)=x_f$，初始时刻  $t_0$ 不固定，初始状态 $x(t_0)$ 或自由、或受约束。例如，空对地导弹的攻击。

如果把初始状态 $x(t_0)$ 称为起点，把末端状态  $x(t_f)$  称为终点，有如下几个性质：

- 起点固定时，$x(t_0)=x_0,\quad \delta x(t_0)=0$ ；
- 起点自由时，$x(t_0) \ne 0$ ；
- 终点固定时，$x(t_f)=x_f,\quad \delta x(t_f)=0$ ；
- 终点自由时，$x(t_f) \ne x_f$ 。

则得到末端时刻 $t_f$ 固定时的各种横截条件表格。

| 序号 |         名称         |                           横截条件                           |
| :--: | :------------------: | :----------------------------------------------------------: |
|  1   |  固定起点、末端自由  | $\left(L-\dot{x}^\mathrm{T}\frac{\partial L}{\partial \dot{x}}\right)\bigg \|_{t_f}=0\\ (\frac{\partial L}{\partial \dot{x}})\bigg \|_{t_f}=0, \quad x(t_0) = x_0$ |
|  2   | 起点固定、末端受约束 | $\left(L-(\dot{c}-\dot{x})^\mathrm{T}\frac{\partial L}{\partial \dot{x}}\right)\bigg \|_{t_f}=0\\ x(t_f)=c(t_f), \quad x(t_0) = x_0$ |
|  3   | 末端固定、起点受约束 | $\left(L-(\dot{\Psi_0}-\dot{x})^\mathrm{T}\frac{\partial L}{\partial \dot{x}}\right)\bigg \|_{t_f}=0\\ x(t_0)=\Psi_0(t_0), \quad x(t_f) = x_f$ |
|  4   |  末端固定、起点自由  | $\left(L-\dot{x}^\mathrm{T}\frac{\partial L}{\partial \dot{x}}\right)\bigg \|_{t_0}=0\\ (\frac{\partial L}{\partial \dot{x}})\bigg \|_{t_0}=0, \quad x(t_f) = x_f$ |

注：这里jekyll有bug，没法正常显示代码中的“丨”，一旦有2个“丨”就会识别成markdown的表格符号，大家可以通过latex编辑器来看一下没有显示出来的公式。

## 2.3 用变分法解最优控制问题

对于一般的最优控制问题，应广泛采用**拉格朗日乘子法**，引入**哈密顿函数概念**，将泛函条件极值问题转化为无约束泛函极值问题，以获得最优解的必要条件和充分条件。

### 2.3.1 末端时刻固定时的最优解

当末端时刻 $t_f$ 固定时，最优控制问题可以归结为如下一般形式：

$$
\min_{u(t)}J=\varphi[x(t_f)]+ \int_{t_0}^{t_f}  L(x,u,t)\mathrm{d}t		\\
\mathrm{s.\ t.} \quad f(x,u,t)-\dot{x}(t)=0, \quad x(t_0) =x_0	\\ \Psi [x(t_f)]=0
$$

假设各向量维数同前。末端状态 $x(t_f)$ 存在如下状态：

- 末端状态**受约束**；
- 末端状态**自由**；
- 末端状态**固定**。

若末端自由，则 $\Psi [x(t_f)]=0$ 不会出现；若末端固定，则因 $\delta x(t_f)=0$，那么横截条件 $\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x(t_f)}\gamma$ 将不存在。

下面给出了==末端时刻**固定**时最优解的必要条件==。

**【定理】** 对于如下最优控制问题

$$
\min_{u(t)}J=\varphi[x(t_f)]+ \int_{t_0}^{t_f}  L(x,u,t)\mathrm{d}t，\quad t_f固定		\\
\mathrm{s.\ t.} \quad f(x,u,t)-\dot{x}(t)=0, \quad x(t_0) =x_0	\\ \Psi [x(t_f)]=0，\quad （末端受约束情况有此项、末端自由和末端固定没有此项）	
\tag{12}
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

a）若末端**受约束**，则有

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x(t_f)}\gamma \\
\Psi[x(t_f)]=0
$$

按我理解，上式应写为

$$
x_i(t_0) = x_{i0} \\
\lambda_i(t_f)=\frac{\partial \varphi}{\partial x_i(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x_i(t_f)}\gamma \\
\Psi[x(t_f)]=0
$$

式中，$i$ 代表第$i$ 个微分方程（或动态系统方程）。

b）若末端**自由**，则有

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)}
$$

c）若末端**固定**，则有

$$
x(t_0) = x_0 \\
x(t_f) = x_f
$$

**③ 极值条件**


$$
\frac{\partial H}{\partial u}=0
$$

### 总结

- 先找对应的模型，或是末端**受约束**，或是末端**自由**，或是末端**固定**。

- 按照“ ① 求解正则方程 —> ② 边界条件 —> ③ 极值条件”的顺序解题。解到 “③ 极值条件”一般可以求出 $u(t)$ ，再把 $u(t)$ 代回求解 $\lambda(t)$ 或 $x(t)$ ，将末端时刻 $t_f$ 代入方程中，可求出最优轨线 $x^*(t)$ 和最优控制解 $u^*(t)$ 。

---

### 例题

（最优控制理论与系统【胡寿松】, 第三版, pp35-36）试求被控系统

$$
\left\{\begin{matrix}
\dot x_1(t) = x_2(t)\\
\dot x_2(t) = u(t)
\end{matrix}\right.
$$

由已知初态 $x_1(0)=0$ 和 $x_2(0)=0$ 出发，在 $t_f=1$ 时转移到目标集合

$$
x_1(1)+x_2(1)=1
$$

且使性能指标

$$
J=\frac{1}{2}\int_{0}^{1}u^2(t)\mathrm{d}t
$$

为最小的最优控制 $u^*(t)$ 及相应的最优轨迹 $x^*(t)$。

---------------------

**【解】**      本题是积分型性能指标、 $t_f$固定、末端受约束的最优化问题。按照**总结**中提到的解题步骤来解题。

**① 求正则方程$x(t)$ 和 $\lambda (t)$**

构建哈密顿函数

$$
H = \frac{1}{2}u^2(t) + \lambda_1 x_2(t) + \lambda_2 u(t)
$$

求解正则方程

$$
\dot x_1 = \frac{\partial H}{\partial \lambda_1}=x_2(t), 
\quad \dot x_2 = \frac{\partial H}{\partial \lambda_2}=u(t) \\
\dot \lambda_1 = -\frac{\partial H}{\partial x_1}=0, 
\quad \lambda_2 = -\frac{\partial H}{\partial x_2}=-\lambda_1(t)
$$

**② 边界条件**

$$
x_1(0)=0,\quad x_2(0)=0 \\
\lambda_1(1)=\frac{\partial \varphi}{\partial x_1(1)} +\frac{\partial \Psi^\mathrm{T}}{\partial x_1(1)}\gamma=\gamma,\quad 
\lambda_2(1)=\frac{\partial \varphi}{\partial x_2(1)} +\frac{\partial \Psi^\mathrm{T}}{\partial x_2(1)}\gamma=\gamma \\
x_1(1)+x_2(1)=1
$$

**③ 极值条件**

$$
\frac{\partial H}{\partial u}=u(t)+\lambda(t)=0\Rightarrow u(t)=-\lambda(t)
$$

又因为

$$
\dot \lambda_1(t) = 0 \Rightarrow \lambda_1(t) = c_1 \\
\dot \lambda_2(t) = -\lambda_1(t) =-c_1 \Rightarrow \lambda_2(t) = -c_1 t + c_1
$$

得到

$$
u(t) = -\lambda_2(t) = -c_1 t + c_1
$$

那么

$$
\dot x_2(t)=u(t)=-c_1 t+c_1 \Rightarrow x_2(t)=\frac{1}{2}c_1t^2-c_2t+c_3 \\
\dot x_1(t)=x_2(t)=\frac{1}{2}c_1t^2-c_2t+c_3\Rightarrow x_1(t)=\frac{1}{6}c_1t^3-\frac{1}{2}c_2t^2+c_3t+c_4
$$

**④ 解方程**

代入初值 $x_1(0)=0$ 、 $x_2(0)=0$ 以及末端状态 $x_1(1)+x_2(1)=1$，得到

$$
\left\{\begin{matrix}
x_1(0)=c_4=0 \\
x_2(0)=c_3=0 \\
x_1(1)+x_2(2)=\frac{1}{6}c_1-\frac{1}{2}c_2+\frac{1}{2}c_1-c_2=\frac{2}{3}c_1-\frac{3}{2}c_2=1
\end{matrix}\right.
$$

又因为

$$
\lambda_1(1)=\lambda_2(1)=\gamma
$$

得到

$$
c_1=-c_1 \times1 + c_2
$$

解得

$$
c_1=\frac{1}{2}c_2
$$

代回上式，得到 $c_1$ 、 $c_2$ 、 $c_3$、 $c_4$的值为

$$
\left\{\begin{matrix}
c_1 = -\frac{3}{7} \\
c_2 = -\frac{6}{7} \\
c_3 = 0 \\
c_4 = 0
\end{matrix}\right.
$$

**⑤ 求解结果**

最优轨线

$$
\left\{\begin{matrix}
x_1^*(t)=-\frac{1}{14}t^3+\frac{3}{7}t^2 \\
x_2^*(t)=-\frac{3}{14}t^2+\frac{6}{7}t
\end{matrix}\right.
$$

最优控制

$$
u^*(t)=-\frac{3}{7}t+\frac{6}{7}
$$

代码为

```matlab
clear;clc;close all;
syms x1 x2 u
% 系统方程
Dx1 = x2;
Dx2 = u;
% 代价函数
syms g
g = 0.5*u^2;

% Step 1：构造Hamilton函数，求正则方程
syms lambda1 lambda2
H = g + lambda1*Dx1 + lambda1*Dx2;
% 求解正则方程
Dlambda1 = -diff(H,x1);
% Dlambda2 = -diff(H,x2);

% Step 2：边界条件
syms Psi gamma
Psi = x1 + x2 - 1;
lambda1(1) = diff(Psi,'x1')*gamma;
% lambda2(1) = diff(Psi,'x2')*gamma;

% Step 3：极值条件
dU = diff(H,'u');
% sol_u = solve(dU,u);

% Step 4：解方程
syms t c1 c2 c3 c4 gamma
Flambda1 = int(Dlambda1);
if Flambda1 == '0'
    Flambda1 = c1;
end
Int_lambda1 = Flambda1;
Dlambda2 = Int_lambda1;
Flambda2 = -int(Dlambda2,t) + c2;
Int_lambda2 = Flambda2;
sol_u = -Int_lambda2;

Dx2 = subs(Dx2,u,sol_u);
x2 = int(Dx2) + c3;
Dx1 = x2;
% x1 = int(Dx1) + c4;

eq1 = c1 == gamma;
eq2 = -c1 + c2 == gamma;
eq3 = (1/6)*c1 - (1/2)*c2 + (1/2)*c1 - c2 == 1;
s = solve(eq1,eq2,eq3,c1,c2,gamma);

c1 = s.c1;
c2 = s.c2;
c3 = 0;
c4 = 0;
x1 = (c1*t^3)/6 - (c2*t^2)/2 + c3*t + c4
x2 = (c1*t^2)/2 - c2*t + c3
lambda2 = -c1*t + c2;
u = -lambda2
```



下面给出了==末端时刻固定时最优解的充分条件==。

**【定理】**  

对于如下最优控制问题

$$
\min_{u(t)}J=\varphi[x(t_f)]+ \int_{t_0}^{t_f}  L(x,u,t)\mathrm{d}t，\quad t_f固定		\\
\mathrm{s.\ t.} \quad f(x,u,t)-\dot{x}(t)=0, \quad x(t_0) =x_0	\\ \Psi [x(t_f)]=0
$$

式中 $t_f$ 固定。是性能泛函 $J$ 取极小值的最优解的充分条件是下列等价勒让德条件之一成立：

$$
\begin{bmatrix}
  \frac{\partial^2 H}{\partial x^2} &  \frac{\partial^2 H}{\partial u \partial x } \\
  (\frac{\partial^2 H}{\partial x \partial u })^\mathrm{T} & \frac{\partial^2 H}{\partial u^2}  
\end{bmatrix}>0, \quad
\frac{\partial^2 \Theta[x(t_f),\gamma]}{\partial x^2(t_f)} \geq 0
$$

$$
\begin{bmatrix}
  \frac{\partial^2 H}{\partial x^2} &  \frac{\partial^2 H}{\partial u \partial x } \\
  (\frac{\partial^2 H}{\partial x \partial u })^\mathrm{T} & \frac{\partial^2 H}{\partial u^2}  
\end{bmatrix}\geq 0, \quad
\frac{\partial^2 \Theta[x(t_f),\gamma]}{\partial x^2(t_f)} > 0
$$

式中

$$
H(x,u,\lambda,t)=L(x,u,t)+\lambda^{\mathrm{T}}(t)f(x,u,t) \\
\Theta[x(t_f),\gamma]=\varphi[x(t_f)]+\gamma^{\mathrm{T}}\mathit{\Psi}[x(t_f)]
$$

### 2.3.2 末端时刻自由时的最优解

当末端时刻 $t_f$ 自由时，末端状态又可区分为**受约束**、**自由**或**固定**三种情况。

当末端时刻 $t_f$ 固定时，最优控制问题可以归结为如下一般形式：

$$
\min_{u(t)}J=\varphi[x(t_f)]+ \int_{t_0}^{t_f}  L(x,u,t)\mathrm{d}t		\\
\mathrm{s.\ t.} \quad f(x,u,t)-\dot{x}(t)=0, \quad x(t_0) =x_0	\\ \Psi [x(t_f)]=0
$$

假设各向量维数同前。末端状态 $x(t_f)$ 存在如下状态：

- 末端状态**受约束**；
- 末端状态**自由**；
- 末端状态**固定**。

若末端自由，则 $\Psi [x(t_f)]=0$ 不会出现；若末端固定，则因 $\delta x(t_f)=0$，那么横截条件 $\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x(t_f)}\gamma$ 将不存在。

下面给出了==末端时刻**自由**时最优解的必要条件==。

**【定理】** 对于如下最优控制问题

$$
\min_{u(t)}J=\varphi[x(t_f)]+ \int_{t_0}^{t_f}  L(x,u,t)\mathrm{d}t，\quad t_f固定		\\
\mathrm{s.\ t.} \quad f(x,u,t)-\dot{x}(t)=0, \quad x(t_0) =x_0	\\ \Psi [x(t_f)]=0，\quad （末端受约束情况有此项、末端自由和末端固定没有此项）	
\tag{12}
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

a）若末端**受约束**，则有

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x(t_f)}\gamma \\
\Psi[x(t_f)]=0
$$

按我理解，上式应写为

$$
x_i(t_0) = x_{i0} \\
\lambda_i(t_f)=\frac{\partial \varphi}{\partial x_i(t_f)} +\frac{\partial \Psi^\mathrm{T}}{\partial x_i(t_f)}\gamma \\
\Psi[x(t_f)]=0
$$

式中，$i$ 代表第$i$ 个微分方程（或动态系统方程）。

b）若末端**自由**，则有

$$
x(t_0) = x_0 \\
\lambda(t_f)=\frac{\partial \varphi}{\partial x(t_f)}
$$

c）若末端**固定**，则有

$$
x(t_0) = x_0 \\
x(t_f) = x_f
$$

**③ 极值条件**


$$
\frac{\partial H}{\partial u}=0
$$

**④ 哈密顿函数在最优轨线末端满足**

a）若末端**受约束**，则有

$$
H(t_f)=-\frac{\partial \varphi}{\partial t_f}-\gamma^{\mathrm{T}}(t_f)\frac{\partial \Psi}{\partial t_f}
$$

b）若末端**自由**，则有

$$
H(t_f)=-\frac{\partial \varphi}{\partial t_f}
$$

c）若末端**固定**，则有

$$
H(t_f)=-\frac{\partial \varphi}{\partial t_f}
$$

### 总结

- 先找对应的模型，或是末端**受约束**，或是末端**自由**，或是末端**固定**。

- 按照“ ① 求解正则方程 —> ② 边界条件 —> ③ 极值条件 —> ④ 哈密顿函数在最优轨线末端满足条件”的顺序解题。解到 “③ 极值条件”一般可以求出 $u(t)$ ，解到 “④ 哈密顿函数在最优轨线末端满足条件”可以求出 $u(t)$ 的真正表达式，再把 $u(t)$ 代回求解 $\lambda(t)$ 或 $x(t)$ ，将末端时刻 $t_f$ 代入方程中，可求出最优轨线 $x^*(t)$ 和最优控制解 $u^*(t)$ 。

---

### 例题

（最优控制理论与系统【胡寿松】, 第三版, pp40-41）设一阶系统方程为

$$
\dot x(t)=u(t)
$$

性能指标取为

$$
 J=t_f+\frac{1}{2}\int_0^{t_f}u^2(t)\mathrm{d}t
$$

式中 $t_f$ 自由。试确定最优控制 $u^*(t)$，使系统由 $x(0)=1$ 转移到 $x(t_f)=0$ ，并使性能指标为极小值。

---

**【解】**      本题是复合型性能指标、 $t_f$自由、末端固定、控制变量取值无约束的最优化问题。按照**总结**中提到的解题步骤来解题。

**① 求正则方程$x(t)$** 和 $\lambda (t)$ 

构建哈密顿函数

$$
H = \frac{1}{2}u^2(t)+\lambda u(t)
$$

求解正则方程

$$
\dot x = \frac{\partial H}{\partial \lambda_1}=u(t) \\
\dot \lambda = -\frac{\partial H}{\partial x}=0 \Rightarrow \lambda(t)=c_1
$$

**② 边界条件**

$$
x(0)=0,\quad x(t_f)=0 \\
$$

**③ 极值条件**

$$
\frac{\partial H}{\partial u}=u(t)+\lambda(t)=0\Rightarrow u(t)=-\lambda(t)=-c_1
$$

**④ 哈密顿函数在最优轨线末端满足**

因为末端**固定**，所以

$$
H(t_f)=-\frac{\partial \varphi}{\partial t_f}=-1 \\
\Rightarrow \frac{1}{2}u^2(1)+c_1u(1)=-1		 \\
\Rightarrow \frac{1}{2}c_1^2+c_1^2=-1 			 \\
\Rightarrow c_1^2=\frac{1}{2} 					 \\
\Rightarrow c_1=\sqrt{2}
$$

那么


$$
\dot x(t)=-c_1=-\sqrt{2}        \\
\Rightarrow x(t)=-\sqrt{2}t+c_2
$$

因为

$$
x(0)=c_2=1
$$

所以

$$
x(t)=-\sqrt{2}t+1
$$

又

$$
x(t_f)=-\sqrt{2}t_f+1=0 \\
\Rightarrow t_f=\frac{1}{\sqrt{2}}
$$

**⑤ 求解结果**

最优轨线

$$
x^*(t)=-\sqrt{2}t+1
$$

最优控制

$$
u^*(t)=-\sqrt{2}
$$

**思考**：按照步骤一步一步解即可，书上的解法反而让人感觉眼花缭乱，摸不着重点。

至此，使用变分法解最优控制问题便已全部介绍完毕。

[^注1]: 初始时刻和末端时刻的值



