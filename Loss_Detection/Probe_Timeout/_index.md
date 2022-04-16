---
title: "6.2. 探测包超时"
anchor: "6.2_Probe_Timeout"
weight: 6200
rank: "h2"
---

A Probe Timeout (PTO) triggers the sending of one or two probe datagrams when ack-eliciting packets are not acknowledged within the expected period of time or the server may not have validated the client's address. A PTO enables a connection to recover from loss of tail packets or acknowledgments.

当ACK触发包没有在期望的时间内得到确认或服务器可能还没有验证完客户端的地址时，探测包超时（PTO）能够引发一至两个探测数据报。PTO使得连接能够从丢失队尾数据包或确认的状态中恢复过来。

As with loss detection, the PTO is per packet number space. That is, a PTO value is computed per packet number space.

就像丢包检测一样，每个数据包号空间中的PTO也是独立的。也就是说，每个数据包号空间中的PTO值是单独计算的。

A PTO timer expiration event does not indicate packet loss and MUST NOT cause prior unacknowledged packets to be marked as lost. When an acknowledgment is received that newly acknowledges packets, loss detection proceeds as dictated by the packet and time threshold mechanisms; see Section 6.1.

PTO计时器的超时事件并不表明数据包遭遇了丢包，并且它{{< req_level MUST_NOT >}}使得在它之前的尚未确认的数据包被认定为丢包。当接收到确认且它新确认了一些数据包时，丢包检测机制会遵循数据包数量阈值和数据包发送时间阈值启动；详见[第6.1章]()。

The PTO algorithm used in QUIC implements the reliability functions of Tail Loss Probe [RFC8985], RTO [RFC5681], and F-RTO algorithms for TCP [RFC5682]. The timeout computation is based on TCP's RTO period [RFC6298].

QUIC中使用的PTO算法实现了尾部丢失探测（详见《[RFC8985]()》）中的可靠度函数、RTO（详见《[RFC5681]()》）和面向TCP的F-RTO算法（详见《[RFC5682]()》）。超时的计算方法是基于TCP的RTO时间（详见《[RFC6298]()》）的。
