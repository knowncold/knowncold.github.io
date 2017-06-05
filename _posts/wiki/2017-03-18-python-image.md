---
title : Python-PIL笔记
layout: wiki_page
category: wiki
---

几何变换

	im = Image.open('test.jpg')
	out = im.rotate(90)		# 长宽不一样的时候要注意
	out = im.transpose(Image.FLIP_LEFT_RIGHT)	# 水平翻转
	out = im.transpose(Image.FLIP_TOP_BOTTOM)	# 垂直翻转
	out = im.transpose(Image.ROTATE_90)
	out = im.transpose(Image.ROTATE_180)
	out.save('out.jpg')
