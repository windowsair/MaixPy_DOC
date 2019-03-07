sensor
=========
传感器模块，进行摄像头配置及图像抓取等，用于控制开发板摄像头完成摄像任务。
## 方法

### 单目摄像头重置函数

重置并初始化摄像头。这里会自动扫描并获取摄像头地址
```python
import sensor
sensor.reset() #初始化单目摄像头
```

#### 参数

无

#### 返回值

无

### 双目摄像头重置函数

芯片只有一个dvp接口，所以通过pwdn引脚来选择sensor。pwdn引脚可以通过shutdown接口来控制。指定sensor后其余操作不变。详细请见例程2

```python
import sensor
sensor.binocular_reset()#初始化单目摄像头
```

#### 参数

无

#### 返回值

无


### 启动函数

启动/关闭芯片捕获图像

```
import sensor
sensor.run(enbale)
```

#### 参数

* enbale: 1表示开启，0 表示停止

#### 返回值

* return: 返回1

### 设置帧大小

用于设置摄像头输出帧大小，k210最大支持VGA格式，大于VGA将无法获取图像。
MaixPy开发板配置的屏幕是320*240分辨率，推荐设置为QVGA格式

```
sensor.set_framesize(framesize)
```

#### 参数

* `framesize`: 帧大小

#### 返回值

* `True` : 设置成功
* `False`: 设置错误

### 设置帧格式

用于设置摄像头输出格式，k210支持rgb565和yuv422格式。MaixPy开发板配置的屏幕是使用rgb565设置，推荐设置为RGB565格式

```
sensor.set_pixformat(format)
```
#### 参数

* `format`: 帧格式

#### 返回值

* `True` : 设置成功
* `False`: 设置错误

### 开始图像捕捉

开启图像捕捉功能

```
sensor.run(enable)
```
#### 参数

* `enable`: 1 表示开始抓取图像 0 表示停止抓取图像

#### 返回值

* `True` : 设置成功
* `False`: 设置错误


### 获取图像

控制摄像头捕捉图像

```
img = sensor.snapshot()
```
#### 参数

无

#### 返回值

* `img`: 返回的图像对象

### 关闭摄像头

关闭摄像头/切换摄像头

```
sensor.shutdown(enable/select)
```
#### 参数

单目摄像头
* `enable`: 1 表示开启摄像头 0 表示关闭摄像头

双目摄像头
* `select`: 通过写入0或1来切换摄像头

#### 返回值

无

### 跳帧

跳过指定帧数或者跳过指定时间内的图像

```
sensor.skip_frames([n,time])
```
#### 参数

* `n`: 跳过 n 帧图像

* `time`: 跳过指定时间，单位为ms


#### 返回值

无

### 分辨率宽度

获取摄像头分辨率宽度

```
sensor.width()
```
#### 参数

无

#### 返回值

* `int`类型的摄像头分辨率宽度



### 分辨率高度 

```
sensor.height()
```
#### 参数

无

#### 返回值

* `int`类型的摄像头分辨率高度

### 获取帧缓冲

获取当前帧缓冲区

```
sensor.get_fb()
```
#### 参数

无

#### 返回值

* `image`类型的对象

### 获取ID

获取当前摄像头ID

```
sensor.get_id()
```
#### 参数

无

#### 返回值

* `int`类型的ID

### 设置彩条模式

将摄像头设置为彩条模式

```
sensor.set_colorbar(enable)
```
#### 参数

* `enable`: 1 表示开启彩条模式 0 表示关闭彩条模式

#### 返回值

无

### 设置对比度

设置摄像头对比度

```
sensor.set_contrast(contrast)
```
#### 参数

* `constrast`: 摄像头对比度，范围为[-2,+2]

#### 返回值

* `True` : 设置成功
* `False`: 设置错误

### 设置亮度

设置摄像头亮度

```
sensor.set_brightness(brightness)
```
#### 参数

* `constrast`: 摄像头亮度，范围为[-2,+2]

####  返回值

* `True` : 设置成功
* `False`: 设置错误

### 设置饱和度

设置摄像头饱和度

```
sensor.set_saturation(saturation)
```
#### 参数

* `constrast`: 摄像头饱和度，范围为[-2,+2]

#### 返回值

* `True` : 设置成功
* `False`: 设置错误

### 设置自动增益

设置摄像自动增益模式

```
sensor.set_auto_gain(enable,gain_db)
```

#### 参数

* `enable`: 1 表示开启自动增益 0 表示关闭自动增益
* `gain_db`: 关闭自动增益时，设置的摄像头固定增益值，单位为db


#### 返回值

无

### 获取增益值

获取摄像头增益值

```
sensor.get_gain_db()
```

#### 参数

无

#### 返回值

* `float`类型的增益值

### 设置水平镜像

设置摄像头水平镜像

```
sensor.set_hmirror(enable)
```

#### 参数

* `enable`: 1 表示开启水平镜像 0 表示关闭水平镜像

#### 返回值

无

### 写入寄存器

往摄像头寄存器写入指定值

```
sensor.__write_reg(address, value)
```

#### 参数

* `address`: 寄存器地址
* `value`  ： 写入值

#### 返回值

无

### 读取寄存器

读取摄像头寄存器值

```
sensor.__read_reg(address)
```

#### 参数

* `address`: 寄存器地址

#### 返回值

* `int`类型的寄存器值

## 例程


### 例程 1 

```python
import sensor	
import lcd
lcd.init()
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
while 1:
    img = sensor.snapshot()
    lcd.display(img)
```

### 例程 2

```python
import sensor
import image
import lcd
import time
lcd.init()
sensor.binocular_reset()
sensor.shutdown(False)#选择sensor并初始化
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.shutdown(True)#选择sensor并初始化
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
while True:
    sensor.shutdown(False) #选择sensor
    img=sensor.snapshot()
    lcd.display(img)
    time.sleep_ms(100)
    sensor.shutdown(True) #选择sensor
    img=sensor.snapshot()
    lcd.display(img)
    time.sleep_ms(100)
```