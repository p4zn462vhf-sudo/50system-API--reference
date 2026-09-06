# 商户提币查询 (withdraw.query)

**应用场景**：商户系统向平台发起对商户提币下单、商户提币挂单交易查询，平台返回当前商户提币下单、商户提币挂单的交易状态。

***

### 1. 请求报文

#### 业务参数定义

| 节点名            | 字段名称  | 可空 | 类型      | 备注                                        |
| -------------- | ----- | -- | ------- | ----------------------------------------- |
| `merchUid`     | 商户UID | 必填 | N       | 商户UID                                     |
| `merchOrderNo` | 商户单号  | 可空 | C(0,64) | 与 `orderNo` 二选一                           |
| `orderNo`      | 平台单号  | 可空 | C(0,19) | 与 `merchOrderNo` 二选一                      |
| `tradeDate`    | 交易日期  | 可空 | C(0,8)  | 下单时返回的交易日期，格式:yyyyMMdd，只使用商户单号查询时，交易日期为必填 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"orderNo\":\"2023110102163082318\"}",
  "cusUid": "42400",
  "transCode": "withdraw.query",
  "signMsg": "0JpmZtFVRKY27s2f2bH0B5smaggw96n/JkFlRQYi7mk0xLJmJKQ25FpE7LLIl7tvXeSQiMPXuNK+bjnnWO1lcFnjmJvFB7EA6LylVHXi2BqZihU0jaMaUf4wPVM+C+cnNLHPTFP/U/yEG3vB95GDd3gHjrVSrryOX7X1gy1qZQStIUFtnRhWivGuTKciqReumOHa7ll8pfeFqiqtB3cBaEas+4Kg1t03pcpHitGB6lzcbQpssI7DKJp/cjqSZbh589k+nUpLLb4MolyyroJ0Z2xUIuAqeNW50SAbTsrJWI8JmUP4DD0OABpjJl4TWXbYxy+O2dcUZuCA+o1+vSTnHQ==",
  "version": "01",
  "reqSn": "424001698776547193"
}
```

***

### 2. 响应报文

#### 业务参数定义

| 节点名             | 字段名称   | 可空 | 类型        | 备注                                                                               |
| --------------- | ------ | -- | --------- | -------------------------------------------------------------------------------- |
| `merchUid`      | 商户UID  | 必填 | N         | 商户UID                                                                            |
| `merchOrderNo`  | 商户单号   | 必填 | C(1,64)   | 商户系统唯一订单号                                                                        |
| `orderNo`       | 平台单号   | 必填 | C(19)     | 平台唯一订单号                                                                          |
| `tradeDate`     | 交易日期   | 必填 | C(8)      | 平台下单日期，格式:yyyyMMdd                                                               |
| `userNo`        | 用户编号   | 必填 | C (1,32)  | 发起支付的用户唯一标识                                                                      |
| `userName`      | 用户真实姓名 | 必填 | C (1,32)  | 用户付款账户的真实姓名                                                                      |
| `orderAmt`      | 下单数量   | 必填 | N         | 下单数量                                                                             |
| `payAmt`        | 实际交易数量 | 可空 | N         | 实际交易数量，交易成功时返回                                                                   |
| `status`        | 订单状态   | 必填 | N         | <p><code>10</code>: 处理中<br><code>100</code>: 交易成功<br><code>-100</code>: 交易失败</p> |
| `payTime`       | 交易完成时间 | 可空 | C(0,21)   | 交易成功时返回                                                                          |
| `recAddress`    | 提币地址   | 可空 | C (0,128) | 提币地址，获取路径：进入钱包链接 > 我的 > 提币地址                                                     |
| `extrasContent` | 扩展内容   | 可空 | C (0,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256                                                  |
| `remark`        | 业务返回描述 | 必填 | C(1,64)   | 业务返回描述                                                                           |

> **解析说明**：
>
> 1. `retCode` 为 `000000`，代表提币查询成功，以 `data` 中 `status` 为判定交易是否成功（`status = 100` 代表提币交易成功，不代表提币的挂单到账，挂单是否到账，请关注提币到账确认链接中订单状态）。
> 2. `retCode` 为 `000010`，代表未查询到订单信息，可以判定交易失败。
> 3. `retCode` 为其它情况，代表提币查询失败，请重新查询。

#### 响应报文示例

```json
{
  "cusUid": "42400",
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"MB7625514709\",\"orderNo\":\"2023110102163082318\",\"tradeDate\":\"20231101\",\"userNo\":\"9528\",\"userName\":\"张三\",\"orderAmt\":900,\"payAmt\":900,\"status\":100,\"payTime\":\"2023-11-01 02:16:30\",\"recAddress\":\"424001000372XCDDFFF80559CDBC51\",\"remark\":\"提币结果推送商户结果失败:出售挂单未下架\"}",
  "reqSn": "424001698776547193",
  "retCode": "000000",
  "retMsg": "请求成功",
  "signMsg": "iShcU4n75YUaINaPRMpJfMgdSkphTo+GkJFIWcYXnHpxZ6f1xGd5N0sAoihE08i8TpquL8ZiHJbUXsFQ3+73sbE8WeWqnISj1VTu+sW4wZwaFZbnCi4068sjWcXoWJ4a/wd6LpRCfLa05EuUdVXYDoxEAS9KiWyVN2SRk8YL7CMVDRFx3bu6x1HmZ3Y5+No8QVTSSlctcIAe3DMnvRsHZvzfqc1ZmrXUmvqajwe3vBSqLANU894lqo9QiS+kIqGF7AlwVa3N0jnMYg2LIuVlKkfZMvBpeFdwlGKdtVfuiuJuVkNHg9OjhF5cEOucYlOjbzUW+I/fBRcTeNngTrxYCQ==",
  "transCode": "withdraw.query",
  "version": "01"
}
```
