GPIO
=========

General Purpose Input Output （通用输入/输出）简称为GPIO，或总线扩展器。

K210上有高速gpio和通用gpio
在 K210 上， GPIO 有一下特征：
* 高速 GPIO：

  高速 GPIO 为 GPIOHS，共 32 个。具有如下特点：
  * 可配置输入输出信号
  * 每个 IO 具有独立中断源
  * 中断支持边沿触发和电平触发
  * 每个 IO 可以分配到 FPIOA 上 48 个管脚之一
  * 可配置上下拉，或者高阻

* 通用 GPIO：

    通用 GPIO 共 8 个，具有如下特点:
    * 8 个 IO 使用一个中断源
    * 可配置输入输出信号
    * 可配置触发 IO 总中断，边沿触发和电平触发
    * 每个 IO 可以分配到 FPIOA 上 48 个管脚之一


**注意**:

一下 GPIOHS 默认已经被使用， 程序中如非必要尽量不要使用：

| GPIOHS | 功能|
| ------ | --- |
| GPIOHS31 | LCD_DC      |
| GPIOHS30 | LCD_RST     |
| GPIOHS29 | SD_CS       |
| GPIOHS28 | MIC_LED_CLK |
| GPIOHS27 | MIC_LED_DATA |



## 构造函数

```python
class GPIO(ID,MODE,PULL,VALUE)
```

通过指定的参数新建一个 SPI 对象

### 参数

* `ID`： 使用的GPIO引脚(一定要使用GPIO里带的常量来指定)

* `MODE`： GPIO模式

  ​	• GPIO.IN就是输入模式

  ​	• GPIO.OUT就是输出模式

* `PULL`： GPIO上下拉模式
* 
  ​	• GPIO.PULL_UP 上拉
  
  ​	• GPIO.PULL_DOWN 下拉
  
  ​	• GPIO.PULL_NONE  即不上拉也不下拉
  

## 方法


### value

修改/读取GPIO引脚状态

```python
GPIO.value([value])
```

#### 参数

* `[value]`： 可选参数，如果此参数不为空，则返回当前GPIO引脚状态


#### 返回值

如果`[value]`参数不为空，则返回当前GPIO引脚状态


### irq

配置一个中断处理程序，当pin的触发源处于活动状态时调用它。如果管脚模式为pin.in，则触发源是管脚上的外部值。

```python
GPIO.irq(CALLBACK_FUNC,TRIGGER_CONDITION,GPIO.WAKEUP_NOT_SUPPORT,PRORITY)
```

#### 参数


* `CALLBACK_FUNC`：回调函数，当中断触发的时候被调用，他拥有两个参数，GPIO和PIN_NUM

  ​	• GPIO返回的是GPIO对象

  ​	• PIN_NUM返回的是触发中断的GPIO引脚号(只有GPIOHS支持中断，所以这里的引脚号也是GPIOHS的引脚号)

* `TRIGGER_CONDITION`：当GPIO引脚处于这个状态时触发中断

  ​	• GPIO.IRQ_RISING 上升沿触发

  ​	• GPIO.IRQ_FALLING 下降沿触发

  ​	• GPIO.IRQ_BOTH  上升沿和下降沿都触发


#### 返回值

无

### disirq

关闭中断

```python
GPIO.disirq()
```

#### 参数

无

#### 返回值

无

### mode

GPIO模式

```python
GPIO.mode(MODE)
```

#### 参数

* MODE

  ​	• GPIO.IN就是输入模式

  ​	• GPIO.OUT就是输出模式

#### 返回值

无

### pull

GPIO上下拉模式

```python
GPIO.pull(PULL)
```

#### 参数

* PULL

  ​	• GPIO.IRQ_RISING 上升沿触发

  ​	• GPIO.IRQ_RISING 下降沿触发

  ​	• GPIO.IRQ_BOTH  上升沿和下降沿都触发

#### 返回值

无

## 常量

* `GPIO0`: GPIO0
* `GPIO1`: GPIO1
* `GPIO2`: GPIO2
* `GPIO3`: GPIO3
* `GPIO4`: GPIO4
* `GPIO5`: GPIO5
* `GPIO6`: GPIO6
* `GPIO7`: GPIO7
* `GPIOHS0`: GPIOHS0
* `GPIOHS1`: GPIOHS1
* `GPIOHS2`: GPIOHS2
* `GPIOHS3`: GPIOHS3
* `GPIOHS4`: GPIOHS4
* `GPIOHS5`: GPIOHS5
* `GPIOHS6`: GPIOHS6
* `GPIOHS7`: GPIOHS7
* `GPIOHS8`: GPIOHS8
* `GPIOHS9`: GPIOHS9
* `GPIOHS10`: GPIOHS10
* `GPIOHS11`: GPIOHS11  
* `GPIOHS12`: GPIOHS12
* `GPIOHS13`: GPIOHS13
* `GPIOHS14`: GPIOHS14
* `GPIOHS15`: GPIOHS15
* `GPIOHS16`: GPIOHS16
* `GPIOHS17`: GPIOHS17
* `GPIOHS18`: GPIOHS18
* `GPIOHS19`: GPIOHS19
* `GPIOHS20`: GPIOHS20
* `GPIOHS21`: GPIOHS21
* `GPIOHS22`: GPIOHS22
* `GPIOHS23`: GPIOHS23
* `GPIOHS24`: GPIOHS24
* `GPIOHS25`: GPIOHS25
* `GPIOHS26`: GPIOHS26
* `GPIOHS27`: GPIOHS27
* `GPIOHS28`: GPIOHS28
* `GPIOHS29`: GPIOHS29
* `GPIOHS30`: GPIOHS30
* `GPIOHS31`: GPIOHS31
* `GPIO.IN`: 输入模式
* `GPIO.OUT`:输出模式
* `GPIO.PULL_UP`:上拉
* `GPIO.PULL_DOWN`:下拉
* `GPIO.PULL_NONE`:即不上拉也不下拉
* `GPIO.IRQ_RISING`:上升沿触发
* `GPIO.IRQ_RISING`:下降沿触发
* `GPIO.IRQ_BOTH`:上升沿和下降沿都触发



### DEMO1

```python
import utime
from Maix import GPIO
fm.register(board_info.LED_R,fm.fpioa.GPIO0)
led_r=GPIO(GPIO.GPIO0,GPIO.OUT)
utime.sleep_ms(500)
led_r.value()
fm.unregister(board_info.LED_R,fm.fpioa.GPIO0)
```

### DEMO2

```python
import utime
from Maix import GPIO
fm.register(board_info.LED_R,fm.fpioa.GPIO0)
led_r=GPIO(GPIO.GPIO0,GPIO.IN)
utime.sleep_ms(500)
led_r.value()
fm.unregister(board_info.LED_R,fm.fpioa.GPIO0)
```

### DEMO3

```python
import utime
from Maix import GPIO
def test_irq(GPIO,pin_num):
    print("key",pin_num,"\n")

fm.register(board_info.BOOT_KEY,fm.fpioa.GPIOHS0)
key=GPIO(GPIO.GPIOHS0,GPIO.IN,GPIO.PULL_NONE)
utime.sleep_ms(500)
key.value()
key.irq(test_irq,GPIO.IRQ_BOTH,GPIO.WAKEUP_NOT_SUPPORT,7)

key.disirq()
fm.unregister(board_info.BOOT_KEY,fm.fpioa.GPIOHS0)
```

