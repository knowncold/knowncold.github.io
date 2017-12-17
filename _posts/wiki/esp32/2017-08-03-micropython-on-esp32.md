---
title: ESP32配置microPython
layout: page
category: wiki
---
> microPython是该团队针对微处理器（一般指无法运行Linux操作性系统）做出的一个python的实现，官方有一些支持的板子，esp32作为一块性能高于esp8266，同时也具备很不错的wifi、蓝牙功能的开发板，也在microPython的支持之中，当前开发文档参考[microPython on esp8266](http://docs.micropython.org/en/latest/esp8266/index.html)，毕竟是同一类板子。

## 配置ESP-IDF

## 编译固件

根目录

```
$ make -C mpy-cross
$ git submodule init lib/berkeley-db-1.xx
$ git submodule update
```

## 下载固件并烧写

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

## 连接ESP32
在Linux上推荐使用picocom来打开串口连接ESP32，Windows直接使用putty也行，注意要指定115200的波特率

```
$ sudo apt-get install picocom
$ sudo picocom -b 115200 /dev/ttyUSB0
```

## 使用Python

用picocom打开串口之后，按一下板子上的rst，可以看到串口出来的是一个python的REPL（交互解释器），当前一般的开发方法是通过串口的这个解释器逐行写代码进去，但是重启之后就会失效，想要断电保存并上电自启动也行，但操作相对会比较麻烦，不在本文的讨论之内。

![]()

注意这里的python是python3.4加上一点点3.5的特性，支持绝大部分的python核心数据类型和一些核心库。

最简单的你可以测试一下一些简单的内建函数

![]()

这个REPL占用了esp32的UART0（GPIO1=TX，GPIO3=RX），不过它的tab补全非常给力，比pc上的iPython差一点，但比起PC上的python shell是好到哪里去都不知道了。

## 控制引脚

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
