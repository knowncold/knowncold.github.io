---
title  : YouCompleteMe的安装
layout : wiki_page
category : wiki
---
之前在Windows上尝试安装YCM的时候非常痛苦，各种出错，现在已经忘了具体的出错和解决方案了，但是我是不会再去`Windows上`用`Gvim`了.. 
相比较而言在Ubuntu上安装就简单多了去了，这次我安装了C系语言、JavaScript还有Python的补全。

首先通过Vundle下载YCM的`Github`资源，然后再执行编译

	$ sudo apt-get install build-essential cmake
	$ sudo apt-get install python-dev python3-dev
	$ sudo apt-get install npm
	$ cd ~/.vim/bundle/YouCompleteMe
	$ ./install.py --clang-completer --tern-completer

其中下载`Clang`的时候巨慢无比，最后放置了一节毛概课才下完。

### YCM Generater

`YCM`在使用的时候会自动在编译前寻找语法错误，包括已包含的库里面是否有当前使用的函数之类的功能，这些东西都是类似于`.vimrc`对于`Vim`一样，`YCM`有`.ycm_extra_conf.py`，通过这个配置文件，在使用`Vim`的时候，它才知道去哪些库找到当前这些函数是否可用，还有是否支持C++11和以前的C++兼容之类的问题。

使用`YCM Generater`可以很方便的来自动生成对应的`.ycm_extra_conf.py`，通过`Vundle`就能很方便的安装好。

##### 环境要求

* Python 2
* Clang

#####  使用方法

```bash
./config_gen.py PROJECT_DIRECTORY
```

> 这个`PROJECT_DIRECTORY`就是需要配置文件的项目的根目录了，
