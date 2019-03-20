NES game emulator
=======

Classic NES game simulator, take us back to childhood ~~


Or? Let us find a way to let it play itself?


## Function

### init(rc_type=nes.KEYBOARD, cs, mosi, miso, clk, repeat=16, vol=5)

Initialize `NES` emulator

#### Parameters

* `tc_type`： remote controller type,  keyboard（`nes.KEYBOARD`）（pay attention just serial port, not USB keyboard connected to borad~） or PS2 joystick（`nes.JOTSTICK`）。 
> Recommend `PS2` joystick， better experience， serial tools can not push multiple key once a time, or write your own script to forward keys, maybe find [here](https://github.com/sipeed/MaixPy_scripts/tree/master/tools_on_PC) for some script?）

* `cs`： If use `PS2` joystick with `SPI` interface, give `cs` pin number
* `mosi`：  If use `PS2` joystick with `SPI` interface, give `mosi` pin number
* `miso`：  If use `PS2` joystick with `SPI` interface, give `miso` pin number
* `clk`：   If use `PS2` joystick with `SPI` interface, give `clk` pin number
* `repeat`： Just for keyboard mode, key repeat times
* `vol`： Init volume, you can change by shortcut later


### run(nes)

Run `NES` game(`ROM`)

#### Parameters

* `nes`：  File path of game's `ROM` ， `/sd/mario.nes` for example


## Shortcut

### Keyboard(/serial port)

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
* `eixt` ： no
* `volume -` ： `R2`
* `volume +` ： `R1`
* `run speed -` ： `L1`
* `run speed +` ： `L2`


## Examples

## Demo 1： Keyboard( Sefial port)

```python
import nes, lcd

lcd.init(freq=15000000)
nes.init(nes.KEYBOARD)
nes.run("/sd/mario.nes")

```

## Demo 2： PS2 jotstick

```python
import nes, lcd

lcd.init(freq=15000000)
nes.init(nes.JOYSTICK, cs=19, clk=18, mosi=23, miso=21)
nes.run("/sd/mario.nes")

```





