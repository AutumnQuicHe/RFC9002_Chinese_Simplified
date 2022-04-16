---
title: "A.10. 检测丢包"
anchor: "A.10_Detecting_Lost_Packets"
weight: 10100
rank: "h2"
---

DetectAndRemoveLostPackets is called every time an ACK is received or the time threshold loss detection timer expires. This function operates on the sent_packets for that packet number space and returns a list of packets newly detected as lost.

每次接收到**ACK帧**或时间阈值丢包检测计时器超时时，都会调用`DetectAndRemoveLostPackets`。该函数对响应数据包号空间中的已发送数据包（`sent_packets`）进行操作，并返回一份最新被认定为丢包的数据包的列表。

Pseudocode for DetectAndRemoveLostPackets follows:

`DetectAndRemoveLostPackets`的伪代码如下：

{{% block_ref
indx="Pseudocode_10_10_1" %}}

```
DetectAndRemoveLostPackets(pn_space):
  assert(largest_acked_packet[pn_space] != infinite)
  loss_time[pn_space] = 0
  lost_packets = []
  loss_delay = kTimeThreshold * max(latest_rtt, smoothed_rtt)

  // 在数据包被认定为丢失前经过的最少时间，但不小于`kGranularity`。
  loss_delay = max(loss_delay, kGranularity)

  // 在此时间之前发送的数据包被认定为丢包。
  lost_send_time = now() - loss_delay

  foreach unacked in sent_packets[pn_space]:
    if (unacked.packet_number > largest_acked_packet[pn_space]):
      continue

    // 标记数据包为丢包，或设置一个它应该被标记为丢包的时间。
    // 注意：这里使用`kPacketThreshold`的前提是
    // 假定了在数据包号空间中没有由发送方引入的空档。
    if (unacked.time_sent <= lost_send_time ||
        largest_acked_packet[pn_space] >=
          unacked.packet_number + kPacketThreshold):
      sent_packets[pn_space].remove(unacked.packet_number)
      lost_packets.insert(unacked)
    else:
      if (loss_time[pn_space] == 0):
        loss_time[pn_space] = unacked.time_sent + loss_delay
      else:
        loss_time[pn_space] = min(loss_time[pn_space],
                                  unacked.time_sent + loss_delay)
  return lost_packets
```

{{% /block_ref %}}
