---
title: '生成式模型(一) 变分自动编码器(VAE)'
date: '2025-09-04T13:08:47+08:00'
lastmod: '2025-09-04T13:08:47+08:00'
author: "木子"
tags: # 标签
  - 
  - 
keywords: # 关键词
  - 
  - 
description: "文章描述"
summary: 概述
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

变分自编码器（VAE）是一种深度生成模型，由 Kingma 等人在 2014 年提出。它的核心思想是：用概率的方法去理解和生成数据，而不仅仅是“记住数据”。

# 自动编码器（AE）

普通的自动编码器就像一个学生在做笔记：它看到一张西瓜的照片，就把照片压缩成一堆数字（潜在特征向量），然后再根据这些数字尽量画出原来的西瓜。但是，这个学生可能只会把数字“记住”，而不懂它们代表的意义。结果是，我们无法随意拿出一组数字，让学生画出一颗合理的新西瓜——因为我们不知道哪些数字组合才是合理的西瓜特征。

# VAE 的魔法

VAE 就像给这个学生发了一个指南：告诉他这些数字应该服从某种规律（潜在分布），并教他如何从这些规律里随机挑数字画出西瓜。这样，我们就可以从潜在空间里采样，生成形状、颜色、大小都合理的新西瓜，而不仅仅是复刻已有的照片。

# VAE 原理

