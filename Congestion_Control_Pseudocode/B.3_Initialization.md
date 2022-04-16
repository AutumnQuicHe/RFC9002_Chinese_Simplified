---
title: "B.3. 初始化"
anchor: "B.3_Initialization"
weight: 11030
rank: "h2"
---

At the beginning of the connection, initialize the congestion control variables as follows:

在连接的一开始，以这种方式初始化拥塞控制变量：

{{% block_ref
indx="Pseudocode_11_3_1" %}}

```
congestion_window = kInitialWindow
bytes_in_flight = 0
congestion_recovery_start_time = 0
ssthresh = infinite
for pn_space in [ Initial, Handshake, ApplicationData ]:
  ecn_ce_counters[pn_space] = 0
```

{{% /block_ref %}}
