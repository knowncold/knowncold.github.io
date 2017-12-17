---
title: Mac配置iTerm和zsh
layout: page
category: wiki
hidden: true
---

iTerm直接去官网下载就行。

## 快捷键打开iTerm
使用`Automator.app`，新建服务->运行AppleScript，“服务”收到改为没有输入

```
on run {input, parameters}

	(* Your script goes here *)
	tell application "iTerm"
		reopen
		activate
	end tell
end run
```

保存成`Open iTerm`，打开系统偏好设置->键盘->快捷键->服务，找到刚刚保存的服务，直接设置快捷键。

## zsh

```
brew install zsh
```

在系统偏好设置->用户与群组，点开锁，管理员右键进入高级选项，修改登录shell为`zsh`

### oh my zsh
一键安装

```bash
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

### 主题

一般使用agnoster，需要下载PowerLine的字体并在iTerm里面设置。
