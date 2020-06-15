---
layout: post
title: "DL学习笔记（六）"
subtitle: "目标检测常用方法"
date: 2020-06-14 15:20:18
author: "QIY"
header-img: "img/DL.png"
header-mask: 0.3
catalog: true
tags:
    - Deep Learning
---


> Deep Learning 的学习需稳扎稳打 积累自己的小厂库

* TOC
{:toc}

# 1 R-CNN（Selective Search + CNN + SVM）

![](/img/in-post/200614-target-detection/132b39621693e0f0004fcd82242d4fa9.png)

候选窗结合深度学习方法

## 1.1 思路

先找出可能位置，即候选区域（Region Proposal）。<br />
再利用图像中的纹理/边缘/颜色等信息，提高准确性。

## 1.2 步骤

1.  从图像中提取2000个候选区域Region Proposal。
2.  将每个Region Proposal缩放（warp）成统一的227x227大小并输入到CNN。

3.  将CNN的fc7层的输出作为特征输入到SVM进行分类；

## 1.3存在的问题

Region Proposal太多，每一个都需要进行CNN提取特征+SVM运算量太大，检测速度慢。

# 2 SPP-Net(ROI Pooling)

SPP：Spatial Pyramid Pooling(空间金字塔)

![](/img/in-post/200614-target-detection/757c8c1dcbe4a8c2ae66cfd13f9237e0.png)

（2015年IEEE）

## 2.1 创新点

对全连接层的输入数据进行crop（crop：从一个大图抠出网络输入大小的patch，如227x227）
在卷积层的最后接入金字塔池化层，使得后面全连接层的输入固定。此时网络的输入可以是任意尺度的，在SPP
layer中每一个pooling的filter会根据输入调整大小。

## 2.2 对比R-CNN

RCNN对每个区域进行卷积计算，而SPPnet值愮对原图提取一次卷积特征，计算时间提升100倍+；

# 3 Fast R-CNN(Selective Search + CNN + ROI)

![](/img/in-post/200614-target-detection/f3e2af43352746c5c7062a83476c596c.png)

## 3.1 对比R-CNN

Fast R-CNN是在R-CNN基础上采纳了SPP net方法，具体如下：

1.  最后一个卷积层加入一个ROI pooling layer。<br />
2.  使用softmax代替SVM。<br />
3.  除了region proposal外，采用端到端的训练过程，使用多任务损失函数(multi-task
    loss),将边框回归Bounding Box Regression直接加入到CNN网络中训练。

1.  优点：实时检测位置，在多类检测同时，保证准确率，提升处理速度。Fast rcnn:不是每一个候选框输入CNN，而是输入一张完整的图片，再得到每个候选框的特征（第五个卷积层）

## 3.2 存在的问题

选择性搜索候选框，这个也非常耗时。

# 4 Faster R-CNN(RPN + CNN + ROI)

![](/img/in-post/200614-target-detection/7cb6691054f50044f15f61e96b1a2ab6.png)

## 4.1 思想

为了解决Fast R-CNN而提出，加入了一个提取边缘的神经网络，用于找候选框。

## 4.2 RPN

![](/img/in-post/200614-target-detection/1107783297e11d53b313893fd83792da.png)

1.  在feature map上滑动窗口<br />
2.  创建一个神经网络用于物体分类+框位置的回归<br />
3.  滑动窗口的位置提供了物体的大小位置信息<br />
4.  框的回归提供了框更精确的位置<br />

## 4.3 四个损失函数

RPN classification（anchor good/bad）<br />
RPN regression(anchor-\>propoassal)<br />
Fast R-CNN classification(over classes)<br />
Fast R-CNN regression(proposal -\> boxs)<br />
# 5 YOLO

![](/img/in-post/200614-target-detection/803d650f54039182f8a462275bffabf0.png)

## 5.1 背景

Faster R-CNN目前是主流，但速度仍不能满足实时要求。

## 5.2 步骤

1.给一个输入图像，首先将图像划分成7\*7的网络<br />
2.对于每个网格，都预测2个边框（包括每个边框是目标的置信度以及每个边框区域在多个类别上的概率）<br />
3.预测出7\*7\*2个目标窗口，然后根据阈值去除可能性比较低的窗口，最后NMS去除沉余串口。<br />
## 5.3 网络结构

![](/img/in-post/200614-target-detection/46bdef5345888b2457cec55b254c7c62.png)
Yolo将目标检测任务转换成回归问题，加快检测速度，因采用7\*7网格进行回归导致精度不高，后面又引入了yolo2/yolo9000等。

# 6 SSD

## 6.1 思想
SSD结合yolo回归思想和faster rcnn的anchor机制，结合region
proposal的思想实现定位精度提升。

论文截图：

![](/img/in-post/200614-target-detection/fab7b82a767de0aaf6fe74146a815ed7.png)

## 6.2 原理

1.yolo预测位置使用的是全图特征，SSD使用的是位置周围的特征<br />
2.建立位置和特征的对应关系<br />
2.1某一层特征图大小8\*8，使用3\*3的滑窗提取每个位置的特征，然后这个特征回归得到目标的坐标信息和类别信息。<br />
2.2 feature map，利用多层的特征，结合多尺度，不同层采用不同大小的滑动窗，即滑动视野不同。
