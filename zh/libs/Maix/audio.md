audio
============

抽象的音频对象，该对象可以被当做参数传入也可以直接使用其方法来播放音频

## 模块函数

###  构造函数

构造 audio 对象

```
import audio
test_audio = audio.Audio(array or path or points)
```

####  参数

该接口能传入一个参数，每个参数会决定不同的音频类型

* `array`: `bytearray`类型的数据，可以将该数据转换为音频对象

* `path`: 打开的音频文件路径，目前支持 双声道wav 格式

* `points`: 开辟有 points 个采样点数的音频缓冲，一个采样点大小为 32bit。为0的情况下将不开辟缓冲

####  返回值

`test_audio`: 返回一个音频对象


###  bytes转换函数

将音频对象中的音频数据转换为 `bytearray` 类型的对象

```
audio_data = test_audio.tobyte()
```

####  参数

无

####  返回值

`audio_data`: 返回的音频数据 `bytearray` 对象


### 播放预处理函数

用于预处理音频对象，在播放之前需要对音频文件进行解析，所以需要预处理。这里需要传入一个播放用的 I2S 设备

```
wav_info = test_audio.play_process(i2s_dev)
```

####  参数

* `i2s_dev`: 用于播放的i2s设备


####  返回值

`wav_info` : 该 wav 文件的头部信息 ,`list`类型，分别是`numchannels`, `samplerate`, `byterate`, `blockalign`, `bitspersample`, `datasize`

### 播放函数

读取音频文件并且解析播放，一般配合循环来使用

```
res = test.play()
```

####  参数

无


####  返回值

`res`：读取的大小，当为 `None` 时播放结束

### 音频后处理函数

完成音频播放，该函数必须在播放完毕后调用，因为需要回收不要的数据

```
test.finish()
```

####  参数

无

####  返回值

无

## 例程

播放双声道wav音频

```python 
import audio
from Maix import I2S
from Maix import GPIO
fm.register(8, fm.fpioa.GPIO0)
wifi_en=GPIO(GPIO.GPIO0,GPIO.OUT)
wifi_en.value(0)
fm.register(34,fm.fpioa.I2S2_OUT_D1)
fm.register(35,fm.fpioa.I2S2_SCLK)
fm.register(33,fm.fpioa.I2S2_WS)
test = audio.Audio(path = file_name)
wav_dev = I2S(I2S.DEVICE_2)
wav_info = test.play_process(wav_dev)
print("wav file head information: "wav_info)
wav_dev.channel_config(wav_dev.CHANNEL_1, I2S.TRANSMITTER,resolution = I2S.RESOLUTION_16_BIT ,cycles = I2S.SCLK_CYCLES_32, align_mode = I2S.RIGHT_JUSTIFYING_MODE)
wav_dev.set_sample_rate(wav_info[1])
while True:
    ret = test.play()
    if ret == None:
        print("None")
        break
test.finish()
```
