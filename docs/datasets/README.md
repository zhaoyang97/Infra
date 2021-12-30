

## 简介

本项目旨在减少收集各种数据集的时间成本，提供各种数据集的配置文件及其获取方式，数据集已开放在Titan_Xp_4的目录`/root/commonfile/data`
* 可在mmlab系列框架上使用
* 如果用其他框架，任然可以使用本项目的提供的方式[获取数据集](#get_dataset)


目前已支持以下数据集：

分类
- [x] imagenet

目标检测
- [x] coco
- [x] voc
- [x] tct 30000
- [x] tct
- [x] tianchi tct

语义分割
- [x] ade20k
- [x] cityscapes
- [x] pascal context
- [x] ddr
- [x] idrid


## 使用方法
以mmdetection为例，介绍配置coco数据集的方式：

### 1. 添加配置文件
将[coco_detection.py](detection/coco_detection.py)放到目录pathto/mmdetection/configs/\_base\_/datasets/下

### 2. 在mmdetection的同级目录下创建data文件夹

```plain
pathto
├── data
├── mmdetection
│   ├── mmdet
│   ├── tools
│   ├── configs
│   │   ├── _base_
│   │   │   ├── datasets
```

### 3. 传数据集到data目录
从xp4上获取coco数据集
```plain
scp -P your_port -r root@202.197.66.62:/root/commonfile/data/coco pathto/data/
```
把your_port换成Titan_Xp_4账号对应的端口，pathto/data/换成第二步中新建的data文件夹目录。<span id="get_dataset"></span>

<!---
// cd到data目录，然后执行
scp -P 44120 -r root@202.197.66.62:/root/commonfile/data/coco ./
-->

## 获取数据集
除tct相关数据集外，其余数据集均可通过以下命令获取：
```plain
scp -P your_port -r root@202.197.66.62:/root/commonfile/data/dataset_floder pathto/data/
```
把your_port换成Titan_Xp_4账号对应的端口，pathto/data/换成第二步中新建的data文件夹目录，dataset_floder换成数据集的文件夹名，各个数据集对应的文件夹名如下：
<br>数据集：对应的dataset_floder
* imagenet: imagenet
* coco: coco
* voc: voc0712
* tct 30000: TCT_30000
* ade20k: ade20k
* cityscapes：cityscapes
* pascal context：voc0712
* ddr：DDR  
* idrid: IDRID


## 获取tct数据集

### 实验室的tct数据集
tct的小数据集tct 30000可以通过上一节提供的方式获取，完整的tct数据集获取方式如下：
```plain
scp -P your_port -r root@202.197.66.62:/root/commonfile/data/TCTAnnotatedData pathto/data/
```

### 阿里天池tct数据集
```plain
scp -P 19230 -r root@122.207.82.55:/root/userfolder/tianchi/zipdata pathto/data/
```
天池数据集在rtx2080 03，[@冯硕](https://github.com/FengShuo96)的个人账号中，密码请咨询冯硕。


## 说明

### DDR&IDRiD
提供的DDR和IDRiD数据集是做过数据增广后的数据集，包括旋转90°、180°、270°、水平翻转、垂直翻转，算上原图一共是6倍。<br>
这两个数据集是自定义的数据集，如使用mmlab系列框架，需要注册数据集，流程如下：<br>
将[lesion_dataset.py](https://github.com/puzzledsky/mmsegmentation-lesion/blob/master/mmseg/datasets/lesion_dataset.py)和[custom.py](https://github.com/puzzledsky/mmsegmentation-lesion/blob/master/mmseg/datasets/custom.py)添加到目录`mmsegmentation/mmseg/datasets`，并在该目录下的\_\_init\_\_.py文件中添加`from .lesion_dataset import LesionDataset`, `__all__`中添加'LesionDataset'.

### imagenet
imagenet数据集默认的是8卡、每卡32图，学习率=0.01。
使用不同的batchsize，学习率应该按照以下公式设置：<br>
lr = 0.01 \* [卡数] \* [每卡图像] / (8\*32). <br>
例如用8卡、每卡64图，那么学习率应该设为0.01/2=0.005。


## 致谢
感谢[@刘浩天](https://github.com/puzzledsky)提供的DDR和IDRiD数据集的注册文件<br>
感谢[@冯硕](https://github.com/FengShuo96)提供的阿里天池tct数据集和简化后的TCT_30000数据集<br>



<!---
## 说明
可能需要修改的地方：
每卡的图像改这个参数：samples_per_gpu

## 注意事项

### 学习率的设置

#### imagenet

### tct
tct数据集只提供了图像，注释文件需要自己放入对应目录。
TODO 收集tct的注释文件
-->
