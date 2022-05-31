---
permalink: /:year-:month-:day/:title
layout: post
title:  "PyQt5+Anaconda+PyCharm安装和配置"
author: "Lei Lie"
categories: Study
tag: notdouban
---

说明：

PyQt 5用于设计Ui。

Anaconda用于配置环境。

PyCharm用于写代码。

# Anaconda配置环境

首先说Anaconda如何配置环境。打开Anaconda Prompt (Anaconda3)，创建新环境。

## 创建新环境

![图1-命令软件]({{site.page}}/images/img-2021-04-29/图1-命令软件.webp)

输入如下代码

```shell
conda create -n 环境名字 python==python版本
```

我这里写的是

```shell
conda create -n pyqt python==3.8
```

建立好新的环境之后，开始安装pyqt5需要的包。

注意，这些代码都要在`pyqt`（上面创建好的环境里面写）。如果发现你的环境不在自己创建好的环境里面，在Anaconda Prompt (Anaconda3)中输入如下代码

```shel
activate pyqt(或者你自己创建好的环境名)
```

如果在界面最前面显示`pyqt`（或者你自己创建好的环境名），例如这样

![图2-激活环境]({{site.page}}/images/img-2021-04-29/图2-激活环境.webp)

就说明现在电脑处于这个环境中了，就可以开始安装pyqt5需要的包了。

## 安装PyQt 5

输入代码安装PyQt 5。

```python
pip install PyQt5
```

 如果下载速度慢，可以加上豆瓣源或者清华源。

这个是加上豆瓣源的代码。

```python
pip install PyQt5 -i https://pypi.douban.com/simple
```

这个是加上清华源的代码。

```python
pip install PyQt5 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

[这个网址](https://blog.csdn.net/tanzuozhev/article/details/76022189)说豆瓣源里面很多包不全，建议使用清华大学的，姑且一听。因为我是忠实豆瓣用户，所以还是信仰加成喜欢使用豆瓣源。

## 安装PyQt5 Tools

再安装经常需要使用到的Qt工具，代码为

```python
pip install PyQt5-tools
```

也可以加上豆瓣源或者清华源。

豆瓣源的代码为

```python
pip install PyQt5-tools -i https://pypi.douban.com/simple
```

清华源的代码为

```python
pip install PyQt5-tools -i https://pypi.tuna.tsinghua.edu.cn/simple
```

这样，目前所需的python包已经安装好了。

# PyCharm中配置环境

打开PyCharm，新建项目。

## 导入环境

建好项目之后，按顺序点击“文件->设置->项目解释器->右上角的‘齿轮’->全部显示->（选择刚才在anaconda创建好的环境）->确定”，如下图所示。

![图3-PyCharm配置环境]({{site.page}}/images/img-2021-04-29/图3-PyCharm配置环境.webp)

这样，这个项目的环境就配置为用anaconda创建好的环境了。

# 测试

新建一个.py文件，输入代码

```python
impory pyqt5
```

如果不出现错误信息，则说明一切正常，pyqt5安装成功。

# PyQt5 放进 PyCharm中

按顺序点击“文件->设置->工具->外部工具->箭头指向的'+'号”，如下图所示。

![外部工具]({{site.page}}/images/img-2021-04-29/图4-外部工具.webp)

## QtDesigner的设置

将QtDesigner导入进PyCharm中。上面点击“+”后，会弹出来一个窗口，按下图所示的参数设置。

![图5-QtDesigner设置]({{site.page}}/images/img-2021-04-29/图5-QtDesigner设置.webp)

名称：QtDesigner

程序：`D:\Anaconda3\envs\pyqt\Lib\site-packages\qt5_applications\Qt\bin\designer.exe`

参数：（不设置）因为我发现设置了之后进入Qt Designer后老给我弹窗口，不设置参数反而不弹窗口了。

工作目录：`$FileDir$`

文件`designer.exe`的位置是我自己在创建的anaconda环境中找到的，和别人博客文章里面的可能不太一致。各位在导入自己的`designer.exe`时候，要注意看看自己的文件`designer.exe`在哪里，不一定和我的文件位置一样。这个位置仅作参考。我是在window文件搜索框里面搜索了一下`designer.exe`，才找到这个文件的。

## PyUIC的设置

PyUIC的话，按下图设置。它的功能是将QtDesigner生成的.ui文件转换成.py文件。

![图6-PyUIC设置]({{site.page}}/images/img-2021-04-29/图6-PyUIC设置.webp)

名称：PyUIC

程序：`D:\Anaconda3\envs\pyqt\Scripts\pyuic5.exe`

*程序路径仅供参考。*

参数：`$FileName$ -o $FileNameWithoutExtension$.py`

工作目录：`$FileDir$`

## PyRCC的设置

按照下图设置即可。

![图7-PyRCC设置]({{site.page}}/images/img-2021-04-29/图7-PyRCC设置.webp)

名称：PyUIC

程序：`D:\Anaconda3\envs\pyqt\Scripts\pyrcc5.exe`

*程序路径仅供参考。*

参数：`$FileName$ -o $FileNameWithoutExtension$_rc.py`

工作目录：`$FileDir$`

# 一个简单的使用实例

## 使用QtDesigner设计ui

打开PyCharm，点击上方菜单栏的“工具”，按顺序点击“External Tools”和“QtDesigner”，即可打开QtDesigner。

如下图所示。

![图8-打开QtDesigner]({{site.page}}/images/img-2021-04-29/图8-打开QtDesigner.webp)

打开之后，选择“Main Window”，再点击“创建”，就可创建一个新的窗口，大小为“默认大小”，把这个窗口命名为MyFirstWindow.ui。如下两张图所示。

![图9-QtDesigner]({{site.page}}/images/img-2021-04-29/图9-QtDesigner.webp)

![图10-新建窗口]({{site.page}}/images/img-2021-04-29/图10-新建窗口.webp)

下面涉及两个知识点：

- 一个是导入qrc资源文件
- 另一个是qss样式表的简单设置

### .qrc资源的导入

首先讲怎么导入qrc资源。右下角有个资源浏览器，先点“资源浏览器”，再点这个窗口左上角的铅笔。如下图所示。

![图11-资源管理器]({{site.page}}/images/img-2021-04-29/图11-资源管理器.webp)

出来资源浏览器后，点击中间的“打开”。这里我已经建好了一个.qrc文件，所以直接选中再点确定就好了，如下图所示。想要新建.qrc的话，点击左边的新建即可。

![图12-导入qrc]({{site.page}}/images/img-2021-04-29/图12-导入qrc.webp)

导入成功。

![图13-导入成功]({{site.page}}/images/img-2021-04-29/图13-导入成功.webp)

.qrc文件导入成功后，要把里面的图片显示在.ui中，需要使用到`QLabel`部件。

从左边选择“Label”，再在最右边找到“QLabel”下面的“pixmap”，点击右边的三个小圆点，弹出来图中央的窗口，选择刚才导入的.qrc资源文件，选择一张图片，即可将该图片导入至.ui中。

![图14-给QLabel添加图片]({{site.page}}/images/img-2021-04-29/图14-给QLabel添加图片.webp)

![图15-添加成功]({{site.page}}/images/img-2021-04-29/图15-添加成功.webp)

### qss的设置

从左边选择“Push Button”，将按钮拖入窗口中，右键单击按钮，选择“修改样式表”，弹出来一个编辑样式表的框，如下图所示。

![图16-添加按钮及其qss样式]({{site.page}}/images/img-2021-04-29/图16-添加按钮及其qss样式.webp)

在框内输入如下代码。

```css
QPushButton {
    background-color:qlineargradient(spread:pad, x1:0, y1:0, x2:1, y2:0, stop:0 #e66465, stop:1 #9198e5);
	color:rgb(227, 227, 227);
    padding: 4px;
    min-width: 65px;
    min-height: 12px;
	border-bottom-left-radius:20px;
	border-top-right-radius:20px;
	font-size: 16px;
}
 
QPushButton:hover {
    background-color: qlineargradient(spread:pad, x1:0, y1:0, x2:1, y2:0, stop:0 #FF6465, stop:1 #9198FF);
	color:white;
}
QPushButton:pressed{
	background-color: rgb(65, 65, 65);
	color:white;
}
```

这里不展开讲解这些代码的作用了。qss的语法和css差不多，可以在[菜鸟教程](https://www.runoob.com/css3/css3-tutorial.html)里面看看css的教程。这里我实现了三种功能。

（1）改变按钮的原来样式，效果如下图所示。

![图17-效果]({{site.page}}/images/img-2021-04-29/图17-效果.webp)

（2）鼠标悬置于按钮上时，改变按钮的背景色和文字的颜色，效果如下图所示，因为颜色比较相近，区别不大，看不太出来。

![图18-鼠标悬置效果]({{site.page}}/images/img-2021-04-29/图18-鼠标悬置效果.webp)

（3）点击按钮时，改变按钮的背景色和文字的颜色，效果如下图所示。

![图19-鼠标点击效果]({{site.page}}/images/img-2021-04-29/图19-鼠标点击效果.webp)

到这里，一个简单的ui界面已经设计完毕。下面讲解.ui文件和.qrc文件如何转成python可用的.py文件。

## 将.ui转换为.py

回到PyCharm，右键单击刚才设计好的`MyFirstWindow.ui`，点击“External Tools”，再点击“PyUIC”，即可将.ui转换为.py，如下图所示。

![图20-ui转换为py]({{site.page}}/images/img-2021-04-29/图20-ui转换为py.webp)

## 将.qrc转换为.py

右键单击`apprcc_rc`，点击“External Tools”，再点击“PyRCC”，即可将.qrc转换为.py，如下图所示。

![图21-qrc转换为py]({{site.page}}/images/img-2021-04-29/图21-qrc转换为py.webp)

`apprcc_rc.py`是由资源文件`apprcc.qrc`转换而来，在pycharm中，右键单击资源文件`apprcc.qrc`，点击External Tools，再点击PyRCC，就会将资源文件转换为.py文件了。因为Qt Designer导入资源文件是默认加`_rc`的，所以生成的.py文件名字最后也会有`_rc`。

需要注意的是，如果.ui文件中导入了.qrc文件，就必须要将该.qrc文件转换成.py，不然没法运行。因为在从`MyFirstWindow.ui`生成的`MyFirstWindow.py`中，有一行代码是

```python
import apprcc_rc
```

也就是导入资源文件。不导入这个资源文件的话，程序没办法运行。要不就是删除这行代码，那么.ui中所有使用到该.qrc文件生成的样式，都无法正常显示。

**所以这个`import`指令还是很重要的。**

.ui和.qrc转换为.qrc的结果如下图所示。

![图22-转换结果]({{site.page}}/images/img-2021-04-29/图22-转换结果.webp)

## 在PyCharm中运行程序

在PyCharm中新建一个.py文件，命名为`CallMyFirstWindow`，输入如下代码：

```python
"""
#-------------------------------------------------------------------#
#                     Li Lingwei's Python Code                      #     
#-------------------------------------------------------------------#
#                                                                   #
#                   @Project Name : PyQt 5                          #
#                   @File Name    : CallMyFirstWindow.py            #
#                   @Programmer   : Li Lingwei                      #
#                   @Start Date   : 2021/4/29                       #
#                   @Last Update  : 2021/4/29                       #
#-------------------------------------------------------------------#
# Function:                                                         #
#                  显示MyFirstWindow.py的窗口                        #
#-------------------------------------------------------------------#
"""
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow
from MyFirstWindow import Ui_MainWindow


class MainWindow(QMainWindow, Ui_MainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.setupUi(self)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    myWin = MainWindow()
    myWin.show()
    sys.exit(app.exec_())
```

点击运行，可以正常显示在Qt Designer中设计好的窗口，效果如下图所示。

![图23-运行程序结果]({{site.page}}/images/img-2021-04-29/图23-运行程序结果.webp)

至此， PyQt5+Anaconda+PyCharm安装、配置和使用全部讲解完毕。