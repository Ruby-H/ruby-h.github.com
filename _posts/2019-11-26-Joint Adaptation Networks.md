---
title: Joint Adaptation Networks
tags: JAN
show_author_profile: true
excerpt_separator: <!--more-->
article_header:
  type: cover
  image:
    src: /assets/images/background/Milotic.jpg
---

摘要：

众所周知，深度网络能成功将可迁移的特征从源域适应到不同的目标域。本文提出了联合适应网络（joint adaptation networks，JAN），该网络基于提出的联合最大均值差异（joint maximummean discrepancy，JMMD）基准来学习对齐跨域的多个特定于域的层的联合分布，从而训练迁移网络。文章采用对抗训练测略，来最大化JMMD，从而使源域和目标域的分布更加可分。

![Image](/assets/images/papers/JAN.png){:.border}
<!--more-->

详细正文：

首先明确本文的目的，本文的目标在于降低跨领域联合分布的变化，从而学习可迁移的特征和分类器。文章通过最小化源风险（source risk）和域差异（domain discrepancy）来最小化目标风险（target risk）。最近的研究表明，与传统的手工特征相比，深度网络可以学习更多的可迁移特征。本文在此基础上通过深度网络学习可迁移特征来解决无监督域自适应问题。

网络结构如下图，文中使用的深度卷积网络包括AlexNet和ResNet。基于前人的研究可知，卷基层可以学习跨域的通用特征，因此，在将预训练的深度模型从源域转移到目标域时，我们选择微调卷基层特征。然而，深层特征虽然可以减少域分布的差异。文章以AlexNet为例，特征分布$${P{ \left( {X\mathop{{}}\nolimits^{{S}}} \right) }}$$