---
layout: page
title:  Ubuntu更新后“系统的网络服务与此版本的网络管理器不兼容”
---
更新一大堆软件以后，开机发现网络连接跪了,打开系统的网络连接面板，出现系统的网络服务与此版本的网络管理器不兼容的错误。

	$ sudo su
	$ cd /etc/NetworkManager/system-connections/
	$ ls -la
	$ mv <filename> /home/<username>/
	$ NetworkManager
