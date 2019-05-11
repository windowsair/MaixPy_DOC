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

下载 [kflash_gui](https://github.com/sipeed/kflash_gui/releases), 会得到一个压缩包

解压到一个文件夹，双机 `kflash_gui.exe`(/`kflsh_gui`) 即可运行， `Windows`下建议右键`固定到开始页面` 或者`固定到任务栏`， `Linux` 下可以自己新建一个[kflash_gui.desktop](https://github.com/sipeed/kflash_gui/blob/master/kflash_gui.desktop)， 修改文件地址， 使用管理员身份复制到`/usr/share/application`目录，然后在系统菜单界面就可以看到`kflash_gui`这款应用了


## 获得固件

从 [github](https://github.com/sipeed/MaixPy/releases) 页面下载

固件为 `.bin` 结尾或者 `.kfpkg` 的文件

如何打包`kfpkg`见 [这里](http://blog.sipeed.com/p/390.html)

固件命名说明：

* maixpy_v*_no_lvgl.bin： MaixPy固件, 不带LVGL版本.(LVGL是嵌入式GUI框架, 写界面的时候需要用到)
* maixpy_v*_full.bin： 完整版的MaixPy固件(MicroPython + OpenMV API + lvgl )
* maixpy_v0.3.1_minimum.bin： MaixPy固件最小集合，不支持 `MaixPy IDE`， 不包含`OpenMV`的相关算法
* face_model_at_0x300000.kfpkg： 人脸模型，放置在地址位 0x300000， 可以和`.bin`分开多次下载，不冲突
* elf.7z： elf 文件，普通用户不用关心，用于死机调试



## 下载固件到开发板

* 打开 `kflash_gui` 应用

* 然后选择固件、设置选项， 点击下载即可， 更多特性介绍、使用说明见[kflash_gui 项目主页](https://github.com/sipeed/kflash_gui)

![](../../assets/kflash_gui_screenshot_1.png)

![](../../assets/kflash_gui_screenshot_download.png)




> 对于早期的 `Maix Go`， 如果确认选项是对的，仍然无法下载， 可以尝试将三相拨轮按键拨向 `Down` 的位置并保持再下载
![Go Key Down](../../assets/Go_Key_Down.png)



