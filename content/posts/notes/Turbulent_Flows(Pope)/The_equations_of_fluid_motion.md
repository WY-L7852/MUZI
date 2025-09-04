---
title: 'The equations of fluid motion'
date: '2025-07-08T15:54:12+08:00'
lastmod: '2025-07-08T15:54:12+08:00'
author: "木子"
tags: # 标签
  - 流体力学
keywords: # 关键词
  - 流体力学
  - 牛顿流体
  - NS方程
description: "简要回顾控制constant-property Newtonian fluids的NS方程"
summary: constant-property Newtonian fluids的NS方程
weight: # 权重
slug: "" # 使用 slug属性 来作为当前文章的有效 url 的末尾部分
draft: false # 是否为草稿
comments: false # 本页面是否显示评论
reward: false # 打赏
mermaid: false #是否开mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
math: true
cover:
    image: "" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: "" # 图片的替代文本
    relative: false # 控制图片路径的解析方式
---
# Continuum fluid properties
Knusden number(Kn)远小于1时，特别是小于0.001时，可以认为流体属于连续介质范畴。
$$
Kn = \frac{\lambda}{l}
$$
其中 $\lambda$ 是分子平均自由程（molecular mean free path，即一个分子在碰撞前能自由移动的平均距离；$l$ 是特征长度，通常是问题中涉及的宏观尺度，比如管道直径(微管流动), 翼型长度(空气动力学), 湍流涡旋的尺度。

当视作连续介质时，意味着存在一个中间尺度 $l^*$, 并且$\lambda << l^* << l$，使得分子性质在一个体积 $V = {l^*}^3$ 上的平均。比如在 $x$ 处的区域 $\mathcal{V_x}$ 的密度 $\rho(\mathbf{x}, t)$ 为 $\mathcal{V_x}$ 的质量除以体积 $V$

# Eulerian and Lagrangian fields

**Eulerian fields**:

通过固定的空间点来描述物理量，而不跟踪具体的粒子或物体。比如连续密度场 $\rho(\mathbf{x}, t)$, 连续速度场 $\boldsymbol{U}(\mathbf{x}, t)$

**Lagrangian Fields:**

跟踪单个粒子或物体的轨迹来研究系统的演化

## Solution of Exercise

$$
\begin{equation}
\frac{d\boldsymbol{s}}{dt}=\frac{d}{dt}\boldsymbol{X}^+(t, \boldsymbol{Y}+d\boldsymbol{Y})-\frac{d}{dt}\boldsymbol{X}^+(t, \boldsymbol{Y})
\end{equation}
$$

位移 $\boldsymbol{X}^+$ 对时间的导数就是速度，所以上式可以化简为

$$
\begin{equation}
\frac{d\boldsymbol{s}}{dt}=\boldsymbol{U}^+(t, \boldsymbol{Y}+d\boldsymbol{Y})-\boldsymbol{U}^+(t, \boldsymbol{Y})
\end{equation}
$$

将速度转换至Eulerian fields, 可得

$$
\begin{equation}
\label{eq:4}
\frac{d\boldsymbol{s}}{dt}=\boldsymbol{U}(\boldsymbol{X}^+(t, \boldsymbol{Y}+d\boldsymbol{Y}), t)-\boldsymbol{U}(\boldsymbol{X}^+(t, \boldsymbol{Y}), t)
\end{equation}
$$

根据原式

$$
\begin{equation}
\boldsymbol{s}(t)=\boldsymbol{X}^+(t,\boldsymbol{Y}+d\boldsymbol{Y})-\boldsymbol{X}^+(t, \boldsymbol{Y})
\end{equation}
$$

可得

$$
\begin{equation}
\label{eq:6}
\boldsymbol{X}^+(t,\boldsymbol{Y}+d\boldsymbol{Y})=\boldsymbol{s}(t)+\boldsymbol{X}^+(t, \boldsymbol{Y})
\end{equation}
$$

将等式 $\ref{eq:6}$ 代入等式 $\ref{eq:4}$ , 可得
$$
\begin{equation}
\label{eq:7}
\frac{d\boldsymbol{s}}{dt}=\boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y})+\boldsymbol{s}(t), t\Big)-\boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y}), t\Big)
\end{equation}
$$
将 $\boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y})+\boldsymbol{s}(t), t\Big)$ 在 $\boldsymbol{X}^+(t, \boldsymbol{Y})+\boldsymbol{s}(t)$  处泰勒展开，可以得到
$$
\boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y})+\boldsymbol{s}(t), t\Big) = \boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y}), t\Big) + \nabla \boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y}), t\Big) \cdot \boldsymbol{s}(t)+R
$$
由于 $d\boldsymbol{Y}$ 是无限小量，高阶小项 $R$ 可以忽略。代入等式 $\ref{eq:7}$ ，可以得到
$$
\begin{equation}
\frac{d\boldsymbol{s}}{dt}= \nabla \boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y}), t\Big) \cdot \boldsymbol{s}(t)
\end{equation}
$$
转换为行向量的记法。可得
$$
\begin{align*}
\frac{d\boldsymbol{s}}{dt} &=\boldsymbol{s}(t) \cdot \nabla \boldsymbol{U}\Big(\boldsymbol{X}^+(t, \boldsymbol{Y}), t\Big)\\
&=\boldsymbol{s} \cdot (\nabla \boldsymbol{U})_{\boldsymbol{x} = \boldsymbol{X}^+(t, \boldsymbol{Y})}
\end{align*}
$$

# The continuity equation

## 质量守恒或连续性方程的推导

假设存在一个控制体，密度为 $\rho$ ，体积为 $V$ ，那么其质量 $M$ 为:
$$
M = \int_V\rho dV
$$
考虑其边界面为 $S$，有向外的法向量 $\mathbf{n}$，那么单位时间流经边界 $S$ 的的物质质量 $\Phi$ 为:
$$
\Phi = \int_S\rho \mathbf{U} \cdot\mathbf{n}dS
$$
其中 $\mathbf{U}$ 为物质的速度矢量。

根据质量守恒定理，质量不会凭空消失或产生，所以对于一个固定的控制体积，其质量变化应当等于从其边界处流入流出的质量总和，所以有
$$
\frac{dM}{dt} = -\Phi
$$
这里存在一个负号是因为我们假设法向量 $\mathbf{n}$ 是指向控制体外的，也就是说控制体内的质量是在减少，因此 $\frac{dM}{dt} <0$ 。

将 $M$ 和 $\Phi$ 代入，有

$$
\frac{d}{dt}\int_V\rho dV = -\int_S\rho \mathbf{U} \cdot\mathbf{n}dS
$$

根据散度定理，右边会变成 $-\int_V \nabla\cdot(\rho\mathbf{U}) dV$ ，因此得到

$$
\begin{align*}
&\frac{d}{dt}\int_V\rho dV = -\int_V \nabla\cdot(\rho\mathbf{U}) dV \\ \\
&\frac{d}{dt}\int_V\rho dV + \int_V \nabla\cdot(\rho\mathbf{U}) dV = 0 \\ \\
&\int_V \Big(\frac{d\rho}{dt} + \nabla\cdot(\rho\mathbf{U})\Big) dV= 0
\end{align*}
$$

由于上述等式对于任意体积 $V$ 都成立，因此被积函数为 0。最终，可以得到质量守恒方程也就是连续性方程：

$$
\frac{\partial \rho}{\partial t} + \nabla \cdot (\rho\boldsymbol{U})=0
$$


