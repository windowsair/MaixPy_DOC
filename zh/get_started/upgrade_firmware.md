更新 MaixPy 固件
===========



## 连接硬件

连接 Type C 线， 一端到开发板， 一端到电脑

## 安装驱动

主要是安装串口驱动，因为板子是通过 USB 转串口设备与电脑连接。
根据板子的 USB 转串口芯片型号装驱动。

> 在 `Linux` 或者`Mac`下操作串口， 如果不想每次都使用`sudo`命令， 执行`sudo usermod -a -G dialout $(whoami)` 将自己添加到`dialout`用户组即可，可能需要注销或者重启才能生效


### 对于 `Dan Dock` 和 `Maix Bit`

开发板使用了 `CH340` ：

Linux 不需要装驱动，系统自带了，使用`ls /dev/ttyUSB*` 即可看到设备号
Windows 在网上搜索一下下载安装即可，然后可以在`设备管理器`中看到串口设备



### 对于 `Maix Go`

开发板使用了一颗 `STM32` 来实现模拟串口以及 `JTAG`功能

这款 `STM32` 芯片的固件出厂默认采用 [open-ec](https://github.com/sipeed/open-ec) 的固件， 如果没问题，则会出现一个或者两个串口， 比如 `Linux` 下出现两个串口 `/dev/ttyUSB0` 和 `/dev/ttyUSB1`， 下载和访问串口时请使用 `/dev/ttyUSB1`。 Windows 也类似。

如果需要重新烧录这个固件，可以从 [github](https://github.com/sipeed/open-ec/releases) 或者 [官网下载 open-ec 固件](http://dl.sipeed.com/MAIX/tools/flash-zero.bin)， 然后使用 `ST-LINK` 连接板子上引出的 `STM32` 的 `SW` 引脚（`GND`, `SWDIO`, `SWCLK`）进行烧录。（目前版本的 `Go` 板子上的 `STM32` 不支持串口烧录，只能使用 `ST-LINK` 进行烧录， 有需要请自行购买，或者使用一款板子用 `IO` 模拟也可以（比如树莓派） ）

除了 `open-ec` 还有 `CMSIS-DAP` 固件， 相比 `open-ec` 可以模拟 `JTAG` 来对板子进行调试， `open-ec` 目前还未支持模拟 `JTAG`， 可以 [从官网下载固件](http://dl.sipeed.com/MAIX/tools/maix_go_cmsisdap_new.hex)， 使用 `ST-LINK` 对其进行烧录， 在 `Linux` 下会出现 `/dev/ttyACM0` 设备

> ST-LINK 对 `STM32` 的烧录方法资料很全，请自行搜索





## 获得升级工具

### Ubuntu(Linux)

下载工具：[kflash.py](https://github.com/sipeed/kflash.py)

```bash
sudo apt update
sudo apt install git python3 python3-pip
sudo pip3 install pyserial
git clone https://github.com/sipeed/kflash.py
```

### Windows

K-Flash： 从 [github](https://github.com/kendryte/kendryte-flash-windows/releases) 下载

或者从 [kendryte](https://kendryte.com/downloads/) 官方页面下载

如果是 `Maix Go` 开发板， 目前只能使用 `kflash.py` 脚本进行烧录， 所以需要电脑安装 `python3`，

然后下载[kflash.py](https://github.com/sipeed/kflash.py)



## 获得固件

从 [github](https://github.com/sipeed/MaixPy/releases) 页面下载

固件为 `.bin` 结尾或者 `.kfpkg` 的文件



## 下载固件到开发板

### Linux

使用如下命令来进行烧录，可以使用`python3 kflash.py --help`来获取帮助

```
sudo python3 kflash.py -p /dev/ttyUSB0 -b 2000000 -B dan firmware.bin
```
*  `-p` 是指定设备， 可以通过`ls /dev/ttyUSB*`来查看设备
* `-b`是指定波特率， 如果下载失败，可以降低波特率再次尝试
* `-B`是指定板子，如果没有支持的型号也不用担心，仍然可以下载，只是下载完后可能需要手动复位才能启动。 其中 `Maix Go` 使用 `-B goD`(`STM32` 烧录了 `CMSIS-DAP` 固件) 或者 `-B goE`(`STM32` 烧录了 `open-ec` 固件)

> `Maix Go` 如果确认选项是对的，仍然无法下载， 可以尝试将三相拨轮按键拨向 `Down` 的位置并保持再下载
![Go Key Down](../../assets/Go_Key_Down.png)


### Windows

双机运行下载的软件，运行后选择固件、串口等，点击下载即可

![kflash windows](../../assets/kflash_win.png)

如果开发板是 `Maix Go`， 目前只能使用 `kflash.py` 进行下载， 下载方法同 `Linux` 上的方法， 参见上面所述


