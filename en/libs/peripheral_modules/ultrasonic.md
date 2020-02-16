modules.ultrasonic
======

Ultrasonic sensor

![](../../../assets/ultrasonic.jpg)

## Construction method ultrasonic(gpiohs)

### Parameters

* `gpiohs`: gpiohs number, you need to register the pin with` fm` first, for example
```python
from fpioa_manager import *
from modules import ultrasonic

fm.register (board_info.D [6], fm.fpioa.GPIOHS0, force = True)
device = ultrasonic (fm.fpioa.GPIOHS0)
```

### return value

Return object

## method measure(unit, timeout)

### Parameters

* `unit`: unit, take the value in the following constants
* `timeout`: timeout time in microseconds (us)

## Constant

### ultrasonic.UNIT_CM

Unit of distance returned, cm

### ultrasonic.UNIT_INCH

Units of distance returned, feet



