FFT operation
============
The FFT fast Fourier transform module performs Fourier transform on the input data and returns the corresponding frequency amplitudes. The FFT fast Fourier operation can convert the time domain signal into the frequency domain signal.

## Module function

### arithmetic function

Enter time domain data and perform Fourier transform

```
import FFT
res = FFT.run(data, points, shift)
```

#### Parameters

* `data`: input time domain data, `bytearray` type

* `points`: FFT operation points, only supports 64, 128, 256 and 512 points

* `shift`: offset, default is 0

####  return value

`res`: Returns the calculated frequency domain data, presented as `list` type. The list has `points` tuples, each tuple has 2 elements, the first element is the real part, and the second is Imaginary

### Frequency function

FFT

```
res = FFT.freq(points, sample_rate)
```

#### Parameters

* `points`: Calculate points

* `sample_rate`: sample rate

####  return value

`res` : Returns a list of the frequency values ​​of all frequency points after the operation

### Amplitude function

It is used to calculate the amplitude of each frequency point after the FFT operation. It is currently used as a test. Users can write their own amplitude processing functions in python.

```
amp = FFT.amplitude(FFT_res)
```

#### Parameters

`FFT_res`: function `run` results after running


#### return value

`res` : Returns a list that stores the magnitude of each frequency point

### Routine

Acquire sound and perform FFT operation, and display the calculated data on the screen as a histogram

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
