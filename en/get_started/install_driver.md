Install driver
====

We need to install the serial port driver as the board is connected to the computer through the USB to serial converter. Install the driver according to the board's USB to serial port chip model.

> In you are using `Linux` or `Mac` and you don't want to use `sudo` every time, add yourself to the `dialout` users group with the following command: `sudo usermod -a -G dialout $(whoami)`

### For `Dan Dock` or `Maix Bit`

The CH340 chip is being used, Linux does not need to install the driver as the system already comes with it. Execute `ls /dev/ttyUSB*` to check if the device is found. If using Windows, search and download the drivers on the Internet, then open `Device manager` and look if the serial port is listed.

### For `Maix Go`

An `STM32` is being used to implement the serial port and the `JTAG` functionality.

By default, this `STM32` chip is running a build of the [open-ec](https://github.com/sipeed/open-ec) firmware. If everything is right, one or two serial ports will appear. In `linux` the following two serial ports will appear: `/dev/ttyUSB0` and `/dev/ttyUSB1`. Please use `/dev/ttyUSB1` when downloading and accessing the serial port. Windows is similar.

If you need to re-burn this firmware, you can download it from [GitHub](https://github.com/sipeed/open-ec/releases) or [open-ec firmware](http://dl.sipeed.com/MAIX/tools/flash-zero.bin), then use the `STM32`'s `SW` pins (`GND`, `SWDIO`, `SWCLK`) from the `ST-LINK` connection board for programming. (The `STM32` on the current version of the `Go` board does not support serial port burning. It can only be burned using `ST-LINK`. Please purchase it if you need it, or use a board with `IO simulation` such as the Raspberry Pi))

Currently, `open-ec` cannot simulate `JTAG` to debug the board. Use `CMSIS-DAP` to do so. You can download it [from the official website](http://dl.sipeed.com/MAIX/tools/maix_go_cmsisdap_new.hex) and then burn it using an `ST-LINK`. Afterwards, `/dev/ttyACM0` will appear under `linux`.

> ST-LINK has a very complete description of the burning method of `STM32`, please search for yourself.

**Please note that updating the firmware of STM32 is not the same as updating the MaixPy firmware. Generally, you do not need to update the firmware of STM32. The default is enough. STM32 is just a USB to serial port tool! Do not be confused.**

### For the new `Maixduino` and `Maix Bit` versions that come with a microphone (using a `CH552` chip)

For the boards with a `CH552` chip, to get the `USB` serial port, `FT2232` drivers need to be installed. Search yourself for `FT2232 drivers`. In those boards, the `JTAG` function is not available.