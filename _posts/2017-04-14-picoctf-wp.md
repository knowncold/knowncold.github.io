---
title: picoCTF Write Up
layout: page
category: tutorial
---
> 最终 
> 293RD PLACE	2,710/6,575 PTS  
> 三个人中我打了大部分的题，大概是开始打CTF以来做出最多的一次吧  
> 毕竟简单，但也没有打过很多的高中生..

*LEVEL 1*

<hr/>

## Digital Camouflage

在WireShark里面直接搜索`user`或者`psword`，能找到类似的

	userid=grassers&pswrd=cHJ2cUJaTnFZdw%3D%3D

URL解码加上base64解码之后

> FLAG:
> prvqBZNqYw

## Special Agent User
在WireShark里面搜索`agent`就能找到

	User-Agent: Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.2117.157 Safari/537.36

到`http://www.useragentstring.com./`这个网站查询一下就能知道这个浏览器的版本了

> FLAG:
> Chrome 35.0.2117.157
