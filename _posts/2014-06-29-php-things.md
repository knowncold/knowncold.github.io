---
layout: wiki_page
title: PHP笔记
category: wiki
---
开头要有`<?php`,结尾要有`?>`

常量定义

	define ("TOKEN",random);
	
变量直接使用，不用定义类型。

    $a = 1;
    $b = "我是变量";

语句结束有引号

函数声明

	function name(){
		//code
		return 0;
	}

退出

	exit(0);

等于`==`  
`0`和`false`等价
全等于`===`  
可区分`0`、`false`、`空字符串`和`空数组`。

内建函数

	simplexml_load_string(data)；//处理XML
	$a = array($a,$b,$c);		//组装数组
	sort ( $array );		//字典排序，即按照字母顺序
	$d = implode ( $array );	//将数组合并成字符串
	sha1 ($d);			//字符串SHA1加密
	var_dump($ary);
	var_export($ary);	//返回值，无类型,有格式
	file_put_contents("/st.txt","$a",FILE_APPEND);
	

	echo "<br>";
	echo $a;
	echo "$a,$b";
	echo $a.",".$b;

数组

	$ary = array("s","w",2);
	$ary[0]= "s";
	$ary = array(
		'dog' => 23;
		2 => 38
	);
	$ary['q'] = "asd";

变量转成整数类型
	
	$a = intval($b);

连接数据库
> 注意PHP版本

	mysql_connect(SAE_MYSQL_HOST_M . ':' . SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS);
	mysql_select_db(SAE_MYSQL_DB);
	$query = @mysql_fetch_array(mysql_query("select content from ctf2 where id='$id'"));
