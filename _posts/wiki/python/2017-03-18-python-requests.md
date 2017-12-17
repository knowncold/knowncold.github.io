---
layout: page
title: Python-Requests笔记
category: wiki
---

Python `Requests`库的口号就是`HTTP for Humans`，比起其他的一些HTTP工具也确实方便人性化一点..

## 基本的GET请求
```python
r = requests.get(url)
r.status	# 返回的HTTP状态
r.text		# 返回的原文
r.encoding	# 返回的编码
payload = {'key1': 'value1', 'key2': 'value2'}	# 附带的参数
r = requests.get(url, params=payload)
r.url	# 请求的完整url包括了GET参数
```

## POST请求
```python
>>> r = requests.post('http://httpbin.org/post', data = {'key':'value'})
>>> json=
```

### json数据
```python
import json

headers = {'Content-Type': 'application/json'}
url = ""
post_data = {"key1":value1, "key2":value2}
r = requests.post(url, json=post_data)
```

## Session

## Cookies

## JSON

## 下载文件
