---
title: olsrd Packet
layout: page
category: olsrd
---

## 分类

分为`Hello Message`，`MID Message`，`TC Message`，`HNA Message`。

## Packet

使用UDP协议和698端口。

### Packet格式

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Packet Length         |    Packet Sequence Number     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Message Type |     Vtime     |         Message Size          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Originator Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time To Live |   Hop Count   |    Message Sequence Number    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
:                            MESSAGE                            :
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Message Type |     Vtime     |         Message Size          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Originator Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time To Live |   Hop Count   |    Message Sequence Number    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
:                            MESSAGE                            :
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
:                                                               :
        (etc.)
```

### Packet Header

#### Packet Length

packet的字节数

#### Packet Sequence Number

每个新的packet发送PSN就加一

### Message Header

#### Message Type

范围在0-127

```
HELLO_MESSAGE   =   1
TC_MESSAGE      =   2
MID_MESSAGE     =   3
HNA_MESSAGE     =   4
```

#### Vtime

从接受到这个Packet之后的有效时间

$$
validity \ time = C * ( 1 + \frac{a}{16}) * 2^{b}
$$

$a$和$b$是`Vtime`的高低4位。

$$
C = \frac{1}{16} seconds
$$

#### Message Size

一个Message段的大小字节数

#### Originator Address

在传输中不可更改，是Message的生成者的Address，要和UDP头部的Address区分开来。

#### Time To Live

一个Message传播的最大跳数，这个Message被再次传播前，TTL必须先减一；如果接收到TTL是0或者1，就不能再传播了，一般而言不会收到0的情况。

用来规定这个flooding的传播半径。

#### Hop Count

Originator初始化为0，后面接收到的节点每次传播前加一。

#### Message Sequence Number

Originator每次产生新的就加一，初始不一定是0，用来确保这个Message不会被任何的节点重复传播两次。

## Generate Packet

> src/generate_msg.c

```c
void
generate_hello(void *p)
{
  struct hello_message hellopacket;
  struct interface *ifn = (struct interface *)p;

  olsr_build_hello_packet(&hellopacket, ifn);

  if (queue_hello(&hellopacket, ifn))
    net_output(ifn);

  olsr_free_hello_packet(&hellopacket);

}

void
generate_tc(void *p)
{
  struct tc_message tcpacket;
  struct interface *ifn = (struct interface *)p;

  olsr_build_tc_packet(&tcpacket);

  if (queue_tc(&tcpacket, ifn) && TIMED_OUT(ifn->fwdtimer)) {
    set_buffer_timer(ifn);
  }

  olsr_free_tc_packet(&tcpacket);
}

void
generate_mid(void *p)
{
  struct interface *ifn = (struct interface *)p;

  if (queue_mid(ifn) && TIMED_OUT(ifn->fwdtimer)) {
    set_buffer_timer(ifn);
  }

}

void
generate_hna(void *p)
{
  struct interface *ifn = (struct interface *)p;

  if (queue_hna(ifn) && TIMED_OUT(ifn->fwdtimer)) {
    set_buffer_timer(ifn);
  }
}

void
generate_stdout_pulse(void *foo __attribute__ ((unused)))
{
  if (olsr_cnf->debug_level == 0)
    return;

  pulse_state = pulse_state == 3 ? 0 : pulse_state + 1;

  printf("%c\r", pulsedata[pulse_state]);

}
```

## Hello Message

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Reserved             |     Htime     |  Willingness  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Link Code   |   Reserved    |       Link Message Size       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                  Neighbor Interface Address                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                  Neighbor Interface Address                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
:                             .  .  .                           :
:                                                               :
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Link Code   |   Reserved    |       Link Message Size       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                  Neighbor Interface Address                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                  Neighbor Interface Address                   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
:                                                               :
:                                       :
(etc.)
```
