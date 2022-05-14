---
permalink: /:year-:month-:day/:title
layout: post
title:  "数理战术学中微分方程的解法"
author: "Lei Lie"
categories: Study
tag: notdouban
---
这篇文章摘抄自沙昌基教授《数理战术学》第七章 多兵种对多兵种作战的微分对策模型，提供了求解微分方程的代码，防止自己忘记。

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

设双方的作战指数向量为

$$
u=(9),\quad v=\begin{bmatrix}
1	\\
9
\end{bmatrix}
$$

战斗时间为

$$
T=\frac{1}{6}\mathrm{ln}10+\mathrm{ln}\frac{10}{9}
$$

则支付函数为

$$
\Theta=\Theta(x_1(T),y_1(T),y_2(T))=9x_1(T)-y_1(T)-9y_2(T)
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

（3）横截条件为

$$
\left\{\begin{matrix}
\begin{aligned}
\lambda(T) &= \frac{\partial \mathit \Theta}{\partial x_1} \bigg |_T = 9,	\\
\mu_1(T) &= \frac{\partial \mathit \Theta}{\partial y_1} \bigg |_T = -1,		\\
\mu_2(T) &= \frac{\partial \mathit \Theta}{\partial y_2} \bigg |_T = -9	
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

且当$-\mu_1(t)>-\mu_2(t)$时，有

$$
\phi_1(t)=1,\quad \phi_2(t)=0.
\tag{7}
$$

当$\mu_1(t)<-\mu_2(t)$时，有

$$
\phi_1(t)=0,\quad \phi_2(t)=1.
\tag{8}
$$

今因$-\mu_1(T)<-\mu_2(T)$，故在临近时刻 $T$ 的一个时间区间内上式均成立，即存在$\Delta \in [0,T)$，使得

$$
-\mu_1(t) < -\mu_2(t), \quad \Delta < t \le T
\tag{9}
$$

现在来考察$\Delta$的最小值，并将这最小值仍记作$\Delta$。如果该$\Delta>0$，则意味着$\Delta$再不能减小，从而必有

$$
-\mu_1(\Delta)=-\mu_2(\Delta)
\tag{9}
$$

由式$(8)$有

$$
\phi_1(t)=0, \quad \phi_2(t)=1, \quad \Delta < t \le T
$$

那么，联立式$(4)$、式$(6)$和式$(8)$，则式$(4)$变为

$$
\left\{\begin{matrix}
\begin{aligned}
\dot \lambda &= \mu_2,	\\
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
% 【delta < t <= T】 区间
syms lambda(t) mu1(t) mu2(t) T
eq1 = diff(lambda,t)==mu2;con1 = lambda(T)==9;
eq2 = diff(mu1,t)==9*lambda;con2 = mu1(T)==-1;
eq3 = diff(mu2,t)==lambda;con3 = mu2(T)==-9;
sol1 = dsolve(eq1,eq2,eq3,con1,con2,con3);
```

求解结果为

![fig1]({{site.page}}/images/img-2021-10-20/fig1.png)

书中求解结果为

$$
\left\{\begin{matrix}
\begin{aligned}
\lambda &= 9e^{T-t},	\\
\mu_1 &= -81e^{T-t} + 80, \\
\mu_2 &= -9e^{T-t},
\end{aligned}
\end{matrix}\right.
\tag{10}
$$

MATLAB求解结果与书中求解结果相同。

下面来求$\Delta$，因式$(10)$在$\Delta \le t \le T$中成立，且$\Delta$不能再小，故由式$(9)$可得

$$
\begin{aligned}
-81e^{T-\Delta}+80&=-9e^{T-\Delta}	\\
\Rightarrow e^{T-\Delta}&=\frac{10}{9}	\\
\Rightarrow T-\Delta&=\mathrm{ln}\frac{10}{9}	\\
\Rightarrow \Delta&=T-\mathrm{ln}\frac{10}{9}	\\
&=\frac{1}{6}\mathrm{ln}10
\end{aligned}
\tag{11}
$$

此处求解$\Delta$的MATLAB代码为

```matlab
%% 《数理战术学》第7.3节 式（7.30）
syms delta
T = (1/6)*log(10) + log((10/9));
res.lambda = subs(sol1.lambda, t, delta);
res.mu1 = subs(sol1.mu1, t, delta);
res.mu2 = subs(sol1.mu2, t, delta);
res.delta = solve(res.mu1==res.mu2, delta);
res.delta = eval(res.delta);
```

求解结果为

![fig2]({{site.page}}/images/img-2021-10-20/fig2.png)

与书中结果相同。

于是，得到

$$
\left\{\begin{matrix}
\begin{aligned}
\lambda(\Delta) &= 9 \cdot \frac{10}{9} = 10,	\\
\mu_1(\Delta) &= -81 \cdot \frac{10}{9} + 80 = -10,	\\
\mu_2(\Delta) &= -9 \cdot \frac{10}{9} = -10.
\end{aligned}
\end{matrix}\right.
\tag{12}
$$

在$t=\Delta$处，因$\lambda(\Delta)=10>0$，故

$$
\dot \mu_1 (\Delta) = 9\lambda(\Delta)=90>\dot \mu_2(\Delta) = \lambda(\Delta) = 10.
$$

于是在$t=\Delta$的左邻域内，有

$$
-\mu_1(t) > -\mu_2(t)
\tag{13}
$$

由式$(13)$和式$(7)$，有

$$
\phi_1(t)=1, \quad \phi_2(t)=0, \quad 0 \le t < \Delta.
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
\quad 0 \le t \le \Delta
$$

考虑终值条件式$(12)$进行求解。

此处编写MATLAB代码求解上述微分方程，仿真代码为

```matlab
%% 《数理战术学》第7.3节 式（7.33）下面的式子
% 【0 <= t <= delta】 区间
syms delta
eq1 = diff(lambda,t)==mu1;
con1 = lambda(delta)==10;
con2 = mu1(delta)==-10;
con3 = mu2(delta)==-10;
sol2 = dsolve(eq1,eq2,eq3,con1,con2,con3);
```

求解结果为

![fig3]({{site.page}}/images/img-2021-10-20/fig3.png)

书中求解结果为

$$
\left\{\begin{matrix}
\begin{aligned}
\lambda &= \frac{10}{3}e^{3(t-\Delta)} + \frac{20}{3}e^{3(\Delta-t)},	\\
\mu_1 &= 10e^{3(t-\Delta)} - 20e^{3(\Delta-t)}, \\
\mu_2 &= \frac{10}{9}e^{3(t-\Delta)} - \frac{20}{9}e^{3(\Delta-t)} - \frac{80}{9},
\end{aligned}
\end{matrix}\right.
\tag{14}
$$

MATLAB求解结果与书中求解结果相同。

通过求解该微分对策问题，发现甲方最优火力分配策略确实发生了改变。在$[0,\Delta)$时间内应集中力量攻击乙方的第一类作战单位，而在$(\Delta,T]$时间内应集中力量攻击乙方第二类作战单位。

下面来求解相应战斗过程的微分方程组$(1)$的解，并证明甲、乙双方作战单位兵力始终保持如下关系

$$
x_1(t) \ge 0, \quad y_1(t) \ge 0, \quad y_2(t) \ge 0
$$

下面在MATLAB中编写代码求解微分方程组$(1)$，为方便求解，将式$(1)$再次写在下面

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

在$t \in [0,\Delta)$时间内，MATLAB仿真代码为

```matlab
%% 《数理战术学》第7.3节 求解式（7.19）
% 【0 <= t <= delta】 区间
syms x1(t) y1(t) y2(t)
eq1 = diff(x1)==-9*y1-y2;   con1 = x1(0)==100;
eq2 = diff(y1)==-x1;        con2 = y1(0)==30;
eq3 = diff(y2)==0;          con3 = y2(0)==30;
sol3 = dsolve(eq1,eq2,eq3,con1,con2,con3);
```

求解结果为

![fig4]({{site.page}}/images/img-2021-10-20/fig4.png)

书中求解结果为

$$
\left\{\begin{matrix}
\begin{aligned}
x_1 &= 100e^{-3t},	\\
y_1 &= \frac{100}{3}e^{-3t}-\frac{10}{3},	\\
y_2 &= 30,
\end{aligned}
\end{matrix}\right.
\tag{15}
\quad 0 \le t < \Delta
$$

MATLAB求解结果与书中求解结果相同。

在$t \in [\Delta,T]$时间内，MATLAB仿真代码为

```matlab
%% 《数理战术学》第7.3节 求解式（7.19）
% 【delta <= t <= T】 区间
delta = 0.3838;
eq1 = diff(x1)==-9*y1-y2;   con1 = x1(0)==subs(sol3.x1,t,delta);
eq2 = diff(y1)==0;          con2 = y1(0)==subs(sol3.y1,t,delta);
eq3 = diff(y2)==-x1;        con3 = y2(0)==subs(sol3.y2,t,delta);
sol4 = dsolve(eq1,eq2,eq3,con1,con2,con3);
syms delta
sol4.x1 = subs(sol4.x1,t,t-delta);
sol4.y1 = subs(sol4.y1,t,t-delta);
sol4.y2 = subs(sol4.y2,t,t-delta);
```

求解结果为

![fig5]({{site.page}}/images/img-2021-10-20/fig5.png)

书中求解结果为

$$
\left\{\begin{matrix}
\begin{aligned}
x_1 &= 20 \sqrt{10} e^{\Delta-t}-10 \sqrt{10} e^{t-\Delta},	\\
y_1 &= \frac{10}{3} \sqrt{10} - \frac{10}{3},	\\
y_2 &= 20\sqrt{10} e^{\Delta-t} + 10\sqrt{10} e^{t-\Delta} - 30\sqrt{10} + 30.
\end{aligned}
\end{matrix}\right.
\quad \Delta \le t \le T
\tag{16}
$$

MATLAB求解结果与式$(16)$相比，得到的式子更加复杂。经过核算，

```matlab
200*exp(-5757/5000) = 63.2388
100*exp(-5757/5000) = 31.6194
300*exp(-5757/5000) = 94.8683
100*exp(-5757/5000)/3-10/3 = 7.2065
```

$$
\left\{\begin{matrix}
\begin{aligned}
20\sqrt{10}&=63.2456,	\\
10\sqrt{10}&=31.6228,	\\
30\sqrt{10}&=94.8683,	\\
\frac{10}{3} \sqrt{10} - \frac{10}{3} &= 7.2076.
\end{aligned}
\end{matrix}\right.
$$

两者相差不多，可以认为MATLAB的求解结果与书中结果相同。

至此，使用MATLAB求解微分方程的问题已解决。

将所求得的表达式画图，在MATLAB中编写代码实现，代码为

```matlab
%% 画图
% 战斗时间
T = (1/6)*log(10) + log(10/9);
delta = (1/6)*log(10);

% 表达式
k = 1;
len = length(0:0.01:T);
x1 = zeros(1,len);
y1 = zeros(1,len);
y2 = zeros(1,len);
for t = 0:0.01:T
    if t <= delta
        x1(k) = 100*exp(-3*t);
        y1(k) = (100/3)*exp(-3*t) - (10/3);
        y2(k) = 30;
        k = k+1;
    else
        x1(k) = 20*sqrt(10)*exp(delta-t) - 10*sqrt(10)*exp(t-delta);
        y1(k) = (10/3)*sqrt(10) - (10/3);
        y2(k) = 20*sqrt(10)*exp(delta-t) + 10*sqrt(10)*exp(t-delta) - 30*sqrt(10) + 30;
        k = k+1;
    end
end
figure('Color',[1,1,1]);box on;
t = 0:0.01:T;
plot(t, x1, 'LineWidth', 1.5);hold on;
plot(t, y1, 'LineWidth', 1.5);
plot(t, y2, 'LineWidth', 1.5);
set(gca,'FontName','Times New Roman',...
        'FontSize',13,...
        'LineWidth',1.3,...
        'XTick',(0:0.05:t(end)),...
        'YTick',(0:10:110))
legend('x1','y1','y2');
xlabel('时间单位','FontName','宋体');ylabel('兵力','FontName','宋体');
axis([0 t(end) 0 110]);
```

结果为

![fig7]({{site.page}}/images/img-2021-10-20/fig7.png)

全部源码如下（==作者MATLAB版本为2018b==）：

```matlab
clc;clear;
format long;                            % 将数据精度设置为长精度
%% 《数理战术学》第7.3节 式（7.23）、式（7.29）
% 【delta <= t <= T】 区间
syms lambda(t) mu1(t) mu2(t) T
eq1 = diff(lambda,t)==mu2;con1 = lambda(T)==9;
eq2 = diff(mu1,t)==9*lambda;con2 = mu1(T)==-1;
eq3 = diff(mu2,t)==lambda;con3 = mu2(T)==-9;
sol1 = dsolve(eq1,eq2,eq3,con1,con2,con3);
% sol1=dsolve(diff(lambda)==mu2, diff(mu1)==9*lambda, diff(mu2)==lambda,...
%            lambda(T)==9, mu1(T)==-1, mu2(T)==-9);

%% 《数理战术学》第7.3节 式（7.30）
syms delta
T = (1/6)*log(10) + log((10/9));
res.lambda = subs(sol1.lambda, t, delta);
res.mu1 = subs(sol1.mu1, t, delta);
res.mu2 = subs(sol1.mu2, t, delta);
res.delta = solve(res.mu1==res.mu2, delta);
res.delta = eval(res.delta);

%% 《数理战术学》第7.3节 式（7.33）下面的式子
% 【0 <= t <= delta】 区间
syms delta
eq1 = diff(lambda,t)==mu1;
con1 = lambda(delta)==10;
con2 = mu1(delta)==-10;
con3 = mu2(delta)==-10;
sol2 = dsolve(eq1,eq2,eq3,con1,con2,con3);

%% 《数理战术学》第7.3节 求解式（7.19）
% 【0 <= t <= delta】 区间
syms x1(t) y1(t) y2(t)
eq1 = diff(x1)==-9*y1-y2;   con1 = x1(0)==100;
eq2 = diff(y1)==-x1;        con2 = y1(0)==30;
eq3 = diff(y2)==0;          con3 = y2(0)==30;
sol3 = dsolve(eq1,eq2,eq3,con1,con2,con3);

% 【delta <= t <= T】 区间
delta = 0.3838;
eq1 = diff(x1)==-9*y1-y2;   con1 = x1(0)==subs(sol3.x1,t,delta);
eq2 = diff(y1)==0;          con2 = y1(0)==subs(sol3.y1,t,delta);
eq3 = diff(y2)==-x1;        con3 = y2(0)==subs(sol3.y2,t,delta);
sol4 = dsolve(eq1,eq2,eq3,con1,con2,con3);
syms delta
sol4.x1 = subs(sol4.x1,t,t-delta);
sol4.y1 = subs(sol4.y1,t,t-delta);
sol4.y2 = subs(sol4.y2,t,t-delta);

%% 画图
% 战斗时间
T = (1/6)*log(10) + log(10/9);
delta = (1/6)*log(10);

% 表达式
k = 1;
len = length(0:0.01:T);
x1 = zeros(1,len);
y1 = zeros(1,len);
y2 = zeros(1,len);
for t = 0:0.01:T
    if t <= delta
        x1(k) = 100*exp(-3*t);
        y1(k) = (100/3)*exp(-3*t) - (10/3);
        y2(k) = 30;
        k = k+1;
    else
        x1(k) = 20*sqrt(10)*exp(delta-t) - 10*sqrt(10)*exp(t-delta);
        y1(k) = (10/3)*sqrt(10) - (10/3);
        y2(k) = 20*sqrt(10)*exp(delta-t) + 10*sqrt(10)*exp(t-delta) - 30*sqrt(10) + 30;
        k = k+1;
    end
end
figure('Color',[1,1,1]);box on;
t = 0:0.01:T;
plot(t, x1, 'LineWidth', 1.5);hold on;
plot(t, y1, 'LineWidth', 1.5);
plot(t, y2, 'LineWidth', 1.5);
set(gca,'FontName','Times New Roman',...
        'FontSize',13,...
        'LineWidth',1.3,...
        'XTick',(0:0.05:t(end)),...
        'YTick',(0:10:110))
legend('x1','y1','y2');
xlabel('时间单位','FontName','宋体');ylabel('兵力','FontName','宋体');
axis([0 t(end) 0 110]);

%% 正确代码 (echoyoung)
% syms a(t) b(t) c(t)
% res=dsolve(diff(a)==c, diff(b)==9*a, diff(c)==a, a(T)==9, b(T)==-1, c(T)==-9);
```