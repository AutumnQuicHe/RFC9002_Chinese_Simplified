---
title: "B.9. 从在途字节数中移除被丢弃的数据包"
anchor: "B.9_Removing_Discarded_Packets_from_Bytes_in_Flight"
weight: 11090
rank: "h2"
---

当初始密钥或握手密钥被弃用时，在这些空间中发送的数据包不再被计入在途字节数中。

`RemoveFromBytesInFlight`的伪代码如下：

{{% block_ref
indx="Pseudocode_11_9_1" %}}

```
RemoveFromBytesInFlight(discarded_packets):
  // 从在途字节数中移除所有未得到确认的数据包。
  foreach packet in discarded_packets:
    if packet.in_flight
      bytes_in_flight -= size
```

{{% /block_ref %}}
