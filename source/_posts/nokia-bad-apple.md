---
title: Nokia5110上的Bad Apple
date: 2016-02-15 00:00:00
last_update: 2019-03-03 00:00:00
tags:
---


「有屏幕的地方就有BadApple」
<!--more-->

熟悉二次元的朋友对于Badapple一定不会陌生，基于它本身的黑白手绘风格，被大量技术宅广泛演绎在各种屏幕上。

本篇教程的重点并不在于代码，而在于解决这个问题的思路，假如只是纠结于这个项目本身的话，就不能理解到通用的其它屏幕显示的方法了。

我们的最终目标是在Nokia5110屏幕上用树莓派驱动来完整显示这个视频。

<div style="text-align: center;">
<iframe src="//player.bilibili.com/player.html?aid=706&cid=3724723&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width='680px' height='600px'> </iframe></div>

先来看一下效果

<iframe src="//player.bilibili.com/player.html?aid=67065773&cid=116294569&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width='680px' height='600px'> </iframe>


首先我们知道，对于Nokia5110而言，直接在树莓派写一个驱动，使它能够像外接hdmi显示屏一样流畅显示这样的难度显然比较大，反正我没什么思路。

那么退而求其次，我们也能想到，视频是一帧一帧的播放的，假如我们能够用树莓派来显示一张图片，那么当树莓派更换图片显示的速度够快时，就能达到视频的效果

## 初步整理一下步骤

- 获得BadApple原视频，必须要最超清的
- 把视频的每一帧分割出来，做成一张一张图片
- 由于视频的分辨率和nokia5110的分辨率（84x84）不相同，所以需要批量转换
- 在5110上能够显示一张图片
- 获得原视频中的音乐，在播放图片的同时播放音乐
- 编写代码，使得显示图片的速率和原程序的帧率相近，肉眼看起来才能有相似的效果

## 获得原视频
通过网络搜索，很容易就能找到相关的视频，不过想要找到超清的，还是需要花一番功夫

想要下载看的话，可以直接看这个百度云的文件
链接：http://pan.baidu.com/s/1ntR2MDj 
密码：ybsw

## 分割视频

方法有很多种，我想找个最简单的，发现linux下有命令行工具就直接用了，不过在Ubuntu下没找到。。由于项目要用到树莓派，索性就在树莓派上使用ffmpeg这个命令行工具来从上面的视频中来获得我们需要的每一张帧。  
第一步是要把上面的视频上传到树莓派上，可以用SFTP的方式或者简单的U盘复制过去，命令行下运行：

```bash
$ ffmpeg -i badapple.mkv example.%d.jpg
```

具体的ffmpeg工具使用方法可以参考这个[http://blog.chinaunix.net/uid-72186-id-2657751.html](http://blog.chinaunix.net/uid-72186-id-2657751.html)
然后就会有六千多张图片生成，也就是视频播放时的那些帧了，仔细一算六千多除以30帧每秒的帧率，恰巧就是视频的时间长度。


![](https://i.postimg.cc/nzgPg948/nokia-bad-apple-1.png)

## 转换图片大小
然而这些图片不能直接使用，我们选择的是超清级别的视频，查看图片的属性就会知道分辨率是1440x1080，但是我们打算使用的Nokia屏幕确是84x84的
假如你要使用其他的屏幕，也很难一下子找到恰好符合屏幕的视频，还好我们有一些图片处理软件，比如PS、美图秀秀，找其中一种就可以批量转换分辨率了，图片稍微会有一些变形，毕竟不是等比例的缩放。

## 显示图片

![](https://i.postimg.cc/hjfY217b/nokia-bad-apple-2.jpg)

用树莓派在nokia屏幕上显示图片，可以参考这个adafruit的教程和他们的python库，使用的是SPI方法。
https://learn.adafruit.com/nokia-5110-3310-lcd-python-library
这个教程比较详细，这里就不重复了。

## 音乐
音乐也要考虑几个方面，比如时间长度是否和我们需要的一样，毕竟网上版本很多，有长有短很正常；音乐的音效是否足够好。。。
用最简单的方法就是从上面的那个视频里直接提取出音乐了
提取的方法有不少，百度一下一般就能解决，这里也不详述了。
用python语言播放音乐有很多种方法，这里我用了以前用过的一种，通过pygame库来播放。
使用时只需要这样就能播放badapple.flac的音乐了

```python
import pygame
pygame.init()
pygame.mixer.init()
pygame.mixer.music.load("badapple.flac")
pygame.mixer.music.play()
```

## 像视频一样播放图片
这是这份教程的难度所在啊。。建议你先不看下面，自己想一想，反正我试了好几次才想到这个办法

一开始我想偷懒，同时以为树莓派性能不会太强，就索性让音乐开始之后，直接开始显示图片，用一个while循环不停歇地显示下一张图片，一次性显示完6571张。

结果发现树莓派还是比较给力的。。在音乐结束前就播放完图片了。。

于是我就想，既然速度这么快，那么我在图片显示的间隔里加几毫秒是不是就行了，于是在while循环里每一次显示完图片就sleep几毫秒。

结果发现很难正好凑到音乐结束

仔细考虑一下，显示图片的速度本身是不可控的，和树莓派当时的环境温度啊自己的CPU使用情况啊都会有关。。每次凑当然结果会不一样

终于我想到了，只有时间是几乎完全确定的，假如我们从音乐开始播放就计时

```python
t = time.time()
```

那么，while循环显示图片的时候就可以通过帧率30和时间来算出下一次要显示哪一张图片！

```python
count = (int((time.time()-t)*30))+1
```

也就是说假如这次显示图片的过程长了，那么我们就跳过下一帧，假如这次显示图片超级快，那么我们就再显示一次这张图，当作拖一下时间，那么无论怎么变化，最后肯定会和音乐一起结束播放！

代码如下，注释没加，如果需要可以评论出来：

```python
import time

import Adafruit_Nokia_LCD as LCD
import Adafruit_GPIO.SPI as SPI

from PIL import Image
import pygame

# Raspberry Pi hardware SPI config:
DC = 20
RST = 21
SPI_PORT = 0
SPI_DEVICE = 0

# Hardware SPI usage:
disp = LCD.PCD8544(DC, RST, spi=SPI.SpiDev(SPI_PORT, SPI_DEVICE, max_speed_hz=4000000))

# Initialize library.
disp.begin(contrast=60)
t = time.time()
count = 1
disp.clear()
disp.display()
pygame.init()
pygame.mixer.init()
pygame.mixer.music.load("badapple.flac")
pygame.mixer.music.play()
while count<6571:

        # Clear display.
        tt = time.time()

        picnum = 'example.'+ str(count)+'.jpg'

        # Load image and convert to 1 bit color.
        image = Image.open(picnum).convert('1')
        # Display image.
        disp.image(image)
        disp.display()
        count = (int((time.time()-t)*30))+1
        print (time.time()-tt)
print 'Press Ctrl-C to quit.'
t = t-time.time()
print t
while True:
        time.sleep(1.0)
```

然后参照上文提到的adafruit的教程连好线，插上音响，运行python代码就可以成功地播放了

用类似的方法就可以在其他的平台和显示屏上愉快地享受BadApple了。
