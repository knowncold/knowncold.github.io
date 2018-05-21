---
title: Emacs Notes
layout: page
category: wiki
---

## 基本概念
和`.vimrc`对应的，Emacs一般是`.emacs`，也有其他的。

## 基本键位

- `C-x C-s` 保存
- `C-x C-c` 退出
- `C-x C-f` 打开文件
- `C-s`向下查找
- `C-r`向前查找
- `C-g`取消查找

## Evil
在Emacs中使用vim的键位，这也是我能够开始使用Emacs的原因

## NeoTree
仿Vim的`NerdTree`的插件，官方有中文[wiki](https://www.emacswiki.org/emacs/NeoTree_%E4%B8%AD%E6%96%87wiki)


```elisp
(add-to-list 'load-path "/some/path/neotree")
(require 'neotree)
(global-set-key [f8] 'neotree-toggle)
```

需要添加下面的代码才能解决键位的冲突:

```elisp
(add-hook 'neotree-mode-hook
	(lambda ()
		(define-key evil-normal-state-local-map (kbd "TAB") 'neotree-enter)
		(define-key evil-normal-state-local-map (kbd "SPC") 'neotree-enter)
		(define-key evil-normal-state-local-map (kbd "q") 'neotree-hide)
		(define-key evil-normal-state-local-map (kbd "RET") 'neotree-enter)))
```

### 使用

- `g`刷新树
- `H`切换显示隐藏文件
- `C-c C-c`切换根目录
- `C-c C-n`创建文件或者目录
- `C-c C-r`重命名文件或者目录
- `C-c C-d`删除文件或者目录

## 设置主题

在[这个网站](https://emacsthemes.com/)选择喜欢的主题，比如`molokai`，首先下载这个扩展包

```elisp
(require 'package)
(add-to-list 'package-archives
             '("melpa" . "http://melpa.org/packages/"))
(package-initialize)
```

然后通过`M-x package-refresh-content`下载包索引

接着通过`M-x package-list-packages`获取当前可用插件的列表，搜索到相关的Theme之后通过鼠标就能完成下载

`M-x load-theme`使用`Tab`键获取列表选择一个，暂时使用这个Theme

在配置文件中写入下面的代码可以自动启动加载这个Theme:

```elisp
(load-theme 'molokai t)
```

## Org-mode

## Flycheck
http://www.flycheck.org/en/latest/

## Helm

## ycmd
http://blog.csdn.net/chris_linchen/article/details/46896495
http://blog.csdn.net/csfreebird/article/details/48898801
http://blog.csdn.net/victo2012/article/details/50570099

## Markdown

## 设置字体

直接在`.emacs`添加下面的配置

```elisp
(set-default-font "Monaco-13")
```

## 缓冲和窗口
`C-x 4 b`在另一个window中打开或新建一个缓冲
`C-x 5 b`在另一个frame中打开或新建一个缓冲
`C-x C-b`显示所有缓冲
`C-x 4 f filename`在另一个窗口打开文件
`C-x ^` (enlarge-window)增高当前窗口
`C-x }` (enlarge-window-horizontally)将当前窗口变宽
`C-x +` (balance-windows)所有窗口一样高
`C-x -` (shrink-window-if-larger-than-buffer)如果窗口比缓冲大就缩小

## 设置行号
```elisp
(global-linum-mode 1) ; always show line numbers
(setq linum-format "%d| ")  ;set format
```

## company-mode 修改补全的键位
原生的补全是`M-n`，和之前的`TAB`的习惯不是很适应，google到一个[issue](https://github.com/company-mode/company-mode/issues/68)，但是他给的方法不好用，可以直接修改源代码`/company/company.el`，搜索到原来的地方：

```elisp
(define-key keymap (kbd "M-n") 'company-select-next)

(define-key keymap [tab] 'company-complete-common)
(define-key keymap (kbd "TAB") 'company-complete-common)
```

把后两行注释掉，前一行修改成`TAB`即可，需要注意的是还得`M-x byte-compile-file`来编译成`*.elc`文件才能生效。

## Scheme

## 参考资料

- [一年成为 Emacs 高手 (像神一样使用编辑器)](https://github.com/redguardtoo/mastering-emacs-in-one-year-guide/blob/master/guide-zh.org)
- [陈斌](http://blog.binchen.org/index.html)
- http://emacsrocks.com/
- https://github.com/purcell/emacs.d
- [Planet Emacsen](http://planet.emacsen.org/)
- http://docs.huihoo.com/homepage/shredderyin/index.html
