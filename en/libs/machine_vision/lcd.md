Lcd screen display driver
====



## function

### lcd.init(type=1)

Initialize the `LCD` screen to display black

#### Parameters

* `type`: type of `LCD` (reserved for future use):
  * `0`: None
  * `1`: lcd shield (default)
> type is a key-value parameter that must be explicitly called by writing type= in the function call

### lcd.deinit()

Unregister the `LCD` driver to release the I/O pins

### lcd.width()

Returns the width of the `LCD` (horizontal resolution)


### lcd.height()

Returns the height of the `LCD` (vertical resolution).


### lcd.type()

Returns the type of `LCD` (reserved for future use):

0: None
1: lcd Shield


### lcd.set_backlight(state)

Setting the backlight status of `LCD`, turning off the backlight will greatly reduce the energy consumption of the LCD expansion board.

> //TODO: Not implemented

#### Parameters

* `state`: backlight brightness, value [0,100]

### lcd.get_backlight()

Return to backlight status

#### return value

Backlight brightness, value [0,100]

### lcd.display(image, roi=Auto)

Display a `image` (GRAYSCALE or RGB565) on the LCD.

Roi is a rectangular tuple (x, y, w, h) of a region of interest. If not specified, it is an image rectangle

If the roi width is less than the lcd width, the vertical black border is used to make roi at the center of the screen (that is, fill the unoccupied area with black).

If the roi width is greater than the lcd width, roi is at the center of the screen, and the unmatched pixels are not displayed (ie, the LCD displays the center of roi in window form).

If the roi height is less than the lcd height, use a vertical black border to center roi in the center of the screen (ie fill the unoccupied area with black).

If the roi height is greater than the lcd height, roi is at the center of the screen, and the unmatched pixels are not displayed (ie, the LCD displays the center of roi in window form).

> roi is a key-value parameter that must be explicitly called by writing roi= in a function call.

### lcd.clear()

Empty the LCD screen to black.




## Routine

### Routine 1: Display English

```python
import lcd

lcd.init()
lcd.lcd.draw_string(100, 100, "hello maixpy", lcd.RED, lcd.BLACK)

```

### Routine 2: Displaying pictures

```python
import lcd
import image

img = image.Image("/sd/pic.bmp")
lcd.display(img)
```

### Routine 3: Display English in the form of displaying pictures

```python
import lcd
import image

img = image.Image()
img.draw_string(60, 100, "hello maixpy", scale=2)
lcd.display(img)
```

### Routine 4: Real-time display of images captured by the camera

```python
import sensor, lcd

sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.skip_frames()
sensor.run(1)
lcd.init()

while(True):
    lcd.display(sensor.snapshot())
```