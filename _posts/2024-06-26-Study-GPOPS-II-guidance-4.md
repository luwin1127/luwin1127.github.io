---
permalink: /:year-:month-:day/:title
layout: post
title:  "GPOPS-II教程(4): 多级火箭上升最优控制问题"
author: "Lei Lie"
categories: Essay
tag: notdouban
---

# 本篇重点

> - `eventgroup`的使用；
> - `path`的使用；
> - 多个`phase`的使用。

# 问题描述

该问题考虑的是多级运载火箭的最优控制问题。控制目标是使火箭从地面发射并进入地球静止转移轨道（Geostationary transfer orbit，GTO），同时最大限度地利用上一子级火箭的剩余燃料。火箭上升过程分为四个阶段。

- 第一阶段，从地面发射，当一子级固体火箭发动机燃料耗尽时结束；
- 第二阶段，从一子级固体火箭发动机燃料耗尽时开始，当二子级固体火箭发动机燃料耗尽时结束；
- 第三阶段，从二子级固体火箭发动机燃料耗尽时开始，当第一主发动机耗尽时结束；
- 第四阶段，从第一主发动机耗尽时开始，当火箭到达GTO结束。

> 参考文献：
> [1] Betts, J. T., Practical Methods for Optimal Control and Estimation Using Nonlinear Programming, SIAM Press, Philadelphia, 2009.
> [2] Benson, D. A., A Gauss Pseudospectral Transcription for Optimal Control, Ph.D. thesis, Department of Aeronautics and Astronautics, Massachusetts Institute of Technology, Cambridge, Massachusetts, 2004.
> [3] Rao, A. V., Benson, D. A., Christopher Darby, M. A. P., Francolin, C., Sanders, I., and Huntington, G. T., “Algorithm 902: GPOPS, A MATLAB Software for Solving Multiple-Phase Optimal Control Problems Using the Gauss Pseudospectral Method,” ACM Transactions on Mathematical Software,
Vol. 37, No. 2, April–June 2010, pp. 22:1–22:39.

相关数据如下表所示。

|                        | 固体发动机 |   阶段1   |  阶段2   |
| :--------------------: | :--------: | :-------: | :------: |
|   总质量（$\rm kg$）   |  $19290$   | $104380$  | $19300$  |
| 推进剂质量（$\rm kg$） |  $17010$   |  $95550$  | $16820$  |
| 发动机推力（$\rm N$）  |  $628500$  | $1083100$ | $110094$ |
|    比冲（$\rm s$）     |   $284$    |  $301.7$  | $462.4$  |
|       发动机数量       |    $9$     |    $1$    |   $1$    |
|  燃烧时间（$\rm s$）   |   $75.2$   |   $261$   |  $700$   |

## 动力学方程

火箭的动力学方程在笛卡尔地球中心惯性坐标系（Cartesian Earth centered inertial，ECI）下表示为
$$
\left \{ \begin{array}{l}
\dot{\textbf{r}} = \textbf{v},\\
\dot{\textbf{v}} = -\frac{\mu}{||\textbf{r}||^3}\textbf{r} + \frac{T}{m} \textbf{u} + \frac{\textbf{D}}{m},\\
\dot{m} = -\frac{T}{g_0 T_{sp}}.
\end{array}\right.
$$

式中，

- $\textbf{r}(t) = \begin{bmatrix}x(t) & y(t) & z(t) \end{bmatrix}^\top$代表火箭在ECI下的位置，
- $\textbf{v}=\begin{bmatrix}v_x(t) & v_y(t) & v_z(t)\end{bmatrix}^\top$代表火箭在ECI下的速度，
- $T$代表真空推力，
- $g_0$为海拔高度0m时的重力加速度，
- $I_{sp}$为发动机的比冲，
- $\textbf{u}=\begin{bmatrix}u_x & u_y & u_z\end{bmatrix}^\top$代表推力方向，
- $\textbf{D} = \begin{bmatrix} D_x & D_y & D_z \end{bmatrix}^\top$为大气阻力。

大气阻力的表达式为

$$
\textbf{D} = -\frac{1}{2}C_D A_{ref} \rho ||\textbf{v}_{\rm rel}|| \textbf{v}_{\rm rel}
$$

式中，$C_D$为阻力系数，$A_{ref}$为参考区域，$\rho$为大气强度，$\textbf{v}_{\rm rel}$为地球相对速度，$\textbf{v}_{\rm rel}$的表达式为

$$
\textbf{v}_{\rm rel} = \textbf{v} - \boldsymbol{\omega} \times \textbf{r}
$$

式中，$\boldsymbol{\omega}$为地球相对惯性空间的角速度。大气强度可根据如下经验函数写出表达式，为

$$
\rho = \rho_0 \exp(-h/h_0)
$$
式中，

- $\rho_0$为海拔高度0m时的大气强度，
- $h = ||\textbf{r}|| - R_e$为高度，
- $R_e$为地球赤道半径，
- $h_0$为基准高度。

数据可见下表。

|           常量           |   数值    |
| :----------------------: | :-------: |
|   载荷质量（$\rm kg$）   |  $4164$   |
|  $A_{ref}$（$\rm m^2$）  |  $4 \pi$  |
|          $C_d$           |   $0.5$   |
| $\rho_0$（$\rm kg/m^3$） |  $1.225$  |
|    $h_0$（$\rm km$）     |   $7.2$   |
|     $t_1$（$\rm s$）     |  $75.2$   |
|     $t_2$（$\rm s$）     |  $150.4$  |
|     $t_3$（$\rm s$）     |   $261$   |
|    $R_e$（$\rm km$）     | $6378.14$ |
|   $V_E$（$\rm km/s$）    |  $7.905$  |

## 约束条件

该问题的初始条件为

$$
\begin{array}{l}
\textbf{r}(t_0) = \textbf{r}_0 = \begin{bmatrix}5605.2 & 0 & 3043.4\end{bmatrix}^\top \quad \text{km},	\\
\textbf{v}(t_0) = \textbf{v}_0 = \begin{bmatrix}0 & 0.4076 & 0\end{bmatrix}^\top \quad \text{km/s},	\\
m(t_0) = m_0 = 301454 \quad \text{kg}.
\end{array}
$$

终端条件为火箭所到达的GTO的轨道根数，为

$$
\begin{aligned}
a_f &= 24361.14 \quad \text{km},	\\
e_f &= 0.7308,	\\
i_f &= 28.5 \quad \text{deg},	\\
\Omega_f &= 269.8 \quad \text{deg},	\\
\omega_f &= 130.5 \quad \text{deg}.
\end{aligned}
$$

式中，

- $a$：半长轴长度。
- $e$：偏心率。
- $i$：轨道倾角。
- $\Omega$：升交点赤经。
- $\omega$：近地点辐角。
- **真近点角**$\varphi$不用给出，因为火箭在GTO内的确切位置无法给出。

上述轨道六根数可通过转换矩阵从惯性系转换为ECI系。

状态的路径约束为

$$
|\textbf{r}| \ge R_r
$$

其意义为火箭距离地心的长度必须大于地球半径。$R_r$为地球半径。控制量的路径约束为

$$
||\textbf{u}||^2_2 = u^2_1 + u^2_2 + u^2_3 = 1
$$

最后，火箭上升轨迹的四个阶段中的每一个状态，需要加上连结约束，避免阶段切换时出现不连续的情况，表达式为

$$
\begin{aligned}
\textbf{r}^{(p)}(t_f) = - \textbf{r}^{(p+1)}(t_0) &= \textbf{0},	\\
\textbf{v}^{(p)}(t_f) = - \textbf{v}^{(p+1)}(t_0) &= \textbf{0},	\\
m^{(p)}(t_f) - m^{(p)}_{dry} - m^{(p+1)}(t_0) &= 0.
\end{aligned}
$$

式中，上标$^p$代表目前所处的阶段，$p=1,2,3$。

## 性能指标

该问题的性能指标为
$$
J = -m^{(4)}(t_f)
$$
式中，上标$^{(4)}$代表第四阶段的火箭质量。

# GPOPS代码

## `main function`

按照之前处理问题的顺序，一步一步写下去。再次复习一下写`main function`的六个步骤：

1. 初始参数设置；
2. 边界条件设置；
3. 初值猜测；
4. 设置GPOPS求解器参数；
5. 求解；
6. 画图。

### 1. 初始参数设置

```matlab
%% 01.初始参数设置
%----------------------------------------------------%
%------------------- 辅助参数设置 -------------------%
%----------------------------------------------------%
earthRadius         = 6378145;
gravParam           = 3.986012e14;
initialMass         = 301454;
earthRotRate        = 7.29211585e-5;
seaLevelDensity     = 1.225;
densityScaleHeight  = 7200;
g0                  = 9.80665;

scales.length       = earthRadius;
scales.speed        = sqrt(gravParam/scales.length);
scales.time         = scales.length/scales.speed;
scales.acceleration = scales.speed/scales.time;
scales.mass         = initialMass;
scales.force        = scales.mass*scales.acceleration;
scales.area         = scales.length^2;
scales.volume       = scales.area.*scales.length;
scales.density      = scales.mass/scales.volume;
scales.gravparam    = scales.acceleration*scales.length^2;

omega               = earthRotRate*scales.time;
auxdata.omegaMatrix = omega*[0 -1 0;1 0 0;0 0 0];
auxdata.mu          = gravParam/scales.gravparam;
auxdata.cd          = 0.5;
auxdata.sa          = 4*pi/scales.area;
auxdata.rho0        = seaLevelDensity/scales.density;
auxdata.H           = densityScaleHeight/scales.length;
auxdata.Re          = earthRadius/scales.length;
auxdata.g0          = g0/scales.acceleration;

lat0                = 28.5*pi/180;         
x0                  = auxdata.Re*cos(lat0);
y0                  = 0;                   
z0                  = auxdata.Re*sin(lat0);
r0                  = [x0 y0 z0];
v0                  = r0*auxdata.omegaMatrix.';

btSrb = 75.2/scales.time;
btFirst = 261/scales.time;
btSecond = 700/scales.time;

t0 = 0/scales.time;
t1 = 75.2/scales.time;
t2 = 150.4/scales.time;
t3 = 261/scales.time;
t4 = 961/scales.time;

mTotSrb      = 19290/scales.mass;
mPropSrb     = 17010/scales.mass;
mDrySrb      = mTotSrb-mPropSrb;
mTotFirst    = 104380/scales.mass;
mPropFirst   = 95550/scales.mass;
mDryFirst    = mTotFirst-mPropFirst;
mTotSecond   = 19300/scales.mass;
mPropSecond  = 16820/scales.mass;
mDrySecond   = mTotSecond-mPropSecond;
mPayload     = 4164/scales.mass;
thrustSrb    = 628500/scales.force;
thrustFirst  = 1083100/scales.force;
thrustSecond = 110094/scales.force;
mdotSrb      = mPropSrb/btSrb;
ispSrb       = thrustSrb/(auxdata.g0*mdotSrb);
mdotFirst    = mPropFirst/btFirst;
ispFirst     = thrustFirst/(auxdata.g0*mdotFirst);
mdotSecond   = mPropSecond/btSecond;
ispSecond    = thrustSecond/(auxdata.g0*mdotSecond);

af = 24361140/scales.length;
ef = 0.7308;
incf = 28.5*pi/180;
Omf = 269.8*pi/180;
omf = 130.5*pi/180;
nuguess = 0;
cosincf = cos(incf);
cosOmf = cos(Omf);
cosomf = cos(omf);
oe = [af ef incf Omf omf nuguess];
[rout,vout] = launchoe2rv(oe,auxdata.mu);
rout = rout';
vout = vout';

m10 = mPayload+mTotSecond+mTotFirst+9*mTotSrb;
m1f = m10-(6*mdotSrb+mdotFirst)*t1;
m20 = m1f-6*mDrySrb;
m2f = m20-(3*mdotSrb+mdotFirst)*(t2-t1);
m30 = m2f-3*mDrySrb;
m3f = m30-mdotFirst*(t3-t2);
m40 = m3f-mDryFirst;
m4f = mPayload;

auxdata.thrustSrb    = thrustSrb;
auxdata.thrustFirst  = thrustFirst;
auxdata.thrustSecond = thrustSecond;
auxdata.ispSrb       = ispSrb;
auxdata.ispFirst     = ispFirst;
auxdata.ispSecond    = ispSecond;

rmin = -2*auxdata.Re;
rmax = -rmin;
vmin = -10000/scales.speed;
vmax = -vmin;
```

### 2. 边界条件设置

设置边界条件的时候要注意，因为本问题分成了**四个阶段**，就要分别设置四个阶段的边界条件。需要用到`phase`这个变量，前述教程中没有提过`phase`的原因是都只有一个阶段，GPOPS-II会默认`phase`为1。现在有四个阶段的话，用`iphase`来计数，用以区别四个阶段不同的参数。

在这个部分我只给出`phase=1`的代码，要是把四个阶段的代码全部写出来实在太过累赘，剩下的`phase=2`、`phase=3`、`phase=4`和`phase=1`的情况类似，变量名换一下即可。

```matlab
%% 02.边界条件设置
%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 1;
% 边界条件
bounds.phase(iphase).initialtime.lower = [t0]; 
bounds.phase(iphase).initialtime.upper = [t0]; 
bounds.phase(iphase).finaltime.lower = [t1]; 
bounds.phase(iphase).finaltime.upper = [t1]; 
bounds.phase(iphase).initialstate.lower = [r0(1:3),v0(1:3),m10];    
bounds.phase(iphase).initialstate.upper = [r0(1:3),v0(1:3),m10];    
bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f];
bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m10];
bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f]; 
bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m10]; 
bounds.phase(iphase).control.lower = -ones(1,3);
bounds.phase(iphase).control.upper = +ones(1,3);
bounds.phase(iphase).path.lower  = 1;
bounds.phase(iphase).path.upper  = 1;
```

> 注意最后两行代码，写的是
>
>```matlab
>bounds.phase(iphase).path.lower  = 1;
>bounds.phase(iphase).path.upper  = 1;
>```
>
>这是前文提到的控制量路径约束的代码，它解决的是等式约束的问题，控制量路径约束再重写在下面，是这样：
>
>$$
>||\textbf{u}||^2_2 = u^2_1 + u^2_2 + u^2_3 = 1
>$$
>
>至于状态量路径约束
>
>$$
>|\textbf{r}| \ge R_r
>$$
>
>没有显式地用`path`表示，而是在`main function`中的`bound`部分和`continuous function`中表示的。
>
>（这部分可以不用看，想要说明的是诸如状态量路径约束这样的**不等式约束**应该怎么写。）
>
>在第一阶段的`main function`中的`bound`部分，代码如下。
>
>```matlab
>rmin = -2*auxdata.Re;
>rmax = -rmin;
>
>bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f];
>bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m10];
>bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f]; 
>bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m10]; 
>```
>
>`rmin = -2*auxdata.Re;`和`rmax=2*auxdata.Re`（即`rmax = -rmin`）给出了一个很宽的状态量约束空间，能够保证**状态量一定不会大于地球赤道半径的两倍**。
>
>在第一阶段的`continuous function`部分，代码如下。
>
>```matlab
>r1 = x1(:,1:3);
>rad1 = sqrt(sum(r1.*r1,2));
>h1 = rad1-input.auxdata.Re;
>>rho1 = input.auxdata.rho0*exp(-h1/input.auxdata.H);
>bc1 = (rho1./(2*m1)).*input.auxdata.sa*input.auxdata.cd;
>bcspeed1 = bc1.*speedrel1;
>bcspeedmat1 = repmat(bcspeed1,1,3);
>Drag1 = -bcspeedmat1.*vrel1;
>
>vdot1 = thrust1+Drag1+grav1;
>```
>
>而上面的代码则**保证状态量一定不会小于地球赤道半径**，也就是说不会让火箭飞进地球里面去。

还有要写的是阶段切换时的边界约束，代码如下。

```matlab
%----------------------------------------------------%
%------------- 阶段切换的连续性保证条件 -------------%
%----------------------------------------------------%
bounds.eventgroup(1).lower = [zeros(1,6), -6*mDrySrb, 0];
bounds.eventgroup(1).upper = [zeros(1,6), -6*mDrySrb, 0];
bounds.eventgroup(2).lower = [zeros(1,6), -3*mDrySrb, 0];
bounds.eventgroup(2).upper = [zeros(1,6), -3*mDrySrb, 0];
bounds.eventgroup(3).lower = [zeros(1,6), -mDryFirst, 0];
bounds.eventgroup(3).upper = [zeros(1,6), -mDryFirst, 0];

%----------------------------------------------------%
%-------------- 第四阶段的终端条件约束 --------------%
%----------------------------------------------------%
bounds.eventgroup(4).lower = [af, ef, incf, Omf, omf];
bounds.eventgroup(4).upper = [af, ef, incf, Omf, omf];
```

上面是保证GPOPS-II在求解四个阶段时各个变量（时间、状态量、控制量等）不会断开，保证每个阶段的量都是接续的。

### 3.初值猜测

同样地，初值猜测也要写四个阶段，这里也只给出第一阶段的初值猜测。其余阶段的初值猜测变一下变量名就可以写出。

```matlab
%% 03.初值猜测
%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 1;
% 初值猜测
guess.phase(iphase).time = [t0; t1];
guess.phase(iphase).state(:,1) = [r0(1); r0(1)];
guess.phase(iphase).state(:,2) = [r0(2); r0(2)];
guess.phase(iphase).state(:,3) = [r0(3); r0(3)];
guess.phase(iphase).state(:,4) = [v0(1); v0(1)];
guess.phase(iphase).state(:,5) = [v0(2); v0(2)];
guess.phase(iphase).state(:,6) = [v0(3); v0(3)];
guess.phase(iphase).state(:,7) = [m10; m1f];
guess.phase(iphase).control(:,1) = [0; 0];
guess.phase(iphase).control(:,2) = [1; 1];
guess.phase(iphase).control(:,3) = [0; 0];
```

### 4.设置GPOPS求解器参数

要多说一嘴的是，因为有四个阶段，所以要设置四个阶段的网格条件，代码如下。

```matlab
%% 04.设置GPOPS求解器参数
%----------------------------------------------------%
%-------------- 每个阶段的网格细化条件 --------------%
%----------------------------------------------------%
for i=1:4
  meshphase(i).colpoints = 4*ones(1,10);
  meshphase(i).fraction = 0.1*ones(1,10);
end
```

其余部分按照常规写法即可，代码如下。

```matlab
setup.name = 'Launch-Vehicle-Ascent-Problem';
setup.functions.continuous = @launchContinuous;
setup.functions.endpoint = @launchEndpoint;
setup.mesh.phase = meshphase;
setup.nlp.solver = 'snopt';
setup.bounds = bounds;
setup.guess = guess;
setup.auxdata = auxdata;
setup.derivatives.supplier = 'sparseFD';
setup.derivatives.derivativelevel = 'second';
setup.derivatives.dependencies = 'sparseNaN';
setup.scales.method = 'automatic-bounds';
setup.mesh.method = 'hp1';
setup.mesh.tolerance = 1e-6;
setup.method = 'RPMintegration';
```

### 5.求解

```matlab
%% 05.求解
%----------------------------------------------------%
%----------- 使用 GPOPS2 求解最优控制问题 -----------%
%----------------------------------------------------%
totaltime = tic;
output = gpops2(setup);
totaltime = toc(totaltime);
```

### 6.画图

```matlab
%% 06.画图
% 数据处理
solution    = output.result.solution;
%----------------------------------------------------%
%-------------------- 第一阶段 ----------------------%
%----------------------------------------------------%
t{1}        = solution.phase(1).time*1e3;
rad1        = solution.phase(1).state(:,1:3);
alt{1}      = (sqrt(dot(rad1,rad1,2))-auxdata.Re)*scales.length/1000;
velocity1   = solution.phase(1).state(:,4:6);
speed{1}    = sqrt(dot(velocity1,velocity1,2));
mass{1}     = solution.phase(1).state(:,7);
control{1}  = solution.phase(1).control;

%----------------------------------------------------%
%-------------------- 第二阶段 ----------------------%
%----------------------------------------------------%
t{2}        = solution.phase(2).time*1e3;
rad2        = solution.phase(2).state(:,1:3);
alt{2}      = (sqrt(dot(rad2,rad2,2))-auxdata.Re)*scales.length/1000;
velocity2   = solution.phase(2).state(:,4:6);
speed{2}    = sqrt(dot(velocity2,velocity2,2));
mass{2}     = solution.phase(2).state(:,7);
control{2}  = solution.phase(2).control;

%----------------------------------------------------%
%-------------------- 第三阶段 ----------------------%
%----------------------------------------------------%
t{3}        = solution.phase(3).time*1e3;
rad3        = solution.phase(3).state(:,1:3);
alt{3}      = (sqrt(dot(rad3,rad3,2))-auxdata.Re)*scales.length/1000;
velocity3   = solution.phase(3).state(:,4:6);
speed{3}    = sqrt(dot(velocity3,velocity3,2));
mass{3}     = solution.phase(3).state(:,7);
control{3}  = solution.phase(3).control;

%----------------------------------------------------%
%-------------------- 第四阶段 ----------------------%
%----------------------------------------------------%
t{4}        = solution.phase(4).time*1e3;
rad4        = solution.phase(4).state(:,1:3);
alt{4}      = (sqrt(dot(rad4,rad4,2))-auxdata.Re)*scales.length/1000;
speed{4}    = solution.phase(4).state(:,4:6);
velocity4   = solution.phase(4).state(:,4:6);
speed{4}    = sqrt(dot(velocity4,velocity4,2));
mass{4}     = solution.phase(4).state(:,7);
control{4}  = solution.phase(4).control;

% 第一张图：高度
figure('Color',[1,1,1])
pp = plot(t{1},alt{1},'-o',t{2},alt{2},'-o',t{3},alt{3},'-o',t{4},alt{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('altitude (km)');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthEast');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.25);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchAltitude.png

% 第二张图：速度
figure('Color',[1,1,1])
pp = plot(t{1},speed{1},'-o',t{2},speed{2},'-o',t{3},speed{3},'-o',t{4},speed{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('speed (m/s)');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthEast');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.5);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchSpeed.png

% 第三张图：控制量
figure('Color',[1,1,1])
pp = plot(t{1},control{1},'-o',t{2},control{2},'-o',t{3},control{3},'-o',t{4},control{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('control');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthWest');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.5);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchControl.png
```

## `continuous function`

按照前面给出的动力学方程写`continuous function`即可。

$$
\left \{ \begin{array}{l}
\dot{\textbf{r}} = \textbf{v},\\
\dot{\textbf{v}} = -\frac{\mu}{||\textbf{r}||^3}\textbf{r} + \frac{T}{m} \textbf{u} + \frac{\textbf{D}}{m},\\
\dot{m} = -\frac{T}{g_0 T_{sp}}.
\end{array}\right.
$$

`continuous function`一般分成四步来写：

1. 取输入参数；

2. 计算过程变量；

3. 写动力学方程；

4. 给输出。

拿第一阶段的动力学方程作为说明。

### 1. 取输入参数

```matlab
%----------------------------------------------------%
%--------------- 第一阶段动力学方程 -----------------%
%----------------------------------------------------%
t1 = input.phase(1).time;
x1 = input.phase(1).state;
u1 = input.phase(1).control;
r1 = x1(:,1:3);
v1 = x1(:,4:6);
m1 = x1(:,7);
```

### 2. 计算过程变量

```matlab
rad1                = sqrt(sum(r1.*r1,2));
omegaMatrix         = input.auxdata.omegaMatrix;
omegacrossr         = r1*omegaMatrix.';
vrel1               = v1-omegacrossr;
speedrel1           = sqrt(sum(vrel1.*vrel1,2));
h1                  = rad1-input.auxdata.Re;
rho1                = input.auxdata.rho0*exp(-h1/input.auxdata.H);
bc1                 = (rho1./(2*m1)).*input.auxdata.sa*input.auxdata.cd;
bcspeed1            = bc1.*speedrel1;
bcspeedmat1         = repmat(bcspeed1,1,3);
Drag1               = -bcspeedmat1.*vrel1;
muoverradcubed1     = input.auxdata.mu./rad1.^3;
muoverradcubedmat1  = [muoverradcubed1 muoverradcubed1 muoverradcubed1];
grav1               = -muoverradcubedmat1.*r1;

TSrb1   = 6*input.auxdata.thrustSrb*ones(size(t1));
TFirst1 = input.auxdata.thrustFirst*ones(size(t1));
TTot1   = TSrb1+TFirst1;
m1dot1  = -TSrb1./(input.auxdata.g0*input.auxdata.ispSrb);
m2dot1  = -TFirst1./(input.auxdata.g0*input.auxdata.ispFirst);
```

### 3. 写动力学方程

```matlab
% 第一阶段：动力学约束
Toverm1     = TTot1./m1;
Tovermmat1  = [Toverm1 Toverm1 Toverm1];
thrust1     = Tovermmat1.*u1;
rdot1       = v1;
vdot1       = thrust1+Drag1+grav1;
mdot1       = m1dot1+m2dot1;
path1   = [sum(u1.*u1,2)];
```

这里要注意记得把`path`，也就是控制量的路径约束带上，`path1 = [sum(u1.*u1,2)];`就是$||\textbf{u}||^2_2 = u^2_1 + u^2_2 + u^2_3 = 1$的简易写法。

### 4. 给输出

```matlab
phaseout(1).dynamics = [rdot1 vdot1 mdot1];
phaseout(1).path = path1;
```

## `endpoint function`

复习一下性能指标函数，为
$$
J = -m^{(4)}(t_f)
$$
因此代码中可写为

```matlab
output.objective = -xf{4}(7);
```

其余部分按照四个阶段分别写就好，代码如下。

```matlab
%----------------------------------------------------%
%-------------------- 第一阶段 ----------------------%
%----------------------------------------------------%
t0{1} = input.phase(1).initialtime;
tf{1} = input.phase(1).finaltime;
x0{1} = input.phase(1).initialstate;
xf{1} = input.phase(1).finalstate;

%----------------------------------------------------%
%-------------------- 第二阶段 ----------------------%
%----------------------------------------------------%
t0{2} = input.phase(2).initialtime;
tf{2} = input.phase(2).finaltime;
x0{2} = input.phase(2).initialstate;
xf{2} = input.phase(2).finalstate;

%----------------------------------------------------%
%-------------------- 第三阶段 ----------------------%
%----------------------------------------------------%
t0{3} = input.phase(3).initialtime;
tf{3} = input.phase(3).finaltime;
x0{3} = input.phase(3).initialstate;
xf{3} = input.phase(3).finalstate;

%----------------------------------------------------%
%-------------------- 第四阶段 ----------------------%
%----------------------------------------------------%
t0{4} = input.phase(4).initialtime;
tf{4} = input.phase(4).finaltime;
x0{4} = input.phase(4).initialstate;
xf{4} = input.phase(4).finalstate;
```

再加上各个阶段切换之间的约束条件即可，代码如下。

```matlab
% Event Group 1: 连结第一阶段和第二阶段
output.eventgroup(1).event = [x0{2}(1:7)-xf{1}(1:7), t0{2}-tf{1}];
% Event Group 2: 连结第二阶段和第三阶段
output.eventgroup(2).event = [x0{3}(1:7)-xf{2}(1:7), t0{3}-tf{2}];
% Event Group 3: 连结第三阶段和第四阶段
output.eventgroup(3).event = [x0{4}(1:7)-xf{3}(1:7), t0{4}-tf{3}];
% Event Group 4: 进入至GTO轨道的终端约束
orbitalElements = launchrv2oe(xf{4}(1:3).',xf{4}(4:6).',input.auxdata.mu);
output.eventgroup(4).event = orbitalElements(1:5).';
```

## 坐标转换函数

这部分是将火箭的位置-速度模型转换为航天中常用的轨道六根数，方便描述轨道特性，所以加上了（1）位置-速度模型转轨道六根数函数和（2）轨道六根数转位置-速度模型。

### 位置-速度模型转轨道六根数函数

```matlab
%-------------------------------------------------------------------------%
%------------------- BEGIN Function launchrv2oe.m ------------------------%
%-------------------------------------------------------------------------%
function oe = launchrv2oe(rv,vv,mu)
K  = [0;0;1];
hv = cross(rv,vv);
nv = cross(K,hv);
n  = sqrt(nv.'*nv);
h2 = (hv.'*hv);
v2 = (vv.'*vv);
r  = sqrt(rv.'*rv);
ev = 1/mu *( (v2-mu/r)*rv - (rv.'*vv)*vv );
p  = h2/mu;
e  = sqrt(ev.'*ev);
a  = p/(1-e*e);
i  = acos(hv(3)/sqrt(h2));
Om = acos(nv(1)/n);
if (nv(2)<0-eps)
  Om = 2*pi-Om;
end
om = acos(nv.'*ev/n/e);
if (ev(3)<0)
  om = 2*pi-om;
end
nu = acos(ev.'*rv/e/r);
if (rv.'*vv<0)
  nu = 2*pi-nu;
end
oe = [a; e; i; Om; om; nu];
end
%-------------------------------------------------------------------------%
%---------------------- END Function launchrv2oe.m -----------------------%
%-------------------------------------------------------------------------%
```



### 轨道六根数转位置-速度模型函数

```matlab
%-------------------------------------------------------------------------%
%------------------- BEGIN Function launchoe2rv.m ------------------------%
%-------------------------------------------------------------------------%
function [ri,vi] = launchoe2rv(oe,mu)
a=oe(1); e=oe(2); i=oe(3); Om=oe(4); om=oe(5); nu=oe(6);
p = a*(1-e*e);
r = p/(1+e*cos(nu));
rv = [r*cos(nu); r*sin(nu); 0];
vv = sqrt(mu/p)*[-sin(nu); e+cos(nu); 0];
cO = cos(Om);  sO = sin(Om);
co = cos(om);  so = sin(om);
ci = cos(i);   si = sin(i);
R  = [cO*co-sO*so*ci  -cO*so-sO*co*ci  sO*si;
      sO*co+cO*so*ci  -sO*so+cO*co*ci -cO*si;
      so*si            co*si           ci];
ri = R*rv;
vi = R*vv;
end
%-------------------------------------------------------------------------%
%---------------------- END Function launchoe2rv.m -----------------------%
%-------------------------------------------------------------------------%
```

## 完整代码

```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% 功能描述：多级火箭上升问题
% 作者：Lei Lie
% 时间：2024/06/26
% 版本：1.0
% - 完成了代码框架的初始搭建
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
clc;
clear;
close all;

%% 01.初始参数设置
%----------------------------------------------------%
%------------------- 辅助参数设置 -------------------%
%----------------------------------------------------%
earthRadius         = 6378145;
gravParam           = 3.986012e14;
initialMass         = 301454;
earthRotRate        = 7.29211585e-5;
seaLevelDensity     = 1.225;
densityScaleHeight  = 7200;
g0                  = 9.80665;

scales.length       = earthRadius;
scales.speed        = sqrt(gravParam/scales.length);
scales.time         = scales.length/scales.speed;
scales.acceleration = scales.speed/scales.time;
scales.mass         = initialMass;
scales.force        = scales.mass*scales.acceleration;
scales.area         = scales.length^2;
scales.volume       = scales.area.*scales.length;
scales.density      = scales.mass/scales.volume;
scales.gravparam    = scales.acceleration*scales.length^2;

omega               = earthRotRate*scales.time;
auxdata.omegaMatrix = omega*[0 -1 0;1 0 0;0 0 0];
auxdata.mu          = gravParam/scales.gravparam;
auxdata.cd          = 0.5;
auxdata.sa          = 4*pi/scales.area;
auxdata.rho0        = seaLevelDensity/scales.density;
auxdata.H           = densityScaleHeight/scales.length;
auxdata.Re          = earthRadius/scales.length;
auxdata.g0          = g0/scales.acceleration;

lat0                = 28.5*pi/180;         
x0                  = auxdata.Re*cos(lat0);
y0                  = 0;                   
z0                  = auxdata.Re*sin(lat0);
r0                  = [x0 y0 z0];
v0                  = r0*auxdata.omegaMatrix.';

btSrb = 75.2/scales.time;
btFirst = 261/scales.time;
btSecond = 700/scales.time;

t0 = 0/scales.time;
t1 = 75.2/scales.time;
t2 = 150.4/scales.time;
t3 = 261/scales.time;
t4 = 961/scales.time;

mTotSrb      = 19290/scales.mass;
mPropSrb     = 17010/scales.mass;
mDrySrb      = mTotSrb-mPropSrb;
mTotFirst    = 104380/scales.mass;
mPropFirst   = 95550/scales.mass;
mDryFirst    = mTotFirst-mPropFirst;
mTotSecond   = 19300/scales.mass;
mPropSecond  = 16820/scales.mass;
mDrySecond   = mTotSecond-mPropSecond;
mPayload     = 4164/scales.mass;
thrustSrb    = 628500/scales.force;
thrustFirst  = 1083100/scales.force;
thrustSecond = 110094/scales.force;
mdotSrb      = mPropSrb/btSrb;
ispSrb       = thrustSrb/(auxdata.g0*mdotSrb);
mdotFirst    = mPropFirst/btFirst;
ispFirst     = thrustFirst/(auxdata.g0*mdotFirst);
mdotSecond   = mPropSecond/btSecond;
ispSecond    = thrustSecond/(auxdata.g0*mdotSecond);

af = 24361140/scales.length;
ef = 0.7308;
incf = 28.5*pi/180;
Omf = 269.8*pi/180;
omf = 130.5*pi/180;
nuguess = 0;
cosincf = cos(incf);
cosOmf = cos(Omf);
cosomf = cos(omf);
oe = [af ef incf Omf omf nuguess];
[rout,vout] = launchoe2rv(oe,auxdata.mu);
rout = rout';
vout = vout';

m10 = mPayload+mTotSecond+mTotFirst+9*mTotSrb;
m1f = m10-(6*mdotSrb+mdotFirst)*t1;
m20 = m1f-6*mDrySrb;
m2f = m20-(3*mdotSrb+mdotFirst)*(t2-t1);
m30 = m2f-3*mDrySrb;
m3f = m30-mdotFirst*(t3-t2);
m40 = m3f-mDryFirst;
m4f = mPayload;

auxdata.thrustSrb    = thrustSrb;
auxdata.thrustFirst  = thrustFirst;
auxdata.thrustSecond = thrustSecond;
auxdata.ispSrb       = ispSrb;
auxdata.ispFirst     = ispFirst;
auxdata.ispSecond    = ispSecond;

rmin = -2*auxdata.Re;
rmax = -rmin;
vmin = -10000/scales.speed;
vmax = -vmin;

%% 02.边界条件设置
%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 1;
% 边界条件
bounds.phase(iphase).initialtime.lower = [t0]; 
bounds.phase(iphase).initialtime.upper = [t0]; 
bounds.phase(iphase).finaltime.lower = [t1]; 
bounds.phase(iphase).finaltime.upper = [t1]; 
bounds.phase(iphase).initialstate.lower = [r0(1:3),v0(1:3),m10];    
bounds.phase(iphase).initialstate.upper = [r0(1:3),v0(1:3),m10];    
bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f];
bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m10];
bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m1f]; 
bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m10]; 
bounds.phase(iphase).control.lower = -ones(1,3);
bounds.phase(iphase).control.upper = +ones(1,3);
bounds.phase(iphase).path.lower  = 1;
bounds.phase(iphase).path.upper  = 1;

%----------------------------------------------------%
%---------------- 第二阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 2;
% 边界条件
bounds.phase(iphase).initialtime.lower = [t1]; 
bounds.phase(iphase).initialtime.upper = [t1]; 
bounds.phase(iphase).finaltime.lower = [t2]; 
bounds.phase(iphase).finaltime.upper = [t2]; 
bounds.phase(iphase).initialstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m2f];
bounds.phase(iphase).initialstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m20];
bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m2f];
bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m20];
bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m2f]; 
bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m20]; 
bounds.phase(iphase).control.lower = -ones(1,3);
bounds.phase(iphase).control.upper = +ones(1,3);
bounds.phase(iphase).path.lower  = 1;
bounds.phase(iphase).path.upper  = 1;

%----------------------------------------------------%
%---------------- 第三阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 3;
% 边界条件
bounds.phase(iphase).initialtime.lower = [t2]; 
bounds.phase(iphase).initialtime.upper = [t2]; 
bounds.phase(iphase).finaltime.lower = [t3]; 
bounds.phase(iphase).finaltime.upper = [t3]; 
bounds.phase(iphase).initialstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m3f];
bounds.phase(iphase).initialstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m30];
bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m3f];
bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m30];
bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m3f]; 
bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m30]; 
bounds.phase(iphase).control.lower = -ones(1,3);
bounds.phase(iphase).control.upper = +ones(1,3);
bounds.phase(iphase).path.lower  = 1;
bounds.phase(iphase).path.upper  = 1;

%----------------------------------------------------%
%---------------- 第四阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 4;
% 边界条件
bounds.phase(iphase).initialtime.lower = [t3]; 
bounds.phase(iphase).initialtime.upper = [t3]; 
bounds.phase(iphase).finaltime.lower = [t3]; 
bounds.phase(iphase).finaltime.upper = [t4]; 
bounds.phase(iphase).initialstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m4f];
bounds.phase(iphase).initialstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m40];
bounds.phase(iphase).state.lower = [rmin*ones(1,3),vmin*ones(1,3),m4f];
bounds.phase(iphase).state.upper = [rmax*ones(1,3),vmax*ones(1,3),m40];
bounds.phase(iphase).finalstate.lower = [rmin*ones(1,3),vmin*ones(1,3),m4f]; 
bounds.phase(iphase).finalstate.upper = [rmax*ones(1,3),vmax*ones(1,3),m40]; 
bounds.phase(iphase).control.lower = -ones(1,3);
bounds.phase(iphase).control.upper = +ones(1,3);
bounds.phase(iphase).path.lower  = 1;
bounds.phase(iphase).path.upper  = 1;

%----------------------------------------------------%
%------------- 阶段切换的连续性保证条件 -------------%
%----------------------------------------------------%
bounds.eventgroup(1).lower = [zeros(1,6), -6*mDrySrb, 0];
bounds.eventgroup(1).upper = [zeros(1,6), -6*mDrySrb, 0];
bounds.eventgroup(2).lower = [zeros(1,6), -3*mDrySrb, 0];
bounds.eventgroup(2).upper = [zeros(1,6), -3*mDrySrb, 0];
bounds.eventgroup(3).lower = [zeros(1,6), -mDryFirst, 0];
bounds.eventgroup(3).upper = [zeros(1,6), -mDryFirst, 0];

%----------------------------------------------------%
%-------------- 第四阶段的终端条件约束 --------------%
%----------------------------------------------------%
bounds.eventgroup(4).lower = [af, ef, incf, Omf, omf];
bounds.eventgroup(4).upper = [af, ef, incf, Omf, omf];

%% 03.初值猜测
%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 1;
% 初值猜测
guess.phase(iphase).time = [t0; t1];
guess.phase(iphase).state(:,1) = [r0(1); r0(1)];
guess.phase(iphase).state(:,2) = [r0(2); r0(2)];
guess.phase(iphase).state(:,3) = [r0(3); r0(3)];
guess.phase(iphase).state(:,4) = [v0(1); v0(1)];
guess.phase(iphase).state(:,5) = [v0(2); v0(2)];
guess.phase(iphase).state(:,6) = [v0(3); v0(3)];
guess.phase(iphase).state(:,7) = [m10; m1f];
guess.phase(iphase).control(:,1) = [0; 0];
guess.phase(iphase).control(:,2) = [1; 1];
guess.phase(iphase).control(:,3) = [0; 0];

%----------------------------------------------------%
%---------------- 第二阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 2;
% 初值猜测
guess.phase(iphase).time = [t1; t2];
guess.phase(iphase).state(:,1) = [r0(1); r0(1)];
guess.phase(iphase).state(:,2) = [r0(2); r0(2)];
guess.phase(iphase).state(:,3) = [r0(3); r0(3)];
guess.phase(iphase).state(:,4) = [v0(1); v0(1)];
guess.phase(iphase).state(:,5) = [v0(2); v0(2)];
guess.phase(iphase).state(:,6) = [v0(3); v0(3)];
guess.phase(iphase).state(:,7) = [m10; m1f];
guess.phase(iphase).control(:,1) = [0; 0];
guess.phase(iphase).control(:,2) = [1; 1];
guess.phase(iphase).control(:,3) = [0; 0];

%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 3;
% 初值猜测
guess.phase(iphase).time       = [t2; t3];
guess.phase(iphase).state(:,1) = [rout(1); rout(1)];
guess.phase(iphase).state(:,2) = [rout(2); rout(2)];
guess.phase(iphase).state(:,3) = [rout(3); rout(3)];
guess.phase(iphase).state(:,4) = [vout(1); vout(1)];
guess.phase(iphase).state(:,5) = [vout(2); vout(2)];
guess.phase(iphase).state(:,6) = [vout(3); vout(3)];
guess.phase(iphase).state(:,7) = [m30; m3f];
guess.phase(iphase).control(:,1) = [0; 0];
guess.phase(iphase).control(:,2) = [1; 1];
guess.phase(iphase).control(:,3) = [0; 0];

%----------------------------------------------------%
%---------------- 第一阶段参数设置 ------------------%
%----------------------------------------------------%
iphase = 4;
% 初值猜测
guess.phase(iphase).time    = [t3; t4];
guess.phase(iphase).state(:,1) = [rout(1) rout(1)];
guess.phase(iphase).state(:,2) = [rout(2) rout(2)];
guess.phase(iphase).state(:,3) = [rout(3) rout(3)];
guess.phase(iphase).state(:,4) = [vout(1) vout(1)];
guess.phase(iphase).state(:,5) = [vout(2) vout(2)];
guess.phase(iphase).state(:,6) = [vout(3) vout(3)];
guess.phase(iphase).state(:,7) = [m40; m4f];
guess.phase(iphase).control(:,1) = [0; 0];
guess.phase(iphase).control(:,2) = [1; 1];
guess.phase(iphase).control(:,3) = [0; 0];

%% 04.设置GPOPS求解器参数
%----------------------------------------------------%
%-------------- 每个阶段的网格细化条件 --------------%
%----------------------------------------------------%
for i=1:4
  meshphase(i).colpoints = 4*ones(1,10);
  meshphase(i).fraction = 0.1*ones(1,10);
end

setup.name = 'Launch-Vehicle-Ascent-Problem';
setup.functions.continuous = @launchContinuous;
setup.functions.endpoint = @launchEndpoint;
setup.mesh.phase = meshphase;
setup.nlp.solver = 'snopt';
setup.bounds = bounds;
setup.guess = guess;
setup.auxdata = auxdata;
setup.derivatives.supplier = 'sparseFD';
setup.derivatives.derivativelevel = 'second';
setup.derivatives.dependencies = 'sparseNaN';
setup.scales.method = 'automatic-bounds';
setup.mesh.method = 'hp1';
setup.mesh.tolerance = 1e-6;
setup.method = 'RPMintegration';

%% 05.求解
%----------------------------------------------------%
%----------- 使用 GPOPS2 求解最优控制问题 -----------%
%----------------------------------------------------%
totaltime = tic;
output = gpops2(setup);
totaltime = toc(totaltime);

%% 06.画图
% 数据处理
solution    = output.result.solution;
%----------------------------------------------------%
%-------------------- 第一阶段 ----------------------%
%----------------------------------------------------%
t{1}        = solution.phase(1).time*1e3;
rad1        = solution.phase(1).state(:,1:3);
alt{1}      = (sqrt(dot(rad1,rad1,2))-auxdata.Re)*scales.length/1000;
velocity1   = solution.phase(1).state(:,4:6);
speed{1}    = sqrt(dot(velocity1,velocity1,2));
mass{1}     = solution.phase(1).state(:,7);
control{1}  = solution.phase(1).control;

%----------------------------------------------------%
%-------------------- 第二阶段 ----------------------%
%----------------------------------------------------%
t{2}        = solution.phase(2).time*1e3;
rad2        = solution.phase(2).state(:,1:3);
alt{2}      = (sqrt(dot(rad2,rad2,2))-auxdata.Re)*scales.length/1000;
velocity2   = solution.phase(2).state(:,4:6);
speed{2}    = sqrt(dot(velocity2,velocity2,2));
mass{2}     = solution.phase(2).state(:,7);
control{2}  = solution.phase(2).control;

%----------------------------------------------------%
%-------------------- 第三阶段 ----------------------%
%----------------------------------------------------%
t{3}        = solution.phase(3).time*1e3;
rad3        = solution.phase(3).state(:,1:3);
alt{3}      = (sqrt(dot(rad3,rad3,2))-auxdata.Re)*scales.length/1000;
velocity3   = solution.phase(3).state(:,4:6);
speed{3}    = sqrt(dot(velocity3,velocity3,2));
mass{3}     = solution.phase(3).state(:,7);
control{3}  = solution.phase(3).control;

%----------------------------------------------------%
%-------------------- 第四阶段 ----------------------%
%----------------------------------------------------%
t{4}        = solution.phase(4).time*1e3;
rad4        = solution.phase(4).state(:,1:3);
alt{4}      = (sqrt(dot(rad4,rad4,2))-auxdata.Re)*scales.length/1000;
speed{4}    = solution.phase(4).state(:,4:6);
velocity4   = solution.phase(4).state(:,4:6);
speed{4}    = sqrt(dot(velocity4,velocity4,2));
mass{4}     = solution.phase(4).state(:,7);
control{4}  = solution.phase(4).control;

% 第一张图：高度
figure('Color',[1,1,1])
pp = plot(t{1},alt{1},'-o',t{2},alt{2},'-o',t{3},alt{3},'-o',t{4},alt{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('altitude (km)');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthEast');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.25);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchAltitude.png

% 第二张图：速度
figure('Color',[1,1,1])
pp = plot(t{1},speed{1},'-o',t{2},speed{2},'-o',t{3},speed{3},'-o',t{4},speed{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('speed (m/s)');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthEast');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.5);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchSpeed.png

% 第三张图：控制量
figure('Color',[1,1,1])
pp = plot(t{1},control{1},'-o',t{2},control{2},'-o',t{3},control{3},'-o',t{4},control{4},'-o');
xl = xlabel('time (s)');
yl = ylabel('control');
ll = legend('Phase 1','Phase 2','Phase 3','Phase 4','Location','SouthWest');
set(xl,'FontSize',14);
set(yl,'FontSize',14);
set(ll,'FontSize',14);
set(pp,'LineWidth',1.5);
set(gca,'FontSize',14,'FontName','Times New Roman');
print -dpng launchControl.png

%-------------------------------------------------------------------------%
%------------------- BEGIN Function launchContinuous.m -------------------%
%-------------------------------------------------------------------------%
function phaseout = launchContinuous(input)
%----------------------------------------------------%
%--------------- 第一阶段动力学方程 -----------------%
%----------------------------------------------------%
t1 = input.phase(1).time;
x1 = input.phase(1).state;
u1 = input.phase(1).control;
r1 = x1(:,1:3);
v1 = x1(:,4:6);
m1 = x1(:,7);

rad1                = sqrt(sum(r1.*r1,2));
omegaMatrix         = input.auxdata.omegaMatrix;
omegacrossr         = r1*omegaMatrix.';
vrel1               = v1-omegacrossr;
speedrel1           = sqrt(sum(vrel1.*vrel1,2));
h1                  = rad1-input.auxdata.Re;
rho1                = input.auxdata.rho0*exp(-h1/input.auxdata.H);
bc1                 = (rho1./(2*m1)).*input.auxdata.sa*input.auxdata.cd;
bcspeed1            = bc1.*speedrel1;
bcspeedmat1         = repmat(bcspeed1,1,3);
Drag1               = -bcspeedmat1.*vrel1;
muoverradcubed1     = input.auxdata.mu./rad1.^3;
muoverradcubedmat1  = [muoverradcubed1 muoverradcubed1 muoverradcubed1];
grav1               = -muoverradcubedmat1.*r1;

TSrb1   = 6*input.auxdata.thrustSrb*ones(size(t1));
TFirst1 = input.auxdata.thrustFirst*ones(size(t1));
TTot1   = TSrb1+TFirst1;
m1dot1  = -TSrb1./(input.auxdata.g0*input.auxdata.ispSrb);
m2dot1  = -TFirst1./(input.auxdata.g0*input.auxdata.ispFirst);

% 第一阶段：动力学约束
Toverm1     = TTot1./m1;
Tovermmat1  = [Toverm1 Toverm1 Toverm1];
thrust1     = Tovermmat1.*u1;
rdot1       = v1;
vdot1       = thrust1+Drag1+grav1;
mdot1       = m1dot1+m2dot1;
phaseout(1).dynamics = [rdot1 vdot1 mdot1];

% 第一阶段：路径约束
path1   = [sum(u1.*u1,2)];
phaseout(1).path = path1;

%----------------------------------------------------%
%--------------- 第二阶段动力学方程 -----------------%
%----------------------------------------------------%
t2 = input.phase(2).time;
x2 = input.phase(2).state;
u2 = input.phase(2).control;
r2 = x2(:,1:3);
v2 = x2(:,4:6);
m2 = x2(:,7);

rad2                = sqrt(sum(r2.*r2,2));
omegaMatrix         = input.auxdata.omegaMatrix;
omegacrossr         = r2*omegaMatrix.';
vrel2               = v2-omegacrossr;
speedrel2           = sqrt(sum(vrel2.*vrel2,2));
h2                  = rad2-input.auxdata.Re;
rho2                = input.auxdata.rho0*exp(-h2/input.auxdata.H);
bc2                 = (rho2./(2*m2)).*input.auxdata.sa*input.auxdata.cd;
bcspeed2            = bc2.*speedrel2;
bcspeedmat2         = repmat(bcspeed2,1,3);
Drag2               = -bcspeedmat2.*vrel2;
muoverradcubed2     = input.auxdata.mu./rad2.^3;
muoverradcubedmat2  = [muoverradcubed2 muoverradcubed2 muoverradcubed2];
grav2               = -muoverradcubedmat2.*r2;

TSrb2   = 3*input.auxdata.thrustSrb*ones(size(t2));
TFirst2 = input.auxdata.thrustFirst*ones(size(t2));
TTot2   = TSrb2+TFirst2;
m1dot2  = -TSrb2./(input.auxdata.g0*input.auxdata.ispSrb);
m2dot2  = -TFirst2./(input.auxdata.g0*input.auxdata.ispFirst);

% 第二阶段：动力学约束
Toverm2     = TTot2./m2;
Tovermmat2  = [Toverm2 Toverm2 Toverm2];
thrust2     = Tovermmat2.*u2;
rdot2       = v2;
vdot2       = thrust2+Drag2+grav2;
mdot2       = m1dot2+m2dot2;    
phaseout(2).dynamics = [rdot2 vdot2 mdot2];

% 第二阶段：路径约束
path2   = [sum(u2.*u2,2)];
phaseout(2).path = path2;

%----------------------------------------------------%
%--------------- 第三阶段动力学方程 -----------------%
%----------------------------------------------------%
t3 = input.phase(3).time;
x3 = input.phase(3).state;
u3 = input.phase(3).control;
r3 = x3(:,1:3);
v3 = x3(:,4:6);
m3 = x3(:,7);

rad3 = sqrt(sum(r3.*r3,2));
omegaMatrix         = input.auxdata.omegaMatrix;
omegacrossr         = r3*omegaMatrix.';
vrel3               = v3-omegacrossr;
speedrel3           = sqrt(sum(vrel3.*vrel3,2));
h3                  = rad3-input.auxdata.Re;
rho3                = input.auxdata.rho0*exp(-h3/input.auxdata.H);
bc3                 = (rho3./(2*m3)).*input.auxdata.sa*input.auxdata.cd;
bcspeed3            = bc3.*speedrel3;
bcspeedmat3         = repmat(bcspeed3,1,3);
Drag3               = -bcspeedmat3.*vrel3;
muoverradcubed3     = input.auxdata.mu./rad3.^3;
muoverradcubedmat3  = [muoverradcubed3 muoverradcubed3 muoverradcubed3];
grav3               = -muoverradcubedmat3.*r3;

% 第三阶段：动力学约束
TTot3       = input.auxdata.thrustFirst*ones(size(t3));
Toverm3     = TTot3./m3;
Tovermmat3  = [Toverm3 Toverm3 Toverm3];
thrust3     = Tovermmat3.*u3;
rdot3       = v3;
vdot3       = thrust3+Drag3+grav3;
mdot3       = -TTot3./(input.auxdata.g0*input.auxdata.ispFirst);
phaseout(3).dynamics = [rdot3 vdot3 mdot3];

% 第三阶段：路径约束
path3 = [sum(u3.*u3,2)];
phaseout(3).path = path3;

%----------------------------------------------------%
%--------------- 第四阶段动力学方程 -----------------%
%----------------------------------------------------%
t4 = input.phase(4).time;
x4 = input.phase(4).state;
u4 = input.phase(4).control;
r4 = x4(:,1:3);
v4 = x4(:,4:6);
m4 = x4(:,7);

rad4                = sqrt(sum(r4.*r4,2));
omegacrossr         = r4*input.auxdata.omegaMatrix.';
vrel4               = v4-omegacrossr;
speedrel4           = sqrt(sum(vrel4.*vrel4,2));
h4                  = rad4-input.auxdata.Re;
rho4                = input.auxdata.rho0*exp(-h4/input.auxdata.H);
bc4                 = (rho4./(2*m4)).*input.auxdata.sa*input.auxdata.cd;
bcspeed4            = bc4.*speedrel4;
bcspeedmat4         = repmat(bcspeed4,1,3);
Drag4               = -bcspeedmat4.*vrel4;
muoverradcubed4     = input.auxdata.mu./rad4.^3;
muoverradcubedmat4  = [muoverradcubed4 muoverradcubed4 muoverradcubed4];
grav4               = -muoverradcubedmat4.*r4;

% 第四阶段：动力学约束
TTot4       = input.auxdata.thrustSecond*ones(size(t4));
Toverm4     = TTot4./m4;
Tovermmat4  = [Toverm4 Toverm4 Toverm4];
thrust4     = Tovermmat4.*u4;
rdot4       = v4;
vdot4       = thrust4+Drag4+grav4;
mdot4       = -TTot4/(input.auxdata.g0*input.auxdata.ispSecond);
phaseout(4).dynamics = [rdot4 vdot4 mdot4];

% 第四阶段：路径约束
path4 = [sum(u4.*u4,2)];
phaseout(4).path = path4;
end
%-------------------------------------------------------------------------%
%--------------------- END Function launchContinuous.m -------------------%
%-------------------------------------------------------------------------%

%-------------------------------------------------------------------------%
%--------------------- BEGIN Function launchEndpoint.m -------------------%
%-------------------------------------------------------------------------%
function output = launchEndpoint(input)
%----------------------------------------------------%
%-------------------- 第一阶段 ----------------------%
%----------------------------------------------------%
t0{1} = input.phase(1).initialtime;
tf{1} = input.phase(1).finaltime;
x0{1} = input.phase(1).initialstate;
xf{1} = input.phase(1).finalstate;

%----------------------------------------------------%
%-------------------- 第二阶段 ----------------------%
%----------------------------------------------------%
t0{2} = input.phase(2).initialtime;
tf{2} = input.phase(2).finaltime;
x0{2} = input.phase(2).initialstate;
xf{2} = input.phase(2).finalstate;

%----------------------------------------------------%
%-------------------- 第三阶段 ----------------------%
%----------------------------------------------------%
t0{3} = input.phase(3).initialtime;
tf{3} = input.phase(3).finaltime;
x0{3} = input.phase(3).initialstate;
xf{3} = input.phase(3).finalstate;

%----------------------------------------------------%
%-------------------- 第四阶段 ----------------------%
%----------------------------------------------------%
t0{4} = input.phase(4).initialtime;
tf{4} = input.phase(4).finaltime;
x0{4} = input.phase(4).initialstate;
xf{4} = input.phase(4).finalstate;

% Event Group 1: 连结第一阶段和第二阶段
output.eventgroup(1).event = [x0{2}(1:7)-xf{1}(1:7), t0{2}-tf{1}];
% Event Group 2: 连结第二阶段和第三阶段
output.eventgroup(2).event = [x0{3}(1:7)-xf{2}(1:7), t0{3}-tf{2}];
% Event Group 3: 连结第三阶段和第四阶段
output.eventgroup(3).event = [x0{4}(1:7)-xf{3}(1:7), t0{4}-tf{3}];
% Event Group 4: 进入至GTO轨道的终端约束
orbitalElements = launchrv2oe(xf{4}(1:3).',xf{4}(4:6).',input.auxdata.mu);
output.eventgroup(4).event = orbitalElements(1:5).';

% 性能指标
output.objective = -xf{4}(7);
end
%-------------------------------------------------------------------------%
%--------------------- END Function launchEndpoint.m ---------------------%
%-------------------------------------------------------------------------%
```

## 仿真结果

高度：

![launchAltitude](./../images/img-2024-06-26/launchAltitude.png)

速度：

![launchSpeed](./../images/img-2024-06-26/launchSpeed.png)

控制量：

![launchControl](./../images/img-2024-06-26/launchControl.png)

# 最后

这篇文档的主要目的是通过解决多级火箭上升最优控制问题，来说明`eventgroup`和`path`的功能。

## `eventgroup`

`eventgroup`是用来管理GPOPS-II中事件触发的集合，一般多用于解决多阶段的最优控制中多阶段之间的变量连续性问题。主要有以下几个作用：

- 触发事件的处理。
- 约束条件的激活。
- 控制参数的切换。

## `path`

`path`通常是所求解问题中的路径约束，也就是等式约束，用于描述整个求解时间历程里状态量或控制量必须满足的条件。

这个部分其实也体现了GPOPS-II的部分局限性，它没有很好地办法处理不等式约束或者非线性约束，只能通过强硬地约束上下界来控制变量的边界。这和估计的精度有很大关联，如果估计的约束上下限不准确，会造成求解难度加大。

## `phase`

`phase`是GPOPS中处理多阶段最优控制问题需要使用的变量，也是GPOPS的亮点和难点所在。有了`phase`，多阶段的最优控制问题只需要按照不同的阶段写出相同的参数设置即可，然而难也是难在这里，因为各个阶段可能存在差异，这些差异容易产生细微、难以发现的错误，导致出BUG，所以这个部分要十分仔细地写代码。

## 联系方式

欢迎通过邮箱联系我：lordofdapanji@foxmail.com

来信请注明你的身份，否则恕不回信。
