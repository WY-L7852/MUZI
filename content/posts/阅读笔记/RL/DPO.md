---
title: 'DPO损失函数的数学推导'
date: '2025-07-31T17:26:40+08:00'
lastmod: '2025-07-31T17:26:40+08:00'
author: "木子"
tags: # 标签
  - 强化学习
  - 机器学习
keywords: # 关键词
  - 
  - 
description: "本文记录了DPO的损失函数是如何推导的"
summary: 概述
math: true
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
在 RHLF 中，我们通常训练一个奖励模型来拟合人类偏好，用于评估生成内容的质量。但既然我们已经拥有了成对的人类偏好数据（即哪个回答更好），是否可以跳过奖励模型的训练，直接使用这些偏好对来优化策略模型？

实际上，我们并不需要预测一个回答被人类喜欢的“程度”，只需要知道在一对回答中哪个更受偏好即可。这样一来，偏好学习问题可以被转化为一个基于偏好对的“相对判断”问题，而不是绝对评分问题。

DPO（Direct Preference Optimization）就是基于这种思想，它不再训练显式的奖励函数，而是通过比较 preferred 和 dispreferred 回复在策略下的概率，直接优化策略本身。这个过程虽然形式上类似于一个对比型的二元分类损失，但本质上是在提升模型生成人类偏好内容的能力。

在开始对DPO推导之前，我们需要先复习一下 KL 散度和 Bradley-Terry 模型。

## KL 散度

KL散度是衡量两个概率分布差异的度量工具,其数学定义为:

$$
KL(P||Q) = \sum_{x \in X}P(x)log(\frac{P(x)}{Q(x)})=\mathbb{E}_{x\sim P(x)}[\log (\frac{P(x)}{Q(x)})]
$$

KL散度有以下特点:

1. 非负性: $KL(P||Q) \geq 0$
2. 非对称性: $KL(P||Q) \neq KL(Q||P)$
3. 当且仅当 $P=Q$ 时, $KL(P||Q)=0$

DPO中，为了防止模型为过度追求人类偏好导致生成结果胡编乱造，我们需要限制优化后的策略 $\pi(y|x)$ 与初始策略 $\pi_{ref}(y|x)$ 的差异。我们使用KL散度(Kullback-Leibler散度)来计算两个策略之间的差异。

## Bradley-Terry 模型

Bradley-Terry模型是一个用于分析配对比较数据的概率模型,主要用于:

1. 对事物进行排序和评级
2. 预测两个对象之间的胜率
3.  从成对比较中估计对象的潜在能力值

模型的核心思想是:

- 每个对象 $i$ 都有一个正实数参数,表示其能力值
- 对象 $i$ 胜过对象 $j$ 的概率为:

$$
P(i \succ j)=\frac{e^{\alpha_i}}{e^{\alpha_i}+e^{\alpha_j}}
$$

$\alpha_i$ 和 $\alpha_j$ 分别是对象 $i$ 和 $j$ 的能力值

## DPO推导

* $x$ : 输入
* $y$ : 模型输出
* $(x, y)$ : 状态-动作对
* $r(x, y)$ : 衡量回答质量的reward函数

受Bradley-Terry模型启发,我们定义 $y_1$ 优于 $y_2$ 的概率为:

$$
P(y_1 \succ y_2)=\frac{\exp(r(x, y_1))}{\exp(r(x, y_1))+\exp(r(x, y_2))}
$$

利用sigmoid函数 $\sigma(x) = \frac{1}{1+\exp(-x)}$，我们将概率转化为:

$$
P(y_1 \succ y_2) = \frac{1}{1+\exp(r(x, y_2)-r(x, y_1))}=\sigma(r(x, y_1)-r(x, y_2))
$$

为了让模型更可能生成人类偏好的输出，我们需要最大化上述概率，而训练时通常要最小化损失，因此，我们对上述概率取负数，此外，为了方便后续计算，我们还要为他取对数，于是得到单一样本的误差:

$$
\mathcal{l}_{single} = -ln\Big(\sigma(r(x, y_1)-r(x, y_2))\Big)
$$

所以，总损失可以表示为:

$$
\mathcal{Loss} = -\mathbb{E}_{(x,y_w,y_l)\sim D}[-ln\Big(\sigma(r(x, y_w)-r(x, y_l))\Big)]
$$

接下来就需要找到奖励函数 $r$ 。为找到这个函数，我们需要从RHLF的目标函数出发，目标函数如下：

$$
\max_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[ r(x, y) \right] - \beta \mathbb{D}_\text{KL} \left[ \pi_\theta(y|x) \parallel \pi_\text{ref}(y|x) \right] \right\}
$$

代入KL散度公式可得：

$$
\max_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[ r(x, y) \right] - \beta \mathbb{E}_{x\sim \mathcal{D}, y \sim \pi_\theta(y|x)}\left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)})\right]\right\}
$$

根据期望的性质 $\mathbb{E}[x+y] = \mathbb{E}[x]+\mathbb{E}[y]$，可得

$$
\max_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[ r(x, y)  - \beta \log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)})\right]\right\}
$$

除上 $\beta$ 并转化为最小化目标(添加负号):

$$
\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)})-\frac{1}{\beta} r(x, y)\right]\right\}
$$

把 $\frac{1}{\beta} r(x, y)$ 转化为 $\log \left(\exp \left(\frac{1}{\beta} r(x, y)\right)\right)$，得到:

$$
\begin{align*}
&\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)})-\log \left(\exp \left(\frac{1}{\beta} r(x, y)\right)\right)\right]\right\} \\ \\
= &\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)\exp \left(\frac{1}{\beta} r(x, y)\right)})\right]\right\}
\end{align*}
$$

接下来我们对 $\log$ 内部的部分除上一个 $Z(x)$ 再乘上一个 $Z(x)$，其中:

$$
Z(x) = \sum_y\pi_{ref}(y|x)\exp\left(\frac{1}{\beta} r(x, y) \right)
$$

式子便转换为:

$$
\begin{align*}
&\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)\exp \left(\frac{1}{\beta} r(x, y)\right)\frac{1}{Z(x)}Z(x)})\right]\right\}\\\\
= &\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi_\text{ref}(y|x)\exp \left(\frac{1}{\beta} r(x, y)\right)\frac{1}{Z(x)}})-\log Z(x)\right]\right\}
\end{align*}
$$

而我们注意到 $Z(x)$ 是一个不包含待优化策略 $\pi$ 的函数，因此接下来的化简步骤不需要考虑他。

设 $\pi^*(y|x) = \pi_\text{ref}(y|x)\exp \left(\frac{1}{\beta} r(x, y)\right)\frac{1}{Z(x)}$，根据 $Z(x)$ 的定义，$\pi^*$ 满足**概率分布大于0且和1为** 的定义，是一个合法的概率分布。

因此我们的目标变为:

$$
\begin{align*}
&\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi^*(y|x)})-\log Z(x)\right]\right\}\\ \\
=&\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi^*(y|x)})\right]-\mathbb{E}_{x \sim \mathcal{D}, y \sim\pi_\theta(y|x)}\left[\log Z(x)\right] \right\}
\end{align*}
$$

第一项与 KL 散度的形式一致，所以进一步转化为

$$
\begin{align*}
&\min_{\pi_\theta} \left\{ \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)} \left[\log (\frac{ \pi_\theta(y|x)}{\pi^*(y|x)})\right]-\mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)}\left[\log Z(x)\right] \right\} \\ \\
=&\min_{\pi_\theta} \left\{ \mathbb{D}_{KL}\big(\pi_\theta(y|x)||\pi^*(y|x)\big) -\mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(y|x)}\left[\log Z(x)\right] \right\}
\end{align*}
$$

前面提到过 $Z(x)$ 是与优化目标无关的函数，因此当上式的KL散度项最小，则上式最小，所以可以显示得到解为:

$$
\begin{align*}
\pi_\theta(y|x) &=\pi^*(y|x) \\ 
&=\pi_\text{ref}(y|x)\exp \left(\frac{1}{\beta} r(x, y)\right)\frac{1}{Z(x)}
\end{align*}
$$

经过移项操作，可以得到:

$$
\begin{align*}
\exp \left(\frac{1}{\beta} r(x, y)\right) &= Z(x)\frac{\pi_\theta(y|x)}{\pi_\text{ref}(y|x)}\\ \\
r(x, y) &= \beta\ln \left(Z(x)\frac{\pi_\theta(y|x)}{\pi_\text{ref}(y|x)}\right)
\end{align*}
$$

代入之前定义的损失函数:

$$
\begin{align*}
\mathcal{l}_{single} &= -ln\Big(\sigma(r(x, y_w)-r(x, y_l))\Big) \\ \\
&=-ln\left(\sigma \left(\beta\ln \left(Z(x)\frac{\pi_\theta(y_w|x)}{\pi_\text{ref}(y_w|x)}\right)-\beta\ln \left(Z(x)\frac{\pi_\theta(y_l|x)}{\pi_\text{ref}(y_l|x)}\right)\right)\right)\\ \\
&=-ln\left(\sigma \left(\beta\ln Z(x)+\beta\ln \left(\frac{\pi_\theta(y_w|x)}{\pi_\text{ref}(y_w|x)}\right)-\beta\ln Z(x)-\beta\ln \left(\frac{\pi_\theta(y_l|x)}{\pi_\text{ref}(y_l|x)}\right)\right)\right)\\ \\
&=-ln\left(\sigma \left(\beta\ln \left(\frac{\pi_\theta(y_w|x)}{\pi_\text{ref}(y_w|x)}\right)-\beta\ln \left(\frac{\pi_\theta(y_l|x)}{\pi_\text{ref}(y_l|x)}\right)\right)\right)\\ \\
\end{align*}
$$

