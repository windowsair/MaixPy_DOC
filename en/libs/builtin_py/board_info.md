board_info
===============

`board_info`: Mainly used for user-friendly development board pin configuration, built-in user-friendly naming and interface, which allows users to reduce the dependence on the electrical connection schematic.


`board_info` is a global variable defined with `Board_Info` class, written by `MicroPython` and integrated to firmware, source code see [fpioa_manager.py](https://github.com/sipeed/MaixPy/blob/master/ports/k210-freertos/mpy_support/builtin-py/fpioa_manager.py)



## Members

Board_info has many pin indexes and a list

### pin_namelist

The list is mainly used internally by the class, and the user does not operate it.

### Pin Index
The pin index is mainly to convert numbers into human-friendly strings, which is convenient for users to program.

Enter the following, please be careful not to ignore the `.` number, then press the `tab key` to complete, you can see the board-related pin functions.

```
Board_info.
```

For example, enter the following code, it will return the number `8`, which represents the 8th pin of the development board, and its electrical connection is the enable pin of the wifi module.

```
board_info.WIFI_EN
```

## method

### Search method

When the user does not know the pin electrical connection, you can use this method to find

```
Board_info.pin_map(pin_num)
```
#### Parameters

This method does not pass in parameters or pass in a parameter

* `pin_num`: pin number, range [6,47]

Board-level electrical connection information for all pins will be printed when no parameters are passed in

When the parameters are passed in, only the board-level electrical connection information for the specified pin is printed.

#### return value

* Parameter error returns `False`
* Unknown error return `False`
* Find successful return information

## Routine

### Routine 1

```python
from board import board_info

Wifi_en_pin = board_info.WIFI_EN
Print(wifi_en_pin)# output is 8
Board_info.pin_map()# print all
Board_info.pin_map(8)# prints only pin 8 information
```
