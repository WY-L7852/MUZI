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