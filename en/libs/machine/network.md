network
=========


This module is used to initialize various network card drivers. The network card has these function: connecting routes, disconnecting routes, viewing network card connection information, checking whether connections are made. 

Please make sure the antenna is connected before using WiFi. 


### [esp8285](#ESP8285_Module)

Some development boards expose an `AT` network interface module, such as an `esp8285` interface connected to the `k210` processor via a serial port.

Pin `8` is the enable pin. Instantiate a `GPIO` object set it high to enable, low to disable. To reset, set it low, wait a short while, then set it high. 

Sample code [wifi_ap_scan.py] (https://github.com/sipeed/MaixPy_scripts/blob/master/network/demo_wifi_ap_scan.py)


### [esp32](#ESP32_Module)

There is currently a `esp32` module in the `MaixDuino` development board connected to `k210` via `SPI`. 
There is also a separate `TF` plug-in module. 


ESP8285_Module
=========

## network.ESP8285(uart)

Instantiate an `ESP8285` NIC object, passing in an `uart` object. On the `dock` and `GO` currently supported by `MaixPy`, use the AT command module as `WiFi`. So the `uart` object is the object that communicates with the `AT` module, you can view the `uart` module routine.

Calling this method will initialize `ESP8285`, throwing an exception if it fails. 


### Parameters

* `uart`: UART object that communicates with the AT module

### Return Value

* `ESP8285`: NIC object


## ESP8285

### connect(ssid, key)

Connect to a hotspot (AP/router). 

#### Parameters

* `ssid`: hotspot `SSID`
* `key`: password for hotspot

#### Return Value

None. An exception will be thrown if an error occurs. 

### 2.2. ifconfig

Display the current WiFi connection information. Currently, the network does not support setting the network card configuration.

```
nic.ifconfig()
```

#### Parameters

None

#### Return Value

`tuple` type, the elements are all strings: `(ip, netmask, gateway, dns_server, dhcp_server, mac, ssid)`, if not queried or invalid, the value is `"0".

### isconnected

Check if WiFi is connected. 

```
nic.isconnected()
```

#### Parameters

None

#### Return Value

`True`: Already connected
`False`: Disconnected

### disconnect

Disconnect WiFi. 

#### Parameters

None

#### Return Value

None

### scan

Scan for surrounding WiFi hotspots. 

#### Parameters

None

#### Return Value

A `list` object, each element containing a string, the string comes from the response of the `AT` module, the content is the same as described in `esp8285` `AT directive document`, as follows:
`'<ecn>,<ssid>,<rssi>,<mac>,<channel>,<freq offset>,<freq cali>,<pairwise_cipher>, <group_cipher>,<bgn>,<wps>'`

* `<ecn>`: encryption method
   * 0: OPEN
   * 1: WEP
   * 2: WPA_PSK
   * 3: WPA2_PSK
   * 4: WPA_WPA2_PSK
   * 5: WPA2_Enterprise (current AT does not support connection to this encrypted AP)
* `<ssid>`: string parameter, SSID of AP
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

example:
```
info_strs = ['4,"ChinaNet-lot0",-79,"c8:50:e9:e8:21:3e",1,-42,0,4,3,7,1', '4,"TOPSTEP2G4",-7
0,"f8:e7:1e:0d:0d:f8",1,-57,0,4,4,7,0']
```
This may seem strange, because each AP's information is a string of characters, there are integers and strings in the message, the string is enclosed in double quotes, so you need to deal with it again after getting this string, for example:

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

The output is:

```
[[4, 'ChinaNet-lot0', -79, 'c8:50:e9:e8:21:3e', 1, -42, 0, 4, 3, 7, 1], [4, 'TOPSTEP2G4', -70, 'f8:e7:1e:0d:0d:f8', 1, -57, 0, 4, 4, 7, 0]] 
```

Then for example to get every `AP` `SSID`:

```
for ap_info in info:
    print(ap_info[1])
```

### enable_ap(ssid, key, chl=5, ecn=3)

Connect to the specified WiFi hotspot. 

#### Parameters

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

Disconnect from the currently connected WiFi hotspot. 


### Constants

#### OPEN

No encryption and no password required. 

#### WPA_PSK

`WPA_PSK` encryption

#### WPA2_PSK

`WPA2_PSK` encryption

#### WPA_WPA2_PSK

`WPA_WPA2_PSK` encryption


## Code

Refer to [code in the network directory] (https://github.com/sipeed/MaixPy_scripts/tree/master/network)

ESP32_Module
=========

## network.ESP32_SPI(cs,rst,rdy,mosi,miso,sclk)

To instantiate an `ESP32_SPI` NIC object, you need to pass in the corresponding `GPIOHS FUNC`.

If the number of parameters passed in is incorrect, an error will be returned.


### Parameters

* `fpioa_func` for pin function

### Return Value

* `ESP32_SPI` NIC object


## ESP32_SPI

### adc

Read the `adc` value of the `esp32` module

#### Parameters

None

#### Return Value

`tuple`, the `adc` value of 5 channels is `"PIN36", "PIN39", "PIN34", "PIN35", "PIN32"`

#### Code

[demo_esp32_read_adc.py](https://github.com/sipeed/MaixPy_scripts/blob/master/network/demo_esp32_read_adc.py)

