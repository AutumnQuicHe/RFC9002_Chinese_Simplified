---
title: "A.11. 在启用初始密钥或握手密钥时"
anchor: "A.11_Upon_Dropping_Initial_or_Handshake_Keys"
weight: 10110
rank: "h2"
---

当弃用初始密钥或握手密钥时，位于这些空间中的数据包会被丢弃，且丢包检测状态会被更新。

`OnPacketNumberSpaceDiscarded`的伪代码如下：

{{% block_ref
indx="Pseudocode_10_11_1" %}}

```
OnPacketNumberSpaceDiscarded(pn_space):
  assert(pn_space != ApplicationData)
  RemoveFromBytesInFlight(sent_packets[pn_space])
  sent_packets[pn_space].clear()
  // 重置丢包检测计时器和PTO计时器。
  time_of_last_ack_eliciting_packet[pn_space] = 0
  loss_time[pn_space] = 0
  pto_count = 0
  SetLossDetectionTimer()
```

{{% /block_ref %}}
