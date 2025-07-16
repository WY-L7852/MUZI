---
title: 'Conditional neural field latent diffusion model for generating spatiotemporal turbulence'
date: '2025-07-11T12:56:54+08:00'
lastmod: '2025-07-11T12:56:54+08:00'
author: "木子"
tags: # 标签
  - 机器学习
  - 流体力学
keywords: # 关键词
  - 扩散模型
  - 流体生成
description: "介绍一种名为CoNFiLD的流体生成模型"
summary: 介绍一种名为CoNFiLD的流体生成模型
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

# Background
在Eddy-resolving-turbulence simulations中，传统方法，如直接数值模拟或大涡模拟需要极高的计算资源，这限制了他们在高雷诺数或者实时场景下的应用。

基于深度学习的代理模型，通常是确定性框架(比如CNN等，这些模型由于参数固定，所以同样的输入会得到同样的输出)，难以捕捉到湍流本身的混沌性与随机性。

CoNFiLD结合了条件神经场与扩散模型的生成式深度学习框架，能够在复杂三维空间下高效、稳定、可控地生成具有物理一致性和随机性的湍流场。

# Method
基本结构为 CNF Encoder ——> diffusion model ——> CNF Decoder
CNF 采用 SIREN 结构

## 条件生成
扩散后验采样(DPS) 进行条件生成 具体原理之后再写

条件神经场依靠Auto-decoder的方式进行训练，将latent vector直接作为视作优化参数，这样不需要额外构建encoder。

推理过程：
1. 输入一个在latent space里的高斯噪声到LDM
2. LDM每去噪一次，就将结果输入CNF解码器解码，将解码结果和观测值也就是我们的条件向量作比较，将他们的差异回传，利用梯度上升去优化去噪结果。
3. 反复进行第二个步骤直到生成最终结果。

