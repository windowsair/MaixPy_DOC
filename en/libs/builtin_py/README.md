Built-in class
===========

The library is a user-level interface that encapsulates the underlying classes of MaixPy, making it easy for users to use MaixPy, which includes the following:

* [fpioa_manager](fm.md)
* [board_info](board_info.md)

需要注意的是， 这些类在开机启动的时候在 `_boot.py` 里面已经被导入了， 所以在串口终端可以直接使用， 但是， 如果是执行文件， 则需要手动写代码导入， 否则找不到类
Be attention, theses class imported at the start up by `_boot.py`, so we can directly use it in serial terminal without any import,
**but if we execute file, we need to import by ourselves~**

```python
from board import board_info
from fpioa_manager import fm
```

or

```python
from fpioa_manager import fm, board_info
```

