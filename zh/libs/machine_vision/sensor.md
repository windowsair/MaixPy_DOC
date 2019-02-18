sensor
=========
传感器模块，进行摄像头配置及图像抓取等，用于控制开发板摄像头完成摄像任务

## 1. 重置函数

重置并初始化摄像头。这里会自动扫描并获取摄像头地址
```
import sensor
sensor.reset()
```

#### 1.1. 参数

无

#### 1.2. 返回值

无

## 2. 启动函数

启动摄像头

```
import sensor
sensor.run(enbale)
```

#### 2.1. 参数

* enbale: 1表示开启，0 表示停止

#### 2.2. 返回值

* return: 返回1

## 2. 设置帧大小

用于设置摄像头输出帧大小，k210最大支持VGA格式，大于VGA将无法获取图像。
MaixPy开发板配置的屏幕是320*240分辨率，推荐设置为QVGA格式

```
sensor.set_framesize(framesize)
```

#### 2.1. 参数

`framesize`: 帧大小

#### 2.2. 返回值

`True` : 设置成功
`False`: 设置错误

### 3. 设置帧格式

用于设置摄像头输出格式，k210支持rgb565和yuv422格式。MaixPy开发板配置的屏幕是使用rgb565设置，推荐设置为RGB565格式

```
sensor.set_pixformat(format)
```
#### 3.1. 参数

`format`: 帧格式

#### 3.2. 返回值

`True` : 设置成功
`False`: 设置错误

### 4. 开始图像捕捉

开启图像捕捉功能

```
sensor.run(enable)
```
#### 4.1. 参数

`enable`: 1 表示开始抓取图像 0 表示停止抓取图像

#### 4.2. 返回值

`True` : 设置成功
`False`: 设置错误


### 5. 获取图像

控制摄像头捕捉图像

```
img = sensor.snapshot()
```
#### 5.1. 参数

无

#### 5.2. 返回值

`img`: 返回的图像对象

### 6. 关闭摄像头

关闭摄像头

```
sensor.shutdown(enable)
```
#### 6.1. 参数

`enable`: 1 表示开启摄像头 0 表示关闭摄像头

#### 6.2. 返回值

无

### 7. 跳帧

跳过指定帧数或者跳过指定时间内的图像

```
sensor.skip_frames([n,time])
```
#### 7.1. 参数

`n`: 跳过 n 帧图像

`time`: 跳过指定时间，单位为ms


#### 7.2. 返回值

无

### 8. 分辨率宽度

获取摄像头分辨率宽度

```
sensor.width()
```
#### 8.1. 参数

无

#### 8.2. 返回值

`int`类型的摄像头分辨率宽度



### 9. 分辨率高度 

```
sensor.height()
```
#### 9.1. 参数

无

#### 9.2. 返回值

`int`类型的摄像头分辨率高度

### 10.获取帧缓冲

获取当前帧缓冲区

```
sensor.get_fb()
```
#### 10.1. 参数

无

#### 10.2. 返回值

`image`类型的对象

### 11.获取ID

获取当前摄像头ID

```
sensor.get_id()
```
#### 11.1. 参数

无

#### 11.2. 返回值

`int`类型的ID

### 12.设置彩条模式

将摄像头设置为彩条模式

```
sensor.set_colorbar(enable)
```
#### 12.1. 参数

`enable`: 1 表示开启彩条模式 0 表示关闭彩条模式

#### 12.2. 返回值

无

### 13.设置对比度

设置摄像头对比度

```
sensor.set_contrast(contrast)
```
#### 13.1. 参数

`constrast`: 摄像头对比度，范围为[-2,+2]

#### 13.2. 返回值

`True` : 设置成功
`False`: 设置错误

### 14.设置亮度

设置摄像头亮度

```
sensor.set_brightness(brightness)
```
#### 14.1. 参数

`constrast`: 摄像头亮度，范围为[-2,+2]

#### 14.2. 返回值

`True` : 设置成功
`False`: 设置错误

### 15.设置饱和度

设置摄像头饱和度

```
sensor.set_saturation(saturation)
```
#### 15.1. 参数

`constrast`: 摄像头饱和度，范围为[-2,+2]

#### 15.2. 返回值

`True` : 设置成功
`False`: 设置错误

### 16.设置自动增益

设置摄像自动增益模式

```
sensor.set_auto_gain(enable,gain_db)
```

#### 16.1. 参数

`enable`: 1 表示开启自动增益 0 表示关闭自动增益
`gain_db`: 关闭自动增益时，设置的摄像头固定增益值，单位为db


#### 16.2. 返回值

无

### 17.获取增益值

获取摄像头增益值

```
sensor.get_gain_db()
```

#### 17.1. 参数

无

#### 17.2. 返回值

`float`类型的增益值

### 18.设置水平镜像

设置摄像头水平镜像

```
sensor.set_hmirror(enable)
```

#### 18.1. 参数

`enable`: 1 表示开启水平镜像 0 表示关闭水平镜像

#### 18.2. 返回值

无

### 19. 写入寄存器

往摄像头寄存器写入指定值

```
sensor.__write_reg(address, value)
```

#### 19.1. 参数

`address`: 寄存器地址
`value`  ： 写入值

#### 19.2. 返回值

无

### 20. 读取寄存器

读取摄像头寄存器值

```
sensor.__read_reg(address)
```

#### 19.1. 参数

`address`: 寄存器地址

#### 19.2. 返回值

`int`类型的寄存器值

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