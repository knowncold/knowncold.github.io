---
title: Arduino M0 Pro 上手
layout: page
category: project
---
首先为了连接Arduino M0 Pro，需要一根Micro USB线，同时提供供电和编程功能。  
连接USB线到板上的Programming端口，这个端口靠近DC电源接口，为了烧写代码，在IDE中从 工具>主板 的菜单选择Arduino M0 Pro(Programming port)，在串口选择里面选择正确的串口。

>必须要使用Arduino.org家的Arduino IDE1.7.5或之后的IDE  
>也可以尝试Arduino Studio。

###和其他基于Atmega主板的区别  
Arduino M0 Pro有和UNO的一样的接口布局，总体而言，你可以像使用其他的主板一样使用M0 Pro，显然这里会有很多重要的区别和一些函数的扩展。

##电压  
M0 Pro上的单片机的工作电压是3.3V，这也就意味着你不能够在它的IO口上使用超过3.3V的电压，当你连接传感器和驱动器的时候也一定要注意电压限制，如果使用一般主板支持的5V电压，当然会造成不可逆的伤害。  
板子也可以通过DC电源接口来获得电源（6-20V）。  

M0 Pro有一个支持USBhost协议的高效电压转换器，使用Native port作为USB host的时，主板要给这些USB设备供电，比如鼠标或键盘。

##M0 Pro上的串口  
Arduino M0 Pro有两个USB端口，Native port(通过SerialUSB对象来支持CDC虚拟通信)直接连在SAMD21单片机上，另一个Programming port则是连接在ATMEL embedded debugger (EDBG)，作为板上的编程器和调试器同时扮演着USB转串口的角色，Programming port是默认的用来烧写代码和通信的端口。
Programming port 连接在SAMD21的第一个UART上，在写代码的时候它就是"Serial"对象。

而Native port直接连在SAMD21的USBhost引脚上，使用Native port可以让M0 Pro直接作为USB外围设备连接到电脑上，也可以连接其他的外围USB设备包括鼠标、键盘、Android手机，在写代码的时候就可以通过"SerialUSB"对象来操作。

##Native port  
以1200波特率打开或关闭这个串口的时：flash的内存会被擦除，然后重新启动bootloader。这个过程是被单片机控制着的，所以如果单片机被任何原因打断，很可能这个擦除过程会失败。

以其他的波特率打开或关闭这个串口的时候则不会重启SAMD21，为了使用串口监视器和查看你的程序是从哪里开始的，你最好在setup()函数里面加几行代码，这会确保SerialUSB串口正确打开之后才开始执行程序：

    while (!SerialUSB);
按下板上的Reset按键，也会导致SAMD21和USB通信重启，这个中断意味着如果串口监视器开着，那就必须要关闭并重新打开监视器来重启通信。

##Programming port
这个串口连接在Atmel EDBG上，通过它你可以完全掌控SAMD21，比如你可以使用它来烧写bootloader或者得到完整的flash内容，使用它来烧写代码是最安全的方式。

写代码的时候，在IDE里使用"Serial"对象，所以此前在UNO上能用的关于串口的程序通过这个串口也能在M0 Pro上运行。  
与UNO不同的是，在M0 Pro上打开串口监视器（或者其他的串口通信）都不会造成单片机重启，按下Reset键，也不会造成正在连接的USB关闭，因为只有SAMD21被重启了。

##ADC和PWM
M0 Pro可以改变他的模拟读写的位数（默认读为10位，写是8位），它最高可以支持12位的ADC和PWM。

##烧写代码
尽管两个串口都可以用来烧写代码，但是推荐使用Programming port。
其他的操作和之前的主板一样。

>参考:[Getting started with the Arduino M0 Pro](http://labs.arduino.org/Getting+Started+with+Arduino+M0+Pro)
