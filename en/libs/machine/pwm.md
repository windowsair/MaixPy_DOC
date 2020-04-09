machine.PWM
=========

PWM: Pulse width modulation module, hardware-supported PWM, can specify any pin (0 to 47 pins)

Each PWM depends on a timer, that is, when the timer is bound to the PWM function, it cannot be used as an ordinary timer. Because there are 3 timers and each timer has 4 channels, that is, a maximum of 12 PWM waveforms can be generated at the same time.

## Constructor

`` `python
class machine.PWM (tim, freq, duty, pin, enable = True)
`` `

Create a new PWM object with the specified parameters

### Parameters

* `tim`: Each PWM relies on a timer to generate a waveform, so a timer object needs to be passed here. The timer ID and channel number must be specified when the timer object is initialized.
* `freq`: PWM waveform frequency
* `duty`: PWM duty cycle, refers to the percentage of high level in the entire cycle, value: [0,100]
* `[pin]`: PWM output pin. Instead of setting it, use [fm] (../ builtin_py / fm.md) to manage the pin mapping uniformly.
* `enable`: Whether to start generating the waveform immediately, the default bit` True`, and to start generating the PWM waveform on the specified pin immediately after the object is generated

## Method

### init

Constructor-like

`` `python
PWM.init (tim, freq, duty, pin, enable = True)
`` `

#### parameters

Same as constructor

#### return value

no


### freq

Get or set the PWM frequency

`` `python
PWM.freq (freq)
`` `

#### parameters

* `freq`: PWM frequency, optional parameter, if no parameter is passed, the step setting only returns the current frequency value

#### return value

Actual PWM frequency currently set


### duty

Gets or sets the PWM duty cycle

`` `python
PWM.duty (duty)
`` `

#### parameters

* `duty`: PWM duty cycle is optional. If no parameter is passed, the step setting only returns the current duty cycle value.

#### return value

PWM duty cycle value currently set


### enable

Enable PWM output to generate waveform immediately on specified pin

`` `python
PWM.enable ()
`` `

#### parameters

no

#### return value

no

### disable

Disable PWM output, no longer generate waveforms at specified pins

`` `python
PWM.disable ()
`` `

#### parameters

no

#### return value

no

### deinit / \ __ del \ __

Unregister the PWM hardware, release the occupied resources, and shut down the PWM clock

`` `python
PWM.deinit ()
`` `

#### parameters

no

#### return value

no

#### Examples

`` `python
pwm.deinit ()
`` `
or
`` `python
del pwm
`` `

## Constants

no


## Examples


### Routine 1 (breathing light)

`` `python
from machine import Timer, PWM
import time
from fpioa_manager import board_info

tim = Timer (Timer.TIMER0, Timer.CHANNEL0, mode = Timer.MODE_PWM)
ch = PWM (tim, freq = 500000, duty = 50, pin = board_info.LED_G)
duty = 0
dir = True
while True:
    if dir:
        duty + = 10
    else:
        duty-= 10
    if duty> 100:
        duty = 100
        dir = False
    elif duty <0:
        duty = 0
        dir = True
    time.sleep (0.05)
    ch.duty (duty)
`` `

### Routine 2

`` `python
import time
import machine
from fpioa_manager import board_info

tim = machine.Timer (machine.Timer.TIMER0, machine.Timer.CHANNEL0, mode = machine.Timer.MODE_PWM)
ch0 = machine.PWM (tim, freq = 3000000, duty = 20, pin = board_info.LED_G, enable = False)
ch0.enable ()
time.sleep (3)
ch0.freq (2000000)
print ("freq:", ch0.freq ())
ch0.duty (60)
time.sleep (3)
ch0.disable ()
`` `

