---
title: 'Basic Concepts'
date: '2026-03-30T16:18:50+08:00'
lastmod: '2026-03-30T16:18:50+08:00'
author: "木子"
tags: # 标签
  - CFD
keywords: # 关键词
  - CFD

description: "基础流体知识"
summary: 概述
weight: 1
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
cover:
    image: "" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: "" # 图片的替代文本
    relative: false # 控制图片路径的解析方式
---
# Basic Concept of Fluid Flow

流体是一种分子结构不会抵抗外部剪切力的物质。非常微小的力也会使其形变(deformation)。

绝大多数情况下 ，流体可以看作**连续介质(continuum)**。

流体的流动是由外部施加的力引起的，这些力可以被分为两类:

1. 表面力(surface force)
2. 体力(body force)

在不同的流速下，流体的流动行为有较大区别。低速时，惯性可忽略，此时为**Creeping Flow**，随着速度提升，惯性的作用变得明显，流体微元(Fluid Particle)沿着平滑轨迹移动，此时为 **Laminar**；当速度进一步提升，流体的行为会不稳定，此时就成为了**turbulent**。

马赫数(Mach Number， $Ma=\frac{流速}{声速}$):  流体流速与该流体中声速的比值。这个数字决定了是否考虑流体的动能与内能的转换 。$Ma < 0.3$，流体可考虑为不可压缩流体(imcompressible fluid)；$Ma < 1$，亚音速(subsonic)；$Ma > 1$，超音速(Supersonic)，出现激波(shock wave)；$Ma > 5$，高超音速(Hypersonic)，高温高压，可能存在化学反应。

**Control Mass** 也称为质量控制系统，是一个封闭系统，包含一个固定大小的质量，与环境没有质量交换 ，但是可以进行能量交换。

**Control Volume** 则是一个开放系统，在流体力学这里我个人觉得可以理解为空间上的一个固定区域，能够进行 质量流动。

**extensive property** 广延量，取决于系统的大小，如质量，体积等属性

**intensive property** 强度量，不依赖于系统的质量，如压力，密度等

##  Conservation Principles

雷诺运输定理(Reynolds' Transport theorem):
$$
\frac{d}{dt}\int_{\Omega_{CM}} \rho\phi d\Omega = \frac{d}{dt}\int_{\Omega_{CV}}\rho\phi d\Omega + \int_{S_{CV}} \rho \phi (\mathbf{v-v_b})\cdot\mathbf{n}dS
$$

- $\phi$表示单位质量物质所携带的物理量。
- $\mathbf{v_b}$表示控制体的移动速度

接下来进行简单的推导：

![](Computational_method_for_Fluid_Dynamics/Basic_Concept_fig_1.png)

假设再$t$时刻时，一个CM正好与一个固定的CV重合，那么此时存在
$$
\Phi_{CM}(t) = \Phi_{CV}(t)
$$
当$t+dt$时刻时，如上图所示，有
$$
\Phi_{CV}(t+dt) = \Phi_{CM}(t+dt)-d\Phi_{out}+d\Phi_{in}
$$
所以$\Phi_{CV}$的变化率可以写为:
$$
\begin{align}
\frac{d\Phi_{CV}}{dt} &= \frac{\Phi_{CV}(t+dt)-\Phi_{CV}(t)}{dt}\\
&=\frac{\Phi_{CM}(t+dt)-d\Phi_{out}+d\Phi_{in} -\Phi_{CV}(t)}{dt}\\
\end{align}
$$
由于前面提到过，$t$时刻时CM和CV是重合的，因此，有
$$
\begin{align}
\frac{d\Phi_{CV}}{dt} &=\frac{\Phi_{CM}(t+dt)-\Phi_{CM}(t)}{dt}-\frac{d\Phi_{out}-d\Phi_{in}}{dt}\\
&=\frac{d\Phi_{CM}(t)}{dt}-\frac{d\Phi_{out}-d\Phi_{in}}{dt}
\end{align}
$$
整理之后可得
$$
\frac{d\Phi_{CM}(t)}{dt} = \frac{d\Phi_{CV}}{dt}+\frac{d\Phi_{out}-d\Phi_{in}}{dt}
$$
可以注意到的是，等式右侧的第二项 $\frac{d\Phi_{out}-d\Phi_{in}}{dt}$描述的是该控制体内流入流出的总量，也就是流经控制体表面的通量总和，可以写成积分形式为:
$$
\int_{CS}\rho\phi\mathbf{v}\cdot\mathbf{n}dS
$$
此处将控制体速度视为 0。

此外，$\Phi_{CM}(t)$和$\Phi_{CV}(t)$也可以写为体积分形式，最终可以得到完整的积分形式雷诺运输定理。








