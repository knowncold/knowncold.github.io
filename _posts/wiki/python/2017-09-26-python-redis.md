---
title: Python-Redis
layout: page
category: wiki
---

```python
import redis

r = redis.Redis(host='127.0.0.1', port=6379)
r.set('name', 'test', ex=3600)

r.get('name')
```
