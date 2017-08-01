---
title : Python笔记
layout : wiki_page
category : wiki
---

```python
print "Hello World"	# 单双引号没有大区别
print a,b	# 会自动加一个空格
print 4/3, 4.0/3
print round(1.45),round(1.51)	# 四舍五入
sys.exit(0)
abs(a)
range(a,b,c)	# 从到b，间隔c
str_val.replace('!','')

import random
float_num = random.uniform(10,20)
```

## 文档

```python
pydoc os
pydoc file.seek
```

## 格式化输出

```python
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
format(val, '.2f')	# return a string
return '$%0.2f' % amount
return '${:.2f}'.format(amount)
```

## \转义

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

```python
while True:
	for i in ["/","-","|","\\","|"]:
		print "%s\r" % i,
```

## 输入

```python
input_t = raw_input("> ")	# string
print "%s %r" % (input_t,input_t)
print "%d" % int(input_t)
```

## 文件开头的编码

	# -*- coding: utf-8 -*-
	不加的话就会中文乱码
	注意缩进，四格空格和TAB

## string <-> list

```python
list = list(str)
str = ''.join(list)
```

## 控制流

```python
for i in range(a,b):
	pass
x in range(1,10)	# 0<x<10

while (exp):
	pass

if (exp):
	pass
elif (exp):
	pass
else:
	pass
```

## 获取命令行参数

```python
import sys
argv = sys.argv[0]	# argv[0]=='python'
argv = sys.argv[1]
```

## 字符串

```python
str_list = str_data.split('.')	# 通过.划分字符串到一个list
pos = str_data.find(' ')	# 查找首位置
"0" in str
c = a + b
```

## 类型转换

```python
str(int_data)	# 整型转字符串
int(str_data)	# 字符串转整型
float(str_data)
```

## 列表

```python
for ele in elements_list:
	for i in ele:
		pass
elements_list.apend(ele)
words = sorted(words)	# 排序
word = words.pop(0)
word = words.pop(-1)
words.append(word)	# 其实是append(words,word)
```

## 切片

```python
words[3:5]
```

### 字符串逆向

## 函数

```python
def func(*args):	#  args as list
	"""			"""
	pass
def func(arg1, arg2):
	pass
```

```python
import module_name
a = module_name.b()
from module_name import *
a = b()

help(module_name.func)	# 返回"""	"""
```

## 打开文件

```python
f = open(name,'w')
all_text = f.read()
line = f.readline()
f.truncate()	# 重写
二进制的区别？？
f.close()	# 为什么
f.seek(0)	# 回到原位

indata = open(file).read()	# 不需要close了
```

## 获取文件的名字和扩展名

```python
import os
(name, extension) = os.path.splitext(path_in)
print name,extension

os.path.exists(file_name)	# 文件是否存在
file_size = os.path.getsize(path)
```

## 图片灰度化

```python
from PIL import Image
im = Image.open(path).convert('L')
im.save(name+'_grey'+'.bmp')
```

## 图片和数组转换

```python
im = array(Image.new('L', (width, height)))	# 新建图片
im = array(Image.open(path))
pic= Image.fromarray(im, 'L')
im.shape[0]	# 高度
im.shape[1]	# 宽度
```

## 二进制文件读写

```python
import struct
byte_struct = struct.pack('B', data)	# char的二进制表示
file.write(byte_struck)
a, = struct.upack("B", file.read(1))
```

## 运行系统命令行命令

```python
import commands
cmd = 'ls ' + dir
out = cmmands.getoutput(cmd)
outlist = out.split('\n')
```

## 布尔

```python
"test" and "test"	# return "test"
1 and 1				# return 1
True and 1			# return 1

with X as Y:
	pass

assert False,"error"

class

del X[Y]

exec 'print "hello"'

except ValueError, e:
	print e
finally:
	pass
global X
1 is 1	# ==True

s = lambda y: y ** y if y<0 else y
raise ValueError("No")
try:
	pass
def X():
	yield Y
	X().next()

X = None

dic = {'x':1,'y':2}
```

## 字符串格式化

	%d	Decimal integers (not floating point).	"%d" % 45 == '45'
	%i	Same as %d.	"%i" % 45 == '45'
	%o	Octal number.	"%o" % 1000 == '1750'
	%u	Unsigned decimal.	"%u" % -1000 == '-1000'
	%x	Hexadecimal lowercase.	"%x" % 1000 == '3e8'
	%X	Hexadecimal uppercase.	"%X" % 1000 == '3E8'
	%e	Exponential notation, lowercase 'e'.	"%e" % 1000 == '1.000000e+03'
	%E	Exponential notation, uppercase 'E'.	"%E" % 1000 == '1.000000E+03'
	%f	Floating point real number.	"%f" % 10.34 == '10.340000'
	%F	Same as %f.	"%F" % 10.34 == '10.340000'
	%g	Either %f or %e, whichever is shorter.	"%g" % 10.34 == '10.34'
	%G	Same as %g but uppercase.	"%G" % 10.34 == '10.34'
	%c	Character format.	"%c" % 34 == '"'
	%r	Repr format (debugging format).	"%r" % int == "<type 'int'>"
	%s	String format.	"%s there" % 'hi' == 'hi there'
	%%	A percent sign.	"%g%%" % 10.34 == '10.34%'

## Data Types

	Type	Description	Example
	True	True boolean value.	True or False == True
	False	False boolean value.	False and True == False
	None	Represents "nothing" or "no value".	x = None
	strings	Stores textual information.	x = "hello"
	numbers	Stores integers.	i = 100
	floats	Stores decimals.	i = 10.389
	lists	Stores a list of things.	j = [1,2,3,4]
	dicts	Stores a key=value mapping of things.	e = {'x': 1, 'y': 2}

## json

```python
import json
test = {"username":"测试","age":16}
to_json = json.dumps(test)	# str
from_json = json.loads(to_json)	# dict
```

## Operators

	Operator	Description	Example
	+	Addition	2 + 4 == 6
	-	Subtraction	2 - 4 == -2
	*	Multiplication	2 * 4 == 8
	**	Power of	2 ** 4 == 16
	/	Division	2 / 4.0 == 0.5
	//	Floor division	2 // 4.0 == 0.0
	%	String interpolate or modulus	2 % 4 == 2
	<	Less than	4 < 4 == False
	>	Greater than	4 > 4 == False
	<=	Less than equal	4 <= 4 == True
	>=	Greater than equal	4 >= 4 == True
	==	Equal	4 == 5 == False
	!=	Not equal	4 != 5 == True
	<>	Not equal	4 <> 5 == True
	( )	Parenthesis	len('hi') == 2
	[ ]	List brackets	[1,3,4]
	{ }	Dict curly braces	{'x': 5, 'y': 10}
	@	At (decorators)	@classmethod
	,	Comma	range(0, 10)
	:	Colon	def X():
	.	Dot	self.x = 10
	=	Assign equal	x = 10
	;	semi-colon	print "hi"; print "there"
	+=	Add and assign	x = 1; x += 2
	-=	Subtract and assign	x = 1; x -= 2
	*=	Multiply and assign	x = 1; x *= 2
	/=	Divide and assign	x = 1; x /= 2
	//=	Floor divide and assign	x = 1; x //= 2
	%=	Modulus assign	x = 1; x %= 2
	**=	Power assign	x = 1; x **= 2

## Reduce

```python
sum=reduce(lambda x,y:x+y,(1,2,3,4,5,6,7))
```
