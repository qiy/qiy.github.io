---
layout: post
title:  "03线性代数基础"
categories: ALSA
tags:  linear-algebra PCA 
author: 柒月 
---

* content
{:toc}
引言，对X矩阵降维步骤：
1计算协方差矩阵；
2计算协方差矩阵的特征值，特征向量；
3取特征值较大的特征向量作为变换向量；
4降维后①方差尽可能大②无关性尽可能大；
*WELCOME TO READ ME*
## 1 特征值与特征向量 ##
Ax=λX
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/2353828a268c863e0b85ac6fe2c81ba1.png)

几何意义：

Ax：对x进行旋转，若Ax与x同线，则x为A的特征向量，λ即对原始向量的伸缩。

对维度的压缩，可根据特征值大小排序，删掉特征值小的量，如一个特征值是5，一个是0.1，去掉0.1整体的丢失并不多。上图Ax3可看出特征值越大，信号能量越大。

对于非方阵的特征值计算：奇异值分解。他是可以适应任意矩阵分解的方法：

  假设A是一个M \* N的矩阵，那么得到的U是一个M \*
M的方阵（里面的向量是正交的，U里面的向量称为左奇异向量），Σ是一个M \*
N的矩阵（除了对角线的元素都是0，对角线上的元素称为奇异值），V’(V的转置)是一个N
\*
N的矩阵，里面的向量也是正交的，V里面的向量称为右奇异向量），从图片来反映几个相乘的矩阵的大小可得下面的图片:

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4fc49c4fe318064c90f94b9ef78c2e87.png)

那么矩阵A的奇异值和方阵的特征值是如何对应的?

我们将一个矩阵AT \* A，将会得到一个方阵，我们用这个方阵求特征值可以得到：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/670ad6bda5f3b9279b528c6e643c744a.png)

这里得到的Vi就是上面的右奇异向量，此外，我们可以得到：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/306fa58ed9f3569175222afd69741257.png)

这里的σi就是上面说的奇异值。ui就是上面的左奇异向量。奇异值σ跟特征值相似，在矩阵Σ中也是按从大到小的方式排列，而且σ的值减小的特别的快，在很多的情况下前10%甚至1%的奇异值之和就占了全部奇异值之和的99%以上。也就是说可以用前r个大的奇异值来近似的描述矩阵，这里定义奇异值的分解：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/28cc90702450ffa9de69355c3e75fa89.png)

r是一个远远小于m和n的值，矩阵的乘法看起来是这个样子：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/446f2782983119cfb7a7c7fbb75beaab.png)

  右边的三个矩阵相乘的结果将会是一个接近于A的矩阵，在这儿，r越接近于n，则相乘的结果越接近于A。而这三个矩阵的面积之和（在存储观点来说，矩阵面积越小，存储量就越小）要远远小于原始的矩阵A，我们如果想要压缩空间来表示原矩阵A，我们存下这里的三个矩阵：U、Σ、V就好了（奇异值分解摘录：[HansonGao19](https://blog.csdn.net/u013108511)）

## 2 降维（PCA）##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ab1588e22e39bd13013c9b4a23c2981a.png)

降维：让行与行治安的相关性尽可能小，尽可能接近90°。降维后尽可能无关性大些。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/c55e0d56a4aa7111ed7349d1a08d6206.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/d5b979abaca7729f6145b83ed76a2e21.png)

\-

对X矩阵降维步骤：
1计算协方差矩阵；
2计算协方差矩阵的特征值，特征向量；
3取特征值较大的特征向量作为变换向量；
4降维后①方差尽可能大②无关性尽可能大；
