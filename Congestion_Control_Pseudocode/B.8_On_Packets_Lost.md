---
title: "B.8. 在丢包时"
anchor: "B.8_On_Packets_Lost"
weight: 11080
rank: "h2"
---

This is invoked when DetectAndRemoveLostPackets deems packets lost.

该过程会在`DetectAndRemoveLostPackets`将数据包认定为丢包时被调用。

{{% block_ref
indx="Pseudocode_11_8_1" %}}

```
OnPacketsLost(lost_packets):
  sent_time_of_last_loss = 0
  // 从`bytes_in_flight`中移除遭遇丢包的数据包。
  for lost_packet in lost_packets:
    if lost_packet.in_flight:
      bytes_in_flight -= lost_packet.sent_bytes
      sent_time_of_last_loss =
        max(sent_time_of_last_loss, lost_packet.time_sent)
  // 如果在途数据包遭遇丢包，那么触发拥塞事件。
  if (sent_time_of_last_loss != 0):
    OnCongestionEvent(sent_time_of_last_loss)

  // 如果这些数据包的丢包表明了持续拥塞，
  // 那么重置拥塞窗口。
  // 只考虑在取得首份RTT样本后发送的数据包。
  if (first_rtt_sample == 0):
    return
  pc_lost = []
  for lost in lost_packets:
    if lost.time_sent > first_rtt_sample:
      pc_lost.insert(lost)
  if (InPersistentCongestion(pc_lost)):
    congestion_window = kMinimumWindow
    congestion_recovery_start_time = 0
```

{{% /block_ref %}}
