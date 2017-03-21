---
title : 使用SSH连接Github
layout : wiki_page
category : wiki
---

### 生成SSH Key

	$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
	Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]
	Enter passphrase (empty for no passphrase): [Type a passphrase]
	Enter same passphrase again: [Type passphrase again]

### 添加SSH Key到ssh-agent

	$ eval "$(ssh-agent -s)"
	$ ssh-add ~/.ssh/id_rsa

### 添加SSH Key到Github
	
	$ sudo apt-get install xclip
	$ xclip -sel clip < ~/.ssh/id_rsa.pub

在用户界面的`Settings`选择`SSH and GPG keys`，写个名字来区分不同的电脑，然后复制进去刚刚的key保存好就行了。

### 测试SSH连接

	$ ssh -T git@github.com

正常的会返回

	The authenticity of host 'github.com (192.30.252.1)' can't be established.
	RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
	Are you sure you want to continue connecting (yes/no)?

	The authenticity of host 'github.com (192.30.252.1)' can't be established.
	RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
	Are you sure you want to continue connecting (yes/no)?

	Hi username! You've successfully authenticated, but GitHub does not
	provide shell access.

### 日常使用
以上都正常以后就可以通过ssh来直接使用`git clone`、`git push`、`git pull`了。
