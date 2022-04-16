---
title: "A.7. 在接收到确认时"
anchor: "A.7_On_Receiving_an_Acknowledgment"
weight: 10070
rank: "h2"
---

When an ACK frame is received, it may newly acknowledge any number of packets.

当接收到某**ACK帧**时，它可能新确认任意数量的数据包。

Pseudocode for OnAckReceived and UpdateRtt follow:

`OnAckReceived`和`UpdateRtt`的伪代码如下：

{{% block_ref
indx="Pseudocode_10_7_1" %}}

```
IncludesAckEliciting(packets):
  for packet in packets:
    if (packet.ack_eliciting):
      return true
  return false

OnAckReceived(ack, pn_space):
  if (largest_acked_packet[pn_space] == infinite):
    largest_acked_packet[pn_space] = ack.largest_acked
  else:
    largest_acked_packet[pn_space] =
        max(largest_acked_packet[pn_space], ack.largest_acked)

  // `DetectAndRemoveAckedPackets`找到新确认的数据包
  // 并将它们从`sent_packets`中移除。
  newly_acked_packets =
      DetectAndRemoveAckedPackets(ack, pn_space)
  // 如果没有新确认的数据包，那么什么都不做。
  if (newly_acked_packets.empty()):
    return

  // 如果最大已确认数据包是此次新确认的，
  // 并且此次至少确认一个ACK触发包，那么更新RTT。
  if (newly_acked_packets.largest().packet_number ==
          ack.largest_acked &&
      IncludesAckEliciting(newly_acked_packets)):
    latest_rtt =
      now() - newly_acked_packets.largest().time_sent
    UpdateRtt(ack.ack_delay)

  // 如果存在ECN信息，那么处理它们。
  if (ACK frame contains ECN information):
      ProcessECN(ack, pn_space)

  lost_packets = DetectAndRemoveLostPackets(pn_space)
  if (!lost_packets.empty()):
    OnPacketsLost(lost_packets)
  OnPacketsAcked(newly_acked_packets)

  // 除非客户端不确定服务器是否
  // 已验证完自身地址，否则重置`pto_count`。
  if (PeerCompletedAddressValidation()):
    pto_count = 0
  SetLossDetectionTimer()


UpdateRtt(ack_delay):
  if (first_rtt_sample == 0):
    min_rtt = latest_rtt
    smoothed_rtt = latest_rtt
    rttvar = latest_rtt / 2
    first_rtt_sample = now()
    return

  // `min_rtt`会忽略确认延迟。
  min_rtt = min(min_rtt, latest_rtt)
  // 在握手确认后用`max_ack_delay`来限制`ack_delay`
  if (handshake confirmed):
    ack_delay = min(ack_delay, max_ack_delay)

  // 如果确认延迟是个合理值，
  // 那么使用它调整RTT。
  adjusted_rtt = latest_rtt
  if (latest_rtt >= min_rtt + ack_delay):
    adjusted_rtt = latest_rtt - ack_delay

  rttvar = 3/4 * rttvar + 1/4 * abs(smoothed_rtt - adjusted_rtt)
  smoothed_rtt = 7/8 * smoothed_rtt + 1/8 * adjusted_rtt
```

{{% /block_ref %}}
