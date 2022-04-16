---
title: "7. 拥塞控制"
anchor: "7_Congestion_Control"
weight: 7000
rank: "h1"
---

This document specifies a sender-side congestion controller for QUIC similar to TCP NewReno [RFC6582].

本文档为QUIC定义了一种与TCP的NewReno（详见《[RFC6582]()》）算法类似的位于发送方的拥塞控制器。

The signals QUIC provides for congestion control are generic and are designed to support different sender-side algorithms. A sender can unilaterally choose a different algorithm to use, such as CUBIC [RFC8312].

QUIC为拥塞控制提供一些通用的信号，它们被设计为能够支持各种位于发送方的算法。发送方可以单方面地选择使用不同的算法，例如CUBIC（详见《[RFC8312]()》）。

If a sender uses a different controller than that specified in this document, the chosen controller MUST conform to the congestion control guidelines specified in Section 3.1 of [RFC8085].

如果发送方使用的控制器与本文档中定义的不同，那么所选的控制器{{< req_level MUST >}}遵循《[RFC8085]()》的[第3.1章]()中规定的拥塞控制规范。

Similar to TCP, packets containing only ACK frames do not count toward bytes in flight and are not congestion controlled. Unlike TCP, QUIC can detect the loss of these packets and MAY use that information to adjust the congestion controller or the rate of ACK-only packets being sent, but this document does not describe a mechanism for doing so.

与TCP类似，仅包含**ACK帧**的数据包不会被计入在途字节计数，也不会受到拥塞控制。与TCP不一样的是，QUIC能够检测到这些数据包的丢包情况，并且{{< req_level MAY >}}使用此信息来调整拥塞控制器或调整仅包含**ACK帧**的数据包的发送速率，但本文档中并没有描述有关如何进行此过程的机制。

The congestion controller is per path, so packets sent on other paths do not alter the current path's congestion controller, as described in Section 9.4 of [QUIC-TRANSPORT].

如《[QUIC传输]()》的[第9.4章]()所述，每条路径上的拥塞控制器是独立的，所以在其他路径上发送的数据包不会影响当前路径上的拥塞控制器。

The algorithm in this document specifies and uses the controller's congestion window in bytes.

本文档中的算法以字节为单位指定和使用控制器的拥塞窗口。

An endpoint MUST NOT send a packet if it would cause bytes_in_flight (see Appendix B.2) to be larger than the congestion window, unless the packet is sent on a PTO timer expiration (see Section 6.2) or when entering recovery (see Section 7.3.2).

如果在途字节数（详见[附录B.2]()）会超过拥塞窗口，那么终端{{< req_level MUST_NOT >}}发送数据包，除非这个数据包是因为PTO超时而发送的（详见[第6.2章]()），或是因为启动了恢复流程而发送的（详见[第7.3.2章]()）。
