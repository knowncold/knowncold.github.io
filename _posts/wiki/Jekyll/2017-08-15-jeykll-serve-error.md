---
title: Ubuntu Jekyll serve报错
layout: page
category: wiki
---

在Ubuntu运行`jekyll serve`出错

```
FATAL: Listen error: unable to monitor directories for changes.
Visit https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers for info on how to fix this.
```

原因是serve的时候，jekyll需要监控一个目录的改动，而这个监控的数量是有限制的，运行下面的命令就好了。

```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
