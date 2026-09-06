# 提币结果通知(withdraw.notify)

**应用场景**：当提币订单状态发生变更（如交易成功或失败）时，平台向商户提交提币下单时传入的 `notifyUrl` 发送异步结果通知。

***

### 1. 通知报文

#### 业务参数定义

| 节点名             | 字段名称     | 可空 | 类型        | 备注                                                       |
| --------------- | -------- | -- | --------- | -------------------------------------------------------- |
| `merchUid`      | 商户UID    | 必填 | N         | 商户UID                                                    |
| `merchOrderNo`  | 商户单号     | 必填 | C (1,64)  | 商户系统唯一订单号                                                |
| `currency`      | 交易币种（代币） | 必填 | C (1,16)  | 参考交易币种字典                                                 |
| `orderNo`       | 平台单号     | 必填 | C (19)    | 平台唯一订单号                                                  |
| `tradeDate`     | 交易日期     | 必填 | C (8)     | 平台下单日期，格式:yyyyMMdd                                       |
| `userNo`        | 用户编号     | 必填 | C (1,32)  | 发起支付的用户唯一标识                                              |
| `userName`      | 用户真实姓名   | 必填 | C (1,32)  | 用户付款账户的真实姓名                                              |
| `orderAmt`      | 下单数量     | 必填 | N         | 下单数量                                                     |
| `payAmt`        | 实际交易数量   | 必填 | N         | 实际交易数量，交易成功时返回                                           |
| `status`        | 订单状态     | 必填 | N         | <p><code>100</code>: 交易成功<br><code>-100</code>: 交易失败</p> |
| `payTime`       | 交易完成时间   | 必填 | C (19)    | 交易成功时返回，格式：`yyyy-MM-dd HH:mm:ss`                         |
| `extrasContent` | 扩展内容     | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                          |
| `remark`        | 业务返回描述   | 可空 | C (0,64)  | 业务返回描述                                                   |

> **解析说明**： 以 `data` 中的 `status` 作为判定提币是否成功的依据（`status = 100` 代表提币交易成功，不代表提币的挂单到账，挂单是否到账，请关注提币到账确认链接中的订单状态）。

#### 通知报文示例

```json
{
  "cusUid": "54453",
  "data": "{\"merchUid\":54453,\"merchOrderNo\":\"MB5850674588\",\"currency\":\"CNY\",\"orderNo\":\"2023102710333381795\",\"tradeDate\":\"20231027\",\"userNo\":\"sell006\",\"userName\":\"张三\",\"orderAmt\":\"1000\",\"payAmt\":\"1000\",\"status\":100,\"payTime\":\"2023-10-27 10:33:59\",\"remark\":\"交易成功\"}",
  "reqSn": "1698480682805",
  "signMsg": "OULVSZR2aWIutwDKk9rNjGUPBxYT8+UJaCY0yeilcPYu1a4kY2h5TjT6QQARVwEa3ajzXP0RYxiO04E8WXNNzJ7d01EKNSR+jtgf7Mp3t0ka9LXoZq6f/zYUUALONYXRJR6mFw+bcFm6EG6wneHzu3DoFlhP52SLxgjSv6HhrQa/vzcLsaRzDUuRHxCxs1fA5RH++meWCI3nyL7ax5BdLIWrC9UBAcsVoVXTmboGj3v3/GRloKntwg04WqLCMZlkKY+tnxNqpVEyiGnSRZ/q99+iMbrMfJPfevt96mWV+ynQIYsOQ9iDahxtmJQ6iGcT2zuKGF2+hLPPhUUQYUIcbQ==",
  "transCode": "withdraw.notify",
  "version": "1.0"
}
```
