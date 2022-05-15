---
title: "B.7. 处理ECN信息"
anchor: "B.7_Process_ECN_Information"
weight: 11070
rank: "h2"
---

该过程会在从对端接收到具有ECN相关字段的**ACK帧**时被调用。

{{% block_ref
indx="Pseudocode_11_7_1" %}}

```
ProcessECN(ack, pn_space):
  // 如果由对端报告的`ECN-CE`计数增加了，
  // 那么这可能是一次新的拥塞事件。
  if (ack.ce_counter > ecn_ce_counters[pn_space]):
    ecn_ce_counters[pn_space] = ack.ce_counter
    sent_time = sent_packets[ack.largest_acked].time_sent
    OnCongestionEvent(sent_time)
```

{{% /block_ref %}}
