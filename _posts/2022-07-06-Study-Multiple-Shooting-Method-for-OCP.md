---
permalink: /:year-:month-:day/:title
layout: post
title:  "数值法求解最优控制问题（三）——多重打靶法"
author: "Lei Lie"
categories: Study,Code
tag: notdouban
---

# 写在前面

在[数值法求解最优控制问题（二）——打靶法](https://leilie.top/2022-07-02/Study-Shooting-Method-for-OCP)中介绍了单次打靶法的原理，本篇介绍多重打靶法（Multiple Shooting Method，MSM）。

多重打靶法与单次打靶法都是将控制变量在时间区间内离散，再在每个子时间区间中使用多项式函数进行近似。但两者区别之处在于，多重打靶法把每个子时间区间内状态变量初始值作为待优化参数，分段求解状态微分方程，并设置每段时间区间的连续性条件约束。

# 多重打靶法

下面介绍多重打靶法的原理，并根据[数值法求解最优控制问题（一）——梯度法](https://leilie.top/2022-06-25/Study-Gradient-Method-for-OCP)的算例，给出matlab代码。

多重打靶法的核心思想是：将连续的控制向量在离散时间网格上参数化；同时将状态向量在每个时间子区间上的初始值也作为新的自由度，即同样作为待优化的参数，独立求解每个子区间的状态微分方程组，然后引入匹配条件作为等式约束以保证状态轨迹的连续性。这样，原问题被转化为一个非线性规划（NonLinear Programming，NLP）问题进行求解。

多重打靶法的原理是：在算法初始阶段，对于给定的待优化参数值，分别计算出目标函数值和状态变量曲线。随着算法迭代，状态曲线在连续性条件约束下趋于连续，目标函数趋于最优值。

首先，在时间区间 $[t_0,t_f]$ 内划分 $N+1$ 个子时间区间 $I_k$，为：

$$
\begin{matrix}
I_k = [t_{k-1},t_k], \quad (k=1,2,\dots,N+1), \\
t_0 \le t_1 \le \dots \le t_k \le \dots \le t_{N-1} \le t_{N+1} = t_f,    
\end{matrix}
$$

式中，$t_k$ 为时间节点，是一个固定值。

下面开始离散控制量。

假设控制量 $\boldsymbol{u}(t) \in \mathbb{R}^{n_u}$，第 $i$ 个分量为 $u_i(t)(i=1,2,\dots,n_u)$，则其数学描述为：

$$
u_i(t) = \sum_{k=1}^{N} u_i^k(t) \chi_k(t),
$$

式中，$u_i^k(t)$ 为控制分量 $u_i(t)$ 在时间区间 $I_k$ 内的值，$\chi_k(t)$ 为单位开关函数，为

$$
\chi_k(t) = 
\left \{
\begin{matrix}
    1, \quad t \in [t_{k-1},t_k],   \\
    0, \quad t \notin [t_{k-1},t_k].
\end{matrix}
\right.
$$

$u_i^k(t)$ 可近似表示为：

$$
u_i^k(t) \approx \sum_{r=1}^{M+1} \sigma_{ir}^k [\phi_{ir}^k(t)]^M,
$$

式中，$\phi_r(t)$ 是基函数， $M$ 是基函数的阶次， $\sigma_r$ 是线性组合系数，即控制参数。根据 $\phi_r(t)$ 和 $M$ 的选择不同，$\boldsymbol{u}(t)$ 有多种参数化表达形式，例如有分段常数近似、分段线性近似、分段抛物线近似和分段光滑样条函数近似等形式。

控制量被离散后，下面需要将状态变量在相同时间网格上进行打靶。

引入辅助向量 $\boldsymbol{s}=[\boldsymbol{s}_1^{\rm T},\boldsymbol{s}_2^{\rm T},\dots,\boldsymbol{s}_N^{\rm T}]$，分量 $s_k(k=1,2,\dots,N)$ 为状态变量 $\boldsymbol{x} \in \mathbb{R}^{n_x}$ 在每个子时间区间上的初始值，为：

$$
\boldsymbol{s}_k = \boldsymbol{x}_0^k = [x_{10}^k,x_{20}^k,\dots,x_{j0}^k,\dots,x_{n_x 0}^k].
$$

上式中，各状态变量在打靶区间内是彼此不相关的，动态微分方程可以作为初值问题（Initial Value Problem，IVP）独立求解，为：

$$
\begin{matrix}
    \boldsymbol{\dot x}_k(t) = \boldsymbol{f}[\boldsymbol{\sigma}_k,\boldsymbol{x}_k(t),t], \quad t \in [t_{k-1},t_k],  \\
    \boldsymbol{x}_k(t_k) = \boldsymbol{s}_k.
\end{matrix}
$$

为了保证状态变量在整个时间域上的连续性，引入连续性约束条件作为等式约束，为：

$$
\boldsymbol{x}_k(t_{k+1}) - \boldsymbol{x}_{k+1} = 0.
$$

由此，最优控制问题被转化为确定控制参数 $\sigma$ 和 辅助参数 $\boldsymbol{s}$ 的有限维 NLP 问题，为

$$
\begin{aligned}
    &\min_{\sigma,s} \quad J = \Phi_0[\boldsymbol{x}(t_f)] + \sum_{k=1}^{N+1} \int_{t_{k-1}}^{t_k} L_0[t,\boldsymbol{x}_k(t),\boldsymbol{\sigma}_k] \text{d}t,   \\
    &\text{s.t.} \\
    &\boldsymbol{\dot x}_k(t) = \sum_{k=1}^{N+1} \boldsymbol{f}[\boldsymbol{\sigma}_k,\boldsymbol{x}_k(t),t],    \\
    &\boldsymbol{x}_k(t_k) = \boldsymbol{s}_k,   \\
    &\Phi_m[\boldsymbol{x}(t_f)] + \sum_{k=1}^{N+1} \int_{t{k-1}}^{t_k} L_m[t,\boldsymbol{x}_k(t),\boldsymbol{\sigma}_k]\text{d}t = \boldsymbol{0},\quad m = 1,2,\dots,m_1,  \\
    &\Phi_m[\boldsymbol{x}(t_f)] + \sum_{k=1}^{N+1} \int_{t{k-1}}^{t_k} L_m[t,\boldsymbol{x}_k(t),\boldsymbol{\sigma}_k]\text{d}t \le \boldsymbol{0},\quad m = m_1+1,\dots,m_1+m_2,  \\
    &\boldsymbol{x}_k(t_{k+1}) - \boldsymbol{s}_{k+1} = 0,  \\
    &\boldsymbol{u}_L \le \boldsymbol{\sigma} \le \boldsymbol{u}_U, \\
    &t_{k-1} \le t \le t_k, \quad k = 1,2,\dots,N+1.
\end{aligned}
$$

# 算法步骤

1. 确定最优控制问题的离散点数、起止时间、子时间区间段数、状态变量初值、控制变量初值；
2. 计算状态变量数量、控制变量数量和约束条件数量：
- 状态变量数量 = 状态变量数量 * 离散点数，
- 控制变量数量 = 控制变量维度 * 离散点数，
- 约束条件数量 = 状态变量维度 * 离散点数 + 子时间区间段数 + 边界条件数量 + 路径约束数量 * 离散点数，
- 若终端时间不固定，则需将设计变量个数加 1。
3. 将目标函数转化为离散形式；
4. 调用 matlab 的 `fmincon()` 求解该 NLP 问题。 

# 算例

该算例选自《最优化与最优控制》第2版第257页**例13.1**。

---

设由状态方程及初始条件 $\dot x = -x^2+u$ ，$x_0=10$ ，性能指标 $J(u)=0.5\int_{0}^{1}(x^2+u^2)\text{d}t$ ，求解最优控制使 $J$ 为极小。

---

# 代码

这里给出多重打靶法的[完整代码](../../assets/download/files-2022-07-06/multiple-shooting-method-for-OCP.zip)。

```matlab
%--------------------------------------------------------------------------
% This code demonstrates an example of solving constrained optimization problem 
% with multiple shooting method.
% Author: Vinh Quang Nguyen - University of Massachusetts, Amherst
%--------------------------------------------------------------------------
% 说明：应用多重打靶法求解无约束最优控制问题
% 例子：《最优化与最优控制》 pp. 257 例13.1 
% 类型：无控制约束的最优控制问题
% 时间：2022/07/05
%--------------------------------------------------------------------------
clear;clc;close all;

%% 01 初始参数设置
p.ns = 1; p.nu = 1;                     % 状态量个数和控制量个数
p.t0 = 0; p.tf = 1;                     % 初始时间和终止时间
p.x0 = 10;                              % 初始条件

% 多重打靶法参数设置
p.N = 20;                               % 打靶点数 => (N-1) 个子时间区段
p.M = 4;                                % 每个子时间区段包含的打靶点
p.t = linspace(p.t0,p.tf,p.N);          % 时间序列

% 设置状态量和控制量的索引
p.x_index = 1:p.N;
p.u_index = p.N+1:2*p.N;
%% 02 求解算法
% 设置初值
y0 = ones((p.ns + p.nu)* p.N, 1);
% 设定求解器设置
options = optimoptions('fmincon','Display','Iter','Algorithm','sqp','MaxFunEvals',1e5); 

tic;
[X,fval,exitflag,output] = fmincon(@(y) objfun(y, p),y0,[],[],[],[],[],[],@(y) noncon(y, p),options);
toc; 

%% 03 处理数据
p.x = reshape(X(p.x_index), [], p.ns);
p.u = reshape(X(p.u_index), [], p.nu);

%% 04 画图
window_width = 500;
window_height = 416;

% 状态量和控制量
k = 1;
figure('color',[1 1 1],'position',[300+k*window_width,300,window_width,window_height]);
plot(p.t, p.x, 'o-', 'LineWidth',1.5);hold on;
plot(p.t, p.u, 'x-', 'LineWidth',1.5);
xlabel('Time');
ylabel('State & control');
set(gca,'FontSize',15,...
        'FontName','Times New Roman',...
        'LineWidth',1.5);
legend('$x(t)$','$u(t)$',...
        'Location','Northeast',...
        'FontSize',10,...
        'interpreter','latex');

% 保存数据
% .\ 下一级文件夹
% ..\ 上一级文件夹
% save(['.\','multi_shooting_method.mat']);
%% 子函数  
% 目标函数
function f = objfun(y,p)
    % 得到状态量和控制量
    x = y(p.x_index);
    u = y(p.u_index);
    L = u.^2/2 + x.^2/2;            % 积分项
    f = trapz(p.t,L);               % 计算目标函数
end

% 状态方程
function dy = state_eq(y,u)
    dy = -y^2 + u;
end

% 约束条件
function [c,ceq] = noncon(y,p)
    % 得到状态量和控制量
    x = reshape(y(p.x_index),[],p.ns);
    u = reshape(y(p.u_index),[],p.nu);
    
    % 时间步长
    h = p.tf/(p.N-1)/(p.M-1);
    
    % 每次子时间区段进行单次打靶法
    states_at_nodes = zeros(p.N, p.ns);
    for i = 1:p.N-1
       x0 = x(i,:);
       u0 = u(i,:);
       states = zeros(p.M,p.ns);
       states(1,:) = x0;
       for j =1:p.M-1
           k1 = state_eq(states(j,:), u0);
           k2 = state_eq(states(j,:) + h/2 * k1, u0);
           k3 = state_eq(states(j,:) + h/2 * k2, u0);
           k4 = state_eq(states(j,:) + h * k3, u0);
           states(j+1,:) = states(j,:) + h/6*(k1 + 2*k2 + 2*k3 + k4);
       end
       states_at_nodes(i+1,:) = states(end,:);
    end
    
    % 保证各区段起始点的连续性
    ceq_temp = x(2:end,:) - states_at_nodes(2:end,:);
    
    % 把初始时刻的状态约束放到 ceq 中
    ceq_temp = [ceq_temp; x(1,:) - p.x0];
    ceq = reshape(ceq_temp, [], 1);
    
    % 不等式约束
    c = [];
end
```

# 结果

状态量和控制量的变化曲线如下。

<!-- ![状态量和控制量](../../images/img-2022-07-06/img1.jpg) -->
<div align=center><img src="../../images/img-2022-07-06/img1.jpg" width = "500" height = "416" /></div>

图中第1个点的控制量不太稳定，离其他时间区间控制量的值相差较大，且与打靶法、梯度法初始时刻的控制量值不符，这里我还没完全了解为什么会出现这样的结果。

不过，随着离散点的增多，初始时刻控制量的值会越来越接近真实值。

# 对比

下面对比多重打靶法和单次打靶法的优劣，分别测试20个离散点、30个离散点、40个离散点和50个离散点时的算法效果。

不同离散点数量计算时间的对比。

<!-- ![状态量和控制量](../../images/img-2022-07-06/img2.jpg) -->
<div align=center><img src="../../images/img-2022-07-06/img2.jpg" width = "500" height = "416" /></div>

不同离散点数量状态量和控制量变化曲线的对比。

<!-- ![状态量和控制量](../../images/img-2022-07-06/img3.jpg) -->
<div align=center><img src="../../images/img-2022-07-06/img3.jpg" width = "750" height = "624" /></div>

多重打靶法的计算时间比单次打靶法的计算时间短，因为多重打靶法将子时间区间初始状态变量作为设计变量，纳入 NLP 问题的求解中；单次打靶法没有将状态变量视作设计变量，因此计算目标函数时需要求解微分方程组，求解微分方程组时又需要插值求解控制量 $u(t)$，插值时需要用到 `interp1()` 函数，`interp1()` 函数是造成单次打靶法计算时间增加的罪魁祸首。多重打靶法在计算目标函数不需要用到 `interp1()` ，因此计算时间短。

在状态量和控制量对比图中，可以发现，随着离散点数从20点增加到50点，初始时刻的控制量确实会越来越接近真实值。与单次打靶法对比，多重打靶法的控制量不会随着离散点数的增加而振荡，这也是多重打靶法优于单次打靶法的地方。

# 思考

## 通过线性外推的方式平滑初始时刻的控制量

因为初始时刻的控制量与其他时刻的控制量差距太大，我认为可能初始时刻的控制量可用性不高，因此考虑是否可以使用线性外推的方式平滑初始时刻的控制量。

为此，根据公式

$$
k = \frac{u_0-u_1}{t_0-t_1} = \frac{u_1-u_2}{t_1-t_2}   \\
\Rightarrow
u_0 = u_1 + \frac{(u_1-u_2)(t_0-t_1)}{t_1-t_2}
$$

编写代码，为

```matlab
%% 03 处理数据
p.x = reshape(X(p.x_index), [], p.ns);
p.u = reshape(X(p.u_index), [], p.nu);
% 尝试平滑第一个时间点的值
u0 = p.u(1+1) + (p.u(1+1)-p.u(2+1))*(p.t(0+1)-p.t(1+1))/(p.t(1+1)-p.t(2+1));
p.u(1) = u0;
```

得到结果。

<!-- ![状态量和控制量](../../images/img-2022-07-06/img4.jpg) -->
<div align=center><img src="../../images/img-2022-07-06/img4.jpg" width = "500" height = "416" /></div>

可以看见，初始时刻的控制量比没有前的控制量要更加平滑一些了。

## 对于NLP求解器初值维度的疑问

多重打靶法的代码我是在[Multiple shooting method example](https://ww2.mathworks.cn/matlabcentral/fileexchange/62727-multiple-shooting-method-example)找到的。作者的代码里，离散点数为30，`N=30`；状态量维度为`2x30`，控制量维度为`1x29`。因为作者认为控制量是每个子时间区段内的控制，一共有29个子时间区段，那么只能有29个控制量。

但是，按照这个思路求解算例问题时遇到了困难。

因为算例的目标函数既包括状态量又包括控制量。状态量和控制量的段数不同，那么后面的运算无法进行；而[Multiple shooting method example](https://ww2.mathworks.cn/matlabcentral/fileexchange/62727-multiple-shooting-method-example)代码解决的最优控制问题离目标函数只有控制量，所以不存在这个问题。

我猜测作者的代码在本算例中不适用，需要做一些修改。

通过阅读文献，我认为控制量和状态量的子时间区间段数是相等的，所以改写了作者代码，应用到本算例中求解最优控制问题。结果表明可以正常运算，但初始时刻控制量的值存在问题，适用性低。目前还没有想到合理的解决方案，如果有想法解决这个疑问，欢迎通过电子邮箱联系。

我的邮箱是：lordofdapanji@foxmail.com。

谢谢。