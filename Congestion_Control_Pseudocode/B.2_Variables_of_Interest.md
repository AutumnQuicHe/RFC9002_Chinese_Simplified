---
title: "B.2. 感兴趣的变量"
anchor: "B.2_Variables_of_Interest"
weight: 11020
rank: "h2"
---

Variables required to implement the congestion control mechanisms are described in this section.

本节描述了实现拥塞控制机制所需的变量。

max_datagram_size:
The sender's current maximum payload size. This does not include UDP or IP overhead. The max datagram size is used for congestion window computations. An endpoint sets the value of this variable based on its Path Maximum Transmission Unit (PMTU; see Section 14.2 of [QUIC-TRANSPORT]), with a minimum value of 1200 bytes.

`max_datagram_size`：

:   发送方当前的最大载荷尺寸。其中不包含UDP或IP头部。最大的数据包尺寸会被用于计算拥塞窗口。终端基于其路径最大传输单元（PMTU；详见《[QUIC传输]()》的[第14.2章]()）来设置该值，且不会低于1200字节。

ecn_ce_counters[kPacketNumberSpace]:
The highest value reported for the ECN-CE counter in the packet number space by the peer in an ACK frame. This value is used to detect increases in the reported ECN-CE counter.

`ecn_ce_counters[kPacketNumberSpace]`：

:   该数据包号空间中由对端在**ACK帧**中为ECN-CE计数器报告的最大值。该值被用于检测ECN-CE计数是否增加。

bytes_in_flight:
The sum of the size in bytes of all sent packets that contain at least one ack-eliciting or PADDING frame and have not been acknowledged or declared lost. The size does not include IP or UDP overhead, but does include the QUIC header and Authenticated Encryption with Associated Data (AEAD) overhead. Packets only containing ACK frames do not count toward bytes_in_flight to ensure congestion control does not impede congestion feedback.

`bytes_in_flight`：

:   所有已发送的、包含至少一个ACK触发帧或**填充帧**的且尚未得到确认或被认定为丢包的数据包以字节为单位的尺寸总和。其中不包含IP或UDP头部，但是包含QUIC头部和带有关联数据的认证加密（AEAD）开销。仅包含**ACK帧**的数据包不会被计入`bytes_in_flight`以确保拥塞控制不会妨碍拥塞反馈。

congestion_window:
Maximum number of bytes allowed to be in flight.

`congestion_window`：

:   允许的在途字节数的最大值。

congestion_recovery_start_time:
The time the current recovery period started due to the detection of loss or ECN. When a packet sent after this time is acknowledged, QUIC exits congestion recovery.

`congestion_recovery_start_time`：

:   因为检测到丢包或ECN而进入当前恢复期的时间。当在此时间后发送的数据包得到确认时，QUIC会退出拥塞恢复。

ssthresh:
Slow start threshold in bytes. When the congestion window is below ssthresh, the mode is slow start and the window grows by the number of bytes acknowledged.

`ssthresh`：

:   慢启动以字节为单位的阈值。当拥塞窗口尺寸低于`ssthresh`时，就会处于慢启动状态，并且窗口会随着得到确认的字节数增长而扩大。

The congestion control pseudocode also accesses some of the variables from the loss recovery pseudocode.

拥塞控制的伪代码还访问了一些来自丢包恢复伪代码中的变量。

