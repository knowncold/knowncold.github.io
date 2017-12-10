---
title: Jekyll笔记
layout: page
category: wiki
hidden: true
---

{% assign open = '{%' %}
嵌入Liquid代码时，防止一并转义，使用`{{ open }} raw %}`标签

```liquid
{% raw %}
{% raw %}
{% for i in list %}
  {{ i.name }}
{% endfor %}
{% endraw %}
{{ open }} endraw %}
```

还要考虑的问题是假如需要在这个代码里面加入`{{ open }} endraw %}`会非常麻烦。
