---
permalink: /:year-:month-:day/:title
layout: post
title:  "Python、STK的连接和使用"
author: "Lei Lie"
categories: Essay
tag: notdouban
---

# 写在前面

预备知识：

在[Python、STK、MATLAB的连接和使用](https://luwin1127.github.io/2023-07-10/Study-Connection-STK-Python)中，实现了 Python 与 MATLAB 的连接。不过需要通过 MATLAB 作为媒介才可以实现，每次调用都要先启动 MATLAB，启动多次之后，容易内存堆满，导致电脑死机。最近发现可以不通过 MATLAB 就能实现 Python 与 STK 的连接，核心是利用 COM 组件进行通信，即 Python 中的 comtypes 包。comtypes 包可以调用 Windows API 和 COM 组件，


本文所使用软件版本如下：

- Python：3.7.16
- STK：11.6
- Miniconda：4.12.0

# 环境配置

首先，安装 STK 11.6，STK的安装过程在[STK、MATLAB、SQL的连接和使用](https://luwin1127.github.io/2021-07-29/Study-STK-Matlab-SQL)里面已经详细讲过，这里不再赘述。下面用 Miniconda 安装一个新环境，用来实现与 Python 的通信。用 Miniconda 的原因是它比 Anaconda 精简，只包含基本的 Python 和 Conda，占电脑空间少。用 Miniconda 的核心目的是管理包和管理环境，不同的项目可以使用不同的环境和不同的包，不用一股脑都装在一个环境下面，方便项目开发。

打开 Miniconda 窗口，创建一个名为 `python_to_stk` 的环境

```python
conda create -n python_to_stk python=3.7.16
```

命令窗口显示如下图所示画面，代表环境安装成功。

![img1](../../images/img-2023-08-28/img1.jpg)

接下来激活安装好的环境。

```python
conda activate python_to_stk
```

安装`comtypes`包。

```python
conda install comtypes
```

也可以用下面的代码。

```python
pip install comtypes
```

两者的区别请见 https://www.zhihu.com/question/395145313/answer/2551141843。

这里我使用 `conda install` 的原因是 `pip install` 有时候网络不好，需要挂上其他网站源才能下载安装，而 `conda install` 没有这个烦恼。

安装好 `comtypes` 包后，需要运行如下代码（代码出自 https://blog.csdn.net/u011575168/article/details/86582961） ，完成首次连接。

```python
import comtypes
from comtypes.client import CreateObject
# 打开 STK 软件
stk = CreateObject("STK11.Application")
# 显示 STK 界面
stk.Visible = True
# 获取Object Model的根对象：IAgStkObjectRoot
# 此接口为Object Model中的最顶层接口，由此接口可创建场景、地面站、卫星等
root = stk.Personality2
# 创建Astrogator相关的模块：AgStkGatorLib
comtypes.client.GetModule((comtypes.GUID("{090D317C-31A7-4AF7-89CD-25FE18F4017C}") ,1,0))
```

连接成功之后，会在屏幕上显示 STK 的标题页，如果没有显示或者代码报错，则证明没有连接成功。另外，连接成功之后会会生成 `gen` 文件夹，位置在 `[Miniconda3]\envs\python_to_stk\Lib\site-packages\comtypes\gen`，如果没有出现该文件夹，也说明没有连接成功。

里面包含连接 STK 必备的函数，包括：

1. STKObjects.py
2. STKUtil.py
3. AgSTKGraphicsLib.py
4. AgSTKVgtLib.py
5. AgUiApplicationLib.py
6. AgUiCoreLib.py
7. AgStkGatorLib.py

文件夹里面还有一些其他的函数，但就我自己使用而言，上面几个函数已经完全够用了。剩余其他的函数有什么功能，留待以后用到的时候再讲解。

# 使用

完成连接之后，就可以写代码与 STK 交互，代码如下（代码出自 https://zhuanlan.zhihu.com/p/519384711）。

```python
from comtypes.client import CreateObject
from comtypes.gen import STKObjects
# 创建STK11桌面应用
stk = CreateObject("STK11.Application")
# STK11可见
stk.Visible = 1
# 获取IAgStkObjectRoot接口
root = stk.Personality2
# 查看root属性
root.NewScenario('sce_test')
# 让变量Sce获取场景句柄
Sce = root.CurrentScenario
# 创建sat0，sat1...sat7共8颗卫星
for num in range(0,8):  # 
    SatObj = Sce.Children.New(18,'sat'+str(num))
    SatIAF = SatObj.QueryInterface(STKObjects.IAgSatellite)
    #查看当前卫星轨道预报模型
    SatIAF.PropagatorType
    # 其返回值为7，表示预报模型为二体模型(ePropagatorTwoBody)
    ProIAF = SatIAF.Propagator
    # 由IAgVePropagator跳转至IAgVePropagatorTwoBody
    ProTwoBodyIAF = ProIAF.QueryInterface(STKObjects.IAgVePropagatorTwoBody)
    # 设置卫星坐标系为J2000，轨道六要素为7000km，0，num×10°，0°，0°，0°
    ProTwoBodyIAF.InitialState.Representation.AssignClassical(3,7000,0,num*10,0,0,0)
    # 传递参数
    ProTwoBodyIAF.Propagate()

# 保存Scenario2.sc文件（这里的路径可以自己设置）
root.SaveAs(r'D:\opps\Desktop\PythonProject\Scenario2.sc')
# 保存Scenario2.vdf文件
root.SaveAs(r'D:\opps\Desktop\PythonProject\Scenario2.vdf')
```

上面代码的作用是创建 8 颗卫星，并且给定每颗卫星的轨道六要素，轨道动力学模型为二体模型。Python 代码运行完毕之后，在文件夹里会出现 STK 的卫星模型文件和场景文件，如下图所示。

![img2](../../images/img-2023-08-28/img2.jpg)

打开 `Scenario2.sc` 文件，可以在 STK 中看见如下图所示的场景，证明 Python 与 STK 连接并运行成功。

![img3](../../images/img-2023-08-28/img3.jpg)

这次实现了简单的 Python 与 STK 交互，但每个函数怎么用的问题有待进一步研究，之后如果要深入研究的话，我会继续写文用以记录。

# 最后

欢迎通过邮箱联系我：lordofdapanji@foxmail.com

来信请注明你的身份，否则恕不回信。
