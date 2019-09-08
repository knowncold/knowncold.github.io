---
title: 树莓派打造怀旧游戏机
date: 2016-09-01 10:28:06
category: project
---

之前就关注过很多怀旧游戏机的项目，但是苦于很多项目的复杂难度和硬件要求，一直没有动手，前几天逛到一个网站，发现这个项目，支持不少硬件平台，而且搭建也很简单。

<!-- more -->

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_2.png)

#### 下载镜像和烧写

进入官网，就可以`Get Lakka`，选择树莓派2或3，这两个其实是同一个系统镜像（同时可以看到这个项目真的支持很多硬件平台）。

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_2.png)

下载完解压是一个普通的树莓派镜像文件![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_3.png)

如果是Windows系统，可以用Win32DiskImager来烧写sd卡，然后和往常一样，sd卡插入树莓派，hdmi连接到显示器，然后有音响的话，还能插上音响，插好手柄（大多数，最后通电，就能成功地打开Lakka系统了。

#### 初次启动

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_4.png)

第一次启动会出现上面的界面，然后会有大概30秒的时间来覆盖文件系统到整个sd卡并重启，重启成功就会有下面的画面。

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_5.png)

#### 开始游戏

Lakka支持很多很多模拟器，包括NES（红白机），GBA，雅塔礼等等，事先把游戏rom放到文件系统的roms文件中（Windows系统可能需要Linux虚拟机）。

- 启动系统后，选择Load Core，确定游戏相应的模拟器型号；
- 选择Load Content，浏览文件系统，确定想玩的游戏，就会开始相应的游戏；
- 一时间就会想起小时候玩小霸王的感觉了

##### 坦克大战  
![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_6.jpg)

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_7.jpg)

##### 超级玛丽奥  
![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_8.jpg)

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_9.jpg)

##### 一个不知道叫什么的很老的游戏  
![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_10.jpg)

##### 雪人兄弟  
![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_11.jpg)

![](http://o73wiy9vn.bkt.clouddn.com/lakka-on_12.jpg)

> 成功在实验室搭起一个游戏机之后，为了有点怀旧的感觉  
> 刻意用了废弃的显示器  
> 【不然就用4K电视机了
