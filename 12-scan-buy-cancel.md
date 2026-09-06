# 代币空投结果通知(airdrop.notify)

**应用场景**：当代币空投订单状态发生变更（如空投成功或失败）时，平台向商户提交空投下单时传入的 `notifyUrl` 发送异步结果通知。

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
| `orderAmt`      | 代币空投数量   | 必填 | N         | 代币空投数量                                                   |
| `status`        | 订单状态     | 必填 | N         | <p><code>100</code>: 交易成功<br><code>-100</code>: 交易失败</p> |
| `payTime`       | 空投完成时间   | 必填 | C (19)    | 交易成功时返回，格式：`yyyy-MM-dd HH:mm:ss`                         |
| `extrasContent` | 扩展内容     | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                          |

> **解析说明**： 以 `data` 中的 `status` 作为判定商户代币空投是否成功的依据（`status = 100` 代表空投交易成功；`status = -100` 代表空投交易失败）。

#### 通知报文示例

```json
{
  "cusUid": "48351",
  "data": "{\"currency\":\"TTT\",\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\",\"merchOrderNo\":\"MB0280342697\",\"merchUid\":48351,\"orderAmt\":\"301.1\",\"orderNo\":\"2024051522173803077\",\"payTime\":\"2024-05-15 22:17:38\",\"remark\":\"交易成功\",\"status\":100,\"tradeDate\":\"20240515\",\"userName\":\"地在\",\"userNo\":\"xxx0408\"}",
  "reqSn": "1715782658444",
  "signMsg": "XszwEOR0lZ/WjVQN9TintijQUNtMCcogbSTZkw3p/ZavNDcsVMxM1VAus/XhlqzYgXV3c+MXwtD3pb9egynXqNw5qITJefhnXmsDVf5sC7dmnyYcCVrHLjDXUj2vlblZKffRtXfId41v7Oxrf5nQ0QjmSP9+5MsUyjzZLFXdeVcrRM++BhhyTHOwNewZE4SKe26R8v0XD8LOw06Hk3Es8lVberV86ttgSQwOxNlZY96y1g19pvqR7mL5zI9mkw6L6tCKE4x4JrgvJ8noElqXmoNony1ikri35iyx5gfixdMPNWpRPgw204os1oYXWYFzqbkii6t5ixIzJW6z2LgXTA==",
  "transCode": "airdrop.notify",
  "version": "1.0"
}
```
