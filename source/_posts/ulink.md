---
title: Ulink——基于微信的物联网平台
date: 2014-09-21 00:00:00
tags:
---


本文成稿于高三初，参加了ArduinoCN的比赛，许多图片现已丢失，唯一可考是论坛现存的[帖子](http://www.arduino.cn/thread-7368-1-1.html)和当时的发帖时间`2014-9-21 11:10`。   <!--more-->
> 然而直到大学，前来交流该项目方案的朋友仍然络绎不绝  
> 若想了解最新的项目情况，请关注[YouLinkedMe](http://youlinked.me)  

参赛项目：Ulink

参赛组员： 
1人——某高三 

## 联系方式

如果想做这个项目，但觉得有困难，请直接联系  
微信号  ====  
QQ群   29171518 

> 商业用途请用上述联系方式及时联系我。

由于新浪云的不断更新，许多界面有所变化，操作也有变化，目前需要新浪云实名认证，有些步骤类似，也有些步骤完全不一样了，多百度百度就行了。

## 项目介绍

用Arduino控制物联网的方案很多，被控端和控制端的连接方式也有很多，比如蓝牙，Wifi，433Mhz模块；如果涉及到互联网和手机的远程控制，也有一些成熟的业界方案，比较普遍的是一些物联网公司比如智能插座，一个公司专门推出一个专门的App，每个App都长得各种各样，有些操作方便，有些麻烦。

<div style="text-align: center">
<iframe height=498 width=510 src='http://player.youku.com/embed/XNzk3NDMyOTg4' frameborder=0 'allowfullscreen'></iframe>
</div>

当我有一天看到有人用微博控制Arduino的时候，感觉相当酷炫，但是微博控制的方便性和安全性是不高的，同时微信却有极高的安全性，对于每天使用微信的人来说，操作显然是最方便的。

整个项目的开发，主要是软件层面，因为控制实现以后，只要把点亮LED的代码改成别的，就可以控制另外的设备了，成功之后才发现，需要的技能其实不少：  
Linux，Arduino，PHP，微信公众平台开发，MySQL  

如果如果你原本只是玩Arduino或硬件的，这时候去看我的代码（虽然我也是自己学的，写的也很短），PHP网络之类的比较难懂，这也是为什么我不能够把我所有的开发流程一下子写出来的原因。即使你有了我的服务器代码，然后再去进行自己的架设，那也不是随便改几个参数就可以的，我也想过直接开发一个 物联网平台，无奈一转眼已经高三。

换句话说，只要修改相应的代码，其他有关物联网远程控制或数据获取的参赛项目都可以接入本项目，使得操作更加方便

代码全在Github。[https://github.com/llLord/Ulink/tree/master/V2.5](https://github.com/llLord/Ulink/tree/master/V2.5)


整个原理是不难理解的。

![](https://i.postimg.cc/k4KwvNFZ/ulink-0.png)

### 控制Arduino

首先服务器上有一个数据库，数据库里面有几个记录，每个开关都有一个对应的值。  
在微信中，我们对一个公众平台发送类似于`开灯`，`打开热水器`之类的命令，以“开灯”为例公众平台的后台服务器会对这个命令进行判断，如果符合预设的命令，就会进入数据库，找到这个LED对应的记录，把这个记录对应的值改为`1`（值其实是随意的）。  
这里的命令发送方式包括文字消息，语音消息。  

与此同时，Arduino通过W5100扩展板，不断向一个服务器上的页面发送请求，请求中会包含一些诸如请求的开关ID，密码等参数，服务器核实后，就会进入数据库，找到对应的开关的记录，把对应的值`1`反馈给Arduino，Arduino收到反馈后，就会进行判断，如果是`1`，就把对应引脚上的LED点亮。如果是`0`，就把它熄灭。  
Arduino的这个过程是不断的进行的，但由于网络和性能问题，通常会有几秒钟的延迟。

![](https://i.postimg.cc/RVMGwH3s/ulink-1.png)

![](https://i.postimg.cc/QNg0BbMK/ulink-2.png)

## Arduino提交物联网数据
这次首先是Arduino通过一些连接方式，接收到物联网的相关数据，比如温度值”26“，然后向服务器上的一个页面提交请求，请求中包括传感器ID，密码，提交的数据等参数，服务器核实后，就会进入数据库，把传递上来的值，写入相应的传感器记录。  
这个过程也是不断的进行的。


而用户需要这些数据的时候，就可以通过微信发送命令，比如”卧室温度“，后台服务器判断后，就会进入数据库，找到相应的传感器记录，提取温度值，编入预设的反馈消息格式比如“报告主人，卧室温度为26℃。

![](https://i.postimg.cc/4yBJn9M7/ulink-3.png)


鉴于许多朋友可能认为本项目难以上手，所以详细的记录了一遍部属的过程

## 实例

首先是服务器和微信端，  
服务器端选择新浪云，毕竟这是不买VPS的一种比较好的方案，如果有自己的服务器，那么看了代码就懂了，也就不用看服务器端的部署了:）。  
[http://sae.sina.com.cn/](http://sae.sina.com.cn/)，注册并登录，应该有免费云豆吧。  

在管理页面选择创建新应用。

![](https://i.postimg.cc/VsRsKx6f/ulink-4.png)

数据可以这么填，有些空会影响后面的过程

![](https://i.postimg.cc/rFwV3f0S/ulink-5.png)

进入该应用的管理页面，并选择左边的代码管理

![](https://i.postimg.cc/x8x29SJL/ulink-6.png)

编辑代码

![](https://i.postimg.cc/XYZV7Xgq/ulink-7.png)

编辑器

![](https://i.postimg.cc/t4L91yr5/ulink-8.png)

添加文件

![](https://i.postimg.cc/RVpSBwK2/ulink-9.png)

粘贴下面的代码,注意修改代码，有些值后面会看到

```php
<?php  if ($_GET['data'] && ($_GET['token'] == "doubleq")) {//可以改token,这相当于密码，在Arduino端改成相应的值即可
        $con = mysql_connect(SAE_MYSQL_HOST_M.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS); 
        $data = $_GET['data'];
        mysql_select_db("app_ulink42", $con);//要改成相应的数据库名
 
        $result = mysql_query("SELECT * FROM switch");
        while($arr = mysql_fetch_array($result)){//找到需要的数据的记录，并读出状态值
                if ($arr['ID'] == 1) {
                        $state = $arr['state'];
                }
        }
        $dati = date("h:i:sa");//获取时间
        $sql ="UPDATE sensor SET timestamp='$dati',data = '$data'
        WHERE ID = '1'";//更新相应的传感器的值
        if(!mysql_query($sql,$con)){
            die('Error: ' . mysql_error());//如果出错，显示错误
        }
        mysql_close($con);
        echo "{".$state."}";//返回状态值，加“{”是为了帮助Arduino确定数据的位置
}else{
        echo "Permission Denied";//请求中没有type或data或token或token错误时，显示Permission Denied
}
?>
```

同样地，把`index.php`改掉

```php
<?php
 
//错误日志
function echo_server_log($log){
        file_put_contents("log.txt", $log, FILE_APPEND);
}
 
//定义TOKEN
define ( "TOKEN", "ulink" );
 
//验证微信公众平台签名
function checkSignature() {
        $signature = $_GET ['signature'];
        $nonce = $_GET ['nonce'];
        $timestamp = $_GET ['timestamp'];
        $tmpArr = array ($nonce, $timestamp, TOKEN );
        sort ( $tmpArr );
         
        $tmpStr = implode ( $tmpArr );
        $tmpStr = sha1 ( $tmpStr );
        if ($tmpStr == $signature) {
                return true;
        }else{
                return false;
        }
}
if(false == checkSignature()) {
        exit(0);
}
 
//接入时验证接口
$echostr = $_GET ['echostr'];
if($echostr) {
        echo $echostr;
        exit(0);
}
 
//获取POST数据
function getPostData() {
        $data = $GLOBALS['HTTP_RAW_POST_DATA'];
        return        $data;
}
$PostData = getPostData();
 
//验错
if(!$PostData){
        echo_server_log("wrong input! PostData is NULL");
        echo "wrong input!";
        exit(0);
}
 
//装入XML
$xmlObj = simplexml_load_string($PostData, 'SimpleXMLElement', LIBXML_NOCDATA);
 
//验错
if(!$xmlObj) {
        echo_server_log("wrong input! xmlObj is NULL\n");
        echo "wrong input!";
        exit(0);
}
 
//准备XML
$fromUserName = $xmlObj->FromUserName;
$toUserName = $xmlObj->ToUserName;
$msgType = $xmlObj->MsgType;
 
 
if($msgType == 'voice') {//判断是否为语音
        $content = $xmlObj->Recognition;
}elseif($msgType == 'text'){
        $content = $xmlObj->Content;
}else{
        $retMsg = '只支持文本和语音消息';
}
 
if (strstr($content, "温度")) {
        $con = mysql_connect(SAE_MYSQL_HOST_M.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS); 
        mysql_select_db("app_ulink42", $con);//修改数据库名
 
        $result = mysql_query("SELECT * FROM sensor");
        while($arr = mysql_fetch_array($result)){
          if ($arr['ID'] == 1) {
                  $tempr = $arr['data'];
          }
        }
        mysql_close($con);
 
    $retMsg = "报告大王："."\n"."主人房间的室温为".$tempr."℃，感谢您对主人的关心";
}else if (strstr($content, "开灯")) {
        $con = mysql_connect(SAE_MYSQL_HOST_M.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS); 
 
 
        $dati = date("h:i:sa");
        mysql_select_db("app_ulink42", $con);//修改数据库名
 
        $sql ="UPDATE switch SET timestamp='$dati',state = '1'
        WHERE ID = '1'";//修改开关状态值
 
        if(!mysql_query($sql,$con)){
            die('Error: ' . mysql_error());
        }else{
                mysql_close($con);
                $retMsg = "好的主人";
        }
}else if (strstr($content, "关灯")) {
        $con = mysql_connect(SAE_MYSQL_HOST_M.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS); 
 
 
        $dati = date("h:i:sa");
        mysql_select_db("app_ulink42", $con);//修改数据库名
 
        $sql ="UPDATE switch SET timestamp='$dati',state = '0'
        WHERE ID = '1'";//修改开关状态值
 
        if(!mysql_query($sql,$con)){
            die('Error: ' . mysql_error());
        }else{
                mysql_close($con);
                $retMsg = "好的主人";
        }        
}else{
        $retMsg = "暂时不支持该命令";
}
 
//装备XML
$retTmp = "<xml>
                <ToUserName><![CDATA[%s]]></ToUserName>
                <FromUserName><![CDATA[%s]]></FromUserName>
                <CreateTime>%s</CreateTime>
                <MsgType><![CDATA[text]]></MsgType>
                <Content><![CDATA[%s]]></Content>
                <FuncFlag>0</FuncFlag>
                </xml>";
$resultStr = sprintf($retTmp, $fromUserName, $toUserName, time(), $retMsg);
 
//反馈到微信服务器
echo $resultStr;
?>
```

回到应用管理页面，选择左边的MySQL，单击初始化

![](https://i.postimg.cc/GpGdGNV1/ulink-10.png)

管理MySQL

![](https://i.postimg.cc/3Rh7B1Q0/ulink-11.png)

新建数据表

![](https://i.postimg.cc/26xYXWCc/ulink-12.png)

参数如下

![](https://i.postimg.cc/3JqT6YHT/ulink-13.png)

同样的方法，建立一个名字为`sensor`字段数为3的数据表  
参数如下

![](https://i.postimg.cc/8PcGts94/ulink-14.png)

分别插入一条记录

![](https://i.postimg.cc/QN0sQc4V/ulink-15.png)
![](https://i.postimg.cc/RZnBxyMn/ulink-16.png)

然后进入[http://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index](http://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index)，申请测试号，  
填写相应配置

![](https://i.postimg.cc/fbnhzCLq/ulink-17.png)

这时候可以用手机端进行测试了，可以发送包含”温度“，”关灯“，”开灯“的文字消息，或者语音消息，都会有相应的反馈。

还有Arduino端在下面的代码中，我把一个LED接在D7上，用于显示控制的效果，一个DS18B20接在D2上，用来显示上传传感器数据的效果。

```cpp
#include <OneWire.h>
#include <DallasTemperature.h>
#include <SPI.h>
#include <Ethernet.h>
 
 
char state = '0';
char c;
byte mac[] = { 
  0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED};
IPAddress ip(192,168,0,177);
 
IPAddress myDns(192,168,0,1);
 
EthernetClient client;
 
char server[] = "1.ulink42.sinaapp.com";
int sensrdata = 0;
 
unsigned long lastConnectionTime = 0;          
boolean lastConnected = false;                 
const unsigned long postingInterval = 200*1000;  
  
// 定义DS18B20数据口连接arduino的2号IO上
#define ONE_WIRE_BUS 2
  
// 初始连接在单总线上的单总线设备
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);
  
void setup(){
  // 设置串口通信波特率
  Serial.begin(9600);
  delay(1000);
  Ethernet.begin(mac, ip, myDns);
  Serial.print("My IP address: ");
  Serial.println(Ethernet.localIP());
  pinMode(7, OUTPUT);   
  // 初始库
  sensors.begin();
}
  
void loop(void){ 
  sensors.requestTemperatures();
  sensrdata = sensors.getTempCByIndex(0); 
 
  if(state == '0'){
    digitalWrite(7, LOW);      
  }else if(state == '1'){
    digitalWrite(7, HIGH);
  }
 
  while(client.available()) {
    c = client.read();
    if (c == '{'){
      state = client.read();
    }
  }
 
  if (!client.connected() && lastConnected) {
    Serial.println("disconnecting.");
    client.stop();
  }
 
  if(!client.connected() && (millis() - lastConnectionTime > postingInterval)) {
    if (client.connect(server, 80)) {
 
      // send the HTTP PUT request:
      client.print("GET /downup.php?token=doubleq&data=");
      client.print(sensrdata);
      client.println(" HTTP/1.1");
      client.println("Host: 1.ulink42.sinaapp.com");
      client.println("User-Agent: arduino-ethernet");
      client.println("Connection: close");
      client.println();
 
      lastConnectionTime = millis();
    }else {
      Serial.println("connection failed");
      Serial.println("disconnecting.");
      client.stop();
    }
  }
  lastConnected = client.connected();
}
```

## 番外篇

由于这是一个物联网平台，所以只要是可以发起HTTP请求的，都可以接入。
一个比较合理的方案是，树莓派或PCduino做连接互联网的主要控制器，通过XBee,433Mhz等无线方式来控制小型的Arduino节点，再由Arduino来控制物联网终端，当然，如果你不介意网络扩展板的价格，每个Arduino直接联网也是可以的。

## 树莓派
使用Python，requests库，用更强的性能，可以实现更加更快速的反应。

## PCduino
暑假参加了一个黑客马拉松，用了PCduino来实现微信的控制，因为PCduino上直接跑了一个Ubuntu，队友用Python直接写了一个WebSocket类型的连接方式，我目前还没有学会这个技术（是没时间呀TAT），使用WebSocket的话，资源消耗会小很多，而响应速度会快很多，失败几率也会大幅下降，但是Arduino应该不能使用WebSocket吧？？

![](https://i.postimg.cc/cC5VVM2m/ulink-18.png)

![](https://i.postimg.cc/25QNVWLk/ulink-19.jpg)

