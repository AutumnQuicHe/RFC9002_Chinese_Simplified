---
title: "A.9. 在超时时"
anchor: "A.9_On_Timeout"
weight: 10090
rank: "h2"
---

When the loss detection timer expires, the timer's mode determines the action to be performed.

当丢包检测计时器超时时，计时器的模式决定了需要采取的行动。

Pseudocode for OnLossDetectionTimeout follows:

`OnLossDetectionTimeout`的伪代码如下：

{{% block_ref
indx="Pseudocode_10_9_1" %}}

```
OnLossDetectionTimeout():
  earliest_loss_time, pn_space = GetLossTimeAndSpace()
  if (earliest_loss_time != 0):
    // 基于数据包发送时间阈值的丢包检测法。
    lost_packets = DetectAndRemoveLostPackets(pn_space)
    assert(!lost_packets.empty())
    OnPacketsLost(lost_packets)
    SetLossDetectionTimer()
  return

  if (没有在途的ACK触发包):
    assert(!PeerCompletedAddressValidation())
    // 客户端发送了解死锁数据包：填充了初始数据包来挣得
    // 更多的抗放大额度，握手数据包则证明了对地址的所有权。
    if (有握手密钥):
      SendOneAckElicitingHandshakePacket()
    else:
      SendOneAckElicitingPaddedInitialPacket()
  else:
    // PTO。如果有新数据可用，那就发送，否则重传旧数据。
    // 如果两者均不可用，那就发送一个Ping帧。
    _, pn_space = GetPtoTimeAndSpace()
    SendOneOrTwoAckElicitingPackets(pn_space)

  pto_count++
  SetLossDetectionTimer()
```

{{% /block_ref %}}
