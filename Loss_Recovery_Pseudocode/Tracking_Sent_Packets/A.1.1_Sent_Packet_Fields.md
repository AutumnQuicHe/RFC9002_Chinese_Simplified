---
title: "A.1.1. 已发送的数据包的追踪字段"
anchor: "A.1.1_Sent_Packet_Fields"
weight: 10011
rank: "h3"
---

packet_number:
The packet number of the sent packet.

数据包号（`packet_number`）：

:   已发送数据包的数据包号。

ack_eliciting:
A Boolean that indicates whether a packet is ack-eliciting. If true, it is expected that an acknowledgment will be received, though the peer could delay sending the ACK frame containing it by up to the max_ack_delay.

是否触发ACK（`ack_eliciting`）：

:   一个表明该数据包是否触发ACK的布尔值。若为真值，则应该接收到确认，不过对端可以推迟发送包含该确认的ACK帧，但不会晚于`max_ack_delay`。

in_flight:
A Boolean that indicates whether the packet counts toward bytes in flight.

是否计入在途字节数（`in_flight`）：

:   一个表明该数据包是否会被计入在途字节数的布尔值。

sent_bytes:
The number of bytes sent in the packet, not including UDP or IP overhead, but including QUIC framing overhead.

发送字节数（`sent_bytes`）：

:   在数据包中发送的字节数，不包含UDP或IP的头部，但包含QUIC的头部。

time_sent:
The time the packet was sent.

发送时间（`time_sent`）：

:   该数据包被发送时的时间。
