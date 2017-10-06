---
title: Jekyll使用Rouge
layout: page
category: wiki
---
之前一直配不好`Jekyll`的语法高亮，前段时间又尝试配置了一次`Pygments`，但是马上Github自动发来邮件说`Build Warning`，原来是他们强制要求使用`Rouge`的语法高亮，不支持`Pygments`。

配置之前觉得很麻烦，其实也很简单

`Rouge`本身内置了好几种语法高亮的样式，可以用`rougify`这个命令来生成样式，虽然我也不知道`rougify`这个命令什么时候就在我电脑上了..

	$ rougify monokai > synatax.css

其他的样式包括了

	$ rougify help style
	usage: rougify style [<theme-name>] [<options>]

	Print CSS styles for the given theme.  Extra options are
	passed to the theme.  Theme defaults to thankful_eyes.

	options:
	  --scope	(default: .highlight) a css selector to scope by

	available themes:
	  base16, base16.dark, base16.monokai, base16.monokai.light, base16.solarized, base16.solarized.dark, colorful, github, molokai, monokai, monokai.sublime, thankful_eyes

生成后的`css`放在随便哪里，然后在`layout`文件里面写上连接就能正常使用了，不过如果之前自己写的`css`涉及到了`<code>`或者`<pre>`这些标签的话，似乎是会和这个语法高亮的`css`产生冲突的，可能需要手动修改一下这个文件。
