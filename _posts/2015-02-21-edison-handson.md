---
layout: page
title: Edison上手
category: project
---
>寒假之前就向[Arduino中文社区](http://www.arduino.cn)申请了，由于快递之类的原因，拿到手时就很晚了，又受害于拖延症和作业，现在才写:(  
>  
>顺便给奈何推销一下他们的[免费硬件](http://www.elecspark.com/free-hardware/)。

拿到手的时候，Edison已经被他们拼接好了，原来是核心板和扩展板分开的，看样子核心板下面的引脚之类的非常多，现在都不敢把它拆开来看了...  
板子正反面的芯片和电路也非常密集，跳帽也很多，四周的四个脚柱就是防止上电之后短路的。  
然而，当我拿起它的时候，很轻很轻非常轻...比树莓派轻多了。难怪它在物联网和可穿戴方面有很大的前景，也是Intel取悦创客们的一大利器！

目前网上资源教程都还比较少，主要还是官方的[英文资料](http://www.intel.com/support/maker/edison.htm)。

|需要的材料和工具：|
|---------   |
|Edison核心板 |
|Arduino扩展板|
|两根Micro USB连接线|或者一根Micro USB连接线和一个7-15V的DC电源适配器|

下面就介绍一下windows下上手Edison的过程。
![](https://farm8.staticflickr.com/7283/16597444615_aa10ebc358.jpg)

 - 第一步当然要组装好Edison，接到Arduino扩展板上，跳帽什么的就先不要动了。
 - 上电和连接  
Edison供电有两种方式  
  - 第一种是直接连接右上角的接口，官网说7-15V DC，我暂时没有这种电源适配器，就选择了第二种方式。  
  > 但是如果你要使用Wifi，舵机或其他Arduino扩展板，比较推荐这种方式来供电。
  - 第二种是通过USB线由计算机供电，首先需要将板子右边中间的拨动开关拨到朝两个USB口的位置，然后分别用两根USB线把这两个接口连到计算机的USB接口上，其中，靠上的那个接口用于供电和数据传输（包括后面的Arduino程序下载），下面的一个接口，则是用于串口调试。  
  >用于供电的计算机USB口可能会出现电流不足，比如我的笔记本一个USB口就出现了这种情况，这时可以使用额外供电的读卡器来充当计算机的USB口。
 - 上电后，板上的两个LED会亮起来，如果出现亮暗不稳定，就意味着电流不足。  
如果计算机出现提示，有文件驱动器插入，则表明Edison已经正常启动。  
![](https://farm9.staticflickr.com/8616/16565076942_412f555139_z.jpg)
 - 给计算机安装驱动  
下载[FTDI驱动](http://www.ftdichip.com/Drivers/CDM/CDM%20v2.10.00%20WHQL%20Certified.exe)，按照提示，一步步下去安装完成即可。  

 - 用Putty进行串口调试  
打开设备管理器，找到Edison的串口    ![](https://farm8.staticflickr.com/7418/16380266467_0b3be070b0.jpg)  

>注意：务必右键查看串口属性，并使其每秒位数为115200  

![](https://farm8.staticflickr.com/7419/16596254741_ce2f081dd7_z.jpg)

![](https://farm8.staticflickr.com/7407/16378720610_a4f143b23c_o.png)  
在putty的串口连接中填写相应的串口号和速度即可连接。  
>连接后需回车一次才有显示。  

![](https://farm8.staticflickr.com/7423/15977669613_a52ac6c0a6_z.jpg)
这就进入了Edison的命令行！

 - 设置用户名，密码，wifi等可以运行
```
configure_edison --setup
```
按照提示进行下去即可。
也可以运行
```
configure_edison --wifi
```
来设置Wifi，而不设置用户名等，此时SSH服务无法打开，必须要设置密码才行。
```
configure_edison --password
```
输入
```
ifconfig
```
可以查看Edison的ip地址，在浏览器中输入相应的地址，即可查看它的web页面。  
![](https://farm8.staticflickr.com/7351/16411604119_91e83b442c.jpg)
参考：
 - [http://www.intel.com/support/edison/sb/CS-035336.htm](http://www.intel.com/support/edison/sb/CS-035336.htm)
