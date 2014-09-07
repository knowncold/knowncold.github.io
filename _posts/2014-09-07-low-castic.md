---
layout: default
title: 如何用一个下午（3小时）完成三个人合作的浙江省二等奖项目
---
原作是一个帽式超声波身高测量仪，找到的相关信息如下。

![报道中的照片](http://ww1.sinaimg.cn/mw690/c7749b66gw1ek4059di9dj208c05tdgc.jpg)

> 装了两块传感器 戴上就能量身高
> 
> 镇海龙赛中学高二学生邓扬、徐亦柯和穆泓凯发明了一个能量身高的帽子：在帽子上装平衡传感器、超声波传感器，再加上显示感、蓝牙就可以了。其工作原理是：戴上帽子，平衡传感器感觉到你在平地上站稳了，超声波传感器向地面发射电磁波，然后再接收由地面反射的电磁波。
> 
> “得出来的数据除以２，再加上帽子上高出的一小短段距离，就是戴帽子人的身高了，身高数据就显示在帽檐前的显示屏上，通过蓝牙还能显示在手机与电脑上，方便数据采集。”邓扬说，“我们所有零件都来自淘宝网，一共才２００元钱。”
> 
> 现场有位摄影师兴致勃勃地把帽子身上，仪器显示他的身高是１７４厘米。摄影师说：“挺准的，我的净身高是１７３厘米。”
> 
> [报道](http://news.cnnb.com.cn/system/2013/11/24/007912485.shtml)


拿了宁波市一等奖，浙江省二等奖，虽然是三个人合作的。  
碰巧过几天社团招新，把这个作为例子来介绍Arduino吧:)。
###需要的零件###
* 两块Arduino板
* LCD1602
* 10K电位器
* 超声波传感器
* 433Mhz无线传输模块


###使用一块UNO###
![超声波传感器](http://ww1.sinaimg.cn/mw690/c7749b66jw9ek41nkgamjj21kw0w0wuo.jpg)

主角是超声波传感器，所以先去下了超声波库，在GoogleCode上，如果下不了，可以去[这里](http://pan.baidu.com/s/1eQ9ZYSE)。

实验一下库里的例程，就基本会用了。
	#include <NewPing.h>
	
	#define TRIGGER_PIN  12  
	#define ECHO_PIN     11  
	
	NewPing sonar(TRIGGER_PIN, ECHO_PIN); //设置相关的引脚定义
	
	void setup() {
	  Serial.begin(9600); //设置串口
	}
	
	void loop() {
	  delay(50);//每50ms发射接收一次
	  unsigned int uS = sonar.ping();//接收数据
	  Serial.print("Ping: ");
	  Serial.print(uS/100);
	  Serial.println("cm");
	}
效果是这样的。

![超声波效果](http://ww1.sinaimg.cn/mw690/c7749b66gw1ek41h6tohoj21kw0w0n61.jpg)

接着是LCD1602的实验，在例程Hello World上改的。

	#include <LiquidCrystal.h>
	LiquidCrystal lcd(8, 9, 5, 4, 3, 2);//定义引脚
	int he = 213;测试数字
	void setup() {
	  lcd.begin(16, 2);//开始
	  lcd.print("The Height is :");//在第一行打印
	}
	
	void loop() {
	  lcd.setCursor(9, 1);//设置“打印点”
	  lcd.print(he);//打印数字
	  lcd.setCursor(12, 1);
	  lcd.print("cm");
	}

效果是这样的。
![LCD效果](http://ww4.sinaimg.cn/mw690/c7749b66jw9ek41p5c1a4j20y20j67a2.jpg)

把两个合起来，程序变成这样了，其中要注意打印数位不同的数字时，都是从左开始打印的。

	#include <NewPing.h>//超声波定义
	#define TRIGGER_PIN  12  
	#define ECHO_PIN     11  
	NewPing sonar(TRIGGER_PIN, ECHO_PIN); 
	
	#include <LiquidCrystal.h>//LCD定义
	LiquidCrystal lcd(8, 9, 5, 4, 3, 2);
	
	void setup() {//预先打印一些不变的文字
	  	lcd.begin(16, 2);
	  	lcd.print("The Height is :");
	  	lcd.setCursor(12, 1);
	  	lcd.print("cm");
	}
	
	void loop() {
	  	lcd.setCursor(9, 1);
		lcd.print("   ");
		unsigned int hait = sonar.ping()/100;//获取距离数据
		if(hait>99){//三位数的情况
		    lcd.setCursor(9, 1);
				lcd.print(hait);
		}else if(hait<100 && hait>9){//两位数的情况
		 	  lcd.setCursor(10, 1);
				lcd.print(hait);
		}else if(hait<10){//一位数的情况
		    lcd.setCursor(11, 1);
				lcd.print(hait);
		}
		delay(100);防止测量不稳定时，数据更新过快
	}

一块板上的版本基本实现。

![版本一](http://ww4.sinaimg.cn/mw690/c7749b66jw9ek41ljmd2vj21kw0w04k9.jpg)

###添加无线传输###
考虑到超声波模块放到帽子上，显示屏当然可以应该在手里看，因此
使用433Mhz的无线传输模块。  
使用[RCSwitch库](http://pan.baidu.com/s/1pJ2qhdh)，之前使用过，例程：

	//发射
	#include <RCSwitch.h>
	
	RCSwitch mySwitch = RCSwitch();
	int i = 0;
	void setup(){
	   mySwitch.enableTransmit(12);
	}
	void loop(){
	  i = i+1;
	  mySwitch.send(i, 24);
	  delay(1000);
	}
</hr>

	//接收
	#include <RCSwitch.h>
	RCSwitch mySwitch = RCSwitch();
	
	void setup(){
	  mySwitch.enableReceive(0);
	  Serial.begin(9600); 
	}
	void loop(){
	  if (mySwitch.available()) {
	    int value = mySwitch.getReceivedValue();
	    if (value != 0) {
	      Serial.println(value);
	    }
	    mySwitch.resetAvailable();
	  }
	}

接收端使用UNO，发射端使用Leonardo。  
程序如下：

	//接收
	#include <LiquidCrystal.h>
	LiquidCrystal lcd(8, 9, 5, 4, 3, 10);
	
	#include <RCSwitch.h>
	RCSwitch mySwitch = RCSwitch();
	
	void setup() {
	  mySwitch.enableReceive(0);
	  lcd.begin(16, 2);
	  lcd.print("The Height is :");
	  lcd.setCursor(12, 1);
	  lcd.print("cm");
	}
	
	void loop() {
	  if (mySwitch.available()) {
	    int hait = mySwitch.getReceivedValue();
	    if (hait != 0) {
			lcd.setCursor(9, 1);
			lcd.print("   ");
			if(hait>99){
			    	lcd.setCursor(9, 1);
					lcd.print(hait);
			}else if(hait<100 && hait>9){
			 	 	 lcd.setCursor(10, 1);
					lcd.print(hait);
			}else if(hait<10){
			   		lcd.setCursor(11, 1);
					lcd.print(hait);
			}
			delay(100);
	    }
	    mySwitch.resetAvailable();
	  }  
	}
</hr>
	
	//发射
	#include <NewPing.h>
	#define TRIGGER_PIN  12  
	#define ECHO_PIN     11  
	NewPing sonar(TRIGGER_PIN, ECHO_PIN); //设置相关的引脚定义
	
	#include <RCSwitch.h>
	RCSwitch mySwitch = RCSwitch();
	
	
	void setup(){
	   mySwitch.enableTransmit(12);
	}
	void loop(){
	  delay(50);//每50ms发射接收一次
	  unsigned int hait = sonar.ping();//接收数据
	  mySwitch.send(hait, 24);
	
	}

###使用蓝牙###
使用蓝牙，可以把数据发送到配对的手机或电脑上，这里使用了OpenJumper的蓝牙模块，使用很简单，和串口几乎一样。

	#include <NewPing.h>

	#define TRIGGER_PIN  12  
	#define ECHO_PIN     11  

	NewPing sonar(TRIGGER_PIN, ECHO_PIN); //设置相关的引脚定义

	void setup() {
	  Serial.begin(9600); //设置串口
	}

	void loop() {
	  delay(50);//每50ms发射接收一次
	  unsigned int hait = sonar.ping()/100;//接收数据
	  Serial.print(hait);
	}

效果如下：

![蓝牙效果](http://ww3.sinaimg.cn/mw690/c7749b66gw1ek45u64sgjj20k00zk76v.jpg)



> 其实可以直接通过APP转发到服务器，体育老师都不用手写了，直接上传到电子档案的数据库。

</br>
至此，我花了两小时实验，一小时写这篇文章。
