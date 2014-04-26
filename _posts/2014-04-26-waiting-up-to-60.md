---
layout: default
title: 开机出现等待60秒
---
上次解决网络图标消失后，发现每次开机都要出现
	Waiting up to 60 seconds for network configuration
终于找到了解决方法。

	$ sudo su
	# cd /etc/init
	# vim failsafe.conf

	把其中的
	sleep 59
改成0,如果不行就改成5或10.
