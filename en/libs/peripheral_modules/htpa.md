HTPA Thermal Infrared Temperature Measurement Module (modules.htpa)
====

Hyman HTPA 32x32 Thermal Infrared Questioning Module

![](../../../assets/htpa.png)

## Construction method htpa (i2c, scl_pin, sda_pin, i2c_freq)

Create an instance

### Parameters

* `i2c`: I2C number, such as` I2C.I2C0`, value [0, 2] (see `machine.I2C`)
* `scl_pin`: I2C SCL pin
* `sda_pin`: I2C SDA pin
* `i2c_freq`: I2C clock frequency


### return value

htpa object


## Instance method temperature()

Get the sensor temperature value, which can only be called by the instance

### return value

Array, the length is the width x height of the sensor, such as `32x32`

## Instance method width()

Gets the sensor resolution width, which can only be called by the instance

### return value

Integer, width

## instance method height()

Gets the sensor resolution width, which can only be called by the instance


## Examples

[heimann_HTPA_32x32](https://github.com/sipeed/MaixPy_scripts/tree/master/modules/heimann_HTPA_32x32)