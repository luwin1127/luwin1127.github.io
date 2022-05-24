---
permalink: /:year-:month-:day/:title
layout: post
title:  "Python通过命令行调用MATLAB的简单实现"
author: "Lei Lie"
categories: Study
tag: notdouban
---

电脑系统：Windows 10

主要思路是通过 Windows 系统中的 Shell 来调用 MATLAB 运行 `.m` 文件。

参考了 MathWorks 官方文档：[从 Windows 系统提示符启动 MATLAB 程序](https://ww2.mathworks.cn/help/matlab/ref/matlabwindows.html)。

给出 MATLAB 的测试代码：

```python
% test.m
funtion res = test(a, b)
res = a + b;
dips(res);
fprintf('hello, world!')
end
```

将其命名为`test.m`，并放置于 D 盘根目录。

**首先在 Shell 中运行 `.m` 文件。**

将鼠标移动至电脑左下角【开始】菜单（就是四个小方块），单击右键，选择 `Windows PowerShell` ，即进入了 Shell 中。然后输入如下指令：

```shell
matlab -nojvm -nosplash -sd D:\ -r "test(1,2)"
```

下面对每个参数进行解释：

（[这篇文章](https://blog.csdn.net/sinat_33425327/article/details/79125745?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.pc_relevant_baidujshouduan)也有说明）

- `-nojvm`：启动命令窗口，不显示 MATLAB 正常界面上的元素，可有效减少内存。
- `-nosplash`：启动 MATLAB 时不展示版权页。
- `quit`：MATLAB 程序执行完毕后，自动关闭 MATLAB 命令行窗口，运行结果也会随之关闭。这条指令我没有加上去，但日后编程过程中会经常用。对于这条指令，它的写法一般如下

```shell
matlab -nojvm -nosplash -sd D:\ -r "test(1,2)";quit
```
记得加上分号。

运行代码之后得到结果如下图所示：

![img1]({{site.page}}/images/img-2021-07-28/img1.png)

**然后在Python中实现同样的操作**

Python  `os` 模块中的 `os.system()` 就是对在系统层面执行命令行语句，因此可以考虑使用 `os.system()` 作为调用 MATLAB 文件的函数。

代码也很简单，只有两行，如下所示。

```python
import os

os.system("matlab -nojvm -nosplash -sd D:\ -r \"test(1,2)\";quit")
```
即可实现在 Python 中通过命令行调用 MATLAB 程序。
