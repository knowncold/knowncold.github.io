---
title: microPython常用函数
layout: page
category: wiki
---
> microPython的函数很多


## machine module

### CPU主频

```python
import machine

machine.freq()      # 获得当前CPU频率
machine.freq(160000000)     # 设置当前CPU频率
```

### 控制引脚

```python
from machine import Pin

p0 = Pin(0, Pin.OUT)    # GPIO0设置为输出模式
p0.value(1)             # p0输出高电平
p0.value(0)             # p0输出低电平
p0.value()              # 当前p0设置的电平

p2 = Pin(2, Pin.IN)     # GPIO2设置为输入模式
p2.value()              # p2的电平
p3 = Pin(3, Pin.IN, Pin.PULL_UP)    # GPIO3设置为上拉的输入模式
p4 = Pin(4, Pin.OUT, value=1)   # 创建Pin对象同时设置初始电平
```

可以设置的GPIO有 0, 1, 2, 3, 4, 5, 12, 13, 14, 15, 16；其中1、3作为REPL的串口使用，16用于从睡眠状态唤醒，使用的时候都需要注意。

### PWM

```python
from machine import Pin, PWM

pwm0 = PWM(Pin(0))      # 通过Pin对象来创建PWM对象
pwm0.freq()             # 获得当前的PWM频率
pwm0.freq(1000)         # 设置PWM频率
pwm0.duty()             # 获得当前的PWM占空比
pwm0.duty(200)          # 设置占空比
pwm0.deinit()           # 关闭PWM

pwm2 = PWM(Pin(2), freq=500, duty=512) # 创建PWM同时设置参数
```

除了GPIO16都可以使用PWM，频率参数的范围是1到1000，占空比参数的范围是0到1023。

### ADC

```python
from machine import ADC

adc = ADC(0)    # 在ADC引脚上创建ADC对象
adc.read()      # 获取ADC值，范围是0-1024
```

### 软SPI
可以用于所有的引脚

```python
from machine import Pin, SPI

spi = SPI(-1, baudrate=100000, polarity=1, phase=0, sck=Pin(0), mosi=Pin(2), miso=Pin(4))   # 创建SPI对象

spi.init(baudrate=200000) # 设置波特率

spi.read(10)            # 读取10字节
spi.read(10, 0xff)      # 读取十字节，并写出0xff

buf = bytearray(50)     # 创建一个缓冲字节流
spi.readinto(buf)       # 读入到这个字节流
spi.readinto(buf, 0xff) # 读入字节流并发送0xff

spi.write(b'12345')     # 发送5个字节

buf = bytearray(4)      
spi.write_readinto(b'1234', buf) # 发送并读取到buf
spi.write_readinto(buf, buf) # 发送buf并读取到buf
```

### 硬件SPI
硬件SPI更快（达到80Mhz），但是只适用于特定的引脚：  
MISO是GPIO12，MOSI是GPIO13，SCK是GPIO14

和软串口一样有同样的方法函数，只是构造函数不同

```python
from machine import Pin, SPI

hspi = SPI(1, baudrate=80000000, polarity=0, phase=0)
```

SPI(0)被用于FlashROM，不对用户开放

### I2C
I2C适用于所有的引脚

```python
i2c = I2C(scl=Pin(5), sda=Pin(4), freq=100000)

i2c.readfrom(0x3a, 4)   # 从0x3a读取4字节
i2c.writeto(0x3a, '12') # 发送12到0x3a

buf = bytearray(10)     # 创建十字节的缓冲字节流
i2c.writeto(0x3a, buf)  # 发送字节流到0x3a
```

### 深度睡眠模式
连接GPIO16和reset引脚

```python
import machine

# 配置RTC.ALARM0来唤醒设备
rtc = machine.RTC()
rtc.irq(trigger=rtc.ALARM0, wake=machine.DEEPSLEEP)

# 检查是否reset是否是由唤醒引起的
if machine.reset_cause() == machine.DEEPSLEEP_RESET:
    print('woke from a deep sleep')

# 设置RTC.ALARM0在10秒后唤醒设备
rtc.alarm(rtc.ALARM0, 10000)

# 设备进入深度睡眠
machine.deepsleep()
```

### 定时器

```python
from machine import Timer

tim = Timer(-1)
tim.init(period=5000, mode=Timer.ONE_SHOT, callback=lambda t:print(1))
tim.init(period=2000, mode=Timer.PERIODIC, callback=lambda t:print(2))
```

其中period的单位是毫秒

## network module

### 连接WIFI

```python
import network

wlan = network.WLAN(network.STA_IF) # 创建WLAN STA接口
wlan.active(True)       # 激活WLAN
wlan.scan()             # 扫描附近的WIFI
wlan.isconnected()      # 查看当前是否已经连上WIFI
wlan.connect('essid', 'password') # 连接到附近的WIFI
wlan.config('mac')      # 获得mac地址
wlan.ifconfig()         # 返回IP/子网掩码/网关/DNS地址
```

可以把下面这个函数放到`boot.py`里面，每次启动一行代码就能连接WIFI了

```python
def do_connect(ssid, password):
    import network
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print('connecting to network...')
        wlan.connect(ssid, password)
        while not wlan.isconnected():
            pass
    print('network config:', wlan.ifconfig())
```

### 创建WIFI

```python
ap = network.WLAN(network.AP_IF) # 创建一个AP
ap.active(True)         # 激活这个AP
ap.config(essid='ESP-AP') # 设置这个AP的SSID
```

连接上WIFI之后就能进一步使用socket库了

## time module

### 延时和计时

```python
import time

time.sleep(1)               # 延时一秒
time.sleep_ms(500)          # 延时500毫秒
time.sleep_us(10)           # 延时10微秒
start = time.ticks_ms()     # 得到内部计时的某个时间点
delta = time.ticks_diff(time.ticks_ms(), start) # 计算过去的时间段的长度
```

## 单总线
单总线协议适用于所有的引脚

```python
from machine import Pin
import onewire

ow = onewire.OneWire(Pin(12)) # 在GPIO12上创建单总线协议
ow.scan()               # 返回总线上的设备列表
ow.reset()              # 重置总线
ow.readbyte()           # 读取一个字节
ow.writebyte(0x12)      # 往总线写0x12
ow.write('123')         # 往总线写'123'
ow.select_rom(b'12345678') # 选择特定设备
```

### ds18x20
用于DS18S20和DS18B20的驱动库

```python
import time, ds18x20
ds = ds18x20.DS18X20(ow)
roms = ds.scan()
ds.convert_temp()
time.sleep_ms(750)
for rom in roms:
    print(ds.read_temp(rom))
```

## DHT驱动
DHT适用于所有的引脚

```python
import dht
import machine

d = dht.DHT11(machine.Pin(4))
d.measure()
d.temperature() # eg. 23 (°C)
d.humidity()    # eg. 41 (% RH)

d = dht.DHT22(machine.Pin(4))
d.measure()
d.temperature() # eg. 23.6 (°C)
d.humidity()    # eg. 41.3 (% RH)
```
