---
title: "3. QUIC传输机制的设计"
anchor: "3_Design_of_the_QUIC_Transmission_Machinery"
weight: 3000
rank: "h1"
---

All transmissions in QUIC are sent with a packet-level header, which indicates the encryption level and includes a packet sequence number (referred to below as a packet number). The encryption level indicates the packet number space, as described in Section 12.3 of [QUIC-TRANSPORT]. Packet numbers never repeat within a packet number space for the lifetime of a connection. Packet numbers are sent in monotonically increasing order within a space, preventing ambiguity. It is permitted for some packet numbers to never be used, leaving intentional gaps.

在QUIC中，每次传输都会发送数据包头部，它表明了密级并且包含着一个数据包序列号（就是下文中的数据包号）。密级表明了数据包号空间，正如《[QUIC传输]()》的[第12.3章]()中所述。在一条连接的生命周期中，同一数据包号空间中的数据包号不会重复。同一数据包号空间中的数据包号以单调递增的方式发送以避免歧义。保留某些数据包号不去使用，故意留出一些空档，是被允许的。

This design obviates the need for disambiguating between transmissions and retransmissions; this eliminates significant complexity from QUIC's interpretation of TCP loss detection mechanisms.

这种设计使得不再需要区分传输和重传；它从QUIC版的TCP丢包检测机制中消去了大量的复杂度。

QUIC packets can contain multiple frames of different types. The recovery mechanisms ensure that data and frames that need reliable delivery are acknowledged or declared lost and sent in new packets as necessary. The types of frames contained in a packet affect recovery and congestion control logic:

QUIC数据包可以包含不同类型的多种帧。恢复机制确保了要求可靠分发的数据和帧要么被确认，要么被认定为丢包，然后在需要时用新数据包重新发送。数据报中包含的帧类型会影响恢复与拥塞控制的逻辑：

* All packets are acknowledged, though packets that contain no ack-eliciting frames are only acknowledged along with ack-eliciting packets.

* 所有数据包都会被确认，不过仅包含非ACK触发帧的数据包只会和ACK触发包一起被确认。

* Long header packets that contain CRYPTO frames are critical to the performance of the QUIC handshake and use shorter timers for acknowledgment.

* 包含**加密帧**的长包头数据包对QUIC握手的性能至关重要，对于它们的确认会使用更短的计时器。

* Packets containing frames besides ACK or CONNECTION_CLOSE frames count toward congestion control limits and are considered to be in flight.

* 包含除了**ACK帧**和**连接关闭帧**之外的帧的数据包会被计入拥塞控制计数，并被认为是在途数据包。

* PADDING frames cause packets to contribute toward bytes in flight without directly causing an acknowledgment to be sent.

* **填充帧**会使得数据包被计入在途字节数，但不会直接引发确认。
