---
title: Windows和Ubuntu双系统的时间问题
layout: page
category: wiki
---

Windows会比Ubuntu的系统时间慢8小时

Ubuntu下:

    $ sudo timedatectl set-local-rtc 1
