---
title: "B.5. 在数据包得到确认时"
anchor: "B.5_On_Packet_Acknowledgment"
weight: 11050
rank: "h2"
---

该过程会被丢包检测的`OnAckReceived`调用，并且会被传入在`sent_packets`中最新的已确认数据包（`acked_packets`）。

在拥塞回避状态下，为拥塞窗口尺寸使用整型来表达的实现者应该小心的进行除法操作，并且可以使用在《[RFC3465]()》的[第2.1章]()中建议的替代方案。

{{% block_ref
indx="Pseudocode_11_5_1" %}}

```
InCongestionRecovery(sent_time):
  return sent_time <= congestion_recovery_start_time

OnPacketsAcked(acked_packets):
  for acked_packet in acked_packets:
    OnPacketAcked(acked_packet)

OnPacketAcked(acked_packet):
  if (!acked_packet.in_flight):
    return;
  // 从`bytes_in_flight`中移除
  bytes_in_flight -= acked_packet.sent_bytes
  // 如果是受到应用或流量控制的限制，
  // 那么不要扩大拥塞窗口。
  if (IsAppOrFlowControlLimited())
    return
  // 在恢复期不要扩大拥塞窗口。
  if (InCongestionRecovery(acked_packet.time_sent)):
    return
  if (congestion_window < ssthresh):
    // 慢启动。
    congestion_window += acked_packet.sent_bytes
  else:
    // 拥塞回避。
    congestion_window +=
      max_datagram_size * acked_packet.sent_bytes
      / congestion_window
```

{{% /block_ref %}}
