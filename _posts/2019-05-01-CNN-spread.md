---
layout: post
title:  "08 CNN推展案例"
categories: MATH
tags:  CNN R-CNN FAST-RCNN FASTER-RCNN
author: 柒月
---

* content
{:toc}
1 图像识别思路
*WELCOME TO READ ME*
# 1 图像识别思路 #

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/1ea4a075c263c44ab1d2a9d3b9af485c.png)

前向运算得到loss，反向传播得到梯度；

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/407e638511498b005acaee6791223f4e.png)

可分上图四个阶段。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/d052fce4c2c7cdbb04df2aedd099edc7.png)

一个 (x,y,w,h)四个变量就可以框出来。

如何localization呢？

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/8aad8190a3c17afacab566c070a80b6e.png)

用欧氏损失（L2）输出4个数。

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/d7ae5ac9ce43ae7872e68fc38c3f8797.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e77955f7aae18de819a353d857b59535.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/898f3d7c0b0b72363a32747bfce2f75f.png)

\-

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/f6394a7384afb7611e7793c7bbef1c62.png)

对于欧氏损失：

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/60da234ecf23c076fc784e8d5ae4e351.png)

方法一 看作回归问题，取点用欧式损失

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/cac4782a0f3c966b94952b9fd08727aa.png)

方法二 取框 滑动 比较得分函数

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/8fba1caa02f91f9a37fb51a7f1214c48.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ce6747e3c7dbd88104137980e7b35a2c.png)

由于全连接层运算量太大，将全连接层变换成卷积方式，共享权重

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9c32a46be6bb49966ec7af7276b17ad8.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/a3630b5cf78dd14a93c3a1a821a73f35.png)

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/fcb00b968025fb992d4dbe027b10f1cc.png)

regression问题，这次情况很难处理

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/6ceb370ccafe3187770974a81d97e20c.png)

采用分类方法

![](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/4403bdaa8fc58eb2470fb374912f682e.png)

# 2 图像识别方法 #

## 2.1 R-CNN 横空出世   ##
R-CNN（Region
CNN，区域卷积神经网络）可以说是利用深度学习进行目标检测的开山之作。

R-CNN算法的流程如下  


![https://static.oschina.net/uploads/space/2018/0331/144342_iY9y_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9d85fdba61058fa1182e16da54d713d2.png)

   
1、输入图像  
2、每张图像**生成1K\~2K**个候选区域  
3、对每个候选区域，使用深度网络提取特征（AlextNet、VGG等CNN都可以）  
4、将特征送入每一类的SVM 分类器，判别是否属于该类  
5、使用回归器精细修正候选框位置

下面展开进行介绍  
1、生成候选区域  
使用Selective
Search（选择性搜索）方法对一张图像生成约2000-3000个候选区域，基本思路如下：  
（1）使用一种过分割手段，将图像分割成小区域  
（2）查看现有小区域，合并可能性最高的两个区域，重复直到整张图像合并成一个区域位置。优先合并以下区域：  
- 颜色（颜色直方图）相近的  
- 纹理（梯度直方图）相近的  
- 合并后总面积小的  
- 合并后，总面积在其BBOX中所占比例大的  
在合并时须保证合并操作的尺度较为均匀，避免一个大区域陆续“吃掉”其它小区域，保证合并后形状规则。  
（3）输出所有曾经存在过的区域，即所谓候选区域  
2、特征提取  
使用深度网络提取特征之前，首先把候选区域归一化成同一尺寸227×227。  
使用CNN模型进行训练，例如AlexNet，一般会略作简化，如下图：  


![https://static.oschina.net/uploads/space/2018/0331/144354_qJvD_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/d205ebff29e5dc4c01b71758ad5f3504.png)

   
3、类别判断  
对每一类目标，使用一个线性SVM二类分类器进行判别。输入为“深度网络（如上图的AlexNet）输出的4096维特征”，输出是否属于此类。  
4、位置精修  
目标检测的衡量标准是重叠面积：许多看似准确的检测结果，往往因为候选框不够准确，重叠面积很小，故需要一个位置精修步骤，对于每一个类，训练一个线性回归模型去判定这个框是否框得完美，如下图：  


![https://static.oschina.net/uploads/space/2018/0331/144403_IPBp_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/9fe49013ea6662e6021e93d02e579b39.png)

   
R-CNN将深度学习引入检测领域后，一举将PASCAL VOC上的检测率从35.1%提升到53.7%。

## 2.2 Fast R-CNN大幅提速 ##  
继2014年的R-CNN推出之后，Ross Girshick在2015年推出Fast
R-CNN，构思精巧，流程更为紧凑，大幅提升了目标检测的速度。  
Fast
R-CNN和R-CNN相比，训练时间从84小时减少到9.5小时，测试时间从47秒减少到0.32秒，并且在PASCAL
VOC 2007上测试的准确率相差无几，约在66%-67%之间。  


![https://static.oschina.net/uploads/space/2018/0331/144411_NCm0_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/31271dad464bbd58147e547d503ba4f2.png)

   
Fast R-CNN主要解决R-CNN的以下问题：  
1、训练、测试时速度慢  
R-CNN的一张图像内候选框之间存在大量重叠，提取特征操作冗余。而Fast
R-CNN将整张图像归一化后直接送入深度网络，紧接着送入从这幅图像上提取出的候选区域。这些候选区域的前几层特征不需要再重复计算。  
2、训练所需空间大  
R-CNN中独立的分类器和回归器需要大量特征作为训练样本。Fast
R-CNN把类别判断和位置精调统一用深度网络实现，不再需要额外存储。

下面进行详细介绍  
1、在特征提取阶段，通过CNN（如AlexNet）中的conv、pooling、relu等操作都不需要固定大小尺寸的输入，因此，在原始图片上执行这些操作后，输入图片尺寸不同将会导致得到的feature
map（特征图）尺寸也不同，这样就不能直接接到一个全连接层进行分类。  
在Fast R-CNN中，作者提出了一个叫做**ROI
Pooling的网络层**，这个网络层可以把不同大小的输入映射到一个固定尺度的特征向量。ROI
Pooling层将每个候选区域均匀分成M×N块，对每块进行max
pooling。将特征图上大小不一的候选区域转变为大小统一的数据，送入下一层。这样虽然输入的图片尺寸不同，得到的feature
map（特征图）尺寸也不同，但是可以加入这个神奇的ROI
Pooling层，对每个region都提取一个固定维度的特征表示，就可再通过正常的softmax进行类型识别。  


![https://static.oschina.net/uploads/space/2018/0331/144419_hVfy_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/8750beae8738ea2ded4a09c693a59834.png)

2、在分类回归阶段，在R-CNN中，先生成候选框，然后再通过CNN提取特征，之后再用SVM分类，最后再做回归得到具体位置（bbox
regression）。而在Fast R-CNN中，作者巧妙的把最后的bbox
regression也放进了神经网络内部，与区域分类合并成为了一个multi-task模型，如下图所示：  


![https://static.oschina.net/uploads/space/2018/0331/144431_NbDC_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e1bae802bcf736ebcc1e534c136c1241.png)

   
实验表明，这两个任务能够共享卷积特征，并且相互促进。

Fast R-CNN很重要的一个贡献是成功地让人们看到了Region
Proposal+CNN（候选区域+卷积神经网络）这一框架**实时检测的希望**，原来多类检测真的可以在保证准确率的同时提升处理速度。

## 2.3 Faster R-CNN更快更强   ##
继2014年推出R-CNN，2015年推出Fast R-CNN之后，目标检测界的领军人物Ross
Girshick团队在2015年又推出一力作：Faster
R-CNN，使简单网络目标检测速度达到17fps，在PASCAL
VOC上准确率为59.9%，复杂网络达到5fps，准确率78.8%。  
在Fast R-CNN还存在着瓶颈问题：Selective
Search（选择性搜索）。要找出所有的候选框，这个也非常耗时。那我们有没有一个更加高效的方法来求出这些候选框呢？  
在Faster
R-CNN中加入一个**提取边缘的神经网络**，也就说找候选框的工作也交给神经网络来做了。这样，目标检测的四个基本步骤（候选区域生成，特征提取，分类，位置精修）终于被统一到一个深度网络框架之内。如下图所示：  


![https://static.oschina.net/uploads/space/2018/0331/144441_YVAg_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/68a48794fe6fd0d1c9fb7921f34db0e8.png)

   
Faster R-CNN可以简单地看成是“区域生成网络+Fast
R-CNN”的模型，用区域生成网络（Region Proposal Network，简称RPN）来代替Fast
R-CNN中的Selective Search（选择性搜索）方法。  
如下图  


![https://static.oschina.net/uploads/space/2018/0331/144447_Pe7l_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/e62e0a2070f1ca7c1c151d311ba3cd6a.png)

   
RPN如下图：  


![https://static.oschina.net/uploads/space/2018/0331/144456_Vava_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/df1fba60bf7ffe140a15d29824e1968f.png)

   
RPN的工作步骤如下：  
- 在feature map（特征图）上滑动窗口  
- 建一个神经网络用于物体分类+框位置的回归  
- 滑动窗口的位置提供了物体的大体位置信息  
- 框的回归提供了框更精确的位置

Faster R-CNN设计了提取候选区域的网络RPN，代替了费时的Selective
Search（选择性搜索），使得检测速度大幅提升，下表对比了R-CNN、Fast R-CNN、Faster
R-CNN的检测速度：  


![https://static.oschina.net/uploads/space/2018/0331/144503_nUf1_876354.png](https://raw.githubusercontent.com/iqiy/Mat-Lib/master/ed9a53a1a4c5c60369c809838a420dc1.png)

总结  
R-CNN、Fast R-CNN、Faster
R-CNN一路走来，基于深度学习目标检测的流程变得越来越精简、精度越来越高、速度也越来越快。基于region
proposal（候选区域）的R-CNN系列目标检测方法是目标检测技术领域中的最主要分支之一。

