代码结构
========

本文主要介绍 Micropython 代码中 port 目录下的 `k210-freertos` 文件夹，也就是kendryte210平台的代码结构。

## 目录简介

简单介绍 `k210-freertos` 中的文件及目录

### 文件

#### 代码文件

`main.c` 是程序入口的代码之外，如果贡献开发者需要对 main 函数进行修改我们就可以在这个文件中进行编辑

#### 脚本文件

一般情况下，请不要对脚本文件进行修改，以免发生意外情况

`config.sh` 是编译时需要使用的配置脚本,主要是使用 export 配置shell环境中变量以让编译正常进行

`build.sh` 是用于编译代码，代码编译请看 `代码编译` 章节

`clean_inc.sh` 用于编译过程中生成的所有mk文件

`flash.sh` 用于 linux 环境下烧写程序

#### mk文件

mk文件是在编译过程中生成的中间文件，一般用于包含代码文件的路径

### 文件夹

#### 代码文件夹

以下目录是用于存放 `MaixPy` 的所有代码

`mpy_support` 存放 MicroPython 与 Openmv 相关的代码

`platform` 存放和平台相关的代码，如sdk,驱动等等

`third_party` 存放可移植的第三方代码，比如spiffs

#### 其他文件夹

`output` 存放编译输出文件

`tools` 存放开发过程需要用的工具脚本，比如打包文件系统

`build` 存放编译过程中.o文件，所有代码文件夹下都带有一个 build 文件夹，作用都是存放.o文件

`inc` 存放各个子目录的中间的mk文件，一般情况不修改该目录下的文件

`docs` 存放文档和例程demo


## 目录详解

以下我们将详细说明在开发过程中我们经常用到的目录，以方便贡献开发者可以更快上手开发

### mpy_support

`mpy_support` 文件夹是用于存放和 micropython 有关的所有代码，包括 micropython 所有平台移植代码，如 mpconfigport.h、mpconfigboard.h等等。同时也存放所有 micropython 标准模块如 os、time等等，还有常见的硬件模块，如 SPI、IIC等等

#### standard_lib

`standard_lib` 用于存放 micropython 的标准库代码，比如 machine 、 socket 、 os 等等，每个模块都有自己独立的文件夹，其中 `include` 文件夹存放的是所有的头文件。开发标准模块时可以参考这里

#### omv

`omv` 存放的是与 openmv 移植相关的代码，如果需要开发 lcd 、 sensor 、 image 等模块则可以修改该目录下的文件

#### Maix

`Maix` 存放的是平台特性功能的代码，比如 FPIOA 功能，其他平台少有，所以我们将 FPIOA 的相关代码存放在 Maix 目录下。后期可能开发的功能如 I2S  、 KPU等都存放在这里。

#### builtin-py

`builtin-py` 存放的是MaixPy内置的 Python 文件，Micropython支持将 Python 文件编译到固件中，我们把所有想要内置的 Python 文件都存放在这里以让这些文件可以编译进固件中。比如fm 和 board_info 等功能都是使用这里的 Python 文件实现的

### platform

platform 存放的是所有平台相关代码，比如 sdk 、 驱动等等

#### sdk

`sdk` 存放了 kendryte210 的软件开发工具包，一般情况下我们不需要修改这里的代码

#### drivers 

`drivers` 存放了开发板上常见外设的代码，比如lcd 、 flash 、 sd卡等等

### third_party

`third_party` 存放了所有第三方移植代码，如果在开发过程中需要用到其他第三方的库或者功能，需要把代码存放在该目录下

#### spiffs

`spiffs` 存放了spiffs文件系统的移植代码，在开放过程中，如果需要对 spiffs 文件系统进行配置修改，可以在该目录找到配置文件并修改

### tools

`tool` 是开发过程中使用到的自己编写的功能脚本，比如打包文件系统，去除固件中多余的数组等等，在开发时如果自己编写了脚本，需要将脚本放在该目录下

### output

`output` 目录存放了所有的编译最终结果，包括每个代码文件夹和第三方代码生成的静态库、输出的二进制文件和elf文件，其中我们烧录的就是二进制.bin文件

 









