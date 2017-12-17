---
title: Ubuntu开启ssh
layout: page
category: wiki
---

现在寝室以前的一台笔电几乎用作台式机了，甚至不想在上面写代码了，于是直接当作一个小服务器，在上面搭各种环境，用新笔电传代码上去

	$ sudo apt-get install openssh-server
	$ service ssh start

另一台笔电就能直接ssh上去了
