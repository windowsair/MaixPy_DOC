NES game emulator
=======

Classic NES game emulator, take us back to childhood!

Or... Let us find a way to let it play itself!

## Function

### init(rc_type=nes.KEYBOARD, cs, mosi, miso, clk, repeat=16, vol=5)

Initializes the `NES` emulator

#### Parameters

* `tc_type`： Remote control type,  keyboard（`nes.KEYBOARD`） （The serial port communicates with the computer's keyboard, it's not directly connected to the board's USB port...） or PS2 joystick（`nes.JOYSTICK`）。 
> A `PS2` joystick is recommended for a better experience. Serial communication can't send more than one key at a time. If you want, you can try to write your own scripts [here](https://github.com/sipeed/MaixPy_scripts/tree/master/tools_on_PC)

* `cs`： If using a `PS2` joystick with `SPI` interface, enter the `cs` pin number
* `mosi`： If using a `PS2` joystick with `SPI` interface, enter the `mosi` pin number
* `miso`： If using a `PS2` joystick with `SPI` interface, enter the `miso` pin number
* `clk`： If using a `PS2` joystick with `SPI` interface, enter the `clk` pin number
* `repeat`： (Only for keyboard mode!) key repetition rate
* `vol`： Initial volume, can be adjusted later

### run(nes)

Run a `NES` game (`ROM`)

#### Parameters

* `nes`： File path of the game's `ROM` ， `/sd/mario.nes` for example

## Shortcuts

### Keyboard (serial port)

* `move` ： `W A S D`
* `A` ： `J`
* `B` ： `K`
* `start` ： `M` or `Enter`
* `option`： `N` or `\`
* `exit` ： `ESC`
* `volume -` ： `-`
* `volume +` ： `=`
* `run speed -` ： `R`
* `run speed +` ： `F`

### Joystick

* `move` ： `<-` `^` `V` `->`
* `A` ： `□`
* `B` ： `×`
* `start` ： `START`
* `select`： `SELECT`
* `exit` ： no
* `volume -` ： `R2`
* `volume +` ： `R1`
* `run speed -` ： `L1`
* `run speed +` ： `L2`


## Examples

## Demo 1： Keyboard (Serial port)

```python
import nes, lcd

lcd.init(freq=15000000)
nes.init(nes.KEYBOARD)
nes.run("/sd/mario.nes")

```

## Demo 2： PS2 joystick

```python
import nes, lcd

lcd.init(freq=15000000)
nes.init(nes.JOYSTICK, cs=19, clk=18, mosi=23, miso=21)
nes.run("/sd/mario.nes")

```
