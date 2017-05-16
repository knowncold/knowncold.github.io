---
title: 试戴智能手表
layout: page
category: life
---
很早就拿到了手表，但是种种原因之后，，拖到了现在。拿到快递打开后是精美的外壳包装，实际到手的视觉感和图片的渲染效果还是挺相近的。

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-1.jpg)

表盘有一张贴纸。

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-2.jpg)

需要扫码下载对应平台的app才能愉快的使用手表。

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-3.jpg)

打开app和蓝牙进行配对

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-4.PNG)

app中有应用商店，是其他开发者分享的,可以直接下载到连接着蓝牙的手表中去

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-5.jpg)

安装应用的过程

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-6.PNG)

连接上iPhone之后，iPhone所有的相关通知都会在手表端进行提示，很多时候就不需要时不时的打开手机查看了

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-7.PNG)

到手之后马上有新固件提醒就顺便更新了

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-8.jpg)

我现在使用的app和表盘

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-9.jpg)

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-10.jpg)

![](http://o73wiy9vn.bkt.clouddn.com/maibu-watch-11.jpg)

总体而言麦步智能手表在这个价位上，挺适合开发者的，自己需要什么就能编写，很多传感器都能直接调用，还能和arduino互动，但是也有很多不足，比如外观上感觉这个表带不给力，平时路上戴着感觉有些奇怪，其次本身的振动也有些奇异，不像常见的手机振动的感觉，如果颜值上能再上一层楼的话，相信会好上很多；  
另外有些app安装之后可能是开发者开发的bug，偶尔会出现崩溃重启的现象，不清楚是我的个别情况还是大家都有的。

粗略的尝试了一下手表的开发过程，PC端在arm编译器上利用官方的很多函数库，编译后的输出文件通过qq电脑发送到手机，然后就能通过手机app蓝牙的方式上传到手表上，手表上就相当于添加了一个手表app。
