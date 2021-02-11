# Rich feature hierarchies for accurate object detection and semantic segmentation@2014

region-proposals with CNN所以被当作是RCNN

## 组成模块
1.category-independent region proposals@selective search；

2.一个大的卷积神经网络@针对每个区域都会抽取对应的特征；

3.一堆class-specific线性SVMs.

![](RCNN.png)

### 训练阶段（multi-stage training）
1.Domain-specific finetuning: 这里就是将原始的CNN的ImageNet的全连接层是随机初始化的。(1000-way classification layer变成N+1-way classification layer. 其中N是number of object类，然后还有一个是对应的background。
那么这里我们就是使用selective search获得了一堆region proposals，然后使用与ground truth:IoU大于等于0.5来将bbx标记为positive，其余是negative。然后每次训练CNN参数的时候我们都是使用32正windows和96个bg windows。（mini-batch）。

这里就是我们bias the sampling towards positive windows because 它们比起background而言是稀少的。

2.Object category 分类器: 我们对于正负例最难判断的事IoU 0.3来区分只是部分接近的proposal，然后就是选定了阈值之后，使用一个linear SVM per class来进行分类。

核心:为什么我们对于训练阶段搞CNN和SVM选择不同的positive和negative呢？比如对于CNN则是IoU大于0.5的都是positive，然后其余都是negative。SVM则是只使用ground-truth boxes作为positive examples对于特定的class，然后label proposal少于0.3IoU的作为negative@@hard negative。然后其余的不care。

这里主要的就是SVM的训练不在于finetuning，而且这么设置训练的结果不如CNN的训练的结果。CNN同理，这么设置取得了最好的结果。

原因分析:
```
1.对于finetuning而言事实上data是limited的@如果使用SVM的训练设置（hard examples）。这种jitter（0。5-1）可以看作是一种抑制过拟合的手段，扩大了positive examples @30倍。

2.这种大的set对于finetuning是很有意义的。

```

### 关于为什么使用SVM和CNN，而不是单纯CNN结束。
1.这个表现变差了。54-》50，这里很大的可能是因为咱们之前定义在finetuning的positive examples本身也不是准确的定位。以及咱们的softmax分类器对应的negative examples也不是hard negative。

2.当然如果能改进CNN的finetuning说不定是一个改进整个套路的过程。

### 当然这里我们也使用了回归器。

### 测试detection
这里的核心就是我们对于每张测试图像使用selective search获取大约2000个region proposals，然后我们warp each proposal并且使用CNN来获取对应的特征。然后对于每个class，我们使用SVM来进行score的ranking。然后对于每个class，我们使用NMS来抑制掉IoU overlap with a higher scoring region的proposal。
