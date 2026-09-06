# 商户购买查询 (buy.query)

**应用场景**：商户系统向平台发起购买订单交易查询，平台返回当前购买订单的交易状态

***

### 请求报文（业务参数定义）

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| orderNo | 平台单号 | 可空 | C(1,64) | 平台唯一订单号 |
| merchOrderNo | 商户单号 | 可空 | C(1,64) | 商户系统唯一订单号 |
| tradeDate | 交易日期 | 条件选填 | C(8) | 下单时返回的交易日期，格式:yyyyMMdd，只使用商户单号查询时，交易日期为必填 |

#### 请求报文示例

```json
{
  "data": "{\"merchUid\":\"42400\",\"orderNo\":\"2023110100294782308\"}",
  "cusUid": "42400",
  "transCode": "buy.query",
  "signMsg": "cqxfymBRTsquEkrH+67VUoshUuGgcGpA2h+IWOb54KSoBex2GQPRe5iamqqzflQvkQFlA0FF83ArbsVAsPBehcAW0EmdUECOb6//Fg807T9LoqsaEoRAI5M0OFkxxbAGH6Kvj7mmyJQyAPogUbFAOGtsd11afXWjLdvyUFtqL5DsBYutHk664wdYx9B3u1uJBDBRrHDW+4Kf9hP/9FeLXb1isR7MsUhWL2V31yTksSGKhtmnARgh25JBgMYVnJZqmv+8kNnextnkaB7StdMFG6P+u/JLe+ZhgFSlISqvUOFdXXiiD+QIqHglrfmucwH5QjJvCXI6k8fchgmWQwTKog==",
  "version": "01",
  "reqSn": "424001698770250748"
}
```
响应报文(业务参数定义)

| 节点名 | 字段名称 | 可空 | 类型 | 备注 |
| --- | --- | -- | --- | --- |
| merchUid | 商户UID | 必填 | N | 商户UID |
| merchOrderNo | 商户单号 | 必填 | C(1,64) | 商户系统唯一订单号 |
| orderNo | 平台单号 | 必填 | C(1,64) | 平台唯一订单号 |
| tradeDate | 交易日期 | 必填 | C(8) | 平台下单日期，格式: yyyyMMdd |
| payType | 支付方式 | 必填 | C(1,32) | 支付渠道类型，如：BankTransfer |
| orderAmt | 下单金额 | 必填 | N | 下单金额 |
| payAmt | 实际支付金额 | 必填 | N | 实际支付金额 |
| status | 业务状态码 | 必填 | N | 10:待支付; 100:交易成功；-100:交易失败 |
| payTime | 支付时间 | 必填 | C(19) | 支付完成时间，格式: yyyy-MM-dd HH:mm:ss |
| remark | 业务返回描述 | 必填 | C(1,128) | 业务处理结果或失败原因描述 |
| extrasContent | 扩展字段 | 可空 | C(1,256) | 预留扩展对象，没有则不传，扩展内容序列化后总长度不可超过256 |


注：解析说明

1、retCode 为000000，代表购买查询成功，以data中status为判定交易是否成功（status = 100,代表交易成功）

2、retCode 为000010，代表未查询到订单信息，可以判定交易失败

3、retCode 为其它情况，代表购买查询失败，请重新查询

响应报文示例
```json
{
  "transCode": "buy.query",
  "version": "01",
  "cusUid": "42400",
  "reqSn": "424001698770250748",
  "retCode": "000000",
  "retMsg": "请求成功",
  "data": "{\"merchUid\":42400,\"merchOrderNo\":\"QC9223149407\",\"orderNo\":\"2023110100294782308\",\"tradeDate\":\"20231101\",\"payType\":\"BankTransfer\",\"orderAmt\":110.1,\"payAmt\":110.1,\"status\":100,\"payTime\":\"2023-11-01 00:29:47\",\"remark\":\"购买结果受理计算商户手续费失败:查询业务计费模板失败或为空\",\"extrasContent\":\"{\\\"a\\\":1111,\\\"b\\\":2222}\"}",
  "signMsg": "gEtjEsm35J3Q4IBoOwF7hdlsBw5wh9mxeAyQvVcnuX4dNKh9E1Wf77w9StAfqIyNOE1AVIYKM9n50WJrnPBn5P+naZMzpS7mjBmXCoZ0NbSEpaSsh3hxPv0DCPZiPeA+eda+bHSbnL+d7GqQwuND35TNJSVrFJEhQ6uRfwW2tjuPWcOdZV03yTYgE0rwFhVFx4DClDIkl4z5qZmzjkeGRGFwO8GeA8dxel2TBAwVeqwLwONfh8dvHytH9bOvSb5GO8lz28Yq2R7D9T3sZU5/uqp1lFVdPOMTqE58WhBAnW6wPYs3J4T3ksMdY5yC08E8ynUAKmRBIFhiHeMlBM8gIQ=="
}
```
