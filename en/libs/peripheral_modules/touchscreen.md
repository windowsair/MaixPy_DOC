touchscreen
=============

Touchscreen related operation, like get click status or get click coordinate

the drivers supported currently:

* ns2009 ( default )

To change driver, we need to rebuild the `Maixpy` firmware



## Global Function

### init(i2c=None, cal=None)

Initialize touchscreen

> This API may be changed later, considering the different type of touchscreen

#### Parameters

* `i2c`: Currently just support `I2C` touchscreen, so should give `I2C` object, we may rename this paramter or remove it later
* `cal`: Calibration data, a touple consist of  `7` integer, get by `touchscreen.calibrate()` function

### calibrate()

Calibrate touchscreen with LCD screen pixels

#### Return

Return a tuple consist of `7` integer, you can save it to file system or `flash`, use it in `init` function, so we no need to calibrate every power on

### read()

Read the click status of touchscreen, and return coordinate of click( press )

#### Return

一个由 `3` 个整型值组成的元组 `(status, x, y)`， 注意这个值会一直保持上一个状态
A touple consist of `3` integet `(status, x, y)`, be attention, the value always keep the last value if status did'nt change

* `status`： click status, values: `touchscreen.STATUS_PRESS`， `touchscreen.STATUS_MOVE`， `touchscreen.STATUS_RELEASE`
* `x`：  `x`  coordinate
* `y`：  `y`  coordinate


## Constant

### touchscreen.STATUS_PRESS

The touchscreen is pressed, the firt value of tuple returned by `read()`

### touchscreen.STATUS_MOVE

The touchscreen is pressed and pen is moving, the firt value of tuple returned by `read()`

### touchscreen.STATUS_RELEASE

The touchscreen is released, the firt value of tuple returned by `read()`



## Examples

## Demo 1: Drawing Board

Drawing board, you can clear content with `boot` key

> uncomment `ts.calibrate()` to execute calibration program


```python
import touchscreen as ts
from machine import I2C
import lcd, image
from board import board_info
from fpioa_manager import *

board_info=board_info()

fm.register(board_info.BOOT_KEY, fm.fpioa.GPIO1)
btn_clear = GPIO(GPIO.GPIO1, GPIO.IN)

lcd.init()
i2c = I2C(I2C.I2C0, freq=400000, scl=30, sda=31)
ts.init(i2c)
#ts.calibrate()
lcd.clear()
img = image.Image()
status_last = ts.STATUS_IDLE
x_last = 0
y_last = 0
draw = False
while True:
    (status,x,y) = ts.read()
    print(status, x, y)
    if draw:
        img.draw_line((x_last, y_last, x, y))
    if status_last!=status:
        if (status==ts.STATUS_PRESS or status == ts.STATUS_MOVE):
            draw = True
        else:
            draw = False
        status_last = status
    lcd.display(img)
    x_last = x
    y_last = y
    if btn_clear.value() == 0:
        img.clear()
ts.__del__()
```
