---
title: "5. 预估往返时间"
anchor: "5_Estimating_the_Round-Trip_Time"
weight: 5000
rank: "h1"
---

At a high level, an endpoint measures the time from when a packet was sent to when it is acknowledged as an RTT sample. The endpoint uses RTT samples and peer-reported host delays (see Section 13.2 of [QUIC-TRANSPORT]) to generate a statistical description of the network path's RTT. An endpoint computes the following three values for each path: the minimum value over a period of time (min_rtt), an exponentially weighted moving average (smoothed_rtt), and the mean deviation (referred to as "variation" in the rest of this document) in the observed RTT samples (rttvar).

终端在架构中更上层的位置测量从数据包发送的一刻起至它被确认为止所经历的时间，并将之作为RTT样本。终端使用RTT样本和由对端报告的主机延迟（详见《[QUIC传输]()》的[第13.2章]()）来以统计上的方法创建对于网络路径RTT的描述。终端为每条路径上观测到的RTT样本（`rttvar`）计算以下三个值：在一段时间内的最小值（`min_rtt`）、以指数形式加权的滑动平均值（`smoothed_rtt`）、以及平均差（在后文中称之为“偏差”）。
