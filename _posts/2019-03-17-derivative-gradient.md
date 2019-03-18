---
layout: post
title:  "01导数到梯度"
categories: MATH
tags:  derivative gradient
author: 柒月
---

* content
{:toc}
引言：
对于一元函数，自变量x在x轴方向活动；模型：平面
对于二元函数，自变量在x-y平面活动，方向任意；模型：三维立体
对于三元函数，自变量在x-y-z的三维立体空间活动，方向任意:模型

*WELCOME TO READ ME*

# 1 导数到梯度 #
## 1.1 对于1元1阶导数 ##
f’(x)
对于2元1阶导数

对于2元函数有2个自变量，常用x，y表示，即f(x,y)，导数也有x和y两个方向的分量，所以引入了偏导数，而偏导是沿一个方向即坐标轴方向的变化率，而在多元函数中变化率可以是任意方向的。此时就引入了梯度，所谓梯度就是偏导数组成的列向量。

换种表示方法，n元函数f(x1,x2,x3,,,,xn)，即当X=(x1,x2,x3,,,xn)T时，f(X)=
f(x1,x2,x3,,,,xn)；

梯度的表示符号用记为grad f（X）或▽f（X）=

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/c71e026d79b955ecd2a14b78c4591afd.png)

所谓“梯度的方向就是函数增大最快的方向”，梯度的本身就是输入参数向量在函数方向上的偏导向量。

梯度方向的理解：

对于一元函数，自变量x在x轴方向活动；模型：平面

对于二元函数，自变量在x-y平面活动，方向任意；模型：三维立体

对于三元函数，自变量在x-y-z的三维立体空间活动，方向任意:模型

如二元函数中,z=x2+y2图像：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/22d069edf826390aa883d784f56593fd.png)

可知梯度：gradient =[ 2\*x, 2\*y]

在点（2，2）的梯度向量可为[4,4]：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/758a924a6b37eb74fed632ced0ffc52b.png)

## 1.2 对于多元2阶（hession矩阵） ##

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e7c4335c636fe2f331babe2b9c6a1692.png)

hession矩阵为对称矩阵，

# 2 泰勒级数与极值（由标量到矢量） #

标量：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/5a5e556a034000616af1054400bdce6c.png)

矢量：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ff2cea0dc75d0cfa47e1f11de88a8a1c.png)

机器学习中所说的步长即：δ。

范数性质：1正定性/2齐次性/3三角不等性/4相容性。

\|\|a\|\| ：表示a的标量大小。

计算也不喜欢解方程，也喜欢迭代。