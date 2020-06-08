---
layout:     post
title:      "ML学习笔记（二.二）"
subtitle:   "图像线性分类器"
date:       2020-06-08 21:20:18
author:     "QIY"
header-img: "img/ML.png"
header-mask: 0.3 
catalog:    true
tags:
    - Machine Learning
---


> Machine Learning 的学习需稳扎稳打

* TOC
{:toc}


基本原则：测试集对比训练集；

# 1 临近法

![](/img/in-post/200608_classifier//8633c91db3e17d58fc19a1b082c39137.png)

如下 逐位减法

![](/img/in-post/200608_classifier//a04b963e7cab84e6a81e0efc3426258b.png)

\-

![](/img/in-post/200608_classifier//14dd1069ed97b1230639201a5cd82cf3.png)

\-

![](/img/in-post/200608_classifier//4695f0da2861e6f1f43dfa98998ac6c7.png)

![](/img/in-post/200608_classifier//df21750fcccc591d97c5721337e7fe41.png)

每一行为某一类的权重，最后那类权重高则判别为哪一类。

\-

![](/img/in-post/200608_classifier//c499ee95a09c6d8adb81e143bbae47f8.png)

神经网络：高维非线性切割。

\-

![](/img/in-post/200608_classifier//86f78edcbb08ea7a0d2e8cac66c9b002.png)

# 2 损失函数

## 2.1 支持向量机损失

![](/img/in-post/200608_classifier//90674bc95ab2c6323d7bb65ae4aaa65b.png)

（重点！）

![](/img/in-post/200608_classifier//7f6981897132be661de5649599218fca.png)

max（0，11-13+10）= 8，值比较大，则说明与真实差距小。

![](/img/in-post/200608_classifier//5755990dc81f9e18d91de135d80b15bc.png)

## 2.2 互熵损失

![](/img/in-post/200608_classifier//fc44685550f8e47a0435d6268bdfab0f.png)

这样就无负数了。

![](/img/in-post/200608_classifier//50ca5d0c3dffb0c4e6c795804bc7c1cd.png)

normalize：正则化后，其值小于1。

# 3 总结

线性分类器与非线性分类器（摘录：CSDN）

• 定义：只考虑二类的情形，所谓线性分类器即用一个超平面将正负样本分离开，表达式为
y=wx
。这里是强调的是平面。而非线性的分类界面没有这个限制，可以是曲面，多个超平面的组合等。【如果模型是参数的线性函数，并且存在线性分类面，那么就是线性分类器，否则不是。SVM两种都有(看线性核还是高斯核)。】

• 在线性分类器的基础上，用分段线性分类器可以实现更复杂的分类面。

非线性判别函数解决比较复杂的线性不可分样本分类问题，解决问题比较简便的方法是采用多个线性分界面将它们分段连接，用分段线性判别划分去逼近分界的超曲面。

• 常见的线性分类器有：LR,贝叶斯分类，单层感知机、线性回归。

优缺点：算法简单和具有“学习”能力。线性分类器速度快、编程方便，但是可能拟合效
果不会很好。

• **常见的非线性分类器：决策树、RF、GBDT、多层感知机**。

优缺点：非线性分类器编程复杂，但是效果拟合能力强。

对比学习：

<https://qiy.net/2020/06/03/ML_SVM_note1/>

<https://qiy.net/2020/06/04/ML_KNN_K-means/>
