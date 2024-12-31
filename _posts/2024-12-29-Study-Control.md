---
permalink: /:year-:month-:day/:title
layout: post
title:  "有限、固定、指定时间控制的Lyapunov函数推导"
author: "Lei Lie"
categories: Essay
tag: notdouban 
---

# 1

给定某控制系统的初始时间为 $t_0$ ，初始状态为 $v_0$ ，要求 $t$ 时刻的状态 $v$ 。

设Lyapunov函数为

$$
\dot{V} = -k v^\alpha, \quad 0 < \alpha < 1, \quad k > 0.
$$

改写为

$$
\frac{\text{d}v}{\text{d}t} = -k v^\alpha,
$$

把 $-k v^\alpha$ 移到左边，为

$$
-\frac{1}{k v^\alpha} \text{d}v = \text{d}t,
$$

两边求积分，为

$$
-\frac{1}{k} \int_{v_0}^{v} \frac{1}{v^\alpha} \text{d}v = \int_{t_0}^{t} 1 \text{d}t,
$$

继续算，为

$$
-\frac{1}{k} \frac{1}{1-\alpha} v^{1-\alpha} \bigg |_{v_0}^v = t \bigg |_{t_0}^t
$$

在这里作简化处理，认为 $t_0 = 0$，$v = 0$ ，有

$$
t = \frac{1}{k} \frac{1}{1-\alpha} v_0^{1-\alpha}.
$$

这里的收敛时间 $t$ 只与 $k,\alpha,v_0$ 有关。

# 2

给定某控制系统的初始时间为 $t_0$ ，初始状态为 $v_0$ ，要求 $t$ 时刻的状态 $v$ 。

设Lyapunov函数为

$$
\dot{V} = - k_1 v^\alpha - k_2 v^\beta, \quad 0 < \alpha < 1, \quad \beta > 1, \quad k_1,k_2 > 0.
$$

改写为

$$
\frac{\text{d}v}{\text{d}t} = - k_1 v^\alpha - k_2 v^\beta,
$$

把 $k_1 v^\alpha-k_2 v^\beta$ 移到左边，为

$$
\frac{1}{k_1 v^\alpha + k_2 v^\beta} \text{d}v = \text{d}t,
$$

两边求积分，为

$$
\int_{v_0}^{v} \frac{1}{k_1 v^\alpha + k_2 v^\beta}  \text{d}v = \int_{t_0}^{t} 1 \text{d}t,
$$

最后会得到

$$
t = \frac{1}{k_1 (1-\alpha)} + \frac{1}{k_2 (\beta-1)}
$$

# 3

给定某控制系统的初始时间为 $t_0$ ，初始状态为 $v_0$ ，要求 $t$ 时刻的状态 $v$ 。

设Lyapunov函数为

$$
\dot{v} = - k_1 v^{1-\frac{\alpha}{2}} - k_2 v^{1+\frac{\alpha}{2}}, \quad 0 < \alpha < 1, \quad k_1,k_2 > 0.
$$

最后会得到

$$
t = \frac{1}{k_1 (1-\alpha)} + \frac{1}{k_2 (\beta-1)}
$$

---

# prompt

问题1：为什么有$\dot V \le -(\alpha V^p + \beta V^q)^k$，其中$\alpha,\beta,p,q,k>0:pk<1,qk>1$，就有$\dot V \le -\alpha^k V^{pk}$？

问题2：“因此，$\alpha V^{pk}(x(t))+\beta V^{qk}(x(t))$会比$\alpha V(x(t))+\beta V(x(t))$小”，怎么推导出来的？
