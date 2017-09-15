---
layout: page
title: Linux命令行
category: wiki
---

	$ ls

	$ cd dir/
	$ cd ..

	$ mv file1 file2
	$ mv /file dir/file

	$ rm file
	$ rm -rf dir

	$ mkdir

	$ ssh usr@192.168.1.201

	$ sudo apt-get install software
	$ sudo apt-get update

	$ less file

	$ ps
	$ ps aux | less
	$ top

	$ kill PID
	$ cat
	$ head file
	$ tail file
	$ touch file

	$ man command

	$ tree

	$ scp /home/file root@192.168.1.105:home/pi
	$ scp root@192.168.1.105:home/pi /home/knoc
	$ scp -r home/pi/ root@asd:home/pi
	$ scp -r root@as.sd.awq.we:home/pi /home/pi

	$ tar -jxvf xxx.tar.bz2

	$ unzip xxx.zip

	$ df -h
	$ sudo dd bs=1M if=~/下载/2014-01-07-wheezy-raspbian.img of=/dev/sdb

	$ sudo umount /dev/sdb1

	$ ls -l | grep "^-" | wc -l

	$ sed -i "s/default/page/g" `grep default -rl _posts`

	$ chmod -R 777 /html

往一个程序输入`stdin`

	$ echo 'This string will be piped to stdin.' | /my/bash/script

输出开机时的调试信息

	$ demesg
