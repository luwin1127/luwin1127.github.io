---
permalink: /:year-:month-:day/:title
layout: post
title:  "Python、STK、MATLAB的连接和使用"
author: "Lei Lie"
categories: Essay
tag: notdouban
---

# 写在前面

预备知识：

[Python通过命令行调用MATLAB的简单实现](https://luwin1127.github.io/2021-07-28/Study-Python-to-Matlab)。这篇文章讲明了如何使用 Python 调用 MATLAB 的原理和过程，并给出相应代码。

[STK、MATLAB、SQL的连接和使用](https://luwin1127.github.io/2021-07-29/Study-STK-Matlab-SQL)。这篇文章讲明了STK 11.2、MATLAB 2014a、SQL 2019的安装过程，以及互联过程，并给出相应代码。

本文在上面两篇文章的基础上，讲解如何使用 Python 驱动 STK。

实际上要通过MATLAB作为媒介，即先使用 Python 驱动 MATLAB，MATLAB 驱动 STK，这样间接地实现 Python 驱动 STK。

本文所使用软件版本如下：

- Python：3.9.12
- STK：11.6
- MATLAB：2018b

本文所测试硬件版本如下：

- 系统：Win 11
- CPU：AMD Ryzen 7 6800H
- GPU：AMD Radeon(TM) Graphics

# 安装和连接

[STK、MATLAB、SQL的连接和使用](https://luwin1127.github.io/2021-07-29/Study-STK-Matlab-SQL)里用的是 STK 11.2 和 MATLAB 2014a，于今而言版本过老，因此更换了新版本的 STK 和 MATLAB，并在本文作者硬件上做相应测试，可以跑通。

因为 STK 11.2 最高只支持 MATLAB 2016 的连接，因此本文将 STK 升级为 STK 11.6 版本。
 
**请注意：先安装 MATLAB 2018b，再安装 STK 11.6。**

假如电脑里面没有 MATLAB 2018b 而先安装 STK 11.6 的话，STK 会认为你没有安装 MATLAB，于是不会自动设置与 MATLAB 的连接。所以一定要先安装 MATLAB，再安装 STK 11.6。如果真的先安装了 STK，再安装了 MATLAB 的话，可以下载一个[MATLAB Connectors 1.011](../../assets/download/files-2023-07-10/MATLAB Connectors v1.011.zip)。坦诚地讲，但这样的安装方式我没有实际测试过，结果未知。如果有效的话，欢迎通过邮件告知我，提前谢谢你。

两个软件的安装教程如下：

MATLAB 2018b的安装教程请见[这里](../../assets/download/files-2023-07-10/MATLAB R2018b Win64 Crack.zip)。

STK的安装教程请见[这里](../../assets/download/files-2023-07-10/Install manusrcipt of STK 11.6.pdf)。

**两个软件安装好之后，打开MATLAB 2018b的“设置路径”，会看见导入3个关于STK的文件夹，分别是：**

- **`C\Program Files\AGI\STK 11\bin`；**
- **`C\Program Files\AGI\STK 11\bin\Matlab`；**
- **`C:\ProgramData\AGI\STK MATLAB`。**

**请将`C:\ProgramData\AGI\STK MATLAB`文件夹在路径中删除。经过测试发现，如若将此文件夹导入至 MATLAB 路径中，将会出现 MATLAB Command 窗口中无法正常退出的 Bug。**

同样地，如果有其他解决方法的话，欢迎通过邮件告知我，提前谢谢你。

# 使用

**将Python IDE、MATLAB 2018b 和 STK 11.6 打开**，输入如下代码。

## Python代码

下面是Python代码。

```python
import os

# 运行 .m 文件
os.system("matlab -nojvm -nosplash -r \"cd D:\opps\Desktop\;matlab_to_stk;\";quit")
```

上面的代码虽然只有两行，但十分够用，也说明了Python的简洁美丽。

`import os` 代表导入 `os` 包。

`os.system("matlab -nojvm -nosplash -r \"cd D:\opps\Desktop\;matlab_to_stk;\";quit")` 中，每一条指令都值得单独说明：

- `matlab`：调用 MATLAB 引擎；
- `-nojvm`：调用 MATLAB 命令窗口，但不显示 MATLAB 界面元素（例如工作空间、脚本界面等），可减少内存消耗；
- `-nosplash`：调用 MATLAB 时不显示版权页，这样显得清爽；
- `-r \"cd D:\opps\Desktop\;matlab_to_stk;\";`：`-r` 为执行 MATLAB 命令；`\"` 为使用双引号；`cd D:\opps\Desktop\;` 是将 MATLAB 的路径设置在 `D:\opps\Desktop\` 下，可以设置为其他路径，根据用户把 `matlab_to_stk.m` 文件放的位置不同而不同；`matlab_to_stk;` 为执行 `matlab_to_stk.m` 文件；`quit` 为程序结束后关闭 MATLAB 命令窗口。

这样，就可以成功调用 `matlab_to_stk.m` 文件了。

## MATLAB

下面是 MATLAB 的代码。

```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% 功能说明：MATLAB与STK互联
% 作者：
% 版本：v1.0
% 时间：2023/7/10
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%% 1.初始化需要的接口函数
stkInit;                                % 建立连接
remMachine = stkDefaultHost;
conid = stkOpen(remMachine);            % 得到连接句柄（用于发送指令）
% 判断场景是否存在
scen_open = stkValidScen;
if scen_open == 1
    rtn = questdlg('Close the current scenario?');
    if ~strcmp(rtn,'Yes')
        stkClose(conid);
        return;
    else
        stkUnload('/*');
    end
end

%% 2.建立场景、航天器和观测站
% 建立场景
disp('创建场景中......');
disp('-------------------------');
stkNewObj('/','Scenario','TestScenario');

% 设定场景时间
disp('设定场景时间......');
disp('-------------------------');
% 设定场景开始时间
% startDate = '27'; startMonth = 'Nov'; startYear = '2019';
% startHour = '00'; startMin = '00';     startSec = '00.000';
% strStartTime = strcat(startDate,32,startMonth,32,startYear,32,startHour,':',startMin,':',startSec);% 逗号之间的“32”代表“空格”之意
strStartTime = '27 Nov 2019 00:00:00.000';

% 设定场景结束时间
% stopDate = '27'; stopMonth = 'Nov'; stopYear = '2019';
% stopHour = '02'; stopMin = '00'; stopSec = '00.000';
% strEndTime = strcat(stopDate,32,stopMonth,32,stopYear,32,stopHour,':',stopMin,':',stopSec);% 逗号之间的“32”代表“空格”之意
strEndTime = '28 Nov 2019 02:00:00.000';
stkSetTimePeriod(strStartTime,strEndTime,'GREGUTC');
stkSetEpoch(strStartTime,'GREGUTC');
stkSyncEpoch;

% 设定场景动画开始时间
strQuteBegTime = ['"' strStartTime '"']; % 时间写入命令是得加双引号，比较刻板
rtn = stkConnect(conid,'Animate','Scenario/TestScenario',['SetValues ' strQuteBegTime ' 10 0.1']);
% 下面这个用来参考格式
% rtn = stkConnect(conid,'Animate','Scenario/TestScenario','SetValues "27 May 2019 06:14:00.000" 60 0.1');
% 设定动画时间回到起始点
rtn = stkConnect(conid,'Animate','Scenario/TestScenario','Reset');

% 建立航天器
disp('建立航天器......');
disp('-------------------------');
stkNewObj('*/','Satellite','AoXiangI');

%% 3.设定星座中各卫星的属性
%--------------------------------------------------------------%
%               注意：这里的单位都是弧度(rad)
%                     千万别用错单位！
%--------------------------------------------------------------%
disp('仿真运行中......');
disp('-------------------------');
rtn = stkConnect(conid,'Animate','Scenario/TestScenario','Start End');
d2r = pi/180;
% 卫星积分起止时间、轨道历元时间、步长
t_start=0; t_stop=2*60*60; orbitEpoch=t_start; dt=10;
tSimu = t_start+1:t_stop/dt;
% 卫星初始轨道根数
a = 6928.137*1000;  e = 0.0; i = 53.0*d2r;
w = 0*pi/180; Raan = 160*pi/180; M = 0*d2r;

% 设置卫星轨道属性
stkSetPropClassical('*/Satellite/AoXiangI','J2Perturbation','J2000',t_start,t_stop,dt,orbitEpoch,a,e,i,w,Raan,M);
% 设置卫星姿态属性(下面写的这个是错的)
Qua = [sqrt(2)/2;0;0;sqrt(2)/2];  QuaMat = repmat(Qua,1,length(tSimu));
stkSetAttitudeCBI('*/Satellite/AoXiangI','AoXiangI',tSimu,QuaMat);

tic;
global satPos satVel
satPos = zeros(t_stop/dt,3);
satVel = zeros(t_stop/dt,3);
time   = zeros(1,t_stop/dt);
k = 1;
for t = 1:dt:t_stop
    [satPos1,satVel1] = stkPosVelCBI('*/Satellite/AoXiangI',t);
    satPos(k,:) = satPos1;
    satVel(k,:) = satVel1;
    k = k+1;
end
toc;

% 保存数据至csv中
col_str = {'sat_pos_x','sat_pos_y','sat_pos_z','sat_vel_x','sat_vel_y','sat_vel_z'};
res_table = table(satPos(:,1),satPos(:,2),satPos(:,3),satVel(:,1),satVel(:,2),satVel(:,1),'VariableNames',col_str);
writetable(res_table, 'satellite_data.csv');
stkClose(conid);
```

# 结果

最后，就可以得到下图所示结果。

![图1](../../images/img-2023-07-10/img1.png)

# 容易出错的地方

## 1、设备有多个 MATLAB 

当设备上有多个 MATLAB 版本时，比如有 MATLAB 2014a 和 MATLAB 2018b，STK 11.6 与 MATLAB 2018b 连接上可以正常使用，但使用命令行调用 MATLAB 引擎会出现没有连接上 STK 的问题。因为系统路径里默认是另一个版本的 MATLAB，调用 MATLAB 引擎时默认调用另一个版本的 MATLAB，而另一个版本的 MATLAB 没有与 STK 连接，所以无法正常使用。

解决方法是设置系统的路径变量，将与 STK 连接上的 MATLAB 加入系统路径变量中。

## 2、运行 .m 文件没有反应或报错

一定记得先打开 STK 11.6，再运行 MATLAB 程序。

# 最后

欢迎通过邮箱联系我：lordofdapanji@foxmail.com

来信请注明你的身份，否则恕不回信。
