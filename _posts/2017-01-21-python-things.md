---
title : Python笔记
layout : default
---

获取命令行参数

	import sys
	argv = sys.argv[1]

文件开头的编码

	# -*- coding: utf-8 -*-

进制转换
	
	str(int_num)	#整型转字符串
	
打开文件

	f = open(name,'w')
	二进制的区别？？

获取文件的名字和扩展名

	import os
	(name, extension) = os.path.splitext(path_in)
	print name,extension

图片灰度化

	from PIL import Image
	im = Image.open(path).convert('L')
	im.save(name+'_grey'+'.bmp')

	im.shape[0]	#高度
	im.shape[1]	#宽度

