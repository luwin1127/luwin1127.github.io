---
permalink: /:year-:month-:day/:title
layout: post
title:  "STK、MATLAB、SQL的连接和使用"
author: "Lei Lie"
categories: Study
tag: notdouban
---


这里要实现的功能是，通过把 MATLAB 和 STK 连接起来，再把 MATLAB 和 SQL 连接起来。通过 MATLAB 将初始参数传给 STK，STK 生成仿真数据，随后把数据传回 MATLAB，MATLAB 再把数据传递至 SQL 中保存下来。

# 安装和连接

首先，按照“MATLAB -> STK -> SQL”的顺序安装软件。

这篇文章里三种软件的版本分别是

- MATLAB 2014a
- STK 11.2
- SQL 2019

## （1）MATLAB 2014a

破解教程：[MATLAB 2014a 的破解教程](https://jingyan.baidu.com/article/ce4366491af8373773afd3b6.html)

## （2）STK 11.2
进入安装界面，把这里的复选框全部勾选（因为不知道以后项目需要用到什么，所以全装），就算不想全部安装，最起码也要勾上“Matlab Connectors”，这个 connectors 是和 MATLAB 连接起来必须的软件。点击“Install”。

![img1]({{site.page}}/images/img-2021-07-29/img1.png)

随后，一路点击 next 和 agree，到下面这一步，可以自己选择要安装的位置，建议不要安装在C盘，安装在其他盘也是可以正常使用的。

![img2]({{site.page}}/images/img-2021-07-29/img2.png)

安装好之后，打开下载的包里面的 crack 文件夹，找到 STK 11，把这个文件夹复制到安装软件的位置。

![img3]({{site.page}}/images/img-2021-07-29/img3.png)

![img4]({{site.page}}/images/img-2021-07-29/img4.png)

放在这里，替换一下，就可以了。

安装好之后STK 11.2之后，再打开下载的包里面的 Matlab_Connectors ，点击 setup.exe，安装好 MATLAB 2014a 和 STK 11.2 的连接器。

![img5]({{site.page}}/images/img-2021-07-29/img5.png)

STK 11.2 支持的 MATLAB 有如下这些版本：

-  R2008a (7.6)
-  R2010a (7.10)
-  R2010b (7.11)
-  R2011a (7.12)
-  R2011b (7.13)
-  R2012a (7.14)
-  R2012b (8.0)
-  R2013a (8.1)
-  R2013b (8.2)
-  R2014a (8.3)
-  R2014b (8.4)
-  R2015a (8.5)
-  R2015b (8.6)
-  R2016a (9.0)
-  R2016b (9.1)

到这里，MATLAB 2014a 与 STK 11.2 的连接已经搭建起来了。

然后，打开 MATLAB 2014a，把下面两个路径导入至matlab路径中：

- `C:\ProgramData\AGI\STK MATLAB`，需要注意的是，C盘中`ProgramData`文件夹是隐藏的，如需查看，需要点击“查看”中的“隐藏的项目”。
-  `D:\Program Files (x86)\AGI\STK 11\bin\Matlab`，这里是我在自己电脑的STK 11.2的安装路径，这个文件里面的`matlab`是 AGI 公司写好的 MATLAB 程序，设置好路径后，就可以直接使用这些 `.m` 文件。就和 MATLAB 的 toolbox 一样。

**测试**

打开 MATLAB 2014a，输入代码

```python
stkInit; 
```

在命令行中出现

![img6]({{site.page}}/images/img-2021-07-29/img6.png)

证明MATLAB 2014a 和 STK 11.2 的连接已经完成。

## （3）SQL Server

去[官网](https://www.microsoft.com/zh-cn/sql-server/sql-server-downloads)下载 SQL Server 2019 Express，必须联网才能下载、安装。 安装好 SQL Server 2019 Express 后，再安装 SSMS （安装完毕之后会弹出网页，直接去网页安装就可以了）。

MATLAB 2014a 和 SQL 2019的安装，看[以前写过的文章](https://blog.csdn.net/Ruins_LEE/article/details/114189099)。


# 使用
打开 MATLAB 2014a、STK 11.2 和 SQL 2019，在 MATLAB 2014a中新建脚本文件，输入如下代码，如果报错，证明一切正常。但需要注意的是，得先在 SQL Server 中建立好与代码对应的数据库和表名、列名。

这里 SQL Server 中数据库的名字为 `mydb`，表名为 `r_t0` 和 `v_t0` 。其中 `r_t0` 包含 `r_t0_x`、`r_t0_y`、和`r_t0_z`三个列名； `v_t0` 包含 `v_t0_x`、`v_t0_y`、和`v_t0_z`三个列名

```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%  功能说明： Matlab与STK互联
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%% 1.初始化需要的接口函数
stkInit;                                % 建立连接
remMachine = stkDefaultHost;
conid = stkOpen(remMachine);            % 得到连接句柄（用于发送指令）
% 判断场景是否存在
scen_open = stkValidScen;
if scen_open == 1
    rtn = questdlg('Close the current scenario?')
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
h = waitbar(1,'创建场景中......','name','Matlab/STK仿真演示平台');
stkNewObj('/','Scenario','TestScenario');
pause(1);
close(h);

% 设定场景时间
disp('设定场景时间......');
disp('-------------------------');
h = waitbar(1,'设定场景时间......','name','Matlab/STK仿真演示平台');
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
pause(1);
close(h);

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

h = waitbar(1,'建立航天器......','name','Matlab/STK仿真演示平台');
pause(1);
close(h);

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
h = waitbar(0,'仿真运行中......','name','Matlab/STK仿真演示平台');
global satPos satVel
satPos = zeros(t_stop/dt,3);
satVel = zeros(t_stop/dt,3);
time   = zeros(1,t_stop/dt);
k = 1;
for t = 1:dt:t_stop
    [satPos1,satVel1] = stkPosVelCBI('*/Satellite/AoXiangI',t);
    satPos(k,:) = satPos1;
    satVel(k,:) = satVel1;
    waitbar(k/(t_stop/dt));
    k = k+1;
end
pause(0.5);
close(h);
toc;
msgbox('仿真结束，请查看结果！','Matlab/STK仿真演示平台');
stkClose(conid);

%% 
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%  功能说明： Matlab与STK互联
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
% 建立MATLAB与SQL的连接
conn = database('mydb','','');

% 删除数据库中原来的数据
curs_r = exec(conn,'delete from r_t0');
curs_v = exec(conn,'delete from v_t0');

% 向数据库中存储数据
satPara = zeros(k-1,6);
satPara(:,1:3) = satPos;
satPara(:,4:6) = satVel;
% 得到矩阵的行列
[row,col] = size(satPara);
% 插入数据
satParaCell = cell(row,col);
for i = 1:row
    for j = 1:col
        satParaCell(i,j) = num2cell(satPara(i,j));
    end
end
colname_r = {'r_t0_x','r_t0_y','r_t0_z'};
insert(conn,'r_t0',colname_r,satParaCell(:,1:3));
colname_v = {'v_t0_x','v_t0_y','v_t0_z'};
insert(conn,'v_t0',colname_v,satParaCell(:,4:6));

% Again
curs_r = exec(conn,'delete from r_t0');
curs_v = exec(conn,'delete from v_t0');
colname_r = {'r_t0_x','r_t0_y','r_t0_z'};
insert(conn,'r_t0',colname_r,satParaCell(:,1:3));
colname_v = {'v_t0_x','v_t0_y','v_t0_z'};
insert(conn,'v_t0',colname_v,satParaCell(:,4:6));

% 关闭连接
close(curs_r);
close(curs_v);
close(conn);
```

MATLAB 代码执行完毕后，可以在 SQL Server中查看数据是否写入至数据库中，输入如下代码。

```sql
SELECT * FROM r_t0
SELECT * FROM v_t0
```

得到下图结果，证明数据正常写入了数据库中。

![img7]({{site.page}}/images/img-2021-07-29/img7.png)