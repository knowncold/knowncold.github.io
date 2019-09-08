---
title: ESP32上的microPython
date: 2017-08-03
category: wiki
---
microPython是该团队针对微处理器（一般指无法运行Linux操作性系统）做出的一个python的实现，官方有一些支持的板子，esp32作为一块性能高于esp8266，同时也具备很不错的wifi、蓝牙功能的开发板，也在microPython的支持之中，当前开发文档参考[microPython on esp8266](http://docs.micropython.org/en/latest/esp8266/index.html)，毕竟是同一类板子。

<!--more-->

## 烧写和配置ESP32

#### 编译固件

根目录

```
$ make -C mpy-cross
$ git submodule init lib/berkeley-db-1.xx
$ git submodule update
```

#### 下载固件并烧写

[https://micropython.org/download#esp32](https://micropython.org/download#esp32)

另外需要`esptool.py`，通过pip来安装。

```
pip install esptool.py
```

首先最好擦除原有的固件

```
sudo esptool.py --chip esp32 --port /dev/ttyUSB0 erase_flash
```

接着把刚刚下载的固件烧写上去

```
sudo esptool.py --chip esp32 --port /dev/ttyUSB0 write_flash -z 0x1000 ~/Downloads/esp32-20170803-v1.9.1-394-g79feb956.bin
```

#### 连接ESP32
在Linux上推荐使用picocom来打开串口连接ESP32，Windows直接使用putty也行，注意要指定115200的波特率

```
$ sudo apt-get install picocom
$ sudo picocom -b 115200 /dev/ttyUSB0
```

#### 使用Python

用picocom打开串口之后，按一下板子上的rst，可以看到串口出来的是一个python的REPL（交互解释器），当前一般的开发方法是通过串口的这个解释器逐行写代码进去，但是重启之后就会失效，想要断电保存并上电自启动也行，但操作相对会比较麻烦，不在本文的讨论之内。

![]()

注意这里的python是python3.4加上一点点3.5的特性，支持绝大部分的python核心数据类型和一些核心库。

最简单的你可以测试一下一些简单的内建函数

![]()

这个REPL占用了esp32的UART0（GPIO1=TX，GPIO3=RX），不过它的tab补全非常给力，比pc上的iPython差一点，但比起PC上的python shell是好到哪里去都不知道了。

#### 控制引脚

microPython通过一个叫`machine`的module来控制引脚

```python
from machine import Pin

p0 = Pin(0, Pin.OUT)    # 设置GPIO0的output模式
p0.value(1)             # 设置IO0为高电平
p0.value(0)             # 设置IO0为低电平
```

比如下面的例子就能用一秒的间隔来闪烁LED

```python
import time
from machine import Pin

p0 = Pin(0, Pin.OUT)
state = 0
for i in range(0, 15):
    p0.value(state)
    state = 1 - state
    time.sleep(1)
```

![]()

另外对于这种比较长的代码，可以事先在PC上写完，然后在REPL中通过`CRTL+E`进入粘贴模式，复制粘贴完之后`CTRL+D`就行了。

microPython还有WiFi、PWM、SPI、I2C等一系列功能，以后的文章中会一个一个讲过来。

## microPython常用函数

#### CPU主频

```python
import machine

machine.freq()      # 获得当前CPU频率
machine.freq(160000000)     # 设置当前CPU频率
```

#### 控制引脚

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

#### PWM

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

#### ADC

```python
from machine import ADC

adc = ADC(0)    # 在ADC引脚上创建ADC对象
adc.read()      # 获取ADC值，范围是0-1024
```

#### 软SPI
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

#### 硬件SPI
硬件SPI更快（达到80Mhz），但是只适用于特定的引脚：  
MISO是GPIO12，MOSI是GPIO13，SCK是GPIO14

和软串口一样有同样的方法函数，只是构造函数不同

```python
from machine import Pin, SPI

hspi = SPI(1, baudrate=80000000, polarity=0, phase=0)
```

SPI(0)被用于FlashROM，不对用户开放

#### I2C
I2C适用于所有的引脚

```python
i2c = I2C(scl=Pin(5), sda=Pin(4), freq=100000)

i2c.readfrom(0x3a, 4)   # 从0x3a读取4字节
i2c.writeto(0x3a, '12') # 发送12到0x3a

buf = bytearray(10)     # 创建十字节的缓冲字节流
i2c.writeto(0x3a, buf)  # 发送字节流到0x3a
```

#### 深度睡眠模式
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

#### 定时器

```python
from machine import Timer

tim = Timer(-1)
tim.init(period=5000, mode=Timer.ONE_SHOT, callback=lambda t:print(1))
tim.init(period=2000, mode=Timer.PERIODIC, callback=lambda t:print(2))
```

其中period的单位是毫秒

#### 连接WIFI

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

#### 创建WIFI

```python
ap = network.WLAN(network.AP_IF) # 创建一个AP
ap.active(True)         # 激活这个AP
ap.config(essid='ESP-AP') # 设置这个AP的SSID
```

连接上WIFI之后就能进一步使用socket库了

#### 延时和计时

```python
import time

time.sleep(1)               # 延时一秒
time.sleep_ms(500)          # 延时500毫秒
time.sleep_us(10)           # 延时10微秒
start = time.ticks_ms()     # 得到内部计时的某个时间点
delta = time.ticks_diff(time.ticks_ms(), start) # 计算过去的时间段的长度
```

#### 单总线
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

#### ds18x20
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

#### DHT驱动
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

## 高级应用

#### 内部文件系统
microPython支持标准的Python的文件模块，可以使用`open()`这类原生函数。

> 需要注意的是esp32上实时资源少，需要及时关闭掉一些file、socket。

#### 创建一个文件

```python
>>> f = open('data.txt', 'w')
>>> f.write('some data')
9
>>> f.close()
```

其中这个9是指write()函数写进去的字节数

#### 查看一个文件

```python
>>> f = open('data.txt')
>>> f.read()
'some data'
>>> f.close()
```

#### 文件目录操作

```python
>>> import os   # 引用os模块
>>> os.listdir()    # 查看当前目录下的所有文件
['boot.py', 'port_config.py', 'data.txt']
>>> os.mkdir('dir') # 创建目录
>>> os.remove('data.txt')   # 删除文件
```

## esp启动顺序

首先运行_boot.py这个脚本，把文件系统挂载上，这个部分一般是固定的，不推荐用户来修改，可能会出很多奇怪的问题。

当文件系统挂载成功后，运行boot.py，在这个脚本里面，用户可以设置一些在REPL里面需要使用的变量或者函数，每次重启esp32，这个脚本也会运行一次，但是如果这个地方写错了代码， 比如进入了死循环之类的，你就需要重新刷固件了。

最后系统会从文件系统运行main.py（如果不存在，就不会运行），这个文件就是用来每次启动的时候运行用户程序而不是进入REPL的，对于一些小的脚本，你可以直接写成一个main.py名字的文件，不过也会推荐你把一个大应用分散来写，写成多个小程序，在main.py里面这么写就好了：

```python
import my_app
my_app.main()
```

#### 设置开机自启动的脚本

对boot.py和main.py这两个文件进行修改都可以，比如对main.py进行修改：

```python
>>> file = open("main.py", 'w')
>>> file.write("""import time
... for i in range(0,10):
...     time.sleep(1)
...     print(i)""")
64
>>> file.close()
```

通过快捷键`ctrl+D`，软启动esp32，就能看到上面的效果了

```python
>>>
PYB: soft reboot
0
1
2
3
4
5
6
7
8
9
MicroPython v1.9.1-394-g79feb956 on 2017-08-03; ESP32 module with ESP32
Type "help()" for more information.
>>>
```

## 网络socket应用
简单的连接WiFi和设置热点可以看上一篇教程，成功之后就可以考虑TCP socket连接了。

在这里我们可以用socket模块，但其实有更加方便的模块，urequests（u表示这个模块和标准python的模块相比有许多没有方法没有实现）：

```python
import urequests

r = urequests.get('http://www.baidu.com')   # 发起HTTP的GET请求
r.text  # 查看服务器返回的内容
```

urequests实现了主要的几个方法，比如get、post、put、delete这几种请求，在网络方面使用起来非常方便。

