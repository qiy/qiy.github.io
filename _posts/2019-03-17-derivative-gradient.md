---
layout: post
title:  "导数到梯度"
categories: MATH
tags:  derivative gradient
author: 柒月
---

* content
{:toc}
## 1 导数到梯度 ##

*WELCOME TO READ ME*

**1.1**

对于1元1阶导数

f’(x)

对于2元1阶导数

对于2元函数有2个自变量，常用x，y表示，即f(x,y)，导数也有x和y两个方向的分量，所以引入了偏导数，而偏导是沿一个方向即坐标轴方向的变化率，而在多元函数中变化率可以是任意方向的。此时就引入了梯度，所谓梯度就是偏导数组成的列向量。

换种表示方法，n元函数f(x1,x2,x3,,,,xn)，即当X=(x1,x2,x3,,,xn)T时，f(X)=
f(x1,x2,x3,,,,xn)；

梯度的表示符号用记为grad f（X）或▽f（X）=

![](https://i.imgur.com/QOCK0Ee.png)

所谓“梯度的方向就是函数增大最快的方向”，梯度的本身就是输入参数向量在函数方向上的偏导向量。

梯度方向的理解：

对于一元函数，自变量x在x轴方向活动；模型：平面

对于二元函数，自变量在x-y平面活动，方向任意；模型：三维立体

对于三元函数，自变量在x-y-z的三维立体空间活动，方向任意；模型

如二元函数中：

z=x2+y2图像：
![](https://i.imgur.com/yqwgK14.png)

可知梯度：gradient =[ 2\*x, 2\*y]

在点（2，2）的梯度向量可为[4,4]：
![](https://i.imgur.com/Jczad12.png)
**1.2**

对于多元2阶（hession矩阵）:
![](https://i.imgur.com/JrMtlJF.png)
hession矩阵为对称矩阵，

## 2 泰勒级数与极值（由标量到矢量） ##
标量：
![](https://i.imgur.com/8F3ynrb.png)
矢量：
![](https://i.imgur.com/KKS4bWc.png)
机器学习中所说的步长即：δ。

范数性质：1正定性/2齐次性/3三角不等性/4相容性。

||a|| ：表示a的标量大小。

计算也不喜欢解方程，也喜欢迭代。
