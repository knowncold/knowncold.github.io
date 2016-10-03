---
layout: default
title: Windows安装Vundle
---

### 安装Git
Vundle基于Git，每一个插件都是一个项目的Repository，通过Vundle可以在_vimrc里面通过简单的指令，一键安装/更新/删除所有插件。

下载并安装[Git-For-Windows](https://git-for-windows.github.io/)
>注意添加环境变量

在命令提示符中输入`git --version`能正常显示时则安装成功

### 配置Curl脚本
在Git的安装路径的cmd文件夹下新建一个curl.cmd文件，编辑内容为

	@rem Do not use "echo off" to not affect any child calls.
	@setlocal

	@rem Get the abolute path to the parent directory, which is assumed to be the
	@rem Git installation root.
	@for /F "delims=" %%I in ("%~dp0..") do @set git_install_root=%%~fI
	@set PATH=%git_install_root%\bin;%git_install_root%\mingw\bin;%git_install_root%\mingw64\bin;%PATH%
	@rem !!!!!!! For 64bit msysgit, replace 'mingw' above with 'mingw64' !!!!!!!

	@if not exist "%HOME%" @set HOME=%HOMEDRIVE%%HOMEPATH%
	@if not exist "%HOME%" @set HOME=%USERPROFILE%

	@curl.exe %*

>注意你的git是32位还是64位的

保存后，在命令提示符输入`curl --version`能正常显示时则安装成功

### 下载Vundle

从Github上clone下来（其实下载去网页下载zip文件然后复制过去也是可以的）

	git clone https://github.com/gmarik/vundle "C:\Program Files (x86)\Vim\vimfiles\bundle\vundle"

这时的文件目录应该类似于

	Vim  
	+---vim74  
	+---vimfiles  
	+------bundle  
	+---------vundle  
	+------------autoload 

### 配置Vundle并安装插件
编辑_vimrc文件

	set nocompatible              " 去除VI一致性,必须
	filetype off                  " 必须

	" 设置包括vundle和初始化相关的runtime path
	set rtp+=~/.vim/bundle/Vundle.vim
	call vundle#begin()
	" 另一种选择, 指定一个vundle安装插件的路径
	"call vundle#begin('~/some/path/here')

	" 让vundle管理插件版本,必须
	Plugin 'VundleVim/Vundle.vim'

	" 以下范例用来支持不同格式的插件安装.
	" 请将安装插件的命令放在vundle#begin和vundle#end之间.
	" Github上的插件
	" 格式为 Plugin '用户名/插件仓库名'
	Plugin 'tpope/vim-fugitive'
	" 来自 http://vim-scripts.org/vim/scripts.html 的插件
	" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
	Plugin 'L9'
	" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
	Plugin 'git://git.wincent.com/command-t.git'
	" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
	Plugin 'file:///home/gmarik/path/to/plugin'
	" 插件在仓库的子目录中.
	" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
	Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
	" 安装L9，如果已经安装过这个插件，可利用以下格式避免命名冲突
	Plugin 'ascenator/L9', {'name': 'newL9'}

	" 你的所有插件需要在下面这行之前
	call vundle#end()            " 必须
	filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
	" 忽视插件改变缩进,可以使用以下替代:
	"filetype plugin on
	"
	" 简要帮助文档
	" :PluginList       - 列出所有已配置的插件
	" :PluginInstall    - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
	" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
	" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
	"
	" 查阅 :h vundle 获取更多细节和wiki以及FAQ
	" 将你自己对非插件片段放在这行之后

>其中`Plugin`后面的Repository路径有多种写法
>推荐WakaTime插件

保存_vimrc文件后，打开`gvim`，运行`:PluginInstall`

### 参考资料
[Vundle_README](https://github.com/VundleVim/Vundle.vim/blob/master/README_ZH_CN.md)  
[Windows下安装Vim插件管理Vundle](http://blog.csdn.net/zhuxiaoyang2000/article/details/8636472)  
[我的_vimrc](https://github.com/knowncold/IDE_Settings/blob/master/Vim/_vimrc)  
