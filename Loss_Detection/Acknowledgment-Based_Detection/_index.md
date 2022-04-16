---
title: "6.1. 基于确认的检测"
anchor: "6.1_Acknowledgment-Based_Detection"
weight: 6100
rank: "h2"
---

Acknowledgment-based loss detection implements the spirit of TCP's Fast Retransmit [RFC5681], Early Retransmit [RFC5827], Forward Acknowledgment [FACK], SACK loss recovery [RFC6675], and RACK-TLP [RFC8985]. This section provides an overview of how these algorithms are implemented in QUIC.

基于确认的丢包检测整合实现了TCP的快速重传（详见《[RFC5681]()》）、早期重传（详见《[RFC5827]()》）、前向确认（详见《[FACK]()》）、SACK丢包恢复（详见《[RFC6675]()》）和RACK-TLP（详见《[RFC8985]()》）的精髓。本节概述了这些算法在QUIC中是如何实现的。

A packet is declared lost if it meets all of the following conditions:

如果数据包满足了所有以下条件，那么它会被认定为丢包：

* The packet is unacknowledged, in flight, and was sent prior to an acknowledged packet.

* 该数据包处于未被确认、在途且发送时间先于某已被确认的数据包。

* The packet was sent kPacketThreshold packets before an acknowledged packet (Section 6.1.1), or it was sent long enough in the past (Section 6.1.2).

* 该数据包的发送顺序比某已被确认的数据包还要早`kPacketThreshold`的数据包（详见[第6.1.1章]()），或距离其发送已经过去足够久的时间（详见[第6.1.2章]()）。

The acknowledgment indicates that a packet sent later was delivered, and the packet and time thresholds provide some tolerance for packet reordering.

确认能表明某个后发送的数据包已经被接收到，而数据包数量阈值和数据包发送时间阈值为数据包乱序提供了一定的容忍度。

Spuriously declaring packets as lost leads to unnecessary retransmissions and may result in degraded performance due to the actions of the congestion controller upon detecting loss. Implementations can detect spurious retransmissions and increase the packet or time reordering threshold to reduce future spurious retransmissions and loss events. Implementations with adaptive time thresholds MAY choose to start with smaller initial reordering thresholds to minimize recovery latency.

将数据包错误地认定为丢包会导致不必要的重传，并有可能因为拥塞控制器在检测到丢失时的行为而产生性能上的损失。QUIC实现可以检测到无效重传，然后提高针对乱序的数据包数量阈值或数据包发送时间阈值来减少将来的无效重传和错误的丢包事件。具有自适应的时间阈值的QUIC实现{{< req_level MAY >}}选择以较小的初始乱序阈值来启动，以最小化恢复延迟。
