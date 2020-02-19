Power
====



## Connect hardware

Connect development board to computer with Type C cable

Check if the device has been properly identified:

Under Linux can `ls /dev/ttyUSB*` or `ls /dev/ttyACM*` to see, if not you can `ls /dev` come look for the specific device name with the relevant serial chip and drive， and you can use `sudo dmesg` to find device mount message

If Windows, just open Device Manager to view

If the device is not found, you need to confirm if the driver is installed and if the connection is good.

## Check firmware version

Open serial terminal tool, push reset button of board, compare to[github](https://github.com/sipeed/MaixPy/releases) or [master branch](http://dl.sipeed.com/MAIX/MaixPy/release/master/) to check firmware version

e.g.
```
[MaixPy] init end

 __  __              _____  __   __  _____   __     __
|  \/  |     /\     |_   _| \ \ / / |  __ \  \ \   / /
| \  / |    /  \      | |    \ V /  | |__) |  \ \_/ /
| |\/| |   / /\ \     | |     > <   |  ___/    \   /
| |  | |  / ____ \   _| |_   / . \  | |         | |
|_|  |_| /_/    \_\ |_____| /_/ \_\ |_|         |_|

Official Site : https://www.sipeed.com
Wiki          : https://maixpy.sipeed.com

MicroPython v0.5.0-12-g284ce83 on 2019-12-31; Sipeed_M1 with kendryte-k210
Type "help()" for more information.
```
Version is `v0.5.0-12-g284ce83`, or you can get version by code:
```
import sys
sys.implementation.version
```

## Exevute script(code)

* Open terminal, after push down the reset button, we can see:
```
>>> 
```
this means we can input code now, if no this symbol, type in `Ctrl+C` to cacel running script

* Then input hello world program
```
>>> print("hello world")
hello world
>>> 
```

## Paste multiple lines of code

If we need paste code like
```python
import os
f = os.listdir()
print(f)
```

* Copy code
* Type in `Ctrl+E`
* Paste code
* Type in `Ctrl+D` to execute code

```python
>>> 
paste mode; Ctrl-C to cancel, Ctrl-D to finish
=== import os
=== f = os.listdir()
=== print(f)
['boot.py','main.py', 'freq.conf']
>>> 

```

> If code is too large, the serial port may lose data, which will cause a syntax error prompt. You can try more times



