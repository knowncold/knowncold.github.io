---
title: WiDo—Arduino+WiFi
layout: page
category: tutorial
---
> 首先必须要感谢DFRobot给我的试用机会！

在Wido之前我用过W5100的以太网模块，相比较而言，Wido就是相当便携，非常方便，而且对于Wifi信号的接受能力是比较强的。

> 拿到板子之后，断断续续玩了几次，中途上学过了两周，玩的差不多了，正好可以来写教程。

目前网上比较好的教程也就官方的[WIKI](http://wiki.dfrobot.com.cn/index.php?title=(SKU:DFR0321)Wido_WIFI%E7%89%A9%E8%81%94%E7%BD%91%E8%8A%82%E7%82%B9%E6%8E%A7%E5%88%B6%E5%99%A8_%E5%85%BC%E5%AE%B9Arduino_%E5%86%85%E6%B5%8B%E7%89%88)，参数什么的先不看了，首先作为一块wifi板子，当然要连上wifi，下载好[库](https://github.com/Lauren-ED209/Adafruit_CC3000_Library/archive/master.zip)，先用官方的例程。

第一个是`buildtest.ino`，用来初步测试。
注意下载时要选择Leonardo板卡。

打开串口监视器就会这样的信息。

![](http://o73wiy9vn.bkt.clouddn.com/wido-guide-1.PNG)

Wido会在串口调试中打印出CC3000固件的版本，Wido模块的mac地址以及附近路由器的数量，SSID，信号强度以及加密类型。

比如我的就是版本1.28，MAC地址0x00 0x19 0x94 0x47 0xE3 0x7B，找到的两个wifi信号是

    SSID Name    : Tenda_084FB8
    RSSI         : 35
    Security Mode: 2

    SSID Name    : TP-LINK_33701E
    RSSI         : 35
    Security Mode: 3

加密模式中数字0到3分别表示 `WLAN_SEC_UNSEC`, `WLAN_SEC_WEP`, `WLAN_SEC_WPA` or `WLAN_SEC_WPA2`（这点可以在代码的注释中看到）

这个例程也可以用来查看四周的wifi信号。

看完有哪些信号，当然就要连上他们了，在代码中找到这两句

```cpp
#define WLAN_SSID       "myNetwork"        // cannot be longer than 32 characters!
#define WLAN_PASS       "myPassword"       //
```

就把引号的里面内容改成自己的Wifi内容，比如

```cpp
#define WLAN_SSID       "TP-LINK_33701E"        // cannot be longer than 32 characters!
#define WLAN_PASS       "19890226"     //
```

再次下载到板子里去，打开串口监视器，就会这样类似的信息。连上了路由器，而且还ping了别的网站。

![](http://o73wiy9vn.bkt.clouddn.com/wido-guide-2.PNG)

同时进入路由器的页面，也可以看到有关的WIDO信息

![](http://o73wiy9vn.bkt.clouddn.com/wido-guide-3.PNG)

我又主动Ping了一下WIDO，出现这个样子，我也不清楚= =

![](http://o73wiy9vn.bkt.clouddn.com/wido-guide-4.PNG)

这个实例代码有点烦，而且现在对我没什么用，那就下次再看吧= =

官方WIKI中的实例二有点好玩，但不怎么有用，下次有空了再说= =

wiki中的案例三，就是一个比较有用的应用了，可以让wido连上外网的一台服务器，并发送数据，照着原文的方法，很容易就可以实现相似的结果。
为了折腾，当然要连上自己的服务器，既然要连主机，你就先连一下同一个局域网的笔电。

那么首先你要连接的电脑要有服务器软件，我用了Apache加PHP（直接装AppServ），最简单的当然是在主机上建一个文本，让wido去访问。

比如我在`D:\AppServ\www\twdio`中建了一个`hi.txt`的文件，内容是

```
Hi,Wido!
Im PC,nice to meet you！
```

创建完成后可以用别的PC或手机试试

这时候再来分析实例三的代码（比刚才的那段短多了= =），整个代码的逻辑比较明显。
首先设定参数来初始化wido，然后连上路由器，接着再想办法连上目标服务器。

```cpp
#include <Adafruit_CC3000.h>
#include <ccspi.h>
#include <SPI.h>
#include <string.h>
#include "utility/debug.h"

#define WiDo_IRQ   7
#define WiDo_VBAT  5
#define WiDo_CS    10

Adafruit_CC3000 WiDo = Adafruit_CC3000(WiDo_CS, WiDo_IRQ, WiDo_VBAT,
SPI_CLOCK_DIVIDER);

#define WLAN_SSID       "TP-LINK_33701E"      
#define WLAN_PASS       "19890226"    
#define WLAN_SECURITY   WLAN_SEC_WPA2

//以上都是设置参数初始化

#define TOKEN           "7dafd269e271c9bcd0b69f61c3ff6af4"  //这是用来连接DFRobot的服务器时的凭证，在其他项目中当然不需要，所以删掉
```

看setup

```cpp
void setup(){

  Serial.begin(115200);//打开串口
  Serial.println(F("Hello, Wido!\n")); //先来一句hello

  Serial.println(F("\nInitialising the CC3000 ..."));
  if (!WiDo.begin())//开始初始化，初始化失败的话，就输出下面一句话，并进入死循环
  {
    Serial.println(F("Unable to initialise the CC3000! Check your wiring?"));
    while(1);
  }

  if (!WiDo.connectToAP(WLAN_SSID,WLAN_PASS,WLAN_SECURITY)) {//尝试连接路由器，如果失败，当然也输出一句话，进入死循环
    Serial.println(F("Failed!"));
    while(1);
  }

  Serial.println(F("Connected!"));//上面没有出错的话，就会出现这一句了，已经连上路由器了


  Serial.println(F("Request DHCP"));//等待分配ip地址
  while (!WiDo.checkDHCP())
  {
    delay(100); // ToDo: Insert a DHCP timeout!这好像是公司还没开发完= = ，那就先跳过
  }  
}
```

然后就进入尝试连接服务器的阶段了

```cpp
void loop(){
  static Adafruit_CC3000_Client IoTclient;//给他取个名字

  if(IoTclient.connected()){    //如果连上服务器了

    int sensValue = analogRead(0) *0.0048828125 * 100;//收集传感器数值
/*-------------------------------------------------------------------------------    
    这一段试图和服务器进行一次HTTP连接，但这个连接不是一般性普通的HTTP连接
    char clientString[50];
    sprintf(clientString,"%s%s%s%d%s","GET /data?token=",TOKEN,"¶m=",sensValue," HTTP/1.1");
    Serial.println(clientString);


    IoTclient.fastrprintln(clientString);

    IoTclient.fastrprint(F("\r\n"));
    IoTclient.fastrprint(F("\r\n"));

    Serial.println();

//-------------------------------------------------------------------------------*/
    unsigned long lastRead = millis();//标记连上的时间
    while (IoTclient.connected() && (millis() - lastRead < TIMEOUT_MS)) {//如果现在还连着，而且时间还没到，这里时间限制就是TIMEOUT_MS，如果设置太短会导致连接未完毕就断开，反之则会导致数据更新过慢
      while (IoTclient.available()) {//正在连接中的话
        char c = IoTclient.read();//读取服务器返回的HTTP信息，并通过串口打印出来
        Serial.print(c);
        lastRead = millis();//如果中途服务器端或中间的连接出现问题，就要靠这个时间点来计时了
      }
    }
    IoTclient.close();//时间到了之后，就要自己关闭连接了
  }
  else{//如果还没连上，那就连吧
    uint32_t ip = WiDo.IP2U32(182,254,130,180);//服务器的IP地址
    IoTclient = WiDo.connectTCP(ip,8124);//通过8124端口连接，一般而言，大多数普通的HTTP连接都是80端口的
    Serial.println("Connecting IoT Server...");//串口输出，表示正在努力连接
  }  
  delay(5000);
}
```

分析完了之后，发现一个难点，就是创建一个HTTP连接，一般的GET连接就行，半个月前绞尽脑汁= =，后来发现仔细去看HTTP协议和W5100的例程就可以知道怎么做了。

GET的格式应该是这样的

```cpp
client.fastrprintln("GET /index.html HTTP/1.1");
client.fastrprintln("Host: example.net");
client.fastrprintln("User-Agent: wido");
client.fastrprintln("Connection: close");
client.fastrprint(F("\r\n"));
client.fastrprint(F("\r\n"));
```

那么就有了一个测试例程。

```cpp
#include <Adafruit_CC3000.h>
#include <ccspi.h>
#include <SPI.h>
#include <string.h>
#include "utility/debug.h"

#define WiDo_IRQ   7
#define WiDo_VBAT  5
#define WiDo_CS    10

Adafruit_CC3000 WiDo = Adafruit_CC3000(WiDo_CS, WiDo_IRQ, WiDo_VBAT,
SPI_CLOCK_DIVIDER);

#define WLAN_SSID       "TP-LINK_33701E"  
#define WLAN_PASS       "19890226"
#define WLAN_SECURITY   WLAN_SEC_WPA2


#define TIMEOUT_MS      2000

void setup(){

  Serial.begin(115200);

  Serial.println(F("Hello, Wido!\n"));
  Serial.println(F("\nInitialising the CC3000 ..."));
  if (!WiDo.begin())
  {
    Serial.println(F("Unable to initialise the CC3000! Check your wiring?"));
    while(1);
  }

  if (!WiDo.connectToAP(WLAN_SSID,WLAN_PASS,WLAN_SECURITY)) {
    Serial.println(F("Failed!"));
    while(1);
  }

  Serial.println(F("Connected!"));
  Serial.println(F("Request DHCP"));
  while (!WiDo.checkDHCP())
  {
    delay(100);
  }  
}


void loop(){
  static Adafruit_CC3000_Client client;

  if(client.connected()){   

    client.fastrprintln("GET /twido/hi.txt HTTP/1.1");
    client.fastrprintln("Host: 192.168.1.102");
    client.fastrprintln("User-Agent: arduino-ethernet");
    client.fastrprintln("Connection: close");
    client.fastrprint(F("\r\n"));
    client.fastrprint(F("\r\n"));

    Serial.println();

    unsigned long lastRead = millis();
    while (client.connected() && (millis() - lastRead < TIMEOUT_MS)) {
      while (client.available()) {
        char c = client.read();
        Serial.print(c);
        lastRead = millis();
      }
    }
    client.close();
  }else{
    uint32_t ip = WiDo.IP2U32(192,168,1,102);
    client = WiDo.connectTCP(ip,80);
    Serial.println("Connecting IoT Server...");
  }  
  delay(5000);
}
```

结果当然是可以的，

![](http://o73wiy9vn.bkt.clouddn.com/wido-guide-5.PNG)

用这种方法可以连接大多数服务器，当然也可以实现Ulink。
如果觉得还不错，那就赶快去搞一块Wido喽！
