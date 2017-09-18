---
title: Mac OS安装iPython
layout: page
category: wiki
---
在Mac上直接通过`pip install ipython`会报错，完全无法安装上，不管有没有`sudo`

## 解决方法

```python
pip install ipython --user -U
easy_install ipython
```

## 参考

> [http://www.nikochan.cc/2017/02/16/SolutionIpythonInstall/](http://www.nikochan.cc/2017/02/16/SolutionIpythonInstall/)
