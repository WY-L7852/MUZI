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
summary: 守恒方程的推导 
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

## Mass Conservation

当我们$\phi$取 1 时，我们就获得了质量守恒公式:
$$
\frac{\partial}{\partial t}\int\rho d\Omega+\int\rho\mathbf{v}\cdot\mathbf{n}dS = 0
$$
根据高斯散度定理，公式中的面积分可以转变成控制体的散度的积分，那么获得:
$$
\frac{\partial}{\partial t}\int\rho d\Omega+\int\nabla\cdot(\rho\mathbf{v})d\Omega = 0
$$
若控制体无限小，就可以得到质量守恒公式的微分形式:
$$
\frac{\partial\rho}{\partial t}+\nabla\cdot(\rho\mathbf{v})=0
$$

##  Momentum Conservation

当我们$\phi$取 $\mathbf{v}$ 时，我们就获得了动量守恒公式:

$$
\frac{d}{dt}\int_{\Omega_{CM}} \rho\mathbf{v} d\Omega = \frac{d}{dt}\int_{\Omega_{CV}}\rho\mathbf{v} d\Omega + \int_{S_{CV}} \rho\mathbf{v} (\mathbf{v}\cdot\mathbf{n})dS
$$
化简：

设 $\mathbf{v} = (v_i)$，$\mathbf{n} = (n_j)$，那么:
$$
\begin{align*}
\rho\mathbf{v} (\mathbf{v}\cdot\mathbf{n}) &= \rho v_i(v_jn_j)\\
&=(\rho\mathbf{v}\otimes\mathbf{v})\cdot \mathbf{n}
\end{align*}
$$
同样根据高斯散度定理，原式右侧的通量项可以转化为体积分:
$$
\int_\Omega\nabla\cdot(\rho\mathbf{v}\otimes\mathbf{v})d\Omega
$$
接下来分析等式左侧。很明显 ，等式左侧表达的是该控制体内的流体当前所受到的合力 $\mathbf{F}$，而合力$\mathbf{F}$由 body force 和 surface force 组成。其中，体力可以表达为:
$$
\int_\Omega \rho\mathbf{f}_{body}d\Omega
$$
表面力则可以用应力张量表示，也就是:
$$
\int_{S_{CV}}\boldsymbol{\sigma}\cdot\mathbf{n}dS
$$
同样应用高斯散度定理，转化为体积分:
$$
\int_{\Omega}\nabla\cdot\boldsymbol{\sigma}d\Omega
$$
具体此处与应力张量相关的内容可以参考这里的另一篇文章。

最后，去掉两边积分转化为局部形式，动量守恒公式可以化简为:
$$
\rho\mathbf{f}_{body}+\nabla\cdot\boldsymbol{\sigma}=\frac{\partial(\rho\mathbf{v})}{\partial t}+\nabla\cdot(\rho\mathbf{v}\otimes\mathbf{v})
$$
根据恒等式：
$$
\nabla\cdot(\rho\mathbf{v}\otimes\mathbf{v})=\rho(\mathbf{v}\cdot\nabla)\mathbf{v}+\mathbf{v}(\nabla\cdot(\rho \mathbf{v}))
$$
可以进行进一步化简:
$$
\begin{align*}
\rho\mathbf{f}_{body}+\nabla\cdot\boldsymbol{\sigma}&=\frac{\partial(\rho\mathbf{v})}{\partial t}+\nabla\cdot(\rho\mathbf{v}\otimes\mathbf{v})\\
&=\rho\frac{\partial\mathbf{v}}{\partial t}+\mathbf{v}\frac{\partial\rho}{\partial t}+\rho(\mathbf{v}\cdot\nabla)\mathbf{v}+\mathbf{v}(\nabla\cdot(\rho \mathbf{v}))\\
&=\rho\frac{\partial\mathbf{v}}{\partial t}+\rho(\mathbf{v}\cdot\nabla)\mathbf{v}+\mathbf{v}(\underbrace{\frac{\partial\rho}{\partial t}+\nabla\cdot(\rho \mathbf{v})}_\text{连续性方程})
\end{align*}
$$
最后，可以得到
$$
\rho\mathbf{f}_{body}+\nabla\cdot\boldsymbol{\sigma}=\rho\frac{\partial\mathbf{v}}{\partial t}+\rho(\mathbf{v}\cdot\nabla)\mathbf{v}
$$
## Conservation of scalar quantities

其他的一些标量守恒量也可以表达为以下形式:
$$
\frac{\partial}{\partial t}\int\rho \phi d\Omega+\int\rho\phi\mathbf{v}\cdot\mathbf{n}dS = \sum f_\phi
$$
$f_\phi$表示除了流体运动之外，所有导致物理量$\phi$改变的机制，这一项由扩散和源/汇两部分构成。值得注意的是，扩散行为通常是从高扩散到低，所以通常是通过梯度描述的，因此，有通式：
$$
\frac{\partial}{\partial t}\int\rho \phi d\Omega+\int\rho\phi\mathbf{v}\cdot\mathbf{n}dS = \int_S\Gamma\nabla\phi\cdot\mathbf{n}dS+\int_\Omega q_\phi d\Omega
$$
同样有微分形式：
$$
\frac{\partial\rho\phi}{\partial t}+\nabla\cdot(\rho\phi\mathbf{v}) = \nabla\cdot(\Gamma\nabla\phi)+q_\phi
$$






