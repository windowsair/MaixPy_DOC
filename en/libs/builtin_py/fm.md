FPIOA Manager
===============

`fpioa_manager`: referred to as `fm`, this module is used to register internal functions and pins of the chip, to help users manage internal functions and pins. If the functions and pins are already registered, the internal functions and pins will not be available.

Actually `fm` is global variable define with `Fpioa_Manager` class, written by `Micropython` and integrated to firmware, source code see [board.py](https://github.com/sipeed/MaixPy/blob/master/ports/k210-freertos/mpy_support/builtin-py/board.py)


## method

### Registration function

Register pins and functions

```
fm.register(pin,function)
```
#### Parameters

This method must pass in 2 parameters, otherwise it will return a null value.

* `pin`: function mapping pin
* `function` : chip function

#### return value

This method has 2 return values.
* Parameter error returns `None,None`
* Set the success to return `pin, function`
* Setting fails to return `reg_pin, reg_func`, indicating the pins and functions that have been registered

### Logout function

Logout pin and function

```
fm.unregister(pin,function)
```
#### Parameters

This method can pass 1 or 2 parameters. When passing in 1 parameter, you need to add a parameter keyword. If it is 1 parameter, its pins and functions will be logged out.

* `pin`: function mapping pin
* `function` : chip function

#### return value

* Parameter error returns `None,None`
* Set successfully returns `pin, function`, indicating the pin and function being logged out
* Set failure to return `0,0`

## Routine

```python
fm.register(board_info.WIFI_RX,fm.fpioa.UART2_TX)
fm.register(board_info.WIFI_RX,fm.fpioa.UART2_TX)#Register again
fm.register(board_info.WIFI_RX,fm.fpioa.SPI0_SS0)#Register the same pin
fm.register(board_info.WIFI_RX,fm.fpioa.SPI0_SS0)#Register the same function
fm.unregister(board_info.WIFI_RX, fm.fpioa.UART2_TX)#Logout function and pin
fm.register(board_info.WIFI_RX,fm.fpioa.UART2_TX)
fm.unregister(function = fm.fpioa.UART2_TX)#Logout function
fm.register(board_info.WIFI_RX,fm.fpioa.UART2_TX)
fm.unregister(pin = board_info.WIFI_RX)#Logout pin
```

## Appendix

The following pins have been registered when MaxiPy is powered on. Please pay attention to the user.

### SD card
* `Function`: SPI1_SCLK/SPI1_D0/SPI1_D1/GPIOHS7/SPI0_SS1
* `Pin`: PIN25/PIN26/PIN27/PIN28/PIN29

### LCD
* `Function`: SPI0_SS3/SPI0_SCLK/GPIOHS1/GPIOHS2
* `Pin`: PIN36/PIN37/PIN38/PIN39

### sensor
* `Function`: SCCB_SDA/SCCB_SCLK/CMOS_RST/CMOS_VSYNC/CMOS_PWDN/CMOS_HREF/CMOS_XCLK/CMOS_PCLK
* `Pin`: PIN40/PIN41/PIN42/PIN43/PIN44/PIN45/PIN46/PIN47

### REPL
* `Function`: UARTHS_RX/UARTHS_TX
* `Pin`: PIN4/PIN5
