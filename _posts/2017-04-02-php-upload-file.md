---
title: PHP文件上传
layout: wiki_page
category: wiki
---

#### 表单

```html
<form action="upload.php" method="post"
enctype="multipart/form-data">
<label for="file">Filename:</label>
<input type="file" name="file" id="file" /> 
<input type="text" name="descrip" id="qwe"/>
<br/>
<input type="submit" name="submit" value="Submit" />
</form>
```

#### 获取文件信息

```php
<?php
	if($_FILES["file"]["error"]){
		echo "Error:".$_FILES["file"]["error"]."<br/>";
	}else{
		echo "descrip: ".$_POST["descrip"]."<br/>";
		echo "Sucess: ".$_FILES["file"]["name"]."<br/>";
		echo "Type: ".$_FILES["file"]["type"]."<br/>";
		echo "Size: ".($_FILES["file"]["size"] / 1024)."<br />";
		echo "Stored in: ".$_FILES["file"]["tmp_name"];
	}
?>
```

但是此时的文件存在`/tmp`文件夹，需要移动到其他地方去

#### 保存文件

```php
<?php
move_uploaded_file($_FILES["file"]["tmp_name"],
	"./static/".$_FILES["file"]["name"]);
?>
```
要注意文件夹的写入权限

