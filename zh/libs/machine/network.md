network
=========
该模块用于初始化各种网卡驱动，网卡具有连接路由，断开路由，查看网卡连接信息，检查是否连接等功能。目前仅支持ESP8255模块网卡

## 1. 构造函数

构造网卡对象，使用该方法需要传入一个uart对象，在MaixPy目前支持的dock和GO上，是使用AT指令模块作为wifi。所以该uart对象是与AT模块通信的对象，可以查看uart模块例程
```
import network
nic = network.ESP8285(uart)
```

#### 1.1. 参数

* `uart`: 与AT模块通信的UART对象

#### 1.2. 返回值

* `nic`: 网卡对象

## 2. 网卡方法

使用该方法前请确保已经接上了天线

### 2.1. connect

连接路由器

```
nic.connect(uuid,password)
```

#### 参数

* `uuid`: 路由器的uuid
* `password`: 连接路由器的密码

#### 返回值

无

### 2.2. ifconfig

查看wifi连接信息，目前network不支持设置网卡配置

```
nic.ifconfig()
```

#### 参数

无

#### 返回值

* `tuple` 类型的字符串，如果失败的话元组的成员全部为0

### 2.3. isconnected

查看wifi是否连接

```
nic.isconnected()
```

#### 参数

无

#### 返回值

`True`: 已经连接
`False`: 断开连接

### 2.4. disconnect

断开wifi连接
```
nic.disconnect()
```

#### 参数

无

#### 返回值

无


## 例程


### 例程 1 

```python
import network
from machine import UART
fm.register(board_info.WIFI_RX,fm.fpioa.UART2_TX)
fm.register(board_info.WIFI_TX,fm.fpioa.UART2_RX)
uart = UART(UART.UART2,115200,timeout=1000, read_buf_len=4096)
nic=network.ESP8285(uart)
nic.connect("your_uuid","you_password.")
nic.ifconfig()
nic.isconnected()
```
