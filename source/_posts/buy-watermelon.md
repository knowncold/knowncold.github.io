---
title: 有一个人前来买瓜
date: 2021-08-28 20:58:00
tags:
---

复用之前的[思路和代码](/2016/02/15/nokia-bad-apple/)，一边掉分一边把买瓜整完了

<!--more-->

![buy-watermelon](https://i.postimg.cc/vH2n9HZr/IMG-3261.jpg)


<div style="text-align: center;">
<iframe src="//player.bilibili.com/player.html?aid=762633880&bvid=BV1664y1a7tc&cid=398641980&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width='680px' height='600px'> </iframe>
</div>

> 本来想继续用Nokia5110的显示屏，但是84x84的分辨率实在不能很好地匹配这个视频的比例，只能使用128x64 0.96的OLED了
> 本文只做前文的补充，描述关键步骤，就不复述其中的思考和调研了

## 处理原视频

找到相对高清的视频，使用`ffmpeg`处理得到每一帧图片

```bash
ffmpeg -i huaqiang.mp4 huaqiang.%d.jpg
```

我找到的这个视频是24帧的，`2:05`长度的视频得到2993张图片

## 处理图片

和上次`Bad Apple`不同之处在于这个视频是彩色的，不仅要处理分辨率还要处理灰度+二值化的问题，这次不再使用特殊的图片处理软件，利用[ImageMagick](https://imagemagick.org/index.php)就能达成效果

```bash
convert huaqiang.1298.jpg -monochrome -resize 128x64! c.jpg
# 使用!表示不按照比例修改分辨率
```

对于所有的图片，写一个shell脚本进行转换放入文件夹：

```bash
#!/bin/bash

for img in `find . -iname '*.jpg' -type f -maxdepth 1`
do
	new=converted/${img//.\//}
	echo "$new"
	convert "$img" -monochrome -resize 128x64! "$new"
done
```

## 渲染图片

这次使用的是`SSD1306`驱动的128x64显示屏，参考了[adafruit的这个文章](https://learn.adafruit.com/monochrome-oled-breakouts/overview)，使用SPI速度足够了

## 播放音乐

同样使用`ffmpeg`从视频中直接获取音乐，在树莓派上会使用`pygame`来播放这个音乐

```bash
ffmpeg -i huaqiang.mp4 -f wav -ar 16000 huaqiang.wav
```

## 在OLED上播放视频

在树莓派上通过快速渲染图片来做到像视频一样流畅，其中不控制图片渲染的速度和时间，反过来利用时间来选择当次渲染的图片编号：

```python
import adafruit_ssd1306
import board
import busio
import digitalio
from PIL import Image
import time
import pygame

spi = busio.SPI(board.SCK, MOSI=board.MOSI)
reset_pin = digitalio.DigitalInOut(board.D4)
cs_pin = digitalio.DigitalInOut(board.D5)
dc_pin = digitalio.DigitalInOut(board.D6)

oled = adafruit_ssd1306.SSD1306_SPI(128, 64, spi, dc_pin, reset_pin, cs_pin)

oled.fill(0)
oled.show()

pygame.init()
pygame.mixer.init()
pygame.mixer.music.load("huaqiang.wav")

t = time.time()
count = 1
pygame.mixer.music.play()

while count < 2993:
	num = 'converted/huaqiang.'+str(count)+'.jpg'
	image = Image.open(num).convert('1')
	oled.image(image)
	oled.show()
	count = (int((time.time()-t)*24))+1 # 视频帧率为24
```