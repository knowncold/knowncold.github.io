---
title: 在树莓派上玩CS
date: 2014-06-24 10:28:06
category: project
---

好不容易找到一个树莓派上可以玩的「大型游戏」。  <!--more-->
官方有很详细的[说明](http://www.raspbian.org/RaspbianQuake3),具体命令如下。

### 更新

```bash
sudo apt-get update
sudo apt-get dist-upgrade
sudo rpi-update 192
```

第三步我使用时失败，应该是因为我刷的镜像就是最新的。
### 安装需要用到的软件

```bash
sudo apt-get install git gcc build-essential libsdl1.2-dev
```

### 下载雷神之锤的源码

```
mkdir ~/src
cd ~/src
git clone https://github.com/raspberrypi/quake3.git
cd quake3
```

### 修改build.sh文件  
把第8行改成:  

```
ARM_LIBS=/opt/vc/lib
```

把第16行改成:

```
INCLUDES="-I/opt/vc/include -I/opt/vc/include/interface/vcos/pthreads"
```

把第19行注释掉:

```
#CROSS_COMPILE=bcm2708-
```

- 编译（大概一小时）

```
./build.sh
```


- 找到一些文件（应该是素材之类的吧），并把他们放到  
```
build/release-linux-arm/baseq3:
```

`pak0.pk3`, `pak1.pk3`, `pak2.pk3`, `pak3.pk3`, `pak4.pk3`, `pak5.pk3`, `pak6.pk3`, p`ak7.pk3`, `pak8.pk3`  
网上有人整理好了,去[网盘](http://dl.vmall.com/c0dylv4dzp)下载就行。

- 提升pi用户的权限

```
sudo usermod -a -G video [your_username]
```

- 运行`ioquake3.arm`，开始玩耍吧。


> 参考：[http://ju.outofmemory.cn/entry/3235](http://ju.outofmemory.cn/entry/3235)
