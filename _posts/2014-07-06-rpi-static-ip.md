---
layout: page
title: 树莓派设置静态地址
---

由于学校机房的特殊环境，一根网线好像对应一个静态地址，所以树莓派不能使用原装的dhcp方式，需要手动设置。

一次性设置方式：

	$ sudo ifconfig eth0 192.168.1.222 netmask 255.255.255.0
	$ sudo route add page gw 192.168.1.1
	$ sudo ifconfig eth0 up

可以马上测试一下

	$ sudo apt-get update


永久性测试方式：

	$ sudo vim /etc/network/interfaces

修改为
	
	auto lo

	iface lo inet loopback
	iface eth0 inet static

	address 192.168.1.222
	netmask 255.255.255.0
	gateway 192.168.1.1

	allow-hotplug wlan0
	iface wlan0 inet manual
	wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf

重启服务

	$ sudo service networking restart

修改DNS

	$ sudo vim /etc/resolv.conf

改为类似于这样的
	
	nameserver 8.8.8.8
	nameserver 8.8.4.4
	nameserver 208.67.220.220
	nameserver 208.67.222.222	

再测试一下：

	$ sudo apt-get update


### 修改WiFi的静态地址
原来可能是这样的
	# interfaces(5) file used by ifup(8) and ifdown(8)

	# Please note that this file is written to be used with dhcpcd
	# For static IP, consult /etc/dhcpcd.conf and 'man dhcpcd.conf'

	# Include files from /etc/network/interfaces.d:
	source-directory /etc/network/interfaces.d

	auto lo
	iface lo inet loopback

	iface eth0 inet manual

	allow-hotplug wlan0
	iface wlan0 inet manual
	    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

	allow-hotplug wlan1
	iface wlan1 inet manual
	    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf







PS:  似乎设置静态地址对于WiFi连接也有影响。
