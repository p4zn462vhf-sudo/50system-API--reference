# 钱包余额查询 (wap.balance)

**应用场景**：商户系统向平台发起钱包余额查询，平台返回被查询钱包余额数据。

***

### 1. 请求报文

#### 业务参数定义

| 节点名        | 字段名称   | 可空 | 类型       | 备注          |
| ---------- | ------ | -- | -------- | ----------- |
| `merchUid` | 商户UID  | 必填 | N        | 商户UID       |
| `userNo`   | 用户编号   | 必填 | C (1,32) | 发起支付的用户唯一标识 |
| `userName` | 用户真实姓名 | 必填 | C (1,32) | 用户付款账户的真实姓名 |
| `currency` | 币种（代币） | 必填 | C (1,16) | 币种          |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":48351,\"userNo\":\"1120\",\"currency\":\"TTT\",\"userName\":\"1120\"}",
  "cusUid": "48351",
  "transCode": "wap.balance",
  "signMsg": "WKmrn+NrjIi991ltPJcHZTKUD4MK//4TDg9phYAcm5GplqXMs7TBqoM56rktPIUTxfEmaQ/iNczyxQ8wJGLoelYq9up0z17of2unipJ0nhrnsa8qboiDn6aGtTaPmzOSO6lAZ7hj1pDsfAQtj9YAaZhwBepU5PQa9daOoySFFxGbEVW2HUkPEQF1U9a1a9TFlOc/M+1AltV3tn2lxXo5dOssS8/ktn7G/KKLCvJF96qY2ILiVKO/pPpJV1MNynt5qj4R1kEC71if0B91nEICVOntVQFzdbHAYe2ApeRt9DgGmNb+p7/lY0LbPzioExP5CXPhcse6LGstCUq334F9SA==",
  "version": "01",
  "reqSn": "483511700644561873"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名          | 字段名称   | 可空 | 类型       | 备注          |
| ------------ | ------ | -- | -------- | ----------- |
| `merchUid`   | 商户UID  | 必填 | N        | 商户UID       |
| `userNo`     | 用户编号   | 必填 | C (1,32) | 发起支付的用户唯一标识 |
| `userName`   | 用户真实姓名 | 必填 | C (1,32) | 用户付款账户的真实姓名 |
| `currency`   | 币种（代币） | 必填 | C (1,16) | 币种          |
| `balanceAmt` | 余额     | 必填 | N        | 余额          |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，代表钱包余额查询成功。
> 2. `retCode` 为其它情况，代表钱包余额查询失败。

#### 响应报文示例

```json
{
  "transCode": "wap.balance",
  "version": "01",
  "cusUid": "48351",
  "reqSn": "483511700644561873",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"balanceAmt\":522.9,\"currency\":\"TTT\",\"merchUid\":48351,\"userName\":\"1120\",\"userNo\":\"1120\"}",
  "signMsg": "h+JBv3MAqvDG+K+BjCpv9GPl7UBH3lJOtESkeRjwRUt4UzOMlBN5m8InlkoWYDzhRn8vGbbOzsWI8a//Kirly/tKjOurOF2XjHiTMVbmrnHTytE4G8+EHFQO80wxWV+lDy5IaIQTPqZzlUTJRvupY1RwyrFNYMpxoTKjNgHu+U3XsQ0n94dLy+FngY3EKjl/Sr42BxzNMGpgcZZC/S0UtB37ny0TehAypZrGR+9WaLsTe/OAT7QdDMbBi2KS4whPuN9HgtUehBaTJl1sigf6BKWOWt+2QiIkrhDxeDG6x8RhBbzwmDlt7TvIghMfiv6M3GhREvc5Bq85oJT8P0uHCw=="
}
```
