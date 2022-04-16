---
title: "6.2.2. 握手与新路径"
anchor: "6.2.2_Handshakes_and_New_Paths"
weight: 6220
rank: "h3"
---

Resumed connections over the same network MAY use the previous connection's final smoothed RTT value as the resumed connection's initial RTT. When no previous RTT is available, the initial RTT SHOULD be set to 333 milliseconds. This results in handshakes starting with a PTO of 1 second, as recommended for TCP's initial RTO; see Section 2 of [RFC6298].

在相同的网络路径上恢复出来的连接{{< req_level MAY >}}使用先前连接中最终的经平滑的RTT值作为恢复出来的连接的初始RTT。如果没有先前的RTT可用，那么初始RTT{{< req_level SHOULD >}}被设置为333毫秒。这能使得握手以1秒的PTO启动，这与TCP初始RTO的推荐值一致；详见《[RFC6298]()》的[第2章]()。

A connection MAY use the delay between sending a PATH_CHALLENGE and receiving a PATH_RESPONSE to set the initial RTT (see kInitialRtt in Appendix A.2) for a new path, but the delay SHOULD NOT be considered an RTT sample.

连接可以使用从发送**通道挑战帧**起至接收到**回复通道帧**为止所经过的时间来为新路径设置初始RTT（详见[附录A.2]()中的`kInitialRtt`），但是该时间{{< req_level SHOULD_NOT >}}被取作RTT样本。

When the Initial keys and Handshake keys are discarded (see Section 6.4), any Initial packets and Handshake packets can no longer be acknowledged, so they are removed from bytes in flight. When Initial or Handshake keys are discarded, the PTO and loss detection timers MUST be reset, because discarding keys indicates forward progress and the loss detection timer might have been set for a now-discarded packet number space.

当初始密钥和握手密钥被弃用后（详见[第6.4章]()），无法确认任何初始数据包和握手数据包，所以可以将他们从在途字节计数中移除。当弃用初始密钥或握手密钥时，{{< req_level MUST >}}重置PTO和丢包检测计时器，因为弃用密钥表明了进度的推进，而丢包检测计时器可能是为已弃用的数据包号空间设置的。
