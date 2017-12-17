---
title: Batocera上配置neogeo模拟器
layout: page
category: wiki
hidden: true
---

neogeo模拟器和常规的其他模拟器有所区别，首先他的rom是一个zip压缩文件，里面有很多小的文件，不像NES这种是一个nes结尾的小文件。

此外呢，他运行的时候除了本身这个zip文件还需要一个neogeo的文件（被称为BIOS或者驱动文件），具体来讲就是，你想玩一个neogeo游戏时，除了这个游戏本身，在这个目录下面还需要有一个neogeo.zip的驱动文件，否则是无法成功运行的，一开始我甚至以为下载到的拳皇rom都是有问题的、不完整的，其实需要的就是一个neogeo的驱动文件。

对于Batocera的FBA模拟器来说，需要把rom和neogeo的驱动文件一起粘贴到fba_libretro文件夹，然后重新扫描一遍游戏文件，就能开始玩了。

[图片]

## 参考

> https://github.com/recalbox/recalbox-os/wiki/Easy-Arcade-on-Recalbox-%28EN%29
