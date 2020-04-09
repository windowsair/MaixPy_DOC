machine.SPI
==========

SPI (Serial Peripheral Interface) is a synchronous serial protocol consisting of a master and a slave.

The standard 4-wire mode consists of 4 wires: SCK (SCLK), CS (chip select), MOSI, MISO.

On the K210, SPI has the following characteristics:

* There are 4 SPI devices, of which SPI0, SPI1, and SPI3 can only work in master mode, and SPI2 can only work in slave mode. On MaixPy, SPI3 has been used to connect SPI Flash. It is temporarily reserved. It is necessary to consider the open interface and SPI Flash time-sharing multiplexing.
* Supports 1/2/4 / 8-wire full-duplex mode. In MaixPy, currently only supports standard (Motorola) 4-wire full-duplex mode (ie, SCK, MOSI, MISO, CS four pins)
* Maximum transmission rate of 45M
* Support DMA
* 4 configurable hardware chip select



## Constructor

```python
class machine.SPI (id, mode = SPI.MODE_MASTER, baudrate = 500000, polarity = 0, phase = 0, bits = 8, firstbit = SPI.MSB, sck, mosi, miso, cs0, cs1, cs2, cs3)
```

Create a new SPI object with the specified parameters

### Parameters

* `id`: SPI ID, value range [0,3], currently only supports 0 and 1, and can only be master mode, 2 can only be used as slave, currently not implemented, 3 reserved
* `mode`: SPI mode,` MODE_MASTER` or `MODE_MASTER_2` or` MODE_MASTER_4` or `MODE_MASTER_8` or` MODE_SLAVE`, currently only supports `MODE_MASTER`
* `baudrate`: SPI baud rate (frequency)
* `polarity`: Polarity, the value is 0 or 1, which indicates the polarity of the SPI at idle, 0 represents low level, 1 represents high level
* `phase`: phase, value bit 0 or 1, which means to collect data at the first or second edge of the clock, 0 means the first, 1 means the second
* `bits`: data width, default value is 8, value range [4,32]
* `firstbit`: Specify whether to transmit in MSB or LSB order. The default is SPI.MSB.
* `sck`: SCK (Clock) pin, which can directly transmit the pin value. The value range is [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `mosi`: MOSI (host output) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `miso`: MISO (host input) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `cs0`: CS0 (chip select) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `cs1`: CS1 (chip select) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `cs2`: CS2 (chip select) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `cs3`: CS3 (chip select) pin, which can directly pass the pin value, value range: [0,47]. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `d0 ~ d7`: Data pins, used in non-standard 4-wire mode, currently reserved. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.

## Method

### init

Constructor-like

```python
SPI.init (id, mode = SPI.MODE_MASTER, baudrate = 500000, polarity = 0, phase = 0, bits = 8, firstbit = SPI.MSB, sck, mosi, miso, cs0)
```

#### parameters

Same as constructor


#### return value

no


### read

Read data

```python
SPI.read (nbytes, write = 0x00, cs = SPI.CS0)
```

#### parameters

* `nbytes`: the length to read
* `cs`: Select chip select pins. Pins have been set for` cs0` ~ `cs3` during initialization. Here you only need to select` SPI.CS0` ~ `SPI.CS3`. The default is` SPI. .CS0`
* `write`: Because it is full duplex, set the value of the` MOSI` pin when reading. The default is `0x00`, which is always low.


#### return value

`bytes` data


### readinto

Read the data and put it into the specified variable

```python
SPI.readinto (buf, write = 0x00, cs = SPI.CS0)
```

#### parameters


* `buf`:` bytearray` type, which defines the length, and the data is saved here after reading
* `cs`: Select chip select pins. Pins have been set for` cs0` ~ `cs3` during initialization. Here you only need to select` SPI.CS0` ~ `SPI.CS3`. The default is` SPI. .CS0`
* `write`: Because it is full duplex, set the value of the` MOSI` pin when reading. The default is `0x00`, which is always low.


#### return value

no

### write

send data

```python
SPI.write (buf, cs = SPI.CS0)
```

#### parameters

* `buf`:` bytearray` type, which defines the data and length
* `cs`: Select chip select pins. Pins have been set for` cs0` ~ `cs3` during initialization. Here you only need to select` SPI.CS0` ~ `SPI.CS3`. The default is` SPI. .CS0`

#### return value

no

### write_readinto

Send data while reading data to a variable, that is, full duplex

```python
SPI.write (write_buf, read_buf, cs = SPI.CS0)
```

#### parameters

* `write_buf`:` bytearray` type, which defines the data and length to be sent
* `read_buf`:` bytearray` type, which defines where the received data is stored
* `cs`: Select chip select pins. Pins have been set for` cs0` ~ `cs3` during initialization. Here you only need to select` SPI.CS0` ~ `SPI.CS3`. The default is` SPI. .CS0`

#### return value

no

### deinit / \ __ del \ __

Log out of SPI, release hardware, shut down SPI clock

```python
SPI.deinit ()
```

#### parameters

no

#### return value

no

#### Examples

```python
spi.deinit ()
```
or
```
del spi
```

## Constants

* `SPI0`: SPI 0
* `SPI1`: SPI 1
* `SPI2`: SPI 2
* `MODE_MASTER`: as master mode
* `MODE_MASTER_2`: as master mode
* `MODE_MASTER_4`: as master mode
* `MODE_MASTER_8`: as master mode
* `MODE_SLAVE`: as slave mode
* `MSB`: MSB, that is, the high-order or high-order byte is sent first
* `LSB`: LSB, that is, send low-order or low-order byte first
* `CS0`: Chip Select 0
* `CS1`: Chip Select 1
* `CS2`: Chip Select 2
* `CS3`: Chip Selection 3


## Examples

### Example 1: Basic Read and Write

```python
from machine import SPI

spi1 = SPI (SPI.SPI1, mode = SPI.MODE_MASTER, baudrate = 10000000, polarity = 0, phase = 0, bits = 8, firstbit = SPI.MSB, sck = 28, mosi = 29, miso = 30, cs0 = 27)
w = b'1234 '
r = bytearray (4)
spi1.write (w)
spi1.write (w, cs = SPI.CS0)
spi1.write_readinto (w, r)
spi1.read (5, write = 0x00)
spi1.readinto (r, write = 0x00)
```
