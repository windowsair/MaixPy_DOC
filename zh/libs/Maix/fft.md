FFT运算
============
FFT快速傅里叶变换模块，对输入数据进行傅里叶变换并返回相应的频率幅值, FFT快速傅里叶运算可以将时域信号转换为频域信号

## 模块函数

###  运算函数

输入时域数据并进行傅里叶变换

```
import FFT
res = FFT.run(data, points, shift)
```

####  参数

* `data`: 输入的时域数据，`bytearray`类型  

* `points`: FFT, 128，256和512点

* `shift`: 偏移，默认为0  

####  返回值

`res`: 返回计算后的频域数据，以`list`类型呈现，该列表有`points`个元组，每个元组有 2 个元素，第一个元素为实部，第二个为虚部 

### 频率函数

FFT

```
res = FFT.freq(points, sample_rate)
```

####  参数

* `points`: 计算点数

* `sample_rate`: 采样率

####  返回值

`res` : 返回一个列表，该列表存放的进行运算后后所有频率点的频率值

### 幅值函数

用于计算 FFT 运算后的各个频率点的幅值，目前用作测试，用户可以自己在python自行写幅值处理函数

```
amp = FFT.amplitude(FFT_res)
```

#### 参数

`FFT_res`: 函数 `run` 运行后的结果


#### 返回值

`res` : 返回一个列表，该列表存放了各个频率点的幅值

### 例程

采集声音并进行FFT运算，将运算后的数据在屏幕上显示为柱状图

```python
from Maix import GPIO
from Maix import I2S
from Maix import AUDIO
from Maix import FFT
import image
import lcd
lcd.init()
fm.register(8,  fm.fpioa.GPIO0)
wifi_en=GPIO(GPIO.GPIO0,GPIO.OUT)
wifi_en.value(0)
fm.register(20,fm.fpioa.I2S0_IN_D0)
fm.register(19,fm.fpioa.I2S0_WS)
fm.register(18,fm.fpioa.I2S0_SCLK)
rx = I2S(I2S.DEVICE_0)
rx.channel_config(rx.CHANNEL_0, rx.RECEIVER, align_mode = I2S.STANDARD_MODE)
sample_rate = 38640
rx.set_sample_rate(sample_rate)
img = image.Image()
sample_points = 1024
FFT_points = 512
lcd_width = 320
lcd_height = 240
hist_num = FFT_points #changeable
if hist_num > 320:
    hist_num = 320
hist_width = int(320 / hist_num)#changeable
x_shift = 0
while True:
    audio = rx.record(sample_points)
    FFT_res = FFT.run(audio.tobyte(),FFT_points)
    FFT_amp = FFT.amplitude(FFT_res)
    img = img.clear()
    x_shift = 0
    for i in range(hist_num):
        if FFT_amp[i] > 240:
            hist_height = 240
        else:
            hist_height = FFT_amp[i]
        img = img.draw_rectangle((x_shift,240-hist_height,hist_width,hist_height),[255,255,255],2,True)
        x_shift = x_shift + hist_width
    lcd.display(img)
```