---
title: Ubuntu录制视频并做成gif
layout: page
category: wiki
---

## 安装需要的软件

```
sudo apt-get install kazam
sudo apt-get install  mplayer
sudo apt-get install imagemagick
```

## 录制

通过Win键呼出Dash页面选择kazam，用GUI的方式开始录制，可以选定区域录制mp4

## 视频转换成图片

```
mkdir jpgdir
mplayer -ao null tabs.mp4 -vo jpeg:outdir=./jpgdir
```

## 合成gif

```
convert ./jpgdir/*.jpg view.gif
```

## 缩小gif大小

```
convert out.gif -fuzz 10% -layers Optimize optimized.gif
```
