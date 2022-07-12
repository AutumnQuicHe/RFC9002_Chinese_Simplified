---
title: "B.1. 感兴趣的常量"
anchor: "B.1_Constants_of_Interest"
weight: 11020
rank: "h2"
---

在拥塞控制中使用到的常量是基于一系列RFC、论文和常用实践的组合。

`kInitialWindow`：

:   在途字节数的初始值，详见[第7.2章](#7.2_Initial_and_Minimum_Congestion_Window)。

`kMinimumWindow`：

:   拥塞窗口的最小字节数，详见[第7.2章](#7.2_Initial_and_Minimum_Congestion_Window)。

`kLossReductionFactor`：

:   当检测到新的丢包事件而缩小拥塞窗口时使用的缩放因子。在[第7章](#7_Congestion_Control)中推荐的值为`0.5`。

`kPersistentCongestionThreshold`：

:   用于判定持续拥塞的时间量，它被指定为PTO倍率。在[第7.6章](#7.6_Persistent_Congestion)中推荐的值为`3`。
