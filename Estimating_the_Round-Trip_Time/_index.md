---
title: "5. 预估往返时间"
anchor: "5_Estimating_the_Round-Trip_Time"
weight: 5000
rank: "h1"
---

终端在架构中更上层的位置测量从数据包发送的一刻起至它被确认为止所经历的时间，并将之作为RTT样本。终端使用RTT样本和由对端报告的主机延迟（详见《[QUIC传输](../RFC9000_Chinese_Translation)》的[第13.2章](../RFC9000_Chinese_Translation/#13.2_Generating_Acknowledgments)）来以统计上的方法创建对于网络路径RTT的描述。终端为每条路径上观测到的RTT样本（`rttvar`）计算以下三个值：在一段时间内的最小值（`min_rtt`）、以指数形式加权的滑动平均值（`smoothed_rtt`）、以及平均差（在后文中称之为“偏差”）。
