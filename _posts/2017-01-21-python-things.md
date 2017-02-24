---
title : Python笔记
layout : wiki_page
category : wiki
---

	print "Hello World"	# 单双引号没有大区别
	print a,b	# 会自动加一个空格
	print 4/3, 4.0/3
	print round(1.45),round(1.51)

文档
	
	pydoc os
	pydoc file.seek

格式化输出

	print "%d", a
	print "%s %d", (a,b)
	print a,
	print b	# 会接在上一句后面不换行
	c = "中文"
	print "%r", c	# 输出原始格式
					# %r is for debugging, %s is for displaying.
	
	format = "format%s"
	print format % a
	
	print """line1
	line2
	line3"""

\

	\\	Backslash (\)
	\'	Single-quote (')
	\"	Double-quote (")
	\a	ASCII bell (BEL)
	\b	ASCII backspace (BS)
	\f	ASCII formfeed (FF)
	\n	ASCII linefeed (LF)
	\N{name}	Character named name in the Unicode database (Unicode only)
	\r	Carriage Return (CR)
	\t	Horizontal Tab (TAB)
	\uxxxx	Character with 16-bit hex value xxxx (u'' string only eg:u'\U0001F47E')
	\Uxxxxxxxx	Character with 32-bit hex value xxxxxxxx (u'' string only)
	\v	ASCII vertical tab (VT)
	\ooo	Character with octal value ooo
	\xhh	Character with hex value hh

	while True:
		for i in ["/","-","|","\\","|"]:
			print "%s\r" % i,

输入

	input_t = raw_input("> ")	# string
	print "%s %r" % (input_t,input_t)
	print "%d" % int(input_t)
	

文件开头的编码

	# -*- coding: utf-8 -*-
	
	不加的话就会中文乱码

注意缩进，四格空格和TAB

	x in range(1,10)	# 0<x<10

字符串

	c = a + b

控制流

	for i in range(a,b):
		pass
	
	if (exp):
		pass
	elif (exp):
		pass
	else:
		pass

获取命令行参数

	import sys
	argv = sys.argv[0]	# argv[0]=='python'
	argv = sys.argv[1]

字符串

	str_list = str_data.split('.')	# 通过.划分字符串到一个list
	pos = str_data.find(' ')	# 查找首位置

类型转换
	
	str(int_data)	# 整型转字符串
	int(str_data)	# 字符串转整型
	float(str_data)

列表
	
	words = sorted(words)
	word = words.pop(0)
	word = words.pop(-1)

函数
	def func(*args):	#  args as list
		"""			"""
		pass
	def func(arg1, arg2):
		pass

	import module_name
	a = module_name.b()
	from module_name import *
	a = b()

	help(module_name.func)	# 返回"""	"""


打开文件

	f = open(name,'w')
	all_text = f.read()
	line = f.readline()
	f.truncate()	# 重写
	二进制的区别？？
	f.close()	# 为什么
	f.seek(0)	# 回到原位

	indata = open(file).read()	# 不需要close了

获取文件的名字和扩展名

	import os
	(name, extension) = os.path.splitext(path_in)
	print name,extension

	os.path.exists(file_name)	# 文件是否存在

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

布尔
	
	"test" and "test"	# return "test"
	1 and 1				# return 1
	True and 1			# return 1


