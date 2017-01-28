---
layout: page
title: 安装PHP
---
	使用PHP5
Windows下使用[AppServ](http://www.appservnetwork.com/)
	要确保系统中原本没有安装Apache、PHP、MySQL。
	端口选择80，小心其它软件的端口占用。
	选择UTF-8编码，该页不选择，其他步骤全选。
	
	ZendStudio编辑代码
	PHP中文手册
	MySQL中文手册
Linux下使用[XAMPP](https://www.apachefriends.org/zh_cn/index.html)
	使用一些类库可能不正常，如jpgraph.
	
	$ chmod 755 xampp-linux-1.8.3-4-installer.run
	$ sudo ./xampp-linux-1.8.3-4-installer.run
	
	会出现图形化安装界面。
	$ sudo chmod 777 /opt/lampp/htdocs/ -R
