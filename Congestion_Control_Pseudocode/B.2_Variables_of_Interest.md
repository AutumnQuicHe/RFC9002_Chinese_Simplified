---
title: "B.2. 感兴趣的变量"
anchor: "B.2_Variables_of_Interest"
weight: 11020
rank: "h2"
---

本节描述了实现拥塞控制机制所需的变量。

`max_datagram_size`：

:   发送方当前的最大载荷尺寸。其中不包含UDP或IP头部。最大的数据包尺寸会被用于计算拥塞窗口。终端基于其路径最大传输单元（PMTU；详见《[QUIC传输](../RFC9000_Chinese_Translation)》的[第14.2章](../RFC9000_Chinese_Translation/#14.2_Path_Maximum_Transmission_Unit)）来设置该值，且不会低于1200字节。

`ecn_ce_counters[kPacketNumberSpace]`：

:   该数据包号空间中由对端在**ACK帧**中为`ECN-CE`计数器报告的最大值。该值被用于检测`ECN-CE`计数是否增加。

`bytes_in_flight`：

:   所有已发送的、包含至少一个ACK触发帧或**填充帧**的且尚未得到确认或被认定为丢包的数据包以字节为单位的尺寸总和。其中不包含IP或UDP头部，但是包含QUIC头部和带有关联数据的认证加密（AEAD）开销。仅包含**ACK帧**的数据包不会被计入`bytes_in_flight`以确保拥塞控制不会妨碍拥塞反馈。

`congestion_window`：

:   允许的在途字节数的最大值。

`congestion_recovery_start_time`：

:   因为检测到丢包或ECN而进入当前恢复期的时间。当在此时间后发送的数据包得到确认时，QUIC会退出拥塞恢复。

`ssthresh`：

:   慢启动以字节为单位的阈值。当拥塞窗口尺寸低于`ssthresh`时，就会处于慢启动状态，并且窗口会随着得到确认的字节数增长而扩大。

拥塞控制的伪代码还访问了一些来自丢包恢复伪代码中的变量。
