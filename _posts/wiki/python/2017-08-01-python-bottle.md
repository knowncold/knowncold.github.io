---
title: Python Bottle
layout: page
category: wiki
---
使用Ajax时的跨域请求问题

```python
from bottle import route, run, template, response
@route('/')
def index():
    response.headers['Access-Control-Allow-Origin'] = '*'
    response.headers['Access-Control-Allow-Methods'] = 'GET, POST, PUT, OPTIONS'
    response.headers['Access-Control-Allow-Headers'] = 'Origin, Accept, Content-Type, X-Requested-With, X-CSRF-Token'
    return ret_str
```
