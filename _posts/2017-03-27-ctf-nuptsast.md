---
title : 南邮CTF训练平台 WriteUp
layout: page
category: tutorial
---
## 签到题 50

最基础的`view-source`

> nctf{flag_admiaanaaaaaaaaaaa}

## 签到2 50

Inspect一下，把输入框的`maxlength="10"`删掉就好了

> flag is:nctf{follow_me_to_exploit}

## 单身二十年 100

就是个跳转，用`Postman`看一下就能看到那个地址`http://chinalover.sinaapp.com/web8/search_key.php`

	<script>window.location="./no_key_is_here_forever.php"; </script>
	key is : nctf{yougotit_script_now}

## COOKIE 200

改一下`cookie`就行了

> flag:nctf{cookie_is_different_from_session}

## MYSQL 200

首先看到的是
	
	Do you know robots.txt？
	百度百科

进入`http://chinalover.sinaapp.com/web11/robots.txt`，又能看到

	别太开心，flag不在这，这个文件的用途你看完了？
	在CTF比赛中，这个文件往往存放着提示信息

	TIP:sql.php

```php
<?php
if($_GET[id]) {
   $_geT[mysql_connect(SAE_MYSQL_HOST_M . ':' . SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS);
  
  $id = intval($_GET[id]);
  $query = @mysql_fetch_array(mysql_query("select content from ctf2 where id='$id'"));
  if ($_GET[id]==1024) {
	  echo "<p>no! try again</p>";
  }
  else{
	echo($query[content]);
  }
}
?>	
```

可以看到首先最后的`$id`必须是`1024`，但是`$_GET[id]`不能是`1024`，那么就给一个小数比如`http://chinalover.sinaapp.com/web11/sql.php?id=1024.2`就行了

> the flag is:nctf{query_in_mysql}

## Header 250

在该页面的HTTP返回头里面直接就能看到

	HTTP/1.1 200 OK
	Date: Mon, 27 Mar 2017 11:03:52 GMT
	Server: Apache/2.2.15 (CentOS)
	X-Powered-By: PHP/5.3.3
	Flag: nctf{tips_often_hide_here}
	Content-Length: 132
	Connection: close
	Content-Type: text/html; charset=UTF-8

> nctf{tips_often_hide_here}

## esay! 50

	bmN0Znt0aGlzX2lzX2Jhc2U2NF9lbmNvZGV9

加上`=`，直接`base64`解码
	
	nctf{this_is_base64_encode}

