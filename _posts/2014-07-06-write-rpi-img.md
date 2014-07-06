---
layout: default
title: 烧写树莓派镜像
---
Linux下：

1. 在终端中输入

	$ df -h

2. 插入SD卡，重复步骤1
3. 比较两次输出结果，判断哪一个是SD卡的分区，格式应该类似于

	/dev/sdb1

4. 卸载SD卡

	$ sudo umount /dev/sdb1

5. 确定原始设备名称，如/dev/sdb1的原始设备名称为/dev/sdb
6. 解压缩镜像文件后，写入

	$ sudo dd bs=1M if=~/下载/2014-01-07-wheezy-raspbian.img of=/dev/sdb

7. 等待几分钟以后烧写完成，dd命令在最后才会有统计信息

	记录了2825+0 的读入
	记录了2825+0 的写出
	2962227200字节(3.0 GB)已复制，705.317 秒，4.2 MB/秒

为保险起见，可以运行
	$ sudo sync 
	
确保所有数据都被正确写入了

８. 将SD卡插入RPi,应该可以使用了。

