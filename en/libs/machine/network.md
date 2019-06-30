network
=========

该模块用于初始化各种网卡驱动，网卡具有连接路由，断开路由，查看网卡连接信息，检查是否连接等功能。

使用`WiFi`请确保已经接上了天线


This module is used to initialize various network card drivers. The network card has these function: connecting routes, disconnecting routes, viewing network card connection information, checking whether connections are made. 

Please make sure the antenna is connected before using WiFi. 


### [esp8285](#ESP8285_Module)
在部分开发板上带了 一个 使用`AT`方式交互的网卡模块，比如`esp8285`，与`k210`通过串口连接

引脚`8`是使能脚，可以创建一个`GPIO`对象来控制它的高低电平来实现使能和失能，也可以用它复位（先低后高），复位后需要等待一小段时间才能操作，
可以查看例程[wifi_ap_scan.py](https://github.com/sipeed/MaixPy_scripts/blob/master/network/demo_wifi_ap_scan.py)


On some development boards, there is a network card module that uses the `AT` method to interact, such as `esp8285`, which is connected to the `k210` through the serial port.

Pin `8` is the enable pin. You can create a `GPIO` object to control its high and low levels to enable and disable. It can also be reset (low and then high). Wait for a short period after reset. 

Sample code [wifi_ap_scan.py] (https://github.com/sipeed/MaixPy_scripts/blob/master/network/demo_wifi_ap_scan.py)


### [esp32](#ESP32_Module)
目前在`MaixDuino`开发板中有一个 `esp32` 模块通过 `spi` 与`k210`相连
同时也有单独的`TF`插卡式模块


There is currently a `esp32` module in the `MaixDuino` development board connected to `k210` via `spi`
There is also a separate `TF` plug-in module


ESP8285_Module
=========

## network.ESP8285(uart)

构造一个`ESP8285`网卡对象，使用该方法需要传入一个`uart`对象，在`MaixPy`目前支持的`dock`和`GO`上，是使用AT指令模块作为`WiFi`。所以该`uart`对象是与`AT`模块通信的对象，可以查看`uart`模块例程

调用此方法会初始化`ESP8285`， 如果失败会抛出异常


Construct an `ESP8285` NIC object. To use this method, you need to pass in a `uart` object. On the `dock` and `GO` currently supported by `MaixPy`, use the AT command module as `WiFi`. So the `uart` object is the object that communicates with the `AT` module, you can view the `uart` module routine.

Calling this method will initialize `ESP8285`, throwing an exception if it fails


### 参数

* `uart`: 与AT模块通信的UART对象

* `uart`: UART object that communicates with the AT module

### 返回值

* `ESP8285`: 网卡对象

* `ESP8285`: NIC object


## ESP8285

### connect(ssid, key)

连接热点（AP/路由器）

Connection hotspot (AP/router)

#### 参数
#### Parameters

* `ssid`: 热点的`SSID`
* `key`: 热点的密码

* `ssid`: hotspot `SSID`
* `key`: password for hotspot

#### Return Value

无， 如果发生错误会抛出异常

None, an exception will be thrown if an error occurs

### 2.2. ifconfig

查看wifi连接信息，目前network不支持设置网卡配置

Check the wifi connection information. Currently, the network does not support setting the network card configuration.

```
nic.ifconfig()
```

#### Parameters

None

#### Return Value

`tuple` 类型， 元素都是字符串：`(ip, netmask, gateway, dns_server, dhcp_server, mac, ssid)`， 如果没有查询到或者无效，值为`"0"`
`tuple` type, the elements are all strings: `(ip, netmask, gateway, dns_server, dhcp_server, mac, ssid)`, if not queried or invalid, the value is `"0".

### isconnected

查看wifi是否连接
Check if wifi is connected

```
nic.isconnected()
```

#### Parameters

None

#### Return Value

`True`: 已经连接
`False`: 断开连接

`True`: already connected
`False`: Disconnect

### disconnect

断开wifi连接
Disconnect wifi

#### Parameters

None

#### Return Value

None

### scan

扫描周围的热点信息

Scan surrounding hotspot information

#### Parameters

None

#### Return Value

一个 `list`对象， 每个元素包含了一个字符串， 字符串来自`AT`模块的响应，内容和`esp8285`的`AT指令文档`所描述的相同，如下：
`'<ecn>,<ssid>,<rssi>,<mac>,<channel>,<freq	offset>,<freq cali>,<pairwise_cipher>, <group_cipher>,<bgn>,<wps>'`

A `list` object, each element containing a string, the string comes from the response of the `AT` module, the content is the same as described in `esp8285` `AT directive document`, as follows:
`'<ecn>,<ssid>,<rssi>,<mac>,<channel>,<freq offset>,<freq cali>,<pairwise_cipher>, <group_cipher>,<bgn>,<wps>'`

* `<ecn>`：加密⽅式
  * 0：OPEN
  * 1：WEP
  * 2：WPA_PSK
  * 3：WPA2_PSK
  * 4：WPA_WPA2_PSK
  * 5：WPA2_Enterprise（⽬前 AT 不⽀持连接这种加密 AP）
* `<ssid>`：字符串参数，AP 的 SSID
* `<rssi>`：信号强度
* `<mac>`：字符串参数，AP 的 MAC 地址
* `<channel>`：信道号
* `<freq	offset>`：AP 频偏，单位：kHz。此数值除以 2.4，可得到 ppm 值
* `<freq	cali>`：频偏校准值
* `<pairwise_cipher>`:
  * 0：CIPHER_NONE
  * 1：CIPHER_WEP40
  * 2：CIPHER_WEP104
  * 3：CIPHER_TKIP
  * 4：CIPHER_CCMP
  * 5：CIPHER_TKIP_CCMP
  * 6：CIPHER_UNKNOWN
* `<group_cipher>`: 定义与 `<pairwise_cipher>` 相同
* `<bgn>`: bit0 代表 b 模式; bit1 代表 g 模式; bit2 代表 n 模式
         若对应 bit 为 1，表示该模式使能；若对应 bit 为 0，则该模式未使能。
* `<wps>`：0，WPS 未使能；1，WPS 使能

* `<ecn>`: encryption method
   * 0: OPEN
   * 1: WEP
   * 2: WPA_PSK
   * 3: WPA2_PSK
   * 4: WPA_WPA2_PSK
   * 5: WPA2_Enterprise (current AT does not support connection to this encrypted AP)
* `<ssid>`: string parameter, SS ID of AP
* `<rssi>`: signal strength
* `<mac>`: string parameter, AP's MAC address
* `<channel>`: channel number
* `<freq offset>`: AP frequency offset, unit: kHz. This value is divided by 2.4 to get the ppm value.
* `<freq cali>`: frequency offset calibration value
* `<pairwise_cipher>`:
   * 0: CIPHER_NONE
   * 1: CIPHER_WEP40
   * 2: CIPHER_WEP104
   * 3: CIPHER_TKIP
   * 4: CIPHER_CCMP
   * 5: CIPHER_TKIP_CCMP
   * 6: CIPHER_UNKNOWN
* `<group_cipher>`: definition is the same as `<pairwise_cipher>`
* `<bgn>`: bit0 stands for b mode; bit1 stands for g mode; bit2 stands for n mode
          If the corresponding bit is 1, it indicates that the mode is enabled; if the corresponding bit is 0, the mode is not enabled.
* `<wps>`:0, WPS is not enabled; 1, WPS is enabled

比如： 
example:
```
info_strs = ['4,"ChinaNet-lot0",-79,"c8:50:e9:e8:21:3e",1,-42,0,4,3,7,1', '4,"TOPSTEP2G4",-7
0,"f8:e7:1e:0d:0d:f8",1,-57,0,4,4,7,0']
```
这看起来可能会比较奇怪，因为每个AP的信息都是一串字符，信息里面还有整型和字符串，字符串用双引号括起来的，所以拿到这个字符串后需要再次处理后再使用，比如：
This may seem strange, because each AP's information is a string of characters, there are integers and strings in the message, the string is enclosed in double quotes, so you need to deal with it again after getting this string. Use again, such as:
```python
def wifi_deal_ap_info(info):
    res = []
    for ap_str in info:
        ap_str = ap_str.split(",")
        info_one = []
        for node in ap_str:
            if node.startswith('"'):
                info_one.append(node[1:-1])
            else:
                info_one.append(int(node))
        res.append(info_one)
    return res

info_strs = ['4,"ChinaNet-lot0",-79,"c8:50:e9:e8:21:3e",1,-42,0,4,3,7,1', '4,"TOPSTEP2G4",-70,"f8:e7:1e:0d:0d:f8",1,-57,0,4,4,7,0']

info = wifi_deal_ap_info(info_strs)
print(info)
```

输出是：
The output is:

```
[[4, 'ChinaNet-lot0', -79, 'c8:50:e9:e8:21:3e', 1, -42, 0, 4, 3, 7, 1], [4, 'TOPSTEP2G4', -70, 'f8:e7:1e:0d:0d:f8', 1, -57, 0, 4, 4, 7, 0]] 
```

然后比如我们需要获得所有`AP`的`SSID`只需要使用
Then for example we need to get all `AP` `SSID` only need to use
```
for ap_info in info:
    print(ap_info[1])
```

### enable_ap(ssid, key, chl=5, ecn=3)

打开热点
Open hotspot ==> Connect to a hotspot?

#### Parameters

* `ssid`: SSID
* `key`： 密码
* `chl`： WiFi信号的通道号
* `ecn`： 加密方法， 有`OPEN``WPA2_PSK`等，参考本页`ESP8285`的常量部分， 默认值是`3`， 也就是`ESP8285.WPA2_PSK`，比如
```python
nic = network.ESP8285(uart)
nic.enable_ap("maixpy", "12345678", 5, nic.OPEN)
```
或者
```
nic.enable_ap("maixpy", "12345678", 5, network.ESP8285.OPEN)
```

* `ssid`: SSID
* `key`: password
* `chl`: channel number of the WiFi signal
* `ecn`: encryption method, there are `OPEN``WPA2_PSK`, etc., refer to the constant part of this page `ESP8285`, the default value is `3`, which is `ESP8285.WPA2_PSK`, for example
```python
Nic = network.ESP8285(uart)
Nic.enable_ap("maixpy", "12345678", 5, nic.OPEN)
```
or
```
Nic.enable_ap("maixpy", "12345678", 5, network.ESP8285.OPEN)
```


### disable_ap()

关闭热点
Close hotspot


### 常量
### Constant

#### OPEN

热点的加密方式为不需要密码
Hotspot encryption is no password required

#### WPA_PSK

热点的加密方式为 `WPA_PSK`
The hotspot encryption method is `WPA_PSK`

#### WPA2_PSK

热点的加密方式为 `WPA2_PSK`
The hotspot encryption method is `WPA2_PSK`

#### WPA_WPA2_PSK

热点的加密方式为 `WPA_WPA2_PSK`
The hotspot encryption method is `WPA_WPA2_PSK`

## 例程
## Routine


参考[network目录下的例程](https://github.com/sipeed/MaixPy_scripts/tree/master/network)
Refer to [Routines in the network directory] (https://github.com/sipeed/MaixPy_scripts/tree/master/network)

ESP32_Module
=========

## network.ESP32_SPI(cs,rst,rdy,mosi,miso,sclk)

构造一个`ESP32_SPI`网卡对象，需要传入对应的`GPIOHS FUNC`

如果传入参数数量不对，会返回错误

To construct an `ESP32_SPI` NIC object, you need to pass in the corresponding `GPIOHS FUNC`.

If the number of parameters passed in is incorrect, an error will be returned.


### Parameters

* 对应引脚功能的 `fpioa_func`
* `fpioa_func` for pin function

### return value

* `ESP32_SPI` 网卡对象
* `ESP32_SPI` NIC object


## ESP32_SPI

### adc

读取`esp32`模块的`adc`值
Read the `adc` value of the `esp32` module

#### Parameters

None

#### return value

`tunple`，5个通道的`adc`值<br>顺序是`"PIN36", "PIN39", "PIN34", "PIN35", "PIN32"`
`tunple`, the `adc` value of 5 channels<br> is `"PIN36", "PIN39", "PIN34", "PIN35", "PIN32"`

#### 例程
#### Routine

[demo_esp32_read_adc.py](https://github.com/sipeed/MaixPy_scripts/blob/master/network/demo_esp32_read_adc.py)

