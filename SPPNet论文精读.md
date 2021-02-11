Idea来自spatial pyramid matching or SPM.

SPPnet取得了102x faster than RCNN方法的速度。SPP和RCNN已经不一样了，我们不再是所有的都输入，而是直接提取feature map啦+然后SPP+然后FC

重点就是SPPNet对于精度利用不到位，因为RCNN提出的proposal属于CNN的finetuning的方法对于SPP这种完全不靠谱。因为我们改区域了。

## 我们主要就是针对为什么CNN网络需要fixed-size输入的问题，我们就是在cnn中的最后一层CNN使用一个SPP layer，然后将其输入至全连接层。
SPP使用的就是multi-level spatial bins。

这里的multi-level pooling对于object transformations而言是很鲁棒的。

## 1.SPP的好处

首先是图像分类阶段:（同时也是引入多尺度信息的一种方式）

1.很大程度上允许了我们再训练的时候接受不同的大小的图像的输入的训练。这就会增加scale-invariance和reduce 过拟合。

2.就是训练方法就是为了a single network to接受multiple variable input sizes，我们就是approximate it by 多个网络@share all参数的。
而每个网络都是针对一个fixed input size的。

3.这里就是一个图像大小的为一个epoch，然后转换其到另一个图像大小的作为下一个epoch，这样有更好的测试泛化性能。

4.在ImageNet2012数据集上，我们的实验结果证明我们的SPPNet超过了4种不同的CNN architectures over the non-SPP counterparts。

然后是目标检测阶段: 

1.RCNN这里很耗时，因为就是candidate windows都会通过CNN来抽取feature。这里就是repeatedly的过程，真的很耗时。

2.那么我们的SPPnet就是在整张图像上只运行一次。@100倍加速。

3.
