I2S
====================
The I2S module is mainly used to drive I2S devices. The k210 has a total of 3 I2S devices and each device has a total of 4 channels. Before use, it is necessary to map and manage the pins.

## Module functions

### Constructor

Create a new I2S object

```
from Maix import I2S
i2s_dev = I2S (device_num)
```

#### parameters

`device_num` UART number, using the specified I2S, which can be completed by pressing the I2S.` tab

#### return value

Returns an `I2S` object

### Channel configuration function

Used to configure I2S channels. Pin mapping is required before

```
i2s_dev.channel_config (channel, mode, resolution, cycles, align_mode)
```
#### parameters

* `channel`: I2S channel number

* `mode`: channel transmission mode. There are a total of receiving and sending modes.

* `resolution`: channel resolution, ie the number of received data bits

* `cycles`: number of single data clocks

* `align_mode`: channel alignment mode

#### return value

no

### Set the sampling rate

Used to configure the I2S sampling rate

```
i2s_dev.set_sample_rate (sample_rate)
```
#### parameters

`sample_rate`:

#### return value

no

### Receive audio

Receive audio data using I2S

```
audio = i2s_dev.record (points)
```
#### parameters

* `points`: audio points collected at one time

#### return value

audio: an audio audio object

### Send audio

Send audio data using I2S

```
i2s_dev.play (audio)
```
#### parameters

* `audio`: the audio object to send

#### return value
no

## Examples

### Routine 1
```python
from Maix import I2S
import time
fm.register (20, fm.fpioa.I2S0_IN_D0) #GO
fm.register (19, fm.fpioa.I2S0_WS)
fm.register (18, fm.fpioa.I2S0_SCLK)
fm.register (34, fm.fpioa.I2S2_OUT_D1)
fm.register (35, fm.fpioa.I2S2_SCLK)
fm.register (33, fm.fpioa.I2S2_WS)
sample_rate = 44 * 1000
rx = I2S (I2S.DEVICE_0)
rx.channel_config (rx.CHANNEL_0, rx.RECEIVER, align_mode = I2S.STANDARD_MODE)
rx.set_sample_rate (sample_rate)
tx = I2S (I2S.DEVICE_2)
tx.channel_config (tx.CHANNEL_1, tx.TRANSMITTER, align_mode = I2S.RIGHT_JUSTIFYING_MODE)
tx.set_sample_rate (sample_rate)
while True:
    audio = rx.record (256) #sampling points number must be smaller than 256
    tx.play (audio)
```

### Routine 2
```python
from Maix import I2S
from Maix import Audio
from Maix import FFT
import time
fm.register (20, fm.fpioa.I2S0_IN_D0)
fm.register (19, fm.fpioa.I2S0_WS)
fm.register (18, fm.fpioa.I2S0_SCLK)
fm.register (34, fm.fpioa.I2S2_OUT_D1)
fm.register (35, fm.fpioa.I2S2_SCLK)
fm.register (33, fm.fpioa.I2S2_WS)

rx = I2S (I2S.DEVICE_0)
rx.channel_config (rx.CHANNEL_0, rx.RECEIVER, align_mode = I2S.STANDARD_MODE)
rx.set_sample_rate (16000)
tx = I2S (I2S.DEVICE_2)
tx.channel_config (tx.CHANNEL_1, tx.TRANSMITTER, align_mode = I2S.RIGHT_JUSTIFYING_MODE)
tx.set_sample_rate (16000)

while True:
    audio = rx.record (256)
    audio_data = audio.to_bytes ()
    play_audio = Audio (audio_data)
    tx.play (play_audio)
```
