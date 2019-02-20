clock 
========
时钟模块，可以计算在拍摄过程中使用的时间

## 模块方法

### 获取时间戳

获取从开机到现在的时间戳，单位为ms。和 time 模块的 ticks_ms 方法类似

```
import clock
clock.ticks()
```

#### 参数

无

#### 返回值

* 开机到现在的时间戳 ，单位为ms

### 睡眠

让机器睡眠一段时间，单位为ms，和 time 模块的 sleep_ms 方法类似

```
clock.sleep(ms)
```

#### 参数

* `ms`：睡眠的时间

#### 返回值

无

## clock类

clock 模块可以创建 clock 类， clock 类可以计算帧率

### 初始化

```
clk = clock.clock()

```

#### 参数

无

#### 返回值

* `clock` 类型的对象

### 获取时间戳

获取当前的时间戳

#### 参数

无

#### 返回值

无

### 计算帧率

#### 参数

无

#### 返回值

* `int`类型的帧率值

## 例程

```python
import sensor
import image
import lcd
import clock
clock = clock.clock()
lcd.init()
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
while True:
	clock.tick()
	img = sensor.snapshot()
	print("fps = ",clock.fps())
```



























