---
permalink: /:year-:month-:day/:title
layout: post
title:  "用matlab解微分方程"
author: "Lei Lie"
categories: Study
tag: notdouban
---

研究了使用matlab如何解微分方程。

考虑如下微分方程。

$$
\left\{\begin{matrix}
\dot x = -by		\\
\dot y = -ax
\end{matrix}\right. ,\quad
\left\{\begin{matrix}
x(0) = x_0		\\
y(0) = y_0
\end{matrix}\right.
$$

其解为

$$
\left\{\begin{matrix}
x = \frac{(\sqrt a x_0 + \sqrt b y_0)e^{-\sqrt{ab}t}+(\sqrt ax_0-\sqrt by_0)e^{\sqrt{ab}t}}{2\sqrt a}		\\
y = \frac{(\sqrt a x_0 + \sqrt b y_0)e^{-\sqrt{ab}t}-(\sqrt ax_0-\sqrt by_0)e^{\sqrt{ab}t}}{2\sqrt b}		
\end{matrix}\right. 
$$

用MATLAB代码来解这个微分方程，代码为

```python
S1 = dsolve('Dx = -b*y', 'Dy = -a*x', 'y(0) = y0', 'x(0) = x0');
y = S1.y
x = S1.x
```

结果为
```python
y =
 
(exp(-t*(a*b)^(1/2))*(a*x0 + y0*(a*b)^(1/2)))/(2*(a*b)^(1/2)) - (exp(t*(a*b)^(1/2))*(a*x0 - y0*(a*b)^(1/2)))/(2*(a*b)^(1/2))
 
 
x =
 
(exp(t*(a*b)^(1/2))*(a*x0 - y0*(a*b)^(1/2)))/(2*a) + (exp(-t*(a*b)^(1/2))*(a*x0 + y0*(a*b)^(1/2)))/(2*a)
```

把结果写成代数形式，为

$$
\left\{\begin{matrix}
x = \frac{e^{\sqrt {ab}t}(ax_0 - \sqrt{ab}y_0) + e^{-\sqrt {ab}t }(ax_0 + \sqrt{ab} y_0) }{2a}	\\
y = \frac{e^{- \sqrt {ab}t}(ax_0+ \sqrt{ab}y_0) - e^{\sqrt {ab}t }(ax_0- \sqrt{ab} y_0) }{2\sqrt{ab}} \\
\end{matrix}\right.
$$

和用手算的结果一样，也就是说用MATLAB可以解代数微分方程。