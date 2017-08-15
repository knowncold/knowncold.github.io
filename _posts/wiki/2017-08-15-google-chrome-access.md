---
title: WebStorm调试Chrome时ajax出错
layout: page
category: wiki
---

WebStorm调试Chrome时，假如有ajax请求本地文件，就会出现这样的错误

```
Cross origin requests are only supported for protocol schemes: http, data, chrome, chrome-extension, https.
```

只需要从终端启动的时候，加上`--allow-file-access-from-files`的后缀即可。
