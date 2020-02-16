Getting a development board
========

Get your favorite hardware from [Sipeed's official Taobao store](https://shop365481095.taobao.com/) or from [Seeed Studio](https://www.seeedstudio.com/catalogsearch/result/?cat=&q=sipeed)

## Required hardware

### A development board

See boards in [Wiki](https://wiki.sipeed.com/zh/maix/board/)

And get your favorite hardware from [Sipeed's official Taobao store](https://shop365481095.taobao.com/) or from [Seeed Studio](https://www.seeedstudio.com/catalogsearch/result/?cat=&q=sipeed)

### USB Type C cable

<img src="../../assets/type_c.png" height="300" alt="type_c">

Type-C is chosen because it's reversible and it's very friendly for development.

If you're buying from the official Taobao store, you can ask them to include it with your order. Type-C cables are also very common with Android phones.

### Screen

By default, the LCD (24-pin interface) of the st7789 driver chip is used with a resolution of 320x240.

If you're buying from the official Taobao store, you can ask them to include it with your order.

### Camera

MaixPy devices support the ov2640 camera by default(or gc0328 or ov7740), and are often bundled with  Maix devices.  The ov2640 cameras bundled with Maix device are typically offered with two different lens options; a larger focusable fisheye lens, or a smaller fixed-focus lens.

If you're buying from the official Taobao store, you can order a specific camera with your order.

### Micro SD Card (TF Card) (optional)

Some Flash memory within the the device is reserved for a file system, but this internal memory is very slow!  For quicker operation and additional storage, you can insert a `Micro SD` card or a `TF card` into the card slot available on most Maix devices.

When purchasing a memory card, try to choose a new fast Micro SD card, such as a SD 2 generation protocol, Class10 memory card.

Of course, the quality of SD cards on the market is uneven, and the SPI mode may not be compatible. Try to buy a regular card. Or maybe you should customize the driver code ~~

As shown below, the two cards on the left are not supported by the MaixPy driver. Both the middle and the right cards are supported, but the class10 card in the middle is the fastest(and tested maximum capacity is 128GB).

![](../../assets/TF.png)

### ST-Link (used to update the firmware of the STM32 on the development board Maix Go) (optional)

If you purchase a `Maix Go`, it has an embedded `STM32` chip to simulate the USB-to-Serial converter, as well as `JTAG`. If you want to upgrade its firmware later on, it is recommended to buy an `ST-Link` programmer.

### JTAG Debugger (optional)

The `K210` chip supports `JTAG` debugging.  If you need to debug, you will need to use the `JTAG` debugger. Please check the `Sipeed` Taobao store or SeeedStudio.com to buy one.

If you are using a `Maix Go` development board, you won't need to purchase the `JTAG` debugger separately as it has an integrated `STM32` chip that can emulate `JTAG` (`STM32` uses `CMSIS-DAP` or `open-ec` firmware).  `open-ec` firmware is currently not supported, although support will be added later.  Please refer to the `open-ec` GitHub project page for more information.
