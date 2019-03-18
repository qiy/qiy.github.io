---
layout: post
title:  "02概率论基础"
categories: MATH
tags:  derivative gradient
author: 柒月
---

* content
{:toc}
基础：
μ：期望(谬/穆,均值)；σ2:总体方差（西格玛）；
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/a401300c4d205accadf0d728e2e2e2e8.png)；
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/aee06fae9acb52d337db9883a4d39c64.png)为总体方差，![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/882637017673794eb5e6fdaf1bb71a03.png)为变量，![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/d12272b410ab9271fee480a318b0dc87.png)为总体均值，![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ca7b21878acf82ae1057c8a4242d7b2b.png)为总体例数；
*WELCOME TO READ ME*
# 1 随机变量 #
## 1.1 累计分布函数 ##
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ef5af44637e041b8f4d80f61c723713e.png)
## 1.2 概率密度函数 ##
当a与b非常接近时，相减已很不直观了，故引入积分思想，产生概率密度函数：
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/a427ef572bc1553299adf6c35559f6dd.png)

从-3σ 到3σ间概率已占99.7%；
## 1.3 高斯分布（正态分布） ##
![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/18a1c7991334b77ba15c50f877f39b0e.png)

(大写西格玛） \|∑\|:协方差行列式；

协方差：**衡量两个变量的总体误差**。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ffa42420f8acc7a54eac0e66c211e88f.png)

；

方差：协方差的特殊情况，即两个变量是相同的情况（X=Y），几何意义：数的分散程度，越分散代表性越强；

中心极限定理：独立同分布的随机变量求和后，概率收敛于高斯分布。如X=x1+x2+x3+x4，其中xi
独立同分布，那么X收敛于高斯分布。两个高斯分布相加还是高斯分布。一个高斯分布+任意一个随机变量=近似于高斯分布；

## 1.4 贝叶斯公式 ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/f9dba4359e7cad004b6d004bd66f2090.png)

P(A\|B)：事件B已发生的情况下，事件A发生的概率（条件概率）

P(A,B)=P(AB)=P(A∩B)：事件A,B同时发生的概率。

CuA = Ac：A的补集。

根据以上1/2/3公式可计算下题：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9f79a328d934e787937605a9d4ca5a69.png)

已知先验P(A\|B),求后验P(B\|A);

题2：

假设一个班级失败者睡懒觉的概率为99%。而成功者保持早起的概率为99%。对该班级进行懒觉与早起情况做调查，得知0.5%的同学睡懒觉，请问每位调查中每位睡懒觉的同学为失败者的概率多高？

P(S\|L) = 99%

求：P(L\|S)
