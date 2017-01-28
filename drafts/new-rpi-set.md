---
layout: page
title: Raspbian初始设置
---
### 安装Vim
	sudo apt-get install Vim

### 安装tightvncserver

### 设置WiFi
	sudo iwlist wlan0 scan
>输出的ESSID就是WiFi的SSID
	sudo vim /etc/wpa_supplicant/wpa_supplicant.conf
在原来的设置下面再加入
	network={
		ssid="_your-ssid_"
		psk="_your-password_"
	}
	
保存后
	sudo ifdown wlan0
	sudo ifup wlan0
	sudo ifconfig wlan0
>假如保存的wifi信息有误，会导致整个wifi无法启动

### 设置有线网络

### 设置静态IP地址

### 整合成shell脚本

### 安装tightvncserver
