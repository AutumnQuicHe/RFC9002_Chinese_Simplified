---
title: "A.1. 追踪已发送的数据包"
anchor: "A.1_Tracking_Sent_Packets"
weight: 10010
rank: "h2"
---

To correctly implement congestion control, a QUIC sender tracks every ack-eliciting packet until the packet is acknowledged or lost. It is expected that implementations will be able to access this information by packet number and crypto context and store the per-packet fields (Appendix A.1.1) for loss recovery and congestion control.

要准确实现拥塞控制，QUIC发送方得在每一个ACK触发包得到确认或遭遇丢包前保持对它们的追踪。QUIC实现应该有能力通过数据包号和加密上下文访问到该追踪信息，并将每个数据包的追踪字段（详见[附录A.1.1]()）存储起来用于丢包恢复和拥塞控制。

After a packet is declared lost, the endpoint can still maintain state for it for an amount of time to allow for packet reordering; see Section 13.3 of [QUIC-TRANSPORT]. This enables a sender to detect spurious retransmissions.

在数据包被认定为丢包后，终端仍可以将它的状态数据保留一段时间以允许数据包乱序的情况出现；详见《[QUIC传输]()》的[第13.3章]()。这使得发送方能够检测到无效的重传。

Sent packets are tracked for each packet number space, and ACK processing only applies to a single space.

每个数据包号空间中的已发送数据包都会被追踪，而对**ACK帧**的处理则只适用于单个空间。
