machine.I2C
======


I2C bus protocol, simply using two lines (SCL, SDA) can control multiple slaves (master mode).

* Supports master mode and slave mode
* 7-bit / 10-bit addressing mode
* Standard mode <= 100Kb / s
* Fast mode <= 400Kb / s
* Super fast mode <= 1000Kb / s
* High-speed mode 3.4Mb / s

## Constructor

```python
class machine.I2C (id, mode = I2C.MODE_MASTER, scl = None, sda = None, freq = 400000, timeout = 1000, addr = 0, addr_size = 7, on_recieve = None, on_transmit = None, on_event = None)
```

Create a new I2C object with the specified parameters

### Parameters

* `id`: I2C ID, [0 ~ 2] \ (I2C.I2C0 ~ I2C.I2C2 \)
* `mode`: mode, master (` I2C.MODE_MASTER`) and slave (`I2C.MODE_SLAVE`) mode
* `scl`: SCL pin, just pass the pin number directly, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `sda`: SDA pin, just pass the pin number directly, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `freq`: I2C communication frequency, support standard 100Kb / s, fast 400Kb / s, and higher speed (hardware supports super fast mode 1000Kb / s, and high-speed mode 3.4Mb / s)
* `timeout`: timeout time, currently this parameter is reserved, setting is invalid
* `addr`: slave address, if it is in master mode, it is not necessary to set, slave mode represents slave (local) address
* `addr_size`: address length, support 7-bit addressing and 10-bit addressing, value` 7` or `10`
* `on_recieve`: Receive callback function in slave mode
* `on_transmit`: Send callback function in slave mode
* `on_event`: Slave mode event function (start event and end event)

## Method

### init

Constructor-like

```python
I2C.init (id, mode = Timer.MODE_MASTER, scl, sda, freq = 400000, timeout = 1000, addr = 0, addr_size = 7, on_recieve = None, on_transmit = None, on_event = None)
```

#### parameters

Same as constructor



#### return value

no


### scan

Scanning slaves on the I2C bus

```python
I2C.scan ()
```

#### parameters

no

#### return value

list object containing all scanned slave addresses


### readfrom

Read data from the bus

```python
I2C.readfrom (addr, len, stop = True)
```

#### parameters

* `addr`: slave address
* `len`: data length
* `stop`: Whether to generate a stop signal. Reserved. Currently, only the default value True can be used.

#### return value

Read data, type `bytes`

### readfrom_into

Read the data and put it into the specified variable

```python
I2C.readfrom_into (addr, buf, stop = True)
```

#### parameters

* `addr`: slave address
* `buf`:` bytearray` type, which defines the length, and the read data is stored here
* `stop`: Whether to generate a stop signal. Reserved. Currently, only the default value True can be used.

#### return value

no

### writeto

Send data to the slave

```python
I2C.writeto (addr, buf, stop = True)
```

#### parameters


* `addr`: slave address
* `buf`: data to be sent
* `stop`: Whether to generate a stop signal. Reserved. Currently, only the default value True can be used.

#### return value

Number of bytes successfully sent

### readfrom_mem

Read slave register

```python
I2C.readfrom_mem (addr, memaddr, nbytes, mem_size = 8)
```

#### parameters

* `addr`: slave address
* `memaddr`: slave register address
* `nbytes`: the length to read
* `mem_size`: register width, default is 8 bits

#### return value

Returns read data of type `bytes`


### readfrom_mem_into

Read the slave register value into the specified variable

```python
I2C.readfrom_mem_into (addr, memaddr, buf, mem_size = 8)
```

#### parameters

* `addr`: slave address
* `memaddr`: slave register address
* `buf`:` bytearray` type, which defines the length, and the read data is stored here
* `mem_size`: register width, default is 8 bits

#### return value

no


### writeto_mem

Write data to slave register

```python
I2C.writeto_mem (addr, memaddr, buf, mem_size = 8)
```

#### parameters


* `addr`: slave address
* `memaddr`: slave register address
* `buf`: data to be written
* `mem_size`: register width, default is 8 bits

#### return value

no

### deinit / \ __ del \ __

Log off the I2C hardware, release the occupied resources, and shut down the I2C clock

```python
I2C.deinit ()
```

#### parameters

no

#### return value

no

#### Examples

```python
i2c.deinit ()
```
or
```python
del i2c
```


## Constants

* `I2C0`: I2C 0
* `I2C1`: I2C 1
* `I2C2`: I2C 2
* `MODE_MASTER`: as master mode
* `MODE_SLAVE`: as slave mode
* `I2C_EV_START`: event type, start signal
* `I2C_EV_RESTART`: event type, restart signal
* `I2C_EV_STOP`: event type, end signal


## Examples

### Routine 1: Scanning for Slave Devices

```python
from machine import I2C

i2c = I2C (I2C.I2C0, freq = 100000, scl = 28, sda = 29)
devices = i2c.scan ()
print (devices)
```

### Example 2: Read and write

```python
import time
from machine import I2C

i2c = I2C (I2C.I2C0, freq = 100000, scl = 28, sda = 29)
i2c.writeto (0x24, b'123 ')
i2c.readfrom (0x24,5)
```

### Example 3: Slave mode

```python
from machine import I2C

count = 0

def on_receive (data):
    print ("on_receive:", data)

def on_transmit ():
    count = count + 1
    print ("on_transmit, send:", count)
    return count

def on_event (event):
    print ("on_event:", event)

i2c = I2C (I2C.I2C0, mode = I2C.MODE_SLAVE, scl = 28, sda = 29, addr = 0x24, addr_size = 7, on_receive = on_receive, on_transmit = on_transmit, on_event = on_event)
```

### Example 4: OLED (ssd1306 128x64)

```python
import time
from machine import I2C

SSD1306_CMD = 0
SSD1306_DATA = 1
SSD1306_ADDR = 0x3c

def oled_init (i2c):
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xAE, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x20, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x10, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xb0, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xc8, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x00, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x10, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x40, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x81, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xff, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xa1, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xa6, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xa8, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x3F, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xa4, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xd3, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x00, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xd5, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xf0, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xd9, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x22, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xda, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x12, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xdb, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x20, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x8d, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x14, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xaf, mem_size = 8)



def oled_on (i2c):
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0X8D, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0X14, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0XAF, mem_size = 8)

def oled_off (i2c):
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0X8D, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0X10, mem_size = 8)
    i2c.writeto_mem (SSD1306_ADDR, 0x00, 0XAE, mem_size = 8)

def oled_fill (i2c, data):
    for i in range (0,8):
        i2c.writeto_mem (SSD1306_ADDR, 0x00, 0xb0 + i, mem_size = 8)
        i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x10, mem_size = 8)
        i2c.writeto_mem (SSD1306_ADDR, 0x00, 0x01, mem_size = 8)
        for j in range (0,128):
            i2c.writeto_mem (SSD1306_ADDR, 0x40, data, mem_size = 8)

i2c = I2C (I2C.I2C0, mode = I2C.MODE_MASTER, freq = 400000, scl = 28, sda = 29, addr_size = 7)

time.sleep (1)
oled_init (i2c)
oled_fill (i2c, 0xff)

```

