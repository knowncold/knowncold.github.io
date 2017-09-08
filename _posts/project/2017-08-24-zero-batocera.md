---
title: 树莓派Zero掌机
layout: page
category: project
---

使用batocera配置树莓派Zero W掌机，batocera系统与其他游戏机系统相比性能要求低，非常适合用于Zero这种低配置的板子上使用。

## 下载镜像

在[这个网站](https://batocera-linux.xorhub.com/#download)，选择针对树莓派Zero W的镜像下载，Windows用Win32DIskImager直接烧写，Linux用dd命令即可。

## 初次配置

通过HDMI线连接电视机或者显示器，给树莓派正常供电，系统会自动分配空间，自动配置完，然后强制关机直接切断电源就行。

## 配置分辨率

考虑到我们需要用小屏幕来作为掌机的显示，系统不会默认就直接适配屏幕，以我的这个[5寸屏幕](http://wiki.52pi.com/index.php/5-Inch-800x480-HDMI-TFT-LCD-Touch-Screen_SKU:Z-0053)为例，需要修改一些配置文件，这个步骤需要Linux系统或者虚拟机。

首先需要修改`/RECALBOX`目录下的config.txt

将下面这段加到里面去（可能需要sudo命令行）。

```
framebuffer_width=800
framebuffer_height=480
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt  800  480  60  6  0  0  0
device_tree=bcm2710-rpi-3-b.dtb
dtparam=spi=on
```

把`/SHARE/system/recalbox.conf`中的两处`CEA 4 HDMI`改成`DMT 87 HDMI`

使用其他屏幕的时候用类似的操作就行了，配置成功之后应该可以使batocera的主屏幕和游戏界面都完全适配使用的硬件屏幕。

## 设置GPIO控制

修改`/SHARE/system/recalbox.conf`中的两行配置

```
controllers.gpio.enabled=1  
controllers.gpio.args=map=1,2 => controllers.gpio.args=map=1
```

重启之后就可以尝试用下面这张图的相关引脚了，默认是上拉的引脚，所以只要把引脚比如27号连一下GND，看他会不会有左滑的效果，如果有效果的话，就是说软件上配置成功了，硬件上还需要做一些事情，需要焊一块小型的掌机来操作才行。

总体思路就是在一块洞洞板上焊接需要数量的微动开关，然后

## 使用游戏机

正式玩游戏之前需要一些其他额外的配置，
