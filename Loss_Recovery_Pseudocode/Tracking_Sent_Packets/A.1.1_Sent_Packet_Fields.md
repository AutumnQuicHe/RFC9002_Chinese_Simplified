---
title: "A.1.1. 已发送的数据包的追踪字段"
anchor: "A.1.1_Sent_Packet_Fields"
weight: 10011
rank: "h3"
---

数据包号（`packet_number`）：

:   已发送数据包的数据包号。

是否触发ACK（`ack_eliciting`）：

:   一个表明该数据包是否触发ACK的布尔值。若为真值，则应该接收到确认，不过对端可以推迟发送包含该确认的ACK帧，但不会晚于`max_ack_delay`。

是否计入在途字节数（`in_flight`）：

:   一个表明该数据包是否会被计入在途字节数的布尔值。

发送字节数（`sent_bytes`）：

:   在数据包中发送的字节数，不包含UDP或IP的头部，但包含QUIC的头部。

发送时间（`time_sent`）：

:   该数据包被发送时的时间。
