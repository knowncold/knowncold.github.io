---
title: PHP下载文件
layout: wiki_page
category: wiki
---

### 直接给出文件地址

```php
<?php
	echo "<a href=\"./index.html.bak\">Download</a>";
?>
```

### 通过重定向

```php
<?php
	if($_GET['in']=='234'){
		Header("Location: ./index.html.bak");
		exit;
	}
?>
```

### 通过header

但是一开始就不能`echo`其他东西了

```php
<?php
	$filepath = "./index.html.bak";
	$file=fopen($filepath,"r");
	Header("Content-type:application/octet-stream");
	Header("Accept-Ranges:bytes");
	Header("Accept-Length:".filesize($filepath));
	Header("Content-Dispositon:attachment;filename=download.html.bak");

	echo fread($file,filesize($filepath));
	fclose($file);
	exit();
?>
```




