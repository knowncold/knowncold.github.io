---
layout: page
title: 网络服务与管理器不兼容
---

成功连上网以后，手贱更新了所有的软件。

第二天开机的时候，发现没有网络连接那个小图标了！

打开系统设置>网络,	出现“系统的网络服务器与此版本的网络管理器不兼容”。

找到一篇文章。

	$ sudo su
	# cd /etc/NetworkManager/system-connections/
	# ls -la
	# mv <filename> /home/<username>/
	# NetworkManager

终于出现久违的小图标了。
