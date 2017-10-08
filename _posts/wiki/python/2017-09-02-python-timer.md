---
title: Python定时器
layout: page
category: wiki
hidden: true
---

```python
import threading

def func():
    timer = threading.Timer(1.0/60, func)
    timer.start()

timer = threading.Timer(0, func)
timer.start()
```

函数中是否需要`global timer`。
