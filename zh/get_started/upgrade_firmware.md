更新 MaixPy 固件
===========



## 连接硬件

连接 Type C 线， 一端到开发板， 一端到电脑

## 安装驱动

主要是安装串口驱动，因为板子是通过 USB 转串口设备与电脑连接。
根据板子的 USB 转串口芯片型号装驱动。

比如 Dan_Dock 开发板使用的 CH340 ：
Linux 不需要装驱动，系统自带了，使用`ls /dev/ttyUSB*` 即可看到设备号
Windows 在网上搜索一下下载安装即可，然后可以在`设备管理器`中看到串口设备

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


## 获得固件

从 [github](https://github.com/sipeed/MaixPy/releases) 页面下载



## 下载固件到开发板

### Linux

使用如下命令来进行烧录，可以使用`python3 kflash.py --help`来获取帮助

```
sudo python3 kflash.py -p /dev/ttyUSB0 -b 2000000 -B dan firmware.bin
```
> 其中 `-p` 是指定设备， 可以通过`ls /dev/ttyUSB*`来查看设备
> `-b`是指定波特率， 如果下载失败，可以降低波特率再次尝试
> `-B`是指定板子，如果没有支持的型号也不用担心，仍然可以下载，只是下载完后可能需要手动复位才能启动

> 如果不想每次都使用`sudo`命令， 执行`sudo usermod -a -G dialout $(whoami)` 将自己添加到`dialout`用户组即可，可能需要注销或者重启才能生效

### Windows

双机运行下载的软件，运行后选择固件、串口等，点击下载即可

![kflash windows](../../assets/kflash_win.png)

