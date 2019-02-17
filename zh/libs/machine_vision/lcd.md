Maix.lcd
====




## 例程

### 例程 1： 基本刷屏

```
import Maix
from machine import SPI
from Maix import GPIO


st7789 = Maix.st7789(spi_id=SPI.SPI0, cs=SPI.CS3, rst=GPIO.GPIO0, dc=GPIO.GPIO1, dma=0)
lcd = Maix.lcd(st7789)
lcd.init()
lcd.clear(0x0000)

```