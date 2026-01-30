---
title: yolo初学记录1
date: 2026-01-30 15:21:32
tags:
- yolo
---



##### 最近接触了不少的关于自动化的操作，包括不限制dm,乐玩（易语言），pythonautogui这些，在我对游戏进行图像检测并且开始投入使用的时候经常发现图色在游戏检测的严格性，即图色检测必须有百分比的相同性，不可旋转 
以下代码均以python为例

## 介绍
Yolo(You Only Look Once)是一种one-stage目标检测算法，即仅需要 **“看”** 一次就可以识别出图片中物体的class类别和边界框。**Yolov8**是Ultralytics公司最新推出的[Yolo系列](https://so.csdn.net/so/search?q=Yolo%E7%B3%BB%E5%88%97&spm=1001.2101.3001.7020)目标检测算法，可以用于图像分类、物体检测和实例分割等任务。（百度复制）
## pip
miniconda
ultralytics
## 学习
yolo 
1. 参数
	1. source：使用模型 yolo11nt.pt
	2. task：task指的是要执行的具体任务类型，是以下之一 `[detect, segment, classify, pose, obb]`。如果未明确传递，YOLO 将尝试推断 `TASK`（可选）
	3. mode：mode则指的是操作的模式，如训练、预测或验证，是以下之一 `[train, val, predict, export, track, benchmark]`（必需）默认为predict
```python
from ultralytics import YOLO
yolo = YOLO("yolo.pt",task="decect")
# 都是可以的 一样的 因为yolo会自己判断需要怎么做 基本上默认都是detect
yolo = YOLO("yolo.pt")
# 模型
```
### 训练 train
1. 先自己准备好自己要训练的图片
2. 准备好标注软件 这里我使用的是[labelImg](https://github.com/HumanSignal/labelImg)
3. 准备好目录
```bash
datasets
		|_ icon
					|_ img
					|		|__ train
					|		|__ val
					|_ label
							|__ train
							|__ val
需要进行训练的 放在 img>train
需要训练完毕进行验证 放在 img>val 注意 这里的文件图片不能一样 否则无法正确判断你的训练是否足够
labelimg标注完毕的标签也要分开放
放在训练对应的图片标签也要相应的放在 label>train 中
放在训练完毕需要验证的图片对应放在 label>val 中
```

```python
from ultralytics import YOLO
yolo = YOLO("yolo.pt")
yolo.train(data="yolo.yaml",epochs=300,workers=0,batch=16) #开始运行之后就可以正常的训练了！
# 如果电脑出现问题导致中断，以下可以进行按照原先进行继续训练
# 出了错 会在runs目录中生成一个最新的train文件夹 在该文件夹中有两个pt文件 分别为
# 1. best.pt  为截止到结束时训练最好的模型
# 2. last.pt	为截止的时候出现最后一个模型
# 我们采用截止的时候最后一个模型进行继续训练
yolo = YOLO("runs/train/last.pt")
yolo.train(resume=True)

# train的参数
# data (str): 你要训练的文件
# epochs (int):你要训练的轮次 一般越大越好 具体得看你提供多少张图片 图片少 轮次大 则会提前训练我那比 轮次少图片多 则训练完也不会有很好的效果 建议300张图片 300轮+循环  
# batch_size (int): Batch size for training.  （未知）
# imgsz (int): Input image size.  （未知）
# device (str): Device to run training on (e.g., 'cuda', 'cpu'). 选择cpu还是gpu 建议无脑cpu得了 
# workers (int): Number of worker threads for data loading.   （未知）
# optimizer (str): Optimizer to use for training.  （未知）
# lr0 (float): Initial learning rate.  （未知）
# patience (int): Epochs to wait for no observable improvement for early stopping of training.（未知）
```
#### yaml详解
在训练的时候需要提前准备好自己的yaml文件（该文件是官方有自己详细的文本的可以照抄改改路径即可 可以在**ultralytics/cfg/datasets**中去寻找不少官方自己提供的yaml文件）
``` yaml
path: ../datasets/african-wildlife # 这里填写你的文件夹 即./datasets/icon 
train: train/images # 要训练的图片的文件夹路径 path/img/train
val: valid/images # 要验证的图片的文件夹路径 path/img/val
# test:  可以为空没事 
  
# Classes  这里是你用了多少个标签 一定要按照你标签的顺序 
#在labelimg中data文件夹里会有一个classes.txt的文本记录了你当前已经标注好的标签的顺序 按顺序依次排列
#因为在标记好的文本标签中会记录你当前图片是使用了哪一个序号进行的标记,如果这里对照有误则在训练的时候就也会出错！！
names:   
  0: buffalo  
  1: elephant  
  2: rhino  
  3: zebra  
```
在训练完毕之后 可以检查runs/train目录中的图片
### 预测 predict
**该模型即为当前yolo核心之一的功能**（我认为的 另一个就是训练train 和导出）
可以对图像进行预测 并且给出中心点 坐标点 标记 可信度等等内容
![输入图片说明](/imgs/2026-01-29/wJ509I0jNUCe0mZe.png)
左边为原图，右边则为已处理过的图片
这里并没有什么值得额外说明的
```bash
# 参数说明
#source (str | Path | int | PIL.Image | np.ndarray | torch.Tensor | List | Tuple) 
# 文件路径/包含文件路径的数组or字典
#Returns:  
# (List[ultralytics.engine.results.Results])
# 返回一个数组 包含所有信息
print(result[0].boxes) # boxes: ultralytics.engine.results.Boxes object 
```
```python
yolo = YOLO("best.pt")  # 使用刚刚训练好的pt文件
result = yolo.predict("1.jpg",save=True)
# result = yolo("1.jpg",save=True) # 这里是有默认的 yolo默认使用predict 
# source 1.jpg 这里的参数可以有很多 可以是一段视频 如1.mp4 可以是当前电脑的屏幕 = screen 也可以是当前摄像头 =0 如果要使用指定窗口那就得设计到窗口句柄的绑定问题了
# save 意义为是否保存该次的识别内容
# show=True            # 持续展示
# save=True,           # 保存结果
# save_txt=True,       # 保存标签文件
# save_conf=True,      # 保存置信度
# line_width=2,        # 边界框线宽
# stream=True,         # 流模式（减少延迟）
# classes=[0, 2]       # 只检测人(0)和车(2)
keypoints: None
masks: None
names: {0: 'person',1: 'car'}
obb: None
orig_img: array([[[131, 132, 128],
        [131, 132, 128],
        [134, 135, 131],
        ...,
        [139, 140, 131],
        [134, 135, 126],
        [127, 128, 119]],

       [[131, 132, 128],
        [126, 127, 123],
        [131, 132, 128],
        ...,
        [135, 136, 127],
        [130, 131, 122],
        [123, 124, 115]],

       [[130, 131, 127],
        [129, 130, 126],
        [132, 133, 129],
        ...,
        [130, 132, 126],
        [128, 130, 124],
        [124, 126, 120]],

       ...,

       [[106, 113, 110],
        [103, 110, 107],
        [101, 108, 105],
        ...,
        [149, 154, 152],
        [138, 143, 141],
        [118, 124, 119]],

       [[103, 108, 106],
        [101, 106, 104],
        [100, 105, 103],
        ...,
        [147, 152, 150],
        [137, 142, 140],
        [137, 143, 138]],

       [[101, 106, 104],
        [101, 106, 104],
        [101, 106, 104],
        ...,
        [150, 155, 153],
        [126, 131, 129],
        [134, 140, 135]]], shape=(206, 239, 3), dtype=uint8)
orig_shape: (206, 239)
path: 'C:\\Users\\Admin\\Desktop\\ultralytics-8.3.55\\img_1.jpg'
probs: None
save_dir: 'runs\\detect\\predict39'
speed: {'preprocess': 3.3502578735351562, 'inference': 195.32537460327148, 'postprocess': 0.9987354278564453}]
```
#### 结果
```python
print(result[0].boxes)
# 数组内容为 boxes()
# boxes 中包含 name(当前pt包含的所有标签,这里的返回结果和上图的不是同一个模型，所以标签没有truck) orig_img(该图片/帧的像素的BGR) path(当前文件路径) save_dir(当前保存的文件目录) 
# cls 包含了当前帧/图片识别到的所有标签的序号(对应了name中的序号顺序)
# conf 分别对应识别出该序号的可信度。
# xywh 则代表该识别出来的 左上角的 x y 以及对应的 w(宽度width)，h（高度height）
# xyxy 则是四个顶点的上左右下点位
print(result[0].boxes.cls) #可以进行输出
cls: tensor([0., 9., 0., 2., 2., 7., 2., 5., 2.])
conf: tensor([0.7742, 0.7512, 0.7352, 0.5291, 0.4252, 0.3521, 0.3080, 0.2996, 0.2846])
data: tensor([[4.4409e+02, 3.2972e+02, 4.7539e+02, 4.1390e+02, 7.7422e-01, 0.0000e+00],
        [7.9411e+02, 2.5826e+00, 8.4300e+02, 1.2843e+02, 7.5115e-01, 9.0000e+00],
        [3.6902e+02, 3.2709e+02, 4.0428e+02, 4.1387e+02, 7.3520e-01, 0.0000e+00],
        [7.1047e+02, 3.3675e+02, 9.0717e+02, 3.9940e+02, 5.2906e-01, 2.0000e+00],
        [8.9184e+02, 3.5024e+02, 9.3384e+02, 3.7179e+02, 4.2522e-01, 2.0000e+00],
        [1.0327e+02, 2.9343e+02, 2.6267e+02, 3.6778e+02, 3.5206e-01, 7.0000e+00],
        [9.0379e+02, 3.4303e+02, 1.0236e+03, 3.9628e+02, 3.0800e-01, 2.0000e+00],
        [1.0447e+02, 2.9093e+02, 2.6505e+02, 3.6769e+02, 2.9959e-01, 5.0000e+00],
        [6.6924e+02, 3.5639e+02, 7.3689e+02, 3.8833e+02, 2.8465e-01, 2.0000e+00]])
id: None
is_track: False
orig_shape: (779, 1024)
shape: torch.Size([9, 6])
xywh: tensor([[459.7363, 371.8092,  31.3003,  84.1722],
        [818.5564,  65.5080,  48.8840, 125.8507],
        [386.6507, 370.4808,  35.2653,  86.7721],
        [808.8193, 368.0722, 196.7037,  62.6470],
        [912.8385, 361.0144,  42.0067,  21.5456],
        [182.9665, 330.6063, 159.4020,  74.3427],
        [963.7004, 369.6506, 119.8304,  53.2505],
        [184.7574, 329.3147, 160.5760,  76.7594],
        [703.0674, 372.3641,  67.6462,  31.9407]])
xywhn: tensor([[0.4490, 0.4773, 0.0306, 0.1081],
        [0.7994, 0.0841, 0.0477, 0.1616],
        [0.3776, 0.4756, 0.0344, 0.1114],
        [0.7899, 0.4725, 0.1921, 0.0804],
        [0.8914, 0.4634, 0.0410, 0.0277],
        [0.1787, 0.4244, 0.1557, 0.0954],
        [0.9411, 0.4745, 0.1170, 0.0684],
        [0.1804, 0.4227, 0.1568, 0.0985],
        [0.6866, 0.4780, 0.0661, 0.0410]])
xyxy: tensor([[ 444.0861,  329.7231,  475.3864,  413.8953],
        [ 794.1144,    2.5826,  842.9984,  128.4333],
        [ 369.0181,  327.0947,  404.2834,  413.8668],
        [ 710.4675,  336.7487,  907.1712,  399.3957],
        [ 891.8351,  350.2416,  933.8418,  371.7872],
        [ 103.2655,  293.4350,  262.6675,  367.7776],
        [ 903.7852,  343.0254, 1023.6156,  396.2759],
        [ 104.4694,  290.9350,  265.0454,  367.6944],
        [ 669.2443,  356.3937,  736.8905,  388.3345]])
xyxyn: tensor([[0.4337, 0.4233, 0.4642, 0.5313],
        [0.7755, 0.0033, 0.8232, 0.1649],
        [0.3604, 0.4199, 0.3948, 0.5313],
        [0.6938, 0.4323, 0.8859, 0.5127],
        [0.8709, 0.4496, 0.9120, 0.4773],
        [0.1008, 0.3767, 0.2565, 0.4721],
        [0.8826, 0.4403, 0.9996, 0.5087],
        [0.1020, 0.3735, 0.2588, 0.4720],
        [0.6536, 0.4575, 0.7196, 0.4985]])
```
##### 返回结果的小tip
在识别出一张图片的时候我经常会想，当前图片识别出来是否为空？是否有我想要的内容呢
1. 先判断当前图片识别出来是否为空
其实这个思路还是挺简单的
在返回结果的boxes中的clsz中可以看见当前识别出来的内容都有什么 只需要判断这个值是否为空即可 如果是空说明我就没有识别出任何值
```python
print(result[0].boxes.cls) # tensor([0., 9., 0., 2., 2., 7., 2., 5., 2.])
print(len(result[0].boxes.cls)) # 9 
# 这里可以返回数组长度 = 9 说明识别出来一共九个标签 如果=0 那不就是说明为空 没有识别出来任何值
```
2.  判断识别标签中有没有自己想要的标签
```python
print(result[0].boxes.cls) # tensor([0., 9., 0., 2., 2., 7., 2., 5., 2.])
for i in result[0].boxes.cls:
	if (i==0):
		print(i,"这是你要识别出来的标签") 
# 返回结果
# tensor(0.) 这是你要识别出来的标签
# tensor(0.) 这是你要识别出来的标签
```
## 包
anaconda-anon-usage     0.4.4
archspec                0.2.3
boltons                 23.0.0
Brotli                  1.0.9
certifi                 2024.8.30
cffi                    1.17.1
charset-normalizer      3.3.2
colorama                0.4.6
conda                   24.9.2
conda-content-trust     0.2.0
conda-libmamba-solver   24.9.0
conda-package-handling  2.3.0
conda_package_streaming 0.10.0
cryptography            43.0.0
distro                  1.9.0
frozendict              2.4.2
idna                    3.7
jsonpatch               1.33
jsonpointer             2.1
libmambapy              1.5.8
menuinst                2.1.2
numpy                   2.3.5
packaging               24.1
pip                     24.2
platformdirs            3.10.0
pluggy                  1.0.0
pycosat                 0.6.6
pycparser               2.21
PyQt5                   5.15.11
PyQt5-Qt5               5.15.2
PyQt5_sip               12.18.0
PySocks                 1.7.1
requests                2.32.3
ruamel.yaml             0.18.6
ruamel.yaml.clib        0.2.8
setuptools              75.1.0
tqdm                    4.66.5
truststore              0.8.0
urllib3                 2.2.3
wheel                   0.44.0
win-inet-pton           1.1.0
zstandard               0.23.0



