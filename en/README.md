MaixPy Documentation
======

<div class="title_pic">
    <img src="../assets/micropython.png"><img src="../assets/icon_sipeed2.png"  height="60">
</div>


## What is MaixPy

MaixPy ported [Micropython](http://micropython.org/) to [K210](https://kendryte.com/) (a 64-bit dual-core RISC-V CPU with hardware FPU and convolution accelerator). A project that supports MCU routine operations and integrates machine vision and microphone arrays to quickly develop intelligent applications in the `AIOT` field that are extremely cost effective and practical.


> MicroPython is a lean and efficient implementation of the Python 3 programming language that includes a small subset of the Python standard library and is optimised to run on microcontrollers and in constrained environments.

> K210 created for AIOT(AI+IOT) use, It's powerful performance and low cost are very competitive.


Micropython makes programming on K210 hardware easier, the code is open source and can be found on [GitHub]((https://github.com/sipeed/MaixPy))

For example if we want to find an **I2C** device, we just need this code:
```python
from machine import I2C

i2c = I2C(I2C.I2C0, freq=100000, scl=28, sda=29)
devices = i2c.scan()
print(devices)
```

Again, if we want to make a **breathing light** using PWM, we just need this code:
```python
from machine import Timer,PWM
import time

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PWM)
ch = PWM(tim, freq=500000, duty=50, pin=board_info.LED_G)
duty=0
dir = True
while True:
    if dir:
        duty += 10
    else:
        duty -= 10
    if duty>100:
        duty = 100
        dir = False
    elif duty<0:
        duty = 0
        dir = True
    time.sleep(0.05)
    ch.duty(duty)
```

To **take a picture**：

```python
import sensor
import image
import lcd

lcd.init()
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)
while True:
    img=sensor.snapshot()
    lcd.display(img)
```


## About this documentation

Everything related to MaixPy, mainly about
* How to get the hardware (the board).
* How to get started with MaixPy even if you are not an expert in hardware programming.
* Learn MicroPython language basics
* Libraries (API) reference

## Let's get started

To get started, we need one dev board, there are three kind of boards:

* Dan dock with Sipeed M1(Dan) module

![Dan dock](../assets/Dan_Dock.png)

* Sipeed Maix BiT

![BiT](../assets/BiT.png)

* Sipeed Go

![Go](../assets/Go.jpg)

To get any of those boards, visit [Sipeed Official Website](https://sipeed.com/)

More hardware infomation [here](en/hardware/hardware.md)

We can now start writing software, refer to [get started](en/get_started.md)


## Source code

MaixPy source code [on GitHub](https://github.com/sipeed/MaixPy)

Maintained by &copy;<a href="https://www.sipeed.com" style="color: #f14c42">Sipeed</a> Co.,Ltd. More contributors [here](https://github.com/sipeed/MaixPy/graphs/contributors)


## Source code of MaixPy documentation

Doumentation will be edited if the API code is changed.

The source of the documentation can be found on [GitHub](https://github.com/sipeed/MaixPy_DOC)

You **MUST** read the [documentation convention](contribute/doc_convention.md) before editing it!

|   Branch  |   Status  |
| --------- | --------------- |
| master |[![Build Status](https://travis-ci.org/sipeed/MaixPy_DOC.svg?branch=master)](https://travis-ci.org/sipeed/MaixPy_DOC) |
| dev    |[![Build Status](https://travis-ci.org/sipeed/MaixPy_DOC.svg?branch=dev)](https://travis-ci.org/sipeed/MaixPy_DOC)    |


## Feedback

* [Documentation feedback](https://github.com/sipeed/MaixPy_DOC/issues)
* [Code feedback](https://github.com/sipeed/MaixPy/issues)
