---
title: "B.1. 感兴趣的常量"
anchor: "B.1_Constants_of_Interest"
weight: 11020
rank: "h2"
---

Constants used in congestion control are based on a combination of RFCs, papers, and common practice.

在拥塞控制中使用到的常量是基于一系列RFC、论文和常用实践的组合。

kInitialWindow:
Default limit on the initial bytes in flight as described in Section 7.2.

`kInitialWindow`：

:   在途字节数的初始值，详见[第7.2章]()。

kMinimumWindow:
Minimum congestion window in bytes as described in Section 7.2.

`kMinimumWindow`：

:   拥塞窗口的最小字节数，详见[第7.2章]()。

kLossReductionFactor:
Scaling factor applied to reduce the congestion window when a new loss event is detected. Section 7 recommends a value of 0.5.

`kLossReductionFactor`：

:   当检测到新的丢包事件而缩小拥塞窗口时使用的缩放因子。在[第7章]()中推荐的值为`0.5`。

kPersistentCongestionThreshold:
Period of time for persistent congestion to be established, specified as a PTO multiplier. Section 7.6 recommends a value of 3.

`kPersistentCongestionThreshold`：

:   用于判定持续拥塞的时间量，它被指定为PTO倍率。在[第7.6章]()中推荐的值为`3`。
