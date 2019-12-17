---
title: Cantonese Automatic Speech Recognition Using Transfer Learning From Mandarin
tags: GMM-HMM ASC
show_author_profile: true
mathjax: true
excerpt_separator: <!--more-->
article_header:
  type: cover
  image:
    src: /assets/images/background/water.jpg
---

摘要：

文章通过对高资源语言（普通话）的学习，为低资源语言（广东话）开发基本的自动语音识别器。 作者采用在普通话上训练的延时神经网络，并执行几层的权重传递到新初始化的广东话模型。 文章尝试迁移了传输的层数，学习率和预训练的i向量，发现这种方法可以缩短培训时间，减少数据量。 

<!--more-->

详细正文：

数据预处理：

由于普通话的数据集的采样频率是$$16kHz$$，而广东话的数据集的采样频率是$$8kHz$$。为了能更好的提取特征，对普通话数进行下采样。对数据的重采样和数据增强，都是在SoX（Sound eXchange）平台上完成的，在数据增强方面，文章是对原始的音频文件进行速度扰动（90%，100%，110%）从而达到增大数据量的目的。

特征提取：

GMM模型模型的输入特征是13维的梅尔频率倒谱系数（MFCCs）和3维高音特征（pitch features）。语法解析器通常使用的是感性线性预测系数（PLP）。时延神经网络（TDNN）的输入则是39维的MFCCs和4维的pitch features，其中还包括对每个帧输入的100维i矢量。文章还分别伪普通话数据集和广东话数据集分别设计训练了特征提取器。

模型结构：

语音模型包括两阶段：一个GMM-HMM模型，一个TDNN-LSTM模型。具体的模型结果如下两图。

![Image](/assets/images/papers/GMM-HMM.png){:.border}

![Image](/assets/images/papers/TDNN-LSTM.png){:.border}

主体网络没什么特别之处，迁移也做的比较简单，仅仅只是fix了前几层的参数，调整了学习率。