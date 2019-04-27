---
layout: post
title:  "05梯度下降法与反向传播"
categories: MATH
tags:  derivative gradient
author: 柒月
---

* content
{:toc}
## 05梯度下降法与反向传播 ##
*WELCOME TO READ ME*

# 1 损失函数可视化 #

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e680ed15fd7940c064f9afff75b9f1c8.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/c4efde7933c859af65a0108158aa5a1c.png)

//每一个max都是一个合页函数，n个合页相加就是一个凹的；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/b7e9b5485fb94d68ff47a19ce2d23fc3.png)

凸函数的负系数加和是凹函数；？

# 2 最优化 #

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/56e15fcc5ea9e9a81428f56f20ae7478.png)

# 3 计算梯度 #

步长太小，那么到达最优点的时间长；步长太大，可能跳过最优点；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4ce47b6a5e49d038884261222cc88958.png)

数值梯度是一定能算出来的，f（x+h）-f（x） / h；

解析梯度：首先算出偏导，然后带入；

对于数值梯度：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9e3190a1e3c42e40380611190350ce5d.png)

解析梯度：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/02360ae39b099ccfb1d1848656f5d1e2.png)

\-如下计算：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/6742ce76c6d7240af5d98db94496da51.png)

l：不是1，是L,一个bool型计算，如果括号中成立，则输出1，不成立为零；

梯度乘以步长 更换原来的梯度（所有样本）：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/0e68ae7bfa36a1540bbfbbb47c47e0df.png)

抽取局部样本：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e225dc4cedfc3335b741d64c20e35876.png)

\-

# 4 反向传播 #

sigmoid函数

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4e3ad3c60d1759ca05601f0339b4cc16.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/1ff60d88a80be39f9bcd82048d52e732.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/fce2b5e6f717d0eabd4ff7c36c230d2c.png)

\-复合函数求偏导（链式法则）；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/25588a510338e1de486f5144294ee7aa.png)

layer3-layer2-layer1
三层复合，箭头表权重，一层层偏导往前求，最后求出每层权重的偏导；

神经网络的基本形式：每个输入都是向量，中间层神经元-中间变量，最后得到的是所有输入的损失函数，求所有权重最合适的值，对应损失函数最小的权重，求权重的梯度；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/69fce8e1b9b4d4e68bc7a59bf106e40a.png)

c：损失函数（神经网络最后得出值）；

ai：某一层神经网络的值；

ai+1：后一层神经网络的值；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/83d7603bde92f292e36333dca9a38e7d.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/07efecd4aa8b71b808fef332724a0262.png)

\-

符号圈点：列向量x列向量，对应位置相乘，得出同样长度的列向量；

\-------------
