---
title: Linux查看十六进制
layout: page
---

Vim中输入
`%!xxd` 将当前文本转化成16进制
`%!xxd -r`将当前文件转换回文本格式

hexdump
`-c` 以字节为单位，显示出ASCII码
`-C` 以字节为单位，同时显示十六进制和ASCII码  

例如`hexdump -C file`


