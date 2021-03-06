---
title: Joint Adaptation Networks
tags: JAN
show_author_profile: true
mathjax: true
excerpt_separator: <!--more-->
article_header:
  type: cover
  image:
    src: /assets/images/background/grass.jpg
---

摘要：

众所周知，深度网络能成功将可迁移的特征从源域适应到不同的目标域。本文提出了联合适应网络（joint adaptation networks，JAN），该网络基于提出的联合最大均值差异（joint maximummean discrepancy，JMMD）基准来学习对齐跨域的多个特定于域的层的联合分布，从而训练迁移网络。文章采用对抗训练测略，来最大化JMMD，从而使源域和目标域的分布更加可分。

<!--more-->

详细正文：

首先明确本文的目的，本文的目标在于降低跨领域联合分布的变化，从而学习可迁移的特征和分类器。文章通过最小化源风险（source risk）和域差异（domain discrepancy）来最小化目标风险（target risk）。最近的研究表明，与传统的手工特征相比，深度网络可以学习更多的可迁移特征。本文在此基础上通过深度网络学习可迁移特征来解决无监督域自适应问题。

网络结构如下图，文中使用的深度卷积网络包括AlexNet和ResNet。基于前人的研究可知，卷基层可以学习跨域的通用特征，因此，在将预训练的深度模型从源域转移到目标域时，我们选择微调卷基层特征。然而，深层特征虽然可以减少域分布的差异。文章以AlexNet为例，特征分布$$P(X^s)$$和$$Q(X^t)$$的偏移主要出现在特征层fc6和fc7，而标签分布$$P(Y^s)$$和$$Q(Y^t)$$主要出现在fc8。所以文章启用一种无监督的领域自适应来对原始联合分布（仅使用对应层中的联合分布）进行匹配。 

![Image](/assets/images/papers/JAN.png){:.border}

源域和目标域的采样分别来自分布$$P(X,Y)$$和$$Q(X,Y)$$，且$$P \neq Q$$。本文的目的就是构建一个深度网络，来减少联合分布之间的差异，并能学习可迁移的特征和分类器。一般来说分布的差异来自边缘分布的差异（$$P(X) \neq Q(X)$$）和条件分布的差异（$$P(Y/X) \neq Q(Y/X)$$）。

许多现有方法是通过将目标误差，源误差以及源域$$P(X^s)$$目标域$$Q(X^t)$$之间的边缘分布差异相加，来解决迁移学习的问题。而最大均值误差（MMD）是作为最广泛使用的样本检验统计量来衡量$$P(X^s)$$和$$Q(X^t)$$间的差异。但由于这些统计量是针对边缘分布定义的，所以仅适用于协变量适应问题（covariate shift adaptation problems）。

在此文章中，作者基于核嵌入分布理论（kernel embedding of distributions）提出了一种直接测量联合分布$$P(X,Y)$$和$$Q(X,Y)$$之间差异的方法。文中使用最大平均差异（MMD）来计算边缘分布间的差异，再用一种新颖的联合分布差异（Joint Distribution Discrepancy）来测量联合分布间的差异。文章的主要工作就在于提出这个指标，这指标作为再生希尔伯特空间（RKHS）下的联合分布的内核嵌入之间的平方距离，公式如下。

$$D(P,Q)=||C_{\phi,\varphi}(P)-C_{\phi,\varphi}(Q)||_H^2$$

文章将此指标应用在常见的AlexNet，GoogleNet网络的指定层，如下图中，并在Office-31和ImageCLFF-DA的数据集下做了实验，得到了较好的迁移结果。

![Image](/assets/images/papers/JAN2.png){:.border}