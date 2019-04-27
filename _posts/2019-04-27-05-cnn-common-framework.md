---
layout: post
title:  "06CNN与常用框架"
categories: MATH
tags:  derivative gradient
author: 柒月
---

* content
{:toc}
## 06CNN与常用框架 ##
*WELCOME TO READ ME*
# 1.  神经网络是什么？ #

## 1.1 LR与SVM ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/2b0b002d7ec9eabb45bc90ed26af5e23.png)

找到一条直线将两类点分开，通过逻辑回归实现，输入样本训练w/b，

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/99cea52d11d083439cd327a5d8a98cb6.png)

分a大于0.5与小于0.5两类。

添加少量隐藏层：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/8c6e0e5e89e5943143fccf7b4c6c571c.png)

DNN：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/cd059f6969863ccf17e461a2ed68f559.png)

\-

LR和SVM对于非线性显得比较若了，如：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ac904edaa17539f9738992f92b1385b6.png)

不管怎么弯曲都是一条线，那该怎么办呢？

## 1.2 神经网络 ##

**神经网络的思想是多条切割线**：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ff3ebaf486261482602c2322031e04d7.png)

两条线与的关系，当都为1是判断为第一类，当都为0时判断为另外一类。

计算如下：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ac748147e1502eca36e577e5fddea834.png)

（-30，+20，+20）

通过调整base**实现了“与”**；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4682ced6c88cff6b470e51dbb4107ad9.png)

p1层实现了一个分类线，p2层实现了另一个分类线。

**逻辑或**：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/814fcb865d0317e0c6eaa9f3d7f9dd44.png)

（-30，+20，+20）通过输入样本训练**“与”和“或”的组合（即训练权重和偏移**），以达到切割如下如：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/c4090beab17b32ea87d37db7fed34ab3.png)

块内取与，两块取或。

# 2 卷积神经网络 #

## 2.1 卷积神经网络是什么？ ##

1.  传统神经网络（**层间直接线性加权**）：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/70593016575303ca2f3d3b02276f3152.png)

1.  卷积神经网络（每层完成不同的功能）

实际中：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/b176d7f95b89c1f064bea2aa1491a809.png)

介绍：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ab8b134e1383429f5929d261bca79c7c.png)

## 2.2 输入层 ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4f4a7ad523aad91998c9970281de26e1.png)

去均值：

归一化：不一定非得是[-1,1]，只要是某个范围就行，一般rgb数据都是[0，255]，直接用就用就行。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/b0bb5ccd586648e58faf2d6a8395d3f9.png)

只在训练集中取均值，把所有的样本都减去这个均值。测试集减训练集上的这个均值，不用再去计算。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/2399a724904887802abe9a18b2c0f9e0.png)

## 2.3 卷积层 ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/316653a9f4f638ffb7454e8973075f21.png)

不是全连接，每次取一个数据窗口，和这个数据窗口全连接，不断的滑动这个小窗口，直到覆盖所有的样本点，这样做的好处是什么呢？这里的神经元叫做filter（滤波器）。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/0e88c8fa369a6a35ccafd023eafa59d6.png)

步长：每次滑动的距离；

填充值：原始的数据周围补充，为了滑动的整数倍；

如下：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/790076894c7c1d4f4f1b1c86feee04aa.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/80b7ef13113258dd29c07afc1947c594.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/fce05f625bf28d620b5f991f6f722018.png)

步长：2

填充值：1

深度：2（只取了两个）

计算方式：滑动相乘，只是对应位置相乘（内积），再相加，非行列式运算；

为什么叫滤波器：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9b17b8cd5c2b126725f487f4d00bb6a5.png)

\-

每个神经元（filter）的权重是固定的，不是每移动一个换一个。

## 2.4 激励层 ##

激励层其实是一个函数：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/88097046351c8ad2bb7e2e7659671554.png)

即：activation function，给神经元更大的刺激；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e9242674626e1d0db1259634fc50e38a.png)

几个函数比较：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/a2c754389c4e70d1f54eef852ac2375c.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/f956907a66aa5de0fd5a531479d10b5a.png)

很大区域的导数为零；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/c30a170414d5565b3a13f7acabf0ebc6.png)

比sigmoid快六倍，因为求梯度快。但左边仍会挂。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e750fafabdbc50887cb19a3af7c71f7b.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/3b53bd3221a7c5ff080a4d9c6ba88601.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4d7b2abe5fdb66345fc4c45ccbbbaf42.png)

maxout：两条线。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/0218a30e747bd4b68573acf4caae17c2.png)

## 2.5 池化层（压缩） ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9631b37d45dcd27ce3212d4d1696932a.png)

max pooling ：平移取最大值。

## 2.6 全连接 ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/b97d93647a65b581b8f94b326230bfe9.png)

CNN可以在cpu/gpu上训练，在gpu上训练速度大于CPU。


