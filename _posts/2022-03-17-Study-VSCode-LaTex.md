---
permalink: /:year-:month-:day/:title
layout: post
title:  "vscode中配置latex"
author: "Lei Lie"
categories: Study
tag: notdouban
---

 Latex和vscode的配置

之前一直用的是texstudio写论文，但我觉得texstudio的ui不好看，加上实际使用过程中，texstudio的工具栏里面那些功能我一直没有用过，只需要简单地编写tex文档就足够了，所以考虑使用一款颜值高并且比较轻量化的文档编辑器，因此vscode就进入了我的视线之中。

经过一番配置，我成功地在vscode里面调好了tex的编写环境，个人体感和texstudio使用感觉差不多，但颜值更好，使用起来比较开心！写这篇文档是为了记录一下配置心得，防止自己忘记，同时做一个输出，让自己消化一下知识。

需要先说明，这篇文档里面已经默认读者安装好texlive和vscode。

# 1、latex在vscode中的配置

vscode里面配置tex比较简单，分为两步：**（1）安装扩展**，**（2）添加设置**。

## 1.1、 安装扩展

打开vscode，左边工具栏里有个**扩展**栏，选中它！再在搜索框里面搜索**latex**，如下图所示。

![extension_install]({{site.page}}/images/img-2022-03-17/fig1.png)

在**扩展**的下图中标号为1的**已安装**中看一下出现**LaTex Workshop**没有，要是有，说明安装成功；或者看看下图中标号为2的红框是否显示成图中那样，如果是，则表明安装成功。

![installed]({{site.page}}/images/img-2022-03-17/fig2.png)

## 1.2、添加设置

在vscode页面，按下**f1**，输入**json**，选择**首选项：打开设置**这一项，如下图所示。

![settingjson]({{site.page}}/images/img-2022-03-17/fig3.png)

出现如下图所示的页面。

![jsonpage]({{site.page}}/images/img-2022-03-17/fig4.png)

在这个花括号中就可以输入设置代码了，代码如下：

```json
"latex-workshop.latex.tools": [	
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
],
"latex-workshop.latex.recipes": [
    {
        "name": "xelatex",
        "tools": [
            "xelatex"
        ],
    },
    {
        "name": "pdflatex",
        "tools": [
            "pdflatex"
        ]
    },
    {
        "name": "xe->bib->xe->xe",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    },
    {
        "name": "pdf->bib->pdf->pdf",
        "tools": [
            "pdflatex",
            "bibtex",
            "pdflatex",
            "pdflatex"
        ]
    }
],
"latex-workshop.latex.clean.fileTypes": [
    "*.aux",
    "*.bbl",
    "*.blg",
    "*.idx",
    "*.ind",
    "*.lof",
    "*.lot",
    "*.out",
    "*.toc",
    "*.acn",
    "*.acr",
    "*.alg",
    "*.glg",
    "*.glo",
    "*.gls",
    "*.ist",
    "*.fls",
    "*.log",
    "*.fdb_latexmk"
],
//tex文件浏览器，可选项为"none" "browser" "tab" "external"
"latex-workshop.view.pdf.viewer": "tab",
//自动编译tex文件
"latex-workshop.latex.autoBuild.run": "onFileChange",
//显示内容菜单：（1）编译文件；（2）定位游标
"latex-workshop.showContextMenu": true,
//显示错误
"latex-workshop.message.error.show": false,
//显示警告
"latex-workshop.message.warning.show": false,
//从使用的包中自动补全命令和环境
"latex-workshop.intellisense.package.enabled": true,
//设置为never，为不清除辅助文件
"latex-workshop.latex.autoClean.run": "never",
//设置vscode编译tex文档时的默认编译链
"latex-workshop.latex.recipe.default": "lastUsed",
// 用于反向同步的内部查看器的键绑定。ctrl/cmd +点击(默认)或双击
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
```
注意，一定要在花括号里面输入啊！

如果花括号里面有其他写好的设置，记得在最后一行代码上加个英文逗号，再另起一行，写好latex的设置。

# 2 测试

下面测试一下已经配置好的vscode设置是否可以编写tex。

我从IEEE里面下载了一个[latex模板](https://template-selector.ieee.org/secure/templateSelector/publicationType)，在vscode里面看看能不能编译。

下载好latex模板后，

① 把.tex文件拖入vscode中

② 点击左边工具栏的**TEX**按钮

③ 找到**Build LaTex project**，点它！如果不报错的话，则运行成功。我一般会选**Recipe:pdflatex**作为编译手段

④ 找到**View LaTex PDF**，选择**View in VSCode tab**，这个时候就会在vscode的页面中出现生成的pdf了，如下图右边白色区域所示。

![result]({{site.page}}/images/img-2022-03-17/fig5.png)

要是一切正常，到这里就结束了。

# 3 Latex配置代码说明

这里主要是写一些在第1.2节里面配置代码的说明，防止自己忘记。

## 3.1 编译命令

```json
"latex-workshop.latex.tools": [	
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
],
```
此处代码是为下面recipe的编译链提供编译命令，`name`是标签，可被recipe引用，`command`是编译命令。`%DOCFILE%`表示文件路径可以为中文。

## 3.2 编译链

```json
"latex-workshop.latex.recipes": [
    {
        "name": "xelatex",
        "tools": [
            "xelatex"
        ],
    },
    {
        "name": "pdflatex",
        "tools": [
            "pdflatex"
        ]
    },
    {
        "name": "xe->bib->xe->xe",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    },
    {
        "name": "pdf->bib->pdf->pdf",
        "tools": [
            "pdflatex",
            "bibtex",
            "pdflatex",
            "pdflatex"
        ]
    }
],
```
此处代码定义了编译链，即应按照什么顺序选择编译器编译tex文件，`name`是标签，即出现在工具栏里面的名称，`tool`定义了按照何种顺序使用编译器。

## 3.3 清除辅助文件

```json
"latex-workshop.latex.clean.fileTypes": [
    "*.aux",
    "*.bbl",
    "*.blg",
    "*.idx",
    "*.ind",
    "*.lof",
    "*.lot",
    "*.out",
    "*.toc",
    "*.acn",
    "*.acr",
    "*.alg",
    "*.glg",
    "*.glo",
    "*.gls",
    "*.ist",
    "*.fls",
    "*.log",
    "*.fdb_latexmk"
],
```
上面的代码定义了要清除的辅助文件的格式。

```json
//设置为never不清除辅助文件
"latex-workshop.latex.autoClean.run": "never",
```
这里有三个选项，分别是：

① onBuilt：每次编译后都清除辅助文件；

② onFailed： 编译失败时清除辅助文件；

③ never：从不清除辅助文件。

我这里选了`never`，一开始用的是`onFailed`，但我发现一旦编译错误之后，vscode会把所有辅助文件全部清除，导致修改代码去掉bug之后，也无法正常编译了，所以选择了`nerver`。

## 3.4 pdf浏览器选择
```json
//tex文件浏览器，可选项为"none" "browser" "tab" "external"
"latex-workshop.view.pdf.viewer": "tab",
```
上面代码定义了编译后查看pdf的浏览器，有4个选项，分别是：

① none: 不用浏览器；

② brower：使用网页浏览器；

③ tab：使用vscode内置浏览器；

④ external：使用外置pdf浏览器。

我用的是vscode内置浏览器，其实网页浏览器也挺好用的，显字大。

## 3.5 自动编译选项
```json
//自动编译tex文件
"latex-workshop.latex.autoBuild.run": "onFileChange",
```
上面代码定义了自动编译代码的功能，使用默认编译链自动构建tex的project，有3个选项，分别是：

① onFileChange：检测到文件更改的时候自动编译tex；

② onSave：保存的时候自动编译tex；

③ never：不自动编译tex。

我选择的是`onFileChange`，时时编译，保证自己的文档不丢失。

## 3.6 显示菜单内容
```json
//显示内容菜单：（1）编译文件；（2）定位游标
"latex-workshop.showContextMenu": true,
```
上面代码定义了在tex文件中，单击鼠标右键出现的菜单选项。

这是属性为`true`时右键菜单的样子。

![tureContextMenu1]({{site.page}}/images/img-2022-03-17/fig6.png)

这是属性为`false`时右键菜单的样子。

![tureContextMenu2]({{site.page}}/images/img-2022-03-17/fig7.png)

**Build LaTex projrct**为编译latex项目，**SyncTex from cursor**为定位tex代码在pdf文件中的位置，即正向定位。

这里为了方便编译和定位正文内容，我选了`true`选项。

## 3.6 错误和警告信息提示
```json
//显示错误
"latex-workshop.message.error.show": false,
//显示警告
"latex-workshop.message.warning.show": false,
```
上面代码定义了出现错误或者警告的时候是否会出现弹窗，我觉得弹窗很烦人，而且在终端中也可看到，所以选择了`false`属性。

## 3.7 自动补全功能
```json
//从使用的包中自动补全命令和环境
"latex-workshop.intellisense.package.enabled": true,
```
上面代码定义了是否自动补全命令和环境的功能，我觉得挺实用的，少打很多字，特别方便，还有提示，所以选择了`true`属性。

## 3.8 默认编译链选择
```json
//设置vscode编译tex文档时的默认编译链
"latex-workshop.latex.recipe.default": "lastUsed",
```
上面代码设置了vscode编译tex文档中的默认编译链，有2个选项，分别是

① first：选择`latex-workshop.latex.recipes`的第1条作为默认编译链；

② lastUsed：选择上一次使用的编译链作为默认编译链。

我在这里选择了`lastUsed`，因为我发现我用的最多的编译链是`pdflatex`。

## 3.9 反向定位
```
// 用于反向同步的内部查看器的键绑定。ctrl/cmd +点击(默认)或双击
"latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",
```
上面代码定义了从pdf浏览器中定位vscode的tex代码位置的功能，我觉得这条功能十分十分好用，强烈推荐！

这里也有2个选项，分别是：

① ctrl-click：ctrl+鼠标左键单击；

② double-click：鼠标左键双击。

我比较喜欢鼠标左键双击，所以选择的第二个选项。

# 4 结束
到这里整篇文档就结束啦！

我是参考了[这篇文档](https://zhuanlan.zhihu.com/p/166523064)来配置latex的，感谢这位作者！

要是不会安装texlive和vscode的话，[这篇文档](https://zhuanlan.zhihu.com/p/166523064)也有很详细的介绍，大家可以看看。

谢谢大家！