audio
=============

An abstract audio object that can be passed in as a parameter or use its methods to play audio directly

## Module functions

###  Constructor

Constructing an Audio object

```python
audio.Audio (array = None, path = None, points = 1024)
```

#### parameters

This interface can pass a parameter, each parameter will determine a different audio type

* `array`: data of type` bytearray`, which can be converted to audio objects, default `None`

* `path`: The path of the opened audio file, currently only supports the` wav` format. The default is `None`, ** Note ** The keywords` path`, `audio.Audio (" / sd / 1.wav " ) `This is wrong! !! `audio.Audio (path =" /sd/1.wav ")` is correct

* `points`: open audio buffer with points sample points, one sample point size is 32bit. If it is 0, the buffer will not be opened. The default is 1024.

####  return value

Returns an Audio object


### to_bytes: bytes conversion function

Converts audio data in an audio object to an object of type bytearray

```
audio_data = test_audio.to_bytes ()
```

#### parameters

no

####  return value

The returned audio data `bytearray` object


### play_process: Play preprocessing function

It is used to preprocess audio objects, and audio files need to be parsed before playback, so preprocessing is required. Here you need to pass in an I2S device for playback

```
wav_info = test_audio.play_process (i2s_dev)
```

#### parameters

* `i2s_dev`: i2s device for playback


####  return value

Header information of the wav file, `list` type, which are` numchannels`, `samplerate`,` byterate`, `blockalign`,` bitspersample`, `datasize`

### play: Play function

Read audio files and parse them. Generally used in conjunction with loops.


#### parameters

no


####  return value

* `None`: Format does not support playback
* `0`: End of playback
* `1`: Now playing

### finish: Audio post-processing functions

Complete the audio playback. This function must be called after the playback is completed, and the underlying allocated resources are recycled.


#### parameters

no

####  return value

no

## Examples

Play `wav` audio

```python
from fpioa_manager import *
from Maix import I2S, GPIO
import audio

# disable wifi
fm.register (8, fm.fpioa.GPIO0)
wifi_en = GPIO (GPIO.GPIO0, GPIO.OUT)
wifi_en.value (0)

# register i2s (i2s0) pin
fm.register (34, fm.fpioa.I2S0_OUT_D1)
fm.register (35, fm.fpioa.I2S0_SCLK)
fm.register (33, fm.fpioa.I2S0_WS)

# init i2s (i2s0)
wav_dev = I2S (I2S.DEVICE_0)

# init audio
player = audio.Audio (path = "/sd/6.wav")
player.volume (40)

# read audio info
wav_info = player.play_process (wav_dev)
print ("wav file head information:", wav_info)

# config i2s according to audio info
wav_dev.channel_config (wav_dev.CHANNEL_1, I2S.TRANSMITTER, resolution = I2S.RESOLUTION_16_BIT, cycles = I2S.SCLK_CYCLES_32, align_mode = I2S.RIGHT_JUSTIFYING_MODE)
wav_dev.set_sample_rate (wav_info [1])

# loop to play audio
while True:
    ret = player.play ()
    if ret == None:
        print ("format error")
        break
    elif ret == 0:
        print ("end")
        break
player.finish ()
```
