---
layout:     post
title:      "图像处理基础-滤波器（十二）"
subtitle:   "图像处理基础"
date:       2020-05-25 00:20:00
author:     "QIY"
header-img: "img/prml.jpg"
header-mask: 0.3 
catalog:    true
tags:
    - 图像处理
---


> 图像处理 的学习基本上是学十得一。稳扎稳打再来一次




# 1 空间域

## 1 平滑空间滤波器（积分）

线性滤波器

### 1.1.1 均值滤波器

![](/img/in-post/200525_lvboqi/ef3cf9f63a9766e0d854eeaa975b4614.png)

![](/img/in-post/200525_lvboqi/eaedd3135d41d6c916c9006df63f1406.png)

非线性滤波器

### 1.1.2最大值滤波器

原理：用像素领域内的最大值代替该像素

用途：寻找最亮点

方法：排序后找最大值s  
### 1.1.3中值滤波器

原理：用像素领域内的中间值代替该像素

用途：去除噪声

![](/img/in-post/200525_lvboqi/64a927298d58a7593fcfd6af0b753fb5.png)

  
### 1.1.4最小值滤波器

原理：用像素领域内的最小值代替该像素

用途：寻找最暗点

## 2 锐化空间滤波器（微分）

### 1.2.1 一阶梯度法

![](/img/in-post/200525_lvboqi/1d1d1ee58e0234896f3578f98a54a4f8.png)

算子：

![](/img/in-post/200525_lvboqi/2007c8badd231a24fb03da5008920327.png)

效果：

![](/img/in-post/200525_lvboqi/045144bc1086c94c1e5b3444215bd017.png)

### 1.2.2Roberts算法(交叉差分)

![](/img/in-post/200525_lvboqi/9f6dc415b7db14872f4a7e83aa9b34eb.png)

与梯度法对比：

![](/img/in-post/200525_lvboqi/5ebc2f6172ae9da11013133a846f4db3.png)

### 1.2.3Sobel算法

![](/img/in-post/200525_lvboqi/6da3acbb1e5a3193f5703dbed12b034d.png)

对比效果：

![](/img/in-post/200525_lvboqi/1927aa5e29391b6ae4d52ada15007e7e.png)

### 1.2.4 Prewitt算子

![](/img/in-post/200525_lvboqi/2a8076431909830b0e4348e5c6a7e977.png)

### 1.2.5 拉普拉斯算子

二阶，上面的都是一阶。

![](/img/in-post/200525_lvboqi/ebfcde4805a9d4da2982ed08e4dde3a1.png)

![](/img/in-post/200525_lvboqi/204c77d970ea971a9be3472cbebbb937.png)

重点用途：

将原始图像和拉普拉斯图像叠加在一起的简单方法可以保护拉普拉斯锐化处理的效果，同时又能复原背景信息。因此拉普拉斯算子用于图像增强的基本方法如下:

![](/img/in-post/200525_lvboqi/c6f287f9f9bf95f5cc9c36230d9a6a1b.png)

![](/img/in-post/200525_lvboqi/85230dfc875ecc84bf6901e01d71676b.png)

微分算子小结：

一阶微分算子（梯度算子）能用来：①突出小缺陷；②去除慢变化背景；  
二阶微分算子（Laplace算子)能用来：①增强灰度突变处的对比度；

# 2 频率域

傅里叶正变换为：

空转频：

![](/img/in-post/200525_lvboqi/c34caeeef3661849be03de51531ed497.png)

频转空：

![](/img/in-post/200525_lvboqi/0e892a6e1c14ab7fc794c68c5471794e.png)

对于离散数据：

![](/img/in-post/200525_lvboqi/26a1c0d3100256a917046e97f348c5db.png)

![](/img/in-post/200525_lvboqi/11af4146a2f437e06dc68461057f0b4b.png)

二维：

略

离散数据：

![](/img/in-post/200525_lvboqi/50fb130aa56a315c61d359c2e747f73c.png)

![](/img/in-post/200525_lvboqi/74a8ce4d341f40925c1631f6eee7276f.png)

## 2.1 平滑的频率域滤波器

原理，低通，去除高频点（个人理解：高频部分与1阶微分正相关）：

![](/img/in-post/200525_lvboqi/709470801a4b109f992ccec9c6d857b2.png)

### 2.1.1  理想低通滤波器ILPF

![](/img/in-post/200525_lvboqi/ff3819be935e6111e57691199ccc1f7b.png)

![](/img/in-post/200525_lvboqi/193b3ff090e52528b3a39be2bac4dc3d.png)

### 2.1.2  巴特沃思低通滤波器BLPF

![](/img/in-post/200525_lvboqi/ecc08eb83419e93a09d257d831a08714.png)

![](/img/in-post/200525_lvboqi/97a5e4ff5c61e8fe7a770d81d3f3e0da.png)

### 2.1.3 高斯低通滤波器GLPF

![](/img/in-post/200525_lvboqi/24b5405fa1f3c4ccd3bf78e2c5ba9b3e.png)

![](/img/in-post/200525_lvboqi/477499ac04b0bb58dda5920048f26e01.png)

有更加平滑的过渡带，平滑后的图像没有振铃（上面两种都有）现象与BLPF相比，衰减更快，经过GLPF滤波的图像比BLPF处理的图象更模糊一些

## 2.2 频率域锐化滤波器

原理，高通

同样有：理想高通滤波器IHPF，巴特沃斯高通滤波器BHPF，高斯高通滤波器GHPF，方向取反即可；

![](/img/in-post/200525_lvboqi/a9025dd4ef5137c56adf750fbe89a98d.png)

### 2.2.1  频率域拉普拉斯

![](/img/in-post/200525_lvboqi/65a78c3b1e94193830a911f453cea4e2.png)

频率域的拉普拉斯算子可以由如下滤波器实现:

![](/img/in-post/200525_lvboqi/44f80bbe845759c8616946296de7b25e.png)

![](/img/in-post/200525_lvboqi/4da4c125e67ba524b2a50a6989473477.png)

## 2.4 同态滤波器

![](/img/in-post/200525_lvboqi/4bab4b2a0522e639399afb38cd51e8d0.png)
