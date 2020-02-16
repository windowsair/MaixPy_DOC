MaixPy Frequently Asked Questions
=====

## What are the similarities and differences between MaixPy and C development, how do I choose

MaixPy is a scripting language based on Micropython, which does not require compilation and is parsed at runtime. It is easier and more convenient to write, but the runtime is not as good as the C language.
So if you are fast verification, novice, only python, less hair, etc., you can use MaixPy; you can use C language for the pursuit of extreme performance efficiency or familiar with C

## MaixPy IDE cannot successfully connect to the development board

* Check if the serial port is occupied
* After clicking the connection, do not use it with the terminal tool at the same time, otherwise the serial port will be occupied and cannot be opened
* If you have been unable to connect successfully, check:
  * Please check if the development board model is wrong;
  * Observe whether the screen of the development board has changed. If there is no response, it may be that the serial port is selected incorrectly;
  * Try to upgrade to the latest [master branch firmware](http://cn.dl.sipeed.com/MAIX/MaixPy/release/master), and the latest MaixPy IDE software


## The document web page cannot be opened, it is slow

If some pages cannot be accessed, please check the URL (path) is correct, you can return to the homepage (`maixpy.sipeed.com`) and enter again.

For example, this URL is caused by clicking too fast:
`` `
http://localhost:4000/zh/zh/get_started/how_to_read.html
`` `
The correct URL should be:
`` `
http://localhost:4000/zh/get_started/how_to_read.html
`` `

In addition, you can try another network line, such as hanging a proxy, or trying mobile phone traffic.

## Download station file download speed is slow, the file cannot be downloaded

If you experience slow download speed on the dl.sipeed.com download site, you can use the domestic synchronization server cn.dl.sipeed.com to download, the path is the same, and sync once a day;
Some files provide CDN download links, which will be faster. For example, the IDE has a description in readme.txt

## Micro SD card cannot be read

Format Micro SD to FAT format(not FAT32), and try again.

At present the hardware can only support SPI protocol reading, try to buy a formal card

For example: The two cards on the left in the figure below are not supported by the MaixPy driver, the middle and right are supported, but the class10 card in the middle is the fastest (up to 128GB available)
> In addition, I tested several SanDisk, Kingston, Samsung cards purchased online, and found that one Samsung card is unusable

![](../../assets/TF.png)

## Why is the IDE frame rate lowered a lot?

The K210 has no USB peripherals, so it can only communicate with the IDE using a serial port, which is not as fast as a USB device, so it will affect the frame rate. You can turn off the camera preview of the IDE.

## Why the preview camera image on the IDE is blurry

The K210 has no USB peripherals, so it can only communicate with the IDE using a serial port. The speed is not as fast as a USB device. Therefore, the pictures are compressed. If you need to see a clear image, please see it on the screen of the development board, or save it as a picture and transfer it to the computer for viewing

## How to increase the camera frame rate

* Change to a better camera, for example `ov7740` will have a higher frame rate than` ov2640`. But the premise is that the camera circuit must be compatible with the circuit of the development board
* Increase the camera clock frequency(`sensor.reset(freq=)`), but be careful not to be too high, it will make the picture worse
* You can compile the source code yourself, open the camera double buffer, the frame rate will increase, but correspondingly, the memory consumption will also increase (about 384k)


