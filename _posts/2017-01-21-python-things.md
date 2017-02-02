---
title : Python笔记
layout : page
category : wiki
---

控制流

	for i in range(a,b):
		pass

获取命令行参数

	import sys
	argv = sys.argv[0]	# argv=='python'
	argv = sys.argv[1]

文件开头的编码

	# -*- coding: utf-8 -*-

字符串

	str_list = str_data.split('.')	# 通过.划分字符串到一个list
	pos = str_data.find(' ')	# 查找首位置

进制转换
	
	str(int_data)	# 整型转字符串
	int(str_data)	# 字符串转整型
	
打开文件

	f = open(name,'w')
	line = f.readline()
	二进制的区别？？

获取文件的名字和扩展名

	import os
	(name, extension) = os.path.splitext(path_in)
	print name,extension

图片灰度化

	from PIL import Image
	im = Image.open(path).convert('L')
	im.save(name+'_grey'+'.bmp')

图片和数组转换
	
	im = array(Image.new('L', (width, height)))	# 新建图片
	im = array(Image.open(path))
	pic= Image.fromarray(im, 'L')
	im.shape[0]	# 高度
	im.shape[1]	# 宽度

二进制文件读写

	import struct
	byte_struct = struct.pack('B', data)	# char的二进制表示
	file.write(byte_struck)
	a, = struct.upack("B", file.read(1))

获得系统中文件的大小

	file_size = os.path.getsize(path)
	
运行系统命令行命令

	import commands
	cmd = 'ls ' + dir
	out = cmmands.getoutput(cmd)
	outlist = out.split('\n')

