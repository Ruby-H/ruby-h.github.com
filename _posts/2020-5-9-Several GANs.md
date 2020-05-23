---
title: Several GANs
tags: GAN
show_author_profile: true
mathjax: true
excerpt_separator: <!--more-->
article_header:
  type: cover
  image:
    src: /assets/images/background/ghost.jpg
---

摘要：

此文摘录了一些现阶段常见的GAN网络理论及相关改进。大致分为如下两个部分：一、GANs的理论提升；二、GANs的应用。

<!--more-->

详细正文：                                           

Part1 GANs基于Div的改进

1）JS距离的偏差问题

产生这个问题的原因在于，GAN网络在训练判别器的过程中，最大化$$V=E_{x \sim P_{data}}[logD(x)]+E_{x \sim P_{G}}[log(1-D(x))]$$
其实就是在求解$$P_{data}$$与$$P_{G}$$之间JS距离，但是到了训练生成器的过程中，它最小化$$V$$的过程并不一定在最小化$$P_{data}$$与$$P_{G}$$之间JS距离。
这是因为在训练生成器的时候我们是固定D的，而在接下来的继续训练生成器的过程中，minimize的值就不再是$$P_{data}$$与$$P_{G}$$之间的JS距离了（只有max V(G,D)才等于JS距离）。

这个问题可以通过限制训练次数来解决。对于判别器，理论上我们需要让他训练非常多次，直到判别器找到的$$D_0$$是$$ArgMax V(G,D)$$的全局最大解；而对于生成器，我们限制它只能训练一次，
这是为了防止训练完一次后$$V(G,D)$$发生变化导致$$D_0$$不再是$$ArgMax V(G,D)$$的解。

2）训练速度问题

在原始GAN中，判别器对于仿照数据的处理是：$$E_{x \sim P_G}[log(1-D(x))]$$，这个表达式一开始减得很慢，后面减得很快。对于刚开始训练的模型，我们希望在初期$$D(x)$$能够快速底更新，但
不幸的是，目标函数$$log(1-D(x))$$左端刚好是平稳的区域，依据梯度下降原理这会阻碍$$D(x)$$的快速更新。

为了解决这一问题，有人提出了把$$log(1-D(x))$$这个表达式换成$$-log(D(x))$$，同样能满足判别器的目标函数要求，并且在训练初期还能更新的比较快，这种GAN被叫做NSGAN（Non-saturating GAN）。

Part2 深度理解GAN理论

这一章节要表达的是对于GAN理论，不一定要基于JS距离，任何描述数据分布间距离都可以被放到GAN的架构中去。关于JS距离不合理的地方在于：如果有$$P_{G_{0}}$$与$$P_{G_{1}}$$都与$$P_{data}$$
没有交集，那么用JS距离去衡量这两者是一样的，无法区分$$P_{G_{0}}$$与$$P_{G_{1}}$$谁和$$P_{data}$$靠的更近。

于是为了解决上述问题，LSGAN被提出，其主要思想就是将判别器最后的sigmoid激活层改成linear激活层，但这解决的是GAN训练中梯度消失为0的问题，类似的还有RGAN，同样是从这角度出发的，但这绕开了如何
更好地测量$$P_G$$与$$P_{data}$$之间的距离，而在这问题上做出突破的是WGAN。

后续人们提出了WGAN的增强版WGAN-GP，以及SNGAN。解决了WGAN存在的问题：未能将D简直在1-Lipschitz function中。WGAN的贡献在于提出了用Wasserstein距离（也称EM距离）来取代JS距离，从而解决
上述JS距离所存在的问题；SNGAN则是用谱范数标准化神经网络的参数矩阵来让正则化产生更明确的限制。 

Part3 GANs基于Network的改进

1）DCGAN
DCGAN和GAN的原理一样，只是将CNN与GAN相结合，它将上述的generator和discriminator换成了两个卷积神经网络（CNN）。关于DCGAN的详细介绍可以参考博客（https://www.cnblogs.com/lyrichu/p/9054704.html）。

ImprovedDCGAN在DCGAN上做了优化，主要是针对GANs会出现收敛性不稳定的问题提出了不同的增强方法，包括：特征匹配（feature mapping)，批次判别（minibatch discrimination），历史平均（historical averaging），单侧标签平滑（one-sided label smoothing），虚拟批次正态化（virtual batch normalization）。这些方法能够让模型在生成高分辨率图像时表现得更好，而这正是
GANs的弱项之一。

2）SAGAN
因为一般的卷积核很难覆盖很大的区域，所以我们需要解决的问题是，如果找到一种能够利用全局信息的方法。传统的做法，比如用更深的卷积网络，或者直接采用全连接层获取全局信息，明显参数量和计算量太大。直到SAGAN的提出，把Attention机制引入了GANs的图像生成中，才找到一种比较简约高效的方法解决这一问题。

在SAGAN中还提出了两种优化策略，分别是Sepctral Normalization与TTUR，前者为D和G加入了谱范数归一化的方式，让D满足了1-lipschitz限制，同时也避免了G的参数过多导致梯度异常，使得整套训练较为平稳和
高效。后者则对生成器和判别器使用独立的学习率，从而来补偿正则化判别器中慢学习的问题。

3）BigGAN
为了产生逼真、精细的图片，首先想到的就是提升GANs的规模，增大Batchsize和Chanel。实验证明，增大Batchsize会带来在更少的时间训练出更好性能的模型，但也会使得模型在训练上的稳定性下降。而在Channel的
实验上发现一味地增加网络深度并不会带来更好的结果。

BigGAN采用共享嵌入，并在潜在空间的处置上用到了分层潜在空间（Hierarchical Latent Space）技术和正交正则化来实现截断的适应性，从而使z的整个空间能映射到良好的输出样本，来实现大规模高清图片的生成。

4）$$S^2GAN$$、$$S^2GAN-CO$$、$$S^3GAN$$
BigGAN实现了大规模高清图片的生成，但其的训练成本是非常大的，一个显著的缺陷是它需要大量的标注数据才能实现训练。$$S^2GAN$$即针对这点在判别器前加了一个特征提取器，从没有标注的真实训练数据里，学习其表征，然后对其进行聚类，然后把聚类的分配结果当成标注来用。而$$S^3GAN$$则是在$$S^2GAN$$的基础上，提升了其训练的稳定性。