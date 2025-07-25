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

