---
permalink: /:year-:month-:day/:title
layout: post
title:  "用matlab的bvpc求解器解决最优控制问题（即两点边值问题）"
author: "Lei Lie"
categories: Study
tag: notdouban
---

今天看matlab数学文档的时候，发现可以使用`bvp4c`和`bvp5c`这两个求解器解决两点边值问题（Boundary Value Problem, BVP），上网搜了一下，发现CSDN上有人用`bvp4c`解决了一个最优控制问题，下面是对[他的博客](https://blog.csdn.net/lynn15600693998/article/details/86603731)的个人理解。

# 【理论】
`bvp4c`函数用于数值求解两点边值问题，作为Matlab中对ode系列函数的补充。ode系列函数只能数值求解具有初值的常微分方程。

调用格式为

```python
sol = bvp4c(@odefun, @bcfun, solinit, options, p1, p2, ...)
```

（1）`odefun` 为微分方程的函数句柄，调用格式为

```python
function dydx = odefun(x, y, p1, p2, ...)
```

其中，`x` 为标量，`y` 是列向量，`p1` 和 `p2` 以及剩下的参数是在主函数中和 `x` 、`y` 有关的系数。`odefun(x, y)`必须返回列向量 `dydx` ，表示 $f(x,y)$。

（2）`bcfun` 为微分方程的边界条件句柄，调用格式为

```python
function res = bcfun(ya, yb, p1, p2, ...)
```

其中，`ya` 为初值，`yb` 是终值，均为列向量，`p1` 和 `p2` 以及剩下的参数是在主函数中和 `x` 、`y` 有关的系数，`res` 为输出，是列向量。需要注意的是，若在实际问题中，$ya(t_0)$的值为$a$，即$ya(t_0)=a$，则在函数中应写成`ya - a`，`yb`也是一样。

（3）`solinit` 为初始估计解的结构体。使用`bvpinit`函数创建，调用格式为

```python
solinit = bvpinit(x, y)
```

其中，`x` 向量为初始网格的排序节点，应满足边界条件`a=solinit.x(1)`和`b=solinit.x(end)`。`y` 是为初始估计解，使得`solinit.y(:,i)`为在`solinit.x(i)`节点处的初始估计解。以常值作为初始猜测值，此处初值可以任意设置，若任意设置的初值无法解算出合理结果，则可考虑使用continuation方法设置初值，可参考[这篇文章](http://www.joyfulphysics.net/index.php/archives/189/)。

（4）`options` 为可选积分参数。使用`bvpset`函数创建，调用格式为

```python'
options = bvpset('name1','para1','name2','para2',...)
```

一般情况可用`[]`代替。

（5）输出`sol`为结构体，为特定数量点对应的解，其所包含的字段如下表所示。为使曲线变得更光滑，需要在中间插入一些点，使用`deval`函数，`sxint=deval(sol,xint)`, 其中，`xint`为点向量，函数`deval`根据这些点向量求解。`sol`为函数`bvp4c`的输出。

| 参数           | 功能                                                      |
| -------------- | --------------------------------------------------------- |
| sol.x          | Bvp4c 选择的网格                                          |
| sol.y          | sol.x 网格点处y(x)的近似值                                |
| sol.yp         | sol.x 网格点处y’(x)的近似值                               |
| sol.parameters | bvp4c 针对未知参数返回的值(如果有)                        |
| sol.solver     | ‘bvp4c’                                                   |
| sol.stats      | 计算成本统计信息(当设置stats选项和bvpset时，会显示此信息) |

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

# 【解】
![img1]({{site.page}}/images/img-2021-07-13/img1.png)

![img2]({{site.page}}/images/img-2021-07-13/img2.png)

![img3]({{site.page}}/images/img-2021-07-13/img3.png)

![img4]({{site.page}}/images/img-2021-07-13/img4.png)

# 【代码】

```python
%% 主函数
function main_Optimalcpntrol
clc;close all;
T = 2; N = 1000; dt = T/N; M = 1.5;
solinit = bvpinit(linspace(0, T, N), [0 0 0.5 0.5]);
options = bvpset('Stats','on','RelTol',1e-1);
sol = bvp4c(@BVP_ode,@BVP_bc,solinit,options);
t = sol.x;
y = sol.y;

%% 最小化性能指标
X(1,:)=[-2;1];
J=0;
% 此处计算性能指标，将其离散化再计算，值得学习
for k=1:N-1
    % 此处的 if 程序即《最优控制：数学理论与智能方法（上册）》 pp 209 式(4.2.29)
    if y(4,k)>M       % y(4,:)是协态变量p2(t)，那么y(3,:)是协态变量p1(t)
        U = -M;
    elseif y(4,k)<-M
        U = M;
    else
        U = -y(4,k);
    end
    X(k+1,2) = X(k,2) + dt*U;
    u(1,k)   = (X(k+1,2)-X(k,2)) / dt;
    X(k+1,1) = X(k,1) + dt*X(k,2);
    r        = dt * 0.5 * U^2;
    J        = J+r;
end

%% 画图
figure('Color',[1,1,1]);
plot(linspace(0,T,N), X(:,1), '-','LineWidth',1.5);hold on;
plot(linspace(0,T,N), X(:,2), '-.','LineWidth',1.5);
plot(linspace(0,T,999),u,'--','LineWidth',1.5);
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
        'LineWidth',1.2);
%     'YTick',[-2.5:1:2.1]);
saveas(gcf,'fg1.png');
end

%% 微分方程组
function dydt = BVP_ode(t,y)
M = 1.5;
% 用这行代码更好理解
% 此处的 if 程序即《最优控制：数学理论与智能方法（上册）》 pp 209 式(4.2.29)
if y(4)>M       % y(4,:)是协态变量p2(t)，那么y(3,:)是协态变量p1(t)
    u = -M;
elseif y(4)<-M
    u = M;
else
    u = -y(4);
end
% u = -y(4);
% u = max(-M,u);
% u = min(M,u);
dydt = [y(2)
        u
        0
        -y(3)];
end

%% 边界条件函数
function res = BVP_bc(ya,yb)
res = [ya(1) + 2
       ya(2) - 1
       yb(1)
       yb(2)];
end
```

仿真结果如下

![img5]({{site.page}}/images/img-2021-07-13/img5.png)

# 【参考文献】
[1] Jie Zhang, Fei-Yue Wang. 最优控制:数学理论与智能方法(上册)[M].  2017. pp 206-213.

[2] lynn156. 两点边值问题求解---bvp4c函数[OL]. [https://blog.csdn.net/lynn15600693998/article/details/86603731](https://blog.csdn.net/lynn15600693998/article/details/86603731) 2019-01-23.

[3] 瞿立建. 用Matlab bvp4c 解带未知参数的常微分方程边值问题，怎么给出合适的初值？[OL]. [http://www.joyfulphysics.net/index.php/archives/189/](http://www.joyfulphysics.net/index.php/archives/189/) 2018-05-23
