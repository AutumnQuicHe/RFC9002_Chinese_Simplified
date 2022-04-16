---
title: "A.2. 感兴趣的常量"
anchor: "A.2_Constants_of_Interest"
weight: 10020
rank: "h2"
---

Constants used in loss recovery are based on a combination of RFCs, papers, and common practice.

在丢包恢复中使用到的常量是基于一系列RFC、论文和常用实践的组合。

kPacketThreshold:
Maximum reordering in packets before packet threshold loss detection considers a packet lost. The value recommended in Section 6.1.1 is 3.

`kPacketThreshold`：

:   在基于数据包数量阈值的丢包检测法认定某数据包丢包前允许出现乱序数据包的最大数量。在[第6.1.1章]()中推荐的值为`3`。

kTimeThreshold:
Maximum reordering in time before time threshold loss detection considers a packet lost. Specified as an RTT multiplier. The value recommended in Section 6.1.2 is 9/8.

`kTimeThreshold`：

:   在基于数据包发送时间阈值的丢包检测法认定某数据包丢包前允许出现乱序数据包的最长时间。它被指定为RTT倍率。在[第6.1.2章]()中推荐的值为`9/8`。

kGranularity:
Timer granularity. This is a system-dependent value, and Section 6.1.2 recommends a value of 1 ms.

`kGranularity`：

:   计时器粒度。这是一个与系统相关的值，在[第6.1.2章]()中推荐的值为1毫秒。

kInitialRtt:
The RTT used before an RTT sample is taken. The value recommended in Section 6.2.2 is 333 ms.

`kInitialRtt`：

:   在对RTT进行采样前使用的RTT初始值。在[第6.2.2章]()中推荐的值为333毫秒。

kPacketNumberSpace:
An enum to enumerate the three packet number spaces:

`kPacketNumberSpace`：

:   用于枚举三个数据包号空间的枚举值。

{{% block_ref
indx="Pseudocode_10_2_1" %}}

```
enum kPacketNumberSpace {
  Initial,
  Handshake,
  ApplicationData,
}
```

{{% /block_ref %}}
