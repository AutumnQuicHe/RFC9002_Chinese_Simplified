---
title: "A.8. 设置丢包检测计时器"
anchor: "A.8_Setting_the_Loss_Detection_Timer"
weight: 10080
rank: "h2"
---

QUIC loss detection uses a single timer for all timeout loss detection. The duration of the timer is based on the timer's mode, which is set in the packet and timer events further below. The function SetLossDetectionTimer defined below shows how the single timer is set.

QUIC的丢包检测使用一个计时器来检测所有超时事件，计时器的时长取决于计时器的模式，后者是在下文描述的数据包事件和计时器事件中指定的。下文定义的`SetLossDetectionTimer`展示了怎样设置这个计时器。

This algorithm may result in the timer being set in the past, particularly if timers wake up late. Timers set in the past fire immediately.

本算法可能导致计时器被设置到一个过去的时间，尤其是计时器没有被及时唤醒时。被设置到过去的时间的计时器会立即超时。

Pseudocode for SetLossDetectionTimer follows (where the "^" operator represents exponentiation):

`SetLossDetectionTimer`的伪代码如下（其中`^`符号表示幂运算）：

{{% block_ref
indx="Pseudocode_10_8_1" %}}

```
GetLossTimeAndSpace():
  time = loss_time[Initial]
  space = Initial
  for pn_space in [ Handshake, ApplicationData ]:
    if (time == 0 || loss_time[pn_space] < time):
      time = loss_time[pn_space];
      space = pn_space
  return time, space

GetPtoTimeAndSpace():
  duration = (smoothed_rtt + max(4 * rttvar, kGranularity))
      * (2 ^ pto_count)
  // 解死锁PTO从当前时间启动。
  if (没有在途的ACK触发包):
    assert(!PeerCompletedAddressValidation())
    if (有握手密钥):
      return (now() + duration), Handshake
    else:
      return (now() + duration), Initial
  pto_timeout = infinite
  pto_space = Initial
  for space in [ Initial, Handshake, ApplicationData ]:
    if (该space中没有在途的ACK触发包):
      continue;
    if (space == ApplicationData):
      // 除非握手已确认，否则跳过应用数据。
      if (未确认握手):
        return pto_timeout, pto_space
      // 为应用数据空间将`max_ack_delay`和补偿纳入考量
      duration += max_ack_delay * (2 ^ pto_count)

    t = time_of_last_ack_eliciting_packet[space] + duration
    if (t < pto_timeout):
      pto_timeout = t
      pto_space = space
  return pto_timeout, pto_space

PeerCompletedAddressValidation():
  // 假定客户端已隐式地验证了服务器的地址
  if (终端是服务器):
    return true
  // 当接收到受保护的数据包时，
  // 服务器完成地址验证。
  return 已接收到对于握手的确认 || 握手已确认

SetLossDetectionTimer():
  earliest_loss_time, _ = GetLossTimeAndSpace()
  if (earliest_loss_time != 0):
    // 基于数据包发送时间阈值的丢包检测法。
    loss_detection_timer.update(earliest_loss_time)
    return

  if (server is at anti-amplification limit):
    // 如果没有数据可供发送，那么服务器不会设置计时器。
    loss_detection_timer.cancel()
    return

  if (没有在途的ACK触发包 &&
      PeerCompletedAddressValidation()):
    // 没有数据包可供丢包检测，所以不会设置计时器。
    // 但是，如果服务器可能被抗放大上限阻止了发送，
    // 那么客户端需要设置计时器
    loss_detection_timer.cancel()
    return

  timeout, _ = GetPtoTimeAndSpace()
  loss_detection_timer.update(timeout)
```

{{% /block_ref %}}
