---
title: "A.4. 初始化"
anchor: "A.4_Initialization"
weight: 10040
rank: "h2"
---

At the beginning of the connection, initialize the loss detection variables as follows:

在连接的一开始，以这种方式初始化丢包检测变量：

{{% block_ref
indx="Pseudocode_10_4_1" %}}

```
loss_detection_timer.reset()
pto_count = 0
latest_rtt = 0
smoothed_rtt = kInitialRtt
rttvar = kInitialRtt / 2
min_rtt = 0
first_rtt_sample = 0
for pn_space in [ Initial, Handshake, ApplicationData ]:
  largest_acked_packet[pn_space] = infinite
  time_of_last_ack_eliciting_packet[pn_space] = 0
  loss_time[pn_space] = 0
```

{{% /block_ref %}}
