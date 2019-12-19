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

在基于深度学习的机器侦听领域中，一大问题便是当使用未训练过的场景数据时会导致性能的下降。在此文中，文章针对声学场景分类（acoustic scene classification），提出了基于$$H \Delta H$$距离的理论模型和先前针对ASC无监督域自适应的对抗性判别式深度学习方法，并提出了一种使用Wasserstein距离的基于对抗性训练的方法。

<!--more-->

详细正文：

文章讲重点放在了无监督的域迁移上，先前的方法可以大致被分为两类：第一类方法是将域迁移（Domain Adaptation）和对源数据的优化一起执行。第二类方法是将这两阶段分开，先对源数据进行优化，再执行域迁移。

先前的对于ASC无监督对抗DA大多属于第二类，但是在最小化ASC源目标域分布之间的差异的过程中，会受到梯度消失和学习过程缓慢的困扰。

在本文中提出了一种基于ASC无监督对抗DA的深度学习方法，作者用Wasserstein生成对抗网络（WGAN）来解决了上述的问题，文章的主要工作如下：

1、为基于通用音频的深度学习域迁移提供了一种理论方法。

2、提出了采用Wasserstein距离的ASC无监督对抗域迁移。

3、和先前的方法相比，显著提高了ASC无监督对抗迁移的性能。

算法理论及思路：

先建立关于source（S）和target（T）之间的差异指标$$d_{H \Delta H}$$，并将其引入在后续对域迁移的工作中，给其设置了一个上限

$$\epsilon_T(h,f_T)\leq\epsilon_S(h,f_S)+\frac{1}{2}d_{H \Delta H}(Z_S,Z_T)+\lambda$$

其中

$$\lambda=\epsilon_S(h^\ast,f_S)+\epsilon_T(h^\ast,f_T)$$

$$h^\ast=\mathop{argmin}\limits_{h\in H}(\epsilon_S(h,f_S)+\epsilon_T(h,f_T))$$

从上述可以发现，target域的误差$$\epsilon_T(h,f_T)$$是由三个因素影响的：

1、source域的误差$$\epsilon_S(h,f_S)$$

2、$$H \Delta H$$：source和target域的分布距离

3、理想的联合分类器的综合误差\lambda

在域迁移中，假设的是分类器classifier在source和target上都能有不错的表现，所以\lambda会比较小，基本可以忽略其影响，因此DA的主要问题变成获得一个分类器classifier，使其在source域上表现良好，并且能减少source和target两域上的差异。

对抗策略：
得到一个分类器$$h_d$$来预测$$Z$$ $$\iff$$ 训练一个特征提取器来干扰分类器$$h_d$$的判断

具体的训练流程：

首先，特征提取器$$M_S$$从source中训练，并复制一份$$M_T=M_S$$用于target域中。

其次，训练最小化$$d_{H \Delta H}(M_S(x_s),M_T(x_t))$$，并运用对抗策略：一方面优化$$h_d$$(分类器)在$$M_T$$后的输出，一方面使domain classifier error最大。即和GAN loss一样，转化为最优化问题：

\mathop{min}\limits_{h_d} \mathop{max}\limits_{M_T} L_GAN(h_d,M_T)

然而，解这个最优化问题会引起常见的计算方面的问题，包括：梯度消失和训练进程缓慢等。

本文主要应用的网络是DNN和WGAN，其中包含特征提取器$$M$$，label classifier $$h$$和domain classifier $$h_d$$。这里应用wasserestein距离$$W$$来作为衡量$$Z_S$$和$$Z_T$$间的区别，相比之前的$$H \Delta H$$距离，能避免上述在计算上遇到的问题，因为wasserestein距离是$$Z$$空间上的弱拓扑，有许多收敛模式：比如smooth convergence和 point-wise convergence。