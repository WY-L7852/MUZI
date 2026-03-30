---
title: 'Basic_Concepts'
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

绝大多数情况下 ，流体可以看作**连续介质(continuum)** 。

流体的流动是由外部施加的力引起的，这些力可以被分为两类:

1. 表面力(surface force)
2. 体力(body force)

在不同的流速下，流体的流动行为有较大区别。低速时，惯性可忽略，此时为**Creeping Flow**，随着速度提升，惯性的作用变得明显，流体微元(Fluid Particle)沿着平滑轨迹移动，此时为 **Laminar**；当速度进一步提升，流体的行为会不稳定，此时就成为了**turbulent**。

马赫数(Mach Number， Ma):  流体流速与该流体中声速的比值。这个数字决定了是否考虑流体的动能与内能的转换 。$Ma < 0.3$，流体可考虑为不可压缩流体(impressible fluid)；$Ma < 1$ 






