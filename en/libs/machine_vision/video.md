video
=====


Support play and record video with `avi` format

## Global Function

### open(path, record=False, interval=100000, quality=50, width=320, height=240, audio=False, sample_rate=44100, channels=1)

Open a `avi` file to play or record

#### Parameters

* `path`： file path, e.g. `/sd/badapple.avi`
* `record`： record video or not, if `False` just to play video
* `interval`： Record interval, unit: micro second, fps = 1000000/interval, default to `100000`, that is `10` frames per second
* `quality`： `jpeg` compress quality(`%`), default to `50`
* `width`： record screen width, default to `320`
* `height`： record screen height, default to `240`
* `audio`： record audio or not, default to `False`
* `sample_rate`： sample rate of recorded audio, default to `44100` (`44.1k`)
* `channels`： channels of recorded audio, default to `1`

#### Return Value

Return a object, just support `avi` format yet, so just return a instance of `avi` class


## Class `avi`

Get from `video.open()` 

### play()


Play video, decode one frame data(video or audio) every once called

#### Return value

* `0`: play end
* `1`: playing
* `2`: pause(reverve)
* `3`: current frame is video
* `4`: current frame is audio


### volume(volume)

Set play volume

#### Parameters

* `volume`: value:[0,100]

#### Return value

Set value, ranges: [0,100]


### record()


Record video frame, it will block until the interval time up

#### Return Value

The length of current frame( video )




## Examples

### Example 1: Play `avi` video

Encode a video with format: screen size `320x240`， `MJEPG` compress format, `PCM` format audio

You can download `avi` video here: [badapple.avi](http://dl.sipeed.com/MAIX/MaixPy/assets/badapple_320_240_15fps.avi)

```python
import video,time
from Maix import GPIO

fm.register(34,  fm.fpioa.I2S0_OUT_D1)
fm.register(35,  fm.fpioa.I2S0_SCLK)
fm.register(33,  fm.fpioa.I2S0_WS)
fm.register(8,  fm.fpioa.GPIO0)
wifi_en=GPIO(GPIO.GPIO0,GPIO.OUT)
wifi_en.value(0)

v = video.open("/sd/badapple.avi")
print(v)
v.volume(50)
while True:
    if v.play() == 0:
        print("play end")
        break
v.__del__()

```

By default, it will use `I2S0` to display audio, so we need to set their corresponding pins;

Turn off WiFi because of the interference of `Dock` board WiFi on sound quality


### Example 2: Record Video


```python
import video, sensor, image, lcd, time

lcd.init()  
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
sensor.skip_frames(30)
v = video.open("/sd/capture.avi", record=1, interval=200000, quality=50)
i = 0
tim = time.ticks_ms()
while True:
    tim = time.ticks_ms()
    img = sensor.snapshot()
    lcd.display(img)
    img_len = v.record(img)
    # print("record",time.ticks_ms() - tim)
    i += 1
    if i > 100:
        break
print("finish")
v.record_finish()
lcd.clear()
```

You can cancel the print comment to see if the actual recording interval has reached the set frame interval (such as `200000us` set here). The actual print should be `200ms`,
If the actual frame interval is greater than the set value, the actual performance does not meet the set requirements. You need to increase the set frame interval to decrease the frame rate.
In addition, removing the display and printing can also increase the frame rate to some extent.


