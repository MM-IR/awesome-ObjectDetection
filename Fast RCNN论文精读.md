## 摘要总结
1.9倍的训练速度@比起RCNN

2.213倍的测试速度@比起RCNN

3.在PASCAL VOC 2012上面取得了更高的mAP。

1.比如SPPnet，训练VGG16三倍的速度。

2.测试10倍速度@SPPnet

3.更准确。

## 总结: 之前方法的缺陷

### compared with RCNN:

1.训练是一个Multi-stage pipeline. 这里就是ConvNet on object proposals using log loss。然后用SVM来进ConvNet features。

1）finetuning；2）SVM；3）然后bbx regressors。

2.训练是expensive in space and time。所有的关于SVM的我们都需要送到disk里，而这个就是消耗hundreds of gigatypes of storage.

3.Object Detection is slow: 测试时间对于RCNN而言，features are extracted from each object proposals所以一般VGG16需要耗时47s/Image。

### compared with SPPNet：
这个比起RCNN就是10到100的测试速度+++，然后训练时间就是减少了3倍的时间@@faster proposal feature extraction。

1.这里的一个核心的问题就是微调并不能更新卷积layers precede spatial pyramid pooling（不能利用好之前说的conv finetuning的过程）

## 我们方法的contributions
Fast-RCNN就是说comparatively fast to训练以及测试～

1.更高的检测质量mAP 比起RCNN/SPPNet。

2.训练是single-step的，使用的是multi-task loss.

3.训练可以更新所有的网络layers。

4.对于特征缓存没有任何disk storage是需要的。feature caching。

## Fast R-CNN架构以及训练方式
1.网络
