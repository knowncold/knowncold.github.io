---
title  : YouCompleteMe的安装
layout : wiki_page
category : wiki
---
之前在Windows上尝试安装YCM的时候非常痛苦，各种出错，现在已经忘了具体的出错和解决方案了，但是我是不会再去`Windows上`用Gvim了.. 
相比较而言在Ubuntu上安装就简单多了去了，这次我安装了C系语言、JavaScript还有Python的补全。

首先通过Vundle下载YCM的github资源，然后再执行编译

	$ sudo apt-get install build-essential cmake
	$ sudo apt-get install python-dev python3-dev
	$ sudo apt-get install npm
	$ cd ~/.vim/bundle/YouCompleteMe
	$ ./install.py --clang-completer --tern-completer

其中下载`Clang`的时候巨慢无比，最后放置了一节毛概课才下完。

