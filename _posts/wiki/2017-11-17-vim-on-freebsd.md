---
title: FreeBSD上使用VIM
layout: page
category: wiki
---

## 安装完整VIM

```bash
pkg install vim
```

## 安装Git

需要通过Git获取`vimrc`和相关的插件

```bash
pkg install git
```

### 下载vimrc

下载我的[`vimrc`](https://github.com/knowncold/vim)，并复制到用户目录

```bash
git clone https://github.com/knowncold/vim.git
cd vim
cp .vimrc ~/.vimrc
cp colors ~/.vim/colors
```

## 安装Vundle

可以参考之前的文章[Windows安装Vundle](http://blog.knowncold.me/wiki/2016/08/22/vundle-install-on-windows.html)

```bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

## vim插件

打开vim，使用`PluginInstall`命令

## 自动补全
