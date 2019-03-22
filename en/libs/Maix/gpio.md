GPIO
=========

General Purpose Input Output is referred to as GPIO, or bus extender.

High speed gpio and universal gpio on the K210
On the K210, GPIO has the following characteristics:
* High speed GPIO:
  The high-speed GPIO is GPIOHS, a total of 32. Has the following characteristics:
  • Configurable input and output signals
  • Each IO has an independent interrupt source
  • Interrupt support edge trigger and level trigger
  • Each IO can be assigned to one of the 48 pins on the FPIOA
  • Configurable up and down, or high resistance

* General GPIO:

    There are 8 general-purpose GPIOs with the following characteristics:
    • 8 IOs use one interrupt source
    • Configurable input and output signals
    • Configurable trigger IO total interrupt, edge trigger and level trigger
    • Each IO can be assigned to one of the 48 pins on the FPIOA


**Attention**:

Some GPIOHS used by default, don't use these GPIOHS if you don't have to, GPIOHS used as follows:

| GPIOHS | Function |
| ------ | -------- |
| GPIOHS31 | LCD_DC      |
| GPIOHS30 | LCD_RST     |
| GPIOHS29 | SD_CS       |
| GPIOHS28 | MIC_LED_CLK |
| GPIOHS27 | MIC_LED_DATA |


## Constructor

```python
Class GPIO(ID, MODE, PULL, VALUE)
```

Create a new SPI object with the specified parameters

### Parameters

* `ID`: The GPIO pin used (must be specified using the constants in GPIO)

* `MODE`: GPIO mode

  • GPIO.IN is the input mode

  • GPIO.OUT is the output mode

* `PULL`: GPIO pull-down mode
*
  • GPIO.PULL_UP pull up
  
  • GPIO.PULL_DOWN dropdown
  
  • GPIO.PULL_NONE does not pull up or pull down
  

## method


### value

Modify/read GPIO pin status

```python
GPIO.value([value])
```

#### Parameters

* `[value]`: optional parameter, if this parameter is not empty, return the current GPIO pin status


#### return value

Returns the current GPIO pin status if the `[value]` parameter is not empty


### irq

Configure an interrupt handler to call when the pin's trigger source is active. If the pin mode is pin.in, the trigger source is an external value on the pin.

```python
GPIO.irq(CALLBACK_FUNC, TRIGGER_CONDITION, GPIO.WAKEUP_NOT_SUPPORT, PRORITY)
```

#### Parameters


* `CALLBACK_FUNC`: callback function, called when the interrupt is triggered, it has two parameters, GPIO and PIN_NUM

  • GPIO returns a GPIO object

  • PIN_NUM returns the GPIO pin number that triggered the interrupt (only GPIOHS supports interrupts, so the pin number here is also the GPIOHS pin number)

* `TRIGGER_CONDITION`: Trigger interrupt when GPIO pin is in this state

  • GPIO.IRQ_RISING rising edge trigger

  • GPIO.IRQ_RISING falling edge trigger

  • GPIO.IRQ_BOTH is triggered on both rising and falling edges


#### return value

no

### disirq

Close interrupt

```python
GPIO.disirq()
```

#### Parameters

no

#### return value

no

### mode

GPIO mode

```python
GPIO.mode(MODE)
```

#### Parameters

* MODE

  • GPIO.IN is the input mode

  • GPIO.OUT is the output mode

#### return value

no

### pull

GPIO pull-down mode

```python
GPIO.pull(PULL)
```

#### Parameters

* PULL

  • GPIO.IRQ_RISING rising edge trigger

  • GPIO.IRQ_RISING falling edge trigger

  • GPIO.IRQ_BOTH is triggered on both rising and falling edges

#### return value

no

## Constant

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
* `GPIO.IN`: input mode
* `GPIO.OUT`: output mode
* `GPIO.PULL_UP`: Pull up
* `GPIO.PULL_DOWN`: drop down
* `GPIO.PULL_NONE`: does not pull up or pull down
* `GPIO.IRQ_RISING`: rising edge trigger
* `GPIO.IRQ_RISING`: falling edge trigger
* `GPIO.IRQ_BOTH`: Both rising and falling edges are triggered



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

