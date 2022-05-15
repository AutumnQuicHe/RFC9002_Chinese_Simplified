---
title: "2. 约定与定义"
anchor: "2_Conventions_and_Definitions"
weight: 2000
rank: "h1"
---

本文中的关键字“{{< req_level MUST >}}（**MUST**）”、“{{< req_level MUST_NOT >}}（**MUST NOT**）”、“{{< req_level REQUIRED >}}（**REQUIRED**）”、“{{< req_level SHALL >}}（**SHALL**）”、“{{< req_level SHALL_NOT >}}（**SHALL NOT**）”、“{{< req_level SHOULD >}}（**SHOULD**）”、“{{< req_level SHOULD_NOT >}}（**SHOULD NOT**）”、“{{< req_level RECOMMENDED >}}（**RECOMMENDED**）”、“{{< req_level NOT_RECOMMENDED >}}（**NOT RECOMMENDED**）”、“{{< req_level MAY >}}（**MAY**）”，以及“{{< req_level OPTIONAL >}}（**OPTIONAL**）”应理解为BCP 14 《[RFC2119](#RFC2119)》《[RFC8174](#RFC8174)》所描述的，当且仅当它们像本段一样以斜体加粗方式出现的时候。

本文档中使用到的术语定义如下：

ACK触发帧：

:   除了**ACK帧**、**填充帧**和**连接关闭帧**之外的所有帧都被认为是会触发ACK的帧。

ACK触发包：

:   包含ACK触发帧的数据包会使得接收方在不超过最大确认延迟的时间内发送一个**ACK**，它们被称为ACK触发包。

在途数据包：

:   当数据包会触发ACK或包含填充帧，并且它们已被发送出去，但处于未被确认、被认定为丢包或被与旧密钥一并丢弃的状态时，被称为在途数据包。
