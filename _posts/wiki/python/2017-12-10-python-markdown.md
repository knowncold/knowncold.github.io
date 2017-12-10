---
title: Python-Markdwon
layout: page
category: wiki
hidden: true
---

## Markdown
通过这个模块，可以把`Markdown`转换成`HTML`。

```python
import markdown

html = markdown.markdown(str_text)
```

```python
import markdown

html = markdown.markdownFromFile('./file.md')
```

### 防止中文编码导致问题
```python
import codecs

input_file = codecs.open("some_file.txt", mode="r", encoding="utf-8")
text = input_file.read()
html = markdown.markdown(text)
```

## 语法高亮
直接使用`pygments`的`Markdown`扩展

```python
html = markdown.markdown(text, extensions=['markdown.extensions.codehilite', 'markdown.extensions.fenced_code'])
```

> 只使用`codehilite`似乎会出现问题

