machine.Timer
===========

Hardware timers deal with timing of periods and events. Timers are perhaps the most flexible and heterogeneous kind of hardware in MCUs and SoCs, differently greatly from a model to a model. MicroPython’s Timer class defines a baseline operation of executing a callback with a given period (or once after some delay), and allow specific boards to define more non-standard behavior (which thus won’t be portable to other boards).

## Constructors

```python
class machine.Timer(id, channel, mode=Timer.MODE_ONE_SHOT, period=1000, unit=Timer.UNIT_MS, callback=None, arg=None, start=True, priority=1, div=0)
```

Construct a new timer object of the given id. 

### Parameters

* `id`: Timer ID, [0~2] \(Timer.TIMER0~TIMER2\)
* `channel`: Timer channel, [Timer.CHANNEL0~Timer.CHANNEL3]
* `mode`: Timer mode, `MODE_ONE_SHOT` or `MODE_PERIODIC`or `MODE_PWM`
* `period`: Timer period, after `period` the callback will be invoke, (0,~).
* `unit`: unit of timer, default `ms`, `Timer.UNIT_S` or `Timer.UNIT_MS` or `Timer.UNIT_US` or`Timer.UNIT_NS`
* `callback`: Timer callback, two parameters, first is `Timer`, second is user param, see `param` parameter below. 
> callback execute in interrupt, so don't stay too long in callback
* `arg`: Argument dilivered to callback
* `start`: If start instantly timer after init, `True`:start, `False`:not start, need call `start()` function.
* `priority`: interrupt priority, [1,7].
* `div`: Timer clock divider,[0,255],default to 0. clk_timer = clk_pll0/2^(div+1)
> clk_timer*period(unit:s) should < 2^32 and >=1


## Methods

### init

Same to constructor

```python
Timer.init(id, channel, mode=Timer.MODE_ONE_SHOT, period=1000, unit=Timer.UNIT_MS, callback=None, arg=None, start=True, priority=1, div=0)
```

### callback_arg

callback arg of timer obj

### callback

Get or set callback

e.g.
```python
def on_timer(timer,param):
    print("time up:",timer)
    print("param:",timer.callback_arg())

tim.callback(on_timer)
print(on_timer, tim.callback())
```

### period

Get or set period

e.g.
```python
tim.period(2000)
print( tim.period() )
```

### start

Start timer

e.g.
```python
tim.start()
```

### stop

Stop timer

### restart

Restart timer

### deinit/__del__

Deinitialises the timer. Stops the timer, and disables the timer peripheral.

e.g.
```python
tim.deinit()
```
or
```python
del tim
```

## Constants

* `TIMER0`: Timer0 id
* `TIMER1`: Timer1 id
* `TIMER2`: Timer2 id
* `CHANNEL0`: Timer channel 0
* `CHANNEL1`: Timer channel 1
* `CHANNEL2`: Timer channel 2
* `CHANNEL3`: Timer channel 3
* `MODE_ONE_SHOT`: Timer only run once
* `MODE_PERIODIC`: Timer always run
* `MODE_PWM`: Timer used by PWM
* `UNIT_S`: unit flag (s)
* `UNIT_MS`: unit flag (ms)
* `UNIT_US`: unit flag (us)
* `UNIT_NS`: unit flag (ns)


## Demo

### Demo 1

Print data after 3s just once

```python
from machine import Timer

def on_timer(timer):
    print("time up:",timer)
    print("param:",timer.callback_arg())

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_ONE_SHOT, period=3000, callback=on_timer, arg=on_timer)
print("period:",tim.period())
```

### Demo 2

Print every 1s, and stop 5s then restart 5s then shutdown

```python
import time
from machine import Timer

def on_timer(timer,param):
    print("time up:",timer)
    print("param:",param)

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PERIODIC, period=1, unit=Timer.UNIT_S, callback=on_timer, arg=on_timer, start=False, priority=1, div=0)
print("period:",tim.period())
tim.start()
time.sleep(5)
tim.stop()
time.sleep(5)
tim.restart()
time.sleep(5)
tim.stop()
del tim
```



