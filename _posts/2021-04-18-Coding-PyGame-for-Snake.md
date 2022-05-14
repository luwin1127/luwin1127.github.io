---
permalink: /:year-:month-:day/:title
layout: post
title:  "用PyGame实现贪吃蛇游戏"
author: "Lei Lie"
categories: Coding
tag: notdouban
---
# 准备工作

## 配置环境

使用`Anaconda`配置新环境。通用代码为：

```python
create -n 新建环境名字 python == 3.X
```

我写的代码是

```python
create -n GUIexmaple python == 3.6
```

意义为，创建一个名为`GUIexample`的环境，python版本为`3.6`。

## 安装PyGame

进入创建好的环境，代码为

```python
activate GUIexample
```

安装PyGame，代码为

```python
pip install pygame
```

如何测试？

进入Pycharm后，使用`import pygame`测试，如果不报错的话，说明pygame安装成功。

# 开发游戏

这里主要以代码的讲解为主，防止自己以后忘记。

## 导入模块

```python
import pygame
from snakeClass import Point
from snakeFunction import gen_food, rect
from sys import exit
import random
```

`snakeClass`和`snakeFunction`是我自己写的类对象和函数，先不用管这两个模块。

## 建立窗口

```python
pygame.init()                                # 初始化
width = 800                        			 # 窗口宽度
hight = 600                                  # 窗口高度
score = 0                                    # 分数记录
window = pygame.display.set_mode((width, hight))  # 初始化一个准备显示的窗口或屏幕
bgColor = (191, 224, 206)                    # 游戏背景颜色
pygame.display.set_caption('贪吃蛇大作战')     # 设置当前窗口标题
```

`.set_mode()`后会得到一个准备要显示的窗口或屏幕，已经给出了宽为800，高为600。

`.set_caption()`设置窗口左上角的标题。

如果此时点击运行的话，屏幕上会生成一个窗口，但是闪一下就没有了。因此需要设置一个循环，让窗口一直在屏幕上显示，保持刷新。并且通过事件监听让程序获得正常退出死循环的功能。全套代码如下：

```python
pygame.init()                                # 初始化
width = 800                        			 # 窗口宽度
hight = 600                                  # 窗口高度
score = 0                                    # 分数记录
window = pygame.display.set_mode((width, hight))  # 初始化一个准备显示的窗口或屏幕
bgColor = (191, 224, 206)                    # 游戏背景颜色
pygame.display.set_caption('贪吃蛇大作战')     # 设置当前窗口标题

gameFlag = True                             # 控制游戏进行的标志
frameRate = 12                               # 控制游戏的帧率
clock = pygame.time.Clock()         # 创建一个对象来帮助跟踪时间
while gameFlag:
    clock.tick(frameRate)                    # 更新clock对象
    for event in pygame.event.get():         # 从队列中获取事件
        if event.type == pygame.QUIT:
            gameFlag = False
```

得到如下图所示的窗口。

![图1-初始窗口]({{site.page}}/images/img-2021-04-18/图1-初始窗口.png)

知道了如何设置窗口后，下面我们需要构造一个登录界面，这个登录界面包括“开始游戏”、“退出游戏”、“关于作者”三个按钮，“贪吃蛇大作战”一个标题，以及一个图标。

## 设置登录窗口

```python
"""-----------------设置登录窗口的相关参数-----------------"""
startSur = pygame.Surface(window.get_size()) # 生成登录窗口画布
startSur = startSur.convert()
startSurColor = (140, 180, 200)
startSur.fill(startSurColor)
```

## 导入要加载的图片

```python
# 加载各个素材图片 并且赋予变量名
title = pygame.image.load("./Images/title.png")
title.convert()
titleWidth, titleHight = title.get_size()

i0 = pygame.image.load("./Images/snake.png")
i0.convert()

i1 = pygame.image.load("./Images/start_black.png")
i1.convert()
i1Width,i1Hight = i1.get_size()
i11 = pygame.image.load("./Images/start_red.png")
i11.convert()

i2 = pygame.image.load("./Images/end_black.png")
i2.convert()
i2Width,i2Hight = i2.get_size()
i21 = pygame.image.load("./Images/end_red.png")
i21.convert()

i3 = pygame.image.load('./Images/author_black.png')
i3.convert()
i3Width,i3Hight = i3.get_size()
i31 = pygame.image.load('./Images/author_red.png')
i31.convert()
```

这里我用`.convert()`指令的原因是我生成的按钮图片是透明的，所以不需要再使用`.convert_alpha()`转换为透明的了。`.get_size()`函数是为了得到图片的**宽**和**高**（注意顺序）。

还有需要注意的是图片的路径问题，`./`的含义就是相对路径，在主函数的路径下面。

## 构建登录窗口

```python
while loginFlag:
    clock.tick(60)
    buttons = pygame.mouse.get_pressed(3)
    x1, y1 = pygame.mouse.get_pos()
    if x1 >= (width-i1Width)/2 and x1 <= (width+i1Width)/2 and y1 >= (hight-i1Hight)/2+hChange and y1 <= (hight + i1Hight)/2-10+hChange:
        startSur.blit(i11, ((width-i1Width) / 2, (hight-i1Hight) / 2 + hChange))
        if buttons[0]:
            loginFlag = False
            
    elif x1 >= (width-i2Width)/2 and x1 <= (width+i2Width)/2 and y1 >= (hight-i2Hight)/2+i1Hight+10+hChange and y1 <= (hight + i2Hight)/2+i1Hight+hChange:
        startSur.blit(i21, ((width-i2Width) / 2, (hight-i2Hight) / 2 + i1Hight + hItv + hChange))
        if buttons[0]:
            print("游戏退出...")
            pygame.quit()
            exit()

    elif x1 >= (width-i3Width)/2 and x1 <= (width+i3Width)/2 and y1 >= (hight-i3Hight)/2+2*(i1Hight+10)+hChange and y1 <= (hight + i3Hight)/2+2*(i1Hight + 10)+hChange:
        startSur.blit(i31, ((width-i3Width) / 2, (hight-i3Hight) / 2 + 2 * (i1Hight+hItv) + hChange))
    else:
        i0 = pygame.transform.scale(i0, (550,550))
        startSur.blit(title, ((width-titleWidth) / 2, 160))
        startSur.blit(i0, (120, 30))
        startSur.blit(i1, ((width-i1Width) / 2, (hight-i1Hight) / 2 + hChange))
        startSur.blit(i2, ((width-i2Width) / 2, (hight-i2Hight) / 2 + i1Hight + hItv + hChange))
        startSur.blit(i3, ((width-i3Width) / 2, (hight-i3Hight) / 2 + 2 * (i1Hight+hItv) + hChange))

    window.blit(startSur, (0, 0))
    pygame.display.flip()            # 更新屏幕

    # 监听事件
    for event in pygame.event.get():
        # 判断事件类型是否是退出事件
        if event.type == pygame.QUIT:
            print("游戏退出...")
            # quit 卸载所有的模块
            pygame.quit()
            # exit() 直接终止当前正在执行的程序
            exit()
```

`.blit()`实现的功能就是把一张图（例如图A）粘贴到`surface`对象上面去（这里`surface`对象是`startbur`）。

`.flip()`起刷新屏幕的作用。

得到如下图所示的登录界面。

![图2-登录窗口]({{site.page}}/images/img-2021-04-18/图2-登录窗口.png)

需要注意的是，右下角的版本号上面的代码里面没有写入，所以在复制这段代码的时候不出现右下角的版本号是正常的。

登录界面写完了，下面是本文最重要的部分，写贪吃蛇程序。

## 贪吃蛇程序

### 类对象和函数

写`Point`类对象，得到每一个点的x、y坐标。后面画图也以此为基准。

```python
class Point():
    row = 0
    col = 0
    def __init__(self,row,col):
        self.row = row
        self.col = col
    def copy(self):
        return Point(row=self.row,col=self.col)
```

### 食物生成函数

再写生成食物的函数。

```python
# 生成食物并且不让食物生成在蛇的身体里面
def gen_food(Row, Col, head, snake):
    while True:
        position = Point(row=random.randint(0, Row - 1),
                         col=random.randint(0, Col - 1))
        is_coll = False
        if head.row == position.row and head.col == position.col:
            is_coll = True
        for body in snake:
            if body.row == position.row and body.col == position.col:
                is_coll = True
                break
        if not is_coll:
            break
    return position
```

### 画图

画图的函数。

```python
def rect(window, point, color, width, hight, Row, Col):
    # 定位、画图需要left和top
    left = point.col * width / Col
    top = point.row * hight / Row
    # 将方块涂色
    pygame.draw.rect(window, color, (left, top, width / Col, hight / Row))
```

### 蛇的移动

蛇的移动功能。

```python
if direct == 'left':
    head.col -= 1
if direct == 'right':
    head.col += 1
if direct == 'top':
    head.row -= 1
if direct == 'bottom':
    head.row += 1
```

### 死亡判断条件

判断死亡与否。

```python
dead = False                                  # 是否死亡的标志
if head.col < 0 or head.row < 0 or head.col >= Col or head.row >= Row:  # 判断是否出界
    dead = True
for body in snake: # 判断是否触碰蛇身
    if head.col == body.col and head.row == body.row:
            dead = True
            break
if dead:
    print('Your score is ', score)
    print('Game over!')
    gameMusic.stop()
    gameFlag = False
```

### 蛇的跟随移动

蛇的跟随移动功能。

```python
snake = [                                                       # 蛇身坐标，起始2节蛇身+1节蛇头
    Point(row=head.row, col=head.col + 1),
    Point(row=head.row, col=head.col + 2),
    Point(row=head.row, col=head.col + 3)
]
```

把蛇头和所有蛇身坐标以元组的形式放入一个列表，每次移动头的位置会放对应的方向移动1格（把原来的头使用`insert()`插入到前面），并且尾部会消失那么此时使用`pop()`函数实现，注意`pop()`中的参数不填，为缺省值。因为头往前移动了一格，其他不需要变动,只变动头和尾就可以。

代码全览如下。

```python
snake.insert(0, head.copy())
if not eat_yellow:
    snake.pop()               # 删除列表最末端的元素
    # 进行移动
    if direct == 'left':
        head.col -= 1
    if direct == 'right':
        head.col += 1
    if direct == 'top':
        head.row -= 1
    if direct == 'bottom':
        head.row += 1
    dead = False  
```

这里的`cpoy()`是类对象`Point` 写的方法。其实就是创建一个新对象，用于复制自身。

```python
def copy(self):
    return Point(row=self.row, col=self.col)
```

### 是否吃到食物

判断蛇是否吃到食物。

```python
eat_yellow = (head.row == snakeFood.row and head.col == snakeFood.col)             # 判断是否吃下黄色食物
```

吃到食物之后的处理。

```python
if eat_yellow:
    snakeFood = Point(row=random.randint(0, Row-1), col=random.randint(0, Col-1))
    score += 3
    frameRate += 0.02
    eatMusic.play()
```

这里判断一下，如果蛇吃到食物，则增加一格蛇身。食物由重新随机出现在窗口中。

我设置了两类食物，红色食物和黄色食物。红色食物吃到之后加5分，帧率加快0.04（也就是游戏速度），黄色食物吃到之后加3分，帧率加快0.02。

### 处理游戏失败

```python
if head.col < 0 or head.row < 0 or head.col >= Col or head.row >= Row:  # 判断是否出界
    dead = True
    for body in snake:                                      # 判断是否触碰蛇身
        if head.col == body.col and head.row == body.row:
            dead = True
            break
   if dead:
    print('Your score is ', score)
    print('Game over!')
    gameMusic.stop()
    gameFlag = False
```

运行图片如下。

![图3-游戏界面]({{site.page}}/images/img-2021-04-18/图3-游戏界面.png)

## 游戏失败提示窗口

另外，我添加了一个游戏失败窗口提示界面。

代码如下：

```python
width = 300
hight = 200
window = pygame.display.set_mode((width, hight))                # 初始化一个准备显示的窗口或屏幕
bgColor = (0, 0, 0)                                             # 游戏背景颜色
pygame.display.set_caption('贪吃蛇大作战')                        # 设置当前窗口标题
failMusic.play()

while exitFlag:
    clock.tick(60)
    textFail = font.render('You Failed !', True, (255,0,0), bgColor)
    textFailWidth, textFailHight = textFail.get_size()                      # 得到文字位置
    window.blit(textFail, ((width-textFailWidth)/2, (hight-textFailHight)/2-40))                 # 在窗口右上角显示文字

    textScore = 'Your Score is ' + str(score)                          # 通过字体创建文字对象
    text = font.render(textScore, True, (255, 0, 0), bgColor)
    textWidth, textHight = text.get_size()                      # 得到文字位置
    window.blit(text, ((width-textWidth)/2, (hight-textFailHight)/2+textHight-30))             # 在窗口右上角显示文字

    pygame.display.flip()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()
```

效果如下图所示。

![图4-游戏失败]({{site.page}}/images/img-2021-04-18/图4-游戏失败.png)

---

## 其他的附加功能

考虑了添加音乐的部分。

使用了`pygame.mixer.Sound()`创建一个`Sound`对象。

`pygame.mixer.Sound.Play()`用于播放音乐。

`pygame.mixer.Sound.Play()`用于停止音乐。

`pygame.mixer.Sound.set_volume()`用于控制音乐音量。

考虑了计分和文字显示部分。

```
font = pygame.font.SysFont("Times New Roman", 30)
```

设置了显示文本的字体，主要要用`SysFont()`使用的是自己电脑里自带的字体格式。

`font.render()`用于渲染字体。

# 源代码

主函数snakeMain_plus.py

```python
"""
#-------------------------------------------------------------------#
#                     Lei Lie's Python Code                      #     
#-------------------------------------------------------------------#
#                                                                   #
#                   @Project Name : PyGameLearn                     #
#                   @File Name    : snakeMain_plus.py               #
#                   @Programmer   : Lei Lie                      #
#                   @Start Date   : 2021/4/17                       #
#                   @Last Update  : 2021/4/17                       #
#-------------------------------------------------------------------#
# Function:                                                         #
#                                                                   #
#-------------------------------------------------------------------#
"""

import pygame
from snakeClass import Point
from snakeFunction import gen_food, rect
from sys import exit
import random

version = '0.5.1'                                               # 游戏版本
pygame.init()                                                   # 初始化
width = 800                                                     # 窗口宽度
hight = 600                                                     # 窗口高度
score = 0                                                       # 分数记录

window = pygame.display.set_mode((width, hight))                # 初始化一个准备显示的窗口或屏幕
bgColor = (191, 224, 206)                                       # 游戏背景颜色
pygame.display.set_caption('贪吃蛇大作战')                        # 设置当前窗口标题

"""-----------------设置登录窗口的相关参数-----------------"""
startSur = pygame.Surface(window.get_size())                    # 生成登录窗口画布
startSur = startSur.convert()                                   # 使用convert可以转换格式，提高blit的速度
                                                                # 多说一嘴，convert_alpha()既可以提高blit速度，也可以实现透明的效果
startSurColor = (140, 180, 200)
startSur.fill(startSurColor)

# 加载各个素材图片 并且赋予变量名
title = pygame.image.load("./Images/title.png")
title.convert()
titleWidth, titleHight = title.get_size()

i0 = pygame.image.load("./Images/snake.png")
i0.convert()

i1 = pygame.image.load("./Images/start_black.png")
i1.convert()
i1Width,i1Hight = i1.get_size()
i11 = pygame.image.load("./Images/start_red.png")
i11.convert()

i2 = pygame.image.load("./Images/end_black.png")
i2.convert()
i2Width,i2Hight = i2.get_size()
i21 = pygame.image.load("./Images/end_red.png")
i21.convert()

i3 = pygame.image.load('./Images/author_black.png')
i3.convert()
i3Width,i3Hight = i3.get_size()
i31 = pygame.image.load('./Images/author_red.png')
i31.convert()

# 背景音乐
menuMusic = pygame.mixer.Sound('./Sounds/Menu.mp3')
menuMusic.set_volume(0.5)
gameMusic = pygame.mixer.Sound('./Sounds/Asgore.mp3')
gameMusic.set_volume(0.5)
eatMusic = pygame.mixer.Sound('./Sounds/Tick.mp3')
eatMusic.set_volume(0.5)
failMusic = pygame.mixer.Sound('./Sounds/Failed.mp3')
failMusic.set_volume(0.5)

hChange = 30                                                 # 控制图片体位置的变量
hItv = 10                                                    # 控制图片间隔的变量
"""----------------------参数设置完毕----------------------"""

"""-----------------设置贪吃蛇游戏的相关参数-----------------"""
Row = 30                                                        # 定义蛇头坐标
Col = 40
head = Point(row=int(Row / 2), col=int(Col / 2))                # 蛇头坐标定在中间
snake = [                                                       # 蛇身坐标，起始2节蛇身+1节蛇头
    Point(row=head.row, col=head.col + 1),
    Point(row=head.row, col=head.col + 2),
    Point(row=head.row, col=head.col + 3)
]
snakeFood = gen_food(Row, Col, head, snake)                     # 食物坐标
snakeFood_red = gen_food(Row, Col, head, snake)                 # 加更多分的食物的坐标
head_color = (255, 255, 255)                                    # 蛇头颜色可以自定义
snake_color = (195, 195, 195)                                   # 定义蛇身颜色

snakeFood_color = (255, 255, 123)                               # 食物颜色
snakeFood_red_color = (255, 15, 40)                               # 食物颜色
"""----------------------参数设置完毕----------------------"""

"""----------------------设置其他参数----------------------"""
loginFlag = True                                                # 登录窗口的标志
gameFlag = True                                                 # 控制游戏进行的标志
exitFlag = True                                                 # 弹出游戏结束窗口的标志
direct = 'left'                                                 # 设置游戏开始时贪吃蛇的移动方向
frameRate = 12                                                  # 控制游戏的帧率
clock = pygame.time.Clock()                                     # 创建一个对象来帮助跟踪时间
fontVersion = pygame.font.SysFont("Times New Roman", 18)        # 添加字体
font = pygame.font.SysFont("Times New Roman", 30)
"""----------------------参数设置完毕----------------------"""

"""------------------------游戏开始-----------------------"""
menuMusic.play()
while loginFlag:
    clock.tick(60)
    buttons = pygame.mouse.get_pressed(3)
    x1, y1 = pygame.mouse.get_pos()
    if x1 >= (width-i1Width)/2 and x1 <= (width+i1Width)/2 and y1 >= (hight-i1Hight)/2+hChange and y1 <= (hight + i1Hight)/2-10+hChange:
        startSur.blit(i11, ((width-i1Width) / 2, (hight-i1Hight) / 2 + hChange))
        if buttons[0]:
            menuMusic.stop()                                    # 停止播放背景音乐w
            loginFlag = False                                   # 点击”进入游戏“按钮后，跳出该循环，进入while gameFlag循环

    elif x1 >= (width-i2Width)/2 and x1 <= (width+i2Width)/2 and y1 >= (hight-i2Hight)/2+i1Hight+10+hChange and y1 <= (hight + i2Hight)/2+i1Hight+hChange:
        startSur.blit(i21, ((width-i2Width) / 2, (hight-i2Hight) / 2 + i1Hight + hItv + hChange))
        if buttons[0]:
            menuMusic.stop()                                    # 停止播放背景音乐
            print("游戏退出...")
            pygame.quit()
            exit()

    elif x1 >= (width-i3Width)/2 and x1 <= (width+i3Width)/2 and y1 >= (hight-i3Hight)/2+2*(i1Hight+10)+hChange and y1 <= (hight + i3Hight)/2+2*(i1Hight + 10)+hChange:
        startSur.blit(i31, ((width-i3Width) / 2, (hight-i3Hight) / 2 + 2 * (i1Hight+hItv) + hChange))
    else:
        i0 = pygame.transform.scale(i0, (550,550))
        startSur.blit(title, ((width-titleWidth) / 2, 160))
        startSur.blit(i0, (120, 30))
        startSur.blit(i1, ((width-i1Width) / 2, (hight-i1Hight) / 2 + hChange))
        startSur.blit(i2, ((width-i2Width) / 2, (hight-i2Hight) / 2 + i1Hight + hItv + hChange))
        startSur.blit(i3, ((width-i3Width) / 2, (hight-i3Hight) / 2 + 2 * (i1Hight+hItv) + hChange))

        # 显示版本文字
        textVersion = 'Version: ' + version                     # 通过字体创建文字对象
        text = fontVersion.render(textVersion, True, (255, 255, 255), startSurColor)
        textWidth, textHight = text.get_size()                  # 得到文字位置
        startSur.blit(text, (width-textWidth-5, hight-textHight-5))         # 在窗口右下角显示文字

    window.blit(startSur, (0, 0))
    pygame.display.flip()                                       # 更新屏幕

    # 监听事件
    for event in pygame.event.get():
        # 判断事件类型是否是退出事件
        if event.type == pygame.QUIT:
            print("游戏退出...")
            # quit 卸载所有的模块
            pygame.quit()
            # exit() 直接终止当前正在执行的程序
            exit()

gameMusic.play()
while gameFlag:
    clock.tick(frameRate)                                       # 更新clock对象
    for event in pygame.event.get():                            # 从队列中获取事件
        if event.type == pygame.QUIT:
            gameFlag = False
        elif event.type == pygame.KEYDOWN:
            # 这里小细节蛇不可以直接左右上下 要判断当前是在什么状态下前行
            if event.key == 273 or event.key == 119:
                if direct == 'left' or direct == 'right':
                    direct = 'top'
            if event.key == 274 or event.key == 115:
                if direct == 'left' or direct == 'right':
                    direct = 'bottom'
            if event.key == 276 or event.key == 97:
                if direct == 'top' or direct == 'bottom':
                    direct = 'left'
            if event.key == 275 or event.key == 100:
                if direct == 'top' or direct == 'bottom':
                    direct = 'right'

    eat_yellow = (head.row == snakeFood.row and head.col == snakeFood.col)             # 判断是否吃下黄色食物
    eat_red = (head.row == snakeFood_red.row and head.col == snakeFood_red.col)        # 判断是否吃下红色食物

    if eat_yellow:
        snakeFood = Point(row=random.randint(0, Row-1), col=random.randint(0, Col-1))
        score += 3
        frameRate += 0.02
        eatMusic.play()
    if eat_red:
        snakeFood_red = Point(row=random.randint(0, Row-1), col=random.randint(0, Col-1))
        score += 5
        frameRate += 0.04
        eatMusic.play()

    # 处理蛇的身子：1.把原来的头插入到snake的头上；2.把最后一个snake删掉
    snake.insert(0, head.copy())
    if not eat_yellow:
        snake.pop()                                             # 删除列表最末端的元素
        # 进行移动
        if direct == 'left':
            head.col -= 1
        if direct == 'right':
            head.col += 1
        if direct == 'top':
            head.row -= 1
        if direct == 'bottom':
            head.row += 1
        dead = False                                            # 是否死亡的标志
        if head.col < 0 or head.row < 0 or head.col >= Col or head.row >= Row:  # 判断是否出界
            dead = True
        for body in snake:                                      # 判断是否触碰蛇身
            if head.col == body.col and head.row == body.row:
                dead = True
                break
        if dead:
            print('Your score is ', score)
            print('Game over!')
            gameMusic.stop()
            gameFlag = False

    pygame.draw.rect(window, bgColor, (0, 0, width, hight))               # 绘制矩形

    textScore = 'Score: ' + str(score)                          # 通过字体创建文字对象
    text = font.render(textScore, True, (255, 255, 255), bgColor)
    textWidth, textHight = text.get_size()                      # 得到文字位置
    window.blit(text, (width-textWidth-10, 10))                 # 在窗口右上角显示文字

    rect(window, head, head_color, width, hight, Row, Col)      # 重新绘制蛇头
    for body in snake:                                          # 绘制蛇的身子
        rect(window, body, snake_color, width, hight, Row, Col)
    rect(window, snakeFood, snakeFood_color, width, hight, Row, Col) # 刷新食物位置
    rect(window, snakeFood_red, snakeFood_red_color, width, hight, Row, Col)  # 刷新食物位置

    pygame.display.flip()                                       # 将完整带显示的Surface对象更新到屏幕上

width = 300
hight = 200
window = pygame.display.set_mode((width, hight))                # 初始化一个准备显示的窗口或屏幕
bgColor = (0, 0, 0)                                             # 游戏背景颜色
pygame.display.set_caption('贪吃蛇大作战')                        # 设置当前窗口标题
failMusic.play()

while exitFlag:
    clock.tick(60)
    textFail = font.render('You Failed !', True, (255,0,0), bgColor)
    textFailWidth, textFailHight = textFail.get_size()                      # 得到文字位置
    window.blit(textFail, ((width-textFailWidth)/2, (hight-textFailHight)/2-40))                 # 在窗口右上角显示文字

    textScore = 'Your Score is ' + str(score)                          # 通过字体创建文字对象
    text = font.render(textScore, True, (255, 0, 0), bgColor)
    textWidth, textHight = text.get_size()                      # 得到文字位置
    window.blit(text, ((width-textWidth)/2, (hight-textFailHight)/2+textHight-30))             # 在窗口右上角显示文字

    pygame.display.flip()

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            exit()
```

类对象文件snakeClass.py。

```python
"""
#-------------------------------------------------------------------#
#                     Lei Lie's Python Code                      #
#-------------------------------------------------------------------#
#                                                                   #
#                   @Project Name : PyGameLearn                     #
#                   @File Name    : snakeClass.py                    #
#                   @Programmer   : Lei Lie                      #
#                   @Start Date   : 2021/4/16                       #
#                   @Last Update  : 2021/4/16                       #
#-------------------------------------------------------------------#
# Function:                                                         #
#          各种类对象的定义都放在这里                                    #
#-------------------------------------------------------------------#
"""
class Point():
    row = 0
    col = 0
    def __init__(self,row,col):
        self.row = row
        self.col = col
    def copy(self):
        return Point(row=self.row,col=self.col)
```

函数文件snakeFunction.py。

```python
"""
#-------------------------------------------------------------------#
#                     Lei Lie's Python Code                      #     
#-------------------------------------------------------------------#
#                                                                   #
#                   @Project Name : PyGameLearn                     #
#                   @File Name    : snakeFunction.py                #
#                   @Programmer   : Lei Lie                      #
#                   @Start Date   : 2021/4/16                       #
#                   @Last Update  : 2021/4/16                       #
#-------------------------------------------------------------------#
# Function:                                                         #
#          各种函数的定义都放在这里                                     #
#-------------------------------------------------------------------#
"""
import pygame
import random
from snakeClass import Point

# 生成食物并且不让食物生成在蛇的身体里面
def gen_food(Row, Col, head, snake):
    while True:
        position = Point(row=random.randint(0, Row - 1),
                         col=random.randint(0, Col - 1))
        is_coll = False
        if head.row == position.row and head.col == position.col:
            is_coll = True
        for body in snake:
            if body.row == position.row and body.col == position.col:
                is_coll = True
                break
        if not is_coll:
            break
    return position

def rect(window, point, color, width, hight, Row, Col):
    # 定位、画图需要left和top
    left = point.col * width / Col
    top = point.row * hight / Row
    # 将方块涂色
    pygame.draw.rect(window, color, (left, top, width / Col, hight / Row))
```

# 打包成exe文件

因为自己创建的`GUIexample`文件包含的模块太多，打包文件会导致最后成品太大了。因此我新建了一个环境，什么无用的模块都没有安装，只安装了`pyinstaller`模块和`pygame`模块，十分精简，专门用于打包游戏文件。

使用`Anaconda`创建环境在最开始已经说过了，这里不再赘述。

我这里自己创建了新环境`pyexe`，安装pyinstaller，代码为

```python
pip install pyinstaller
```

再安装`pygame`模块就可以了，其他什么模块都不用装。

然后进行文件打包。

点击开始菜单，进入Anaconda Prompt (Anaconda3)中，如下图所示。

![图5-命令软件]({{site.page}}/images/img-2021-04-18/图5-命令软件.png)

使用`activate pyexe`命令激活新创建的环境。

![图6-激活环境]({{site.page}}/images/img-2021-04-18/图6-激活环境.png)

转到写的代码的目录下。

![图7-转换地址]({{site.page}}/images/img-2021-04-18/图7-转换地址.png)

输入如下代码：

```python
pyinstaller -F snakeMain_plus.py -p snakeClass.py -p snakeFunction.py
```

注意：`-F`代表只生成一个`.exe`文件。其他的命令参考[这个网址](http://c.biancheng.net/view/2690.html)。

`-p`为要链接的文件，第一个的文件名一定是主函数。

运行结果如下。

![图8-打包界面]({{site.page}}/images/img-2021-04-18/图8-打包界面.png/)

输入命令运行之后，会生成一个`dict`和`build`文件夹。

![图8-生成文件]({{site.page}}/images/img-2021-04-18/图9-生成文件.png)

我的经验是把主函数里面使用的图片文件夹`Images`和音乐文件夹`Sounds`全部放进`dist`文件夹中。

![图9-放入文件]({{site.page}}/images/img-2021-04-18/图10-放入文件.png)

这样就可以正常运行程序了。

参考资料：

- 贪吃蛇游戏：[用pygame制作贪吃蛇游戏（详细）](https://blog.csdn.net/weixin_44051713/article/details/90042927?ops_request_misc=&request_id=&biz_id=102&utm_term=pygame&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-90042927.pc_search_result_no_baidu_js)
- 登录界面：[Python 用pygame 做一个游戏的开始界面（小白第一篇博客）](https://blog.csdn.net/pythonlols/article/details/88393734)
- 播放音乐：[Pygame：播放声音和音效](https://blog.csdn.net/w15977858408/article/details/104283348)
- pyinstall：[Python PyInstaller安装和使用教程（详解版）](http://c.biancheng.net/view/2690.html)

感谢上述作者的文章。

在这里把自己的源码、使用的图片和音效全部开源，希望能够帮助更多人。

：）

$———End———$