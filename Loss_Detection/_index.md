---
title: "6. 丢包检测"
anchor: "6_Loss_Detection"
weight: 6000
rank: "h1"
---

QUIC发送方使用确认来检测遭遇丢包的数据包，使用PTO来确保确认已被接收到；详见[第6.2章](#6.2_Probe_Timeout)。本章描述了这些算法。

如果某数据包遭遇丢包，那么QUIC传输需要从该丢包的状态中恢复，例如通过重传数据、发送更新后的帧或放弃传输该帧的的方式。更多信息详见《[QUIC传输](../RFC9000_Chinese_Simplified)》的[第13.3章](../RFC9000_Chinese_Simplified/#13.3_Retransmission_of_Information)。

不像RTT测量和拥塞控制那样，每个数据包号空间中的丢包检测是独立的，因为RTT和拥塞控制都是路径的某些属性，而丢包检测还依赖着密钥的可用性。
