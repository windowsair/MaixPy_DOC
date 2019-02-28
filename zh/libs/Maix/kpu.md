KPU
=========

KPU是通用的神经网络处理器，它可以在低功耗的情况下实现卷积神经网络计算，时时获取被检测目标的大小、坐标和种类，对人脸或者物体进行检测和分类。

* KPU 具备以下几个特点：
    •   支持主流训练框架按照特定限制规则训练出来的定点化模型
    •   对网络层数无直接限制，支持每层卷积神经网络参数单独配置，包括输入输出通道数目、输入输 出行宽列高
    •   支持两种卷积内核 1x1 和 3x3
    •	支持任意形式的激活函数
    •	实时工作时最大支持神经网络参数大小为 5.5MiB 到 5.9MiB
    •	非实时工作时最大支持网络参数大小为（Flash 容量-软件体积）

## 模块方法

### 加载模型

从flash或者文件系统中加载模型

```
import KPU as kpu
task = kpu.load(offset or file_path)
```

#### 参数

* `offt_set`: 模型在 flash 中的偏移大小，如 `0xd00000` 表示模型烧录在13M起始的地方
* `file_path`: 模型在文件系统中为文件名， 如 `“/sd/xxx.kmodel”`

##### 返回

* `kpu_net`: kpu 网络对象

### 初始化yolo2网络

为yolo2网络模型传入初始化参数

```
import KPU as kpu
task = kpu.load(offset or file_path)
anchor = (1.889, 2.5245, 2.9465, 3.94056, 3.99987, 5.3658, 5.155437, 6.92275, 6.718375, 9.01025)
kpu.init_yolo2(task, 0.5, 0.3, 5, anchor)
```

#### 参数

* `kpu_net`: kpu 网络对象

* `threshold`: 概率阈值

* `nms_value`: box_iou 门限

* `anchor_num`: 锚点数

* `anchor`: 锚点参数与模型参数一致

### 反初始化

```
import KPU as kpu
task = kpu.load(offset or file_path)
kpu.deinit(task)
```

#### 参数

`kpu_net`: kpu_load 返回的 kpu_net 对象


### 运行yolo2网络

```
import KPU as kpu
import image
task = kpu.load(offset or file_path)
anchor = (1.889, 2.5245, 2.9465, 3.94056, 3.99987, 5.3658, 5.155437, 6.92275, 6.718375, 9.01025)
kpu.init_yolo2(task, 0.5, 0.3, 5, anchor)
img = image.Image()
kpu.run_yolo2(task, img)
```

#### 参数

* `kpu_net`: kpu_load 返回的 kpu_net 对象
* `image_t`：从 sensor 采集到的图像

##### 返回

* `list`: kpu_yolo2_find 的列表 

## 例程

```python
import sensor
import image
import lcd
import KPU as kpu

lcd.init()
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
task = kpu.load(0xd00000, 380*1024) #使用kfpkg将 kmodel 与 maixpy 固件打包下载到 flash
anchor = (1.889, 2.5245, 2.9465, 3.94056, 3.99987,
          5.3658, 5.155437, 6.92275, 6.718375, 9.01025)
a = kpu.init_yolo2(task, 0.5, 0.3, 5, anchor)
while(True):
    img = sensor.snapshot()
    code = kpu.run_yolo2(task, img)
    if code:
        for i in code:
            print(i)
            a = img.draw_rectangle(i.rect())
    a = lcd.display(img)
a = kpu.deinit(task)
```
