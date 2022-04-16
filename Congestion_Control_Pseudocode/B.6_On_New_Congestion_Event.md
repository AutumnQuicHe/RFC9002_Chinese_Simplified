---
title: "B.6. 在响应新的拥塞事件时"
anchor: "B.6_On_New_Congestion_Event"
weight: 11060
rank: "h2"
---

This is invoked from ProcessECN and OnPacketsLost when a new congestion event is detected. If not already in recovery, this starts a recovery period and reduces the slow start threshold and congestion window immediately.

该过程会在检测到新的拥塞事件时被`ProcessECN`和`OnPacketsLost `调用。如果此时并不处于恢复期，那么就会启动恢复期，立即降低慢启动阈值并且缩小拥塞窗口。

{{% block_ref
indx="Pseudocode_11_6_1" %}}

```
OnCongestionEvent(sent_time):
  // 如果已经处于恢复期，那么不进行任何动作。
  if (InCongestionRecovery(sent_time)):
    return

  // 进入恢复期。
  congestion_recovery_start_time = now()
  ssthresh = congestion_window * kLossReductionFactor
  congestion_window = max(ssthresh, kMinimumWindow)
  // 可以发送一个数据包来加速丢包检测。
  MaybeSendOnePacket()
```

{{% /block_ref %}}
