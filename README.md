## PSPnet：Pyramid Scene Parsing Network目标检测模型在Keras当中的实现
---

### 目录
1. [所需环境 Environment](#所需环境)
2. [注意事项 Attention](#注意事项)
3. [文件下载 Download](#文件下载)
4. [预测步骤 How2predict](#预测步骤)
5. [训练步骤 How2train](#训练步骤)
6. [miou计算 miou](#miou计算)
7. [参考资料 Reference](#Reference)

### 性能情况
| 训练数据集 | 权值文件名称 | 测试数据集 | 输入图片大小 | mIOU | 
| :-----: | :-----: | :------: | :------: | :------: | 
| VOC12+SBD | [pspnet_mobilenetv2.h5](https://github.com/bubbliiiing/pspnet-tf2/releases/download/v1.0/pspnet_mobilenetv2.h5) | VOC-Val12 | 473x473| 71.04 | 
| VOC12+SBD | [pspnet_resnet50.h5](https://github.com/bubbliiiing/pspnet-tf2/releases/download/v1.0/pspnet_resnet50.h5) | VOC-Val12 | 473x473| 79.92 | 

### 所需环境
tensorflow-gpu==2.2.0

### 注意事项
代码中的pspnet_mobilenetv2.h5和pspnet_resnet50.h5是基于VOC拓展数据集训练的。训练和预测时注意修改backbone。    

### 文件下载
训练所需的pspnet_mobilenetv2.h5和pspnet_resnet50.h5可在百度网盘中下载。    
链接: https://pan.baidu.com/s/1cmCtnJnyAcREoXmcbmvenA 提取码: pf5b   
VOC拓展数据集的百度网盘如下：  
链接: https://pan.baidu.com/s/1BrR7AUM1XJvPWjKMIy2uEw 提取码: vszf    
### 预测步骤
#### 1、使用预训练权重
a、下载完库后解压，如果想用backbone为mobilenet的进行预测，直接运行predict.py就可以了；如果想要利用backbone为resnet50的进行预测，在百度网盘下载pspnet_resnet50.h5，放入model_data，修改pspnet.py的backbone和model_path之后再运行predict.py，输入。  
```python
img/street.jpg
```
可完成预测。    
b、利用video.py可进行摄像头检测。    
#### 2、使用自己训练的权重
a、按照训练步骤训练。    
b、在pspnet.py文件里面，在如下部分修改model_path和backbone使其对应训练好的文件；**model_path对应logs文件夹下面的权值文件，backbone是所使用的主干特征提取网络**。    
```python
_defaults = {
    "model_path"        : 'model_data/pspnet_mobilenetv2.h5',
    "backbone"          : "mobilenet",
    "model_image_size"  : (473, 473, 3),
    "num_classes"       : 21,
    "downsample_factor" : 16,
    "blend"             : False,
}
```
c、运行predict.py，输入    
```python
img/street.jpg
```
可完成预测。    
d、利用video.py可进行摄像头检测。    

### 训练步骤
#### 1、训练voc数据集
1、将我提供的voc数据集放入VOCdevkit中（无需运行voc2pspnet.py）。  
2、在train.py中设置对应参数，默认参数已经对应voc数据集所需要的参数了，所以只要修改backbone和model_path即可。  
3、运行train.py进行训练。  

#### 2、训练自己的数据集
1、本文使用VOC格式进行训练。  
2、训练前将标签文件放在VOCdevkit文件夹下的VOC2007文件夹下的SegmentationClass中。    
3、训练前将图片文件放在VOCdevkit文件夹下的VOC2007文件夹下的JPEGImages中。    
4、在训练前利用voc2pspnet.py文件生成对应的txt。    
5、在train.py文件夹下面，选择自己要使用的主干模型和下采样因子。本文提供的主干模型有mobilenet和resnet50。下采样因子可以在8和16中选择。需要注意的是，预训练模型需要和主干模型相对应。  
6、注意修改train.py的num_classes为分类个数+1。  
7、运行train.py即可开始训练。  

### miou计算
参考miou计算视频和博客。  

### Reference
https://github.com/ggyyzm/pytorch_segmentation  
https://github.com/bonlime/keras-deeplab-v3-plus
