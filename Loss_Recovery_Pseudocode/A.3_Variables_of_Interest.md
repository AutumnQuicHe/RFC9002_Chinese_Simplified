---
title: "A.3. 感兴趣的变量"
anchor: "A.3_Variables_of_Interest"
weight: 10030
rank: "h2"
---

Variables required to implement the congestion control mechanisms are described in this section.

本节描述了实现丢包检测机制所需的变量。

latest_rtt:
The most recent RTT measurement made when receiving an acknowledgment for a previously unacknowledged packet.

`latest_rtt`：

:   当接收到对于一个未曾确认过的数据包的确认时的最近一次的RTT测量值。

smoothed_rtt:
The smoothed RTT of the connection, computed as described in Section 5.3.

`smoothed_rtt`：

:   当前连接的经平滑的RTT，有关计算方法详见[第5.3章]()。

rttvar:
The RTT variation, computed as described in Section 5.3.

`rttvar`：

:   RTT的偏差，有关计算方法详见[第5.3章]()。

min_rtt:
The minimum RTT seen over a period of time, ignoring acknowledgment delay, as described in Section 5.2.

`min_rtt`：

:   在一段时间内观测到的RTT最小值，并忽略确认延迟，详见[第5.2章]()。

first_rtt_sample:
The time that the first RTT sample was obtained.

`first_rtt_sample`：

:   取得首份RTT样本的时间。

max_ack_delay:
The maximum amount of time by which the receiver intends to delay acknowledgments for packets in the Application Data packet number space, as defined by the eponymous transport parameter (Section 18.2 of [QUIC-TRANSPORT]). Note that the actual ack_delay in a received ACK frame may be larger due to late timers, reordering, or loss.

`max_ack_delay`：

:   接收方有意拖延对处于应用数据数据包号空间中的数据包的确认的最长时间，其定义与同名传输参数一致（详见《[QUIC传输]()》的[第18.2章]()）。注意在接收到的**ACK帧**中的实际`ack_delay`可能会因为计时器延迟、数据包乱序或丢包的原因而超过该值。

loss_detection_timer:
Multi-modal timer used for loss detection.

`loss_detection_timer`：

:   用于丢包检测的多用途计时器

pto_count:
The number of times a PTO has been sent without receiving an acknowledgment.

`pto_count`：

:   在没有接收到确认的情况下PTO超时的触发次数。

time_of_last_ack_eliciting_packet[kPacketNumberSpace]:
The time the most recent ack-eliciting packet was sent.

`time_of_last_ack_eliciting_packet[kPacketNumberSpace]`：

:   最近一个ACK触发包被发送时的时间。

largest_acked_packet[kPacketNumberSpace]:
The largest packet number acknowledged in the packet number space so far.

`largest_acked_packet[kPacketNumberSpace]`：

:   至今为止在该数据包号空间中发送过的最大数据包号。

loss_time[kPacketNumberSpace]:
The time at which the next packet in that packet number space can be considered lost based on exceeding the reordering window in time.

`loss_time[kPacketNumberSpace]`：

:   该数据包号空间中的下一个数据包会因为超过乱序数据包的时间阈值而被认定为丢包的时间。

sent_packets[kPacketNumberSpace]:
An association of packet numbers in a packet number space to information about them. Described in detail above in Appendix A.1.

`sent_packets[kPacketNumberSpace]`：

:   该数据包号空间中数据包号与其对应的数据包信息之间的关联。在上文的[附录A.1]()中已详细描述。
