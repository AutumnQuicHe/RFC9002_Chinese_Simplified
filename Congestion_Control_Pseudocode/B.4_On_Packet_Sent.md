---
title: "B.4. 在发送数据包时"
anchor: "B.4_On_Packet_Sent"
weight: 11040
rank: "h2"
---

Whenever a packet is sent and it contains non-ACK frames, the packet increases bytes_in_flight.

只要被发送的数据包中包含非**ACK帧**，该数据包就会使得`bytes_in_flight`增长。

{{% block_ref
indx="Pseudocode_11_4_1" %}}

```
OnPacketSentCC(sent_bytes):
  bytes_in_flight += sent_bytes
```

{{% /block_ref %}}
