---
permalink: /:year-:month-:day/:title
layout: post
title:  "用GPOPS2解最优控制问题"
author: "Lei Lie"
categories: Study,Code
tag: notdouban
---

要解决的问题是这个：

[用matlab的bvpc求解器解决最优控制问题（即两点边值问题）
](https://blog.csdn.net/Ruins_LEE/article/details/118711310)

之前是用MATLAB的bvpc求解器进行求解。但这种最优控制问题其实是可以使用高斯伪谱法将其转化为非线性规划问题，再用优化方法求解该非线性规划问题。

GPOPS2里面用的最优化算法是序列二次规划算法（sqp）。

再把【例题】写在这里一遍，方便观看。

# 【例题】求解最优控制问题-有约束的停车能耗最优控制

初始时刻车辆位置为$x_1(0)=-2$，速度为$x_2(0)=1$，状态方程为

$$
\left\{\begin{matrix}
\begin{aligned}
\dot x_1(t) &= x_2(t) \\
\dot x_2(t) &= u(t)
\end{aligned}
\end{matrix}\right.
$$

容许控制为

$$
\left | u \right | \le M_1 =1.5
$$

终止条件为

$$
x_1(t_f)=0,\ x_2(t_f)=0,\ t_f=2
$$

要最小化的性能指标为总能耗，其表达式为

$$
J(u)=\int_{t_0}^{t_f}\frac{1}{2}u^2(t)\mathrm dt
$$

# 【代码】
```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% 功能描述：最优控制问题
% 文件名解释：mainVehcle.m 中，main 代表 主函数，
%             Vehcle 代表 停车能耗问题
% 作者：最爱大盘鸡
% 时间：2021/11/3
% 版本：1.0（2021/11/3）
% - 完成了代码框架的初始搭建
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
clc;clear;close all;
tic;
%% 01.初始参数设置
%-------------------------------------------------------------------------%
%----------------------- 设置问题的求解边界 ------------------------------%
%-------------------------------------------------------------------------%
% 设置时间
t0 = 0;
tf = 2;
% 设置状态量初值
x10 = -2;
x20 = 1;
% 设置状态量边界条件
x1Min = -5;
x1Max = 5;
x2Min = -5;
x2Max = 5;
% 设置控制量边界条件
uMin  = -1.5;
uMax  = 1.5;

%% 02.边界条件设置
%-------------------------------------------------------------------------%
%------------------------ 将求解边界设置于问题中 -------------------------%
%-------------------------------------------------------------------------%
bounds.phase.initialtime.lower  = t0; 
bounds.phase.initialtime.upper  = t0;
bounds.phase.finaltime.lower    = tf; 
bounds.phase.finaltime.upper    = tf;
bounds.phase.initialstate.lower = [x10 x20]; 
bounds.phase.initialstate.upper = [x10 x20];
bounds.phase.state.lower        = [x1Min x2Min]; 
bounds.phase.state.upper        = [x1Max x2Max];
bounds.phase.finalstate.lower   = [0 0];
bounds.phase.finalstate.upper   = [0 0];
bounds.phase.control.lower      = uMin; 
bounds.phase.control.upper      = uMax;
bounds.phase.integral.lower     = 0; 
bounds.phase.integral.upper     = 10000;

%% 03.初值猜测
%-------------------------------------------------------------------------%
%------------------------------- 初值猜想 --------------------------------%
%-------------------------------------------------------------------------%
guess.phase.time     = [t0; tf]; 
guess.phase.state    = [[x10; 0],[x20; 0]];
guess.phase.control  = [1; uMin];
guess.phase.integral = 100;

%% 04.设置GPOPS求解器参数
%-------------------------------------------------------------------------%
%---------------------------- 设置求解器参数 -----------------------------%        
%-------------------------------------------------------------------------%
setup.name = 'Vehicle-Stopping-OCP';
setup.functions.continuous  = @vsopcContinuous;
setup.functions.endpoint   	= @vsopcEndpoint;
setup.bounds                = bounds;
setup.guess                 = guess;
setup.nlp.solver            = 'snopt';
setup.derivatives.supplier  = 'sparseCD';
setup.derivatives.derivativelevel = 'second';
setup.mesh.method           = 'hp1';
setup.mesh.tolerance        = 1e-6;
setup.mesh.maxiteration     = 45;
setup.mesh.colpointsmax     = 4;
setup.mesh.colpointsmin     = 10;
setup.mesh.phase.fraction   = 0.1*ones(1,10);
setup.mesh.phase.colpoints  = 4*ones(1,10);
setup.method = 'RPMintegration';

%% 05.求解
%-------------------------------------------------------------------------%
%----------------------- 使用 GPOPS2 求解最优控制问题 --------------------%
%-------------------------------------------------------------------------%
output = gpops2(setup);
solution = output.result.solution;
toc;

%% 06.画图
figure('Color',[1,1,1]);
plot(solution.phase.time(:,1),solution.phase.state(:,1),'-','LineWidth',1.5);hold on;
plot(solution.phase.time(:,1),solution.phase.state(:,2),'-.','LineWidth',1.5);
plot(solution.phase.time(:,1),solution.phase.control(:,1),'--','LineWidth',1.5);
axis([0 2 -2.5 2]);
xlabel('Time',...
       'FontWeight','bold');
ylabel('States',...
       'FontWeight','bold');
legend('Pos','Vel','Acc',...
       'LineWidth',1,...
       'EdgeColor',[1,1,1],...
       'Orientation','horizontal',...
       'Position',[0.5,0.93,0.40,0.055]);
set(gca,'FontName','Times New Roman',...
        'FontSize',15,...
        'LineWidth',1.3);
print -dpng Result.png

%% 函数模块部分
% ----------------------------------------------------------------------- %
% ------------------------- BEGIN: vsopcContinuous.m -------------------- %
% ----------------------------------------------------------------------- %
function phaseout = vsopcContinuous(input)
t  = input.phase.time;
% x1  = input.phase.state(:,1);
x2  = input.phase.state(:,2);
u   = input.phase.control(:,1);

dx1 = x2;
dx2 = u;

phaseout.dynamics = [dx1, dx2];
phaseout.integrand = 0.5*u.^2;
end
% ----------------------------------------------------------------------- %
% -------------------------- END: vsopcContinuous.m --------------------- %
% ----------------------------------------------------------------------- %

% ----------------------------------------------------------------------- %
% -------------------------- BEGIN: vsopcEndpoint.m --------------------- %
% ----------------------------------------------------------------------- %
function output = vsopcEndpoint(input)

J  = input.phase.integral;
output.objective = J;
end
% ----------------------------------------------------------------------- %
% --------------------------- END: vsopcEndpoint.m ---------------------- %
% ----------------------------------------------------------------------- %
```

仿真结果如下

![img1]({{site.page}}/images/img-2021-11-03/img1.png)

和[用matlab的bvpc求解器解决最优控制问题（即两点边值问题）
](https://blog.csdn.net/Ruins_LEE/article/details/118711310)结果一样，所以说高斯伪谱法是可以解最优控制问题的。