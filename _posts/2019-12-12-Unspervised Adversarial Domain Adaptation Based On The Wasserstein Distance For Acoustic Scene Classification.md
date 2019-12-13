---
title: Unspervised Adversarial Domain Adaptation Based On The Wasserstein Distance For Acoustic Scene Classification
tags: Wasserstein ASC
show_author_profile: true
mathjax: true
excerpt_separator: <!--more-->
article_header:
  type: cover
  image:
    src: /assets/images/background/fire.jpg
---

摘要：

在基于深度学习的机器侦听领域中，一大问题便是当使用未训练过的场景数据时会导致性能的下降。在此文中，文章针对声学场景分类（acoustic scene classification），提出了基于$$HDeltaH$$距离的理论模型和先前针对ASC无监督域自适应的对抗性判别式深度学习方法，并提出了一种使用Wasserstein距离的基于对抗性训练的方法。

<!--more-->

详细正文：

文章讲重点放在了无监督的域迁移上，先前的方法可以大致被分为两类：第一类方法是将域迁移（Domain Adaptation）和对源数据的优化一起执行。第二类方法是将这两阶段分开，先对源数据进行优化，再执行域迁移。

先前的对于ASC无监督对抗DA大多属于第二类，但是在最小化ASC源目标域分布之间的差异的过程中，会受到梯度消失和学习过程缓慢的困扰。

在本文中提出了一种基于ASC无监督对抗DA的深度学习方法，作者用Wasserstein生成对抗网络（WGAN）来解决了上述的问题，文章的主要工作如下：

1、为基于通用音频的深度学习域迁移提供了一种理论方法。

2、提出了采用Wasserstein距离的ASC无监督对抗域迁移。

3、和先前的方法相比，显著提高了ASC无监督对抗迁移的性能。

算法理论及思路：

先建立关于source（S）和target（T）之间的差异指标$$d_{H\Delta H}$$，并将其引入在后续对域迁移的工作中，给其设置了一个上限

$$\epsilon_T(h,f_T)\leq\epsilon_S(h,f_S)+\frac{1}{2}d_{H\Delta H}(Z_S,Z_T)+\lambda$$

其中