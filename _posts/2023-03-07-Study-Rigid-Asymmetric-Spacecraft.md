---
permalink: /:year-:month-:day/:title
layout: post
title:  "刚体非对称航天器最优控制问题的解法"
author: "Lei Lie"
categories: Study
tag: notdouban
---

# 题目

例子出自论文 *Direct solution of nonlinear optimal control problems using quasilinearization and Chebyshev polynomials*（DOI：10.1016/S0016-0032(02)00028-5） Section 5.2. Example 2: Rigid asymmetric spacecraft

题目如下：

一个刚体非对称航天器控制问题，其状态方程为

$$
\left \{ \begin{matrix}
    \dot{\omega}_1 = -\frac{I_3-I_2}{I_1} \omega_2 \omega_3 + \frac{u_1}{I_1},  \\
    \dot{\omega}_2 = -\frac{I_1-I_3}{I_2} \omega_1 \omega_3 + \frac{u_2}{I_2},  \\
    \dot{\omega}_3 = -\frac{I_2-I_1}{I_3} \omega_1 \omega_2 + \frac{u_3}{I_3},
\end{matrix}
\right.
$$

式中，$\omega_1$、$\omega_2$ 和 $\omega_3$ 为航天器的角速度。

控制量为 $u_1$、$u_2$、$u_3$，目标函数为

$$
J = 0.5 \int_{0}^{100}(u_1^2+u_2^2+u_3^2) \text{d}t.
$$

其余参数为

$$
\begin{aligned}
    &\omega_1(0)=0.01 \ \text{r/s},\ \omega_2(0)=0.005 \ \text{r/s},\ \omega_3(0)=0.001 \ \text{r/s},    \\
    &\omega_1(t_f)=\omega_2(t_f)=\omega_3(t_f)=0 \ \text{r/s},  \\
    &I_1=86.24 \ \text{kg m}^2,\ I_2=85.07 \ \text{kg m}^2,\ I_3=113.59 \ \text{kg m}^2.
\end{aligned}
$$

Example 2 给出 2 个例子。

**Case 1：状态量和控制量没有不等式约束。**

**Case 2：$\omega_1(t)$ 存在不等式约束。**

满足

$$
\omega_1(t) - (5\times10^{-6}\ t^2 - 5\times10^{-4}\ t + 0.015 ) \le 0.
$$

# 代码

代码如下，[完整代码](../../assets/download/files-2023-03-07/section5_example2.zip)在这里。

## 主函数 section5_example2()
```matlab
%--------------------------------------------------------------------------
% 说明：Direct solution of nonlinear optimal control problems using 
%       quasilinearization and Chebyshev polynomials
%       pp.491-497 example 2
% 作者：Lingwei Li
% 时间：ver 1.0 2023/03/6
%--------------------------------------------------------------------------
function section5_example2
%% 清除变量
clc;clear;close all;

%% 设置初始条件
% 状态
w10 = .01;
w20 = .005;
w30 = .001;

w1_tf = 0;
w2_tf = 0;
w3_tf = 0;

% 参数
I1 = 86.24;
I2 = 85.07;
I3 = 113.59;

%% 伪谱法配点
nP = 75;                                % 配置点个数
nS = 3;                                 % 状态量个数
nU = 3;                                 % 控制量个数

% 伪谱法配点
tau = LGL_nodes(nP-1);                  % 把时间区间离散到[-1,1]中
D   = LGL_Dmatrix(tau);                 % 微分矩阵
weights = LGL_weights(tau);             % 配置点权重

%% 处理算法的初值猜测
t0 = 0;                                 % 起始时间
T  = 100;                               % 终止时间
t  = tau2t(T,t0,tau);                   % 配点后的时间区段

% 提前猜测几个控制量
% 红方控制量
u10 = -.0087*ones(nP,1);
u20 = -.0043*ones(nP,1);
u30 = -.015*ones(nP,1);

% 建立变量索引
% 状态量
w1_index = (1:nP)';
w2_index = (nP+1:2*nP)';
w3_index = (2*nP+1:3*nP)';
% 控制量
u1_index = (3*nP+1:4*nP)';
u2_index = (4*nP+1:5*nP)';
u3_index = (5*nP+1:6*nP)';

%% 为 R:max 的算法设置初始条件
x0 = zeros((nS+nU)*nP,1);
x0([w1_index,w2_index,w3_index]) = [w10*ones(nP,1);w20*ones(nP,1);w30*ones(nP,1)];
x0([w1_index(end),w2_index(end),w3_index(end)]) = [w1_tf;w2_tf;w3_tf];
x0([u1_index,u2_index,u3_index]) = [u10;u20;u30]; 

%% 算法开始
% 循环参数
options = optimset('Display','iter',...             % off/iter/notify-detailed
                   'Algorithm','sqp',...            % 算法选择
                   'TolCon',1e-6,...                % 约束违反度度容差（正标量），默认值为 1e-6
                   'TolX',1e-6,...                  % 关于正标量 x 的终止容差，SQP算法的默认值为 1e-6
                   'TolFun',1e-6);               	% 一阶最优性的终止容差（正标量），默认值为 1e-6                  

tic;
%--- 算法开始 ---%
res = fmincon(@objective,x0,[],[],[],[],[],[],@constraints,options);
%--- 算法结束 ---%
toc;

w1 = res(w1_index);
w2 = res(w2_index);
w3 = res(w3_index);
u1 = res(u1_index);
u2 = res(u2_index);
u3 = res(u3_index);
%% 画图
% 开始画图
window_width = 500;
window_height = 416;

% 状态量
k = 0;
figure('color',[1 1 1],'position',[300+k*window_width,300,window_width,window_height]);
plot(t, w1, '-',  'LineWidth',1.5);hold on;
plot(t, w2, '--', 'LineWidth',1.5);
plot(t, w3, ':',  'LineWidth',1.5);
legend('$\omega_{1}(t)$','$\omega_{2}(t)$','$\omega_{3}(t$)',...
       'Location','Best',...
       'Interpreter','Latex',...
       'FontSize',13);
xlabel('Time');
ylabel('State');
set(gca,'FontSize',13,'FontName','Times New Roman','LineWidth',1.5);

% 控制量
k = 1;
figure('color',[1 1 1],'position',[300+k*window_width,300,window_width,window_height]);
plot(t, u1, '-',  'LineWidth',1.5);hold on;
plot(t, u2, '--', 'LineWidth',1.5);
plot(t, u3, ':',  'LineWidth',1.5);
legend('$u_1(t)$','$u_2(t)$','$u_3(t)$',...
       'Location','Best',...
       'Interpreter','Latex',...
       'FontSize',13);
xlabel('Time');
ylabel('Control');
set(gca,'FontSize',13,'FontName','Times New Roman','LineWidth',1.5);

%% 子函数部分
function dJ = objective(x)
    % 控制量赋值
    u1 = x(u1_index);
    u2 = x(u2_index);
    u3 = x(u3_index);
    
    L = u1.^2 + u2.^2 + u3.^2;
    
    % 目标函数
    dJ = (T-t0)/2 * dot(weights,L)/2;
end

function [c,ceq] = constraints(x)
    % ---------- 赋值 ---------- %
    % 状态量赋值
    w1_con = x(w1_index);
    w2_con = x(w2_index);
    w3_con = x(w3_index);
    
    % 控制量赋值
    u1_con = x(u1_index);
    u2_con = x(u2_index);
    u3_con = x(u3_index);
    % ---------- 结束 ---------- %
    
    % -------- 等式约束 -------- %
    ceq1 = w1_con(1) - w10;
    ceq2 = w2_con(1) - w20;
    ceq3 = w3_con(1) - w30;
    ceq4 = w1_con(end) - w1_tf;
    ceq5 = w2_con(end) - w2_tf;
    ceq6 = w3_con(end) - w3_tf;
    
    % 状态方程
    dw1_con = -(I3-I2)/I1 .* w2_con .* w3_con + u1_con ./ I1;
    dw2_con = -(I1-I3)/I2 .* w1_con .* w3_con + u2_con ./ I2;
    dw3_con = -(I2-I1)/I3 .* w1_con .* w2_con + u3_con ./ I3;
    
    Y = [w1_con,w2_con,w3_con];
    F = (T-t0)/2*real([dw1_con,dw2_con,dw3_con]);
    ceq7 = D*Y - F;
    ceq = [ceq1;ceq2;ceq3;ceq4;ceq5;ceq6;ceq7(:)];
    % ---------- 结束 ---------- %
    
    % ------- 不等式约束 ------- %
%     c = []; % case 1: no inequality constraints
    c = w1_con - (5.*1e-6.*t.^2 - 5.*1e-4.*t + .016); % case 2: inequality constraints on w1(t)
    % ---------- 结束 ---------- %
end

function t = tau2t(Tf,t0,tau)
% 把 tau 转换为 t
% 按照《天基对地打击武器轨道规划与制导技术》2013 p79 式(4.25)编写如下代码
t = (Tf-t0)*tau/2+(Tf+t0)/2;
end
end
```

下面给出在主函数中会用到的函数，一共 4 个。

## 1. 子函数 LGL_nodes()

```matlab
%--------------------------------------------------------------------------
% LGL_nodes.m
% Author: Daniel R. Herber, Graduate Student, University of Illinois at
% Urbana-Champaign
% Date: 06/04/2015
%--------------------------------------------------------------------------
function tau = LGL_nodes(N)
    thetak = (4*[1:N]-1)*pi/(4*N+2);
    sigmak = -(1-(N-1)/(8*N^3)-(39-28./sin(thetak).^2)/(384*N^4)).*cos(thetak);
    ze = (sigmak(1:N-1)+sigmak(2:N))/2;  
    ep = eps*10;
    ze1 = ze+ep+1; 
    while max(abs(ze1-ze))>=ep,
      ze1 = ze;
      [dy,y] = lepoly(N,ze);
      ze = ze-(1-ze.*ze).*dy./(2*ze.*dy-N*(N+1)*y);  % see Page 99 of the book
    end;
    tau=[-1,ze,1]'; % column vector

end
```

## 2. 子函数 LGL_Dmatrix()

```matlab
%--------------------------------------------------------------------------
% LGL_Dmatrix.m
% Author: Daniel R. Herber, Graduate Student, University of Illinois at
% Urbana-Champaign
% Date: 06/04/2015
% Last modified by Lingwei Li at 06/22/2022
%--------------------------------------------------------------------------
function D = LGL_Dmatrix(tau)
    N = length(tau)-1;
    n = N + 1;
    if n==0, D = [];
        return; 
    end    
    xx = tau; y = lepoly(n-1,xx);
    D = (xx./y)*y'-(1./y)*(xx.*y)'; 
    D = D + eye(n); operated                                     
    D = 1./D; 
    D = D - eye(n); 
    D(1,1) = -n*(n-1)/4; D(n,n) = -D(1,1);
end
```
## 3. 子函数 LGL_weights()

```matlab
%--------------------------------------------------------------------------
% LGL_weights.m
% determines Gaussian quadrature weights using Lagrange-Gauss-Lobatto (LGL)
% nodes
%--------------------------------------------------------------------------
% w = LGL_weights(tau)
% tau: LGL nodes
%   w: Gaussian quadrature weights
%--------------------------------------------------------------------------
% Author: Daniel R. Herber, Graduate Student, University of Illinois at
% Urbana-Champaign
% Date: 06/04/2015
%--------------------------------------------------------------------------
function w = LGL_weights(tau)
    % number of nodes
    N = length(tau)-1;
    
    % See Page 99 of the book: J. Shen, T. Tang and L. Wang, Spectral Methods:
    % Algorithms, Analysis and Applications, Springer Series in Compuational
    % Mathematics, 41, Springer, 2011. 
    % Uses the function: lepoly() 
    % Original function: [varargout] = legslb(n) located at
    % http://www1.spms.ntu.edu.sg/~lilian/bookcodes/legen/legslb.m
    [~,y] = lepoly(N,tau(2:end-1));
    % Use the weight expression (3.188) to compute the weights
    w = [2/(N*(N+1));2./(N*(N+1)*y.^2);2/(N*(N+1))]; 

end
```

## 4. 子函数 lepoly()

```matlab
%--------------------------------------------------------------------------
% lepoly.m
% Date: 08/30/2011
%--------------------------------------------------------------------------
function [varargout]=lepoly(n,x)
if nargout==1,
     if n==0, varargout{1}=ones(size(x));  return; end;
     if n==1, varargout{1}=x; return; end;
     polylst=ones(size(x)); poly=x;
     for k=2:n,
	   polyn=((2*k-1)*x.*poly-(k-1)*polylst)/k;
       polylst=poly; poly=polyn;	
     end;
     varargout{1}=polyn;
end;
if nargout==2,
     if n==0, varargout{2}=ones(size(x)); varargout{1}=zeros(size(x)); return;end;
     if n==1, varargout{2}=x; varargout{1}=ones(size(x)); return; end;

     polylst=ones(size(x)); pderlst=zeros(size(x));poly=x; pder=ones(size(x));
    for k=2:n,
      polyn=((2*k-1)*x.*poly-(k-1)*polylst)/k;
      pdern=pderlst+(2*k-1)*poly;
 	  polylst=poly; poly=polyn;
	  pderlst=pder; pder=pdern;
    end;
      varargout{2}=polyn; varargout{1}=pdern;
end;
return 
```

# 仿真结果

注意，case 1 和 case 2 的选择通过 `section5_example2()` 的 `constraints()` 中

```matlab
    % ------- 不等式约束 ------- %
%     c = []; % case 1: no inequality constraints
    c = w1_con - (5.*1e-6.*t.^2 - 5.*1e-4.*t + .016); % case 2: inequality constraints on w1(t)
    % ---------- 结束 ---------- %
end
```

来选择。

## Case 1：状态量和控制量没有不等式约束

代码仿真结果：

状态量：

![状态量变化曲线（case 1）](../images/img-2023-03-07/state%20(case%201).jpg)

控制量：

![控制量变化曲线（case 1）](../images/img-2023-03-07/control%20(case%201).jpg)

论文仿真结果：

状态量：

![状态量变化曲线（case 1）](../images/img-2023-03-07/state%20(case%201)%20in%20paper.jpg)

控制量：

![控制量变化曲线（case 1）](../images/img-2023-03-07/control%20(case%201)%20in%20paper.jpg)

## Case 2：$\omega_1(t)$ 存在不等式约束

代码仿真结果：

状态量：

![状态量变化曲线（case 1）](../images/img-2023-03-07/state%20(case%202).jpg)

控制量：

![控制量变化曲线（case 1）](../images/img-2023-03-07/control%20(case%202).jpg)

论文仿真结果：

状态量：

![状态量变化曲线（case 2）](../images/img-2023-03-07/state%20(case%202)%20in%20paper.jpg)

控制量：

![控制量变化曲线（case 2）](../images/img-2023-03-07/control%20(case%202)%20in%20paper.jpg)

## 分析

case 1 的代码仿真结果与论文仿真结果相似。

case 2 的代码仿真结果中，控制量的波动比较平稳；而论文仿真结果中，控制量的波动略微剧烈一些。因为论文将整个控制区间分成了 $[0,39]$ 和 $[39,100]$ 两个子区间，所以会有震荡。论文里没有把控制区间分段的时候，控制量的波动更加剧烈，如下图所示。

![控制量变化曲线（case 2）未分段](../images/img-2023-03-07/control%20(case%202)%20in%20paper%20(compare).jpg)

时间区间加多后，控制量的变化趋于平稳。代码仿真结果中，控制量结果一直很平稳。其实这就说明结果很好。作者论文中，没有分段的控制量变化十分剧烈，说明他的结果不平稳，把时间区间分段之后才逐渐平稳的。只要时间区间分段足够多，控制量结果一定会趋近平稳的。

# 总结

约束条件其实不难写，认真研究一下之前伪谱法的代码，把 `objective()` 和 `contstrants()` 里面的代码根据具体题目要求改一下，一定可以做出来的。

欢迎通过邮箱联系我：lordofdapanji@foxmail.com

来信请注明你的身份，否则恕不回信。
