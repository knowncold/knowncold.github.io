---
title: Python串口库
layout: page
category: wiki
hidden: true
---

```python
import serial
```

打开的时候，可以设置真正阻塞的模式

```python
timeout = None      # 长时间等待
timeout = 0     # 不阻塞形式 (读完之后就返回)
timeout = x     # x秒后超时 (float allowed)
```

伪串口中断需要用多线程来模拟
