FFT operation
============
The FFT fast Fourier transform module performs Fourier transform on the input data and returns the corresponding frequency amplitudes. The FFT fast Fourier operation can convert the time domain signal into the frequency domain signal.

## Module function

### arithmetic function

Enter time domain data and perform Fourier transform

```
Import FFT
Res = FFT.run(data, points, shift)
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
Res = FFT.freq(points, sample_rate)
```

#### Parameters

* `points`: Calculate points

* `sample_rate`: sample rate

####  return value

`res` : Returns a list of the frequency values ​​of all frequency points after the operation

### Amplitude function

It is used to calculate the amplitude of each frequency point after the FFT operation. It is currently used as a test. Users can write their own amplitude processing functions in python.

```
Amp = FFT.amplitude(FFT_res)
```

#### Parameters

`FFT_res`: function `run` results after running


#### return value

`res` : Returns a list that stores the magnitude of each frequency point

### Routine

Acquire sound and perform FFT operation, and display the calculated data on the screen as a histogram

```python
From Maix import GPIO
From Maix import I2S
From Maix import AUDIO
From Maix import FFT
Import image
Import lcd
Lcd.init()
Fm.register(8, fm.fpioa.GPIO0)
Wifi_en=GPIO(GPIO.GPIO0, GPIO.OUT)
Wifi_en.value(0)
Fm.register(20,fm.fpioa.I2S0_IN_D0)
Fm.register(19,fm.fpioa.I2S0_WS)
Fm.register(18,fm.fpioa.I2S0_SCLK)
Rx = I2S(I2S.DEVICE_0)
Rx.channel_config(rx.CHANNEL_0, rx.RECEIVER, align_mode = I2S.STANDARD_MODE)
Sample_rate = 38640
Rx.set_sample_rate(sample_rate)
Img = image.Image()
Sample_points = 1024
FFT_points = 512
Lcd_width = 320
Lcd_height = 240
Hist_num = FFT_points #changeable
If hist_num > 320:
    Hist_num = 320
Hist_width = int(320 / hist_num)#changeable
X_shift = 0
While True:
    Audio = rx.record(sample_points)
    FFT_res = FFT.run(audio.tobyte(), FFT_points)
    FFT_amp = FFT.amplitude(FFT_res)
    Img = img.clear()
    X_shift = 0
    For i in range(hist_num):
        If FFT_amp[i] > 240:
            Hist_height = 240
        Else:
            Hist_height = FFT_amp[i]
        Img = img.draw_rectangle((x_shift,240-hist_height,hist_width,hist_height),[255,255,255],2,True)
        X_shift = x_shift + hist_width
    Lcd.display(img)
```
