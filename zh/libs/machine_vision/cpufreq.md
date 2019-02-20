cpufreq
========

cpu频率模块，支持程序修改 cpu 和 kpu 频率

## 方法

### 获取当前频率

获取当前 cpu 和 kpu的频率，该方法有2个返回值

```python
import cpufreq
cpu_freq,kpu_freq = cpufreq.get_current_frequencies()

```

#### 参数

无

#### 返回值

* `cpu_freq`：当前cpu的频率，单位为M
* `kpu_freq`：当前kpu的频率，单位为M

### 获取kpu支持的频率

因为 kpu 是通过 pll1 分频而得到的速率且目前 pll1 频率固定为400M，所以目前不能支持任意频率。cpu 理论上支持任意频率的，但会根据实际硬件的情况有所偏差。

```python
import cpufreq
kpu_freqlist = cpufreq.get_kpu_supported_frequencies()

```

### 参数

无

### 返回值

* kpu_freqlist：kpu支持频率列表

### 设置频率

设置 cpu 或者 kpu 频率，请注意在频率设置完毕后可能会导致某些外设性能改变

```python
import cpufreq
kpu_freqlist = cpufreq.set_frequency(cpu = cpu_freq, kpu = kpu_freq)

```

#### 参数

参数可以输入 cpu 或 kpu 中的1个或2者都输入

* `cpu_freq`：想要设置的cpu频率，范围[26,400]

* `kpu_freq`：想要设置的kpu频率，范围请查看 `kpu支持频率列表`

#### 返回值

使用该接口后，如果频率没有变化，则返回空。如果频率有变化，将会重启机器。在使用该接口之前请确认当前情况能能否重启