---
layout: page
title: Windows下通过USB线Edison刷Yocto指南
category: project
---
>看到论坛已经有很多上手开机教程了，但没有人写怎么刷机，而且学会刷机之后就不怕玩坏板子了:）  
>测试系统：Windows8.1

###下载镜像

既然要刷机，首先就要[下载Yocto系统镜像](http://www.intel.com/support/edison/sb/CS-035180.htm)
在网页上找到它。  
![Yocto](https://farm8.staticflickr.com/7323/15946001653_f111e4836e_o.png)


解压后可能是这样的

![Yocto](https://farm9.staticflickr.com/8598/16566205225_5980201d67_b.jpg)

###连接Edison  
![Yocto](https://farm9.staticflickr.com/8654/15943653494_3f21597932_z.jpg)
中间的USB接口使用来供电和数据交换的，角落的USB接口则是用来串口调试的，由于我手头没有合适的DC插头，所以通过USB供电。
>注意：
>USB连接线质量必须过关  
>将中间的拨动开关拨到指向USB接口的一侧

连接正常的话，Edison的绿灯会亮起来,计算机会识别出一个储存设备，记住此时的盘符（比如我这幅图的F）。  
![Yocto](https://farm9.staticflickr.com/8616/16565076942_412f555139_b.jpg)

###安装Edison必要的两个驱动
在[这里](http://www.arduino.cn/thread-7894-1-1.html)已经有介绍了，就先不重复了。
###移除板上的系统
打开Windows的命令行界面，`Win+R`，并输入`CMD`。  
![Yocto](https://farm9.staticflickr.com/8622/15946001063_f173a31538_b.jpg)

输入刚才的盘符对应的字母`f:`，进入该目录，输入`dir`确保该目录是Edison。  
![Yocto](https://farm9.staticflickr.com/8630/16566204175_b9c3901864_b.jpg)

输入`del *`,删除所有文件。可以再输入`dir`进行查看。

![Yocto](https://farm8.staticflickr.com/7457/16378519178_d383e149e9_o.png)
###复制并粘贴最新的镜像
将之前的镜像文件解压后，全部复制到刚清空的盘里，并覆盖重复文件。  
![Yocto](https://farm8.staticflickr.com/7452/16565075892_0e36b93b93_o.png)
###重启Edison，并更新。
打开设备管理器，找到Edison的串口  
![Yocto](https://farm8.staticflickr.com/7418/16380266467_0b3be070b0.jpg)
记住这个`COM5`
>为了保险，可以右键打开属性
>查看端口设置
>将每秒位数设置为115200

打开PuTTY，输入相应的`COM`和`SPEED`  
![Yocto](https://farm8.staticflickr.com/7407/16378720610_a4f143b23c_o.png)

单击`Open`，进入串口调试。
按一次回车键并登录，输入`reboot ota`

![Yocto](https://farm8.staticflickr.com/7376/16566195995_8d1f17331e_b.jpg)
Edison就开始刷入新的系统了，你可以在PuTTY上看到全过程:）

![Yocto](https://farm8.staticflickr.com/7334/15945992803_a7aff15c25_b.jpg )
几分钟之后，就是一个全新的Edison了:D  
![Yocto](https://farm8.staticflickr.com/7333/15943615694_5ba7a21614_b.jpg)
