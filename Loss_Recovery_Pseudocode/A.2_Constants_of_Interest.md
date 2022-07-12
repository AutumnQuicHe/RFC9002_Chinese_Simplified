---
title: "A.2. 感兴趣的常量"
anchor: "A.2_Constants_of_Interest"
weight: 10020
rank: "h2"
---

在丢包恢复中使用到的常量是基于一系列RFC、论文和常用实践的组合。

`kPacketThreshold`：

:   在基于数据包数量阈值的丢包检测法认定某数据包丢包前允许出现乱序数据包的最大数量。在[第6.1.1章](#6.1.1_Packet_Threshold)中推荐的值为`3`。

`kTimeThreshold`：

:   在基于数据包发送时间阈值的丢包检测法认定某数据包丢包前允许出现乱序数据包的最长时间。它被指定为RTT倍率。在[第6.1.2章](#6.1.2_Time_Threshold)中推荐的值为`9/8`。

`kGranularity`：

:   计时器粒度。这是一个与系统相关的值，在[第6.1.2章](#6.1.2_Time_Threshold)中推荐的值为1毫秒。

`kInitialRtt`：

:   在对RTT进行采样前使用的RTT初始值。在[第6.2.2章](#6.2.2_Handshakes_and_New_Paths)中推荐的值为333毫秒。

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
