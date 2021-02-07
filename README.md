# awesome-ObjectDetection
经典的目标检测有关的工作整理

## 1.开山之作: RCNN

### Pros:
第一个将深度学习引入od的工作，大幅度提升性能。

### 技术方法
![](RCNN.png)

这里的selective search就是一种基于图像有关的知识而进行处理的筛选region proposal的方法～
然后就是对这个区域进行缩放@形变，然后CNN feature进行分类/回归。

### 存在的问题Cons:

1. 尺寸归一化导致物体变形，纵横比特征的丢失。

2.重复使用网络计算特征，低实时性@单GPU就很耗时～
