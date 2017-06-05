---
title: 分组密码的工作模式
layout: page
category: wiki
---

分组密码的工作模式(Block Cipher Modes)总共有五种:  
 ECB CBC CFB OFB CTR

![](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-1.png)

## 电码本模式 Electronic Codebook

明文分成64位的分组进行加密，不足64位再填充，每个分组都用同一个密钥加密  
即同样的明文分组就会得到相同的密文

![ECB](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-2.png)

### 局限性

主要局限来自于ECB的密文分组是互相独立的  
适合数据量比较少的时候，比如传输DES密钥  
而对于很长的消息尤其是比较结构化的消息，ECB是不安全的，通过结构特征可能会被破解

## 密文分组链接模式 Cipher Block Chaining

为了解决ECB的密文分组相互独立的问题，CBC的加密输入当前的明文分组和上一次密文分组的异或，使用相同的密钥加密  
因此即使两个明文分组相同，一般情况下它们的密文分组也会不同

其中  
$ C_1 = E(K, [IV \oplus P_1]) $  
$ P_1 = IV \oplus D(K, C_1) $

![CBC](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-3.png)

### 优点和局限性

明文消息中有一个变化都会影响（后面）所有的密文分组，有很好的扩散性  
但是发送方和接收方需要共享一个初始向量IV (Initial Value)  

长度不够的分组，可以填充已知非数据值，或者在最后一块补上填充位的长度

> b1 b2 b3 0 0 0 0 5  
> 三个数据字节，还有五字节的填充数据加计数


## 密码反馈模式 Cipher FeedBack

![CFB](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-4.png)

## 输出反馈模式 Output FeedBack

![OFB](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-5.png)

## 计数器模式 Counter

![CTR](http://o73wiy9vn.bkt.clouddn.com/block-cipher-modes-6.png)
