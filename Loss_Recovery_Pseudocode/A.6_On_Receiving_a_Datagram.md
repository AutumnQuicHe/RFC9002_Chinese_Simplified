---
title: "A.6. 在接收到数据报时"
anchor: "A.6_On_Receiving_a_Datagram"
weight: 10060
rank: "h2"
---

When a server is blocked by anti-amplification limits, receiving a datagram unblocks it, even if none of the packets in the datagram are successfully processed. In such a case, the PTO timer will need to be rearmed.

当服务器被抗放大上限阻止发送时，接收到的数据报能够为它解禁，即使该数据报中没有一个数据包成功得到处理。在这种情况下，需要重新设置PTO计时器。

Pseudocode for OnDatagramReceived follows:

`OnDatagramReceived`的伪代码如下：

{{% block_ref
indx="Pseudocode_10_6_1" %}}

```
OnDatagramReceived(datagram):
  // 如果该数据报能为服务器解禁，
  // 那么设置PTO计时器来避免死锁。
  if (服务器被抗放大上限阻止发送):
    SetLossDetectionTimer()
  if loss_detection_timer.timeout < now():
    // 假设抗放大上限仍生效，
    // 如果PTO会超时，那么执行它。
    OnLossDetectionTimeout()
```

{{% /block_ref %}}
