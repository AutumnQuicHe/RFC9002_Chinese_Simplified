---
title: "7. 拥塞控制"
anchor: "7_Congestion_Control"
weight: 7000
rank: "h1"
---

本文档为QUIC定义了一种与TCP的NewReno算法（详见《[RFC6582](https://www.rfc-editor.org/info/rfc6582)》）类似的位于发送方一侧的拥塞控制器。

QUIC为拥塞控制提供一些通用的信号，它们被设计为能够支持各种位于发送方一侧的算法。发送方可以单方面地选择使用不同的算法，例如CUBIC（详见《[RFC8312](https://www.rfc-editor.org/info/rfc8312)》）。

如果发送方使用的控制器与本文档中定义的不同，那么所选的控制器{{< req_level MUST >}}遵循《[RFC8085](https://www.rfc-editor.org/info/rfc8085)》的[第3.1章](https://www.rfc-editor.org/rfc/rfc8085.html#section-3.1)中规定的拥塞控制规范。

与TCP类似，仅包含**ACK帧**的数据包不会被计入在途字节计数，也不会受到拥塞控制。与TCP不一样的是，QUIC能够检测到这些数据包的丢包情况，并且{{< req_level MAY >}}使用此信息来调整拥塞控制器或调整仅包含**ACK帧**的数据包的发送速率，但本文档中并没有描述如何进行此过程。

如《[QUIC传输](../RFC9000_Chinese_Translation)》的[第9.4章](../RFC9000_Chinese_Translation/#9.4_Loss_Detection_and_Congestion_Control)所述，每条路径上的拥塞控制器是独立的，所以在其他路径上发送的数据包不会影响当前路径上的拥塞控制器。

本文档中的算法以字节为单位指定和使用控制器的拥塞窗口。

如果在途字节数（详见[附录B.2](#B.2_Variables_of_Interest)）会超过拥塞窗口，那么终端{{< req_level MUST_NOT >}}发送数据包，除非这个数据包是因为PTO超时而发送的（详见[第6.2章](#6.2_Probe_Timeout)），或是因为进入了恢复期而发送的（详见[第7.3.2章](#7.3.2_Recovery)）。
