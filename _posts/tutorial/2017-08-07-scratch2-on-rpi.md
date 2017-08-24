---
title: Scratch2.0——针对树莓派的新功能
layout: page
category: tutorial
---

> 在最新的树莓派Raspbian系统上，Scratch2.0是自带的编程软件，这个版本除了可以让我们通过Scratch来操作GPIO的输入输出，还有一些新特性。

## 控制GPIO

和以前的版本一样，现在通过拖动模块，我们就能控制GPIO的输出，获取GPIO的输入，这就意味着我们通过Scratch就能点亮LED、控制蜂鸣器、获得按钮等各种传感器的输入信号来进一步控制scratch小猫的动作；而且比以前更加方便，用下面的两个模块就能完成输入和输出的操作。

![1](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-1.png)

添加这两个模块的方法很简单，打开Scratch2.0之后，我们单击More Blocks和Add an Extension，然后需要选择一个Pi GPIO的扩展，再选择OK。

![2](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-2.PNG)

之后在More Block板块下面就能看到相关的两个模块

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-3.PNG)

在GPIO2连接一个LED，用下面的例子就能让这个LED一秒一秒的间隔闪烁。

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-4.png)

或者在GPIO2连接一个按钮，简单地把这个引脚口设置成input，然后使用”gpio _ is high?” 模块就能来检测这个按钮的状态，在下面的例子中，当按钮被按下的时候Scratch小猫就会说”Pressed”。

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-5.png)

## 复制SPRITE

相比较1.4的版本，Scratch2.0也提供了一些额外的新功能，最主要的一个新特性是我们可以创建sprite的拷贝，也就是从一个sprite复制成多个，每个拷贝都是某个sprite的实例，都会从最开始的sprite继承下它的脚本模块。

比如下面的例子中，每次按下空格键，scratch猫都会扔出一个复制出来的苹果实例，每个苹果被复制出来的时候，都会执行“When I start as a clone”的这部分脚本。

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-6.png)

这种复制的新功能，避免了我们以前重复创建一模一样的sprite（比如做游戏的时候需要创建一堆敌人）的情况。这种复制的新功能，避免了我们以前重复创建一模一样的sprite（比如做游戏的时候需要创建一堆敌人）的情况。

## 自定义的模块

Scratch2.0支持像定义函数一样，自定义模块，方便用户在一个项目里面多次调用和分块编写应用，下面的例子展示了一个jump的自定义模块，用一个jump模块就能对所有的sprite应用这个效果。

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-7.png)

而且自定义模块的时候，可以加入输入值（参数），比如下面的这个画图形的函数模块，有两个参数，一个是图形的边数，一个是边长，可以看到同一个自定义模块，使用的时候输入不同的参数值，就能得到不一样的我们需要的效果。

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-8.png)

![](http://o73wiy9vn.bkt.clouddn.com/scratch2-on-rpi-9.png)

Scratch2.0现在也支持与网络摄像头、麦克风的简单交互，都有了相应的模块，比如这个[Clap-O-Meter](https://scratch.mit.edu/projects/37778440/)项目通过麦克风可以来检测噪声的强度，[Keepie Uppies](https://scratch.mit.edu/projects/36966344/)这个游戏通过摄像头来控制足球。

另外还有几个新功能包括矢量图编辑器和一个声音编辑器以及很多新的人物图案。
